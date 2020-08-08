---
title: 將虛擬閘道新增到租用戶虛擬網路
description: 瞭解如何使用 Windows PowerShell Cmdlet 和腳本，為您租使用者的虛擬網路提供站對站連線能力。
manager: grcusanz
ms.topic: article
ms.assetid: b9552054-4eb9-48db-a6ce-f36ae55addcd
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: 8b6c109948f472154ceff7d97aef63d0a77f4624
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947146"
---
# <a name="add-a-virtual-gateway-to-a-tenant-virtual-network"></a>將虛擬閘道新增到租用戶虛擬網路

>適用於：Windows Server (半年度管道)、Windows Server 2016

瞭解如何使用 Windows PowerShell Cmdlet 和腳本，為您租使用者的虛擬網路提供站對站連線能力。 在本主題中，您會使用網路控制站，將租使用者虛擬閘道新增至屬於閘道集區成員的 RAS 閘道實例。 視每個租使用者使用的頻寬而定，RAS 閘道最多可支援100個租使用者。 網路控制站會在您為租使用者部署新的虛擬閘道時，自動決定要使用的最佳 RAS 閘道。

每個虛擬閘道都會對應至特定的租使用者，且包含一或多個網路連線 (站對站 VPN 通道) 並選擇性地邊界閘道協定 (BGP) 連接。 當您提供站對站連線能力時，您的客戶可以將其租使用者的虛擬網路連線到外部網路，例如租使用者商業網路、服務提供者網路或網際網路。

**當您部署租使用者虛擬閘道時，您會有下列設定選項：**


|                                                        網路連線選項                                                         |                                              BGP 設定選項                                               |
|-------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------|
| <ul><li>IPSec 站對站虛擬私人網路 (VPN) </li><li>Generic Routing Encapsulation (GRE)</li><li>第三層轉送</li></ul> | <ul><li>BGP 路由器設定</li><li>BGP 對等設定</li><li>BGP 路由原則設定</li></ul> |

---

本主題中的 Windows PowerShell 範例腳本和命令會示範如何使用每個選項，在 RAS 閘道上部署租使用者虛擬閘道。


>[!IMPORTANT]
>執行所提供的任何範例 Windows PowerShell 命令和腳本之前，您必須變更所有變數值，讓這些值適用于您的部署。

1.  確認閘道集區物件存在於網路控制卡中。

    ```PowerShell
    $uri = "https://ncrest.contoso.com"

    # Retrieve the Gateway Pool configuration
    $gwPool = Get-NetworkControllerGatewayPool -ConnectionUri $uri

    # Display in JSON format
    $gwPool | ConvertTo-Json -Depth 2

    ```

2.  確認用於路由傳送來自租使用者虛擬網路之封包的子網存在於網路控制卡中。 您也會抓取用於在租使用者閘道和虛擬網路之間路由的虛擬子網。

    ```PowerShell
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

3.  為租使用者虛擬閘道建立新的物件，然後更新閘道集區參考。  您也可以指定用來在閘道與虛擬網路之間路由的虛擬子網。  指定虛擬子網之後，您必須更新虛擬閘道物件內容的其餘部分，然後為該租使用者新增新的虛擬閘道。

    ```PowerShell
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

4. 使用 IPsec、GRE 或第3層 (L3) 轉送建立站對站 VPN 連線。

   >[!TIP]
   >（選擇性）您可以合併所有先前的步驟，並設定具有這三個連線選項的租使用者虛擬閘道。  如需詳細資訊，請參閱[設定具有這三種連線類型 (IPsec、GRE、L3) 和 BGP 的閘道](#optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp)。

   **IPsec VPN 站對站網路連線**

   ```PowerShell
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

   **GRE VPN 站對站網路連接**

   ```PowerShell
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

   **L3 轉送網路連線**<p>
   若要讓 L3 轉送網路連線正常運作，您必須設定對應的邏輯網路。

   1. 設定 L3 轉送網路連線的邏輯網路。  <br>

      ```PowerShell
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

   2. 建立網路連線 JSON 物件，並將它新增至網路控制卡。

      ```PowerShell
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

5. 將閘道設定為 BGP 路由器，並將它新增至網路控制卡。

   1. 新增租使用者的 BGP 路由器。

      ```PowerShell
      # Create a new object for the Tenant BGP Router
      $bgpRouterproperties = New-Object Microsoft.Windows.NetworkController.VGwBgpRouterProperties

      # Update the BGP Router properties
      $bgpRouterproperties.ExtAsNumber = "0.64512"
      $bgpRouterproperties.RouterId = "192.168.0.2"
      $bgpRouterproperties.RouterIP = @("192.168.0.2")

      # Add the new BGP Router for the tenant
      $bgpRouter = New-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_BgpRouter1" -Properties $bgpRouterProperties -Force

      ```

   2. 新增此租使用者的 BGP 對等，其對應于上面新增的站對站 VPN 網路連線。

      ```PowerShell
      # Create a new object for Tenant BGP Peer
      $bgpPeerProperties = New-Object Microsoft.Windows.NetworkController.VGwBgpPeerProperties

      # Update the BGP Peer properties
      $bgpPeerProperties.PeerIpAddress = "14.1.10.1"
      $bgpPeerProperties.AsNumber = 64521
      $bgpPeerProperties.ExtAsNumber = "0.64521"

      # Add the new BGP Peer for tenant
      New-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -BgpRouterName $bgpRouter.ResourceId -ResourceId "Contoso_IPSec_Peer" -Properties $bgpPeerProperties -Force

      ```

## <a name="optional-step-configure-a-gateway-with-all-three-connection-types-ipsec-gre-l3-and-bgp"></a> (選擇性步驟) 設定具有三種連線類型的閘道 (IPsec、GRE、L3) 和 BGP
（選擇性）您可以合併所有先前的步驟，並設定具有三個連線選項的租使用者虛擬閘道：

```PowerShell
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

## <a name="modify-a-gateway-for-a-virtual-network"></a>修改虛擬網路的閘道


**取出元件的設定，並將它儲存在變數中**

```PowerShell
$nwConnection = Get-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW"
```

**導覽變數結構以到達必要屬性，並將它設定為更新值**

```PowerShell
$nwConnection.properties.IpSecConfiguration.SharedSecret = "C0mplexP@ssW0rd"
```

**新增修改過的設定以取代網路控制卡上的舊版設定**

```PowerShell
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId $nwConnection.ResourceId -Properties $nwConnection.Properties -Force
```


## <a name="remove-a-gateway-from-a-virtual-network"></a>從虛擬網路移除閘道
您可以使用下列 Windows PowerShell 命令來移除個別閘道功能或整個閘道。

**移除網路連線**
```PowerShell
Remove-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_IPSecGW" -Force
```

**移除 BGP 對等**
```PowerShell
Remove-NetworkControllerVirtualGatewayBgpPeer -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -BgpRouterName "Contoso_BgpRouter1" -ResourceId "Contoso_IPSec_Peer" -Force
```

**移除 BGP 路由器**
```PowerShell
Remove-NetworkControllerVirtualGatewayBgpRouter -ConnectionUri $uri -VirtualGatewayId "Contoso_VirtualGW" -ResourceId "Contoso_BgpRouter1" -Force
```

**移除閘道器**
```PowerShell
Remove-NetworkControllerVirtualGateway -ConnectionUri $uri -ResourceId "Contoso_VirtualGW" -Force
```

---