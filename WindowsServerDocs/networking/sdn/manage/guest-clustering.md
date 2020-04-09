---
title: 虛擬網路中的客體叢集
description: 連線到虛擬網路的虛擬機器只能使用網路控制站已指派來在網路上進行通訊的 IP 位址。  需要浮動 IP 位址的叢集技術（例如 Microsoft 容錯移轉叢集）需要一些額外的步驟，才能正常運作。
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 8e9e5c81-aa61-479e-abaf-64c5e95f90dc
ms.author: grcusanz
author: AnirbanPaul
ms.date: 08/26/2018
ms.openlocfilehash: 6889b58f5d49a4932ef8277b11e1002e85606f3f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854451"
---
# <a name="guest-clustering-in-a-virtual-network"></a>虛擬網路中的客體叢集

>適用於：Windows Server (半年通道)、Windows Server 2016

連線到虛擬網路的虛擬機器只能使用網路控制站已指派來在網路上進行通訊的 IP 位址。  需要浮動 IP 位址的叢集技術（例如 Microsoft 容錯移轉叢集）需要一些額外的步驟，才能正常運作。

讓浮動 IP 得以連線的方法，是使用軟體 Load Balancer \(SLB\) 虛擬 IP \(VIP\)。  軟體負載平衡器必須在該 IP 的埠上設定健康情況探查，讓 SLB 將流量導向目前具有該 IP 的電腦。


## <a name="example-load-balancer-configuration"></a>範例：負載平衡器設定

這個範例假設您已經建立將成為叢集節點的 Vm，並將它們附加至虛擬網路。  如需指導方針，請參閱[建立 VM 並聯機至租使用者虛擬網路或 VLAN](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create-a-tenant-vm)。  

在此範例中，您將建立代表叢集的浮動 IP 位址的虛擬 IP 位址（192.168.2.100），並設定健康情況探查來監視 TCP 通訊埠59999，以判斷哪一個節點是作用中的節點。

1. 選取 VIP。<p>藉由指派 VIP IP 位址來準備，這可以是與叢集節點位於相同子網中任何未使用或保留的位址。  VIP 必須符合叢集的可重置位址。

   ```PowerShell
   $VIP = "192.168.2.100"
   $subnet = "Subnet2"
   $VirtualNetwork = "MyNetwork"
   $ResourceId = "MyNetwork_InternalVIP"
   ```

2. 建立負載平衡器屬性物件。

   ```PowerShell
   $LoadBalancerProperties = new-object Microsoft.Windows.NetworkController.LoadBalancerProperties
   ```

3. 建立 front\-的結束 IP 位址。

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

4. 建立後端\-結束集區，以包含叢集節點。

   ```PowerShell
   $BackEnd = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPool
   $BackEnd.properties = new-object Microsoft.Windows.NetworkController.LoadBalancerBackendAddressPoolProperties
   $BackEnd.resourceId = "Backend1"
   $BackEnd.resourceRef = "/loadBalancers/$ResourceId/backendAddressPools/$($BackEnd.resourceId)"
   $LoadBalancerProperties.backendAddressPools += $BackEnd
   ```

5. 新增探查以偵測可重置位址目前作用中的叢集節點。 

   >[!NOTE]
   >在下列定義的埠上針對 VM 的永久位址進行探查查詢。  埠只能回應使用中的節點。 

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

6. 新增 TCP 通訊埠1433的負載平衡規則。<p>您可以視需要修改通訊協定和埠。  您也可以為此 VIP 上的其他埠和 protcols 重複此步驟多次。  請務必將 EnableFloatingIP 設定為 $true，因為這會告訴負載平衡器將封包傳送至已有原始 VIP 的節點。

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

7. 在網路控制卡中建立負載平衡器。

   ```PowerShell
   $lb = New-NetworkControllerLoadBalancer -ConnectionUri $URI -ResourceId $ResourceId -Properties $LoadBalancerProperties -Force
   ```

8. 將叢集節點新增至後端集區。<p>您可以視需要將多個節點新增至集區。

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

   建立負載平衡器並將網路介面新增至後端集區之後，您就可以開始設定叢集。  

9. 選擇性如果您使用的是 Microsoft 容錯移轉叢集，請繼續進行下一個範例。 

## <a name="example-2-configuring-a-microsoft-failover-cluster"></a>範例2：設定 Microsoft 容錯移轉叢集

您可以使用下列步驟來設定容錯移轉叢集。

1. 安裝和設定容錯移轉叢集的屬性。

   ```PowerShell
   add-windowsfeature failover-clustering -IncludeManagementTools
   Import-module failoverclusters

   $ClusterName = "MyCluster"
   
   $ClusterNetworkName = "Cluster Network 1"
   $IPResourceName =  
   $ILBIP = "192.168.2.100" 

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

4. 設定叢集 IP 和探查埠。<p>IP 位址必須符合前一個範例中使用的前端 ip 位址，且探查埠必須符合上一個範例中的探查埠。

   ```PowerShell
   Get-ClusterResource "Cluster IP Address" | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}
   ```

5. 啟動叢集資源。

   ```PowerShell
    Start-ClusterResource "Cluster IP Address"  -Wait 60 
    Start-ClusterResource "Cluster Name"  -Wait 60 
   ```

6. 新增其餘的節點。

   ```PowerShell
   Add-ClusterNode $nodes[1]
   ```

_**您的叢集已啟用。**_ 進入指定埠上 VIP 的流量會導向作用中節點。

---