---
title: Azure 中相同地區的叢集對叢集儲存體複本
description: 在 Azure 中的相同區域內的叢集對叢集儲存體複寫
keywords: 儲存體複本，伺服器管理員、 Windows Server、 Azure、 叢集、 相同的區域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: 8dbfab96404f5c98b9861476c0bc654af1bda775
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829139"
---
# <a name="cluster-to-cluster-storage-replica-within-the-same-region-in-azure"></a>Azure 中相同地區的叢集對叢集儲存體複本
您可以在 Azure 中設定叢集對叢集儲存體複本在相同區域內。 在下列範例中，我們使用雙節點叢集，但不限於雙節點叢集的叢集對叢集儲存體複本。 下圖是兩個節點的儲存空間直接存取叢集，可以彼此通訊，會在相同的網域中，並在相同區域內。

觀看以下影片的程序的完整逐步解說。

第一個部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE26f2Y]

第二部分
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE269Pq]

![架構圖顯示在 Azure 中的叢集對叢集儲存體複本相同的區域內。](media\Cluster-to-cluster-azure-one-region\architecture.png)
> [!IMPORTANT]
> 參考的所有範例都專屬於上圖。

1. 建立[資源群組](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)在 Azure 入口網站中的區域中 (**SR AZ2AZ**中**美國西部 2**)。 
2. 建立兩個[可用性設定組](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)資源群組中 (**SR AZ2AZ**) 針對每個群集的上方，建立的一個。 
    a. 可用性設定組 (**az2azAS1**) b。 可用性設定組 (**az2azAS2**)
3. 建立[虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**az2az Vnet**) 的先前建立的資源群組中 (**SR AZ2AZ**)，需要至少一個子網路。 
4. 建立[網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**az2az NSG**)，並新增一個 RDP:3389 輸入的安全性規則。 您可以選擇移除此規則，一旦完成您的設定。 
5. 建立 Windows Server[虛擬機器](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)先前建立的資源群組中 (**SR AZ2AZ**)。 使用先前建立的虛擬網路 (**az2az Vnet**) 和網路安全性群組 (**az2az NSG**)。 
   
   網域控制站 (**az2azDC**)。 您可以選擇建立您的網域控制站的第三個可用性設定組，或在其中兩個可用性設定組中新增的網域控制站。 如果您要新增這兩個叢集所建立的可用性設定組，將它指派標準公用 IP 位址建立 VM 時。 
   - 安裝 Active Directory 網域服務。
   - 建立網域 (Contoso.com)
   - 建立具有系統管理員權限 (contosoadmin) 的使用者 
   - 建立兩部虛擬機器 (**az2az1**， **az2az2**) 中的第一個可用性設定 (**az2azAS1**)。 將標準公用 IP 位址指派給每個虛擬機器中，期間本身。
   - 將至少 2 個受控的磁碟新增至每一部機器
   - 安裝容錯移轉叢集和儲存體複本功能
   - 建立兩部虛擬機器 (**az2az3**， **az2az4**) 中的第二個可用性設定 (**az2azAS2**)。 將標準公用 IP 位址指派給每個虛擬機器中，期間本身。 
   - 將至少 2 個受控的磁碟新增至每個機器。 
   - 安裝容錯移轉叢集和儲存體複本的功能。 
   
6. 連線到網域的所有節點，並以先前建立的使用者提供系統管理員權限。 

7. 變更網域控制站私用 IP 位址的虛擬網路的 DNS 伺服器。 
8. 在我們的範例中，網域控制站**az2azDC**具有私人 IP 位址 (10.3.0.8)。 虛擬網路中 (**az2az Vnet**) 變更 10.3.0.8 的 DNS 伺服器。 連接到"Contoso.com"的所有節點，並提供"contosoadmin"的系統管理員權限。
   - 從所有節點的 contosoadmin 身分登入。 
    
9. 建立叢集 (**SRAZC1**， **SRAZC2**)。 以下是我們的範例 PowerShell 命令
```PowerShell
    New-Cluster -Name SRAZC1 -Node az2az1,az2az2 – StaticAddress 10.3.0.100
```
```PowerShell
    New-Cluster -Name SRAZC2 -Node az2az3,az2az4 – StaticAddress 10.3.0.101
```
10. 啟用儲存空間直接存取
```PowerShell
    Enable-clusterS2D
```   
   
   每個叢集建立虛擬磁碟和磁碟區。 其中的資料，另一個記錄檔。 
   
11. 建立內部的標準 SKU[負載平衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)每個叢集 (**azlbr1**，**azlbr2**)。 
   
   提供的叢集 IP 位址為靜態私人 IP 位址的負載平衡器。
   - azlbr1 = > 前端 IP:10.3.0.100 (挑選未使用的 IP 位址與虛擬網路 (**az2az Vnet**) 子網路)
   - 建立每個負載平衡器後端集區。 加入相關聯的叢集節點。
   - 建立健康狀態探查： 連接埠 59999
   - 建立負載平衡規則：允許 HA 連接埠與啟用浮動 IP。 
   
   提供的叢集 IP 位址為靜態私人 IP 位址的負載平衡器。
   - azlbr2 = > 前端 IP:10.3.0.101 (挑選未使用的 IP 位址與虛擬網路 (**az2az Vnet**) 子網路)
   - 建立每個負載平衡器後端集區。 加入相關聯的叢集節點。
   - 建立健康狀態探查： 連接埠 59999
   - 建立負載平衡規則：允許 HA 連接埠與啟用浮動 IP。 
   
12. 每個叢集節點，開啟連接埠 59999 （健康情況探查）。 
   
    每個節點上執行下列命令：
```PowerShell
netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
```   
13. 指示要接聽的連接埠 59999 上的 健康情況探查訊息，並回應從目前擁有這項資源的節點叢集。 執行一次從任何一個叢集節點，每個叢集。 
    
    在我們的範例，請務必變更"ILBIP 「 根據您的組態值。 執行下列命令，從任何一個節點**az2az1**/**az2az2**:

    ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}
    ```

14. 執行下列命令，從任何一個節點**az2az3**/**az2az4**。 

    ```PowerShell
    $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
    $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
    $ILBIP = "10.3.0.101" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
    [int]$ProbePort = 59999
    Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
    ```   
   請確定這兩個叢集可以連接 / 彼此通訊。 

   請使用容錯移轉叢集管理員中的 「 連線到叢集 」 功能連線到另一個叢集，或檢查另一個叢集會回應來自其中一個目前的叢集節點。  
   
   ```PowerShell
     Get-Cluster -Name SRAZC1 (ran from az2az3)
   ```
   ```PowerShell
     Get-Cluster -Name SRAZC2 (ran from az2az1)
   ```   

15. 建立這兩個叢集的雲端見證。 建立兩個[儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)(**az2azcw**， **az2azcw2**) 在相同的資源群組中每個叢集的 azure 一個 (**SR AZ2AZ**)。

    - 從 [存取金鑰] 中複製的儲存體帳戶名稱和金鑰
    - 建立雲端見證，從 [容錯移轉叢集管理員]，並使用上述的帳戶名稱和金鑰來建立它。

16. 執行[叢集驗證測試](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)再繼續進行下一個步驟。

17. 啟動 Windows PowerShell，然後使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以使用此 cmdlet 僅限需求的模式，快速測試，以及長時間執行的效能評估模式。

18. 設定叢集對叢集儲存體複本。
   
   授與存取權從一個叢集至兩個方向的另一個叢集：

   在我們的範例：

   ```PowerShell
      Grant-SRAccess -ComputerName az2az1 -Cluster SRAZC2
   ```
如果您使用 Windows Server 2016 也執行這個命令：

   ```PowerShell
      Grant-SRAccess -ComputerName az2az3 -Cluster SRAZC1
   ```   
   
19. 建立叢集 SRPartnership:</ol>

 - 叢集**SRAZC1**。
   - 磁碟區的位置:-c:\ClusterStorage\DataDisk1
   - 記錄位置:-g:
 - 叢集**SRAZC2**
    - 磁碟區的位置:-c:\ClusterStorage\DataDisk2
    - 記錄位置:-g:

執行下列命令：

```PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName **SRAZC2** -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDisk2 -DestinationLogVolumeName  g:
```