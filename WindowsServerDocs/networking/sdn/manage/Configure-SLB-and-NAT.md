---
title: 設定軟體負載平衡器以進行負載平衡和網路位址轉譯 (NAT)
description: 本主題是軟體定義網路上如何管理租用戶工作負載和指南 Windows Server 2016 中的虛擬網路的一部分。
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 73bff8ba-939d-40d8-b1e5-3ba3ed5439c3
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 4d53c4bcbe1f37f532f2861d5669201959a9f091
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446676"
---
# <a name="configure-the-software-load-balancer-for-load-balancing-and-network-address-translation-nat"></a>設定軟體負載平衡器以進行負載平衡和網路位址轉譯 (NAT)

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解如何使用軟體定義網路功能\(SDN\)軟體負載平衡器\(SLB\)提供輸出網路位址轉譯\(NAT\)，輸入 NAT 或負載平衡的應用程式的多個執行個體之間。

## <a name="software-load-balancer-overview"></a>軟體負載平衡器概觀

SDN 軟體負載平衡器\(SLB\)您的應用程式提供高可用性和網路效能。 它是第 4 層\(TCP、 UDP\)負載平衡器，將雲端服務或負載平衡器集合中定義的虛擬機器中狀況良好的服務執行個體之間的連入流量分配。

設定 SLB，執行下列操作：

- 負載平衡連入流量的虛擬機器的虛擬網路的外部\(Vm\)，也稱為公用 VIP 負載平衡。
- 負載平衡連入流量的虛擬網路中 Vm 之間、 雲端服務中的 Vm 之間或在內部部署電腦與跨單位虛擬網路中的 Vm 之間。 
- 從虛擬網路的 VM 網路流量轉送至外部目的地，使用網路位址轉譯 (NAT)，也稱為輸出 nat。
- 將外部流量轉送到特定的 VM，也稱為 「 輸入 nat。




## <a name="example-create-a-public-vip-for-load-balancing-a-pool-of-two-vms-on-a-virtual-network"></a>範例：建立負載平衡集區的虛擬網路上的兩個 Vm 的公用 VIP

在此範例中，您建立負載平衡器物件具有公用 VIP 和兩個 Vm 為來處理要求到 VIP 集區成員。 此程式碼範例也會新增 HTTP 健全狀況探查來偵測是否有一個集區成員會變成沒有回應。

1. 準備負載平衡器物件。

   ```PowerShell
    import-module NetworkController

    $URI = "https://sdn.contoso.com"

    $LBResourceId = "LB2"

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

2. 指派前端的 IP 位址，一般稱為虛擬 IP (VIP)。<p>VIP 必須來自其中一個提供給負載平衡器管理員的邏輯網路 IP 集區中的未使用的 IP。 

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

3. 配置後端位址集區，其中包含 Vm 的負載平衡集的成員所組成的動態 Ip (Dip)。 

   ```PowerShell 
    $BackEndAddressPool = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEndAddressPool.ResourceId = "BE1"
    $BackEndAddressPool.ResourceRef = "/loadBalancers/$LBResourceId/backendAddressPools/$($BackEndAddressPool.ResourceId)"

    $BackEndAddressPool.Properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties

    $LoadBalancerProperties.backendAddressPools += $BackEndAddressPool
   ```

4. 定義健康狀態探查的負載平衡器用來判斷後端集區成員的健全狀況狀態。<p>在此範例中，您會定義查詢 HTTP 探查的 RequestPath"/ health.htm。 」  每隔 5 秒，IntervalInSeconds 屬性所指定執行查詢。<p>健康情況探查必須接收 HTTP 回應碼為 200，探查才會良好的後端 IP，請考慮的 11 連續查詢。 如果後端 IP 不是狀況良好的它不會接收流量負載平衡器。

   >[!IMPORTANT]
   >不針對任何存取控制清單 (Acl)，因為這是起始點，讓探查套用至後端 IP 會封鎖流量，或從第一個 IP 子網路中。

   您可以使用下列範例來定義健康狀態探查。

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

5. 定義負載平衡規則傳送到後端 IP 抵達前端 IP 的流量。  在此範例中後, 端集區會接收 TCP 連接埠 80 的流量。<p>您可以使用下列範例定義負載平衡規則：

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

6. 加入網路控制站的負載平衡器組態。<p>使用下列範例將負載平衡器組態新增至網路控制站：

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

7. 請遵循下一個範例中將網路介面新增至這個後端集區。


## <a name="example-use-slb-for-outbound-nat"></a>範例：SLB 用於輸出 NAT

在此範例中，您可以設定 SLB 與後端集區提供 vm 上的虛擬網路私人位址空間來觸達輸出至網際網路的輸出 NAT 功能。 

1. 建立負載平衡器的屬性、 前端 IP 和後端集區。

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

3. 加入網路控制卡中的負載平衡器物件。

   ```PowerShell
    $LoadBalancerResource = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $LBResourceId -Properties $LoadBalancerProperties -Force -PassInnerException
   ```

4. 請遵循下一個範例中新增您要提供網際網路存取的網路介面。

## <a name="example-add-network-interfaces-to-the-back-end-pool"></a>範例：將網路介面新增至後端集區
在此範例中，您網路介面新增至後端集區。  您必須重複此步驟中每個網路介面，可處理對 VIP 提出的要求。 

您也可以重複此程序上將它新增至多個負載平衡器物件的單一網路介面。 例如，如果您有 web 伺服器 VIP 的負載平衡器物件和個別的負載平衡器物件，以提供輸出 nat。
    
1. 取得包含要新增網路介面的後端集區的負載平衡器物件。

   ```PowerShell
   $lbresourceid = "LB2"
   $lb = get-networkcontrollerloadbalancer -connectionuri $uri -resourceID $LBResourceId -PassInnerException
   ```

2. 取得網路介面，並將 backendaddress 集區新增至 loadbalancerbackendaddresspools 陣列。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -PassInnerException
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   ```  

3. 將網路介面，以套用變更。 

   ```PowerShell
   new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06 -properties $nic.properties -force -PassInnerException
   ``` 


## <a name="example-use-the-software-load-balancer-for-forwarding-traffic"></a>範例：使用軟體負載平衡器將流量轉送
如果您需要對應至單一網路介面的虛擬網路上的虛擬 IP，而不需定義個別的連接埠，您可以建立的 L3 轉寄規則。  此規則會轉送所有流量透過指派的 VIP PublicIPAddress 物件中包含的 VM。

如果您定義 VIP 和 DIP 為相同的子網路，然後這相當於執行 L3 轉送，而不需要 nat。

>[!NOTE]
>此程序不需要您建立負載平衡器物件。  指派給網路介面的 PublicIPAddress 是足夠的資訊來執行其設定的軟體負載平衡器。


1. 建立包含 VIP 的公用 IP 物件。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.ipaddress = "10.127.134.7"
   $publicIPProperties.PublicIPAllocationMethod = "static"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 指派給網路介面的 PublicIPAddress。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```

## <a name="example-use-the-software-load-balancer-for-forwarding-traffic-with-a-dynamically-allocated-vip"></a>範例：使用軟體負載平衡器將流量轉送與動態配置 VIP
此範例會重複上一個範例中，與相同的動作，但它會自動從負載平衡器，而不是指定特定的 IP 位址中可用的 Vip 集區配置 VIP。 

1. 建立包含 VIP 的公用 IP 物件。

   ```PowerShell
   $publicIPProperties = new-object Microsoft.Windows.NetworkController.PublicIpAddressProperties
   $publicIPProperties.PublicIPAllocationMethod = "dynamic"
   $publicIPProperties.IdleTimeoutInMinutes = 4
   $publicIP = New-NetworkControllerPublicIpAddress -ResourceId "MyPIP" -Properties $publicIPProperties -ConnectionUri $uri -Force -PassInnerException
   ```

2. 查詢 PublicIPAddress 資源，以判斷所指派的 IP 位址。

   ```PowerShell
    (Get-NetworkControllerPublicIpAddress -ConnectionUri $uri -ResourceId "MyPIP").properties
   ```

   IpAddress 屬性包含指派的位址。  輸出會看起來像這樣：
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
 
3. 指派給網路介面的 PublicIPAddress。

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid 6daca142-7d94-0000-1111-c38c0141be06
   $nic.properties.IpConfigurations[0].Properties.PublicIPAddress = $publicIP
   New-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId $nic.ResourceId -Properties $nic.properties -PassInnerException
   ```
   ## <a name="example-remove-a-publicip-address-that-is-being-used-for-forwarding-traffic-and-return-it-to-the-vip-pool"></a>範例：移除用於轉送流量的 PublicIP 位址，並將它傳回的 VIP 集區
   此範例會移除先前的範例所建立的 PublicIPAddress 資源。  PublicIPAddress 移除之後，PublicIPAddress 的參考會自動移除與網路介面、 流量將會停止正在轉送和重複使用的公用 VIP 集區，將傳回的 IP 位址。  

4. 移除 PublicIP

   ```PowerShell
   Remove-NetworkControllerPublicIPAddress -ConnectionURI $uri -ResourceId "MyPIP"
   ```

---


 