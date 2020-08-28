---
title: fsutil behavior
description: Fsutil behavior 命令的參考文章，可查詢或設定 NTFS 磁片區行為。
manager: dmoss
ms.author: toklima
author: toklima
ms.topic: reference
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 223a326a5ac60dfdb88c20ca3541b0128cf0943e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030166"
---
# <a name="fsutil-behavior"></a>fsutil behavior

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8

查詢或設定 NTFS 磁片區行為，其中包括：

- 建立8.3 個字元長度的檔案名。

- 在 NTFS 磁片區上擴充字元使用於8.3 個字元長度的簡短檔案名。

- 當目錄列在 NTFS 磁片區上時，更新 **上次存取時間** 戳。

- 配額事件寫入系統記錄檔以及 NTFS 分頁集區和 NTFS 未分頁集區記憶體快取層級的頻率。

- 主要檔案資料表區域 (MFT 區域) 的大小。

- 當系統在 NTFS 磁片區上遇到損毀時，無訊息刪除資料。

- 檔案刪除通知 (也稱為修剪或取消對應) 。

## <a name="syntax"></a>語法

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<volumepath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <value> | [<volumepath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <frequency> | symlinkevaluation <symboliclinktype> | disabledeletenotify {1|0}}
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- |------------ |
| 查詢 | 查詢檔案系統行為參數。 |
| set | 變更檔案系統行為參數。 |
| allowextchar `{1|0}` | 允許 (**1**) 或不允許在擴充字元集中使用 (**0**) 字元 (包括在 NTFS 磁片區上的8.3 字元長度短檔案名中使用的變音符號字元) 。<p>您必須重新開機電腦，此參數才會生效。 |
| Bugcheckoncorrupt `{1|0}` | 允許 (**1**) 或不允許在 NTFS 磁片區上發生損毀時， (**0**) 產生 bug 檢查。 這項功能可用來防止 NTFS 在使用自我修復 NTFS 功能時，以無訊息方式刪除資料。<p>您必須重新開機電腦，此參數才會生效。 |
| disable8dot3 [ <volumepath> ] `{1|0}` | 停用 (**1**) 或啟用 (**0**) 在 FAT 和 NTFS 格式的磁片區上建立8.3 個字元長度的檔案名。 （選擇性）以指定為磁片磁碟機名稱的 *volumepath* 前置詞，後面接著冒號或 GUID。 |
| disablecompression `{1|0}` | 停用 ** (1) ** 或啟用 ** (0) ** NTFS 壓縮。<p>您必須重新開機電腦，此參數才會生效。 |
| disablecompressionlimit `{1|0}` | 停用 ** (1) ** 或在 ntfs 磁片區上啟用 ** (0) ** ntfs 壓縮限制。 當壓縮檔案達到特定的片段層級，而不是無法延伸檔案時，NTFS 會停止壓縮檔案的其他範圍。 這麼做是為了讓壓縮的檔案比平常更大。 將此值設定為 **TRUE** 會停用此功能，這項功能會限制系統上壓縮檔案的大小。 我們不建議停用此功能。<p>您必須重新開機電腦，此參數才會生效。 |
| disableencryption `{1|0}` | 停用 ** (1) ** 或啟用 ** (0) ** 在 NTFS 磁片區上加密資料夾和檔案。<p>您必須重新開機電腦，此參數才會生效。 |
| disablefilemetadataoptimization `{1|0}` | 停用 ** (1) ** 或啟用 ** (0) ** 檔案中繼資料優化。 NTFS 對於指定檔案可擁有的範圍數目有限制。 壓縮和稀疏檔案可能會變得非常分散。 根據預設，NTFS 會定期壓縮其內部元資料結構，以允許更多片段化的檔案。 將此值設定為 **TRUE** 會停用此內部優化。 我們不建議停用此功能。<p>您必須重新開機電腦，此參數才會生效。 |
| disablelastaccess `{1|0}` | 停用 (**1**) 或在 NTFS 磁片區上列出目錄時，讓 (**0**) 每個目錄上最後一個存取時間戳的更新。<p>您必須重新開機電腦，此參數才會生效。 |
| disablespotcorruptionhandling `{1|0}` | 停用 ** (1) ** 或啟用 ** (0) ** 找出損毀處理。 也可讓系統管理員執行 CHKDSK 來分析磁片區的狀態，而不讓它離線。 我們不建議停用此功能。<p>您必須重新開機電腦，此參數才會生效。 |
| disabletxf `{1|0}` | 停用 ** (1) ** 或在指定的 NTFS 磁片區上啟用 ** (0) ** txf。 TxF 是 NTFS 功能，可提供檔案系統作業的交易，例如語義。 現在已淘汰 TxF，但仍可使用此功能。 我們不建議您在 C：磁片區上停用這項功能。<p>您必須重新開機電腦，此參數才會生效。 |
| disablewriteautotiering `{1|0}` | 停用階層式磁片區的 ReFS v2 自動分層邏輯。<p>您必須重新開機電腦，此參數才會生效。 |
| encryptpagingfile `{1|0}` | 加密 (**1**) 或不加密 (**0**) Windows 作業系統中的記憶體分頁檔案。<p>您必須重新開機電腦，此參數才會生效。 |
| mftzone `<value>` | 設定 MFT 區域的大小，並以200MB 單位的倍數表示。 將 *值* 設定為 **1** (預設值為 200 MB) 至 **4** (最大值為 800 MB) 。<p>您必須重新開機電腦，此參數才會生效。 |
| memoryusage `<value>` | 設定 NTFS 分頁集區記憶體和 NTFS 非分頁集區記憶體的內部快取層級。 設定為 **1** 或 **2**。 當設定為 **1** (預設) 時，NTFS 會使用預設的分頁集區記憶體量。 當設定為 **2**時，NTFS 會增加其對應清單和記憶體臨界值的大小。  (對應清單是固定大小記憶體緩衝區的集區，核心和裝置驅動程式會建立為檔案系統作業的私用記憶體快取，例如讀取檔案。 ) <p>您必須重新開機電腦，此參數才會生效。 |
| quotanotify `<frequency>` | 設定在系統記錄檔中報告 NTFS 配額違規的頻率。 的有效值為 **0 到 4294967295**的範圍。 預設頻率為 **3600** 秒 (一小時) 。<p>您必須重新開機電腦，此參數才會生效。 |
| symlinkevaluation `<symboliclinktype>` | 控制可在電腦上建立的符號連結類型。 有效的選項包括：<ul><li>**1** -本機至本機符號連結、 `L2L:{0|1}`</li><li>**2** -遠端符號連結的本機 `L2R:{1|0}`</li><li>**3** -遠端至本機符號連結、 `R2R:{1|0}`</li><li>**4** -遠端至遠端符號連結、 `R2L:{1|0}`</li></ul> |
| disabledeletenotify | 停用 (**1**) 或啟用 (**0**) 刪除通知。 刪除通知 (也稱為「修剪」或「取消對應」) 這項功能會通知基礎儲存裝置，因為檔案刪除作業而釋出叢集。 其他情況：<ul><li>針對使用 Ref v2 的系統，預設會停用 trim。</li><li>針對使用 ReFS v1 的系統，預設會啟用 trim。</li><li>若是使用 NTFS 的系統，預設會啟用 trim，除非系統管理員停用它。</li><li>如果您的硬碟或 SAN 報告它不支援 trim，那麼您的硬碟和 San 就不會取得 trim 通知。</li><li>啟用或停用不需要重新開機。</li><li>當下一個取消對應命令發出時，Trim 是有效的。</li><li>登錄變更不會影響現有的傳遞 IO。</li><li>當您啟用或停用 trim 時，不需要重新開機任何服務。</li></ul> |

#### <a name="remarks"></a>備註

- MFT 區域是一個保留區域，可讓主要檔案資料表 (MFT) 視需要展開，以防止 MFT 片段。 如果磁片區上的平均檔案大小為 2 KB 或更小，則將 **mftzone** 值設定為 **2**可能會有説明。 如果磁片區上的平均檔案大小為 1 KB 或更小，則將 **mftzone** 值設定為 **4**可能會有説明。

- 當 **disable8dot3** 設為 **0**時，每次您建立具有長檔名的檔案時，NTFS 會建立第二個具有8.3 字元長度檔案名的檔案專案。 當 NTFS 在目錄中建立檔案時，它必須查閱與長檔名相關聯的8.3 個字元長度的檔案名。 此參數會更新 **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation** 登錄機碼。

- **Allowextchar**參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name**登錄機碼。

- **Disablelastaccess**參數可減少記錄更新到檔案和目錄上**上次存取時間**戳記的影響。 停用 [ **上次存取時間** ] 功能可改善檔案和目錄存取的速度。 此參數會更新 **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate** 登錄機碼。

    **注意：**

    - 即使所有磁片上的值都不是最新的，以檔案為基礎的 **上次存取時間** 查詢都是正確的。 因為正確的值儲存在記憶體中，所以 NTFS 會傳回查詢的正確值。

    - 一小時是 NTFS 可以延遲更新磁片上 **上次存取時間** 的最大時間量。 如果 NTFS 更新其他檔案屬性，例如 **上次修改時間**，而且 **上次存取時間** 更新擱置中，則 NTFS 會以其他更新來更新 **上次存取時間** ，而不會造成額外的效能影響。

    - **Disablelastaccess**參數可能會影響依賴這項功能的程式，例如備份和遠端存放區。

- 增加實體記憶體不一定會增加 NTFS 可用的分頁集區記憶體量。 將 **memoryusage** 設定為 **2** 會提高分頁集區記憶體的限制。 如果您的系統開啟和關閉相同檔案集中的許多檔案，而且尚未針對其他應用程式或快取記憶體使用大量的系統記憶體，這可能會改善效能。 如果您的電腦已經將大量系統記憶體用於其他應用程式或快取記憶體，增加 NTFS 分頁和非分頁集區記憶體的限制，可減少其他進程的可用集區記憶體。 這可能會降低整體系統效能。 此參數會更新 **HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage** 登錄機碼。

- 在 **mftzone** 參數中指定的值是 mft 初始大小與新磁片區上的 mft 區域的近似值，並且會在載入時為每個檔案系統設定。 使用磁片區上的空間時，NTFS 會調整保留給未來 MFT 成長的空間。 如果 MFT 區域已經很大，就不會再保留完整的 MFT 區域大小。 由於 MFT 區域是以超出 MFT 結尾的連續範圍為基礎，因此會在使用空間時壓縮。

    檔案系統在完全使用目前的 MFT 區域之前，並不會判斷新的 MFT 區域位置。 請注意，這絕對不會發生在一般系統上。

- 當刪除通知功能開啟時，某些裝置可能會遇到效能降低的情況。 在此情況下，請使用 **disabledeletenotify** 選項來關閉通知功能。

### <a name="examples"></a>範例

若要查詢 GUID 為 {928842df-5a01-11de-a85c-806e6f6e6963} 所指定之磁片區的停用8.3 名稱行為，請輸入：

```
fsutil behavior query disable8dot3 volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用 **8dot3name** 子命令來查詢8.3 名稱行為。

若要查詢系統以查看是否已啟用 TRIM，請輸入：

```
fsutil behavior query DisableDeleteNotify
```

這會產生類似以下的輸出：

```
NTFS DisableDeleteNotify = 1
ReFS DisableDeleteNotify is not currently set
```

若要覆寫 ReFS v2 (disabledeletenotify) 的預設行為，請輸入：

```
fsutil behavior set disabledeletenotify ReFS 0
```

若要覆寫 TRIM (disabledeletenotify) for NTFS 和 ReFS v1 的預設行為，請輸入：

```
fsutil behavior set disabledeletenotify 1
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [fsutil](fsutil.md)

- [fsutil 8dot3name](fsutil-8dot3name.md)
