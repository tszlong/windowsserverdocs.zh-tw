---
title: Azure 中跨地區的叢集對叢集儲存體複本
description: 叢集至 Azure 中的叢集儲存體複寫跨區域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
manager: mchad
ms.openlocfilehash: f0658434f8a14080b56efa5cacbd20ebeb11438d
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864791"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Azure 中跨地區的叢集對叢集儲存體複本

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

您可以針對 Azure 中的跨區域應用程式，將叢集設定為叢集儲存體複本。 在下列範例中，我們使用雙節點的叢集，但叢集至叢集儲存體複本不受限於雙節點叢集。 下圖是兩個節點的儲存空間直接存取叢集，可彼此通訊、位於相同的網域中，而且是跨區域。

請觀看下列影片，以取得整個程式的完整逐步解說。
> [!video https://www.microsoft.com/videoplayer/embed/RE26xeW]

![Azure 遷移相同區域中的架構圖表展示 C2C SR-IOV。](media/Cluster-to-cluster-azure-cross-region/architecture.png)
> [!IMPORTANT]
> 所有參考的範例都是上述圖例的特定範例。


1. 在 Azure 入口網站中，請在兩個不同區域中建立 [資源群組](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)  。

    例如，**美國西部 2** 中的 **AZ2AZ** 和 **AZCROSS** **美國中西部** 中的 sr-iov，如上所示。

2. 建立兩個 [可用性設定組](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)，每個叢集在每個資源群組中各一個。
    - 可用性設定組 (**az2azAS1**) 的 (**SR-AZ2AZ**) 
    - 可用性設定組 (**azcross-**)  (的 **azcross**) 

3. 建立兩個虛擬網路
   - 在 **az2az**) 、具有一個子網和一個閘道子網的第一個資源 (群組中，建立 [虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**) 。
   - 在第二個資源群組中建立 [虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)  (**azcross-VNET**)  (**SR-azcross**) ，具有一個子網和一個閘道子網。

4. 建立兩個網路安全性群組
   - 在 **az2az**) 的第一個資源 (群組中建立 [網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**az2az-NSG**) 。
   - **(azcross**) ，在第二個資源群組中建立 [網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM) (**azcross-NSG**) 。

   在兩個網路安全性群組中新增一個適用于 RDP：3389的輸入安全性規則。 完成設定之後，您可以選擇移除此規則。

5. 在先前建立的資源群組中建立 Windows Server [虛擬機器](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) 。

   網域控制站 (**az2azDC**) 。 您可以選擇為網域控制站建立第三個可用性設定組，或在兩個可用性設定組的其中一個中新增網域控制站。 如果您要將此新增至針對兩個叢集建立的可用性設定組，請在 VM 建立期間指派標準的公用 IP 位址給它。
      - 安裝 Active Directory 網域服務。
      - 建立網域 (contoso.com) 
      - 建立具有系統管理員許可權的使用者 (contosoadmin) 

   在資源群組中建立兩個虛擬機器 (**az2az1**、 **az2az2**) **()** 在可用性設定組中使用虛擬網路 (**AZ2AZ-Vnet**) 和網路安全性群組 (**AZ2AZ-NSG**)  (**az2azAS1**) 。 在建立時，將標準公用 IP 位址指派給每部虛擬機器。
      - 將至少兩個受控磁片新增至每部電腦
      - 安裝容錯移轉叢集和儲存體複本功能

   在資源群組中建立兩個虛擬機器 (**azcross1**、 **azcross2**) **(AZCROSS**) 在可用性設定組中使用虛擬網路 (**AZCROSS-VNET**) 和網路安全性群組 (**AZCROSS-NSG**) ， **作為** (。 在建立本身的期間，將標準公用 IP 位址指派給每部虛擬機器
      - 將至少兩個受控磁片新增至每部電腦
      - 安裝容錯移轉叢集和儲存體複本功能

   將所有節點連接到網域，並為先前建立的使用者提供系統管理員許可權。

   將虛擬網路的 DNS 伺服器變更為網域控制站私人 IP 位址。
   - 在此範例中，網域控制站 **az2azDC** 具有私人 IP 位址 (10.3.0.8) 。 在虛擬網路中 (**az2az-vnet** 和 **azcross-VNET**) 變更 DNS 伺服器10.3.0.8。

     在此範例中，請將所有節點連線至 "contoso.com"，並將系統管理員許可權提供給 "contosoadmin"。
   - 從所有節點登入為 contosoadmin。

6. 建立叢集 (**SRAZC1**、 **SRAZCross**) 。

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
   > 針對每個叢集建立虛擬磁片和磁片區。 一個用於資料，另一個用於記錄。

8. 為每個叢集建立內部標準 SKU [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) (**azlbr1**、 **azlbazcross**) 。

   提供叢集 IP 位址作為負載平衡器的靜態私人 IP 位址。
      - azlbr1 => 前端 IP： 10.3.0.100 (從虛擬網路挑選未使用的 IP 位址 (**az2az-Vnet**) 子網) 
      - 建立每個負載平衡器的後端集區。 新增相關聯的叢集節點。
      - 建立健康情況探查：埠59999
      - 建立負載平衡規則：允許啟用浮動 IP 的 HA 埠。

   提供叢集 IP 位址作為負載平衡器的靜態私人 IP 位址。
      - azlbazcross => 前端 IP： 10.0.0.10 (從虛擬網路挑選未使用的 IP 位址 (**azcross-VNET**) 子網) 
      - 建立每個負載平衡器的後端集區。 新增相關聯的叢集節點。
      - 建立健康情況探查：埠59999
      - 建立負載平衡規則：允許啟用浮動 IP 的 HA 埠。

9. 建立 Vnet 對 Vnet 連線能力的 [虛擬網路閘道](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM) 。

   - 在第一個資源群組中建立第一個虛擬網路閘道 (**az2az-VNetGateway**) ， (**sr-iov**) 
   - 閘道類型 = VPN，VPN 類型 = 路由式

   - **(azcross** 的第二個資源群組中，建立第二個虛擬網路閘道 (**azcross-VNetGateway**) ) 
   - 閘道類型 = VPN，VPN 類型 = 路由式

   - 建立從第一個虛擬網路閘道到第二個虛擬網路閘道的 Vnet 對 Vnet 連線。 提供共用金鑰

   - 建立從第二個虛擬網路閘道到第一個虛擬網路閘道的 Vnet 對 Vnet 連線。 提供與上述步驟中所提供相同的共用金鑰。

10. 在每個叢集節點上，開啟埠 59999 (健康情況探查) 。

    在每個節點上執行下列命令：

    ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any
    ```

11. 指示叢集接聽埠59999上的健康情況探查訊息，並從目前擁有此資源的節點回應。

    請針對每個叢集，從叢集的任何一個節點執行一次。

    在我們的範例中，請務必根據您的設定值變更 "ILBIP"。 從任何一個節點 az2az1 執行下列命令 **az2az1** / **az2az2**

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}
    ```

12. 從任何一個節點 azcross1 執行下列命令 **azcross1** / **azcross2**
    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}
    ```

    請確定這兩個叢集都可以彼此連接/通訊。

    使用「容錯移轉叢集管理員」中的「連線到叢集」功能來連線至其他叢集，或檢查其他叢集是否回應目前叢集的其中一個節點。

    從我們使用的範例：
    ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
    ```
    ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1)
    ```

13. 為這兩個群集建立雲端見證。 在 Azure 中建立兩個 [儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**、**azcrosssa**) ，每個資源群組中的每個叢集各 **一個 (AZ2AZ**、 **sr-iov AZCROSS**) 。

    - 從 [存取金鑰] 複製儲存體帳戶名稱和金鑰
    - 從「容錯移轉叢集管理員」建立雲端見證，並使用上述的帳戶名稱和金鑰來建立它。

14. 繼續進行下一個步驟之前，請先執行叢集[驗證測試](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)

15. 啟動 Windows PowerShell，然後使用 [Test-SRTopology](/powershell/module/storagereplica/test-srtopology) Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以在僅查看需求的模式中，使用此 Cmdlet 進行快速測試，還可以在評估長時間執行效能的模式中使用。

16. 設定叢集對叢集儲存體複本。
    以雙向方式授與另一個叢集的存取權：

    從我們的範例：
    ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
    ```
    如果您使用的是 Windows Server 2016，則也請執行此命令：

    ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
    ```

17. 建立兩個群集的 SR-Partnership：</ol>

    - 針對叢集 **SRAZC1**
      - 磁片區位置：-c:\ClusterStorage\DataDisk1
      - 記錄檔位置：-g：
    - 針對叢集 **SRAZCross**
      - 磁片區位置：-c:\ClusterStorage\DataDiskCross
      - 記錄檔位置：-g：

執行命令：

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```
