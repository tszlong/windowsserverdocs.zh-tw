---
title: 新增 Virtual 閘道承租人 Virtual 網路
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 351cafd1466c4b3c20e907ea221c9f58129b3443
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>新增 Virtual 閘道承租人 Virtual 網路

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解如何設定承租人 Virtual 閘道、使用 Windows PowerShell cmdlet 和指令碼，提供您 tenants' Virtual 網路網站-連接到他們的組織的網站和網際網路。   
  
RAS 閘道支援最多個數百 tenants，根據承租人每個使用的頻寬。 您可以使用 Network Controller 新增承租人 Virtual 閘道 RAS 閘道成員閘道集區的執行個體。 Network Controller 會自動判斷要為您 tenants 部署新的 Virtual 閘道時使用的最佳 RAS 閘道。  
  
每個 Virtual 閘道對應到特定的承租人，並由一或多個網路連接（網站-VPN 通道），（選擇性）邊境閘道通訊協定 (BGP) 連接。 這可讓您針對他們承租人 Virtual 網路連接到外部網路，例如房客企業網路，網路服務提供者或網際網路。  
  
當部署承租人 Virtual 閘道時，您有下列設定選項：  
  
**網路連接選項**  
- IPSec 網站-virtual 私人網路 (VPN)   
- 一般路由壓縮 (GRE)   
- 層級 3 轉接  
  
**BGP 設定選項**  
- BGP 路由器設定  
- BGP 等設定  
- BGP 路由原則設定  
  
Windows PowerShell 範例指令碼和本主題中的命令示範如何部署承租人 virtual 閘道 RAS 閘道使用這些選項。  
  
本主題包含下列各節。  
  
- [新增房客 virtual 閘道](#bkmk_addgwy)  
- [新增網站-VPN 上網房客（IPsec、GRE 或 L3）](#bkmk_s2s1)  
- [為 BGP 路由器設定閘道](#bkmk_bgp1)  
- [設定閘道所有連接三種類型 (IPsec，GRE，L3) 和 BGP](#bkmk_all3)  
- [修改或移除閘道 Virtual 網路](#bkmk_modify)  
  
   
>[!IMPORTANT]  
>此主題中執行的範例 Windows PowerShell 命令與指令碼所提供的任何之前，您必須變更所有變數值，因此值是適用於您的部署。  
  
## <a name="bkmk_addgwy"></a>新增房客 virtual 閘道  
  
步驟 1：確認中 Network Controller 存在閘道共用物件。  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Gateway Pool configuration  
$gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri  
  
# Display in JSON format  
$gwPool | ConvertTo-Json -Depth 2   
  
  
```  
步驟 2：驗證子網路，以用來傳送承租人的 Virtual 的網路退出封包在於 Network Controller;並擷取是可用於路由 virtual 網路和閘道承租人之間 virtual 子網路。  
  
```  
$uri = "https://ncrest.contoso.com"   
  
# Retrieve the Tenant Virtual Network configuration  
$Vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri  -ResourceId "Contoso_Vnet1"   
  
# Display in JSON format  
$Vnet | ConvertTo-Json -Depth 4   
  
# Retrieve the Tenant Virtual Subnet configuration  
$RoutingSubnet = Get-NetworkControllerVirtualSubnet -ConnectionUri $uri  -ResourceId "Contoso_WebTier" -VirtualNetworkID $vnet.ResourceId   
  
# Display in JSON format  
$RoutingSubnet | ConvertTo-Json -Depth 4   
  
```  
步驟 3：建立 virtual 閘道 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for Tenant Virtual Gateway  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties   
  
# Update Gateway Pool reference  
$VirtualGWProperties.GatewayPools = @()   
$VirtualGWProperties.GatewayPools += $gwPool   
  
# Specify the Virtual Subnet that is to be used for routing between the gateway and Virtual Network   
$VirtualGWProperties.GatewaySubnets = @()   
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet   
  
# Update the rest of the Virtual Gateway object properties  
$VirtualGWProperties.RoutingType = "Dynamic"   
$VirtualGWProperties.NetworkConnections = @()   
$VirtualGWProperties.BgpRouters = @()   
  
# Add the new Virtual Gateway for tenant   
$virtualGW = New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force   
  
```  
  
## <a name="bkmk_s2s1"></a>新增網站-VPN 上網房客（IPsec、GRE 或 L3）  
  
您可以建立-網站 VPN 連接 IPsec、GRE，或層級 3 (L3) 轉寄使用下列範例為每個閘道類型。  
  
### <a name="ipsec-vpn-site-to-site-network-connection"></a>IPsec VPN 網站-網路  
  
建立網路連接 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "P@ssw0rd"   
  
$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   
  
$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   
  
# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.1.10.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.121"   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force   
  
  
```  
### <a name="gre-vpn-site-to-site-network-connection"></a>GRE VPN 網站-網路  
  
建立網路連接 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "GRE"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
$nwConnectionProperties.GreConfiguration.GreKey = 1234   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "10.127.134.122"   
  
# L3 specific configuration (leave blank for GRE)  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_GreGW" -Properties $nwConnectionProperties -Force   
  
```  
### <a name="l3-forwarding-network-connection"></a>L3 轉寄網路  
  
若要設定 L3 轉寄網路，您還必須設定相對應的邏輯網路。   
  
步驟 1: 邏輯的網路設定 L3 轉寄網路。  
  
```  
# Create a new object for the Logical Network to be used for L3 Forwarding  
$lnProperties = New-Object Microsoft.Windows.NetworkController.LogicalNetworkProperties  
  
$lnProperties.NetworkVirtualizationEnabled = $false  
$lnProperties.Subnets = @()  
  
# Create a new object for the Logical Subnet to be used for L3 Forwarding and update properties  
$logicalsubnet = New-Object Microsoft.Windows.NetworkController.LogicalSubnet  
$logicalsubnet.ResourceId = "Contoso_L3_Subnet"  
$logicalsubnet.Properties = New-Object Microsoft.Windows.NetworkController.LogicalSubnetProperties  
$logicalsubnet.Properties.VlanID = 1001  
$logicalsubnet.Properties.AddressPrefix = "10.127.134.0/25"  
$logicalsubnet.Properties.DefaultGateways = "10.127.134.1"  
  
$lnProperties.Subnets += $logicalsubnet  
  
# Add the new Logical Network to Network Controller  
$vlanNetwork = New-NetworkControllerLogicalNetwork -ConnectionUri $uri -ResourceId "Contoso_L3_Network" -Properties $lnProperties -Force  
  
```  
步驟 2：建立網路連接 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for the Tenant Network Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   
  
# Update the common object properties  
$nwConnectionProperties.ConnectionType = "L3"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 10000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 10000   
  
# GRE specific configuration (leave blank for L3)  
$nwConnectionProperties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration   
  
# Update specific properties depending on the Connection Type  
$nwConnectionProperties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration   
$nwConnectionProperties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]   
  
$nwConnectionProperties.IPAddresses = @()   
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress   
$localIPAddress.IPAddress = "10.127.134.55"   
$localIPAddress.PrefixLength = 25   
$nwConnectionProperties.IPAddresses += $localIPAddress   
  
$nwConnectionProperties.PeerIPAddresses = @("10.127.134.65")   
  
# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "14.2.20.1/32"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   
  
# Add the new Network Connection for the tenant  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_L3GW" -Properties $nwConnectionProperties -Force   
  
```  
  
## <a name="bkmk_bgp1"></a>為 BGP 路由器設定閘道  
  
您可以使用下列範例指令碼閘道設定為邊境閘道通訊協定 (BGP) 路由器。  
  
### <a name="add-a-bgp-router-for-the-tenant"></a>新增承租人 BGP 路由器  
  
建立 BGP 路由器 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for the Tenant BGP Router  
$bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties   
  
# Update the BGP Router properties  
$bgpRouterproperties.ExtAsNumber = "0.64512"   
$bgpRouterproperties.RouterId = "192.168.0.2"   
$bgpRouterproperties.RouterIP = @("192.168.0.2")   
  
# Add the new BGP Router for the tenant  
$bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force   
   
  
```  
### <a name="add-a-bgp-peer-for-this-tenant-corresponding-to-the-site-to-site-vpn-network-connection-added-above"></a>新增對 BGP 等這個承租人，上方新增網站到 VPN 網路連接到對應  
  
建立 BGP 等 JSON 物件，並將它新增到網路控制器。  
  
```  
# Create a new object for Tenant BGP Peer  
$bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties   
  
# Update the BGP Peer properties  
$bgpPeerProperties.PeerIpAddress = "14.1.10.1"   
$bgpPeerProperties.AsNumber = 64521   
$bgpPeerProperties.ExtAsNumber = "0.64521"   
  
# Add the new BGP Peer for tenant  
New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force   
  
```  
## <a name="bkmk_all3"></a>設定閘道所有連接三種類型 (IPsec，GRE，L3) 和 BGP  
（選擇性）您可以將結合所有上述步驟，並設定承租人 virtual 閘道所有連接三個選項：   
  
```  
# Create a new Virtual Gateway Properties type object  
$VirtualGWProperties = New-Object Microsoft.Windows.NetworkController.VirtualGatewayProperties  
  
# Update GatewayPool reference  
$VirtualGWProperties.GatewayPools = @()  
$VirtualGWProperties.GatewayPools += $gwPool  
  
# Specify the Virtual Subnet that is to be used for routing between GW and VNET  
$VirtualGWProperties.GatewaySubnets = @()  
$VirtualGWProperties.GatewaySubnets += $RoutingSubnet  
  
# Update some basic properties  
$VirtualGWProperties.RoutingType = "Dynamic"  
  
# Update Network Connection object(s)  
$VirtualGWProperties.NetworkConnections = @()  
  
# IPSec Connection configuration  
$ipSecConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$ipSecConnection.ResourceId = "Contoso_IPSecGW"  
$ipSecConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$ipSecConnection.Properties.ConnectionType = "IPSec"  
$ipSecConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$ipSecConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$ipSecConnection.Properties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration  
  
$ipSecConnection.Properties.IpSecConfiguration.AuthenticationMethod = "PSK"  
$ipSecConnection.Properties.IpSecConfiguration.SharedSecret = "P@ssw0rd"  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode  
  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "SHA256128"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "DES3"  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 1233  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500  
$ipSecConnection.Properties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode  
  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 1234  
$ipSecConnection.Properties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000  
  
$ipSecConnection.Properties.IPAddresses = @()  
$ipSecConnection.Properties.PeerIPAddresses = @()  
  
$ipSecConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.1.10.1/32"  
$ipv4Route.metric = 10  
$ipSecConnection.Properties.Routes += $ipv4Route  
  
$ipSecConnection.Properties.DestinationIPAddress = "10.127.134.121"  
  
# GRE Connection configuration  
$greConnection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$greConnection.ResourceId = "Contoso_GreGW"  
  
$greConnection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$greConnection.Properties.ConnectionType = "GRE"  
$greConnection.Properties.OutboundKiloBitsPerSecond = 10000  
$greConnection.Properties.InboundKiloBitsPerSecond = 10000  
  
$greConnection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$greConnection.Properties.GreConfiguration.GreKey = 1234  
  
$greConnection.Properties.IPAddresses = @()  
$greConnection.Properties.PeerIPAddresses = @()  
  
$greConnection.Properties.Routes = @()  
  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$greConnection.Properties.Routes += $ipv4Route  
  
$greConnection.Properties.DestinationIPAddress = "10.127.134.122"  
  
$greConnection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
  
# L3 Forwarding connection configuration  
$l3Connection = New-Object Microsoft.Windows.NetworkController.NetworkConnection  
$l3Connection.ResourceId = "Contoso_L3GW"  
  
$l3Connection.Properties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties  
$l3Connection.Properties.ConnectionType = "L3"  
$l3Connection.Properties.OutboundKiloBitsPerSecond = 10000  
$l3Connection.Properties.InboundKiloBitsPerSecond = 10000  
  
$l3Connection.Properties.GreConfiguration = New-Object Microsoft.Windows.NetworkController.GreConfiguration  
$l3Connection.Properties.L3Configuration = New-Object Microsoft.Windows.NetworkController.L3Configuration  
$l3Connection.Properties.L3Configuration.VlanSubnet = $vlanNetwork.properties.Subnets[0]  
  
$l3Connection.Properties.IPAddresses = @()  
$localIPAddress = New-Object Microsoft.Windows.NetworkController.CidrIPAddress  
$localIPAddress.IPAddress = "10.127.134.55"  
$localIPAddress.PrefixLength = 25  
$l3Connection.Properties.IPAddresses += $localIPAddress  
  
$l3Connection.Properties.PeerIPAddresses = @("10.127.134.65")  
  
$l3Connection.Properties.Routes = @()  
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo  
$ipv4Route.DestinationPrefix = "14.2.20.1/32"  
$ipv4Route.metric = 10  
$l3Connection.Properties.Routes += $ipv4Route  
  
# Update BGP Router Object  
$VirtualGWProperties.BgpRouters = @()  
  
$bgpRouter = New-Object Microsoft.Windows.NetworkController.VGwBgpRouter  
$bgpRouter.ResourceId = "Contoso_BgpRouter1"  
$bgpRouter.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties  
  
$bgpRouter.Properties.ExtAsNumber = "0.64512"  
$bgpRouter.Properties.RouterId = "192.168.0.2"  
$bgpRouter.Properties.RouterIP = @("192.168.0.2")  
  
$bgpRouter.Properties.BgpPeers = @()  
  
# Create BGP Peer Object(s)  
# BGP Peer for IPSec Connection  
$bgpPeer_IPSec = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_IPSec.ResourceId = "Contoso_IPSec_Peer"  
  
$bgpPeer_IPSec.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_IPSec.Properties.PeerIpAddress = "14.1.10.1"  
$bgpPeer_IPSec.Properties.AsNumber = 64521  
$bgpPeer_IPSec.Properties.ExtAsNumber = "0.64521"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_IPSec  
  
# BGP Peer for GRE Connection  
$bgpPeer_Gre = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_Gre.ResourceId = "Contoso_Gre_Peer"  
  
$bgpPeer_Gre.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_Gre.Properties.PeerIpAddress = "14.2.20.1"  
$bgpPeer_Gre.Properties.AsNumber = 64522  
$bgpPeer_Gre.Properties.ExtAsNumber = "0.64522"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_Gre  
  
# BGP Peer for L3 Connection  
$bgpPeer_L3 = New-Object Microsoft.Windows.NetworkController.VGwBgpPeer  
$bgpPeer_L3.ResourceId = "Contoso_L3_Peer"  
  
$bgpPeer_L3.Properties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties  
$bgpPeer_L3.Properties.PeerIpAddress = "14.3.30.1"  
$bgpPeer_L3.Properties.AsNumber = 64523  
$bgpPeer_L3.Properties.ExtAsNumber = "0.64523"  
  
$bgpRouter.Properties.BgpPeers += $bgpPeer_L3  
  
$VirtualGWProperties.BgpRouters += $bgpRouter  
  
# Finally Add the new Virtual Gateway for tenant  
New-NetworkControllerVirtualGateway -ConnectionUri $uri  -ResourceId "Contoso_VirtualGW" -Properties $VirtualGWProperties -Force  
  
```  
## <a name="bkmk_modify"></a>修改或移除閘道 Virtual 網路  
  
您可以使用下列範例指令碼修改或移除現有閘道。  
  
### <a name="modify-the-configuration-of-an-existing-gateway"></a>修改現有閘道的設定  
您可以使用下列命令來修改現有閘道。  
  
步驟 1：擷取元件的設定，並將它儲存在變數  
  
```  
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"  
```  
步驟 2：瀏覽變數結構瑞曲之戰需要的屬性，並將它設為更新值  
  
```  
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"  
```  
步驟 3：新增修改更換上 Network Controller 的較舊的組態設定  
  
```  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force  
```  
### <a name="remove-a-gateway"></a>移除閘道  
若要移除個人閘道功能或整個閘道，您可以使用下列的 Windows PowerShell 命令。  
  
#### <a name="remove-a-network-connection"></a>移除上網  
```  
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force  
```  
#### <a name="remove-a-bgp-peer"></a>移除 BGP 等  
```  
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force  
```  
#### <a name="remove-a-bgp-router"></a>移除 BGP 路由器  
```  
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force  
```  
#### <a name="remove-a-gateway"></a>移除閘道  
```  
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force   
```  
  
  


