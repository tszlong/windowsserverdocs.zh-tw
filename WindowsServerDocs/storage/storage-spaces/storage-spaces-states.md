---
title: 儲存空間和儲存空間直接存取健康情況和操作狀態
description: 如何尋找及瞭解儲存空間直接存取和儲存空間的不同健康情況和操作狀態 (包括實體磁片、集區和虛擬磁片) ，以及該怎麼做。
author: jasongerend
ms.author: jgerend
ms.date: 12/06/2019
ms.topic: article
manager: brianlic
ms.openlocfilehash: 02e7f1064553e724ae5c246af06385b51fd7e1d3
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96864707"
---
# <a name="troubleshoot-storage-spaces-and-storage-spaces-direct-health-and-operational-states"></a>針對儲存空間和儲存空間直接存取健康情況和操作狀態進行疑難排解

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server (半年通道) 、Windows 10 Windows 8。1

本主題說明儲存集區、虛擬磁片 (位於儲存空間) 的磁片區，以及 [儲存空間直接存取](storage-spaces-direct-overview.md) 和 [儲存空間](overview.md)中磁片磁碟機的健全狀況和操作狀態。 當您嘗試針對各種問題進行疑難排解，例如因為唯讀設定而無法刪除虛擬磁片時，這些狀態可能很有用。 它也會討論為什麼無法將磁片磁碟機新增至 CannotPoolReason)  (集區。

儲存空間有三個主要物件-新增至 *存放集區* 的 *實體磁片* (硬碟、ssd 等 ) 、虛擬化儲存體，讓您可以從集區中的可用空間建立 *虛擬磁片*，如下所示。 集區中繼資料會寫入集區中的每個磁片磁碟機。 磁片區會在虛擬磁片上建立並儲存檔案，但我們不會在這裡討論磁片區。

![實體磁片會新增至存放集區，然後是從集區空間建立的虛擬磁片](media/storage-spaces-states/storage-spaces-object-model.png)

您可以在伺服器管理員或使用 PowerShell 來查看健康情況和操作狀態。 以下是各種 (的範例，在儲存空間直接存取叢集上的) 健康情況和操作狀態，大部分都缺少其叢集節點 (以滑鼠右鍵按一下資料行標頭，以新增 **操作狀態**) 。 這並不是很高興的叢集。

![伺服器管理員顯示儲存空間直接存取叢集中兩個遺失節點的結果-有許多遺失的實體磁片和虛擬磁片處於狀況不良的狀態](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>存放集區狀態

每個存放集區都有健全狀況狀態-狀況 **良好**、**警告** 或 **不明** 狀況 / **不良**，以及一或多個操作狀態。

若要找出集區的狀態，請使用下列 PowerShell 命令：

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

以下是範例輸出，顯示處於未知健康狀態且具有唯讀操作狀態的存放集區：

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

以下各節列出健全狀態和操作狀態。

### <a name="pool-health-state-healthy"></a>集區健全狀況狀態：狀況良好

| 操作狀態    | 描述 |
| ---------            | ---------  |
| [確定] | 儲存集區的狀況良好。 |

### <a name="pool-health-state-warning"></a>集區健全狀況狀態：警告

當存放集區處於 **警告** 健全狀況狀態時，表示集區可供存取，但有一或多個磁片磁碟機失敗或遺失。 因此，您的存放集區可能會降低復原能力。

|操作狀態    |描述|
|---------            |---------  |
|已降級|存放集區中的磁片磁碟機失敗或遺失。 只有裝載集區中繼資料的磁片磁碟機才會發生這種情況。 <br><br>**動作**：檢查磁片磁碟機的狀態，並更換任何故障的磁片磁碟機，然後再發生其他失敗。|

### <a name="pool-health-state-unknown-or-unhealthy"></a>集區健全狀況狀態：未知或狀況不良

當存放集區處於 **未知** 或狀況 **不良** 的健康狀態時，表示儲存集區是唯讀的，而且必須等到集區恢復為 **警告** 或 **正常** 健康狀態時，才能修改存放集區。

|操作狀態    |唯讀原因 |描述|
|---------            |---------       |--------   |
|唯讀|不完整|如果儲存集區失去其 [仲裁](understand-quorum.md)，就會發生這種情況，這表示集區中的大多數磁片磁碟機因某些原因而失敗或已離線。 當集區遺失仲裁時，儲存空間會自動將集區設定設為唯讀，直到有足夠的磁片磁碟機再次可用為止。<br><br>**動作：** <br>1. 重新連接任何遺失的磁片磁碟機，如果您使用儲存空間直接存取，請讓所有伺服器上線。 <br>2. 以系統管理許可權開啟 PowerShell 會話，然後輸入下列命令，將集區重新設定為讀寫：<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||原則|系統管理員將存放集區設定為唯讀。<br><br>**動作：** 若要在容錯移轉叢集管理員中將叢集儲存集區設定為讀寫存取， **請移** 至 [集區]，以滑鼠右鍵按一下集區，然後選取 [ **上線**]。<br><br>若是其他伺服器和電腦，請以系統管理許可權開啟 PowerShell 會話，然後輸入：<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||啟動中|儲存空間正在啟動或等候磁片區連線到集區。 這應該是暫時性的狀態。 一旦完全啟動，集區應該會轉換成不同的操作狀態。<br><br>**動作：** 如果集區處於 *啟動* 狀態，請確定已正確連接集區中的所有磁片磁碟機。|

另請參閱 [Windows Server 存放裝置論壇](/answers/topics/windows-server-storage.html)。

## <a name="virtual-disk-states"></a>虛擬磁片狀態

在 [儲存空間] 中，磁片區會放在虛擬磁片上， (儲存空間) 劃分在集區中的可用空間之外。 每個虛擬磁片都有健全狀況狀態-狀況 **良好**、 **警告**、 **狀況不良** 或 **不明** ，以及一或多個操作狀態。

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

以下各節列出健全狀態和操作狀態。

### <a name="virtual-disk-health-state-healthy"></a>虛擬磁片健全狀況狀態：狀況良好

|操作狀態    |描述|
|---------            |---------          |
|[確定]    |虛擬磁片的狀況良好。|
|次佳    |資料未平均寫入各個磁碟機。 <br><br>**動作**：執行 [StoragePool](/powershell/module/storage/optimize-storagepool) Cmdlet，以優化存放集區中的磁片磁碟機使用量。|

### <a name="virtual-disk-health-state-warning"></a>虛擬磁片健全狀況狀態：警告

當虛擬磁片處於 **警告** 健康狀態時，表示您的資料有一或多個複本無法使用，但儲存空間仍可讀取至少一個資料複本。

|操作狀態    |描述|
|---------            |---------          |
|運作中            |Windows 正在修復虛擬磁片，例如新增或移除磁片磁碟機。 修復完成時，虛擬磁片應該會返回「確定」健全狀態。|
|不完整           |因為有一或多個磁片磁碟機故障或遺失，所以可減少虛擬磁片的復原能力。 不過，遺失的磁碟機包含您的資料最新的複本。<br><br> **動作**： <br>1. 重新連接任何遺失的磁片磁碟機、更換任何故障的磁片磁碟機，如果您使用儲存空間直接存取，請將任何離線的伺服器帶到線上。 <br>2. 如果您不是使用儲存空間直接存取，接下來請使用 [VirtualDisk](/powershell/module/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。<br> 重新連接或更換磁片磁碟機之後，儲存空間直接存取會自動啟動修復。|
|已降級             |因為有一或多個磁片磁碟機故障或遺失，且這些磁片磁碟機上有已過期的資料複本，所以可減少虛擬磁片的復原能力。 <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機、更換任何故障的磁片磁碟機，如果您使用儲存空間直接存取，請將任何離線的伺服器帶到線上。 <br> 2. 如果您不是使用儲存空間直接存取，接下來請使用 [VirtualDisk](/powershell/module/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。 <br>重新連接或更換磁片磁碟機之後，儲存空間直接存取會自動啟動修復。|

### <a name="virtual-disk-health-state-unhealthy"></a>虛擬磁片健全狀況狀態：狀況不良

當虛擬磁片處於狀況 **不良** 的健康狀態時，虛擬磁片上的部分或所有資料目前無法存取。

|操作狀態    |描述|
|---------            |--------   |
|無備援|虛擬磁片已遺失資料，因為有太多磁片磁碟機失敗。<br><br>**動作**：更換失敗的磁片磁碟機，然後從備份還原您的資料。|

### <a name="virtual-disk-health-state-informationunknown"></a>虛擬磁片健全狀況狀態：資訊/不明

如果系統管理員將虛擬磁片設為離線或虛擬磁片已中斷連結，則虛擬磁片也可以位於 **資訊** 健康情況狀態 (如 [儲存空間] 主控台專案) 或 [ **未知** 的健全狀況狀態] 中所示 (（如 PowerShell 所示）) 。

|操作狀態    |卸離原因 |描述|
|---------            |---------       |--------   |
|已卸離             |依原則            |系統管理員將虛擬磁片設為離線狀態，或將虛擬磁片設定為需要手動附件，在這種情況下，您必須在每次 Windows 重新開機時手動連接虛擬磁片。<br><br>**動作**：讓虛擬磁片恢復上線。 若要這樣做，當虛擬磁片位於叢集存放集區時，在容錯移轉叢集管理員選取 **存放集區**  >  **Pools**  >  **虛擬磁片**]，選取顯示 [**離線**] 狀態的虛擬磁片，然後選取 [**上線**]。 <br><br>若要在不在叢集中時讓虛擬磁片恢復上線，請以系統管理員身分開啟 PowerShell 會話，然後嘗試使用下列命令：<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>若要在 Windows 重新開機後自動附加所有非叢集虛擬磁片，請以系統管理員身分開啟 PowerShell 會話，然後使用下列命令：<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |多數磁片狀況不良 |此虛擬磁片使用太多磁片磁碟機失敗、遺失或有過時的資料。   <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機，如果您使用儲存空間直接存取，請將任何離線的伺服器帶到線上。 <br> 2. 在所有磁片磁碟機和伺服器都上線之後，更換任何故障的磁片磁碟機。 如需詳細資訊，請參閱 [健全狀況服務](../../failover-clustering/health-service-overview.md) 。 <br>重新連接或更換磁片磁碟機之後，儲存空間直接存取會自動啟動修復。<br>3. 如果您不是使用儲存空間直接存取，接下來請使用 [VirtualDisk](/powershell/module/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。  <br><br>如果磁片數目超過您的資料複本，而且虛擬磁片未在失敗期間修復，則虛擬磁片上的所有資料都會永久遺失。 在這種情況下，請刪除虛擬磁片、建立新的虛擬磁片，然後從備份還原。|
|                     |不完整 |沒有足夠的磁片磁碟機可讀取虛擬磁片。    <br><br>**動作**： <br> 1. 重新連接任何遺失的磁片磁碟機，如果您使用儲存空間直接存取，請將任何離線的伺服器帶到線上。 <br> 2. 在所有磁片磁碟機和伺服器都上線之後，更換任何故障的磁片磁碟機。 如需詳細資訊，請參閱 [健全狀況服務](../../failover-clustering/health-service-overview.md) 。 <br>重新連接或更換磁片磁碟機之後，儲存空間直接存取會自動啟動修復。<br>3. 如果您不是使用儲存空間直接存取，接下來請使用 [VirtualDisk](/powershell/module/storage/repair-virtualdisk) Cmdlet 來修復虛擬磁片。<br><br>如果磁片數目超過您的資料複本，而且虛擬磁片未在失敗期間修復，則虛擬磁片上的所有資料都會永久遺失。 在這種情況下，請刪除虛擬磁片、建立新的虛擬磁片，然後從備份還原。|
| |逾時|連接虛擬磁片花費的時間太長 <br><br> **動作：** 這通常不會發生，因此您可能會嘗試查看條件是否經過時間。 或者，您可以嘗試使用 [VirtualDisk](/powershell/module/storage/disconnect-virtualdisk) 指令中斷連接虛擬磁片，然後使用 [VirtualDisk](/powershell/module/storage/connect-virtualdisk)  Cmdlet 來重新連接。 |

## <a name="drive-physical-disk-states"></a>磁片磁碟機 (實體磁片) 狀態

以下各節說明磁碟機可能的健全狀態。 集區中的磁片磁碟機在 PowerShell 中是以 *實體磁片* 物件來表示。

### <a name="drive-health-state-healthy"></a>磁碟機健全狀態：Healthy

|操作狀態    |描述|
|---------            |---------          |
|[確定]|磁片磁碟機狀況良好。|
|服務中|磁碟機正在執行某些內部內務處理作業。 當此動作完成時，磁片磁碟機應該會返回「 *確定* 」健全狀態。|

### <a name="drive-health-state-warning"></a>磁片磁碟機健全狀態：警告

處於「警告」狀態的磁碟機可成功讀取和寫入資料，但會發生問題。

|操作狀態    |描述|
|---------            |---------          |
|中斷通訊|磁片磁碟機遺失。 如果您是使用儲存空間直接存取，這可能是因為伺服器已關閉。<br><br>**動作**：如果您使用儲存空間直接存取，請讓所有的伺服器恢復上線。 如果未修正此問題，請重新連接磁片磁碟機、更換磁片磁碟機，或嘗試取得有關此磁片磁碟機的詳細診斷資訊，方法是遵循使用 Windows 錯誤報告 > [實體磁片超時](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out)的步驟進行疑難排解。|
|正在從集區移除|儲存空間正在從存放集區移除磁片磁碟機。 <br><br> 這是暫時性狀態。 移除完成後，如果磁片磁碟機仍連接到系統，磁片磁碟機會轉換成另一個操作狀態， (通常會在原創組區中) 。|
|正在進入維護模式|當系統管理員將磁片磁碟機置於維護模式之後，儲存空間正在將磁片磁碟機置於維護模式。 這是暫時性的狀態-磁片磁碟機應該很快就會處於 *維護模式* 狀態。|
|處於維護模式|系統管理員將磁片磁碟機置於維護模式，並停止磁片磁碟機的讀取和寫入。 這通常是在更新磁片磁碟機固件之前，或在測試失敗時完成。<br><br>**動作**：若要讓磁片磁碟機離開維護模式，請使用 [StorageMaintenanceMode](/powershell/module/storage/disable-storagemaintenancemode) Cmdlet。|
|正在停止維護模式|系統管理員已將磁片磁碟機移出維護模式，而儲存空間正在將磁片磁碟機恢復上線的過程中。 這是暫時性的狀態-磁片磁碟機應該很快就會處於另一個狀態-理想 *狀況良好*。|
|預期性故障|磁片磁碟機回報其接近失敗。<br><br>**動作**：更換磁片磁碟機。|
|IO 錯誤|存取磁碟機時會發生暫時性錯誤。<br><br>**動作**： <br>1. 如果磁片磁碟機未轉換回「 **確定** 」狀態，您可以嘗試使用 [PhysicalDisk](/powershell/module/storage/reset-physicaldisk) 指令 Cmdlet 來抹除磁片磁碟機。 <br> 2. 使用 [修復 VirtualDisk](/powershell/module/storage/repair-virtualdisk) 來還原受影響虛擬磁片的復原。 <br>3. 如果持續發生，請更換磁片磁碟機。|
|暫時性錯誤|磁碟機發生暫時性錯誤。 這通常表示磁片磁碟機沒有回應，但也可能表示儲存空間保護磁碟分割未適當地從磁片磁碟機移除。 <br><br>**動作**： <br>1. 如果磁片磁碟機未轉換回「 **確定** 」狀態，您可以嘗試使用 [PhysicalDisk](/powershell/module/storage/reset-physicaldisk) 指令 Cmdlet 來抹除磁片磁碟機。 <br> 2. 使用 [修復 VirtualDisk](/powershell/module/storage/repair-virtualdisk) 來還原受影響虛擬磁片的復原。 <br>3. 如果此情況持續發生，請更換磁片磁碟機，或嘗試取得有關此磁片磁碟機的詳細診斷資訊，方法是遵循使用 Windows 錯誤報告中的疑難排解步驟，> [實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|異常延遲|磁片磁碟機的執行速度很慢，以儲存空間直接存取中的健全狀況服務來測量。<br><br>**動作**：如果此情況持續發生，請更換磁片磁碟機，使其不會降低整體儲存空間的效能。

### <a name="drive-health-state-unhealthy"></a>磁碟機健全狀態：狀況不良

處於「狀況不良」狀態的磁碟機目前無法進行寫入或存取。

|操作狀態    |描述|
|---------            |---------          |
|無法使用|儲存空間無法使用此磁片磁碟機。 如需詳細資訊，請參閱 [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md);如果您不是使用儲存空間直接存取，請參閱 [儲存空間總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v=ws.11)#Requirements)。|
|Split|磁碟機已與集區分離。<br><br>**動作**：重設磁片磁碟機，並清除磁片磁碟機中的所有資料，並將其新增回集區以作為空磁片磁碟機。 若要這樣做，請以系統管理員身分開啟 PowerShell 會話，執行 [PhysicalDisk](/powershell/module/storage/reset-physicaldisk) Cmdlet，然後執行 [VirtualDisk](/powershell/module/storage/repair-virtualdisk)。 <br><br>若要取得有關此磁片磁碟機的詳細診斷資訊，請遵循使用 Windows 錯誤報告的疑難排解步驟，> [實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|過時的中繼資料|儲存空間在磁片磁碟機上找到舊的中繼資料。<br><br>**動作**：這應該是暫時性的狀態。 如果磁片磁碟機未轉換回 [確定]，您可以執行 [修復 VirtualDisk](/powershell/module/storage/repair-virtualdisk) ，在受影響的虛擬磁片上啟動修復作業。 如果這無法解決問題，您可以使用 [PhysicalDisk 指令程式](/powershell/module/storage/reset-physicaldisk) 重設磁片磁碟機，並抹除磁片磁碟機中的所有資料，然後再執行 [VirtualDisk](/powershell/module/storage/repair-virtualdisk)。|
|無法辨識的中繼資料|儲存空間在磁片磁碟機上找到無法辨識的中繼資料，這通常表示磁片磁碟機有來自不同集區的中繼資料。<br><br>**動作**：若要抹除磁片磁碟機，並將它新增至目前的集區，請重設磁片磁碟機。 若要重設磁片磁碟機，請以系統管理員身分開啟 PowerShell 會話，執行 [PhysicalDisk](/powershell/module/storage/reset-physicaldisk) Cmdlet，然後執行 [VirtualDisk](/powershell/module/storage/repair-virtualdisk)。|
|故障的媒體|磁碟機故障，且儲存空間不會再加以使用。<br><br>**動作**：更換磁片磁碟機。 <br><br>若要取得有關此磁片磁碟機的詳細診斷資訊，請遵循使用 Windows 錯誤報告的疑難排解步驟，> [實體磁片無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|裝置硬體故障|此磁碟機發生硬體故障。 <br><br>**動作**：更換磁片磁碟機。|
|正在更新韌體|Windows 正在更新磁片磁碟機上的固件。 這是暫時性的狀態，持續時間通常少於一分鐘，且在此期間，集區中的其他磁碟機將會處理所有的讀取和寫入。 如需詳細資訊，請參閱 [更新磁片磁碟機固件](../update-firmware.md)。|
|啟動中|磁碟機正在進行作業準備。 這應該是暫時性的狀態 - 完成後，磁碟機應該就會轉換成不同的操作狀態。|

## <a name="reasons-a-drive-cant-be-pooled"></a>磁碟機無法進入集區的原因

有些磁片磁碟機還沒準備好存放集區。 您可以查看實體磁片的屬性，找出磁片磁碟機不適合進行共用的原因 `CannotPoolReason` 。 以下是顯示 CannotPoolReason 屬性的範例 PowerShell 腳本：

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

下表將對各種原因的說明稍作補充。

|原因|描述|
|---|---|
|在集區中|磁片磁碟機已屬於存放集區。 <br><br>磁片磁碟機一次只能屬於一個儲存集區。 若要在另一個存放集區中使用此磁片磁碟機，請先從現有的集區中移除該磁片磁碟機，這會告知儲存空間將磁片磁碟機上的資料移到集區上的其他磁片磁碟機。 或者，如果磁片磁碟機已從其集區中斷連線，而未通知儲存空間，則重設磁片磁碟機。 <br><br>若要安全地從存放集區移除磁片磁碟機，請使用 [PhysicalDisk](/powershell/module/storage/remove-physicaldisk)，或移至伺服器管理員 > 的檔案 **和存放服務**  >  **存放集區**，>**實體磁片**，以滑鼠右鍵按一下磁片磁碟機，然後選取 [**移除磁片**]。<br><br>若要重設磁片磁碟機，請使用 [重設-PhysicalDisk](/powershell/module/storage/reset-physicaldisk)。|
|狀況不良|磁碟機未處於良好狀態，可能需要更換。|
|卸除式媒體|磁碟機歸類為卸除式磁碟機。 <br><br>儲存空間不支援 Windows 以可移動方式辨識的媒體，例如 Blu-Ray 磁片磁碟機。 雖然許多固定磁片磁碟機都位於可移動插槽中，但一般而言，以 Windows *分類* 的媒體作為卸載式並不適用于儲存空間。|
|由叢集使用中|磁碟機目前由容錯移轉叢集使用中。|
|離線|磁碟機已離線。 <br><br>若要讓所有離線磁片磁碟機上線並設定為讀取/寫入，請以系統管理員身分開啟 PowerShell 會話，並使用下列腳本：<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|容量不足|當磁碟分割佔用磁片磁碟機上的可用空間時，通常就會發生這種情況。 <br><br>**動作**：刪除磁片磁碟機上的所有磁片區，並清除磁片磁碟機上的所有資料。 其中一種方法是使用 [純磁片](/powershell/module/storage/clear-disk) PowerShell Cmdlet。|
|驗證進行中|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)正在檢查磁片磁碟機或磁片磁碟機上的磁片磁碟機是否已通過核准可供伺服器系統管理員使用。|
|驗證失敗|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)無法檢查磁片磁碟機或磁片磁碟機上的磁片磁碟機是否已通過核准可供伺服器系統管理員使用。|
|韌體不符合規範|實體磁片磁碟機上的固件不在伺服器管理員使用 [健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)指定的已核准固件修訂清單中。 |
|硬體不符合規範|磁片磁碟機不在伺服器管理員使用 [健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)指定的已核准儲存模型清單中。|

## <a name="additional-references"></a>其他參考資料

- [儲存空間直接存取](storage-spaces-direct-overview.md)
- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [了解叢集和集區仲裁](understand-quorum.md)
