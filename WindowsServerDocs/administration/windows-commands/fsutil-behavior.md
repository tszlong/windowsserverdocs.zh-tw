---
title: fsutil 行為
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.assetid: 84eaba2c-c0af-49e1-bbbd-2ed2928e5e4b
ms.openlocfilehash: 4593739f25c356e72ea39947c67f3e1301573137
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838269"
---
# <a name="fsutil-behavior"></a>fsutil 行為

>適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows 10，Windows Server 2012 R2、 Windows 8.1、 Windows Server 2012 中，Windows 8、 Windows Server 2008 R2、 Windows 7

查詢或設定 NTFS 磁碟區行為，包括：

-   建立 8.3 的字元長度的檔案名稱

-   NTFS 磁碟區上的 8.3 字元長度短檔名中的擴充的字元使用

-   當目錄會列在 NTFS 磁碟區的上次存取時間戳記更新

-   使用哪一個配額事件會寫入至系統記錄檔和 ntfs 的頻率分頁集區和 NTFS 非分頁集區記憶體快取層級

-   主檔案表格區域 （MFT 區域） 的大小

-   無訊息刪除時，系統遇到損毀 NTFS 磁碟區的資料。

-   檔案刪除通知 （也稱為修剪或取消對應）

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
fsutil behavior query {allowextchar | bugcheckoncorrupt | disable8dot3 [<VolumePath>] | disablecompression | disablecompressionlimit | disableencryption | disablefilemetadataoptimization | disablelastaccess | disablespotcorruptionhandling | disabletxf | disablewriteautotiering | encryptpagingfile | mftzone | memoryusage | quotanotify | symlinkevaluation | disabledeletenotify}

fsutil behavior set {allowextchar {1|0} | bugcheckoncorrupt {1|0} | disable8dot3 [ <Value> | [<VolumePath> {1|0}] ] | disablecompression {1|0} | disablecompressionlimit {1|0} | disableencryption {1|0} | disablefilemetadataoptimization {1|0} | disablelastaccess {1|0} | disablespotcorruptionhandling {1|0} | disabletxf {1|0} | disablewriteautotiering {1|0} | encryptpagingfile {1|0} | mftzone <Value> | memoryusage <Value> | quotanotify <Frequency> | symlinkevaluation <SymbolicLinkType> | disabledeletenotify {1|0}}
```

## <a name="parameters"></a>參數

|參數|描述|
|-------------|---------------|
|查詢|查詢的檔案系統行為的參數。|
|設定|變更的檔案系統行為的參數。|
|allowextchar {1&#124;0}|允許 (**1**) 或不允許 (**0**) 擴充字元集的字元是使用 NTFS 磁碟區上的 8.3 字元長度短檔名中設定 （包括變音符號字元）。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|Bugcheckoncorrupt {1&#124;0}|允許 (**1**) 或不允許 (**0**) 產生的 NTFS 磁碟區上的損毀時的錯誤檢查。 這項功能可用來防止 NTFS 以無訊息方式刪除資料和自我修復 NTFS 功能一起使用時。<br /><br />您必須重新啟動您的電腦，此參數才會生效。<br /><br />此參數適用於：Windows Server 2008 R2 和 Windows 7。|
|disable8dot3 [<VolumePath>] {1&#124;0}|停用 (**1**) 或啟用 (**0**) 建立 8.3 FAT 與 NTFS 格式化磁碟區上的字元長度的檔案名稱。 （選擇性） 加上前置詞*VolumePath*指定為磁碟機名稱，後面接著冒號或 GUID。|
|disablecompression {1&#124;0}|停用 **(1)** 或啟用 **(0)** NTFS 壓縮。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disablecompressionlimit {1&#124;0}| 停用 **(1)** 或啟用 **(0)** NTFS 磁碟區上的 NTFS 壓縮限制。 當壓縮的檔案達到特定層級的分散程度，而不是無法延伸檔案時，就會停止 NTFS 壓縮檔案的額外延伸區。 這麼做是為了允許比平常更大的壓縮的檔案。 將此值設定為 TRUE 會停用此功能可限制大小的壓縮檔案系統上。 我們不建議停用此功能。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disableencryption {1&#124;0}|停用 **(1)** 或啟用 **(0)** 的資料夾和檔案在 NTFS 磁碟區加密。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disablefilemetadataoptimization {1&#124;0}|停用 **(1)** 或啟用 **(0)** 檔案中繼資料最佳化。 NTFS 會限制指定的檔案可以有多少的範圍。 壓縮和備援檔案可能會變得嚴重片段化。 根據預設，NTFS 會定期壓縮它的內部中繼資料結構，以允許更分散的檔案。 將此值設定為 TRUE 會停用此內部最佳化。 我們不建議停用此功能。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disablelastaccess {1&#124;0}|停用 (**1**) 或啟用 (**0**) 時，更新最後一個每個目錄的存取時間戳記目錄會列在 NTFS 磁碟區。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disablespotcorruptionhandling {1&#124;0}|停用 **(1)** 或啟用 **(0)** 找出損毀處理。 在 Windows 8 和 Windows Server 2012 中引進的新功能之一是 CHKDSK 的新高可用性形式。 這項功能可讓系統管理員執行 CHKDSK 分析不需要離線的磁碟區的狀態。 我們不建議停用此功能。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disabletxf {1&#124;0}|停用 **(1)** 或啟用 **(0)** txf 在指定的 NTFS 磁碟區上。 TxF 是一項 NTFS 功能，提供檔案系統作業的語意類似的交易。 TxF 是目前 deprected，雖然功能仍然可用。 我們不建議停用此功能，在 c： 磁碟區上。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|disablewriteautotiering {1&#124;0}|停用階層式磁碟區的 ReFS v2 自動階層處理邏輯。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|encryptpagingfile {1&#124;0}|加密 (**1**) 或不加密 (**0**) 之 Windows 作業系統中的記憶體分頁檔。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|mftzone <Value>|設定 [MFT] 區域的大小，並會表示為 200 MB 單位的倍數。 設定*值*從數字**1** （預設值為 200 MB） 至**4** （最大值為 800 MB）。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|memoryusage <Value>|設定 NTFS 分頁集區記憶體和 NTFS 非分頁集區記憶體內部快取層級。 設定為**1**或是**2**。 當設定為**1** （預設）、 NTFS 會使用預設的分頁集區記憶體量。 當設定為**2**，NTFS 會增加記憶體臨界值，其對應清單的大小。 （當對應清單是核心與裝置驅動程式建立私用記憶體快取的檔案系統作業，例如讀取檔案的固定大小的記憶體緩衝區的集區）。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|quotanotify <Frequency>|設定 NTFS 配額違規系統記錄檔中報告的頻率。 有效值介於 0 – 4294967295。 預設頻率為 3600 秒 （1 小時）。<br /><br />您必須重新啟動您的電腦，此參數才會生效。|
|symlinkevaluation <SymbolicLinkType>|控制可以在電腦建立的符號連結的類型。 有效選項包括：<br /><br />1.本機至本機符號連結，L2L: {0&#124;1}<br />2.本機到遠端符號連結，L2R: {0&#124;1}<br />3.遠端至本機符號連結，R2R: {0&#124;1}<br />4.遠端至遠端符號連結，R2L: {0&#124;1}|
|DisableDeleteNotify|停用\( **1** \)或啟用\( **0** \)刪除通知。 刪除通知\(也稱為修剪或取消對應\)是一項功能，會通知基礎儲存裝置的叢集，已經釋出因為檔案刪除作業。 其他情況：<br /><br />-若為使用 ReFS 系統預設會停用 v2，修剪。 適用於 Windows Server 2016。<br />-若為使用 ReFS 系統預設會啟用 v1，修剪。 適用於 Windows Server 2012、 Windows Server 2012 R2 和 Windows Server 2016。<br />-若為使用 NTFS 系統，trim 會啟用根據預設，除非系統管理員停用它。<br />-如果您的硬碟機或 SAN 報告，它不支援修剪，則您的硬碟機和 San 不會收到修剪通知。<br />-啟用或停用不需要重新啟動。<br />-下一個取消對應的命令時，才有效 trim 發出。<br />存在登錄的變更不會影響 IO 的進行。<br />-當您啟用或停用修剪，不需要任何重新啟動服務。<br /><br />此參數是在 Windows Server 2008 R2 和 Windows 7 中引進。 | 

## <a name="remarks"></a>備註

-   MFT 區域是保留的區域，可讓主檔案表格 (MFT) 依需要擴展以防止 MFT 分散。 如果磁碟區上的平均檔案大小為 2 KB 或更少，可以是設定有幫助**mftzone**為 2 的值。 如果磁碟區上的平均檔案大小為 1 KB 或更少，可以是設定有幫助**mftzone**為 4 的值。

-   當**disable8dot3**設為**0**每次您建立的檔案具有長檔名、 NTFS 會建立具有字元長度的 8.3 檔案名稱的第二個檔案項目。 當 NTFS 的目錄中建立檔案時，它必須查閱 8.3 長檔名與相關聯的字元長度的檔案名稱。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisable8dot3NameCreation**登錄機碼。

-   **Allowextchar**參數更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsAllowExtendedCharacterIn8dot3Name**登錄機碼。

-   **Disablelastaccess**參數會減少檔案和目錄上的上次存取時間戳記的記錄更新的影響。 停用**上次存取時間**功能改進的檔案和目錄的存取速度。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsDisableLastAccessUpdate**登錄機碼。

    注意：

    -   檔案為基礎**上次存取時間**查詢是否正確，即使目前沒有到磁碟上的所有值。 NTFS 上查詢傳回正確的值，因為正確的值會儲存在記憶體中。

    -   一小時是 NTFS 會延後更新的時間的最大數量**上次存取時間**磁碟上。 如果 NTFS 這類更新其他檔案屬性**上次修改時間**，以及**上次存取時間**更新正在等待，NTFS 更新**上次存取時間**與其他更新，而不需要其他的效能影響。

    -   **Disablelastaccess**參數可能會影響依賴此功能，例如備份和遠端存放裝置程式。

-   增加實體記憶體不一定會增加分頁集區可用的記憶體數量為 NTFS。 設定**memoryusage**要**2**引發的分頁集區記憶體限制。 如果您的系統開啟和關閉同一個檔案中的許多檔案設定，而且未已經使用大量的系統記憶體或快取記憶體的其他應用程式，這可能會改善效能。 如果您的電腦已使用大量的系統記憶體或快取記憶體的其他應用程式，增加的限制，NTFS 的分頁和非分頁集區的記憶體會降低其他處理序的可用的集區記憶體。 這可能會降低整體系統效能。 此參數會更新**HKLM\SYSTEM\CurrentControlSet\Control\FileSystem\NtfsMemoryUsage**登錄機碼。

-   中指定的值**mftzone**參數的 MFT 加上新的磁碟區上的 MFT 區域的初始大小近似值而設定在每個檔案系統掛接階段。 使用磁碟區的空間時，NTFS 會調整保留供未來的 MFT 成長的空間。 如果 MFT 區域已經很大，完整的 MFT 區域大小未保留一次。 MFT 區域為基礎的 MFT 結尾的連續範圍，因為它會壓縮，在使用的空間。

    檔案系統不會判斷新的 MFT 區域位置，直到目前的 MFT 區域已完全使用。 請注意，這永遠不會在一般的系統上。

-   刪除通知功能開啟時，某些裝置可能會遇到效能降低。 在此案例中，使用**disabledeletenotify**選擇關閉 「 通知 」 功能。

### <a name="BKMK_examples"></a>範例
若要查詢的磁碟區 GUID，{928842df-5a01-11de-a85c-806e6f6e6963}，使用指定的停用 8.3 名稱行為中，輸入：

```
fsutil behavior query disable8dot3 Volume{928842df-5a01-11de-a85c-806e6f6e6963}
```

您也可以使用查詢的 8.3 名稱行為**8dot3name**子命令。

若要查詢的系統，以查看 TRIM 是否已啟用，請輸入：

```
fsutil behavior query DisableDeleteNotify
```
這會產生類似下面的輸出：

    NTFS DisableDeleteNotify = 1
    ReFS DisableDeleteNotify is not currently set

若要覆寫預設行為，TRIM \(disabledeletenotify\) ReFS v2 中，輸入：

```
fsutil behavior set DisableDeleteNotify ReFS 0
```

若要覆寫預設行為，TRIM \(disabledeletenotify\) NTFS 和 ReFS v1 中，輸入：
```
fsutil behavior set DisableDeleteNotify 1
```

#### <a name="additional-references"></a>其他參考資料
[命令列語法關鍵](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)

[Fsutil 8dot3name](Fsutil-8dot3name.md)


