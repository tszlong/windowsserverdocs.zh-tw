---
title: Azure 中跨地區的叢集對叢集儲存體複本
description: 叢集對叢集儲存體複寫跨 Azure 區域
keywords: 儲存體複本，伺服器管理員、 Windows Server，Azure，叢集中，跨不同區域的區域
author: arduppal
ms.author: arduppal
ms.date: 12/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-replica
manager: mchad
ms.openlocfilehash: d9999f786639ff4aa303ed34ade14849cda8feec
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475904"
---
# <a name="cluster-to-cluster-storage-replica-cross-region-in-azure"></a>Azure 中跨地區的叢集對叢集儲存體複本

> 適用於：Windows Server 2019，Windows Server 2016 中，Windows Server （半年通道）

您可以在 Azure 中設定叢集對叢集儲存體複本，跨區域的應用程式。 在下列範例中，我們使用雙節點叢集，但不限於雙節點叢集的叢集對叢集儲存體複本。 下圖是兩個節點的儲存空間直接存取叢集可以彼此通訊、 位於相同的網域，而且跨區域。

請觀看以下影片的程序的完整逐步解說。
> [!video https://www.microsoft.com/en-us/videoplayer/embed/RE26xeW]

![架構圖展示 C2C SR 中 Azure 具有相同的區域。](media\Cluster-to-cluster-azure-cross-region\architecture.png)
> [!IMPORTANT]
> 參考的所有範例都專屬於上圖。


1. 在 Azure 入口網站中，建立[資源群組](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup)兩個不同區域中。

    例如， **SR AZ2AZ**中**美國西部 2**並**SR AZCROSS**中**美國中西部**，如上所示。

2. 建立兩個[可用性設定組](https://ms.portal.azure.com/#create/Microsoft.AvailabilitySet-ARM)，其中每個叢集的每個資源群組中。
    - 可用性設定組 (**az2azAS1**) 中 (**SR AZ2AZ**)
    - 可用性設定組 (**azcross-AS**) 中 (**SR AZCROSS**)

3. 建立兩個虛擬網路
   - 建立[虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**az2az Vnet**) 中的第一個資源群組 (**SR AZ2AZ**)，有一個子網路和一個閘道子網路。
   - 建立[虛擬網路](https://ms.portal.azure.com/#create/Microsoft.VirtualNetwork-ARM)(**azcross VNET**) 中的第二個資源群組 (**SR AZCROSS**)，有一個子網路和一個閘道子網路。

4. 建立兩個網路安全性群組
   - 建立[網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**az2az NSG**) 中的第一個資源群組 (**SR AZ2AZ**)。
   - 建立[網路安全性群組](https://ms.portal.azure.com/#create/Microsoft.NetworkSecurityGroup-ARM)(**azcross NSG**) 中的第二個資源群組 (**SR AZCROSS**)。

   加入這兩個網路安全性群組的 RDP:3389 的一項輸入的安全性規則。 您可以選擇移除此規則，一旦完成您的設定。

5. 建立 Windows Server[虛擬機器](https://ms.portal.azure.com/#create/Microsoft.WindowsServer2016Datacenter-ARM)先前建立的資源群組中。

   網域控制站 (**az2azDC**)。 您可以選擇建立您的網域控制站的第 3 個可用性設定組，或在其中兩個可用性設定組中新增的網域控制站。 如果您要新增這兩個叢集所建立的可用性設定組，將它指派標準公用 IP 位址建立 VM 時。
      - 安裝 Active Directory 網域服務。
      - 建立網域 (contoso.com)
      - 建立具有系統管理員權限 (contosoadmin) 的使用者

   建立兩部虛擬機器 (**az2az1**， **az2az2**) 中的資源群組 (**SR AZ2AZ**) 使用虛擬網路 (**az2az Vnet**) 和網路安全性群組 (**az2az NSG**) 在 可用性設定 (**az2azAS1**)。 將標準公用 IP 位址指派給每個虛擬機器中，期間本身。
      - 將至少兩個受控的磁碟新增至每一部機器
      - 安裝容錯移轉叢集和儲存體複本功能

   建立兩部虛擬機器 (**azcross1**， **azcross2**) 中的資源群組 (**SR AZCROSS**) 使用虛擬網路 (**azcross VNET**) 和網路安全性群組 (**azcross NSG**) 在 可用性設定 (**azcross-AS**)。 本身建立期間，指派給每個虛擬機器的標準公用 IP 位址
      - 將兩個以上的受控的磁碟新增至每一部機器
      - 安裝容錯移轉叢集和儲存體複本功能

   連線到網域的所有節點，並以先前建立的使用者提供系統管理員權限。

   變更網域控制站私用 IP 位址的虛擬網路的 DNS 伺服器。
   - 在範例中，網域控制站**az2azDC**具有私人 IP 位址 (10.3.0.8)。 虛擬網路中 (**az2az Vnet**並**azcross VNET**) 變更 10.3.0.8 的 DNS 伺服器。 

    在此範例中，連接到"contoso.com"的所有節點，並提供"contosoadmin"的系統管理員權限。
   - 從所有節點的 contosoadmin 身分登入。 
 
6. 建立叢集 (**SRAZC1**， **SRAZCross**)。

   以下是範例 PowerShell 命令
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
   > 每個叢集建立虛擬磁碟和磁碟區。 其中的資料，另一個記錄檔。

8. 建立內部的標準 SKU[負載平衡器](https://ms.portal.azure.com/#create/Microsoft.LoadBalancer-ARM)每個叢集 (**azlbr1**， **azlbazcross**)。

   提供的叢集 IP 位址為靜態私人 IP 位址的負載平衡器。
      - azlbr1 = > 前端 IP:10.3.0.100 (挑選未使用的 IP 位址與虛擬網路 (**az2az Vnet**) 子網路)
      - 建立每個負載平衡器後端集區。 加入相關聯的叢集節點。
      - 建立健康狀態探查： 連接埠 59999
      - 建立負載平衡規則：允許 HA 連接埠與啟用浮動 IP。

   提供的叢集 IP 位址為靜態私人 IP 位址的負載平衡器。 
      - azlbazcross = > 前端 IP:10.0.0.10 (挑選未使用的 IP 位址與虛擬網路 (**azcross VNET**) 子網路)
      - 建立每個負載平衡器後端集區。 加入相關聯的叢集節點。
      - 建立健康狀態探查： 連接埠 59999
      - 建立負載平衡規則：允許 HA 連接埠與啟用浮動 IP。 

9. 建立[虛擬網路閘道](https://ms.portal.azure.com/#create/Microsoft.VirtualNetworkGateway-ARM)的 Vnet 對 Vnet 連線能力。

 - 建立第一個虛擬網路閘道 (**az2az VNetGateway**) 的第一個資源群組中 (**SR AZ2AZ**)
 - 閘道類型 = VPN 和 VPN 類型 = 路由式

 - 建立第二個虛擬網路閘道 (**azcross VNetGateway**) 的第二個資源群組中 (**SR AZCROSS**)
 - 閘道類型 = VPN 和 VPN 類型 = 路由式

 - 從第一個虛擬網路閘道，以第二個虛擬網路閘道中建立的 Vnet 對 Vnet 連線。 提供共用的金鑰

 - 從第一個虛擬網路閘道的第二個虛擬網路閘道中建立的 Vnet 對 Vnet 連線。 如上述步驟所提供，請提供相同的共用的金鑰。 

10. 每個叢集節點，開啟連接埠 59999 （健康情況探查）。

   每個節點上執行下列命令：

   ```powershell
      netsh advfirewall firewall add rule name=PROBEPORT dir=in protocol=tcp action=allow localport=59999 remoteip=any profile=any 
   ```

11. 指示要接聽的連接埠 59999 上的 健康情況探查訊息，並回應從目前擁有這項資源的節點叢集。

   執行一次從任何一個叢集節點，每個叢集。 
    
   在我們的範例，請務必變更"ILBIP 「 根據您的組態值。 執行下列命令，從任何一個節點**az2az1**/**az2az2**

   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.3.0.100" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

12. 執行下列命令，從任何一個節點**azcross1**/**azcross2**
   ```PowerShell
     $ClusterNetworkName = "Cluster Network 1" # Cluster network name (Use Get-ClusterNetwork on Windows Server 2012 or higher to find the name. And use Get-ClusterResource to find the IPResourceName).
     $IPResourceName = "Cluster IP Address" # IP Address cluster resource name.
     $ILBIP = "10.0.0.10" # IP Address in Internal Load Balancer (ILB) - The static IP address for the load balancer configured in the Azure portal.
     [int]$ProbePort = 59999
     Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"=$ProbePort;"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";”ProbeFailureThreshold”=5;"EnableDhcp"=0}  
   ```

   請確定這兩個叢集可以連接 / 彼此通訊。

   請使用容錯移轉叢集管理員中的 「 連線到叢集 」 功能連線到另一個叢集，或檢查另一個叢集會回應來自其中一個目前的叢集節點。

   範例中，我們已經使用：
   ```powershell
      Get-Cluster -Name SRAZC1 (ran from azcross1)
   ```
   ```powershell
      Get-Cluster -Name SRAZCross (ran from az2az1) 
   ```

13. 建立這兩個叢集的雲端見證。 建立兩個[儲存體帳戶](https://ms.portal.azure.com/#create/Microsoft.StorageAccount-ARM)(**az2azcw**，**azcrosssa**) 在 Azure 中，每個資源群組中每個叢集的其中一個 (**SR AZ2AZ**， **SR-AZCROSS**)。
   
   - 從 [存取金鑰] 中複製的儲存體帳戶名稱和金鑰
   - 建立雲端見證，從 [容錯移轉叢集管理員]，並使用上述的帳戶名稱和金鑰來建立它。 

14. 執行[叢集驗證測試](../../failover-clustering/create-failover-cluster.md#validate-the-configuration)再繼續進行下一個步驟

15. 啟動 Windows PowerShell，然後使用 [Test-SRTopology](https://docs.microsoft.com/powershell/module/storagereplica/test-srtopology?view=win10-ps) Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以在僅查看需求的模式中，使用此 Cmdlet 進行快速測試，還可以在評估長時間執行效能的模式中使用。
 
16. 設定叢集對叢集儲存體複本。
授與存取權從一個叢集至兩個方向的另一個叢集：

   從我們的範例：
   ```powershell
     Grant-SRAccess -ComputerName az2az1 -Cluster SRAZCross
   ```
如果您使用 Windows Server 2016，也執行這個命令：

   ```powershell
     Grant-SRAccess -ComputerName azcross1 -Cluster SRAZC1
   ```

17. 建立兩個叢集 SR-夥伴關係：</ol>

   - 叢集**SRAZC1**
      - 磁碟區的位置:-c:\ClusterStorage\DataDisk1
      - 記錄位置:-g:
   - 叢集**SRAZCross**
      - 磁碟區的位置:-c:\ClusterStorage\DataDiskCross
      - 記錄位置:-g:

執行命令：

```powershell
PowerShell

New-SRPartnership -SourceComputerName SRAZC1 -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\DataDisk1 -SourceLogVolumeName  g: -DestinationComputerName SRAZCross -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\DataDiskCross -DestinationLogVolumeName  g:
```