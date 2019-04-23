---
title: 儲存空間直接存取的健康情況和操作狀態
description: 如何尋找及了解不同的健全狀況和操作狀態的儲存空間直接存取和儲存空間 （包括實體磁碟、 集區和虛擬磁碟），以及如何處理它們。
keywords: 儲存空間，卸離後，虛擬磁碟、 實體磁碟已降級
author: jasongerend
ms.author: jgerend
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage-spaces
manager: brianlic
ms.openlocfilehash: 5090a68270438bd9a06c7d50f9d4abca066d31e6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849139"
---
# <a name="troubleshoot-storage-spaces-direct-health-and-operational-states"></a>針對儲存空間直接存取的健全狀況和操作狀態進行疑難排解

> 適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012，Windows Server （半年通道），Windows 10，Windows 8.1

本主題說明的健全狀況和操作狀態的儲存體集區、 虛擬磁碟 （位於儲存空間的磁碟區之下），並磁碟機[儲存空間直接存取](storage-spaces-direct-overview.md)並[儲存空間](overview.md)。 嘗試疑難排解各種問題，例如為什麼無法刪除虛擬磁碟，因為唯讀組態時，這些狀態可以是非常寶貴。 它也會討論為何要將磁碟機不能加入集區 (CannotPoolReason)。

儲存空間有三個主要物件-*實體磁碟*(硬碟機，Ssd，等) 加入到*儲存體集區*，虛擬化儲存體，好讓您可以建立*的虛擬磁碟*從中的可用空間集區，如下所示。 集區的中繼資料會寫入至集區中的每個磁碟機。 磁碟區上的虛擬磁碟會建立和儲存您的檔案，但我們不討論以下的磁碟區。

![實體磁碟新增至存放集區，然後從建立虛擬磁碟的集區空間](media/storage-spaces-states/storage-spaces-object-model.png)

在 [伺服器管理員] 中，或使用 PowerShell，您可以檢視健康情況和操作狀態。 以下是各種不同的 （大多不正確） 的健全狀況和操作的狀態，遺漏大多數的叢集節點的儲存空間直接存取叢集上的範例 (以滑鼠右鍵按一下資料行標頭，以新增**操作狀態**)。 這不愉快的叢集。

![顯示遺漏的兩個節點的結果，在儲存空間直接存取叢集中許多遺漏的實體磁碟和狀況不良的虛擬磁碟的伺服器管理員](media/storage-spaces-states/unhealthy-disks-in-server-manager.png)

## <a name="storage-pool-states"></a>儲存體集區狀態

每個存放集區具有健全狀況狀態-**狀況良好**，**警告**，或**未知**/**狀況不良**、 以及為一個或多個操作狀態。

若要了解集區是在何種狀態，使用下列 PowerShell 命令：

```PowerShell
Get-StoragePool -IsPrimordial $False | Select-Object HealthStatus, OperationalStatus, ReadOnlyReason
```

以下是範例輸出顯示存放集區中的唯讀狀態的操作狀態未知的健康情況狀態：

```
FriendlyName                OperationalStatus HealthStatus IsPrimordial IsReadOnly
------------                ----------------- ------------ ------------ ----------
S2D on StorageSpacesDirect1 Read-only         Unknown      False        True
```

下列各節列出的健全狀況和操作狀態。

### <a name="pool-health-state-healthy"></a>集區健全狀況狀態：良好

|操作狀態    |描述|
|---------            |---------  |
|[確定]|存放集區狀況良好。|

### <a name="pool-health-state-warning"></a>集區健全狀況狀態：警告

當儲存體集區處於**警告**健全狀況狀態，即表示集區可供存取，但一或多個磁碟機失敗或遺失。 如此一來，您的儲存體集區可能會降低了恢復功能。

|操作狀態    |描述|
|---------            |---------  |
|衰退|存放集區中沒有失敗或遺失的磁碟機。 只能搭配裝載集區的中繼資料的磁碟機，就會發生此狀況。 <br><br>**動作**:檢查您的磁碟機的狀態，並取代任何失敗的磁碟機，才能有其他的失敗。|

### <a name="pool-health-state-unknown-or-unhealthy"></a>集區健全狀況狀態：未知或狀況不良

當儲存集區處於**不明**或是**狀況不良**健全狀況狀態，這表示該儲存集區處於唯讀狀態，並不能修改，直到集區會傳回到**警告**或是 **[確定]** 健全狀況狀態。

|操作狀態    |唯讀狀態的原因 |描述|
|---------            |---------       |--------   |
|唯讀|不完整|如果存放集區將會遺失其[仲裁](understand-quorum.md)，這表示集區中的大部分磁碟機故障，或基於某些原因皆為離線。 當集區會失去其仲裁時，儲存空間會自動設定集區為唯讀之前再次變成使用足夠的磁碟機。<br><br>**動作：** <br>1.重新連接任何遺失的磁碟機，然後如果您使用儲存空間直接存取，讓所有伺服器建立連線。 <br>2.以系統管理權限開啟 PowerShell 工作階段，然後再輸入的讀寫回設定集區：<br><br> <code>Get-StoragePool <PoolName> -IsPrimordial $False \| Set-StoragePool -IsReadOnly $false</code>|
||原則|系統管理員設定存放集區為唯讀。<br><br>**動作：** 若要設定為可讀寫存取在 「 容錯移轉叢集管理員 」 中的叢集的儲存集區，請前往**集區**，以滑鼠右鍵按一下 集區，然後選取**上線**。<br><br>其他伺服器和電腦，請以系統管理權限開啟 PowerShell 工作階段，然後輸入：<br><br><code>Get-StoragePool <PoolName> \| Set-StoragePool -IsReadOnly $false</code><br><br> |
||Starting|儲存空間已啟動，或等候連接集區中的磁碟機。 這應該是暫時性的狀態。 完全啟動之後，集區應該轉換成不同的操作狀態。<br><br>**動作：** 如果集區處於*啟動*狀態時，請確定已正確連接集區中的所有磁碟機。|

另請參閱[修改存放集區具有唯讀設定](https://social.technet.microsoft.com/wiki/contents/articles/14861.modifying-a-storage-pool-that-has-a-read-only-configuration.aspx)。

## <a name="virtual-disk-states"></a>虛擬磁碟狀態

在儲存空間磁碟區會放會從中切割出集區中的可用空間的虛擬磁碟 （儲存空間）。 每個虛擬磁碟有健全狀況狀態-**狀況良好**，**警告**，**狀況不良**，或**未知**以及一或多個操作狀態。

若要了解狀態的虛擬磁碟會在中，使用下列 PowerShell 命令：

```PowerShell
Get-VirtualDisk | Select-Object FriendlyName,HealthStatus, OperationalStatus, DetachedReason
```

輸出顯示已中斷連結虛擬磁碟 」 和 「 已降級/不完整的虛擬磁碟的範例如下：

```
FriendlyName HealthStatus OperationalStatus      DetachedReason
------------ ------------ -----------------      --------------
Volume1      Unknown      Detached               By Policy
Volume2      Warning      {Degraded, Incomplete} None
```

下列各節列出的健全狀況和操作狀態。

### <a name="virtual-disk-health-state-healthy"></a>虛擬磁碟健全狀況狀態：良好

|操作狀態    |描述|
|---------            |---------          |
|[確定]    |虛擬磁碟是狀況良好。|
|次佳    |資料未平均寫入到磁碟機。 <br><br>**動作**:透過執行來最佳化存放集區的磁碟機使用量[Optimize-storagepool](https://technet.microsoft.com/itpro/powershell/windows/storage/optimize-storagepool) cmdlet。|

### <a name="virtual-disk-health-state-warning"></a>虛擬磁碟健全狀況狀態：警告

當虛擬磁碟會處於**警告**健全狀況狀態，表示一或多個資料複本皆無法使用，但是儲存空間仍可讀取的資料至少一個複本。

|操作狀態    |描述|
|---------            |---------          |
|在服務中            |Windows 修復虛擬磁碟，例如新增或移除磁碟機之後。 修復完成後，虛擬磁碟應該回到正常的健全狀況狀態。|
|不完整           |虛擬磁碟的復原能力會減少，因為一或多個磁碟機失敗或遺失。 不過，遺失的磁碟機包含資料的最新複本。<br><br> **動作**: <br>1.重新連線任何遺失的磁碟機、 取代任何失敗的磁碟機，以及如果您使用儲存空間直接存取，上線任何離線的伺服器。 <br>2.如果您不使用儲存空間直接存取，接下來修復虛擬磁碟使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。<br> 儲存空間直接存取會自動啟動修復視之後重新連接，或取代的磁碟機。|
|衰退             |虛擬磁碟的復原能力會減少，因為一或多個磁碟機失敗或遺失，且您的資料，這些磁碟機上的過期的副本。 <br><br>**動作**: <br> 1.重新連線任何遺失的磁碟機、 取代任何失敗的磁碟機，以及如果您使用儲存空間直接存取，上線任何離線的伺服器。 <br> 2.如果您不使用儲存空間直接存取，接下來修復虛擬磁碟使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。 <br>儲存空間直接存取會自動啟動修復視之後重新連接，或取代的磁碟機。|

### <a name="virtual-disk-health-state-unhealthy"></a>虛擬磁碟健全狀況狀態：Unhealthy

當虛擬磁碟會處於**狀況不良**健全狀況狀態、 部分或所有虛擬磁碟上的資料是目前無法存取。

|操作狀態    |描述|
|---------            |--------   |
|沒有備援|虛擬磁碟已遺失資料，因為太多的磁碟機失敗。<br><br>**動作**:取代失敗的磁碟機，並從備份還原您的資料。|

### <a name="virtual-disk-health-state-informationunknown"></a>虛擬磁碟健全狀況狀態：資訊/未知

虛擬磁碟也可以在**資訊**健全狀況狀態 （如儲存體空間控制台項目中所示） 或**未知**健全狀況狀態 （如下所示，在 PowerShell 中） 是否系統管理員所花費的虛擬磁碟離線或變成已中斷連結虛擬磁碟。

|操作狀態    |卸離的原因 |描述|
|---------            |---------       |--------   |
|已中斷連結             |由原則            |系統管理員讓虛擬磁碟離線，或設定需要手動附加檔案之虛擬磁碟，在此情況下您必須以手動方式附加虛擬磁碟，每次重新啟動 Windows。，<br><br>**動作**:將虛擬磁碟重新上線。 若要執行這項操作時的虛擬磁碟是叢集的儲存集區中，選取在容錯移轉叢集管理員**儲存體** > **集區** > **虛擬磁碟**選取顯示的虛擬磁碟**Offline**狀態，然後選取**上線**。 <br><br>若要讓虛擬磁碟重新上線時不在叢集中，開啟以系統管理員的 PowerShell 工作階段，然後嘗試使用下列命令：<br><br> <code>Get-VirtualDisk \| Where-Object -Filter { $_.OperationalStatus -eq "Detached" } \| Connect-VirtualDisk</code><br><br>Windows 重新啟動之後，自動附加所有非叢集的虛擬磁碟，開啟以系統管理員的 PowerShell 工作階段，然後使用下列命令：<br><br> <code>Get-VirtualDisk \| Set-VirtualDisk -ismanualattach $false</code>|
|            |狀況不良的大部分磁碟 |太多使用此虛擬磁碟的磁碟機失敗、 遺失，或有過時資料。   <br><br>**動作**: <br> 1.重新連線任何遺失的磁碟機，以及如果您使用儲存空間直接存取，上線任何離線的伺服器。 <br> 2.所有磁碟機和伺服器都在線上之後，取代失敗的任何磁碟機。 請參閱[健全狀況服務](../../failover-clustering/health-service-overview.md)如需詳細資訊。 <br>儲存空間直接存取會自動啟動修復視之後重新連接，或取代的磁碟機。<br>3.如果您不使用儲存空間直接存取，接下來修復虛擬磁碟使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。  <br><br>如果多個磁碟失敗有個資料複本，而虛擬磁碟未修復中間失敗，虛擬磁碟上的所有資料都都會永久遺失。 可惜在本例中，刪除虛擬磁碟建立新的虛擬磁碟，然後從備份還原。|
|                     |不完整 |沒有足夠的磁碟機有讀取虛擬磁碟。    <br><br>**動作**: <br> 1.重新連線任何遺失的磁碟機，以及如果您使用儲存空間直接存取，上線任何離線的伺服器。 <br> 2.所有磁碟機和伺服器都在線上之後，取代失敗的任何磁碟機。 請參閱[健全狀況服務](../../failover-clustering/health-service-overview.md)如需詳細資訊。 <br>儲存空間直接存取會自動啟動修復視之後重新連接，或取代的磁碟機。<br>3.如果您不使用儲存空間直接存取，接下來修復虛擬磁碟使用[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk) cmdlet。<br><br>如果多個磁碟失敗有個資料複本，而虛擬磁碟未修復中間失敗，虛擬磁碟上的所有資料都都會永久遺失。 可惜在本例中，刪除虛擬磁碟建立新的虛擬磁碟，然後從備份還原。|
| |逾時|連接虛擬磁碟的時間過長 <br><br> **動作：** 應該不會發生此情況下，因此您可能會嘗試查看時間是否通過條件。 或者，您可以嘗試中斷連線的虛擬磁碟[Disconnect-virtualdisk](https://docs.microsoft.com/powershell/module/storage/disconnect-virtualdisk?view=win10-ps) cmdlet，然後使用[Connect-virtualdisk](https://docs.microsoft.com/powershell/module/storage/connect-virtualdisk?view=win10-ps) cmdlet 來重新連接它。 |

## <a name="drive-physical-disk-states"></a>磁碟機 （實體磁碟） 狀態

下列各節說明可以是磁碟機的健全狀況狀態。 在 PowerShell 中表示集區中的磁碟機*實體磁碟*物件。

### <a name="drive-health-state-healthy"></a>磁碟機健全狀況狀態：良好

|操作狀態    |描述|
|---------            |---------          |
|[確定]|磁碟機狀況良好。|
|在服務中|磁碟機正在執行某些內部內務處理作業。 完成此動作時，磁碟機應該返回*確定*健全狀況狀態。|

### <a name="drive-health-state-warning"></a>磁碟機健全狀況狀態：警告

處於警告狀態可以的磁碟機讀取和成功寫入資料，但發生問題。

|操作狀態    |描述|
|---------            |---------          |
|遺失的通訊|遺失的磁碟機。 如果您使用儲存空間直接存取，這可能是因為伺服器已關閉。<br><br>**動作**:如果您使用儲存空間直接存取，讓所有的伺服器恢復上線。 如果這樣未修正它，磁碟機重新連接，取代它，或嘗試中使用 Windows 錯誤報告進行疑難排解的步驟，以取得詳細的診斷資訊，此磁碟機的相關 >[實體磁碟已逾時](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-timed-out)。|
|從集區中移除|從其儲存體集區移除磁碟機正在儲存空間。 <br><br> 這是暫時性的狀態。 移除已完成，如果磁碟機仍會附加到系統之後的磁碟機轉換成另一個操作狀態 (通常是 OK) 原始集區中。|
|起始的維護模式|磁碟機置於維護模式，系統管理員會將磁碟機置於維護模式之後正在儲存空間。 這是暫時性的狀態-中應該很快就到磁碟機*處於維護模式*狀態。|
|處於維護模式|系統管理員，將磁碟機放置處於維護模式，暫停從讀取與寫入磁碟機。 這通常是完成之前更新磁碟機韌體，或測試失敗時。<br><br>**動作**:若要離開維護模式的磁碟機，請使用[停用 StorageMaintenanceMode](https://technet.microsoft.com/itpro/powershell/windows/storage/disable-storagemaintenancemode) cmdlet。|
|停止維護模式|系統管理員採取的磁碟機移出維護模式，並帶回線上的磁碟機正在儲存空間。 這是暫時性的狀態-磁碟機很快就應該在另一種狀態-理想的情況下*狀況良好*。|
|預測性失敗|所報告的磁碟機，很接近失敗。<br><br>**動作**:更換磁碟機。|
|IO 錯誤|發生暫時性錯誤，存取磁碟機。<br><br>**動作**: <br>1.如果磁碟機不轉換回到 **[確定]** 狀態時，您可以嘗試使用[Reset-physicaldisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 來清除磁碟機。 <br> 2.使用[Repair-virtualdisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)還原受影響的虛擬磁碟的復原。 <br>3.如果這持續發生，請更換磁碟機。|
|暫時性錯誤|發生暫時性錯誤與磁碟機。 這通常表示磁碟機沒有回應，但它也可能表示，從磁碟機不適當地移除保護的磁碟分割的儲存空間。 <br><br>**動作**: <br>1.如果磁碟機不轉換回到 **[確定]** 狀態時，您可以嘗試使用[Reset-physicaldisk](https://docs.microsoft.com/powershell/module/storage/reset-physicaldisk) cmdlet 來清除磁碟機。 <br> 2.使用[Repair-virtualdisk](https://docs.microsoft.com/powershell/module/storage/repair-virtualdisk)還原受影響的虛擬磁碟的復原。 <br>3.如果這持續發生，請更換磁碟機，或嘗試中使用 Windows 錯誤報告進行疑難排解的步驟，以取得詳細的診斷資訊，此磁碟機的相關 >[實體磁碟而無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|異常的延遲|磁碟機的效能不如預期，測量健全狀況服務中儲存空間直接存取。<br><br>**動作**:如果這持續發生，請更換磁碟機，因此它不會減少整體儲存空間的效能。

### <a name="drive-health-state-unhealthy"></a>磁碟機健全狀況狀態：Unhealthy

寫入目前到或無法存取處於狀況不良狀態的磁碟機。

|操作狀態    |描述|
|---------            |---------          |
|無法使用|此磁碟機無法使用儲存空間。 如需詳細資訊，請參閱[儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md); 如果您不使用儲存空間直接存取，請參閱[儲存體空間概觀](https://technet.microsoft.com/library/hh831739(v=ws.11).aspx#Requirements)。|
|分割|磁碟機已變成分開的集區。<br><br>**動作**:重設清除所有資料從磁碟機，並將它新增為空的磁碟機集區磁碟機。 若要這樣做，請開啟 以系統管理員身分執行 PowerShell 工作階段[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然後執行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。 <br><br>若要取得此磁碟機的相關詳細診斷資訊，請依照下列中使用 Windows 錯誤報告進行疑難排解的步驟 >[實體磁碟而無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|過時的中繼資料|儲存空間的磁碟機上找到舊的中繼資料。<br><br>**動作**:這應該是暫時性的狀態。 如果磁碟機不會轉換回 [確定]，您可以執行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)啟動修復作業上受影響的虛擬磁碟。 如果這無法解決此問題，您可以重設的磁碟機[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，抹除所有資料磁碟機，然後再執行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|無法辨識的中繼資料|儲存空間上的磁碟機，這通常表示磁碟機具有中繼資料，從不同的集區上找到無法辨識的中繼資料。<br><br>**動作**:若要抹除的磁碟機，並將它新增至目前的集區，重設磁碟機。 若要重設磁碟機，請開啟 以系統管理員身分執行 PowerShell 工作階段[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk) cmdlet，然後執行[Repair-virtualdisk](https://technet.microsoft.com/itpro/powershell/windows/storage/repair-virtualdisk)。|
|失敗的媒體|磁碟機失敗，而且不會再使用的儲存空間。<br><br>**動作**:更換磁碟機。 <br><br>若要取得此磁碟機的相關詳細診斷資訊，請依照下列中使用 Windows 錯誤報告進行疑難排解的步驟 >[實體磁碟而無法上線](../../failover-clustering/troubleshooting-using-wer-reports.md#physical-disk-failed-to-come-online)。|
|裝置硬體故障|此磁碟機上發生硬體失敗。 <br><br>**動作**:更換磁碟機。|
|更新韌體|Windows 正在更新磁碟機上的軔體。 這是暫時性的狀態，持續時間通常少於一分鐘的時間和期間，集區中的其他磁碟機處理所有的時間讀取和寫入。 如需詳細資訊，請參閱 <<c0> [ 更新磁碟機韌體](../update-firmware.md)。|
|Starting|磁碟機準備作業。 這應該是暫時性的狀態-完成後，磁碟機應該轉換成不同的操作狀態。|

## <a name="reasons-a-drive-cant-be-pooled"></a>無法共用磁碟機的原因

有些磁碟機只是未就緒，無法在存放集區。 您可以了解為何將磁碟機不適合共用藉由查看`CannotPoolReason`的實體磁碟的屬性。 以下是範例 PowerShell 指令碼，以顯示 CannotPoolReason 屬性：

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

下表提供每個原因更詳盡。

|原因|描述|
|---|---|
|集區中|磁碟機已屬於儲存集區。 <br><br>磁碟機可以屬於一次只有一個單一的存放集區。 若要使用此磁碟機，另一個儲存體集區中，先從其現有的集區，它會告訴儲存空間，以將資料磁碟機上移至其他磁碟機，在集區移除磁碟機。 或重設磁碟機，如果磁碟機已中斷連線其集區且不會通知儲存空間。 <br><br>若要安全地移除存放集區的磁碟機，請使用[Remove-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/remove-physicaldisk)，或移至 伺服器管理員 >**檔案和存放服務** > **存放集區**，>**實體磁碟**，以滑鼠右鍵按一下 磁碟機，然後選取**移除磁碟**。<br><br>若要重設的磁碟機，請使用[Reset-physicaldisk](https://technet.microsoft.com/itpro/powershell/windows/storage/reset-physicaldisk)。|
|狀況不良|磁碟機不處於狀況良好的狀態，而且可能需要更換。|
|卸除式媒體|磁碟機都歸類為卸除式磁碟機。 <br><br>儲存空間不支援 Windows 所辨識為卸除式，例如 Blu-ray 磁碟機的媒體。 雖然許多固定磁碟機位於卸除式的位置，在一般情況下，屬於媒體*分類*為卸除式的 Windows 所不適合儲存空間搭配使用。|
|使用叢集|磁碟機，目前正由容錯移轉叢集。|
|離線|磁碟機已離線。 <br><br>若要讓所有離線的磁碟機上線並設定為讀取/寫入，開啟以系統管理員的 PowerShell 工作階段並使用下列指令碼：<br><br><code>Get-Disk \| Where-Object -Property OperationalStatus -EQ "Offline" \| Set-Disk -IsOffline $false</code><br><br><code>Get-Disk \| Where-Object -Property IsReadOnly -EQ $true \| Set-Disk -IsReadOnly $false</code>|
|沒有足夠的容量|這通常發生分割區佔用的磁碟機上的可用空間時。 <br><br>**動作**:刪除任何磁碟區上的磁碟機中，然後再清除磁碟機上的所有資料。 若要這麼做的一個方式是使用[Clear-disk](https://docs.microsoft.com/powershell/module/storage/clear-disk?view=win10-ps) PowerShell cmdlet。|
|正在確認|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)檢查以查看是否磁碟機或磁碟機上的韌體核准使用伺服器管理員。|
|驗證失敗|[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)無法檢查看看是否磁碟機或磁碟機上的韌體核准使用伺服器管理員。|
|不符合規範的韌體|實體磁碟機上的軔體不在清單中的伺服器系統管理員所指定的已核准的韌體版本[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)。 |
|不符合規範的硬體|磁碟機不在清單中的伺服器系統管理員所指定的已核准的儲存體模型[健全狀況服務](../../failover-clustering/health-service-overview.md#supported-components-document)。|

## <a name="see-also"></a>另請參閱

- [儲存空間直接存取](storage-spaces-direct-overview.md)
- [儲存空間直接存取硬體需求](storage-spaces-direct-hardware-requirements.md)
- [了解叢集和集區仲裁](understand-quorum.md)