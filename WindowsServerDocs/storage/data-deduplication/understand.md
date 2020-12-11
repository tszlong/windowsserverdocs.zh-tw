---
description: 深入瞭解：瞭解重復資料刪除
ms.assetid: acc0803b-fa05-4fc3-b94d-2916abf4fdbd
title: 了解重複資料刪除
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: eac0cf9b658ddc4e0676ed53c48b12e62d617d6f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97050066"
---
# <a name="understanding-data-deduplication"></a>了解重複資料刪除

> 適用于： Windows Server 2019、Windows Server 2016、Windows Server (半年通道) 

本文件說明[重複資料刪除](overview.md)的運作方式。

## <a name="how-does-data-deduplication-work"></a><a name="how-does-dedup-work"></a>重複資料刪除如何運作？

Windows Server 中的重複資料刪除是以下列兩個原則建立：

1. **優化不應以寫入磁片的方式取得** 重復資料刪除會使用後置處理模型來優化資料。 所有資料都會在未經最佳化的情況下寫入磁碟，並在稍後由重複資料刪除最佳化。

2. **優化不應變更存取語義** 存取優化磁片區資料的使用者和應用程式，完全不知道他們所存取的檔案已被重復資料刪除。

針對磁碟區啟用之後，重複資料刪除就會在背景執行，以：

- 找出該磁碟區上檔案的重複模式。
- 使用稱為[重新分析點](#dedup-term-reparse-point)並指向該區塊唯一複本的特殊指標，順暢地移動這些部分或區塊。

這會在下列四個步驟中發生︰

1. 掃描檔案系統以尋找符合最佳化原則的檔案。
![掃描檔案系統](media/understanding-dedup-how-dedup-works-1.gif)
2. 將檔案分成可變更大小的區塊。
![將檔案分成多個區塊](media/understanding-dedup-how-dedup-works-2.gif)
3. 識別唯一的區塊。
![識別唯一的區塊](media/understanding-dedup-how-dedup-works-3.gif)
4. 將區塊放入區塊存放區，並選擇性壓縮。
![移至區塊存放區](media/understanding-dedup-how-dedup-works-4.gif)
5. 使用重新分析點將區塊存放區中的原始檔案資料流取代為現在已最佳化的檔案。
![使用重新分析點取代檔案資料流](media/understanding-dedup-how-dedup-works-5.gif)

讀取最佳化檔案時，檔案系統會使用重新分析點將檔案傳送給重複資料刪除檔案系統篩選器 (Dedup.sys)。 篩選器會將讀取作業重新導向至區塊存放區中構成該檔案資料流的適當區塊。 修改某個範圍內已經過重複資料刪除處理的檔案，會在未經最佳化的情況下寫入磁碟，並在下一次執行[最佳化工作](understand.md#job-info)時進行最佳化。

## <a name="usage-types"></a><a id="usage-type"></a>使用類型
下列使用類型針對一般工作負載提供合理的重複資料刪除功能組態︰

| 使用量類型 | 理想的工作負載 | 不同的地方 |
|------------|-----------------|------------------|
| <a id="usage-type-default"></a>預設 | 一般用途檔案伺服器︰<ul><li>小組共用</li><li>工作資料夾</li><li>資料夾重新導向</li><li>軟體開發共用</li></ul> | <ul><li>背景最佳化</li><li>預設的最佳化原則︰<ul><li>最短檔案存在時間 = 3 天</li><li>最佳化使用中檔案 = 否</li><li>最佳化部分檔案 = 否</li></ul></li></ul> |
| <a id="usage-type-hyperv"></a>Hyper-V | 虛擬桌面基礎結構 (VDI) 伺服器 | <ul><li>背景最佳化</li><li>預設的最佳化原則︰<ul><li>最短檔案存在時間 = 3 天</li><li>最佳化使用中檔案 = 是</li><li>最佳化部分檔案 = 是</li></ul></li><li>Hyper-V Interop 的內部調整</li></ul> |
| <a id="usage-type-backup"></a>備份 | 虛擬化的備份應用程式，例如 [Microsoft Data Protection Manager (DPM) ](/previous-versions/system-center/system-center-2012-R2/hh758173(v=sc.12)) | <ul><li>優先順序最佳化</li><li>預設的最佳化原則︰<ul><li>最短檔案存在時間 = 0 天</li><li>最佳化使用中檔案 = 是</li><li>最佳化部分檔案 = 否</li></ul></li><li>使用 DPM/DPM 型解決方案針對 Interop 的內部調整</li></ul> |

## <a name="jobs"></a><a id="job-info"></a>工作
重複資料刪除會使用後置處理策略來最佳化及維護磁碟區的空間效率。

| 作業名稱 | 工作描述 | 預設排程 |
|----------|------------------|------------------|
| <a id="job-info-optimization"></a>優化 | **優化** 工作的刪除重複方式是根據磁片區原則設定，將磁片區上的資料區塊化， (選擇性地) 壓縮這些區塊，並在區塊存放區中唯一儲存區塊。 重複資料刪除使用的最佳化程序已在[重複資料刪除如何運作？](understand.md#how-does-dedup-work)詳細說明。 | 每小時一次 |
| <a id="job-info-gc"></a>垃圾收集 | **記憶體回收** 工作可透過將最近已修改或刪除的檔案已經不再參考的不必要區塊移除，來回收磁碟空間。 | 每個星期六上午 2:35 |
| <a id="job-info-scrubbing"></a>完整性清除 | **完整性清除** 工作可識別區塊存放區中因為磁碟失敗或磁區損毀所造成的損毀。 可能的話，重複資料刪除可以自動利用磁碟區功能 (例如儲存空間磁碟區上的鏡像或同位檢查) 來重建損毀的資料。 此外，當常用區塊被參考 100 次以上後，重複資料刪除會將它們的備份副本保存在稱為作用區的地方。 | 每個星期六上午 3:35 |
| <a id="job-info-unoptimization"></a>取消最佳化 | **取消最佳化** 是只能手動執行的特殊工作，可以復原重複資料刪除所完成的最佳化，以及停用該磁碟區的重複資料刪除。 | [限依需求](run.md#disabling-dedup) |

## <a name="data-deduplication-terminology"></a><a id="dedup-term"></a>重復資料刪除術語
| 詞彙 | 定義 |
|------|------------|
| <a id="dedup-term-chunk"></a>區塊 | 區塊是已由重複資料刪除區塊化演算法選取的檔案區塊，就像其他類似檔案會發生的一樣。 |
| <a id="dedup-term-chunk-store"></a>區塊存放區 | 區塊存放區是 [系統磁碟區資訊] 資料夾中有組織的一系列容器檔案，重複資料刪除會用來儲存唯一的區塊。 |
| <a id="dedup-term-dedup"></a>Dedup | 重複資料刪除的縮寫，一般會在 PowerShell、Windows Server API 和元件，以及 Windows Server 社群中使用。 |
| <a id="dedup-term-file-metadata"></a>檔案中繼資料 | 每個檔案都會包含中繼資料，說明與檔案有關但與檔案主要內容無關的有趣屬性。 例如，建立日期、上次讀取日期、作者等。 |
| <a id="dedup-term-file-stream"></a>檔案資料流 | 檔案資料流是檔案的主要內容。 這是重複資料刪除執行最佳化的檔案部分。 |
| <a id="dedup-term-file-system"></a>檔案系統 | 檔案系統是儲存媒體上的軟體與磁碟上資料結構，讓作業系統用來在儲存媒體上儲存檔案。 NTFS 格式化磁碟區支援重複資料刪除。 |
| <a id="dedup-term-file-system-filter"></a>檔案系統篩選器 | 檔案系統篩選器是修改檔案系統預設行為的外掛程式。 為了保留存取語意，重複資料刪除會使用檔案系統篩選器 (Dedup.sys) 將讀取重新導向至已經完全最佳化，且可供提出讀取要求的使用者或應用程式讀取的內容。 |
| <a id="dedup-term-optimization"></a>優化 | 如果檔案已經過區塊處理且其唯一區塊已經儲存在區塊存放區中，重複資料刪除會認為檔案已最佳化 (或是已進行重複資料刪除)。 |
| <a id="dedup-term-in-policy"></a>最佳化原則 | 最佳化原則會指定要將哪些檔案視為應該要進行重複資料刪除。 例如，如果檔案是全新、已開啟、位於磁碟區上的特定路徑或特定檔案類型，就會被視為是違反原則。 |
| <a id="dedup-term-reparse-point"></a>重新分析點 | 重新 [分析點](/windows/win32/fileio/reparse-points) 是特殊標籤，會通知檔案系統將 i/o 傳遞給指定的檔案系統篩選器。 當檔案的檔案資料流已最佳化時，重複資料刪除會以重新分析點取代檔案資料流，這可以讓重複資料刪除保留對該檔案的存取語意。 |
| <a id="dedup-term-volume"></a>磁碟區 | 磁碟區是邏輯儲存體磁碟機的 Windows 建構，可以會跨一或多部伺服器上的多個實體存放裝置。 重複資料刪除是以磁碟區為依據，在磁碟區上啟用。 |
| <a id="dedup-term-workload"></a>工作負載 | 工作負載是在 Windows Server 上執行的應用程式。 範例工作負載包括一般用途的檔案伺服器、Hyper-V 和 SQL Server。 |

> [!Warning]
> 除非已獲授權的 Microsoft 支援人員另有指示，否則請勿嘗試以手動方式修改區塊存放區。 因為這樣做可能會導致資料損毀或遺失。

## <a name="frequently-asked-questions"></a>常見問題集
**重複資料刪除和其他最佳化產品有何不同？**
重複資料刪除和其他常見的存放裝置最佳化產品之間有幾個重要差異︰

* *重複資料刪除和儲存單一版本有何不同？*
    儲存單一版本 (或 SIS) 是重複資料刪除的前身技術，於 Windows Storage Server 2008 R2 中首度引進。 為了將磁碟區最佳化，儲存單一版本已識別完全相同的檔案，並以 SIS 一般存放區中所儲存之單一檔案複本的邏輯連結取代那些檔案。 不同於儲存單一版本，重複資料刪除可以從沒有完全相同但是共用許多常見模式的檔案，以及本身包含許多重複模式的檔案，取得空間以節省空間。 儲存單一版本已在 Windows Server 2012 R2 中淘汰，並在 Windows Server 2016 中移除以改用重複資料刪除。

* *重複資料刪除和 NTFS 壓縮有何不同？*
    NTFS 壓縮是一項 NTFS 功能，可於磁碟區層級選擇性啟用。 使用 NTFS 壓縮，每個檔案都會在寫入時透過壓縮來個別最佳化。 不同於 NTFS 壓縮，重複資料刪除可以在磁碟區上的所有檔案間取得空間以節省空間。 這一點優於 NTFS 壓縮，因為檔案可能會<u>同時</u>有內部重複資料刪除 (由 NTFS 壓縮解決) 並且與磁碟區上的其他檔案有相似點 (不會由 NTFS 壓縮解決)。 此外，重複資料刪除擁有後置處理模組，這表示新檔案或修改過的檔案 都會以未最佳化的方式寫入磁碟，之後再由重複資料刪除進行最佳化。

* *重復資料刪除與封存檔案格式（例如 zip、rar、.7z、cab 等）有何不同？*
    zip、rar、7z、cab 等封存檔案格式是對一組指定檔案執行壓縮。 與重複資料刪除類似，會最佳化檔案內的重複模式與跨檔案的重複模式。 不過，您必須選擇要包含在封存中的檔案。 存取語意也會不同。 若要存取封存內的特定檔案，您必須開啟封存，並選取特定檔案，然後解壓縮該檔案以供使用。 重複資料刪除會針對使用者與系統管理員，以透明的方式運作，且不需要任何手動執行。 此外，重複資料刪除會保留存取語意：已最佳化的檔案會在最佳化之後維持不變。

**我可以針對我所選的使用類型，變更重複資料刪除設定嗎？**
是。 雖然重複資料刪除會針對 **建議的工作負載** 提供合理的預設值，但是您仍然可能會想要調整重複資料刪除設定，以充分利用您的存放裝置。 此外，其他工作負載將[需要一些調整，以確保重複資料刪除不會干擾工作負載](install-enable.md#enable-dedup-sometimes-considerations)。

**我可以透過手動方式執行重複資料刪除工作嗎？**
可以，[所有的重複資料刪除工作都可以手動執行](run.md#running-dedup-jobs-manually)。 如果排程的工作因為系統資源不足，或是因為發生錯誤而未執行，就可能需要手動執行。 此外，取消最佳化工作僅能手動執行。

**我可以監視重複資料刪除工作的歷程記錄結果嗎？**
可以，[所有的重複資料刪除工作都會在 Windows 事件日誌中產生項目](run.md#monitoring-dedup)。

**我可以針對我系統上的重複資料刪除工作變更預設排程嗎？**
可以，[所有的排程都可以設定](advanced-settings.md#modifying-job-schedules)。 建議您最好修改預設的重複資料刪除排程，以確保重複資料刪除工作有足夠時間完成，且不會與工作負載競用資源。
