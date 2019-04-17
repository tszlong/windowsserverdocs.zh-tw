---
title: 來賓 Virtual 網路叢集化
description: 本主題是輔助的軟體定義網路上如何管理承租人工作負載和 Windows Server 2016 Virtual 網路的一部分。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: shortpatti
ms.openlocfilehash: 5cab7e7c0ca0af848b4b58362388701cc4357860
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="guest-clustering-in-a-virtual-network"></a>來賓 Virtual 網路叢集化

>適用於：Windows Server（以每年次管道）、Windows Server 2016

只允許虛擬電腦連接到 virtual 網路使用 Network Controller 已指派為了通訊網路的 IP 位址。  這表示叢集技術，需要浮動的 IP 位址，例如 Microsoft 容錯、 需要一些額外的步驟，就可以正確運作。

讓浮動 IP 可供連接的方法是使用軟體負載平衡器 \(SLB\) virtual IP \(VIP\)。  軟體負載平衡器必須使 SLB 會直接傳輸到目前擁有該 IP 電腦健康探查的連接埠該 IP 上的設定。

## <a name="example-load-balancer-configuration"></a>範例： 負載平衡器設定

假設，您已經建立 Vm 將會變成叢集節點，其附加到 Virtual 網路。  如需指引，請參考[建立 VM 和連接到承租人 Virtual 網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此範例中，您將建立代表叢集，浮動 IP 位址 virtual IP 位址 (192.168.2.100)，並設定健康探查監視 59999 以判斷哪一個節點為作用中的 TCP 連接埠。

### <a name="step-1-select-the-vip"></a>步驟 1： 選取 VIP
準備指派 VIP IP 位址。  此地址可以是任何未使用或保留地址叢集節點相同的子網路中。  VIP 必須符合叢集浮動地址。

    $VIP = "192.168.2.100"
    $subnet = "Subnet2"
    $VirtualNetwork = "MyNetwork"
    $ResourceId = "MyNetwork_InternalVIP"

### <a name="step-2-create-the-load-balancer-properties-object"></a>步驟 2： 建立負載平衡器屬性物件

您可以使用下列命令範例建立負載平衡器屬性物件。

    $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties

### <a name="step-3-create-a-front-end-ip-address"></a>步驟 3： 建立 front\ 端的 IP 位址

您可以使用下列命令的範例，建立一個 front\ 端的 IP 位址。

    $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
    $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
    $FrontEnd.resourceId = "Frontend1"
    $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
    $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
    $FrontEnd.properties.privateIPAddress = $VIP
    $FrontEnd.properties.privateIPAllocationMethod = "Static"

### <a name="step-4-create-a-back-end-pool-to-contain-the-cluster-nodes"></a>步驟 4： 建立集區 back\ 端包含叢集節點

您可以使用下列範例命令來建立集區 back\ 端

    $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
    $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
    $BackEnd.resourceId = "Backend1"
    $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
    $LoadBalancerProperties.backendAddressPools += $BackEnd

### <a name="step-5-add-a-probe"></a>步驟 5： 新增探查
探查才能偵測浮動的地址是在目前的叢集節點。

>[!NOTE]
>在下方的連接埠 VM 的永久位址探查查詢。  連接埠必須只回應主動節點上。 

    $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
    $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

    $lbprobe.ResourceId = "Probe1"
    $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
    $lbprobe.properties.protocol = "TCP"
    $lbprobe.properties.port = "59999"
    $lbprobe.properties.IntervalInSeconds = 5
    $lbprobe.properties.NumberOfProbes = 11

### <a name="step-5-add-the-load-balancing-rules"></a>步驟 5： 新增負載平衡規則
這個步驟建立負載平衡 1433年的 TCP 連接埠規則。  您可以修改通訊協定，並視需要連接埠。  您可以也重複此步驟多次額外的連接埠和 protcols 此 VIP 上。  請務必 EnableFloatingIP 設為 $true，因為這會告訴負載平衡器與原始 VIP 就地節點傳送封包。

    $LoadBalancerProperties.loadbalancingRules += $lbrule = new-object Microsoft.Windows.NetworkController.LoadBalancingRule
    $lbrule.properties = new-object Microsoft.Windows.NetworkController.LoadBalancingRuleProperties
    $lbrule.ResourceId = "Rules1"

    $lbrule.properties.frontendipconfigurations += $FrontEnd
    $lbrule.properties.backendaddresspool = $BackEnd 
    $lbrule.properties.protocol = "TCP"
    $lbrule.properties.frontendPort = $lbrule.properties.backendPort = 1433 
    $lbrule.properties.IdleTimeoutInMinutes = 4
    $lbrule.properties.EnableFloatingIP = $true
    $lbrule.properties.Probe = $lbprobe

### <a name="step-5-create-the-load-balancer-in-network-controller"></a>步驟 5： 建立網路控制器負載平衡器

若要建立負載平衡器，您可以使用下列命令範例。

    $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force

### <a name="step-6-add-the-cluster-nodes-to-the-backend-pool"></a>步驟 6： 端集區中新增叢集節點

新增兩個集區的成員，會顯示此範例中，但您也可以新增多個節點集區視需要叢集。

    # Cluster Node 1

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

    $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
    $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
    $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

建立負載平衡器並端集區中新增的網路介面之後，您就可以設定叢集。  如果您使用 Microsoft 容錯移轉叢集您可以繼續的下一個範例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>範例 2： 設定 Microsoft 容錯移轉叢集

您可以使用下列步驟來設定容錯移轉叢集。

### <a name="step-1-install-failover-clustering"></a>步驟 1： 安裝容錯

您可以使用下列命令範例安裝和設定容錯移轉叢集屬性。

    add-windowsfeature failover-clustering -IncludeManagementTools
    Import-module failoverclusters

    $ClusterName = "MyCluster"
   
    $ClusterNetworkName = "Cluster Network 1"
    $IPResourceName =  
    $ILBIP = “192.168.2.100” 

    $nodes = @("DB1", "DB2")

### <a name="step-2-create-the-cluster-on-one-node"></a>步驟 2： 建立一個節點叢集

您可以使用下列命令範例節點上建立叢集。

    New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]

### <a name="step-3-stop-the-cluster-resource"></a>步驟 3： 停止叢集資源

若要停止叢集資源，您可以使用下列命令範例。

    Stop-ClusterResource "Cluster Name" 

### <a name="step-4-set-the-cluster-ip-and-probe-port"></a>步驟 4： 設定叢集 IP 和探查連接埠
IP 位址必須符合先前的範例中, 使用的前端 ip 位址和探查連接埠必須符合的探查連接埠，在上一個範例。

    Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

### <a name="step-5-start-the-cluster-resources"></a>步驟 5： 開始叢集資源

您可以使用下列命令範例開始叢集資源。

    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 

### <a name="step-6-add-the-remaining-nodes"></a>步驟 6： 新增剩餘節點

您可以使用下列命令範例新增叢集節點。

    Add-ClusterNode $nodes[1]

完成的最後一個步驟，您叢集為作用中。 將在主動節點導向流量 vip 指定連接埠。