---
title: 叢集對叢集儲存體複寫
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
ms.assetid: 834e8542-a67a-4ba0-9841-8a57727ef876
author: nedpyle
ms.date: 04/26/2019
description: 如何使用儲存體複本將一個叢集中的磁片區複寫到另一個執行 Windows Server 的叢集。
ms.openlocfilehash: d99a7ebf933427e8e065f72261816610e62a433d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961240"
---
# <a name="cluster-to-cluster-storage-replication"></a>叢集對叢集儲存體複寫

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server (半年通道)

儲存體複本可以在叢集之間複寫磁片區，包括使用儲存空間直接存取的叢集複寫。 管理與設定類似於伺服器對伺服器複寫。

您將在叢集對叢集設定中，設定這些電腦與存放裝置，其中一個叢集會將自己那組存放裝置複寫為另一叢集與它那組存放裝置。 雖然並非必要，但這些節點及其存放裝置應位在不同的實體網站。

> [!IMPORTANT]
> 這項測試中的四部伺服器即為範例。 您可以在每個叢集中使用 Microsoft 所支援的任意數目伺服器，此叢集目前適用于儲存空間直接存取叢集，而64則適用于共用儲存體叢集。
>
> 本指南並未涵蓋設定儲存空間直接存取的內容。 如需設定儲存空間直接存取的詳細資訊，請參閱[儲存空間直接存取總覽](../storage-spaces/storage-spaces-direct-overview.md)。

本逐步解說使用下列環境做為範例︰

-   兩個成員伺服器，分別名為 **SR-SRV01** 和 **SR-SRV02**，稍後形成名為 **SR-SRVCLUSA** 的叢集。

-   兩個成員伺服器，分別名為 **SR-SRV03** 和 **SR-SRV04**，稍後形成名為 **SR-SRVCLUSB** 的叢集。

-   代表示兩個不同資料中心的一對邏輯「網站」，一個名為 **Redmond**，而另一個名 **Bellevue**。

![此圖顯示一個範例環境，其 Redmond 網站中的叢集會以 Bellevue 網站中的叢集複寫](./media/Cluster-to-Cluster-Storage-Replication/SR_ClustertoCluster.png)

**圖 1：叢集對叢集複寫**

## <a name="prerequisites"></a>必要條件

* Active Directory 網域服務樹系 (不需要執行 Windows Server 2016)。
* 4-128 部執行 Windows Server 2019 或 Windows Server 2016 Datacenter Edition 的伺服器（兩部2-64 伺服器的叢集）。 如果您執行的是 Windows Server 2019，如果您確定只複寫最多 2 TB 大小的單一磁片區，就可以改用 Standard Edition。
* 兩組存放裝置，使用 SAS JBOD、光纖通道 SAN、共用 VHDX、儲存空間直接存取或 iSCSI 目標。 存放裝置應包含 HDD 和 SSD 兩者混合的媒體。 您必須設定每組存放裝置只能供各自的叢集使用，叢集之間不得共用存取。
* 每組存放裝置必須允許建立至少兩個虛擬磁碟，一個供複寫的資料使用，另一個供記錄檔使用。 實體存放裝置的所有資料磁碟上，必須都要有相同的磁區大小。 實體存放裝置的所有記錄檔磁碟上，必須都要有相同的磁區大小。
* 每部伺服器上至少要有一個乙太網路/TCP 連線，以進行同步複寫，但最好是 RDMA。
* 適當的防火牆與路由器規則，以允許在所有節點之間提供 ICMP、SMB (連接埠 445，若為 SMB 直接傳輸，需再加上 5445) 以及 WS-MAN (連接埠 5985) 雙向流量。
* 針對同步複寫，伺服器間的網路頻寬必須足以容納您的 IO 寫入工作負載，以及平均 =5ms 的來回延遲。 非同步複寫沒有建議的延遲值。
* 複寫的儲存體不可位於包含 Windows 作業系統資料夾的磁碟機上。
* 儲存空間直接存取複寫 & 限制有重要的考慮-請參閱下面的詳細資訊。

這其中許多需求都可以使用 `Test-SRTopology` Cmdlet 來判斷。 如果您至少在一部伺服器上安裝儲存體複本或儲存體複本管理工具功能，便可存取此工具。 只要安裝此 Cmdlet 即可，不需要將儲存體複本設定為使用此工具。 詳細資訊收錄於下列步驟。

## <a name="step-1-provision-operating-system-features-roles-storage-and-network"></a>步驟 1：佈建作業系統、功能、角色、儲存體及網路

1.  在安裝類型為 Windows Server **（桌面體驗）** 的所有四個伺服器節點上安裝 windows server。

2.  新增網路資訊並加入網域，然後予以重新啟動。

    > [!IMPORTANT]
    > 從此時開始，一律要以所有伺服器上內建系統管理員群組成員的網域使用者身分登入。 請務必記住，在圖形化伺服器安裝或 Windows 10 電腦上執行時，一開始要提升 Windows PowerShell 和 CMD 命令提示字元的權限。

3.  將第一組 JBOD 存放裝置機箱、iSCSI 目標、FC SAN 或本機固定式磁碟 (DAS) 存放裝置連線到 **Redmond** 網站中的伺服器。

4.  將第二組存放裝置連線到 **Bellevue** 網站中的伺服器。

5.  視需要在四個節點上全部安裝最新的廠商存放裝置和機箱韌體與驅動程式、最新的廠商 HBA 驅動程式、最新的廠商 BIOS/UEFI 韌體、最新的廠商網路驅動程式，以及最新的主機板晶片組驅動程式。 視需要重新啟動節點。

    > [!NOTE]
    > 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。

6.  確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 務必將 Windows Server 中的電源管理設定為高效能。 視需要重新啟動。

7.  如下所示設定角色：

    -   **圖形化方法**

        1.  執行 **ServerManager.exe**，並新增所有伺服器節點以建立伺服器群組。

        2.  在每個節點上安裝 [檔案伺服器]**** 和 [儲存體複本]**** 角色和功能，然後予以重新啟動。

    -   **Windows PowerShell 方法**

        在 SR-SRV04 或遠端管理電腦上，於 Windows PowerShell 主控台執行下列命令，為延展式叢集在四個節點上安裝所需的功能與角色，並予以重新啟動︰

        ```PowerShell
        $Servers = 'SR-SRV01','SR-SRV02','SR-SRV03','SR-SRV04'

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,Failover-Clustering,FS-FileServer -IncludeManagementTools -restart }
        ```

        如需這些步驟的詳細資訊，請參閱[安裝或卸載角色、角色服務或功能](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)

9. 如下所示設定儲存體：

    > [!IMPORTANT]
    > -   您必須在每個機箱上建立兩個磁碟區︰一個供資料使用，而另一個供記錄檔使用。
    > -   記錄檔和資料磁碟必須初始化為 **GPT**，而非 **MBR**。
    > -   這兩個資料磁碟區的大小必須相同。
    > -   這兩個記錄檔磁碟區的大小應該相同。
    > -   所有複寫的資料磁碟都必須有相同的磁區大小。
    > -   所有記錄檔磁碟都必須有相同的磁區大小。
    > -   記錄檔磁碟區應該使用 Flash 架構的儲存體，例如 SSD。  Microsoft 建議記錄檔儲存體應該要比資料儲存體更快。 記錄檔磁碟區不得用於其他工作負載。
    > -   資料磁碟可以使用 HDD、SSD 或階層式組合，而且可以使用鏡像或同位空間，或是 RAID 1 或 10、RAID 5 或 RAID 50。
    > -   記錄磁片區預設必須至少為8GB，而且可能會根據記錄需求而更大或更小。
    > -   搭配 NVME 或 SSD 快取使用儲存空間直接存取（儲存空間直接存取）時，您會在設定儲存空間直接存取叢集之間的儲存體複本複寫時看到大於預期的延遲增加。 當您在效能 + 容量設定中使用 NVME 和 SSD，而且沒有 HDD 層或容量層級時，延遲變更的比例會高於您所看到的內容。

    此問題發生的原因是，與較慢的媒體相比，SR 記錄機制內的架構限制與 NVME 的極低延遲結合。 使用儲存空間直接存取儲存空間直接存取快取時，SR 記錄的所有 IO 以及應用程式所有最近的讀取/寫入 IO，都會出現在快取中，而且不會發生在效能或容量層級上。 這表示所有 SR 活動都是在相同的速度媒體上執行-不建議使用此設定（ https://aka.ms/srfaq 如需記錄建議，請參閱）。

    搭配 Hdd 使用儲存空間直接存取時，您無法停用或避免快取。 因應措施是，如果只使用 SSD 和 NVME，您可以只設定效能和容量層。 如果使用該設定，而且只將 SR 記錄放在效能層，而只將其服務在容量層上的資料磁片區，您就可以避免上述的高延遲問題。 這種做法可以混合使用更快速且更慢的 Ssd，而且沒有 NVME。

    這種因應措施並非理想，而且有些客戶可能無法使用它。 SR 小組正致力於優化和更新的記錄機制，以供未來減少這些發生的人工瓶頸。 沒有適用于這種情況的 ETA，但當您有提供測試的客戶時，將會更新此常見問題。

-   **對於 JBOD 機箱：**

1. 請確定每個叢集只能看到該網站的存放裝置機箱，同時已正確設定 SAS 連線。

2. 遵循[在獨立伺服器上部署儲存空間](../storage-spaces/deploy-standalone-storage-spaces.md)中提供的**步驟 1-3**，使用 Windows PowerShell 或伺服器管理員，使用儲存空間佈建儲存體。

-   **對於 iSCSI 目標存放裝置：**

1. 請確定每個叢集只能看到該網站的存放裝置機箱。 如果使用 iSCSI，您應該使用一張以上的網路介面卡。

2. 使用廠商的文件來佈建儲存體。 如果使用 Windows iSCSI 目標，請參閱 [iSCSI 目標區塊儲存體，作法](../iscsi/iscsi-target-server.md)。

-   **對於 FC SAN 存放裝置：**

1. 請確定每個叢集只能看到該網站的儲存體機箱，同時您已將主機適當地分區。

2. 使用廠商的文件來佈建儲存體。

-   **對於儲存空間直接存取：**

1. 確定每個叢集只能透過部署儲存空間直接存取看到該網站的存放裝置機箱。 (https://docs.microsoft.com/windows-server/storage/storage-spaces/hyper-converged-solution-using-storage-spaces-direct)

2. 確定 SR 記錄檔磁碟區一定位在最快速的快閃存放裝置，而資料磁碟區位在較慢的高容量存放裝置上。

3. 啟動 Windows PowerShell，然後使用 `Test-SRTopology` Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以在僅查看需求的模式中，使用此 Cmdlet 進行快速測試，還可以在評估長時間執行效能的模式中使用。
   例如

   ```PowerShell
   MD c:\temp

   Test-SRTopology -SourceComputerName SR-SRV01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV03 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp
   ```

     > [!IMPORTANT]
     > 在指定的來源磁碟區上，若在評估期間使用的測試伺服器沒有任何寫入 IO 負載，請考慮新增工作負載，否則不會產生有用的報告。 您應該使用和實際執行類似的工作負載來測試，才能看出實際的數字與建議的記錄檔大小。 或者，只要在測試期間將一些檔案複製到來源磁片區，或下載並執行[DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)以產生寫入 io 即可。 例如，在 D：磁片區的五分鐘內，寫入 IO 工作負載較低的範例：`Diskspd.exe -c1g -d300 -W5 -C5 -b8k -t2 -o2 -r -w5 -h d:\test.dat`

4. 檢查 **TestSrTopologyReport.html** 報告，確定您符合儲存體複本需求。

   ![此圖顯示複寫拓撲報告結果](./media/Cluster-to-Cluster-Storage-Replication/SRTestSRTopologyReport.png)

## <a name="step-2-configure-two-scale-out-file-server-failover-clusters"></a>步驟 2：設定兩個向外延展檔案伺服器容錯移轉叢集
您現在將建立兩個標準的容錯移轉叢集。 完成設定、驗證及測試之後，您將會使用儲存體複本加以複寫。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

### <a name="graphical-method"></a>圖形化方法

1.  針對每個網站中的節點執行 **cluadmin.msc**。

2.  驗證建議的叢集並分析結果，以確保您能繼續進行。 以下使用的範例為 **SR-SRVCLUSA** 與 **SR-SRVCLUSB**。

3.  建立兩個叢集。 確定叢集名稱為 15 個字元以下。

4.  設定檔案共用見證或雲端見證。

    > [!NOTE]
    > WIndows Server 現在包含以雲端（Azure）為基礎的見證選項。 您可以選擇此仲裁選項，而不是檔案共用見證。

    > [!WARNING]
    > 如需仲裁設定的詳細資訊，請參閱[設定和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)中的**見證**設定一節。 如需 `Set-ClusterQuorum` Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

5.  在 **Redmond** 網站中將一部磁碟新增至叢集 CSV。 若要這樣做，在 [存放裝置]**** 區段的 [磁碟]**** 節點中，使用滑鼠右鍵按一下來源磁碟，然後按一下 [新增至叢集共用磁碟區]****。

6.  使用[設定向外延展檔案伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831718(v=ws.11))中的指示，在兩個叢集上建立叢集向外延展檔案伺服器

### <a name="windows-powershell-method"></a>Windows PowerShell 方法

1.  測試建議的叢集並分析結果，以確保您能繼續進行：

    ```PowerShell
    Test-Cluster SR-SRV01,SR-SRV02
    Test-Cluster SR-SRV03,SR-SRV04
    ```

2.  建立叢集 (您必須指定自己的叢集靜態 IP 位址)。 確定每個叢集名稱均為 15 個字元以下︰

    ```PowerShell
    New-Cluster -Name SR-SRVCLUSA -Node SR-SRV01,SR-SRV02 -StaticAddress <your IP here>
    New-Cluster -Name SR-SRVCLUSB -Node SR-SRV03,SR-SRV04 -StaticAddress <your IP here>
    ```

3.  在指向共用 (裝載於網域控制站或一些其他獨立伺服器上) 的每個叢集中，設定檔案共用見證或雲端 (Azure) 見證。 例如：

    ```PowerShell
    Set-ClusterQuorum -FileShareWitness \\someserver\someshare
    ```

    > [!NOTE]
    > WIndows Server 現在包含以雲端（Azure）為基礎的見證選項。 您可以選擇此仲裁選項，而不是檔案共用見證。

    > [!WARNING]
    > 如需仲裁設定的詳細資訊，請參閱[設定和管理仲裁](../../failover-clustering/manage-cluster-quorum.md)中的**見證**設定一節。 如需 `Set-ClusterQuorum` Cmdlet 的詳細資訊，請參閱 [Set-ClusterQuorum](/powershell/module/failoverclusters/set-clusterquorum)。

4.  使用[設定向外延展檔案伺服器](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831718(v=ws.11))中的指示，在兩個叢集上建立叢集向外延展檔案伺服器

## <a name="step-3-set-up-cluster-to-cluster-replication-using-windows-powershell"></a>步驟 3：使用 Windows PowerShell 設定叢集對叢集複寫
現在您將使用 Windows PowerShell 來設定叢集對叢集複寫。 您可以直接在節點上，或從包含 Windows Server 的遠端系統管理電腦執行下列所有步驟遠端伺服器管理工具

1. 藉由在第一個叢集中的任何節點上或從遠端執行**SRAccess**指令程式，授與第一個叢集對另一個叢集的完整存取權。  Windows Server 遠端伺服器管理工具

   ```PowerShell
   Grant-SRAccess -ComputerName SR-SRV01 -Cluster SR-SRVCLUSB
   ```

2. 在第二個叢集的任何節點上或從遠端執行**SRAccess**指令程式，以授與第二個叢集對另一個叢集的完整存取權。

   ```PowerShell
   Grant-SRAccess -ComputerName SR-SRV03 -Cluster SR-SRVCLUSA
   ```

3. 設定叢集對叢集複寫時，要指定來源和目的地磁碟、來源和目的地的記錄檔、來源和目的地叢集名稱，以及記錄檔大小。 您可以在本機伺服器上，或使用遠端管理電腦，執行此命令。

   ```powershell
   New-SRPartnership -SourceComputerName SR-SRVCLUSA -SourceRGName rg01 -SourceVolumeName c:\ClusterStorage\Volume2 -SourceLogVolumeName f: -DestinationComputerName SR-SRVCLUSB -DestinationRGName rg02 -DestinationVolumeName c:\ClusterStorage\Volume2 -DestinationLogVolumeName f:
   ```

   > [!WARNING]
   > 預設記錄檔大小為 8 GB。 根據**test-srtopology** Cmdlet 的結果，您可能會決定以較高或較低的值來使用 **-LogSizeInBytes** 。

4. 若要取得複寫來源和目的地狀態，請使用 **Get-SRGroup** 與 **Get-SRPartnership**，如下所示︰

   ```powershell
   Get-SRGroup
   Get-SRPartnership
   (Get-SRGroup).replicas
   ```

5. 判斷複寫進度，如下所示︰

   1.  在來源伺服器上，執行下列命令，並檢查 5015、5002、5004、1237、5001 及 2200 事件︰

       ```PowerShell
       Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20
       ```
   2.  在目的地伺服器上，執行下列命令來查看可顯示建立合作關係的儲存體複本事件。 此事件會說明已複製的位元組數目和所花費的時間。 範例：

       ```powershell
       Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | Format-List
       ```
       輸出的範例如下：

       ```
       TimeCreated  : 4/8/2016 4:12:37 PM
       ProviderName : Microsoft-Windows-StorageReplica
       Id           : 1215
       Message      : Block copy completed for replica.
           ReplicationGroupName: rg02
           ReplicationGroupId:
           {616F1E00-5A68-4447-830F-B0B0EFBD359C}
           ReplicaName: f:\
           ReplicaId: {00000000-0000-0000-0000-000000000000}
           End LSN in bitmap:
           LogGeneration: {00000000-0000-0000-0000-000000000000}
           LogFileId: 0
           CLSFLsn: 0xFFFFFFFF
           Number of Bytes Recovered: 68583161856
           Elapsed Time (seconds): 117
       ```
   3. 或者，複本的目的地伺服器群組會隨時說明待複製的位元組數目，並可透過 PowerShell 進行查詢。 例如：

      ```PowerShell
      (Get-SRGroup).Replicas | Select-Object numofbytesremaining
      ```

      和進度範例 (將不會終止) 一樣：

      ```PowerShell
        while($true) {
        $v = (Get-SRGroup -Name "Replication 2").replicas | Select-Object numofbytesremaining
        [System.Console]::Write("Number of bytes remaining: {0}`n", $v.numofbytesremaining)
        Start-Sleep -s 5
       }
       ```

6. 在目的地叢集中的目的地伺服器上，執行下列命令，並檢查 5009、1237、5001、5015、5005 及 2200 事件，即可了解處理進度。 此序列中應該不會有任何錯誤警告。 其中將會有許多指出進度的 1237 事件。

   ```PowerShell
   Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL
   ```
   > [!NOTE]
   > 複寫時，目的地叢集磁碟永遠會顯示為 [線上 (沒有存取權)]****。

## <a name="step-4-manage-replication"></a>步驟 4：管理複寫

現在您即將管理和操作叢集對叢集複寫。 您可以直接在叢集節點上，或從包含 Windows Server 遠端伺服器管理工具的遠端系統管理電腦，執行下列所有步驟。

1.  使用 **Get-ClusterGroup** 或 **Failover Cluster Manager**，判斷目前的複寫來源與目的地，還有其狀態。  Windows Server 遠端伺服器管理工具

2.  若要測量複寫效能，請在來源和目的地節點上使用 **Get-Counter** Cmdlet。 計數器名稱如下︰

    -   \Storage Replica Partition I/O Statistics(*)\Number of times flush paused

    -   \Storage Replica Partition I/O Statistics(*)\Number of pending flush I/O

    -   \Storage Replica Partition I/O Statistics(*)\Number of requests for last log write

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Flush Queue Length

    -   \Storage Replica Partition I/O Statistics(*)\Current Flush Queue Length

    -   \Storage Replica Partition I/O Statistics(*)\Number of Application Write Requests

    -   \Storage Replica Partition I/O Statistics(*)\Avg. Number of requests per log write

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Write Latency

    -   \Storage Replica Partition I/O Statistics(*)\Avg. App Read Latency

    -   \Storage Replica Statistics(*)\Target RPO

    -   \Storage Replica Statistics(*)\Current RPO

    -   \Storage Replica Statistics(*)\Avg. Log Queue Length

    -   \Storage Replica Statistics(*)\Current Log Queue Length

    -   \Storage Replica Statistics(*)\Total Bytes Received

    -   \Storage Replica Statistics(*)\Total Bytes Sent

    -   \Storage Replica Statistics(*)\Avg. Network Send Latency

    -   \Storage Replica Statistics(*)\Replication State

    -   \Storage Replica Statistics(*)\Avg. Message Round Trip Latency

    -   \Storage Replica Statistics(*)\Last Recovery Elapsed Time

    -   \Storage Replica Statistics(*)\Number of Flushed Recovery Transactions

    -   \Storage Replica Statistics(*)\Number of Recovery Transactions

    -   \Storage Replica Statistics(*)\Number of Flushed Replication Transactions

    -   \Storage Replica Statistics(*)\Number of Replication Transactions

    -   \Storage Replica Statistics(*)\Max Log Sequence Number

    -   \Storage Replica Statistics(*)\Number of Messages Received

    -   \Storage Replica Statistics(*)\Number of Messages Sent

    如需 Windows PowerShell 中效能計數器的詳細資訊，請參閱 [Get-Counter](/powershell/module/microsoft.powershell.diagnostics/get-counter)。

3.  若要移動一個網站的複寫方向，請使用 **Set-SRPartnership** Cmdlet。

    ```PowerShell
    Set-SRPartnership -NewSourceComputerName SR-SRVCLUSB -SourceRGName rg02 -DestinationComputerName SR-SRVCLUSA -DestinationRGName rg01
    ```

    > [!NOTE]
    > 當初始同步處理正在進行時，Windows Server 會防止角色切換，因為如果您嘗試在允許初始複寫完成之前切換，可能會導致資料遺失。 在完成初始同步處理之前，請勿強制切換方向。

    檢查事件記錄檔，以查看複寫方向變更以及復原模式發生的情況，接著予以調解。 然後寫入 IO 就可以寫入新的來源伺服器所擁有的儲存體。 變更複寫方向，將會在先前的來源電腦上封鎖寫入 IO。

    > [!NOTE]
    > 複寫時，目的地叢集磁碟永遠會顯示為 [線上 (沒有存取權)]****。

4.  若要變更預設 8 GB 的記錄檔大小，請在來源和目的地儲存體複本群組上使用**set-srgroup** 。

    > [!IMPORTANT]
    > 預設記錄檔大小為 8 GB。 根據 **Test-SRTopology** Cmdlet 的結果，您可能會決定以較高或較低的值來使用 -LogSizeInBytes。

5.  若要移除複寫，請在每個叢集上使用 **Get-SRGroup**、**Get-SRPartnership**、**Remove-SRGroup** 及 **Remove-SRPartnership**。

    ```powershell
    Get-SRPartnership | Remove-SRPartnership
    Get-SRGroup | Remove-SRGroup
    ```

    > [!NOTE]
    > 儲存空間複本會卸載目的地磁碟區。 這是原廠設定。

## <a name="additional-references"></a>其他參考

-   [儲存體複本總覽](storage-replica-overview.md)
-   [使用共用存放裝置的延展叢集複寫](stretch-cluster-replication-using-shared-storage.md)
-   [伺服器對伺服器儲存體複寫](server-to-server-storage-replication.md)
-   [儲存體複本：已知問題](storage-replica-known-issues.md)
-   [儲存體複本：常見問題集](storage-replica-frequently-asked-questions.md)
-   [Windows Server 2016 中的儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)
