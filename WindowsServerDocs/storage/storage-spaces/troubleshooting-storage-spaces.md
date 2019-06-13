---
title: 儲存空間直接存取疑難排解
description: 了解如何疑難排解您的儲存空間直接存取部署。
keywords: 儲存空間
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: 44bcf48f3e4a3b4b49ff027d3aa3e5704865e7b5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447879"
---
# <a name="troubleshoot-storage-spaces-direct"></a>疑難排解的儲存空間直接存取

> 適用於：Windows Server 2019，Windows Server 2016

您可以使用下列資訊來疑難排解您的儲存空間直接存取部署。

一般情況下，開始進行下列步驟：

1. 確認 SSD 品牌/型號已通過認證的 Windows Server 2016 和 Windows Server 2019 使用 Windows Server Catalog。 與廠商確認，磁碟機支援儲存空間直接存取。
2. 檢查任何故障的磁碟機的儲存體。 您可以使用 儲存體管理軟體來檢查磁碟機的狀態。 如果任何磁碟機故障，請與您的廠商。 
3. 更新儲存體和磁碟機韌體，如有必要。
   請確定所有節點上安裝最新的 Windows 更新。 您可以取得最新的更新從 Windows Server 2016 [Windows 10 和 Windows Server 2016 更新歷程記錄](https://aka.ms/update2016)以及從 Windows Server 2019 [Windows 10 和 Windows Server 2019 更新歷程記錄](https://support.microsoft.com/help/4464619)。
4. 網路介面卡驅動程式和韌體更新。
5. 執行叢集驗證及檢閱儲存空間直接存取 > 一節，並確認會正確報告將會使用快取的磁碟機和任何錯誤。

如果您仍然遇到問題，請檢閱下列案例。

## <a name="virtual-disk-resources-are-in-no-redundancy-state"></a>虛擬磁碟資源位於沒有備援狀態
儲存空間直接存取系統的節點意外重新啟動的當機或電源失敗。 一或多個虛擬磁碟可能會無法上線，然後，您會看到描述 「 沒有足夠資訊冗餘。 」

|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|大小| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| Mirror| [確定]|  良好| True|  10 TB|  節點 01.conto...|
|Disk3         |Mirror                 |[確定]                          |良好       |True            |10 TB | 節點 01.conto...|
|Disk2         |Mirror                 |沒有備援               |Unhealthy     |True            |10 TB | 節點 01.conto...|
|Disk1         |Mirror                 |{沒有備援，車子}  |Unhealthy     |True            |10 TB | 節點 01.conto...| 

此外之後嘗試將虛擬磁碟連線，下列資訊會記錄在叢集記錄檔 (DiskRecoveryAction)。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

**No 冗餘的操作狀態**可能發生磁碟失敗時，或如果系統無法存取虛擬磁碟上的資料。 如果在重新開機，就會發生在節點上的節點上的維護期間，會發生此問題。

若要修正此問題，請遵循下列步驟：

1. 從 CSV 移除受影響的虛擬磁碟。 這會將它們放在叢集中的"Available storage"群組，並且顯示為 ResourceType 的 「 實體磁碟 」。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 在節點上擁有可用的存放裝置群組，請在沒有備援處於每個磁碟上執行下列命令。 若要識別"Available Storage"群組位於您哪一個節點可以執行下列命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 設定磁碟復原動作，然後再啟動 磁碟。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 應該會自動啟動修復。 等候完成修復。 它可能會進入暫停狀態，並重新啟動。 若要監視進度： 
    - 執行**Get-storagejob**監視修復狀態，以及查看何時完成。
    - 執行**Get-virtualdisk**並確認空間傳回 HealthStatus 的狀況良好。
5. 在修復後完成和虛擬磁碟是狀況良好、 虛擬磁碟參數後的變更。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 請將磁碟離線，然後再連線一次將 DiskRecoveryAction 生效：

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 將受影響的虛擬磁碟新增回 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是覆寫參數，可讓您附加空間中的磁碟區讀寫模式，而不需任何檢查。 屬性可讓您對執行診斷為何，磁碟區不會再上線。 為維護模式非常類似，但您可以在 「 失敗 」 狀態中的資源上叫用。 它也可讓您存取資料，它可以是很有幫助，例如 「 沒有備援 」，您可以取得存取任何資料可以並將它複製。 在 2 月 22，2018 年更新 KB 4077525 已 DiskRecoveryAction 屬性。


## <a name="detached-status-in-a-cluster"></a>在叢集中的已卸離的狀態 

當您執行**Get-virtualdisk** cmdlet，為一或多個儲存空間直接存取虛擬磁碟中斷連結 OperationalStatus。 HealthStatus 不過，藉由回報**Get-physicaldisk**指令程式會指出所有實體磁碟已處於狀況良好的狀態。

以下是範例的輸出**Get-virtualdisk** cmdlet。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  大小|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         Mirror|                 [確定]|                  良好|       True|            10 TB|  節點 01.conto...|
|Disk3|         Mirror|                 [確定]|                  良好|       True|            10 TB|  節點 01.conto...|
|Disk2|         Mirror|                 已中斷連結|            不明|       True|            10 TB|  節點 01.conto...|
|Disk1|         Mirror|                 已中斷連結|            不明|       True|            10 TB|  節點 01.conto...| 


此外，在節點上可能會記錄下列事件：

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

**中斷連結的操作狀態**如果廢棄區域追蹤 （DRT） 記錄檔已滿，可能會發生。 儲存空間會使用廢棄區域追蹤 (DRT) 為鏡像空間，以確定當發生電源中斷，任何進行中的更新中繼資料會記錄以確定的儲存空間可以重做或復原作業，使符合有彈性的儲存空間當電源恢復一致的狀態和系統恢復運作。 DRT 記錄檔已滿時，如果虛擬磁碟無法上線之前 DRT 中繼資料會同步處理和排清。 此程序需要執行完整掃描，這可能需要數小時才能完成。

若要修正此問題，請遵循下列步驟：
1. 從 CSV 移除受影響的虛擬磁碟。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 執行下列命令未出現在線上時每個磁碟上。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 每個已卸離的磁碟區已上線的節點上執行下列命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   此工作應該卸離的磁碟區在線上的所有節點上起始。 應該會自動啟動修復。 等候完成修復。 它可能會進入暫停狀態，並重新啟動。 若要監視進度： 
   - 執行**Get-storagejob**監視修復狀態，以及查看何時完成。
   - 執行**Get-virtualdisk**並確認空間傳回 HealthStatus 的狀況良好。
     - 「 資料完整性掃描的當機復原 」 的工作，不會顯示為儲存體工作，而且沒有任何進度指示器。 如果工作會顯示為執行中，它正在執行。 完成之後，它會顯示已完成。

       此外，您可以使用下列 cmdlet 來檢視執行的排程工作的狀態： 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 一旦完成 「 資料完整性掃描的當機復原 」 時，修復完成和虛擬磁碟是狀況良好、 虛擬磁碟參數後的變更。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. 將受影響的虛擬磁碟新增回 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
   **DiskRunChkdsk 值 7**來附加空間磁碟區，並在唯讀模式中有資料分割。 如此可自行探索的空間和自我修復的觸發修復。 修復將會執行一次自動掛接。 它也可讓您存取資料，很有幫助您可以複製任何資料的存取。 某些錯誤狀況，例如完整的 DRT 記錄檔中，您要執行資料完整性掃描的當機復原排定的工作。

**當機復原工作的資料完整性掃描**用來同步處理和清除完整廢棄區域追蹤 （DRT） 記錄檔。 這項工作可能需要數小時才能完成。 「 資料完整性掃描的當機復原 」 的工作，不會顯示為儲存體工作，而且沒有任何進度指示器。 如果工作會顯示為執行中，它正在執行。 完成之後，它會顯示為已完成。 如果您取消工作，或重新啟動節點，此工作執行時，工作必須從頭重新開始。

如需詳細資訊，請參閱 <<c0> [ 疑難排解儲存空間直接存取的健康情況和操作狀態](storage-spaces-states.md)。

## <a name="event-5120-with-statusiotimeout-c00000b5"></a>STATUS_IO_TIMEOUT c00000b5 5120 事件 

> [!Important]
> **適用於 Windows Server 2016:** 若要減少發生這些徵兆時套用的更新與修正的機會，建議使用下列存放裝置維護模式的程序來安裝[2018 年 10 月 18 日，累計更新，適用於 Windows Server 2016](https://support.microsoft.com/help/4462928)或更新版本時節點目前已安裝 Windows Server 2016 的累積更新從發行[2018 5 月 8 日](https://support.microsoft.com/help/4103723)要[2018 年 10 月 9 日](https://support.microsoft.com/help/KB4462917)。

重新啟動 Windows Server 2016 上的節點，從已發行的累計更新之後，您可能會收到事件 5120 STATUS_IO_TIMEOUT c00000b5 [8，2018 年 KB 4103723](https://support.microsoft.com/help/4103723)到[2018 年 10 月 9 日 KB 4462917](https://support.microsoft.com/help/4462917)安裝。

當您重新啟動節點時，事件 5120 會記錄在系統事件記錄檔，並包含的其中一個下列的錯誤代碼：

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

記錄事件 5120 時，即時傾印會產生以收集偵錯資訊可能會導致其他徵兆或效能造成影響。 產生的即時傾印建立短暫的暫停，若要啟用的寫入傾印檔案的記憶體快照。 系統中有大量記憶體和壓力下可能會造成節點退出叢集成員資格，並造成下列事件 1135 記錄。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

引進在 2018 年 5 月 8，Windows Server 2016 中，已將 SMB 彈性處理儲存空間直接存取叢集內 SMB 網路工作階段的累積更新的變更。 這麼做是為了改善暫時性網路失敗的恢復功能並改善 RoCE 如何處理網路壅塞。 這些改進也不小心增加 SMB 連線嘗試重新連線時的逾時和逾時的等候，重新啟動節點時。 這些問題可能會影響在壓力下的系統。 在規劃的停機時間，最多 60 秒的 IO 暫停也觀察到系統等候逾時的連線時。若要修正此問題，請安裝[2018 年 10 月 18 日，Windows Server 2016 的累積更新](https://support.microsoft.com/help/4462928)或更新版本。

*請注意*此更新將對齊 SMB 連線逾時，若要修正此問題的 CSV 逾時。 它未實作的變更，若要停用即時的傾印產生因應措施一節所述。

### <a name="shutdown-process-flow"></a>關機程序流程：

1. 執行 Get-virtualdisk cmdlet，並確定的 healthstatus 內容值為 狀況良好。
2. 清空節點執行下列 cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 將存放裝置維護模式中的該節點上的磁碟，藉由執行下列 cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 執行**Get-physicaldisk** cmdlet，並確定 OperationalStatus 值是在維護模式。
5. 執行**Restart-computer** cmdlet 來重新啟動節點。
6. 節點重新啟動之後，請從存放裝置維護模式移除該節點上的磁碟，藉由執行下列 cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 恢復節點，執行下列 cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. 檢查重新同步處理工作的狀態，執行下列 cmdlet:

   ```powershell
   Get-StorageJob
   ```

### <a name="disabling-live-dumps"></a>停用即時傾印
若要減輕有大量記憶體，也在壓力下的系統上的即時傾印產生的效果，您可能此外要停用即時傾印的產生。 以下提供三個選項。

>[!Caution]
>此程序可以防止 Microsoft 支援服務可能需要調查此問題的診斷資訊的集合。 支援的代理程式可能需要要求您重新啟用即時的傾印產生根據特定的疑難排解案例。

有兩種方法可以停用即時傾印，如下所述。

#### <a name="method-1-recommended-in-this-scenario"></a>方法 1 （在此案例中的建議選項）
若要完全停用所有傾印，包括即時傾印整個系統，請遵循下列步驟：

1. 建立下列登錄機碼：HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新**ForceDumpsDisabled**索引鍵，建立 REG_DWORD 屬性為 GuardedHost，然後將其值設定為 0x10000000。
3. 將新的登錄機碼套用至每個叢集節點。

>[!NOTE]
>您必須重新啟動電腦，使 n 變更才會生效。

此登錄機碼設定即時之後，傾印建立將會失敗，並產生 「 STATUS_NOT_SUPPORTED 」 錯誤。

#### <a name="method-2"></a>Method 2
根據預設，Windows 錯誤報告可讓每個報表類型，每 7 天只能有一個 LiveDump 和 1 個 LiveDump 每部機器每 5 天。 您可以藉由設定下列登錄機碼，以便只一個 LiveDump 在電腦上永久變更。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*請注意*您必須重新啟動電腦，變更才會生效。

### <a name="method-3"></a>方法 3
若要停用叢集產生的 （例如何時會記錄事件 5120） 的即時傾印，請執行下列 cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
此 cmdlet 後會立即生效，而不需要重新啟動電腦的所有叢集節點上。

## <a name="slow-io-performance"></a>IO 效能變慢

如果您看到的 IO 效能變慢，請檢查快取是否已啟用儲存空間直接存取組態中。 

有兩種方式可檢查： 


1. 使用叢集記錄檔。 在 [選擇的文字編輯器中開啟叢集記錄檔，並搜尋"[=== SBL 磁碟 ===]。 」 這會在節點所產生的記錄檔的磁碟清單。 

     快取已啟用磁碟範例：請注意這裡的狀態是 CacheDiskStateInitializedAndBound，而且沒有出現 GUID 的以下。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未啟用快取：這裡我們可以看到 GUID 不存在，且狀態 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    未啟用快取：預設不啟用當所有磁碟都都是相同類型的大小寫。 這裡我們可以看到 GUID 不存在，且狀態 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用 Get-PhysicalDisk.xml 從 SDDCDiagnosticInfo
    1. 開啟 XML 檔案使用"$d = Import-clixml GetPhysicalDisk.XML"
    2. 執行 「 ipmo 儲存體 」
    3. 執行 「 $d"。 請注意，使用自動選取，沒有筆記本，您會看到如下輸出： 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| 使用量| 大小|
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

## <a name="how-to-destroy-an-existing-cluster-so-you-can-use-the-same-disks-again"></a>如何終結現有的叢集，因此您可以再次使用相同的磁碟

在儲存空間直接存取叢集中，一次您停用儲存空間直接存取並使用清理程序中所述[清理磁碟機](deploy-storage-spaces-direct.md#step-31-clean-drives)、 叢集的儲存集區仍處於離線狀態，以及從移除健全狀況服務叢集。

下一個步驟是要移除的虛設項目儲存集區： 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

現在，如果您執行**Get-physicaldisk**上的任一個節點，您會看到集區中的所有磁碟。 例如，在一個 4 節點的叢集具有 4 個 SAS 磁碟，各 100 GB 呈現給每個節點的實驗室。 在此情況下，儲存空間直接存取的已停用之後，其中移除 SBL （存放匯流排層） 但會保留篩選時，如果您執行**Get-physicaldisk**，它應該報告 4 個磁碟，但不包括本機 OS 磁碟。 而是它會將 16 回報改。 這是相同的叢集中所有節點。 當您執行**Get-disk**命令中，您會看到在本機連接的磁碟編號為 0、 1、 2 等等，如下列範例輸出所示：

|Number| 易記名稱| 序號|HealthStatus|OperationalStatus|大小總計| 磁碟分割樣式|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||良好 | Online|  127 GB| GPT|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
|1|Msft Virtu...||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
|2|Msft Virtu...||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
|4|Msft Virtu...||良好| 離線| 100 GB| RAW|
|3|Msft Virtu...||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|
||Msft Virtu... ||良好| 離線| 100 GB| RAW|


## <a name="error-message-about-unsupported-media-type-when-you-create-an-storage-spaces-direct-cluster-using-enable-clusters2d"></a>錯誤訊息 「 不支援的媒體類型 」 有關當您建立使用 Enable-ClusterS2D 的儲存空間直接存取叢集  

當您執行時，您可能會看到如下所示的錯誤**Enable-ClusterS2D** cmdlet:

![案例 6 錯誤訊息](media/troubleshooting/scenario-error-message.png)

若要修正此問題，請確定 HBA 配接器設定 HBA 模式。 沒有 HBA 應該設定為 RAID 模式。  

## <a name="enable-clusterstoragespacesdirect-hangs-at-waiting-until-sbl-disks-are-surfaced-or-at-27"></a>在 '等候直到 SBL 磁碟會顯示' 或 27%，Enable-clusterstoragespacesdirect 停止回應

您會看到驗證報告中的下列資訊：

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 


問題在於 HPE SAS 擴展器卡片位於磁碟與 HBA 卡之間。 SAS 擴展器會建立第一個磁碟機連線到展開器，然後展開工具本身之間的重複識別碼。  已解決此問題在[HPE 智慧陣列控制器 SAS 擴展器韌體：4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3).

## <a name="intel-ssd-dc-p4600-series-has-a-non-unique-nguid"></a>Intel SSD DC P4600 序列具有非唯一 NGUID
您可能會發現問題的 Intel SSD DC P4600 系列裝置似乎會回報類似 16 位元組 NGUID 例如 0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214 在下列範例中的多個命名空間。


|               uniqueid               | deviceid | MediaType | BusType |               serialnumber               |      size      | canpool | friendlyname | OperationalStatus |
|--------------------------------------|----------|-----------|---------|------------------------------------------|----------------|---------|--------------|-------------------|
|           5000CCA251D12E30           |    0     |    HDD    |   SAS   |                 7PKR197G                 | 10000831348736 |  False  |     HGST     |  HUH721010AL4200  |
| eui.0100000001000000E4D25C000014E214 |    4     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C000014E214 |    5     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_0014_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    6     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |
| eui.0100000001000000E4D25C0000EEE214 |    7     |    SSD    |  NVMe   | 0100_0000_0100_0000_E4D2_5C00_00EE_E214. | 1600321314816  |  True   |    INTEL     |   SSDPE2KE016T7   |

若要修正此問題，請為最新版本更新 Intel 磁碟機上的軔體。  韌體版本從 2018 年 QDV101B1 稱為以解決此問題。

[2018 年發行，Intel SSD 資料中心工具之](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包含韌體更新、 QDV101B1，Intel SSD DC P4600 數列。


## <a name="physical-disk-healthy-and-operational-status-is-removing-from-pool"></a>實體磁碟 「 狀況良好 」 和操作狀態是 「 移除從集區 」 

在 Windows Server 2016 儲存空間直接存取叢集中，您可能會看到一或多個實體磁碟的 healthstatus 內容為 「 良好 」，而 OperationalStatus 為 "（正在移除從集區，確定）。 」 

「 從集區中移除"時，會意圖設定**Remove-physicaldisk**呼叫，但儲存在健康狀態，以維護狀態，並允許復原，如果移除作業失敗。 您可以手動變更 OperationalStatus 為 「 狀況良好與其中一種下列方法：

- 從集區中，移除實體磁碟，然後將它新增回來。
- 執行[清除 PhysicalDiskHealthData.ps1 指令碼](https://go.microsoft.com/fwlink/?linkid=2034205)清除目的。 (可下載。TXT 檔案。 您必須將它儲存為。PS1 檔案才能執行它。）

以下是一些範例，示範如何執行指令碼：

- 使用**SerialNumber**參數來指定您要設為狀況良好的磁碟。 您可以取得從序號**WMI MSFT_PhysicalDisk**或是**Get-physicaldisk**。 （我們只會使用 0 以下序號。）

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**UniqueId**參數來指定磁碟 (從再次**WMI MSFT_PhysicalDisk**或是**Get-physicaldisk**)。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## <a name="file-copy-is-slow"></a>檔案複製變慢

您可能看過使用檔案總管 中，將大型 VHD 複製到虛擬磁碟的問題-檔案複製所花的時間超出預期。

使用檔案總管，Robocopy 或 Xcopy，將大型 VHD 複製到虛擬磁碟不是建議的方法，這會導致低於預期的效能。 複製程序不會透過儲存空間直接存取堆疊，它位於較低的儲存體堆疊，並改為就像是本機複製程序。

如果您想要測試儲存空間直接存取的效能，我們建議您使用 VMFleet 和 Diskspd 負載和壓力測試來取得基準線及設定的儲存空間直接存取的效能期望的伺服器。

## <a name="expected-events-that-you-would-see-on-rest-of-the-nodes-during-the-reboot-of-a-node"></a>預期您會看到的其他節點的節點重新開機期間的事件。 

它可以放心地忽略這些事件： 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果您執行 Azure Vm，您可以忽略此事件：

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 

## <a name="slow-performance-or-lost-communication-io-error-detached-or-no-redundancy-errors-for-deployments-that-use-intel-p3x00-nvme-devices"></a>效能變慢或 「 遺失通訊，「 「 IO 錯誤 」、 「 中斷 」 或 「 否備援 」 錯誤，使用 Intel P3x00 NVMe 裝置的部署

我們已識別會影響使用 Intel P3x00 NVM Express (NVMe) 裝置系列 」 維護版本為 8。"之前的韌體版本為基礎的硬體部分儲存空間直接存取使用者的重要問題 

>[!NOTE]
> 個別的 Oem 可能會根據 Intel P3x00 系列使用唯一的韌體版本字串的 NVMe 裝置的裝置。 如需詳細資訊的最新的韌體版本，請連絡您的 OEM。

如果您在您的 Intel P3x00 系列的 NVMe 裝置為基礎的部署中使用的硬體，我們建議您立即套用最新可用的韌體 (至少維護版本 8)。 這[Microsoft 支援服務文章](https://support.microsoft.com/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供此問題的其他資訊。 
