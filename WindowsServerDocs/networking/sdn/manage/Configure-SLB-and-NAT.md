---
title: 設定軟體負載平衡器以進行負載平衡和網路位址轉譯 (NAT)
description: 本主題是軟體定義網路指南的一部分，說明如何在 Windows Server 2016 中管理租使用者工作負載和虛擬網路。
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 80f1319c1abc845d7e63a2d53868bf7a3c381019
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406092"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>設定軟體負載平衡器以進行負載平衡和網路位址轉譯 (NAT)

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解如何使用軟體定義網路\(SDN\)軟體負載平衡器\(SLB\)來提供輸出網路位址轉譯\(NAT\)，輸入 NAT，或多個應用程式實例之間的負載平衡。

## <a name="software-load-balancer-overview"></a>軟體 Load Balancer 總覽

SDN Software Load Balancer \(SLB\)可為您的應用程式提供高可用性和網路效能。 它是第 4 \(層 TCP，UDP\)負載平衡器，可將連入流量分散到雲端服務或負載平衡器集合中定義的虛擬機器中狀況良好的服務實例之間。

設定 SLB 以執行下列動作：

- 將虛擬網路外部的連入流量負載平衡到虛擬\(機\)vm，也稱為公用 VIP 負載平衡。
- 負載平衡虛擬網路中的 Vm、雲端服務中的 Vm 之間，或內部部署電腦與跨單位虛擬網路中 Vm 之間的連入流量。 
- 使用網路位址轉譯（NAT）（也稱為輸出 NAT），將 VM 網路流量從虛擬網路轉送到外部目的地。
- 將外部流量轉送至特定 VM，也稱為「輸入 NAT」。




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>範例：建立公用 VIP 以負載平衡虛擬網路上兩部 Vm 的集區

在此範例中，您會建立具有公用 VIP 和兩個 Vm 的負載平衡器物件做為集區成員，以提供 VIP 的要求。 此範例程式碼也會新增 HTTP 健康情況探查，以偵測其中一個集區成員是否變得沒有回應。

1. 準備負載平衡器物件。

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. 指派前端 IP 位址，通常稱為虛擬 IP （VIP）。<p>VIP 必須來自提供給負載平衡器管理員的其中一個邏輯網路 IP 集區中未使用的 IP。 

   ```PowerShell
    $VIPIP = "10.127.134.5"
    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException
    
    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"
      
    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig
   ```

3. 配置後端位址集區，其中包含組成負載平衡 Vm 集合成員的動態 Ip （Dip）。 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. 定義負載平衡器用來判斷後端集區成員之健全狀況狀態的健康情況探查。<p>在此範例中，您會定義 HTTP 探查來查詢 "/health.htm." 的 RequestPath  查詢會每隔5秒執行一次，如 IntervalInSeconds 屬性所指定。<p>健康情況探查必須接收 HTTP 回應碼200，以取得11次連續的探查查詢，以將後端 IP 視為狀況良好。 如果後端 IP 狀況不良，則不會接收來自負載平衡器的流量。

   >[!IMPORTANT]
   >請勿在您套用至後端 IP 的任何存取控制清單（Acl）的子網中，封鎖進出第一個 IP 的流量，因為這是探查的來源點。

   使用下列範例來定義健全狀況探查。

   ```PowerShell
    $Probe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $Probe.ResourceId = "Probe1"
    $Probe.ResourceRef = "/loadBalancers/$LBResourceId/Probes/$($Probe.ResourceId)"

    $Probe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties
    $Probe.properties.Protocol = "HTTP"
    $Probe.properties.Port = "80"
    $Probe.properties.RequestPath = "/health.htm"
    $Probe.properties.IntervalInSeconds = 5
    $Probe.properties.NumberOfProbes = 11

    $LoadBalancerProperties.Probes += $Probe
   ```

5. 定義負載平衡規則，將抵達前端 IP 的流量傳送到後端 IP。  在此範例中，後端集區會接收到埠80的 TCP 流量。<p>使用下列範例來定義負載平衡規則：

   ```PowerShell
   $Rule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
   $Rule.ResourceId = "webserver1"

   $Rule.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
   $Rule.Properties.FrontEndIPConfigurations += $FrontEndIPConfig
   $Rule.Properties.backendaddresspool = $BackEndAddressPool 
   $Rule.Properties.protocol = "TCP"
   $Rule.Properties.FrontEndPort = 80
   $Rule.Properties.BackEndPort = 80
   $Rule.Properties.IdleTimeoutInMinutes = 4
   $Rule.Properties.Probe = $Probe

   $LoadBalancerProperties.loadbalancingRules += $Rule
   ```

6. 將負載平衡器設定新增至網路控制卡。<p>使用下列範例，將負載平衡器設定新增至網路控制卡：

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. 遵循下一個範例，將網路介面新增至此後端集區。


## <a name="example-use-slb-for-outbound-nat"></a>範例：針對輸出 NAT 使用 SLB

在此範例中，您會使用後端集區設定 SLB，以便為虛擬網路的私人位址空間上的 VM 提供輸出 NAT 功能，以連到網際網路的輸出。 

1. 建立負載平衡器屬性、前端 IP 和後端集區。

   ```PowerShell
    import-module NetworkController
    $URI = "https://sdn.contoso.com"

    $LBResourceId = "OutboundNATMMembers"
    $VIPIP = "10.127.134.6"

    $VIPLogicalNetwork = get-networkcontrollerlogicalnetwork -ConnectionUri $uri -resourceid "PublicVIP" -PassInnerException

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

    $FrontEndIPConfig = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEndIPConfig.ResourceId = "FE1"
    $FrontEndIPConfig.ResourceRef = "/loadBalancers/$LBResourceId/frontendIPConfigurations/$($FrontEndIPConfig.ResourceId)"

    $FrontEndIPConfig.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEndIPConfig.Properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEndIPConfig.Properties.Subnet.ResourceRef = $VIPLogicalNetwork.Properties.Subnets[0].ResourceRef
    $FrontEndIPConfig.Properties.PrivateIPAddress = $VIPIP
    $FrontEndIPConfig.Properties.PrivateIPAllocationMethod = "Static"

    $LoadBalancerProperties.FrontEndIPConfigurations += $FrontEndIPConfig

    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"
    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

2. 定義輸出 NAT 規則。

   ```PowerShell
    $OutboundNAT = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRule
    $OutboundNAT.ResourceId = "onat1"
    
    $OutboundNAT.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerOutboundNatRuleProperties
    $OutboundNAT.properties.frontendipconfigurations += $FrontEndIPConfig
    $OutboundNAT.properties.backendaddresspool = $BackEndAddressPool
    $OutboundNAT.properties.protocol = "ALL"

    $LoadBalancerProperties.OutboundNatRules += $OutboundNAT
   ```

3. 在網路控制卡中新增負載平衡器物件。

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. 遵循下一個範例，新增您想要提供網際網路存取的網路介面。

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>範例：將網路介面新增至後端集區
在此範例中，您會將網路介面新增至後端集區。  您必須針對每個可以處理對 VIP 所提出要求的網路介面重複此步驟。 

您也可以在單一網路介面上重複此程式，將它新增至多個負載平衡器物件。 例如，如果您有 web 伺服器 VIP 的負載平衡器物件，以及個別的負載平衡器物件來提供輸出 NAT。
    
1. 取得包含後端集區的負載平衡器物件，以新增網路介面。

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. 取得網路介面，並將 backendaddress 集區新增至 loadbalancerbackendaddresspools 陣列。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. 放置網路介面以套用變更。 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>範例：使用軟體 Load Balancer 來轉送流量
如果您需要將虛擬 IP 對應至虛擬網路上的單一網路介面，而不需定義個別埠，您可以建立 L3 轉送規則。  此規則會透過 PublicIPAddress 物件中包含的指派 VIP，轉送進出 VM 的所有流量。

如果您將 VIP 和 DIP 定義為相同的子網，則這相當於執行不含 NAT 的 L3 轉送。

>[!NOTE]
>此程式不會要求您建立負載平衡器物件。  將 PublicIPAddress 指派給網路介面是軟體 Load Balancer 執行其設定的足夠資訊。


1. 建立包含 VIP 的公用 IP 物件。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 將 PublicIPAddress 指派給網路介面。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>範例：使用軟體 Load Balancer 以動態配置的 VIP 轉送流量
這個範例會重複與上一個範例相同的動作，但它會從負載平衡器中可用的 Vip 集區自動設定 VIP，而不是指定特定的 IP 位址。 

1. 建立包含 VIP 的公用 IP 物件。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 查詢 PublicIPAddress 資源，以判斷指派的 IP 位址。

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   IpAddress 屬性包含指派的位址。  輸出看起來會像這樣：
   ```
    Counters                 : {}
    ConfigurationState       :
    IpAddress                : 10.127.134.2
    PublicIPAddressVersion   : IPv4
    PublicIPAllocationMethod : Dynamic
    IdleTimeoutInMinutes     : 4
    DnsSettings              :
    ProvisioningState        : Succeeded
    IpConfiguration          :
    PreviousIpConfiguration  :
   ```
 
3. 將 PublicIPAddress 指派給網路介面。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>範例：移除用於轉送流量的 PublicIP 位址，並將其傳回至 VIP 集區
   這個範例會移除先前範例所建立的 PublicIPAddress 資源。  移除 PublicIPAddress 之後，將會自動從網路介面移除對 PublicIPAddress 的參考，而流量將會停止轉送，而 IP 位址則會傳回給公用 VIP 集區以供重複使用。  

4. 移除 PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 