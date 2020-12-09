---
title: 叢集至 Azure 中相同區域內的叢集儲存體複本
description: 叢集至 Azure 中相同區域內的叢集儲存體複寫
author: arduppal
ms.author: arduppal
ms.date: 04/26/2019
ms.topic: article
manager: mchad
ms.openlocfilehash: 329650a599d9092818bf6923375c03a8d5996825
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96866177"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>叢集至 Azure 中相同區域內的叢集儲存體複本

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

您可以將叢集設定為在 Azure 中的相同區域內進行叢集儲存體複寫。 在下列範例中，我們使用雙節點的叢集，但叢集至叢集儲存體複本不受限於雙節點叢集。 下圖是兩個節點的儲存空間直接存取叢集，可彼此通訊、位於相同的網域，以及位於相同的區域內。

請觀看下列影片，以取得整個程式的完整逐步解說。

第一部
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE26f2Y]

第二部分
> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE269Pq]

![架構圖表會在相同區域內展示 Azure 中的叢集對叢集儲存體複本。](media/Cluster-to-cluster-azure-one-region/architecture.png)
> [!IMPORTANT]
> 所有參考的範例都是上述圖例的特定範例。

1. 在 **美國西部 2**) 中 (**AZ2AZ** 的區域 Azure 入口網站中建立 [資源群組](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)。
2. 在上述建立的 (**AZ2AZ**) 的資源群組中建立兩個 [可用性設定](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)組，每個叢集各一個。
    a. 可用性設定組 (**az2azAS1**) b。 可用性設定組 (**az2azAS2**) 
3. 在先前建立的資源群組中建立 [虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM) (**az2az-Vnet**)  (**SR-az2az**) ，至少有一個子網。
4. 建立 (**az2az-NSG**) 的 [網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)，並為 RDP：3389新增一個輸入安全性規則。 完成設定之後，您可以選擇移除此規則。
5. 在先前建立的資源群組中建立 Windows Server [虛擬機器](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM) (**SR AZ2AZ**) 。 使用先前建立的虛擬網路 (**az2az-Vnet**) 和網路安全性群組 (**az2az-NSG**) 。

   網域控制站 (**az2azDC**) 。 您可以選擇為網域控制站建立第三個可用性設定組，或在其中一個可用性設定組中新增網域控制站。 如果您要將此新增至針對兩個叢集建立的可用性設定組，請在 VM 建立期間指派標準的公用 IP 位址給它。
   - 安裝 Active Directory 網域服務。
   - 建立網域 (Contoso.com) 
   - 建立具有系統管理員許可權的使用者 (contosoadmin) 
   - 在第一個可用性設定組中建立兩個虛擬機器 (**az2az1**、 **Az2az2**)  (**az2azAS1**) 。 在建立時，將標準公用 IP 位址指派給每部虛擬機器。
   - 將至少2個受控磁片新增至每部電腦
   - 安裝容錯移轉叢集和儲存體複本功能
   -  (**az2azAS2**) ，在第二個可用性設定組中建立兩個虛擬機器 (**az2az3**、 **az2az4**) 。 在建立時，將標準公用 IP 位址指派給每部虛擬機器。
   - 將至少2個受控磁片新增至每部電腦。
   - 安裝容錯移轉叢集和儲存體複本功能。

6. 將所有節點連接到網域，並為先前建立的使用者提供系統管理員許可權。

7. 將虛擬網路的 DNS 伺服器變更為網域控制站私人 IP 位址。
8. 在我們的範例中，網域控制站 **az2azDC** 有私人 IP 位址 (10.3.0.8) 。 在虛擬網路 (**az2az-Vnet**) 變更 DNS 伺服器10.3.0.8。 將所有節點連線至 "Contoso.com"，並將系統管理員許可權提供給 "contosoadmin"。
   - 從所有節點登入為 contosoadmin。

9. 建立叢集 (**SRAZC1**、 **SRAZC2**) 。
   以下是範例的 PowerShell 命令
   ```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 –StaticAddress 10.3.0.100
   ```
   ```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 –StaticAddress 10.3.0.101
   ```
10. 啟用儲存空間直接存取
    ```PowerShell
    Enable-clusterS2D
    ```

    針對每個叢集建立虛擬磁片和磁片區。 一個用於資料，另一個用於記錄。

11. 為每個叢集建立內部標準 SKU [Load Balancer](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM) (**azlbr1**、**azlbr2**) 。

    提供叢集 IP 位址作為負載平衡器的靜態私人 IP 位址。
    - azlbr1 => 前端 IP： 10.3.0.100 (從虛擬網路挑選未使用的 IP 位址 (**az2az-Vnet**) 子網) 
    - 建立每個負載平衡器的後端集區。 新增相關聯的叢集節點。
    - 建立健康情況探查：埠59999
    - 建立負載平衡規則：允許啟用浮動 IP 的 HA 埠。

    提供叢集 IP 位址作為負載平衡器的靜態私人 IP 位址。
    - azlbr2 => 前端 IP： 10.3.0.101 (從虛擬網路挑選未使用的 IP 位址 (**az2az-Vnet**) 子網) 
    - 建立每個負載平衡器的後端集區。 新增相關聯的叢集節點。
    - 建立健康情況探查：埠59999
    - 建立負載平衡規則：允許啟用浮動 IP 的 HA 埠。

12. 在每個叢集節點上，開啟埠 59999 (健康情況探查) 。

    在每個節點上執行下列命令：
    ```PowerShell
    netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any
    ```
13. 指示叢集接聽埠59999上的健康情況探查訊息，並從目前擁有此資源的節點回應。
    請針對每個叢集，從叢集的任何一個節點執行一次。

    在我們的範例中，請務必根據您的設定值變更 "ILBIP"。 從任何一個節點 **az2az1** az2az2 中執行下列命令 / ****：

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}
    ```

14. 從任何一個節點 **az2az3** / **az2az4** 執行下列命令。

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"ProbeFailureThreshold"=5;"EnableDhcp"=0}
    ```
    請確定這兩個叢集都可以彼此連接/通訊。

    使用「容錯移轉叢集管理員」中的「連線到叢集」功能來連線至其他叢集，或檢查其他叢集是否回應目前叢集的其中一個節點。

    ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
    ```
    ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
    ```

15. 為這兩個群集建立雲端見證。 在 azure 中建立兩個 [儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM) (**az2azcw**、 **az2azcw2**) ，在相同資源群組中的每個 **叢集 (AZ2AZ**) 。

    - 從 [存取金鑰] 複製儲存體帳戶名稱和金鑰
    - 從「容錯移轉叢集管理員」建立雲端見證，並使用上述的帳戶名稱和金鑰來建立它。

16. 請先執行叢集 [驗證測試](../../failover-clustering/create-failover-cluster.md#validate-the-configuration) ，再繼續進行下一個步驟。

17. 啟動 Windows PowerShell，然後使用 [Test-SRTopology](/powershell/module/storagereplica/test-srtopology) Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以使用僅限需求模式中的 Cmdlet 來進行快速測試，以及長時間執行的效能評估模式。

18. 設定叢集對叢集儲存體複本。

    以雙向方式授與另一個叢集的存取權：

    在我們的範例中：

    ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
    ```
    如果您使用的是 Windows Server 2016，則也請執行此命令：

    ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
    ```

19. 建立叢集的 SRPartnership：</ol>

    - 適用于叢集 **SRAZC1**。
    - 磁片區位置：-c:\ClusterStorage\DataDisk1
    - 記錄檔位置：-g：
    - 針對叢集 **SRAZC2**
    - 磁片區位置：-c:\ClusterStorage\DataDisk2
    - 記錄檔位置：-g：

執行以下命令：

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```
