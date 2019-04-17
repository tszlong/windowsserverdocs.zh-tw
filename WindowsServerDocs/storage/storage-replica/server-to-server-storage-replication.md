---
title: "伺服器對伺服器儲存體複寫"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 10/11/2016
ms.assetid: 61881b52-ee6a-4c8e-85d3-702ab8a2bd8c
ms.openlocfilehash: 9dd2820f641e23dceb5c2ac7110efda0d3345dcf
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="server-to-server-storage-replication"></a>伺服器對伺服器儲存體複寫

> 適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用儲存體複本設定兩部伺服器來同步處理資料，讓每個都有相同磁碟區的相同複本。 本主題提供這種伺服器對伺服器複寫組態的一些背景，以及如何設定和管理環境。

若要管理儲存體複本，您可以使用 PowerShell，或 [Azure Server management tools](https://blogs.technet.microsoft.com/servermanagement/) (Azure 伺服器管理工具)。

> [!IMPORTANT]
>  在此案例中，每部伺服器都應該位於不同的實體或邏輯網站中。 每部伺服器都必須能夠透過網路，彼此進行通訊。  

## <a name="terms"></a>詞彙  
本逐步解說使用下列環境做為範例：  

-   兩部伺服器，名為 **SR SRV05** 和 **SR SRV06**。  

-   代表示兩個不同資料中心的一對邏輯「網站」，一個名為 **Redmond**，而另一個名 **Bellevue**。  

![這個圖表顯示與建築物 9 的伺服器進行複寫之建築物 5 的伺服器](media/Server-to-Server-Storage-Replication/Storage_SR_ServertoServer.png)  

**圖 1：伺服器對伺服器複寫**  

## <a name="prerequisites"></a>必要條件  

* Active Directory Domain Services 樹系 (不需要執行 Windows Server 2016)。  
* 在兩部伺服器安裝 Windows Server 2016 Datacenter Edition。  
* 使用 SAS JBOD、光纖通道 SAN、iSCSI 目標，或本機 SCSI/SATA 儲存體的兩組儲存體。 儲存體應包含 HDD 和 SSD 兩者混合的媒體。 您必須設定每組儲存體只能供各自的伺服器使用，不得共用存取。  
* 每組存放裝置必須允許建立至少兩個虛擬磁碟，一個供複寫的資料使用，另一個供記錄檔使用。 實體存放裝置的所有資料磁碟上，必須都要有相同的磁區大小。 實體存放裝置的所有記錄檔磁碟上，必須都要有相同的磁區大小。  
* 每部伺服器上至少要有一個乙太網路/TCP 連線，以進行同步複寫，但最好是 RDMA。   
* 適當的防火牆與路由器規則，以允許在所有節點之間提供 ICMP、SMB (連接埠 445，若為 SMB 直接傳輸，需再加上 5445) 以及 WS-MAN (連接埠 5985) 雙向流量。  
* 針對同步複寫，伺服器間的網路頻寬必須足以容納您的 IO 寫入工作負載，以及平均 =5ms 的來回延遲。 非同步複寫沒有建議的延遲值。  
* 複寫的儲存體不可位於包含 Windows 作業系統資料夾的磁碟機上。

這其中許多需求都可以使用 `Test-SRTopology cmdlet` 來判斷。 如果您至少在一部伺服器上安裝儲存體複本或儲存體複本管理工具功能，便可存取此工具。 只要安裝此 Cmdlet 即可，不需要將儲存體複本設定為使用此工具。 詳細資訊收錄於下列步驟。  

## <a name="provision-operating-system-features-roles-storage-and-network"></a>佈建作業系統、功能、角色、儲存體及網路  
1.  使用 Windows Server 2016 Datacenter **(桌面體驗)** 的安裝類型，在兩個伺服器節點上安裝 Windows Server 2016。 請不要選擇標準版 (如果有的話)，因為當中不包含儲存體複本。  

2.  新增網路資訊並加入網域，然後予以重新啟動。  

    > [!NOTE]
    > 從此時開始，一律要以所有伺服器上內建系統管理員群組成員的網域使用者身分登入。 請務必記住，在圖形化伺服器安裝或在 Windows 10 電腦上執行時，一開始要提升 PowerShell 和 CMD 命令提示字元的權限。  

3.  將第一組 JBOD 儲存體機箱、iSCSI 目標、FC SAN 或本機固定式磁碟 (DAS) 儲存體連線到 **Redmond** 網站中的伺服器。  

4.  將第二組儲存體連線到 **Bellevue** 網站中的伺服器。  

5.  若適當，在兩個節點上安裝最新的廠商儲存體和機箱韌體與驅動程式、最新的廠商 HBA 驅動程式、最新的廠商 BIOS/UEFI 韌體、最新的廠商網路驅動程式，以及最新的主機板晶片組驅動程式。 視需要重新啟動節點。  

    > [!NOTE]
    > 如需設定共用儲存體和網路硬體，請參閱硬體廠商的文件。  

6.  確定伺服器的 BIOS/UEFI 設定能提供高效能，例如停用 C-State、設定 QPI 速度、啟用 NUMA，以及設定最高的記憶體頻率。 務必將 Windows Server 中的電源管理設定為高效能。 視需要重新啟動。  

7.  如下所示設定角色：  

    -   **圖形化方法**  

        1.  執行 **ServerManager.exe**，並新增所有伺服器節點以建立伺服器群組。  

        2.  在每個節點上安裝 **\[檔案伺服器\]** 和 **\[儲存體複本\]** 角色和功能，然後予以重新啟動。  

    -   **Windows PowerShell 方法**  

        在 SR-SRV06 或遠端管理電腦上，於 Windows PowerShell 主控台執行下列命令，以安裝所需的功能與角色，並予以重新啟動：  

        ```  
        $Servers = 'SR-SRV05','SR-SRV06'  

        $Servers | ForEach { Install-WindowsFeature -ComputerName $_ -Name Storage-Replica,FS-FileServer -IncludeManagementTools -restart }  
        ```  

        如需這些步驟的詳細資訊，請參閱[安裝或解除安裝角色、角色服務或功能](http://technet.microsoft.co/library/hh831809.aspx)  

8.  如下所示設定儲存體：  

    > [!IMPORTANT]  
    > -   您必須在每個機箱上建立兩個磁碟區︰一個供資料使用，而另一個供記錄檔使用。  
    > -   記錄檔和資料磁碟必須初始化為 GPT，而非多位元率 (MBR)。  
    > -   這兩個資料磁碟區的大小必須相同。  
    > -   這兩個記錄檔磁碟區的大小應該相同。  
    > -   所有複寫的資料磁碟都必須有相同的磁區大小。  
    > -   所有記錄檔磁碟都必須有相同的磁區大小。  
    > -   記錄檔磁碟區應該使用快閃記憶儲存體，例如 SSD。 Microsoft 建議記錄檔儲存體應該要比資料儲存體更快。 記錄檔磁碟區不得用於其他工作負載。
    > -   資料磁碟可以使用 HDD、SSD 或階層式組合，而且可以使用鏡像或同位空間，或是 RAID 1 或 10、RAID 5 或 RAID 50。  
    > -   記錄磁碟區預設必須至少有 9 GB，而且根據記錄需求，可能會更大或更小。  
    > -   檔案伺服器角色只有在操作 Test-SRTopology 時才需要，因為它會開啟必要的防火牆連接埠進行測試。
    
    - **對於 JBOD 機箱：**  

        1.  請確定每部伺服器只能看到該網站的儲存體機箱，同時已正確設定 SAS 連線。  

        2.  遵循[在獨立伺服器上部署儲存空間](http://technet.microsoft.com/library/jj822938.aspx)中提供的**步驟 1-3**，使用 Windows PowerShell 或伺服器管理員，使用儲存空間佈建儲存體。  

    - **對於 iSCSI 儲存體：**  

        1.  請確定每個叢集只能看到該網站的儲存體機箱。 如果使用 iSCSI，您應該使用一張以上的網路介面卡。    

        2.  使用廠商的文件來佈建儲存體。 如果使用 Windows iSCSI 目標，請參閱 [iSCSI 目標區塊儲存體，作法](http://technet.microsoft.com/library/hh848268.aspx)。  

    - **對於 FC SAN 儲存體︰**  

        1.  請確定每個叢集只能看到該網站的儲存體機箱，同時您已將主機適當地分區。   

        2.  使用廠商的文件來佈建儲存體。  

    - **對於本機固定式磁碟 (DAS) 儲存體︰**  

        -   請確定儲存體不包含系統磁碟區、分頁檔或傾印檔案。  

        -   使用廠商的文件來佈建儲存體。  

9. 啟動 Windows PowerShell，然後使用 **Test-SRTopology** Cmdlet 來判斷您是否符合所有儲存體複本需求。 您可以在僅查看需求的模式中，使用此 Cmdlet 進行快速測試，還可以在評估長時間執行效能的模式中使用。  

    例如，若要驗證建議的節點，其中每一個都有 **F:** 與 **G:** 磁碟區，且要執行測試長達 30 分鐘：  

        MD c:\temp  

        Test-SRTopology -SourceComputerName SR-SRV05 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName SR-SRV06 -DestinationVolumeName f: -DestinationLogVolumeName g: -DurationInMinutes 30 -ResultPath c:\temp  

    > [!IMPORTANT]
      > 在指定的來源磁碟區上，若在評估期間使用的測試伺服器沒有任何寫入 IO 負載，請考慮新增工作負載，否則不會產生有用的報告。 您應該使用和實際執行類似的工作負載來測試，才能看出實際的數字與建議的記錄檔大小。 或者，只要在測試期間，將一些檔案複製到來源磁碟區，或下載並執行 [DISKSPD](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)，就可以產生寫入 IO。 例如，會寫入 D: 磁碟區長達十分鐘的少量 IO 工作負載範例︰  
      >
      > `Diskspd.exe -c1g -d600 -W5 -C5 -b8k -t2 -o2 -r -w5 -i100 d:\test` 

10. 檢查 **TestSrTopologyReport.html** 報告，確定您符合儲存體複本需求。  

    ![這個畫面顯示拓撲報告](media/Server-to-Server-Storage-Replication/SRTestSRTopologyReport.png)  

## <a name="configure-server-to-server-replication-using-windows-powershell"></a>使用 Windows PowerShell 設定伺服器對伺服器複寫  
現在，您將使用 Windows PowerShell 設定伺服器對伺服器複寫。 您必須直接在節點上，或從包含 Windows Server 2016 RSAT 管理工具的遠端管理電腦，執行下列所有步驟。  

使用免費的伺服器管理員工具 (SMT) 即可支援儲存體複本的圖形化管理。 請檢查這組管理工具之 Preview 可用性的 Azure 文件。 在 SMT 上市之後，將會更新本文件。

1. 請確定您以系統管理員身分使用提升權限的 Powershell 主控台。  
2. 設定伺服器對伺服器複寫時，要指定來源和目的地磁碟、來源和目的地記錄檔、來源和目的地節點，以及記錄檔大小。  

    ```PowerShell  
    New-SRPartnership -SourceComputerName sr-srv05 -SourceRGName rg01 -SourceVolumeName f: -SourceLogVolumeName g: -DestinationComputerName sr-srv06 -DestinationRGName rg02 -DestinationVolumeName f: -DestinationLogVolumeName g:  
    ```  

   輸出：
   ```PowerShell
   DestinationComputerName : SR-SRV06
   DestinationRGName       : rg02
   SourceComputerName      : SR-SRV05
   PSComputerName          :
   ```

    > [!IMPORTANT]
    > 預設記錄檔大小為 8 GB。 根據 `Test-SRTopology` Cmdlet 的結果，您可能會決定以較高或較低的值來使用 -LogSizeInBytes。  

2.  若要取得複寫來源和目的地狀態，請使用 `Get-SRGroup` 和 `Get-SRPartnership`，如下所示：  

    ```PowerShell  
    Get-SRGroup  
    Get-SRPartnership  
    (Get-SRGroup).replicas  
    ```
    輸出：

    ```PowerShell
    CurrentLsn             : 0
    DataVolume             : F:\
    LastInSyncTime         :
    LastKnownPrimaryLsn    : 1
    LastOutOfSyncTime      :
    NumOfBytesRecovered    : 37731958784
    NumOfBytesRemaining    : 30851203072
    PartitionId            : c3999f10-dbc9-4a8e-8f9c-dd2ee6ef3e9f
    PartitionSize          : 68583161856
    ReplicationMode        : synchronous
    ReplicationStatus      : InitialBlockCopy
    PSComputerName         :
    ```

3.  判斷複寫進度，如下所示︰  

    1.  在來源伺服器上，執行下列命令，並檢查 5015、5002、5004、1237、5001 及 2200 事件︰  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica -max 20  
        ```  

    2.  在目的地伺服器上，執行下列命令來查看可顯示建立合作關係的儲存體複本事件。 此事件會說明已複製的位元組數目和所花費的時間。 範例：  

        ```PowerShell  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | Where-Object {$_.ID -eq "1215"} | fl  

        TimeCreated  : 4/8/2016 4:12:37 PM  
        ProviderName : Microsoft-Windows-StorageReplica  
        Id           : 1215  
        Message      : Block copy completed for replica.  

                ReplicationGroupName: rg02  
                ReplicationGroupId: {616F1E00-5A68-4447-830F-B0B0EFBD359C}  
                ReplicaName: f:\  
                ReplicaId: {00000000-0000-0000-0000-000000000000}  
                End LSN in bitmap:   
                LogGeneration: {00000000-0000-0000-0000-000000000000}  
                LogFileId: 0  
                CLSFLsn: 0xFFFFFFFF  
                Number of Bytes Recovered: 68583161856  
                Elapsed Time (ms): 117  
        ```  

        > [!NOTE]
        > 儲存體複本會將目的地磁碟區與其磁碟機代號或掛接點卸載。 這是原本設計的做法。  

    3.  或者，複本的目的地伺服器群組會隨時說明待複製的位元組數目，並可透過 PowerShell 進行查詢。 例如：  

        ```  
        (Get-SRGroup).Replicas | Select-Object numofbytesremaining  
        ```  

        和進度範例 (將不會終止) 一樣：  

        ```  
        while($true) {  

         $v = (Get-SRGroup -Name "RG02").replicas | Select-Object numofbytesremaining  
         [System.Console]::Write("Number of bytes remaining: {0}`r", $v.numofbytesremaining)  
         Start-Sleep -s 5  
        }  
        ```  

    4.  在目的地伺服器上，執行下列命令，並檢查 5009、1237、5001、5015、5005 及 2200 事件，以了解處理進度。 此序列中應該不會有任何錯誤警告。 其中將會有許多指出進度的 1237 事件。  

        ```  
        Get-WinEvent -ProviderName Microsoft-Windows-StorageReplica | FL  
        ```  

## <a name="manage-replication"></a>管理複寫  
現在您將會管理與操作伺服器對伺服器複寫的基礎結構。 您可以直接在節點上，或從包含 Windows Server 2016 RSAT 管理工具的遠端管理電腦，執行下列所有步驟。  

1.  使用 `Get-SRPartnership` 和 `Get-SRGroup` 來判斷目前的複寫來源與目的地及其狀態。  

2.  若要測量複寫效能，請在來源和目的地節點上使用 `Get-Counter` Cmdlet。 計數器名稱如下：  

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

    如需 Windows PowerShell 中效能計數器的詳細資訊，請參閱 [Get-Counter](http://technet.microsoft.com/library/hh849685.aspx)。  

3.  若要移動一個網站的複寫方向，請使用 `Set-SRPartnership` Cmdlet。  

    ```  
    Set-SRPartnership -NewSourceComputerName sr-srv06 -SourceRGName rg02 -DestinationComputerName sr-srv05 -DestinationRGName rg01  
    ```  

    > [!WARNING]  
    > 正在進行初始同步處理時，Windows Server 2016 不會阻止角色切換，因為如果您尚未完成初始複寫，即嘗試切換，可能會導致資料遺失。 在完成初始同步處理之前，請勿強制切換方向。  

    檢查事件記錄檔，以查看複寫方向變更以及復原模式發生的情況，接著予以調解。 然後寫入 IO 就可以寫入新的來源伺服器所擁有的儲存體。 變更複寫方向，將會在先前的來源電腦上封鎖寫入 IO。  

4.  若要移除複寫，在任何節點上使用 `Get-SRGroup`、`Get-SRPartnership`、`Remove-SRGroup` 及 `Remove-SRPartnership`。 請確定您僅在目前的複寫來源上執行 `Remove-SRPartnership` Cmdlet，而不是在目的地伺服器上執行。 在兩部伺服器上執行 `Remove-Group`。 例如，若要從兩部伺服器移除所有複寫︰  

    ```  
    Get-SRPartnership  
    Get-SRPartnership | Remove-SRPartnership  
    Get-SRGroup | Remove-SRGroup  
    ```  

## <a name="replacing-dfs-replication-with-storage-replica"></a>以儲存體複本取代 DFS 複寫  
許多 Microsoft 客戶會將 DFS 複寫部署為非結構化使用者資料 (例如主資料夾和部門共用) 的嚴重損壞修復解決方案。 DFS 複寫已隨附於 Windows Server 2003 R2 和所有更新版本的作業系統中，並在低頻寬網路上運作，這會吸引具有許多節點的高延遲及低變化環境。 不過，DFS 複寫如同資料複寫解決方案，有一些值得注意的限制︰  
* 它不會複寫使用中或開啟的檔案。  
* 它不會以同步方式複寫。  
* 其非同步複寫延遲可能是好幾分鐘，好幾小時，甚至好幾天。  
* 它所依賴的資料庫在電源中斷後可能需要冗長的一致性檢查。  
* 它通常會設定為多重主機，以允許對雙向流動的變更，進而可能覆寫較新的資料。  

儲存體複本沒有這些限制。 不過，它所擁有的一些限制可能會使其在部分環境中變得比較不吸引人︰  

* 它只允許磁碟區之間的一對一複寫。 它可能會複寫多部伺服器之間的不同磁碟區。  
* 雖然它支援非同步複寫，但是它不是針對低頻寬、高延遲網路而設計。  
* 它不允許使用者在複寫正在進行時，存取目的地上受保護的資料  

如果這些不會成為裹足不前的因素，儲存體複本可讓您使用這個較新的技術取代 DFS 複寫伺服器。   
高階程序為︰  

1.  在兩部伺服器上安裝 Windows Server 2016，並設定您的儲存體。 這可能表示升級一組現有的伺服器或全新安裝。  
2.  請確定您想要複寫的任何資料存在於一個或多個資料磁碟區上，而不是在 C: 磁碟機上。   
a.  您也可以使用備份或檔案複本，在另一部伺服器上植入資料以節省時間，並使用精簡佈建的儲存體。 與 DFS 複寫不同的是，不需要使類似中繼資料的安全性完全相符。  
3.  共用來源伺服器上的資料，並讓它能夠透過 DFS 命名空間存取。 為確保伺服器名稱變更為嚴重損壞網站中的伺服器名稱時仍然可以存取該伺服器，此作業相當重要。  
a.  您可以在正常作業期間將無法使用的目的地伺服器上建立相符的共用。   
b.  請勿在「DFS 命名空間」命名空間中，新增目的地伺服器，否則，請確定已停用其所有資料夾目標。  
4.  啟用儲存體複本的複寫，並完成初始同步。複寫可以使用同步或非同步方式進行。   
a.  不過，建議使用同步方式，才能保證目的地伺服器上的 IO 資料一致性。   
b.  我們強烈建議啟用 \[磁碟區陰影複製\]，並使用 VSSADMIN 或您選擇的其他工具，定期製作快照。 這樣會保證應用程式將其資料檔案一致清除至磁碟。 萬一發生嚴重損壞，您可以在可能已使用非同步方式部分複寫的目的地伺服器上，復原快照中的檔案。 快照集複寫及檔案。  
5.  正常運作，直到發生嚴重損壞為止。  
6.  切換目的地伺服器成為新的來源，向使用者呈現其複寫的磁碟區。  
7.  如果使用同步複寫，除非使用者所使用的應用程式在失去來源伺服器期間寫入沒有交易保護的資料 (這與複寫無關)，否則將不需要還原任何資料。 如果使用非同步複寫，比較需要使用 VSS 快照掛接，但請考慮在所有情況下都使用 VSS，讓應用程式取得一致的快照。  
8.  新增伺服器及其共用做為 DFS 命名空間資料夾目標。   
9.  接著，使用者可以存取其資料。  

 > [!NOTE]
 > 嚴重損壞修復規劃是一個複雜的主題，需要非常注意細節。 強烈建議您建立年度即時容錯移轉步驟 Runbook 和效能。 當實際的災害來襲時，混亂將會主導一切，而且可能無法連絡到有經驗的人員。  

## <a name="related-topics"></a>\[相關主題\]  
- [儲存體複本概觀](storage-replica-overview.md)  
- [使用共用存放裝置的延展式叢集複寫](stretch-cluster-replication-using-shared-storage.md)  
- [叢集對叢集儲存體複寫](cluster-to-cluster-storage-replication.md)
- [儲存體複本：已知問題](storage-replica-known-issues.md)  
- [儲存體複本：常見問題集](storage-replica-frequently-asked-questions.md)  

## <a name="see-also"></a>另請參閱  
- [Windows Server 2016](../../get-started/windows-server-2016.md)  
- [Windows Server 2016 中的儲存空間直接存取](../storage-spaces/storage-spaces-direct-overview.md)  
