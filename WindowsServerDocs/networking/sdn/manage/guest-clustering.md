---
title: 虛擬網路中的客體叢集
description: 若要使用網路控制站已指派給網路上通訊的 IP 位址，只能連線到虛擬網路的虛擬機器。  需要浮動 IP 位址，例如 Microsoft 容錯移轉叢集的叢集技術需要一些額外的步驟，才能正確運作。
manager: dougkim
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
ms.date: 08/26/2018
ms.openlocfilehash: 97c20fd07d06b609686daf4d6308a9f248873036
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446338"
---
# <a name="guest-clustering-in-a-virtual-network"></a>虛擬網路中的客體叢集

>適用於：Windows Server （半年通道），Windows Server 2016

若要使用網路控制站已指派給網路上通訊的 IP 位址，只能連線到虛擬網路的虛擬機器。  需要浮動 IP 位址，例如 Microsoft 容錯移轉叢集的叢集技術需要一些額外的步驟，才能正確運作。

讓浮動 IP 可連線的方法是使用軟體負載平衡器\(SLB\)虛擬 IP \(VIP\)。  軟體負載平衡器必須設有該 ip 連接埠上的健康狀態探查，以便 SLB 會將目前擁有該 IP 之電腦的流量導向。


## <a name="example-load-balancer-configuration"></a>範例：負載平衡器組態

這個範例假設您已建立將成為叢集節點，和它們連接至虛擬網路的 Vm。  如需指引，請參閱[建立 VM 並連線到租用戶虛擬網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此範例中建立虛擬 IP 位址 (192.168.2.100) 來代表浮動 IP 位址的叢集，並設定健全狀況探查來監視 TCP 連接埠 59999，以判斷哪一個節點為作用。

1. 選取的 VIP。<p>準備將 VIP IP 位址，它可以是任何未使用或保留位址與叢集節點位於相同子網路指派。  VIP 必須符合叢集的浮動的位址。

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. 建立負載平衡器的屬性物件。

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. 建立 front\-結束 IP 位址。

   ```PowerShell
   $LoadBalancerProperties.frontendipconfigurations += $FrontEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfiguration
   $FrontEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerFrontendIpConfigurationProperties
   $FrontEnd.resourceId = "Frontend1"
   $FrontEnd.resourceRef = "/loadBalancers/$ResourceId/frontendIPConfigurations/$($FrontEnd.resourceId)"
   $FrontEnd.properties.subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $FrontEnd.properties.subnet.ResourceRef = "/VirtualNetworks/MyNetwork/Subnets/Subnet2"
   $FrontEnd.properties.privateIPAddress = $VIP
   $FrontEnd.properties.privateIPAllocationMethod = "Static"
   ```

4. 建立備份\-結尾以包含叢集中節點的集區。

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. 新增探查偵測浮動的位址是目前使用中的叢集節點。 

   >[!NOTE]
   >探查查詢對 VM 的永久位址，在下面定義的連接埠。  作用中節點上必須只會回應連接埠。 

   ```PowerShell
   $LoadBalancerProperties.probes += $lbprobe = new-object Microsoft.Windows.NetworkController.LoadBalancerProbe
   $lbprobe.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerProbeProperties

   $lbprobe.ResourceId = "Probe1"
   $lbprobe.resourceRef = "/loadBalancers/$ResourceId/Probes/$($lbprobe.resourceId)"
   $lbprobe.properties.protocol = "TCP"
   $lbprobe.properties.port = "59999"
   $lbprobe.properties.IntervalInSeconds = 5
   $lbprobe.properties.NumberOfProbes = 11
   ```

6. 新增負載平衡規則的 TCP 通訊埠 1433年。<p>您可以修改的通訊協定和所需的連接埠。  您也可以重複此步驟多次其他連接埠和 protcols 此 VIP 上。  請務必 EnableFloatingIP 會設定為 $true，因為這會告訴負載平衡器將封包傳送至原始的 VIP 就地具有的節點。

   ```PowerShell
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
   ```

7. 網路控制卡中建立負載平衡器。

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. 叢集將節點新增至後端集區。<p>您可以新增任意數目的節點集區所需的叢集。

   ```PowerShell
   # Cluster Node 1

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode1_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force

    # Cluster Node 2

   $nic = get-networkcontrollernetworkinterface  -connectionuri $uri -resourceid "ClusterNode2_Network-Adapter"
   $nic.properties.IpConfigurations[0].properties.LoadBalancerBackendAddressPools += $lb.properties.backendaddresspools[0]
   $nic = new-networkcontrollernetworkinterface  -connectionuri $uri -resourceid $nic.resourceid -properties $nic.properties -force
   ```

   一旦您已建立負載平衡器，並加入後端集區中的網路介面，您就準備好要設定叢集。  

9. （選擇性）如果您使用 Microsoft 容錯移轉叢集，繼續進行下一個範例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>範例 2：設定 Microsoft 容錯移轉叢集

您可以使用下列步驟來設定容錯移轉叢集。

1. 安裝和設定容錯移轉叢集的屬性。

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = “192.168.2.100” 

   $nodes = @("DB1", "DB2")
   ```

2. 在一個節點上建立叢集。

   ```PowerShell
   New-Cluster -Name $ClusterName -NoStorage -Node $nodes[0]
   ```

3. 停止叢集資源。

   ```PowerShell
   Stop-ClusterResource "Cluster Name" 
   ```

4. 設定叢集 IP 和探查連接埠。<p>IP 位址必須符合在上一個範例中，使用的前端 ip 位址及探查連接埠必須符合在上述範例中的探查連接埠。

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. 啟動叢集資源。

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. 將其餘節點。

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**您的叢集才有作用。** _ 移至 VIP 上指定的連接埠的流量會導向在作用中節點。

---