---
title: 您可以在 ADDS 效能微調的 LDAP 考量
description: 您可以在 Active Directory 工作負載的 LDAP 考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 7ac9453159fe97dc15ecbb2ab858214664a2a197
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811532"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>您可以在 ADDS 效能微調的 LDAP 考量

> [!IMPORTANT]
> 以下是金鑰的建議和考量，來最佳化 Active Directory 中的更深入的討論的工作負載的伺服器硬體的摘要[Active Directory 網域服務的容量計劃](https://go.microsoft.com/fwlink/?LinkId=324566)文章。 讀取器會檢閱我們極力[Active Directory 網域服務的容量計劃](https://go.microsoft.com/fwlink/?LinkId=324566)更高的技術知識和這些建議的影響。

## <a name="verify-ldap-queries"></a>確認 LDAP 查詢

確認 LDAP 查詢會符合建立有效率的查詢建議。

沒有關於如何正確撰寫，結構，並分析查詢用於 Active Directory 在 MSDN 上的大量文件。 如需詳細資訊，請參閱 < [Creating More Efficient Microsoft Active Directory-Enabled 應用程式](https://msdn.microsoft.com/library/ms808539.aspx)。

## <a name="optimize-ldap-page-sizes"></a>最佳化 LDAP 頁面大小

當用戶端要求的回應中傳回多個物件的結果，網域控制站必須暫時儲存記憶體中的結果集。 增加的頁面大小會導致更多的記憶體使用量，且不必要地 age 出快取項目。 在此情況下，預設設定是最佳的。 有數個案例的建議已對增加的頁面大小設定。 我們建議使用預設值，除非明確地辨識為不適當。

當查詢有許多的結果時，可能會遇到類似的查詢同時執行的限制。  LDAP 伺服器可能會耗盡稱為 cookie 集區的全域記憶體區域會發生這個問題。  您可能必須增加集區的大小中所述[LDAP 伺服器 Cookie 處理方式](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled)。

若要調整這些設定，請參閱[Windows Server 2008 及更新版本的網域控制站的 LDAP 回應中傳回只 5000 值](https://support.microsoft.com/kb/2009267)。

## <a name="determine-whether-to-add-indices"></a>判斷是否要加入索引

編排屬性索引時，搜尋篩選條件中有屬性名稱的物件。 編製索引，可以減少必須瀏覽時評估篩選條件的物件數目。 不過，這降低寫入作業的效能，因為修改或新增對應的屬性時，就必須更新索引。 但優點通常比儲存體的成本，它也會增加目錄資料庫的大小。 記錄可用來尋找昂貴且效率不佳的查詢。 一旦識別出，建議您索引中對應的查詢來提升搜尋效能的一些屬性。 如需 Active Directory 搜尋的運作方式的詳細資訊，請參閱[Active Directory 搜尋的運作方式](https://technet.microsoft.com/library/cc755809.aspx)。

### <a name="scenarios-that-benefit-in-adding-indices"></a>加入索引中獲益的案例

-   在要求資料的用戶端負載會產生大量的 CPU 使用量，且無法變更或最佳化用戶端查詢行為。 藉由大量負載，請考慮它會顯示本身 Server Performance Advisor 或內建 Active Directory 資料收集器集合中的前 10 個 offender 清單中，並使用多個 CPU 的 1%。

-   用戶端負載會產生大量的磁碟 I/O，由於未編製索引的屬性在伺服器上，且無法變更或最佳化用戶端查詢行為。

-   查詢花費很長的時間，而且缺乏涵蓋索引尚未完成在用戶端，因為可接受時間範圍內。

- 耗用量和 ATQ LDAP 執行緒已耗盡，導致大量高的持續時間的查詢。 監視下列效能計數器：

    - **NTDS\\要求延遲**– 這是受限於多久要求所需程序。 Active Directory 逾時要求之後 120 秒 （預設值），不過，大部分應該執行速度更快並極長時間執行的查詢應該取得隱藏在整體的數字。 尋找此基準，而不是絕對臨界值的變更。

        > [!NOTE]
        > 這裡最高值也可以要求其他的網域和 CRL 檢查的"proxy"延遲的指標。

    - **NTDS\\估計佇列延遲**– 這在理想情況下應該接近 0，以獲得最佳效能，這表示要求花費沒有正在等待的時間。

這些案例，可以使用一或多個下列方法來偵測：

-   [判斷與統計資料控制項的查詢執行時間](https://msdn.microsoft.com/library/ms808539.aspx)

-   [追蹤昂貴且效率不佳的搜尋](https://msdn.microsoft.com/library/ms808539.aspx)

-   在效能監視器中設定 active Directory 診斷資料收集器 ([SPA 的兒子：將 AD 資料收集器集合工具在 Win2008 和更新版本](http://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx))

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Active Directory Advisor 套件

-   搜尋使用以外的任何篩選條件 」 (objectClass =\*) 」，使用祖系的索引。

### <a name="other-index-considerations"></a>其他索引考量

-   請確定建立索引正確解決問題之後已用盡微調查詢的選項。 適當地調整硬體是非常重要。 只有在適當的修正方法是索引屬性，且不嘗試將模糊化硬體問題時，才應該加入索引。

-   索引會將資料庫的大小增加最少屬性編製索引的總大小。 估計資料庫的成長可以因此來評估屬性中取得資料的平均大小，然後乘以必須填入屬性的物件數目。 通常這是相關的資料庫大小增加 1%。 如需詳細資訊，請參閱 <<c0> [ 資料存放區的運作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜尋行為，主要是在組織單位層級，請考慮進行容器化搜尋編製索引。

-   Tuple 索引大於正常的索引，但它是很難估計大小。 作為最低限度值的大小估計的標準索引成長，最多為 20%。 如需詳細資訊，請參閱 <<c0> [ 資料存放區的運作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜尋行為，主要是在組織單位層級，請考慮進行容器化搜尋編製索引。

-   支援中間的搜尋字串和最後一個搜尋字串時，所需 Tuple 索引。 初始搜尋字串不需要 Tuple 索引。

    -   初始搜尋字串-(samAccountName = MYPC\*)

    -   中間的搜尋字串-(samAccountName =\*MYPC\*)

    -   最後一個搜尋字串-(samAccountName =\*MYPC$)

-   正在建立索引時，建立索引將會產生磁碟 I/O。 這是具有低優先順序在背景執行緒上，連入要求將會優先於索引建置。 如果已正確完成環境的容量規劃，這應該十分簡單。 不過，寫入大量案例或未知的網域控制站儲存體上的負載的環境可能會降低用戶端體驗，而且應該完成離峰時間。

-   因為建立索引，就會發生在本機，複寫流量的影響非常少。

如需詳細資訊，請參閱下列各項：

-   [建立更有效率 Microsoft Active Directory 功能的應用程式](https://msdn.microsoft.com/library/ms808539.aspx)

-   [在 Active Directory 網域服務中搜尋](https://msdn.microsoft.com/library/aa746427.aspx)

-   [索引的屬性](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="see-also"></a>另請參閱

- [效能微調 Active Directory 伺服器](index.md)
- [硬體考量](hardware-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md) 
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)