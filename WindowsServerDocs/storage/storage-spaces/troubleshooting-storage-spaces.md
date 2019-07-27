---
title: 儲存空間直接存取疑難排解
description: 瞭解如何針對您的儲存空間直接存取部署進行疑難排解。
keywords: 儲存空間
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7cc5709723b300f46ce108b36501e7ace272cd45
ms.sourcegitcommit: 6f968368c12b9dd699c197afb3a3d13c2211f85b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/26/2019
ms.locfileid: "68544569"
---
# <a name="troubleshoot-storage-spaces-direct"></a>疑難排解儲存空間直接存取

> 適用於：Windows Server 2019、Windows Server 2016

使用下列資訊對您的儲存空間直接存取部署進行疑難排解。

一般來說, 請從下列步驟開始:

1. 確認已使用 Windows Server Catalog 為 Windows Server 2016 和 Windows Server 2019 認證 SSD 的製作/模型。 向廠商確認支援磁片磁碟機以進行儲存空間直接存取。
2. 檢查是否有任何故障的磁片磁碟機的存放裝置。 使用存放裝置管理軟體來檢查磁片磁碟機的狀態。 如果有任何磁片磁碟機發生問題, 請與您的廠商合作。 
3. 視需要更新儲存體並驅動固件。
   請確定所有節點上都已安裝最新的 Windows 更新。 您可以從 windows [10 和 Windows server 2016 更新歷程記錄](https://aka.ms/update2016)取得 windows server 2016 的最新更新, 以及 windows [10 和 windows server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619)的 windows server 2019 更新。
4. 更新網路介面卡驅動程式和固件。
5. 執行叢集驗證並檢查儲存空間直接存取區段, 確保將用於快取的磁片磁碟機正確地回報, 而且沒有錯誤。

如果您仍然遇到問題, 請參閱下列案例。

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>虛擬磁片資源處於無冗余狀態
儲存空間直接存取系統的節點意外重新開機, 因為發生損毀或電源中斷。 然後, 一或多個虛擬磁片可能不會上線, 而您會看到「沒有足夠的冗余資訊」描述。

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|Size| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| [確定]|  良好| True|  10 TB|  節點-01. conto 。|
|Disk3         |Mirror                 |[確定]                          |良好       |True            |10 TB | 節點-01. conto 。|
|Disk2         |Mirror                 |無冗余               |Unhealthy     |True            |10 TB | 節點-01. conto 。|
|Disk1         |Mirror                 |{沒有冗余, InService}  |Unhealthy     |True            |10 TB | 節點-01. conto 。| 

此外, 在嘗試讓虛擬磁片上線之後, 下列資訊會記錄在叢集記錄檔 (DiskRecoveryAction) 中。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

如果磁片故障, 或系統無法存取虛擬磁片上的資料, 則不會發生**任何冗余操作狀態**。 如果節點上的維護期間發生重新開機, 就會發生此問題。

若要修正此問題, 請遵循下列步驟:

1. 從 CSV 移除受影響的虛擬磁片。 這會將它們放入叢集中的 [可用的儲存體] 群組, 並開始顯示為「實體磁片」的 ResourceType。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在擁有可用儲存群組的節點上, 于處於 [無冗余] 狀態的每個磁片上執行下列命令。 若要識別「可用的存放裝置」群組所在的節點, 您可以執行下列命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 設定磁片修復動作, 然後啟動磁片。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 修復應該會自動啟動。 等待修復完成。 它可能會進入暫停狀態並重新啟動。 若要監視進度: 
    - 執行**get-storagejob**來監視修復的狀態, 並查看其何時完成。
    - 執行**VirtualDisk** , 並確認空間傳回狀況良好的 HealthStatus。
5. 修復完成且虛擬磁片狀況良好之後, 請將虛擬磁片參數變更回來。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 讓磁片離線, 然後再次上線, 讓 DiskRecoveryAction 生效:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 將受影響的虛擬磁片新增回 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是一個覆寫參數, 可讓您在不需任何檢查的情況下, 以讀寫模式連接空間磁片區。 屬性可讓您對磁片區無法上線的原因進行診斷。 它與維護模式非常類似, 但您可以在處於失敗狀態的資源上叫用它。 它也可讓您存取資料, 這在「沒有冗余」之類的情況下很有説明, 您可以在其中存取您可以和複製的任何資料。 DiskRecoveryAction 屬性已新增至2018年2月22日, 更新, KB 4077525。


## <a name="detached-status-in-a-cluster"></a>叢集中的卸離狀態 

當您執行**VirtualDisk** Cmdlet 時, 會卸離一或多個儲存空間直接存取虛擬磁片的 OperationalStatus。 不過, **PhysicalDisk**指令程式所報告的 HealthStatus 表示所有實體磁片都處於狀況良好的狀態。

以下是**VirtualDisk** Cmdlet 的輸出範例。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  Size|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 [確定]|                  良好|       True|            10 TB|  節點-01. conto 。|
|Disk3|         Mirror|                 [確定]|                  良好|       True|            10 TB|  節點-01. conto 。|
|Disk2|         Mirror|                 已中斷連結|            不明|       True|            10 TB|  節點-01. conto 。|
|Disk1|         Mirror|                 已中斷連結|            不明|       True|            10 TB|  節點-01. conto 。| 


此外, 節點上可能會記錄下列事件:

```
Log Name: Microsoft-Windows-StorageSpaces-Driver/Operational
Source: Microsoft-Windows-StorageSpaces-Driver 
Event ID: 311 
Level: Error
User: SYSTEM 
Computer: Node#.contoso.local 
Description: Virtual disk {GUID} requires a data integrity scan.  

Data on the disk is out-of-sync and a data integrity scan is required. 

To start the scan, run the following command:   
Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask                

Once you have resolved the condition listed above, you can online the disk by using the following commands in PowerShell:   

Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsReadOnly $false 
Get-VirtualDisk | ?{ $_.ObjectId -Match "{GUID}" } | Get-Disk | Set-Disk -IsOffline  $false
------------------------------------------------------------

Log Name: System
Source: Microsoft-Windows-ReFS 
Event ID: 134
Level: Error 
User: SYSTEM
Computer: Node#.contoso.local 
Description: The file system was unable to write metadata to the media backing volume <VolumeId>. A write failed with status "A device which does not exist was specified." ReFS will take the volume offline. It may be mounted again automatically.
------------------------------------------------------------
Log Name: Microsoft-Windows-ReFS/Operational
Source: Microsoft-Windows-ReFS 
Event ID: 5 
Level: Error 
User: SYSTEM 
Computer: Node#.contoso.local 
Description: ReFS failed to mount the volume. 
Context: 0xffffbb89f53f4180 
Error: A device which does not exist was specified.
Volume GUID:{00000000-0000-0000-0000-000000000000} 
DeviceName: 
Volume Name:
``` 

如果中途區域追蹤 (DRT) 記錄已滿, 則可能會發生卸**離的操作狀態**。 儲存空間會針對鏡像空間使用中途區域追蹤 (DRT), 以確保在發生電源中斷時, 會記錄任何對中繼資料進行中的更新, 以確保儲存空間可以重做或復原作業, 讓儲存空間恢復彈性而當電源恢復時, 就會出現一致的狀態, 而且系統又恢復運作。 如果 DRT 記錄已滿, 則在同步處理和排清 DRT 中繼資料之前, 虛擬磁片將無法上線。 此程式需要執行完整掃描, 這可能需要幾個小時才能完成。

若要修正此問題, 請遵循下列步驟:
1. 從 CSV 移除受影響的虛擬磁片。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在每個未上線的磁片上執行下列命令。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 在卸離的磁片區在線上的每個節點上執行下列命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   這項工作應該在卸離的磁片區已上線的所有節點上起始。 修復應該會自動啟動。 等待修復完成。 它可能會進入暫停狀態並重新啟動。 若要監視進度: 
   - 執行**get-storagejob**來監視修復的狀態, 並查看其何時完成。
   - 執行**VirtualDisk**並確認空間傳回狀況良好的 HealthStatus。
     - 「損毀復原的資料完整性掃描」是不會顯示為儲存作業的工作, 而且沒有任何進度指示器。 如果工作顯示為 [執行中], 則表示它正在執行中。 完成時, 會顯示 [已完成]。

       此外, 您可以使用下列 Cmdlet 來查看執行中排程工作的狀態: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 一旦「損毀復原的資料完整性掃描」完成, 修復就會完成, 且虛擬磁片的狀況良好, 請重新變更虛擬磁片參數。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```

5. 讓磁片離線, 然後再次上線, 讓 DiskRecoveryAction 生效:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 

6. 將受影響的虛擬磁片新增回 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **DiskRunChkdsk 值 7**是用來連接空間磁片區, 並讓磁碟分割處於唯讀模式。 這可讓您藉由觸發修復來進行自我探索和自我修復。 修復將會在裝載後自動執行。 它也可讓您存取資料, 這有助於存取您可以複製的任何資料。 針對某些錯誤狀況 (例如完整的 DRT 記錄), 您必須執行「損毀復原」排程工作的資料完整性掃描。

損毀復原工作**的資料完整性掃描**是用來同步處理和清除完整的中途區域追蹤 (DRT) 記錄。 這項工作可能需要數小時才能完成。 「損毀復原的資料完整性掃描」是不會顯示為儲存作業的工作, 而且沒有任何進度指示器。 如果工作顯示為 [執行中], 則表示它正在執行中。 完成時, 它會顯示為 [已完成]。 如果您在這項工作執行時取消工作或重新開機節點, 則工作將需要從頭開始。

如需詳細資訊, 請參閱[疑難排解儲存空間直接存取的健全狀況和操作狀態](storage-spaces-states.md)。

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>STATUS_IO_TIMEOUT c00000b5 的事件5120 

> [!Important]
> **若為 Windows Server 2016:** 若要減少在套用更新與修正程式時遇到這些徵兆的機率, 建議使用以下的「儲存體維護模式」程式來安裝[2018 年10月18日, Windows Server 2016](https://support.microsoft.com/help/4462928)或更新版本的累積更新當節點目前已安裝從[2018 年5月8日](https://support.microsoft.com/help/4103723)發行的 Windows Server 2016 累積更新[, 2018](https://support.microsoft.com/help/KB4462917)。

當您重新開機 Windows Server 2016 上的節點時, 您可能會收到 STATUS_IO_TIMEOUT c00000b5 的事件 5120, 其累積更新已從[5 月8日](https://support.microsoft.com/help/4103723)發行, 已從 2018 kb 4103723 到[10 月9日, 已安裝 2018 kb 4462917](https://support.microsoft.com/help/4462917) 。

當您重新開機節點時, 事件5120會記錄在系統事件記錄檔中, 並包含下列其中一個錯誤碼:

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

記錄事件5120時, 會產生即時傾印, 以收集可能會造成額外徵兆或效能影響的偵錯工具資訊。 產生即時傾印會建立短暫的暫停, 以讓記憶體快照集寫入傾印檔案。 具有大量記憶體且承受壓力的系統可能會導致節點卸載叢集成員資格, 同時也會記錄下列事件1135。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

從 2018 5 月8日引進的變更到 Windows Server 2016, 這是一種累積更新, 可為儲存空間直接存取叢集內 SMB 網路會話新增 SMB 彈性控制碼。 這麼做的目的是為了改善暫時性網路失敗的復原, 並改善 RoCE 處理網路擁塞的方式。 當 SMB 連線嘗試重新連線, 並在節點重新開機時等待時, 這些改進也會不小心增加時間。 這些問題可能會影響承受壓力的系統。 在未規劃的停機期間, 如果系統等候連線超時, 則也會發現 IO 暫停最多60秒。若要修正此問題, 請安裝[2018 年10月18日, Windows Server 2016](https://support.microsoft.com/help/4462928)或更新版本的累計更新。

*注意*此更新會將 CSV 超時時間與 SMB 連線時間輸出對齊, 以修正此問題。 它不會執行因應措施一節中所述的變更來停用即時傾印產生。

### <a name="shutdown-process-flow"></a>關機程式流程:

1. 執行 VirtualDisk 指令程式, 並確定 HealthStatus 值為狀況良好。
2. 執行下列 Cmdlet 來清空節點:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 執行下列 Cmdlet, 將該節點上的磁片放入儲存體維護模式: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 執行**PhysicalDisk** Cmdlet, 並確定 OperationalStatus 值處於維護模式。
5. 執行 [**重新開機電腦**] Cmdlet 以重新開機節點。
6. 節點重新開機之後, 請執行下列 Cmdlet, 以從存放裝置維護模式移除該節點上的磁片:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 執行下列 Cmdlet 來繼續節點:

   ```powershell
   Resume-ClusterNode
   ```
8. 執行下列 Cmdlet 來檢查重新同步作業的狀態:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>停用即時傾印
若要降低在具有大量記憶體且承受壓力的系統上產生即時傾印的效果, 您可能需要停用即時傾印產生。 以下提供三個選項。

>[!Caution]
>此程式可避免收集 Microsoft 支援服務可能需要調查此問題的診斷資訊。 支援工程師可能必須要求您根據特定的疑難排解案例, 重新啟用即時傾印產生。

有兩種方法可以停用即時傾印, 如下所述。

#### <a name="method-1-recommended-in-this-scenario"></a>方法 1 (在此案例中建議使用)
若要完全停用所有傾印 (包括全系統的即時傾印), 請遵循下列步驟:

1. 建立下列登錄機碼:HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新的**ForceDumpsDisabled**索引鍵底下, 建立 REG_DWORD 屬性作為 GuardedHost, 然後將其值設定為0x10000000。
3. 將新的登錄機碼套用到每個叢集節點。

>[!NOTE]
>您必須重新開機電腦, n 登錄變更才會生效。

設定此登錄機碼之後, 即時傾印建立將會失敗, 並產生「STATUS_NOT_SUPPORTED」錯誤。

#### <a name="method-2"></a>Method 2
根據預設, Windows 錯誤報告每7天只會允許每個報告類型有一個 LiveDump, 每個電腦每5天只有1個 LiveDump。 您可以將下列登錄機碼設定為只允許電腦上的一 LiveDump, 以進行變更。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*注意*您必須重新開機電腦, 變更才會生效。

### <a name="method-3"></a>方法3
若要停用叢集產生的即時傾印 (例如記錄事件5120時), 請執行下列 Cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
此 Cmdlet 會立即影響所有叢集節點, 而不需要重新開機電腦。

## <a name="slow-io-performance"></a>IO 效能緩慢

如果您看到 IO 效能變慢, 請檢查您的儲存空間直接存取設定中是否已啟用快取。 

有兩種方式可以檢查: 


1. 使用叢集記錄檔。 在選擇的文字編輯器中開啟叢集記錄檔, 並搜尋 "[= = = SBL 磁片 = =]。" 這會是產生記錄檔之節點上的磁片清單。 

     已啟用快取的磁片範例:請注意, 這裡的狀態是 CacheDiskStateInitializedAndBound, 而這裡有一個 GUID。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未啟用快取:在這裡, 我們可以看到沒有 GUID 存在, 而且狀態為 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未啟用快取:當所有磁片都屬於相同類型的大小寫時, 預設不會啟用。 在這裡, 我們可以看到沒有 GUID 存在, 而且狀態為 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用 SDDCDiagnosticInfo 中的 Get-PhysicalDisk
    1. 使用 "$d = Import-Clixml GetPhysicalDisk" 開啟 XML 檔案
    2. 執行「ipmo 儲存體」
    3. 執行 "$d"。 請注意, [使用量] 是自動選取, 而不是 [筆記本], 您會看到如下所示的輸出: 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| 使用量| Size|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   [確定]|                良好|      自動選取 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    [確定]|                良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  [確定]|                良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| [確定]|    良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|[確定]|       良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| [確定]|      良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| [確定]|      良好|自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| [確定]|  良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| [確定]| 良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| [確定]|良好| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| [確定]| 良好| 自動選取 |1.82 TB|

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>如何摧毀現有的叢集, 讓您可以再次使用相同的磁片

在儲存空間直接存取叢集中, 一旦您停用儲存空間直接存取並使用[清理磁片磁碟機](deploy-storage-spaces-direct.md#step-31-clean-drives)中所述的清理程式, 叢集存放集區仍會保持離線狀態, 而且健全狀況服務會從叢集移除。

下一步是移除虛設存放集區: 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

現在, 如果您在任何節點上執行**PhysicalDisk** , 您會看到集區中的所有磁片。 例如, 在具有4個 SAS 磁片的4節點叢集的實驗室中, 每個節點會顯示每個節點 100 GB。 在這種情況下, 在停用儲存空間直接存取之後 (這會移除 SBL (儲存匯流排層) 但離開篩選器), 如果您執行**PhysicalDisk**, 它應該會報告4個磁片, 但不包括本機 OS 磁片。 而是改為回報16。 對於叢集中的所有節點而言, 這都是相同的。 當您執行「**磁片磁碟機**」命令時, 您會看到本機連接的磁片編號為0、1、2等等, 如下列範例輸出所示:

|Number| 易記名稱| 序號|HealthStatus|OperationalStatus|總大小| 分割區樣式|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu 。  ||良好 | Online|  127 GB| GPT|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
|1|Msft Virtu 。||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
|2|Msft Virtu 。||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
|4|Msft Virtu 。||良好| 離線| 100 GB| 原始|
|3|Msft Virtu 。||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|
||Msft Virtu 。 ||良好| 離線| 100 GB| 原始|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>當您使用 Enable-clusters2d 建立儲存空間直接存取叢集時, 「不支援的媒體類型」的錯誤訊息  

當您執行**enable-clusters2d** Cmdlet 時, 可能會看到類似下面的錯誤:

![案例6錯誤訊息](media/troubleshooting/scenario-error-message.png)

若要修正此問題, 請確定 HBA 介面卡是以 HBA 模式設定。 不應在 RAID 模式中設定任何 HBA。  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>ClusterStorageSpacesDirect 在「等待 SBL 磁片」或 27% 時停止回應

您會在驗證報告中看到下列資訊:

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


問題在於磁片和 HBA 卡之間的 HPE SAS 擴充器卡。 SAS 擴充器會在連接到擴充器的第一個磁片磁碟機和展開器本身之間建立重複的識別碼。  已在 HPE 智慧陣列[控制器 SAS 擴充器固件中解決此問題:4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3)。

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 系列具有非唯一的 n
在下列範例中, 您可能會看到 Intel SSD DC P4600 系列裝置似乎針對多個命名空間 (例如0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214) 報告類似的16個位元組的問題。


|               uniqueid               | deviceid | MediaType | BusType |               serialnumber               |      size      | canpool | friendlyname | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui. 0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    迅馳     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    迅馳     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    迅馳     |   SSDPE2KE016T7   |
| eui. 0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    迅馳     |   SSDPE2KE016T7   |

若要修正此問題, 請將 Intel 磁片磁碟機上的固件更新為最新版本。  已知從2018年5月 QDV101B1 的固件版本可解決此問題。

[2018 年5月版的 INTEL Ssd 資料中心工具](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包含適用于 INTEL Ssd DC P4600 系列的 QDV101B1 固件更新。


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>實體磁片「狀況良好」和操作狀態為「從集區移除」 

在 Windows Server 2016 儲存空間直接存取叢集中, 您可能會看到一個或多個實體磁片的 HealthStatus 為「狀況良好」, 而 OperationalStatus 為「(從集區移除, 確定)」。 

「從集區移除」是呼叫**PhysicalDisk**時所設定的意圖, 但會儲存在健全狀況中以維護狀態, 並在移除作業失敗時允許復原。 您可以使用下列其中一種方法, 手動將 OperationalStatus 變更為狀況良好:

- 從集區中移除實體磁片, 然後將其新增回去。
- 執行[Clear-PhysicalDiskHealthData 腳本](https://go.microsoft.com/fwlink/?linkid=2034205)以清除意圖。 (可供下載為。TXT 檔案。 您需要將它儲存為。PS1 檔案, 然後才可以執行。)

以下顯示如何執行腳本的一些範例:

- 使用**SerialNumber**參數來指定您需要設定為狀況良好的磁片。 您可以從**WMI MSFT_PhysicalDisk**或**PhysicalDisk**取得序號。 (我們只會在下面的序號使用 0)。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**UniqueId**參數來指定磁片 (再次從**WMI MSFT_PhysicalDisk**或**PhysicalDisk**)。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>檔案複製速度緩慢

您可能會在使用 [檔案瀏覽器] 將大型 VHD 複製到虛擬磁片時遇到問題-檔案複製所花費的時間超出預期。

使用 File Explorer、Robocopy 或 Xcopy 將大型 VHD 複製到虛擬磁片並不是建議的方法, 因為這會導致比預期的效能慢。 複製程式不會通過儲存空間直接存取堆疊, 這在儲存體堆疊上較低, 而是以本機複製程式的方式運作。

如果您想要測試儲存空間直接存取效能, 建議使用 VMFleet 和 Diskspd 來載入和壓力測試伺服器, 以取得基礎線路並設定儲存空間直接存取效能的預期。

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>節點重新開機期間, 您會在其餘節點上看到的預期事件。 

您可以放心地忽略這些事件: 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果您正在執行 Azure Vm, 您可以忽略此事件:

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>使用 Intel P3x00 NVMe 裝置之部署的效能變慢或「遺失通訊」、「IO 錯誤」、「卸離」或「無冗余」錯誤

我們已識別出一個重大問題, 其會影響一些使用硬體的儲存空間直接存取使用者, 這些是以「維護版本8」之前的固件版本為基礎的 Intel P3x00 系列 NVM Express (NVMe) 裝置。 

>[!NOTE]
> 個別 Oem 可能會有裝置以具有唯一固件版本字串的 Intel P3x00 系列 NVMe 裝置為基礎。 如需最新固件版本的詳細資訊, 請洽詢您的 OEM。

如果您在部署中使用的硬體是以 Intel P3x00 系列的 NVMe 裝置為基礎, 建議您立即套用最新可用的固件 (至少維護版本 8)。 這[篇 Microsoft 支援服務文章](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供有關此問題的其他資訊。 
