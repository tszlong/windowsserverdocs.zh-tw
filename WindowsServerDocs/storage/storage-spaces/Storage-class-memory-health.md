---
ms.assetid: 2bab6bf6-90e7-46a7-b917-14a7a8f55366
title: Windows 中的存放裝置類別記憶體 (NVDIMM-N) 健全狀況管理
ms.prod: windows-server
ms.author: jgerend
ms.manager: dongill
ms.technology: storage-spaces
ms.topic: article
author: JasonGerend
ms.date: 06/25/2019
ms.localizationpriority: medium
ms.openlocfilehash: 03d986832e14e0dd7b80324de3c9f14d0537dba5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402902"
---
# <a name="storage-class-memory-nvdimm-n-health-management-in-windows"></a>Windows 中的存放裝置類別記憶體 (NVDIMM-N) 健全狀況管理

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows 10

本文向系統管理員和 IT 專業人員提供 Windows 中存放裝置類別記憶體 (NVDIMM-N) 裝置特定的錯誤處理與健全狀況管理相關資訊，強調說明存放裝置類別記憶體與傳統存放裝置之間的差異。

如果您不熟悉 Windows 對存放裝置類別記憶體裝置的支援，這些短片可提供概觀：
- [在 Windows Server 2016 中使用非動態記憶體（NVDIMM-N）做為區塊存放裝置](https://channel9.msdn.com/Events/Build/2016/P466)
- [在 Windows Server 2016 中使用非動態記憶體（NVDIMM-N）做為位元組可定址的儲存空間](https://channel9.msdn.com/Events/Build/2016/P470)
- [以 Windows Server 2016 中的持續性記憶體加速 SQL Server 2016 效能](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-Windows-Server-2016-SCM--FAST)

另請參閱[瞭解和部署儲存空間直接存取中的持續性記憶體](deploy-pmem.md)。

從 Windows Server 2016 和 Windows 10 (版本 1607) 開始，Windows 中使用原生驅動程式支援 JEDEC 相容的 NVDIMM-N 存放裝置類別記憶體裝置。 雖然這些裝置的行為類似於其他磁碟 (HDD 與 SSD)，還是有一些差異。

這裡所列出的所有狀況都預期是非常罕見的，但還是要根據硬體使用的狀況而定。

以下各種案例可能會參考儲存空間組態。 令人感興趣的特定組態是其中使用兩個 NVDIMM-N 裝置做為儲存空間中的鏡像回寫式快取。 若要設定這類組態，請參閱[使用 NVDIMM-N 回寫式快取設定儲存空間](https://msdn.microsoft.com/library/mt650885.aspx)。

在 Windows Server 2016 中，儲存空間 GUI 會將 NVDIMM N 匯流排類型顯示為「未知」。 這在建立集區、儲存空間 VD 時不會發生任何功能中斷或失效。 您可以執行下列命令來驗證匯流排類型：

```powershell
PS C:\>Get-PhysicalDisk | fl
```
Cmdlet 輸出中的參數 BusType 會正確地將匯流排類型顯示為「SCM」

## <a name="checking-the-health-of-storage-class-memory"></a>檢查存放裝置類別記憶體的健全狀況
若要查詢存放裝置類別記憶體的健全狀況，請在 Windows PowerShell 工作階段中使用下列命令。

```powershell
PS C:\> Get-PhysicalDisk | where BusType -eq "SCM" | select SerialNumber, HealthStatus, OperationalStatus, OperationalDetails
```

這麼做會產生此範例輸出︰

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 良好 | [確定] | |
| 802c-01-1602-117cb64f | 警告 | 預測性失敗 | {超出閾值，NVDIMM\_N 錯誤} |

> [!NOTE]
> 若要尋找事件中指定之 NVDIMM-N 裝置的實體位置，請在 [事件檢視器] 中事件的 **\[詳細資料\]** 索引標籤上，移至 **\[EventData\]**  >  **\[位置\]** 。 請注意，Windows Server 2016 會列出不正確的 NVDIMM-N 裝置位置，但這已在 Windows Server 版本 1709 中修正。

如需了解各種健全狀況的說明，請參閱下列各節。

## <a name="warning-health-status"></a>「警告」健全狀況狀態

這是當您檢查存放裝置類別記憶體裝置的健全狀況，並看到其 [健全狀況狀態] 列為 [警告] 的情況，如下列範例輸出中所示︰

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 良好 | [確定] | |
| 802c-01-1602-117cb64f | 警告 | 預測性失敗 | {超出閾值，NVDIMM\_N 錯誤} |

下表列出有關此情況的部分資訊。

| | 描述 |
| --- | --- |
| 可能的情況 | 違反 NVDIMM-N 警告閾值 |
| 根本原因 | NVDIMM-N 裝置可追蹤各種臨界值，例如溫度、NVM 存留期，及/或能量來源存留期。 當超過這些閾值的其中一個時，作業系統會收到通知。 |
| 一般行為 | 裝置維持完全正常運作。 這是警告，而不是錯誤。 |
| 儲存空間行為 | 裝置維持完全正常運作。 這是警告，而不是錯誤。 |
| 其他資訊 | PhysicalDisk 物件的 OperationalStatus 欄位。 EventLog – Microsoft-Windows-ScmDisk0101/Operational |
| 工作 | 根據違反的警告閾值，為謹慎起見，可能需要考慮取代整個或部分的 NVDIMM-N。 例如，如果 NVM 存留期達到閾值時，取代 NVDIMM-N 很合理。 |

## <a name="writes-to-an-nvdimm-n-fail"></a>寫入 NVDIMM-N 會失敗

這是當您檢查存放裝置類別記憶體裝置的健全狀況，並看到其 [健全狀況狀態] 列為 [狀況不良]，且 [操作狀態] 提及 [IO 錯誤] 的情況，如下列範例輸出中所示︰

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
| 802c-01-1602-117cb5fc | 良好 | [確定] | |
| 802c-01-1602-117cb64f | Unhealthy | {過時的中繼資料、IO 錯誤、暫時性錯誤} | {遺失資料持續性、遺失資料、NV...} |

下表列出有關此情況的部分資訊。

| | 描述 |
| --- | --- |
| 可能的情況 | 遺失持續性 / 備份電源 |
|根本原因|NVDIMM-N 裝置仰賴備份電源以維持其持續性 – 通常是電池或超級電容器。 如果無法使用此備份電源來源或者裝置因為任何原因無法執行備份 (控制器/Flash 錯誤)，資料就會有風險，Windows 會防止對受影響的裝置進行任何進一步寫入作業。 仍可能會進行讀取以撤除資料。|
|一般行為|NTFS 磁碟區將會卸載。<br>[PhysicalDisk 健全狀況狀態] 欄位會針對所有受影響的 NVDIMM-N 裝置顯示「狀況不良」。|
|儲存空間行為|只要僅有一個 NVDIMM-N 受影響，儲存空間將會維持運作。 如果多個裝置受到影響，寫入儲存空間將會失敗。 <br>[PhysicalDisk 健全狀況狀態] 欄位會針對所有受影響的 NVDIMM-N 裝置顯示「狀況不良」。|
|其他資訊|PhysicalDisk 物件的 OperationalStatus 欄位。<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|工作|建議您備份受影響的 NVDIMM-N 的資料。 若要取得讀取權限，您可以手動讓磁碟重新上線 (它會顯示為唯讀 NTFS 磁碟區)。<br><br>若要完全清除這種情況，則必須解決根本原因 (也就是，根據問題來維修電源供應器或是更換 NVDIMM-N)，且 NVDIMM-N 上的磁碟區必須離線並重新上線，或者系統必須重新啟動。<br><br>若要讓 NVDIMM-N 可再度於儲存空間中使用，請使用 **Reset-PhysicalDisk** Cmdlet，這會重新整合裝置並啟動修復程序。|

## <a name="nvdimm-n-is-shown-with-a-capacity-of-0-bytes-or-as-a-generic-physical-disk"></a>NVDIMM-N 會顯示容量為 '0' 位元組或是「一般實體磁碟」

這是當存放裝置類別記憶體裝置顯示容量為 0 位元組且無法使用，或者公開為「一般實體磁碟」物件且 [操作狀態] 為 [遺失通訊] 的情況，如下列範例輸出中所示︰

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|良好|[確定]||
||警告|遺失通訊||

下表列出有關此情況的部分資訊。

||描述|
|---|---|
|可能的情況|BIOS 未向作業系統公開 NVDIMM-N|
|根本原因|NVDIMM-N 裝置是以 DRAM 為基礎。 當參考損毀的 DRAM 位址時，大部分的 CPU 會起始電腦檢查，然後重新啟動伺服器。 部分伺服器平台會取消對應 NVDIMM，以防止作業系統存取它並防止可能因此導致執行另一次電腦檢查。 如果 BIOS 偵測到 NVDIMM-N 已經失敗且需要更換時，這也可能發生。|
|一般行為|NVDIMM-N 會顯示為未初始化，容量為 0 位元組且無法讀取或寫入。|
|儲存空間行為|儲存空間會維持運作 (前提是只有 1 個　NVDIMM-N 受到影響)。<br>NVDIMM-N PhysicalDisk 物件會顯示 [健全狀況狀態] 為 [警告]，且為「一般實體磁碟」|
|其他資訊|PhysicalDisk 物件的 OperationalStatus 欄位。 <br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|工作|NVDIMM-N 裝置必須更換或受到妥善處理，這樣伺服器平台才能將它重新公開給主機作業系統。 建議更換裝置，因為可能發生其他無法修正的錯誤。 將更換裝置新增到儲存空間組態的作業，可以使用 **Add-Physicaldisk** Cmdlet 來完成。|

## <a name="nvdimm-n-is-shown-as-a-raw-or-empty-disk-after-a-reboot"></a>在重新開機後，NVDIMM-N 會顯示為 RAW 或空的磁碟

這是當您檢查存放裝置類別記憶體裝置的健全狀況，並看到其 [健全狀況狀態] 為 [狀況不良]，且 [操作狀態] 為 [無法識別的中繼資料] 的情況，如下列範例輸出中所示︰

| SerialNumber | HealthStatus | OperationalStatus | OperationalDetails |
| --- | --- | --- | --- |
|802c-01-1602-117cb5fc|良好|[確定]|{不明}|
|802c-01-1602-117cb64f|Unhealthy|{無法識別的中繼資料、過時的中繼資料}|{不明}|

下表列出有關此情況的部分資訊。

||描述|
|---|---|
|可能的情況|備份/還原失敗|
|根本原因|備份或還原程序失敗可能會造成 NVDIMM-N 上所有的資料遺失。 作業系統載入時，會顯示為沒有磁碟分割或檔案系統的全新 NVDIMM-N，並呈現為 RAW，代表它沒有檔案系統。|
|一般行為|NVDIMM-N 會處於唯讀模式。 需要明確的使用者動作，才能再次使用它。|
|儲存空間行為|如果只有一個 NVDIMM 受到影響，儲存空間會維持運作)。<br>NVDIMM-N 實體磁碟物件會顯示 [健全狀況狀態] 為 [狀況不良] 且儲存空間不會使用。|
|其他資訊|PhysicalDisk 物件的 OperationalStatus 欄位。<br>EventLog – Microsoft-Windows-ScmDisk0101/Operational|
|工作|如果使用者不想要更換受影響的裝置，他們可以使用 **Reset-PhysicalDisk** Cmdlet 來清除受影響 NVDIMM-N 的唯讀狀況。 在儲存空間環境中，這也會嘗試將 NVDIMM-N 重新整合至儲存空間，並啟動修復程序。|

## <a name="interleaved-sets"></a>交錯式集合

交錯式集合通常可以在平台的 BIOS 中建立，使多個 NVDIMM-N 裝置向主機作業系統顯示為單一裝置。

Windows Server 2016 和 Windows 10 Anniversary Edition 不支援 NVDIMM-N 的交錯式集合。

在撰寫本文時，還沒有任何機制可讓主機作業系統正確地識別類似集合中的個別 NVDIMM-N，並清楚地告知使用者哪一個特定裝置造成錯誤或需要維修。


