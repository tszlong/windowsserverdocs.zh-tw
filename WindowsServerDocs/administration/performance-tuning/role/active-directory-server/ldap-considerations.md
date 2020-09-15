---
title: 新增效能微調中的 LDAP 考慮
description: Active Directory 工作負載中的 LDAP 考慮
ms.topic: article
ms.author: timwi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: dec345b872b6a87d7d0a1414aef6fe9b6da39cc5
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077357"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>新增效能微調中的 LDAP 考慮

> [!IMPORTANT]
> 以下是針對 [Active Directory Domain Services 文章容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566) 中更深入瞭解 Active Directory 工作負載的重要建議和考慮的摘要。 我們強烈建議讀者複習 [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566) ，以獲得更好的技術理解以及這些建議的含意。

## <a name="verify-ldap-queries"></a>確認 LDAP 查詢

確認 LDAP 查詢符合建立有效率的查詢建議。

MSDN 上有廣泛的檔，說明如何適當地撰寫、結構和分析用於 Active Directory 的查詢。 如需詳細資訊，請參閱 [建立更有效率的 Microsoft Active Directory 啟用的應用程式](/previous-versions/ms808539(v=msdn.10))。

## <a name="optimize-ldap-page-sizes"></a>優化 LDAP 頁面大小

以多個物件傳回結果以回應用戶端要求時，網域控制站必須暫時將結果集儲存在記憶體中。 增加頁面大小會造成更多的記憶體使用量，而且可能會不必要地將專案存留在快取中。 在此情況下，預設設定是最佳的。 有幾個案例會提出建議來增加頁面大小設定。 除非明確識別為不足夠，否則建議使用預設值。

當查詢有許多結果時，可能會遇到類似查詢同時執行的限制。  發生這種情況的原因是 LDAP 伺服器可能會耗盡稱為 cookie 集區的全域記憶體區域。  您可能必須增加集區的大小，如 [LDAP 伺服器 Cookie 的處理方式](../../../../identity/ad-ds/manage/how-ldap-server-cookies-are-handled.md)中所述。

若要調整這些設定，請參閱 [Windows Server 2008 和更新版本的網域控制站只會在 LDAP 回應中傳回5000值](https://support.microsoft.com/kb/2009267)。

## <a name="determine-whether-to-add-indices"></a>判斷是否要新增索引

在篩選準則中搜尋具有屬性名稱的物件時，索引屬性會很有用。 編制索引可以減少評估篩選時必須造訪的物件數目。 不過，這會降低寫入作業的效能，因為在修改或加入對應的屬性時，必須更新索引。 它也會增加目錄資料庫的大小，但優點通常會超過儲存體的成本。 記錄可以用來尋找昂貴且效率不佳的查詢。 一旦識別之後，請考慮為對應查詢中使用的一些屬性編制索引，以改善搜尋效能。 如需 Active Directory 搜尋如何運作的詳細資訊，請參閱 [Active Directory 搜尋的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc755809(v=ws.10))。

### <a name="scenarios-that-benefit-in-adding-indices"></a>加入索引的優點

-   要求資料的用戶端負載會產生大量的 CPU 使用量，且用戶端查詢行為無法變更或優化。 藉由大量負載，請考慮它會在伺服器效能建議程式中的前10個入侵清單中顯示自己，或內建的 Active Directory 資料收集器集合，且使用超過1% 的 CPU。

-   由於未編制索引的屬性，用戶端負載會在伺服器上產生大量的磁片 i/o，而且用戶端查詢行為無法變更或優化。

-   因為缺少涵蓋索引，所以查詢需要很長的時間，而且未在可接受的時間範圍內完成。

- 大量查詢有很長的持續時間，會導致 ATQ LDAP 執行緒耗用和耗盡。 監視下列效能計數器：

    - **NTDS \\要求延遲** -這取決於要求處理所花費的時間長度。 Active Directory 在120秒後的要求超時 (預設) ，但大部分的執行速度應該會在整體的數位中隱藏，而且執行速度很長的查詢應該會隱藏起來。 尋找此基準的變更，而不是絕對臨界值。

        > [!NOTE]
        > 這裡的高值也可以是對其他網域和 CRL 檢查的「proxy」要求中的延遲指標。

    - **NTDS \\估計的佇列延遲** ：在理想的情況下，這應該會接近0，以達到最佳效能，這表示要求不需要等待維修。

您可以使用下列一種或多種方法來偵測這些案例：

-   [使用統計資料控制項判斷查詢計時](/previous-versions/ms808539(v=msdn.10))

-   [追蹤昂貴且效率不佳的搜尋](/previous-versions/ms808539(v=msdn.10))

-   Active Directory 內建效能監視器 (中的診斷資料收集器集合工具 [： Win2008 中的 AD 資料收集器集合工具，以及以外的](/archive/blogs/askds/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond)) 

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md) Active Directory Advisor 套件

-   除了 " (objectClass =) " 之外，使用上階索引來搜尋任何篩選準則 \* 。

### <a name="other-index-considerations"></a>其他索引考慮

-   確定在微調查詢已用盡為選項之後，建立索引是解決問題的正確解決方案。 適當地調整硬體大小相當重要。 只有當正確的修正程式是為屬性編制索引，而不是嘗試模糊化硬體問題時，才應該加入索引。

-   索引會增加資料庫的大小，最小值為所編制索引之屬性的總大小。 因此，您可以將資料的平均大小和將填入屬性的物件數目相乘，來評估資料庫成長的估計。 一般來說，這會增加資料庫大小的1%。 如需詳細資訊，請參閱 [資料存放區的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc772829(v=ws.10))。

-   如果搜尋行為主要是在組織單位層級完成，請考慮為容器化搜尋編制索引。

-   元組索引大於一般索引，但較難預估大小。 使用一般索引大小估計作為成長的樓層，最多可達20%。 如需詳細資訊，請參閱 [資料存放區的運作方式](/previous-versions/windows/it-pro/windows-server-2003/cc772829(v=ws.10))。

-   如果搜尋行為主要是在組織單位層級完成，請考慮為容器化搜尋編制索引。

-   需要元組索引，才能支援中詞搜尋字串和最終的搜尋字串。 初始搜尋字串不需要元組索引。

    -   初始搜尋字串– (samAccountName = MYPC \*) 

    -   中搜尋字串- (samAccountName = \* MYPC \*) 

    -   最終搜尋字串– (samAccountName = \* MYPC $) 

-   建立索引時，會在建立索引時產生磁片 i/o。 這會在優先順序較低的背景執行緒上完成，而傳入的要求則會優先于索引建立。 如果已正確地完成環境的容量規劃，這應該是透明的。 但是，如果是不知道網域控制站儲存體負載的大量寫入案例或環境，則可能會降低用戶端體驗，而且應該在幾小時內完成。

-   對複寫流量的影響很低，因為建立索引會在本機進行。

如需詳細資訊，請參閱下列內容：

-   [建立更有效率的 Microsoft Active Directory 啟用應用程式](/previous-versions/ms808539(v=msdn.10))

-   [在 Active Directory Domain Services 中搜尋](/windows/win32/ad/searching-in-active-directory-domain-services)

-   [索引屬性](/windows/win32/ad/indexed-attributes)

## <a name="additional-references"></a>其他參考資料

- [Active Directory 伺服器的效能調整](index.md)
- [硬體考量](hardware-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md)
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)