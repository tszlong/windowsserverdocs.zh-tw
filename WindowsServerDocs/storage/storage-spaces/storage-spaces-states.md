---
title: 儲存空間和儲存空間直接存取的健全狀況和操作狀態
description: 如何尋找及瞭解儲存空間直接存取和儲存空間的不同健全狀況和操作狀態（包括實體磁片、集區和虛擬磁片），以及如何處理這些情況。
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
ms.prod: windows-server
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 83489cb7a8a44de13b5ba245d7ce1cb5ceabc08e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80820971"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>針對儲存空間和儲存空間直接存取的健全狀況和操作狀態進行疑難排解

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server （半年通道）、Windows 10 Windows 8。1

本主題描述存放集區的健全狀況和操作狀態、虛擬磁片（位於儲存空間的磁片區底下），以及磁片磁碟機[儲存空間直接存取](storage-spaces-direct-overview.md)和[儲存空間](overview.md)。 當您嘗試針對各種問題進行疑難排解（例如，因為唯讀設定而無法刪除虛擬磁片）時，這些狀態可能很有價值。 它也會討論為什麼無法將磁片磁碟機新增至集區（CannotPoolReason）。

儲存空間有三個主要物件-新增至*存放集區*的*實體磁片*（硬碟、ssd 等）、虛擬化存放裝置，讓您可以從集區中的可用空間建立*虛擬磁片*，如下所示。 集區中繼資料會寫入集區中的每個磁片磁碟機。 系統會在虛擬磁片上建立磁片區並儲存檔案，但我們不會在這裡討論磁片區。

![實體磁片會新增至儲存集區，然後從集區空間建立虛擬磁片](media/storage-spaces-states/storage-spaces-object-model.png)

您可以在伺服器管理員中，或使用 PowerShell 來查看健康情況和操作狀態。 以下是在儲存空間直接存取叢集上的各種（大多是不良）健康情況和操作狀態的範例，其中遺漏了大部分的叢集節點（以滑鼠右鍵按一下資料行標頭來新增**操作狀態**）。 這不是很高興的叢集。

![伺服器管理員顯示儲存空間直接存取叢集中兩個遺失節點的結果-有許多遺失的實體磁片和虛擬磁片處於狀況不良狀態](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>存放集區狀態

每個存放集區的健康狀態都是「狀況**良好**」、「**警告**」或「**不明**」，/狀況**不良**，以及一或多個操作狀態。

若要找出集區的狀態，請使用下列 PowerShell 命令：

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

以下範例輸出顯示處於不明健全狀況狀態且具有唯讀操作狀態的儲存集區：

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

下列各節列出健全狀況和操作狀態。

### <a name="pool-health-state-healthy"></a>集區健全狀況狀態：狀況良好

| 操作狀態    | 描述 |
| ---------            | ---------  |
| 確定 | 存放集區狀況良好。 |

### <a name="pool-health-state-warning"></a>集區健全狀況狀態：警告

當存放集區處於**警告**健全狀況狀態時，表示集區可供存取，但有一或多個磁片磁碟機失敗或遺失。 因此，您的存放集區可能會有較低的復原能力。

|操作狀態    |描述|
|---------            |---------  |
|衰退|存放集區中的磁片磁碟機失敗或遺失。 只有在裝載集區中繼資料的磁片磁碟機上才會發生此狀況。 <br><br>**動作**：檢查磁片磁碟機的狀態，並更換任何故障的磁片磁碟機，然後再發生其他失敗。|

### <a name="pool-health-state-unknown-or-unhealthy"></a>集區健全狀況狀態：不明或狀況不良

當存放集區處於 [**不明**] 或 [狀況**不良**] 狀態時，表示存放集區是唯讀的，而且必須等到集區回到 [**警告** **] 或 [確定]** 健全狀態之後，才能加以修改。

|操作狀態    |唯讀原因 |描述|
|---------            |---------       |--------   |
|唯讀|未完成|如果存放集區遺失[仲裁](understand-quorum.md)，就會發生這種情況，這表示集區中的大部分磁片磁碟機都已失敗或因某種原因而離線。 當集區失去仲裁時，儲存空間會自動將集區設定設為唯讀，直到有足夠的磁片磁碟機可供使用為止。<br><br>**即席** <br>1. 重新連接任何遺失的磁片磁碟機，如果您使用儲存空間直接存取，請讓所有伺服器上線。 <br>2. 開啟具有系統管理許可權的 PowerShell 會話，然後輸入下列命令，將集區設定回讀寫：<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||Policy(Windows Intune 說明：原則)|系統管理員將存放集區設定為唯讀。<br><br>**動作：** 若要在容錯移轉叢集管理員中將叢集儲存集區設定為讀寫存取，**請移**至 [集區]，以滑鼠右鍵按一下集區，然後選取 [**上線**]。<br><br>若是其他伺服器和電腦，請以系統管理許可權開啟 PowerShell 會話，然後輸入：<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|儲存空間正在啟動，或正在等候集區中的磁片磁碟機連線。 這應該是暫時性的狀態。 一旦完全啟動，集區應該轉換成不同的操作狀態。<br><br>**動作：** 如果集區維持在 [*啟動*中] 狀態，請確定集區中的所有磁片磁碟機都已正確連接。|

另請參閱[修改具有唯讀設定的存放集區](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx)。

## <a name="virtual-disk-states"></a>虛擬磁片狀態

在儲存空間中，磁片區會放在集區中劃分可用空間外的虛擬磁片（儲存空間）上。 每個虛擬磁片的健康狀態都是 [狀況**良好**]、[**警告**]、[狀況**不良**] 或 [**未知**]，以及一或多個操作狀態。

若要找出虛擬磁片的狀態，請使用下列 PowerShell 命令：

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

以下是輸出範例，其中顯示已卸離的虛擬磁片和降級/不完整的虛擬磁片：

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

下列各節列出健全狀況和操作狀態。

### <a name="virtual-disk-health-state-healthy"></a>虛擬磁片健全狀況狀態：狀況良好

|操作狀態    |描述|
|---------            |---------          |
|確定    |虛擬磁片狀況良好。|
|不夠    |資料不會在磁片磁碟機上平均寫入。 <br><br>**動作**：執行[StoragePool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) Cmdlet，以優化存放集區中的磁片磁碟機使用量。|

### <a name="virtual-disk-health-state-warning"></a>虛擬磁片健全狀況狀態：警告

當虛擬磁片處於**警告**健全狀況狀態時，表示您的一或多個資料複本無法使用，但儲存空間仍然可以讀取至少一個資料複本。

|操作狀態    |描述|
|---------            |---------          |
|服務中            |Windows 正在修復虛擬磁片，例如新增或移除磁片磁碟機之後。 當修復完成時，虛擬磁片應該會回到 [確定] 健全狀況狀態。|
|未完成           |因為一或多個磁片磁碟機故障或遺失，虛擬磁片的復原能力會降低。 不過，遺失的磁片磁碟機包含最新的資料複本。<br><br> **動作**： <br>1. 重新連接任何遺失的磁片磁碟機，更換任何故障的磁片磁碟機，如果您是使用儲存空間直接存取，請將任何離線的伺服器上線。 <br>2. 如果您不是使用儲存空間直接存取，請使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。<br> 儲存空間直接存取在重新連接或更換磁片磁碟機之後，視需要自動啟動修復。|
|衰退             |虛擬磁片的恢復功能已減少，因為有一或多個磁片磁碟機故障或遺失，而且這些磁片磁碟機上有過時的資料複本。 <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機，更換任何故障的磁片磁碟機，如果您是使用儲存空間直接存取，請將任何離線的伺服器上線。 <br> 2. 如果您不是使用儲存空間直接存取，請使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。 <br>儲存空間直接存取在重新連接或更換磁片磁碟機之後，視需要自動啟動修復。|

### <a name="virtual-disk-health-state-unhealthy"></a>虛擬磁片健全狀況狀態：狀況不良

當虛擬磁片處於狀況**不良**的健全狀態時，目前無法存取虛擬磁片上的部分或所有資料。

|操作狀態    |描述|
|---------            |--------   |
|無冗余|虛擬磁片已遺失資料，因為有太多磁片磁碟機失敗。<br><br>**動作**：更換失敗的磁片磁碟機，然後從備份還原您的資料。|

### <a name="virtual-disk-health-state-informationunknown"></a>虛擬磁片健全狀況狀態：資訊/未知

如果系統管理員將虛擬磁片設為離線或虛擬磁片已中斷連結，則虛擬磁片也可以是**資訊**健全狀態（如 [儲存空間] 控制台專案中所示）或**不明**的健全狀況狀態（如 PowerShell 中所示）。

|操作狀態    |卸離原因 |描述|
|---------            |---------       |--------   |
|已中斷連結             |依原則            |系統管理員將虛擬磁片離線，或將虛擬磁片設定為需要手動附件，在這種情況下，您必須在每次 Windows 重新開機時手動連接虛擬磁片。<br><br>**動作**：讓虛擬磁片恢復上線。 若要在虛擬磁片位於叢集存放集區時執行這項操作，請在容錯移轉叢集管理員選取 [儲存體] **[ > 集** **區**] > [**虛擬磁片**]，選取顯示**離線**狀態的虛擬磁片，然後選取 [**上線**]。 <br><br>若要讓虛擬磁片在不在叢集中時恢復上線，請以系統管理員身分開啟 PowerShell 會話，然後嘗試使用下列命令：<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>若要在 Windows 重新開機之後自動附加所有非叢集虛擬磁片，請以系統管理員身分開啟 PowerShell 會話，然後使用下列命令：<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |多數磁片狀況不良 |此虛擬磁片使用的磁片磁碟機過多失敗、遺失或有過時的資料。   <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機，如果您是使用儲存空間直接存取，請將任何離線的伺服器上線。 <br> 2. 在所有磁片磁碟機和伺服器都上線之後，更換任何故障的磁片磁碟機。 如需詳細資訊，請參閱[健全狀況服務](../../failover-clustering/health-service-overview.md)。 <br>儲存空間直接存取在重新連接或更換磁片磁碟機之後，視需要自動啟動修復。<br>3. 如果您不是使用儲存空間直接存取，請使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。  <br><br>如果有更多的磁片失敗，但您有資料的複本，而虛擬磁片未在失敗之間修復，則虛擬磁片上的所有資料都會永久遺失。 在這種不幸的情況下，請刪除虛擬磁片、建立新的虛擬磁片，然後從備份還原。|
|                     |未完成 |磁片磁碟機不足，無法讀取虛擬磁片。    <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機，如果您是使用儲存空間直接存取，請將任何離線的伺服器上線。 <br> 2. 在所有磁片磁碟機和伺服器都上線之後，更換任何故障的磁片磁碟機。 如需詳細資訊，請參閱[健全狀況服務](../../failover-clustering/health-service-overview.md)。 <br>儲存空間直接存取在重新連接或更換磁片磁碟機之後，視需要自動啟動修復。<br>3. 如果您不是使用儲存空間直接存取，請使用[VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。<br><br>如果有更多的磁片失敗，但您有資料的複本，而虛擬磁片未在失敗之間修復，則虛擬磁片上的所有資料都會永久遺失。 在這種不幸的情況下，請刪除虛擬磁片、建立新的虛擬磁片，然後從備份還原。|
| |逾時|連接虛擬磁片所花費的時間太長 <br><br> **動作：** 這不應該經常發生，因此您可能會嘗試查看條件是否傳入。 或者，您可以嘗試使用[Disconnect-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) Cmdlet 中斷虛擬磁片的連線，然後使用[VirtualDisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) Cmdlet 來重新連接它。 |

## <a name="drive-physical-disk-states"></a>磁片磁碟機（實體磁片）狀態

下列各節說明磁片磁碟機可能所在的健全狀況狀態。 集區中的磁片磁碟機會在 PowerShell 中以*實體磁片*物件的形式呈現。

### <a name="drive-health-state-healthy"></a>磁片磁碟機健全狀態：狀況良好

|操作狀態    |描述|
|---------            |---------          |
|確定|磁片磁碟機狀況良好。|
|服務中|磁片磁碟機正在執行一些內部的維護作業。 當動作完成時，磁片磁碟機應該會回到 [*確定]* 健全狀況狀態。|

### <a name="drive-health-state-warning"></a>磁片磁碟機健全狀況狀態：警告

處於警告狀態的磁片磁碟機可以成功讀取及寫入資料，但有問題。

|操作狀態    |描述|
|---------            |---------          |
|遺失通訊|磁片磁碟機遺失。 如果您是使用儲存空間直接存取，這可能是因為伺服器已關閉。<br><br>**動作**：如果您使用儲存空間直接存取，請讓所有伺服器恢復上線。 如果無法修正此問題，請重新連接磁片磁碟機，加以取代，或依照使用 Windows 錯誤報告 >[實體磁片超時](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out)中的步驟，嘗試取得關於此磁片磁碟機的詳細診斷資訊。|
|正在從集區移除|儲存空間正在從其存放集區移除磁片磁碟機。 <br><br> 這是暫時性的狀態。 移除完成後，如果磁片磁碟機仍連接到系統，則磁片磁碟機會轉換成原創組區中的另一個操作狀態（通常是正常的）。|
|啟動維護模式|當系統管理員將磁片磁碟機置於維護模式後，儲存空間正在將磁片磁碟機置於維護模式。 這是暫時性的狀態-磁片磁碟機應該很快就會處於*維護模式*狀態。|
|處於維護模式|系統管理員將磁片磁碟機置於維護模式，以停止磁片磁碟機的讀取和寫入。 這通常是在更新磁片磁碟機固件之前，或在測試失敗時完成。<br><br>**動作**：若要讓磁片磁碟機離開維護模式，請使用[StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) Cmdlet。|
|停止維護模式|系統管理員將磁片磁碟機移出維護模式，而儲存空間正在將磁片磁碟機重新上線。 這是暫時性的狀態-磁片磁碟機應該很快就會處於另一個狀態-理想*狀況良好*。|
|預測性失敗|磁片磁碟機回報其接近失敗。<br><br>**動作**：更換磁片磁碟機。|
|IO 錯誤|存取磁片磁碟機時發生暫時性錯誤。<br><br>**動作**： <br>1. 如果磁片磁碟機未轉換回 [**確定]** 狀態，您可以嘗試使用[PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) Cmdlet 抹除磁片磁碟機。 <br> 2. 使用 [[修復 VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) ] 來還原受影響虛擬磁片的復原。 <br>3. 如果這種情況持續發生，請更換磁片磁碟機。|
|暫時性錯誤|磁片磁碟機發生暫時性錯誤。 這通常表示磁片磁碟機沒有回應，但也可能表示儲存空間保護磁碟分割已不適當地從磁片磁碟機移除。 <br><br>**動作**： <br>1. 如果磁片磁碟機未轉換回 [**確定]** 狀態，您可以嘗試使用[PhysicalDisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) Cmdlet 抹除磁片磁碟機。 <br> 2. 使用 [[修復 VirtualDisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk) ] 來還原受影響虛擬磁片的復原。 <br>3. 如果這種情況持續發生，請更換磁片磁碟機，或嘗試遵循使用 Windows 錯誤報告 >[實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步驟來取得有關此磁片磁碟機的詳細診斷資訊。|
|異常延遲|磁片磁碟機的執行速度很慢，儲存空間直接存取中的健全狀況服務來測量。<br><br>**動作**：如果這種情況持續發生，請更換磁片磁碟機，讓它不會降低整體儲存空間的效能。

### <a name="drive-health-state-unhealthy"></a>磁片磁碟機健全狀態：狀況不良

目前無法寫入或存取處於狀況不良狀態的磁片磁碟機。

|操作狀態    |描述|
|---------            |---------          |
|無法使用|儲存空間無法使用此磁片磁碟機。 如需詳細資訊，請參閱[儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md);如果您不是使用儲存空間直接存取，請參閱[儲存空間總覽](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements)。|
|分割|磁片磁碟機已與集區隔開。<br><br>**動作**：重設磁片磁碟機，清除磁片磁碟機中的所有資料，並將其新增回集區做為空的磁片磁碟機。 若要這麼做，請以系統管理員身分開啟 PowerShell 會話，執行[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)指令程式，然後執行 [[修復 VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)]。 <br><br>若要取得有關此磁片磁碟機的詳細診斷資訊，請遵循使用 Windows 錯誤報告 >[實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步驟進行疑難排解。|
|過時的中繼資料|儲存空間在磁片磁碟機上找到舊的中繼資料。<br><br>**動作**：這應該是暫時性的狀態。 如果磁片磁碟機未轉換回 [確定]，您可以執行 [[修復 VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) ]，以在受影響的虛擬磁片上啟動修復操作。 如果這樣做無法解決問題，您可以使用[PhysicalDisk 指令程式](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)來重設磁片磁碟機、抹除磁片磁碟機中的所有資料，然後執行[修復 VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|無法辨識的中繼資料|儲存空間在磁片磁碟機上找到無法辨識的中繼資料，這通常表示磁片磁碟機具有來自其上不同集區的中繼資料。<br><br>**動作**：若要抹除磁片磁碟機，並將它新增至目前的集區，請重設磁片磁碟機。 若要重設磁片磁碟機，請以系統管理員身分開啟 PowerShell 會話，執行[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)指令程式，然後執行 [[修復 VirtualDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)]。|
|失敗的媒體|磁片磁碟機失敗，儲存空間將不會再使用。<br><br>**動作**：更換磁片磁碟機。 <br><br>若要取得有關此磁片磁碟機的詳細診斷資訊，請遵循使用 Windows 錯誤報告 >[實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)中的步驟進行疑難排解。|
|裝置硬體故障|此磁片磁碟機上發生硬體故障。 <br><br>**動作**：更換磁片磁碟機。|
|更新韌體|Windows 正在更新磁片磁碟機上的固件。 這是暫時性的狀態，通常不到一分鐘的時間，而在此期間，集區中的其他磁片磁碟機會處理所有的讀取和寫入。 如需詳細資訊，請參閱[更新磁片磁碟機固件](../update-firmware.md)。|
|Starting|磁片磁碟機正在準備進行操作。 這應該是暫時性的狀態-完成後，磁片磁碟機應該轉換成不同的操作狀態。|

## <a name="reasons-a-drive-cant-be-pooled"></a>磁片磁碟機無法集區的原因

有些磁片磁碟機尚未準備好存放集區。 您可以查看實體磁片的 `CannotPoolReason` 屬性，找出磁片磁碟機不適合進行共用的原因。 以下是顯示 CannotPoolReason 屬性的範例 PowerShell 腳本：

```PowerShell
Get-PhysicalDisk | Format-Table FriendlyName,MediaType,Size,CanPool,CannotPoolReason
```

以下是範例輸出：

```
FriendlyName          MediaType          Size CanPool CannotPoolReason
------------          ---------          ---- ------- ----------------
ATA MZ7LM120HCFD00D3  SSD        120034123776   False Insufficient Capacity
Msft Virtual Disk     SSD         10737418240    True
Generic Physical Disk SSD        119990648832   False In a Pool
```

下表提供有關每個原因的更多詳細資料。

|原因|描述|
|---|---|
|在集區中|磁片磁碟機已屬於存放集區。 <br><br>磁片磁碟機一次只能屬於單一存放集區。 若要在另一個存放集區中使用此磁片磁碟機，請先從現有的集區中移除該磁片磁碟機，這會指示儲存空間將磁片磁碟機上的資料移至集區上的其他磁片磁碟機。 或者，如果磁片磁碟機已從其集區中斷連線，而未通知儲存空間，則重設磁片磁碟機。 <br><br>若要安全地從存放集區移除磁片磁碟機，請使用[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)，或移至伺服器管理員 > 檔案**和存放服務** > **存放集區**，>**實體磁片**]，在磁片磁碟機上按一下滑鼠右鍵，然後選取 [**移除磁片**。<br><br>若要重設磁片磁碟機，請使用[PhysicalDisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)。|
|狀況不良|磁片磁碟機不是處於狀況良好的狀態，可能需要更換。|
|卸除式媒體|磁片磁碟機會分類為卸載式磁片磁碟機。 <br><br>儲存空間不支援 Windows 以可移動方式辨識的媒體，例如藍光磁片磁碟機。 雖然許多固定磁片磁碟機都在卸載式位置中，但在一般情況下，由 Windows*分類*為卸載式的媒體並不適合搭配儲存空間使用。|
|由叢集使用|容錯移轉叢集目前正在使用此磁片磁碟機。|
|離線|磁片磁碟機已離線。 <br><br>若要讓所有離線磁片磁碟機上線，並將其設定為讀取/寫入，請以系統管理員身分開啟 PowerShell 會話，並使用下列腳本：<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|容量不足|當磁碟分割佔用磁片磁碟機上的可用空間時，通常就會發生這種情況。 <br><br>**動作**：刪除磁片磁碟機上的任何磁片區，清除磁片磁碟機上的所有資料。 其中一種方法是使用[純磁片](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps)PowerShell Cmdlet。|
|確認進行中|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)正在檢查磁片磁碟機上的磁片磁碟機或固件是否已核准供伺服器系統管理員使用。|
|驗證失敗|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)無法檢查磁片磁碟機上的磁片磁碟機或固件是否已核准供伺服器系統管理員使用。|
|固件不相容|實體磁片磁碟機上的固件不在伺服器管理員使用[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)所指定的已核准固件修訂清單中。 |
|硬體不符合規範|磁片磁碟機不在伺服器管理員使用[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)所指定的已核准儲存模型清單中。|

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取](storage-spaces-direct-overview.md)
- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [了解叢集和集區仲裁](understand-quorum.md)