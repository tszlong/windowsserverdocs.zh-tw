---
title: 儲存空間直接存取疑難排解
description: 了解如何針對您的儲存空間直接存取部署進行疑難排解。
keywords: 儲存空間
ms.prod: windows-server-threshold
ms.author: ''
ms.technology: storage-spaces
ms.topic: article
author: kaushika-msft
ms.date: 10/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: c7c9573732ff1cf5a998588b1aec81915c227ee2
ms.sourcegitcommit: 5d55c3ebd1ceee7229ea9a9989c69cf93757ed88
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/21/2019
ms.locfileid: "9256931"
---
# 疑難排解儲存空間直接存取

您可以使用下列資訊進行疑難排解您的儲存空間直接存取部署。

一般而言，開始依照下列步驟：

1. 確認 SSD 品牌/型號已使用 Windows Server 類別目錄的 Windows Server 2016 的認證。 確認與廠商的儲存空間直接存取支援的磁碟機。
2. 檢查任何故障的磁碟機的儲存空間。 使用存放裝置管理軟體來檢查磁碟機的狀態。 如果任何磁碟機故障，可與您的廠商搭配。 
3. 更新儲存體，如有必要的磁碟機韌體。
   請確定所有節點上安裝最新的 Windows 更新。 您可以取得最新的更新，以從 Windows Server 2016 [https://aka.ms/update2016](https://aka.ms/update2016)。
4. 網路介面卡驅動程式和韌體更新。
5. 執行叢集驗證並檢閱儲存空間直接存取的區段，確保正確報告將用於快取磁碟機並沒有任何錯誤。

如果您仍有問題，請檢閱下列案例。

## 虛擬磁碟資源是在否備援狀態
儲存空間直接存取的系統的節點重新啟動意外因為當機或電源失敗。 一或多個虛擬磁碟可能會無法上線，然後您會看到描述 「 沒有足夠資訊進行備援。 」
    
|FriendlyName|ResiliencySettingName| OperationalStatus| HealthStatus| IsManualAttach|大小| PSComputerName|
|------------|---------------------| -----------------| ------------| --------------|-----| --------------|
|Disk4| 鏡像| 確定|  Healthy| True|  10 TB|  節點 01.conto...|
|Disk3         |鏡像                 |確定                          |Healthy       |True            |10 TB | 節點 01.conto...|
|Disk2         |鏡像                 |沒有備援               |Unhealthy     |True            |10 TB | 節點 01.conto...|
|Disk1         |鏡像                 |{沒有備援，車子}  |Unhealthy     |True            |10 TB | 節點 01.conto...| 

此外後嘗試使虛擬磁碟上線，下列資訊會記錄在叢集記錄檔 (DiskRecoveryAction) 中。  

```
[Verbose] 00002904.00001040::YYYY/MM/DD-12:03:44.891 INFO [RES] Physical Disk <DiskName>: OnlineThread: SuGetSpace returned 0.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 WARN [RES] Physical Disk < DiskName>: Underlying virtual disk is in 'no redundancy' state; its volume(s) may fail to mount.
[Verbose] 00002904.00001040:: YYYY/MM/DD -12:03:44.891 ERR [RES] Physical Disk <DiskName>: Failing online due to virtual disk in 'no redundancy' state. If you would like to attempt to online the disk anyway, first set this resource's private property 'DiskRecoveryAction' to 1. We will try to bring the disk online for recovery, but even if successful, its volume(s) or CSV may be unavailable. 
``` 

如果磁碟失敗或系統已無法存取虛擬磁碟上的資料，可能會發生**否備援操作狀態**。 如果在節點上的維護期間發生在節點上的重新開機，會發生此問題。
    
若要修正此問題，請依照下列步驟：

1. 從 CSV 移除受影響的虛擬磁碟。 這會將它們放在叢集中的 「 可用的存放裝置 」 群組，並開始顯示為 \ [ResourceType 的 「 實體磁碟 」。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 上擁有可用的存放裝置群組，執行下列命令，在沒有備援狀態每個磁碟上的節點。 若要找出哪一個節點 」 可用的存放裝置 」 群組是在您可以執行下列命令。

   ```powershell
   Get-ClusterGroup
   ```
3. 設定磁碟修復動作，並啟動 [磁碟。
   ```powershell
   Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 1
   Start-ClusterResource -Name "VdiskName"
   ```
4. 修復應會自動啟動。 等待以完成修復。 它可能會進入暫停狀態，並重新開始。 若要監視進度： 
    - 執行**Get StorageJob**監視修復的狀態，並查看它完成時。
    - 執行**Get VirtualDisk** ，並確認空間傳回 Healthstatus\ 的健康情況良好。
5. 在修復之後完成和虛擬磁碟是狀況良好，變更回虛擬磁碟參數。

   ```powershell
    Get-ClusterResource "VdiskName" | Set-ClusterParameter -Name DiskRecoveryAction -Value 0
   ```
6. 採取磁碟離線，然後再連線一次到已生效 DiskRecoveryAction:

   ```powershell
   Stop-ClusterResource "VdiskName"
   Start-ClusterResource "VdiskName"
   ``` 
7. 將受影響的虛擬磁碟新增到 CSV。

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```

**DiskRecoveryAction**是覆寫交換器，可讓附加在沒有任何檢查的讀寫模式中的空間磁碟區。 屬性可讓您為何要將磁碟區將不會上線到進行診斷。 維護模式非常類似，但您可以在失敗狀態的資源上叫用。 它也可讓您存取的資料，可以是很有幫助，例如 「 否備援，「 您可以在此取得存取任何資料可以，並將它複製。 2 月 22 2018 年日，更新，KB 4077525 已加入 DiskRecoveryAction 屬性。
    
    
## 在叢集中分離的狀態 

當您執行**Get-VirtualDisk** cmdlet 時，針對一或多個儲存空間直接存取的虛擬磁碟 OperationalStatus 中斷連結。 不過， **Get-physicaldisk** cmdlet 所回報 Healthstatus\ 表示所有實體磁碟狀況良好的狀態。

以下是輸出的**Get-VirtualDisk** cmdlet 範例。

|FriendlyName|  ResiliencySettingName|  OperationalStatus|   HealthStatus|  IsManualAttach|  大小|   PSComputerName|
|-|-|-|-|-|-|-|
|Disk4|         鏡像|                 確定|                  Healthy|       True|            10 TB|  節點 01.conto...|
|Disk3|         鏡像|                 確定|                  Healthy|       True|            10 TB|  節點 01.conto...|
|Disk2|         鏡像|                 中斷連結|            不明|       True|            10 TB|  節點 01.conto...|
|Disk1|         鏡像|                 中斷連結|            不明|       True|            10 TB|  節點 01.conto...| 


此外，在節點上可能會記錄的下列事件：

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

如果不正常的地區追蹤 (DRT) 記錄檔已滿，可能會發生**分離的操作狀態**。 儲存空間使用廢棄區域的鏡像空間追蹤 (DRT)，以確保電源失敗發生時，任何中繼資料的 「 執行中 」 更新會記錄以確定可以取消復原的儲存空間，或復原作業來將帶的儲存空間回彈性並還原電源時保持一致的狀態，而系統恢復。 如果 DRT 記錄檔已滿，虛擬磁碟無法上線，直到 DRT 中繼資料是同步處理，排清 denorm。 此程序需要執行完整掃描，可能需要數個小時完成。
    
若要修正此問題，請依照下列步驟：
1. 從 CSV 移除受影響的虛擬磁碟。

   ```powershell
   Remove-ClusterSharedVolume -name "VdiskName"
   ``` 
2. 不即將推出線上每個磁碟上，執行下列命令。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 7
   Start-ClusterResource -Name "VdiskName"
   ``` 
3. 中斷連接的磁碟區在線上的每個節點上執行下列命令。 

   ```powershell
   Get-ScheduledTask -TaskName "Data Integrity Scan for Crash Recovery" | Start-ScheduledTask 
   ```
   這項工作應中斷連接的磁碟區在線上的所有節點上起始。 修復應會自動啟動。 等待以完成修復。 它可能會進入暫停狀態，並重新開始。 若要監視進度： 
   - 執行**Get StorageJob**監視修復的狀態，並查看它完成時。
   -  執行**Get VirtualDisk** ，並確認空間傳回 Healthstatus\ 的健康情況良好。
    - 「 資料完整性掃描的損毀修復 」 是工作，不會顯示為儲存工作，並沒有任何進度指示器。 如果工作會顯示為執行，它正在執行。 當它完成時，它會顯示已完成。
    
       此外，您可以檢視正在執行的排程工作的狀態，使用下列 cmdlet: 
       ```powershell
       Get-ScheduledTask | ? State -eq running
       ``` 
4. 儘速完成 」 資料完整性掃描的損毀修復 」 時，修復完成和虛擬磁碟是狀況良好，變更回虛擬磁碟參數。 

   ```powershell
   Get-ClusterResource -Name "VdiskName" | Set-ClusterParameter DiskRunChkDsk 0 
   ```
5. 將受影響的虛擬磁碟新增到 CSV。    

   ```powershell
   Add-ClusterSharedVolume -name "VdiskName"
   ```  
**DiskRunChkdsk 值 7**會用來將附加空間磁碟區，並為唯讀模式中有磁碟分割。 這可讓自我探索空間和以 self-heal 觸發修復。 修復會執行一次自動掛接。 它也可讓您存取的資料，可以是很有幫助來取得任何資料，您可以複製的存取權。 針對某些錯誤狀況，完整 DRT 記錄檔，例如，您需要執行損毀修復排定的工作資料完整性掃描。
    
**當機的修復工作的資料完整性掃描**用來同步處理，並清除完整廢棄區域追蹤 (DRT) 記錄檔。 這項工作可能需要數個小時才能完成。 「 資料完整性掃描的損毀修復 」 是工作，不會顯示為儲存工作，並沒有任何進度指示器。 如果工作會顯示為執行，它正在執行。 當它完成時，它將會顯示為已完成。 如果您取消工作，或重新啟動節點，這項工作執行時，必須以重新開始從開始工作。

如需詳細資訊，請參閱[疑難排解儲存空間直接存取的健康情況與操作狀態](storage-spaces-states.md)。
    
## 事件 5120 與 STATUS_IO_TIMEOUT c00000b5 

> [!Important]
> 若要減少套用修正程式的更新時遇到這些問題的機會，建議使用下列存放裝置維護模式程序安裝在[2018 年 10 月 18 累積更新，適用於 Windows Server 2016](https://support.microsoft.com/help/4462928)或更新版本當節點目前已安裝已發行的 Windows Server 2016 累積更新從[2018 年月 8 日](https://support.microsoft.com/help/4103723)到[2018 年 10 月 9 日](https://support.microsoft.com/help/KB4462917)。

在您重新啟動 Windows Server 2016 上的節點使用發行一次從[可能 8，2018 KB 4103723](https://support.microsoft.com/help/4103723)來安裝[2018 年 KB 4462917 年 10 月 9 日](https://support.microsoft.com/help/4462917)累積更新後，您可能會收到 STATUS_IO_TIMEOUT c00000b5 5120 事件。

當您重新啟動節點時，事件 5120 已經登入系統事件記錄檔，並包含下列錯誤碼的其中一個：

```
Event Source: Microsoft-Windows-FailoverClustering
Event ID: 5120
Description:    Cluster Shared Volume 'CSVName' ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_IO_TIMEOUT(c00000b5)'. All I/O will temporarily be queued until a path to the volume is reestablished. 

Cluster Shared Volume ‘CSVName’ ('Cluster Virtual Disk (CSVName)') has entered a paused state because of 'STATUS_CONNECTION_DISCONNECTED(c000020c)'. All I/O will temporarily be queued until a path to the volume is reestablished.    
```

當系統會記錄事件 5120 時，即時傾印會產生收集，可能會導致其他問題或效能造成影響的偵錯資訊。 產生的實際的傾印，會建立短暫的暫停，若要啟用那麼建立快照的傾印檔案寫入的記憶體。 系統有大量記憶體並壓力下可能會造成卸除退出叢集成員資格，並也會導致下列事件 1135 記錄的節點。

```
Event source: Microsoft-Windows-FailoverClustering
Event ID: 1135  
Description: Cluster node 'NODENAME'was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

2018 年 8 累積更新，來新增儲存空間直接存取叢集內 SMB 網路工作階段的 SMB 具彈性處理引進變更。 This was done to improve resiliency to transient network failures and improve how RoCE handles network congestion.

這些改進也會不慎增加 SMB 連線數次嘗試連線時的逾時，並等待逾時，重新啟動節點時。 這些問題可能會影響壓力下的系統。 在非預期的停機時間，期間最多 60 秒的 IO 暫停也觀察當系統在等待逾時的連線。

若要修正此問題，請安裝[2018 年 10 月 18，累積更新，適用於 Windows Server 2016](https://support.microsoft.com/help/4462928)或更新版本。

*注意：* 此更新對齊與 SMB 連線逾時，若要修正此問題的 CSV 逾時。 它不會實作，變更就停用即時傾印產生因應措施一節所述。
    
### 關機處理程序流程：

1. 執行 Get-VirtualDisk cmdlet，並確定 Healthstatus\ 值為 [狀況良好。
2. 清空節點執行下列 cmdlet:

   ```powershell
   Suspend-ClusterNode -Drain
   ```
3. 將磁碟該存放裝置維護模式中的節點上，藉由執行下列 cmdlet: 

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Enable-StorageMaintenanceMode
   ```
4. 執行**Get-physicaldisk** cmdlet，並確定 OperationalStatus 值是在維護模式。
5. 執行此**重新啟動電腦**cmdlet，以重新啟動節點。
6. 節點重新開機後，移除存放裝置維護模式，從該節點上的磁碟，藉由執行下列 cmdlet:

   ```powershell
   Get-StorageFaultDomain -type StorageScaleUnit | Where-Object {$_.FriendlyName -eq "<NodeName>"} | Disable-StorageMaintenanceMode
   ```
7. 繼續節點執行下列 cmdlet:

   ```powershell
   Resume-ClusterNode
   ```
8. 執行下列 cmdlet 來檢查重新同步工作的狀態：

   ```powershell
   Get-StorageJob
   ```

### 停用即時傾印
若要降低有大量記憶體並壓力下的系統上的即時傾印產生的效果，您此外可能會想要停用即時傾印產生。 以下提供三個選項。

>[!Caution]
>此程序可以防止 Microsoft 支援服務可能需要進行調查此問題的診斷資訊的集合。 支援代理程式可能會有詢問您重新啟用動態傾印代根據特定的疑難排解案例。

有兩種方法，來停用即時傾印，如下所述。

#### 方法 1 （在此案例中的建議選項）
完全停用所有的傾印，包括即時傾印全系統，請依照下列步驟：

1. 建立下列登錄機碼： HKLM\System\CurrentControlSet\Control\CrashControl\ForceDumpsDisabled
2. 在新**ForceDumpsDisabled**機碼下方建立 REG_DWORD 屬性做為 GuardedHost，並再將其值設定為 0x10000000。
3. 適用於每個叢集節點的新的登錄機碼。

>[!NOTE]
>您必須重新啟動電腦，nregistry 變更，才會生效。

此登錄機碼設定的即時之後傾印建立將會失敗並產生 「 STATUS_NOT_SUPPORTED 」 錯誤。

#### 方法 2
根據預設，Windows 錯誤報告可讓每個每 7 天的報告類型只有一個 LiveDump 和只有 1 LiveDump 每部電腦每 5 天。 您可以藉由設定下列登錄機碼只永遠在電腦上允許一個 LiveDump 變更。
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v SystemThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```
```
reg add "HKLM\Software\Microsoft\Windows\Windows Error Reporting\FullLiveKernelReports" /v ComponentThrottleThreshold /t REG_DWORD /d 0xFFFFFFFF /f
```

*注意：* 您必須重新啟動電腦，變更才會生效。

### 方法 3
若要停用叢集產生的實際的傾印 （例如當會記錄事件 5120），執行下列 cmdlet:

```powershell
(Get-Cluster).DumpPolicy = ((Get-Cluster).DumpPolicy -band 0xFFFFFFFFFFFFFFFE)
```
這個 cmdlet 有不需要重新啟動電腦的所有叢集節點上立即生效。
    
## IO 效能變慢

如果您看到 IO 效能變慢，檢查快取是否已啟用在您儲存空間直接存取的設定。 

有兩種方式，來檢查： 
     
 
1. 使用叢集記錄檔。 在選擇的文字編輯器中開啟叢集記錄檔，然後搜尋 「 [=== SBL 磁碟 ===]。 」 這會是在磁碟產生記錄檔的節點上的清單。 

     快取 Enabled 磁碟範例： 請注意此處的狀態是 CacheDiskStateInitializedAndBound，並有以下是存在的 GUID。 

   ```
   [=== SBL Disks ===]
    {26e2e40f-a243-1196-49e3-8522f987df76},3,false,true,1,48,{1ff348f1-d10d-7a1a-d781-4734f4440481},CacheDiskStateInitializedAndBound,1,8087,54,false,false,HGST    ,HUH721010AL4200 ,        7PG3N2ER,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    快取未啟用： 我們可以看到不存在的 GUID，而且狀態 CacheDiskStateNonHybrid。 
    ```
   [=== SBL Disks ===]
    {426f7f04-e975-fc9d-28fd-72a32f811b7d},12,false,true,1,24,{00000000-0000-0000-0000-000000000000},CacheDiskStateNonHybrid,0,0,0,false,false,HGST    ,HUH721010AL4200 ,        7PGXXG6C,A21D,{d5e27a3b-42fb-410a-81c6-9d8cc12da20c},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0],
    ```

    快取未啟用： 當所有的磁碟都是相同的類型預設不會啟用案例。 這裡我們可以看到不存在的 GUID，而且狀態 CacheDiskStateIneligibleDataPartition。 
    ```
    {d543f90c-798b-d2fe-7f0a-cb226c77eeed},10,false,false,1,20,{00000000-0000-0000-0000-000000000000},CacheDiskStateIneligibleDataPartition,0,0,0,false,false,NVMe    ,INTEL SSDPE7KX02,  PHLF7330004V2P0LGN,0170,{79b4d631-976f-4c94-a783-df950389fd38},[R/M 0 R/U 0 R/T 0 W/M 0 W/U 0 W/T 0], 
    ```  
2. 使用 Get-PhysicalDisk.xml 從 SDDCDiagnosticInfo
    1. 開啟 XML 檔案使用 「 $d = 匯入 Clixml GetPhysicalDisk.XML 」
    2. 執行 「 ipmo 存放裝置 」
    3. 執行 「 $d 」。 請注意，使用量會自動選取，不日誌您會看到輸出，就像這樣： 

   |FriendlyName|  SerialNumber| MediaType| CanPool| OperationalStatus| HealthStatus| Usage| 大小|
   |-----------|------------|---------| -------| -----------------| ------------| -----| ----|
   |NVMe INTEL SSDPE7KX02| PHLF733000372P0LGN| SSD| False|   確定|                Healthy|      自動選取 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504008J2P0LGN| SSD|  False|    確定|                Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504005F2P0LGN| SSD|  False|  確定|                Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002A2P0LGN| SSD| False| 確定|    Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7504004T2P0LGN |SSD| False|確定|       Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7504002E2P0LGN| SSD| False| 確定|      Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330002Z2P0LGN| SSD| False| 確定|      Healthy|自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF733000272P0LGN |SSD| False| 確定|  Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02 |PHLF7330001J2P0LGN |SSD| False| 確定| Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF733000302P0LGN |SSD| False| 確定|Healthy| 自動選取| 1.82 TB|
   |NVMe INTEL SSDPE7KX02| PHLF7330004D2P0LGN |SSD| False| 確定| Healthy| 自動選取 |1.82 TB|
    
## 如何讓您可以再次使用相同的磁碟摧毀現有的叢集

在儲存空間直接存取叢集中，一旦您停用儲存空間直接存取，並使用[全新的磁碟機](deploy-storage-spaces-direct.md#step-31-clean-drives)中所述的清理程序，叢集的儲存集區仍會維持處於離線狀態，並從叢集移除健全狀況服務。

下一個步驟是要移除虛設儲存集區： 
   ```powershell
   Get-ClusterResource -Name "Cluster Pool 1" | Remove-ClusterResource
   ```

現在，如果您在任何節點上執行**Get-physicaldisk** ，您會看到已集區中的所有磁碟。 例如，在實驗室的 4 節點叢集使用 4 個 SAS 磁碟，100 GB 每個提供給每個節點。 在此情況下，儲存空間直接存取已停用，這會移除 SBL （存放匯流排層），但如果您執行**Get-physicaldisk**離開篩選器之後，它應該報告 4 個排除本機的作業系統磁碟機的磁碟。 而是它會將 16 報告改為。 這是相同的叢集中的所有節點。 當您執行**Get 磁碟**命令時，您會看到本機連接的磁碟編號以 0、 1、 2 等，如下列範例輸出中所示：

|數字| 易記名稱| 序號|HealthStatus|OperationalStatus|大小總計| 磁碟分割樣式|
|-|-|-|-|-|-|-|-|
|0|Msft Virtu...  ||Healthy | Online|  127 GB| GPT|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
|1|Msft Virtu...||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
|2|Msft Virtu...||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
|4|Msft Virtu...||Healthy| 離線| 100 GB| 原始|
|3|Msft Virtu...||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|
||Msft Virtu... ||Healthy| 離線| 100 GB| 原始|

    
## 「 不支援的媒體類型 」 相關的錯誤訊息當您建立使用 Enable-clusters2d 儲存空間直接存取叢集  

當您執行**Enable-clusters2d** cmdlet 時，您可能會看到類似這樣的錯誤：

![案例 6 錯誤訊息](media/troubleshooting/scenario-error-message.png)

若要修正此問題，請確定 HBA 模式中設定 HBA 介面卡。 沒有 HBA 應該 RAID 模式進行設定。  
    
## 在 '等候直到 SBL 磁碟呈現' 或 27%，Enable-clusterstoragespacesdirect 停止回應

您會看到驗證報告中的下列資訊：

    Disk <identifier> connected to node <nodename> returned a SCSI Port Association and the corresponding enclosure device could not be found. The hardware is not compatible with Storage Spaces Direct (S2D), contact the hardware vendor to verify support for SCSI Enclosure Services (SES). 
    
  
問題是與 HPE SAS 擴展器卡位於磁碟和 HBA 卡之間。 SAS 擴展器建立第一個磁碟機連接到擴展器，以及本身擴展器之間重複的識別碼。  這已在解析[HPE 智慧陣列控制器 SAS 擴展器韌體： 4.02](https://support.hpe.com/hpsc/swd/public/detail?sp4ts.oid=7304566&swItemId=MTX_ef8d0bf4006542e194854eea6a&swEnvOid=4184#tab3)。
    
## Intel SSD DC P4600 系列有非唯一的 NGUID
您可能會看到問題的 Intel SSD DC P4600 系列裝置似乎會報告類似 16 位元組 NGUID 例如 0100000001000000E4D25C000014E214 或 0100000001000000E4D25C0000EEE214 在以下範例中的多個命名空間。

|唯一識別碼| deviceid |MediaType| BusType| serialnumber| size|canpool| friendlyname| OperationalStatus|
|-|-|-|-|-|-|-|-|-
|5000CCA251D12E30| 0| HDD| SAS| 7PKR197G|                  10000831348736 |False|HGST| HUH721010AL4200| 確定|
|eui.0100000001000000E4D25C000014E214 |4|SSD| NVMe|   0100_0000_0100_0000_E4D2_5C00_0014_E214。|1600321314816|True| INTEL| SSDPE2KE016T7|  確定|
|eui.0100000001000000E4D25C000014E214 |5|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_0014_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  確定|
|eui.0100000001000000E4D25C0000EEE214| 6|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  確定|
|eui.0100000001000000E4D25C0000EEE214| 7|        SSD|       NVMe|    0100_0000_0100_0000_E4D2_5C00_00EE_E214。|  1600321314816|    True| INTEL| SSDPE2KE016T7|  確定|

若要修正此問題，更新韌體的 Intel 磁碟機上的最新版本。  韌體版本從 2018 年 QDV101B1 已知解決這個問題。

[2018 年發行的 Intel SSD 資料中心工具](https://downloadmirror.intel.com/27778/eng/Intel_SSD_Data_Center_Tool_3_0_12_Release_Notes_330715-026.pdf)包含的韌體更新，QDV101B1，Intel SSD DC P4600 系列。

    
## 實體磁碟 」 狀況良好 」，且 [操作狀態是 「 移除從集區 」 

在 Windows Server 2016 儲存空間直接存取叢集中，您可能會看到一或多個實體磁碟 Healthstatus\ 做為 「 良好 」 OperationalStatus 時 」 （移除從集區，確定）。 」 

「 從集區中移除 「 意圖時設定**移除 PhysicalDisk**是稱為但儲存在健康情況來維護狀態，並允許復原，如果移除作業失敗。 您可以手動變更 OperationalStatus 以狀況良好的下列方法的其中一個：

- 移除實體磁碟從集區，然後再加回。
- 執行的[清除 PhysicalDiskHealthData.ps1 指令碼](https://go.microsoft.com/fwlink/?linkid=2034205)來清除意圖。 (適用於做為下載。TXT 檔案。 您將需要將它做為儲存。PS1 檔案之前，您可以執行它。）

以下是一些範例顯示如何執行指令碼：

- 您可以使用**SerialNumber**參數來指定您需要將設為 [狀況良好的磁碟。 您可以從**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**取得序號。 （我們只使用 0 的下方序號。）
   
   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -SerialNumber 000000000000000 -Verbose -Force
    ```

- 使用**唯一識別碼**參數來指定磁碟 （一次從**WMI MSFT_PhysicalDisk**或**Get-physicaldisk**）。

   ```powershell
   Clear-PhysicalDiskHealthData -Intent -Policy -UniqueId 00000000000000000 -Verbose -Force
   ```

## 檔案複製變慢

您可能會看到問題，將大型的 VHD 複製到虛擬磁碟使用檔案總管 \]-檔案複製花費的時間比預期。

使用 \ [檔案總管，Robocopy 或 Xcopy 將大型的 VHD 複製到虛擬磁碟不建議的方法，以在較慢比預期的效能，這會造成。 複製程序不會經過儲存空間直接存取堆疊的較低位於儲存堆疊，並改為就像是本機複製程序。

如果您想要測試儲存空間直接存取的效能，建議使用 VMFleet 和 Diskspd 來載入和壓力測試的伺服器，以取得基準線和設定的儲存空間直接存取的效能的期望。

## 預期的其餘節點看到的節點重新開機期間的事件。 

它是安全地忽略這些事件： 

    Event ID 205: Windows lost communication with physical disk {XXXXXXXXXXXXXXXXXXXX }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

    Event ID 203: Windows lost communication with physical disk {xxxxxxxxxxxxxxxxxxxxxxxx }. This can occur if a cable failed or was disconnected, or if the disk itself failed. 

如果您正在執行 Azure Vm，您可以略過此事件：

    Event ID 32: The driver detected that the device \Device\Harddisk5\DR5 has its write cache enabled. Data corruption may occur. 
    
## 效能變慢或 「 通訊，「 「 IO 錯誤 」，「 分離 」 或 「 否備援 」 錯誤使用 Intel P3x00 NVMe 裝置的部署

我們已識別出會影響使用硬體 Intel P3x00 的 NVM Express (NVMe) 與 「 維護發行 8。 」 之前的韌體版本的裝置系列為基礎的一些儲存空間直接存取使用者的重要問題 

>[!NOTE]
> 個別的 Oem 可能會有裝置為基礎的 Intel P3x00 裝置系列的 NVMe 使用唯一的韌體版本的字串。 如需詳細資訊的最新的韌體版本，請連絡您的 OEM。

如果您使用硬體您 Intel P3x00 的 NVMe 裝置系列為基礎的部署中，我們建議您立即套用可用的最新韌體 (至少維護版本 8)。 這個[Microsoft 支援服務文章](https://support.microsoft.com/en-us/help/4052341/slow-performance-or-lost-communication-io-error-detached-or-no-redunda)提供有關此問題的額外資訊。 
