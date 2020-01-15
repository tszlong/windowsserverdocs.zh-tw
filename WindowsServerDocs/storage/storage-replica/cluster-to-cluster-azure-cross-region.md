---
title: Azure 中跨地區的叢集對叢集儲存體複本
description: Azure 中跨區域的叢集對叢集儲存體複寫
keywords: 儲存體複本，伺服器管理員，Windows Server，Azure，叢集，跨區域，不同區域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 806857d5de067c0f4640344ed80338b474dd758e
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950065"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Azure 中跨地區的叢集對叢集儲存體複本

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

您可以在 Azure 中將叢集設定為跨區域應用程式的叢集儲存體複本。 在下列範例中，我們使用雙節點叢集，但叢集對叢集儲存體複本不限於雙節點叢集。 下圖是兩個節點的儲存空間直接存取叢集，可以彼此通訊、位於相同的網域中，而且是跨區域。

如需完整的程式逐步解說，請觀看下列影片。
> [!video https://www.microsoft.com/videoplayer/embed/RE26xeW]

![架構圖展示 C2C SR in Azure 遷移相同的區域。](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> 所有參考的範例都是上述圖例特有的。


1. 在 Azure 入口網站中，在兩個不同的區域中建立[資源群組](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)。

    例如，**美國西部 2**中的**sr AZ2AZ**和**美國中西部**中的**sr AZCROSS** ，如上所示。

2. 建立兩個[可用性設定](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)組，每個叢集的每個資源群組各一個。
    - 中的可用性設定組（**az2azAS1**）（**AZ2AZ**）
    - 可用性設定組（**azcross**）（**azcross**）

3. 建立兩個虛擬網路
   - 在第一個資源群組（**sr-iov-az2az**）中建立[虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)（**az2az-Vnet**），具有一個子網和一個閘道子網。
   - 在第二個資源群組（**sr-iov-azcross**）中建立[虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)（**azcross-VNET**），具有一個子網和一個閘道子網。

4. 建立兩個網路安全性群組
   - 在第一個資源群組（**sr-iov-az2az**）中建立[網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)（**az2az-NSG**）。
   - 在第二個資源群組（**sr-iov-azcross**）中建立[網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)（**azcross-NSG**）。

   將 RDP：3389的一個輸入安全性規則新增至這兩個網路安全性群組。 完成安裝之後，您可以選擇移除此規則。

5. 在先前建立的資源群組中建立 Windows Server[虛擬機器](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)。

   網域控制站（**az2azDC**）。 您可以選擇為您的網域控制站建立第三個可用性設定組，或在這兩個可用性設定組的其中一個新增網域控制站。 如果您要將此新增至針對兩個叢集所建立的可用性設定組，請在建立 VM 期間指派標準的公用 IP 位址給它。
      - 安裝 Active Directory 網域服務。
      - 建立網域（contoso.com）
      - 建立具有系統管理員許可權的使用者（contosoadmin）

   使用可用性設定組（**AZ2AZ**）中的虛擬網路（**AZ2AZ-Vnet**）和網路安全性群組（**NSG-az2azAS1**），在資源群組（**SR-AZ2AZ**）中建立兩部虛擬機器（**az2az1**、 **az2az2**）。 在建立時，將標準公用 IP 位址指派給每部虛擬機器。
      - 將至少兩個受控磁片新增至每部電腦
      - 安裝容錯移轉叢集和儲存體複本功能

   使用可用性設定組（**AZCROSS-AS**）中的虛擬網路（**AZCROSS-VNET**）和網路安全性群組（**NSG-AZCROSS**），在資源群組（**SR-AZCROSS**）中建立兩部虛擬機器（**azcross1**、 **azcross2**）。 在建立時將標準公用 IP 位址指派給每部虛擬機器
      - 在每部電腦上至少新增兩個受控磁片
      - 安裝容錯移轉叢集和儲存體複本功能

   將所有節點連接到網域，並將系統管理員許可權提供給先前建立的使用者。

   將虛擬網路的 DNS 伺服器變更為 [網域控制站] [私人 IP 位址]。
   - 在此範例中，網域控制站**az2azDC**具有私人 IP 位址（10.3.0.8）。 在 [虛擬網路（**az2az-vnet**和**azcross-vnet**）] [變更 DNS 伺服器 10.3.0.8]。 

     在此範例中，將所有節點連接至 "contoso.com"，並將系統管理員許可權提供給 "contosoadmin"。
   - 從所有節點以 contosoadmin 的身分登入。 
 
6. 建立叢集（**SRAZC1**、 **SRAZCross**）。

   以下是範例的 PowerShell 命令
   ```powershell
      New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```powershell
      New-Cluster -Name SRAZCross -Node azcross1,azcross2 –StaticAddress 10.0.0.10
   ```

7. 啟用儲存空間直接存取。

   ```powershell
      Enable-clusterS2D
   ```

   > [!NOTE]
   > 針對每個叢集，建立虛擬磁片和磁片區。 一個用於資料，另一個用於記錄檔。

8. 為每個叢集建立內部標準 SKU [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) （**azlbr1**、 **azlbazcross**）。

   提供叢集 IP 位址做為負載平衡器的靜態私人 IP 位址。
      - azlbr1 = > 前端 IP：10.3.0.100 （從虛擬網路（**az2az-Vnet**）子網挑選未使用的 ip 位址）
      - 為每個負載平衡器建立後端集區。 新增相關聯的叢集節點。
      - 建立健康情況探查：埠59999
      - 建立負載平衡規則：允許 HA 埠，並啟用浮動 IP。

   提供叢集 IP 位址做為負載平衡器的靜態私人 IP 位址。 
      - azlbazcross = > 前端 IP：10.0.0.10 （從虛擬網路（**azcross-VNET**）子網挑選未使用的 ip 位址）
      - 為每個負載平衡器建立後端集區。 新增相關聯的叢集節點。
      - 建立健康情況探查：埠59999
      - 建立負載平衡規則：允許 HA 埠，並啟用浮動 IP。 

9. 建立 Vnet 對 Vnet 連線的[虛擬網路閘道](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM)。

   - 在第一個資源群組（**sr-iov-az2az**）中建立第一個虛擬網路閘道（**az2az-VNetGateway**）
   - 閘道類型 = VPN，而 VPN 類型 = 路由式

   - 在第二個資源群組（**sr-iov-azcross**）中建立第二個虛擬網路閘道（**azcross-VNetGateway**）
   - 閘道類型 = VPN，而 VPN 類型 = 路由式

   - 建立從第一個虛擬網路閘道到第二個虛擬網路閘道的 Vnet 對 Vnet 連線。 提供共用金鑰

   - 建立從第二個虛擬網路閘道連線到第一個虛擬網路閘道的 Vnet 對 Vnet 連線。 提供與上述步驟中所提供相同的共用金鑰。 

10. 在每個叢集節點上，開啟埠59999（健康情況探查）。

    在每個節點上執行下列命令：

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
    ```

11. 指示叢集接聽埠59999上的健康情況探查訊息，並從目前擁有此資源的節點回應。

    針對每個叢集，從叢集的任何一個節點執行一次。 
    
    在我們的範例中，請務必根據您的設定值來變更 "ILBIP"。 從任何一個節點**az2az1**執行下列命令/**az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

12. 從任何一個節點**azcross1**執行下列命令/**azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```

    請確定這兩個叢集都可以彼此連接/通訊。

    使用 [容錯移轉叢集管理員] 中的 [連接到叢集] 功能來連線到其他叢集，或檢查其他叢集是否回應目前叢集的其中一個節點。

    從我們所使用的範例中：
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
    ```

13. 為這兩個叢集建立雲端見證。 在 Azure 中建立兩個[儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)（**az2azcw**、**azcrosssa**），每個資源群組中的每個叢集各一個（**sr-iov-AZ2AZ**、 **sr AZCROSS**）。
   
    - 從「存取金鑰」複製儲存體帳戶名稱和金鑰
    - 從「容錯移轉叢集管理員」建立雲端見證，並使用上述的帳戶名稱和金鑰來建立。 

14. 繼續進行下一個步驟之前，請先執行叢集[驗證測試](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)

15. 啟動 Windows PowerShell，然後使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以在僅查看需求的模式中，使用此 Cmdlet 進行快速測試，還可以在評估長時間執行效能的模式中使用。
 
16. 設定叢集對叢集儲存體複本。
    將一個叢集的存取權授與另一個叢集（雙向）：

    從我們的範例中：
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    如果您使用 Windows Server 2016，則也要執行此命令：

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. 建立兩個叢集的 SR 合作關係：</ol>

    - 針對叢集**SRAZC1**
      - 磁片區位置：-c:\ClusterStorage\DataDisk1
      - 記錄檔位置：-g：
    - 針對叢集**SRAZCross**
      - 磁片區位置：-c:\ClusterStorage\DataDiskCross
      - 記錄檔位置：-g：

執行命令：

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```