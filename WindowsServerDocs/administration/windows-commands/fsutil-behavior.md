---
title: fsutil 行為
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 3ad6ad8d31bbcb4e9c70192efda37a9836eac14b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377019"
---
# <a name="fsutil-behavior"></a>fsutil 行為

>適用於：Windows Server （半年通道）、Windows Server 2016、Windows 10、Windows Server 2012 R2、Windows 8.1、Windows Server 2012、Windows 8、Windows Server 2008 R2、Windows 7

查詢或設定 NTFS 磁片區行為，其中包括：

-   建立8.3 字元長度的檔案名

-   NTFS 磁片區上8.3 字元長度短檔案名中使用的擴充字元

-   在 NTFS 磁片區上列出目錄時，更新上次存取時間戳記

-   將配額事件寫入系統記錄和 NTFS 分頁集區和 NTFS 非分頁集區記憶體快取層級的頻率

-   主要檔案表格區域的大小（MFT 區域）

-   當系統在 NTFS 磁片區上遇到損毀時，無訊息刪除資料。

-   檔案刪除通知（也稱為修剪或取消對應）

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>參數

|                 參數                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|--------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                   query                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            查詢檔案系統行為參數。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                    設定                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            變更檔案系統行為參數。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|          allowextchar {1&#124;0}           |                                                                                                                                                                                                                                                                                                                                                                                                                                  允許（**1**）或禁止在 NTFS 磁片區上的8.3 字元長度短檔案名中使用擴充字元集（包括變音符號字元）的字元（**0**）。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|        Bugcheckoncorrupt {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                      允許（**1**）或不允許（**0**）在 NTFS 磁片區上發生損毀時產生錯誤檢查。 這項功能可在與自我修復 NTFS 功能搭配使用時，用來防止 NTFS 以無訊息方式刪除資料。<br /><br />您必須重新開機電腦，此參數才會生效。<br /><br />此參數適用于：Windows Server 2008 R2 和 Windows 7。                                                                                                                                                                                                                                                                                                                                                                      |
|   disable8dot3 [<VolumePath>] {1&#124;0}   |                                                                                                                                                                                                                                                                                                                                                                                                                                                       停用（**1**）或啟用（**0**）在 FAT 和 NTFS 格式的磁片區上建立8.3 字元長度的檔案名。 （選擇性）將*VolumePath*指定為磁片磁碟機名稱，後面接著冒號或 GUID 的前置詞。                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|       disablecompression {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 停用 **（1）** 或啟用 **（0）** NTFS 壓縮。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|     disablecompressionlimit {1&#124;0}     |                                                                                                                                                                                                                                                                                   在 NTFS 磁片區上停用 **（1）** 或啟用 **（0）** ntfs 壓縮限制。 當壓縮檔案達到特定層級的片段時，NTFS 會停止壓縮檔案的其他範圍，而不是擴充檔案。 這麼做是為了讓壓縮檔案比平常一般。 將此值設定為 [TRUE] 會停用此功能，以限制系統上的壓縮檔案大小。 我們不建議您停用這項功能。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                   |
|        disableencryption {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                停用 **（1）** 或啟用 **（0）** 在 NTFS 磁片區上加密資料夾和檔案。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| disablefilemetadataoptimization {1&#124;0} |                                                                                                                                                                                                                                                                                                                      停用 **（1）** 或啟用 **（0）** 檔案中繼資料優化。 NTFS 對於指定檔案可以擁有的範圍數目有限制。 壓縮和備用檔案可能會變得非常分散。 根據預設，NTFS 會定期壓縮其內部的元資料結構，以允許更多的片段化檔案。 將此值設定為 TRUE 會停用此內部優化。 我們不建議您停用這項功能。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                       |
|        disablelastaccess {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                       停用（**1**）或啟用（**0**）在 NTFS 磁片區上列出目錄時，對每個目錄上的最後一個存取時間戳記記進行更新。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|  disablespotcorruptionhandling {1&#124;0}  |                                                                                                                                                                                                                                                                                                                                                    停用 **（1）** 或啟用 **（0）** 點損毀處理。 Windows 8 和 Windows Server 2012 引進的其中一項新功能是新的高可用性格式的 CHKDSK。 這項功能可讓系統管理員執行 CHKDSK 來分析磁片區的狀態，而不需要讓它離線。 我們不建議您停用這項功能。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                    |
|           disabletxf {1&#124;0}            |                                                                                                                                                                                                                                                                                                                                                                         在指定的 NTFS 磁片區上停用 **（1）** 或啟用 **（0）** txf。 TxF 是一項 NTFS 功能，可提供檔案系統作業的語義之類的交易。 雖然此功能仍然可用，但目前已 deprected TxF。 我們不建議您在 C：磁片區上停用這項功能。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                          |
|     disablewriteautotiering {1&#124;0}     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                停用分層式磁片區的 ReFS v2 自動分層邏輯。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|        encryptpagingfile {1&#124;0}        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                          加密（**1**）或不加密（**0**） Windows 作業系統中的記憶體分頁檔案。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|              mftzone <Value>               |                                                                                                                                                                                                                                                                                                                                                                                                                                           設定 MFT 區域的大小，並以 200 MB 單位的倍數表示。 將*值*設為**1** （預設值為 200 mb）到**4** （最大值為 800 MB）的數位。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|            memoryusage <Value>             |                                                                                                                                                                                                                                                                                   設定 NTFS 分頁集區記憶體和 NTFS 未分頁集區記憶體的內部快取層級。 設定為**1**或**2**。 當設定為**1** （預設值）時，NTFS 會使用分頁集區記憶體的預設數量。 當設定為**2**時，NTFS 會增加其對應清單和記憶體臨界值的大小。 （對應清單是一種固定大小的記憶體緩衝區集區，核心和設備磁碟機會建立為檔案系統作業的私用記憶體快取，例如讀取檔案）。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                   |
|          quotanotify <Frequency>           |                                                                                                                                                                                                                                                                                                                                                                                                                                  設定在系統記錄中報告 NTFS 配額違規的頻率。 的有效值位於0–4294967295的範圍內。 預設頻率為3600秒（一小時）。<br /><br />您必須重新開機電腦，此參數才會生效。                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|    symlinkevaluation <SymbolicLinkType>    |                                                                                                                                                                                                                                                                                                                                                                                                   控制可在電腦上建立的符號連結種類。 有效的選項包括：<br /><br />1.本機符號連結，L2L： {0&#124;1}<br />2.本機到遠端符號連結，L2R： {0&#124;1}<br />3.遠端至本機符號連結，R2R： {0&#124;1}<br />4.遠端到遠端符號連結，R2L： {0&#124;1}                                                                                                                                                                                                                                                                                                                                                                                                   |
|            DisableDeleteNotify             | 停用 \(**1**\)，或啟用 \(**0**\) 刪除通知。 刪除通知 \(also 稱為 trim 或取消對應 @ no__t-1 是一項功能，它會通知基礎儲存裝置，該叢集已因檔案刪除作業而釋放。 其他情況：<br /><br />-對於使用 ReFS v2 的系統，預設會停用 trim。 適用于 Windows Server 2016。<br />-若是使用 ReFS v1 的系統，則預設會啟用 trim。 適用于 Windows Server 2012、Windows Server 2012 R2 和 Windows Server 2016。<br />-若是使用 NTFS 的系統，預設會啟用 trim，除非管理員停用它。<br />-如果您的硬碟機或 SAN 報告不支援修剪，則您的硬碟和 San 不會取得修剪通知。<br />-啟用或停用不需要重新開機。<br />-Trim 會在發出下一個取消對應命令時生效。<br />-現有的傳遞 IO 不會受到登錄變更的影響。<br />-當您啟用或停用 trim 時，不需要重新開機任何服務。<br /><br />此參數是在 Windows Server 2008 R2 和 Windows 7 中引進。 |

## <a name="remarks"></a>備註

-   MFT 區域是一個保留的區域，可讓主要檔案資料表（MFT）視需要擴充，以防止 MFT 分散。 如果磁片區上的平均檔案大小為 2 KB 或更小，則將**mftzone**值設定為2可能會有説明。 如果磁片區上的平均檔案大小為 1 KB 或更小，則將**mftzone**值設定為4會很有説明。

-   當**disable8dot3**設定為**0**時，每次您建立具有長檔名的檔案時，NTFS 會建立具有8.3 字元長度檔案名的第二個檔案專案。 當 NTFS 在目錄中建立檔案時，它必須查閱與長檔名相關聯的8.3 字元長度檔案名。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation**登錄機碼。

-   **Allowextchar**參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name**登錄機碼。

-   **Disablelastaccess**參數會降低將更新記錄到檔案和目錄上的上次存取時間戳記的影響。 停用 [**上次存取時間**] 功能可改善檔案和目錄存取的速度。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate**登錄機碼。

    注意：

    -   以檔案為基礎的**上次存取時間**查詢是正確的，即使所有磁片上的值都不是最新的。 NTFS 會針對查詢傳回正確的值，因為正確的值會儲存在記憶體中。

    -   [一小時] 是 NTFS 延遲更新磁片上**上次存取時間**的最大時間量。 如果 NTFS 更新其他的檔案屬性，例如 [**上次修改時間**]，而 [**上次存取時間**] 更新為 [擱置中]，則 NTFS 會更新**上次存取時間**與其他更新，而不會產生額外的效能影響。

    -   **Disablelastaccess**參數可能會影響依賴這項功能的程式，例如備份和遠端存放。

-   增加實體記憶體並不一定會增加 NTFS 可用的分頁集區記憶體數量。 將**memoryusage**設定為**2**會引發分頁集區記憶體的限制。 如果您的系統開啟和關閉相同檔案集中的許多檔案，而且尚未針對其他應用程式或快取記憶體使用大量的系統記憶體，這可能會改善效能。 如果您的電腦已針對其他應用程式或快取記憶體使用大量的系統記憶體，增加 NTFS 分頁和非分頁集區記憶體的限制，可減少其他進程的可用集區記憶體。 這可能會降低整體系統效能。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage**登錄機碼。

-   在**mftzone**參數中指定的值是 MFT 的初始大小和新磁片區上的 mft 區域的近似值，而且會在掛接時針對每個檔案系統設定。 使用磁片區上的空間時，NTFS 會針對未來的 MFT 成長調整保留的空間。 如果 MFT 區域已經很大，則不會再次保留完整的 MFT 區域大小。 由於 MFT 區域是根據 MFT 結尾的連續範圍而定，因此會隨著使用的空間而縮小。

    在完全使用目前的 MFT 區域之前，檔案系統不會判斷新的 MFT 區域位置。 請注意，這永遠不會發生在一般系統上。

-   當 [刪除通知] 功能開啟時，某些裝置可能會遇到效能降低的情況。 在此情況下，請使用**disabledeletenotify**選項來關閉通知功能。

### <a name="BKMK_examples"></a>典型
若要針對以 GUID {928842df-5a01-11de-a85c-806e6f6e6963} 指定的磁片區查詢停用的8dot3 名稱行為，請輸入：

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用**8dot3name**子命令來查詢8dot3 名稱行為。

若要查詢系統以查看是否已啟用 TRIM，請輸入：

```
fsutil behavior query DisableDeleteNotify
```
這會產生類似以下的輸出：

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

若要覆寫 TRIM v2 \(disabledeletenotify @ no__t-1 的預設行為，請輸入：

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

若要覆寫 TRIM \(disabledeletenotify @ no__t-1 for NTFS 和 ReFS v1 的預設行為，請輸入：
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)


