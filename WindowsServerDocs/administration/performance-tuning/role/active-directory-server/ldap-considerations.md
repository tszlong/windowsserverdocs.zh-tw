---
title: 中的 LDAP 考慮新增效能微調
description: Active Directory 工作負載中的 LDAP 考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: timwi; chrisrob; herbertm; kenbrumf;  mleary; shawnrab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 2ef32b379dcc5d1c2d8217564b639f44d024e5ee
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471544"
---
# <a name="ldap-considerations-in-adds-performance-tuning"></a>中的 LDAP 考慮新增效能微調

> [!IMPORTANT]
> 以下是針對 Active Directory Domain Services 文章的[容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)中更深入的 Active Directory 工作負載優化伺服器硬體的重要建議和考慮的摘要。 我們強烈鼓勵讀者複習[Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)，以獲得更深入的技術理解及這些建議的影響。

## <a name="verify-ldap-queries"></a>確認 LDAP 查詢

確認 LDAP 查詢符合建立有效率的查詢建議。

MSDN 上有廣泛的檔，說明如何適當地撰寫、結構和分析查詢以用於 Active Directory。 如需詳細資訊，請參閱[建立更有效率的 Microsoft Active Directory 啟用應用程式](https://msdn.microsoft.com/library/ms808539.aspx)。

## <a name="optimize-ldap-page-sizes"></a>優化 LDAP 頁面大小

當傳回具有多個物件的結果以回應用戶端要求時，網域控制站必須將結果集暫時儲存在記憶體中。 增加頁面大小會導致更多的記憶體使用量，而且可能會不必要地將專案保留在快取中。 在此情況下，預設設定是最佳的。 在數個案例中，我們提出了建議來增加頁面大小設定。 我們建議使用預設值，除非明確識別為不足夠。

當查詢有許多結果時，可能會遇到類似查詢同時執行的限制。  這是因為 LDAP 伺服器可能會耗盡稱為「cookie 集區」的全域記憶體區域。  可能需要增加集區的大小，如[如何處理 LDAP 伺服器 cookie](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/manage/how-ldap-server-cookies-are-handled)中所述。

若要調整這些設定，請參閱[Windows Server 2008 和更新版本的網域控制站只會在 LDAP 回應中傳回5000值](https://support.microsoft.com/kb/2009267)。

## <a name="determine-whether-to-add-indices"></a>判斷是否要加入索引

在篩選準則中搜尋具有屬性名稱的物件時，索引屬性會很有用。 編制索引可以減少評估篩選時必須造訪的物件數目。 不過，這會降低寫入作業的效能，因為修改或加入對應的屬性時，必須更新索引。 它也會增加目錄資料庫的大小，不過這些優點通常會超過儲存體的成本。 記錄可以用來尋找成本高昂且效率不佳的查詢。 一旦識別出來，請考慮對對應查詢中使用的某些屬性編制索引，以改善搜尋效能。 如需有關 Active Directory 如何搜尋工作的詳細資訊，請參閱[Active Directory 搜尋如何工作](https://technet.microsoft.com/library/cc755809.aspx)。

### <a name="scenarios-that-benefit-in-adding-indices"></a>可受益于新增索引的案例

-   要求資料的用戶端負載會產生大量的 CPU 使用量，而且用戶端查詢行為無法變更或優化。 藉由大量負載，請考慮它本身會顯示在 Server Performance Advisor 的前10個入侵者清單中，或內建的 Active Directory 資料收集器集合中，並使用超過1% 的 CPU。

-   用戶端負載因為未編制索引的屬性而在伺服器上產生大量的磁片 i/o，而且無法變更或優化用戶端查詢行為。

-   查詢會花很長的時間，而且因為缺乏涵蓋索引，而無法在可接受的時間範圍內完成用戶端。

- 具有高持續時間的大量查詢，會導致使用 ATQ LDAP 執行緒的耗用量和耗盡。 監視下列效能計數器：

    - **NTDS \\要求延遲**–這取決於要求處理所需的時間長度。 在120秒（預設值）之後，Active Directory 會超時要求，但大部分的執行速度應該會更快，而且在整體的數位中應該會隱藏長時間執行的查詢。 尋找此基準中的變更，而不是絕對臨界值。

        > [!NOTE]
        > 這裡的高值也可能是對其他網域和 CRL 檢查的「代理」要求延遲的指標。

    - **NTDS \\估計的佇列延遲**–在理想的情況下，這應該會接近0以達到最佳效能，這表示要求沒有時間等待服務。

您可以使用下列一或多種方法來偵測這些案例：

-   [使用統計資料控制項判斷查詢時間](https://msdn.microsoft.com/library/ms808539.aspx)

-   [追蹤昂貴且效率不佳的搜尋](https://msdn.microsoft.com/library/ms808539.aspx)

-   Active Directory 效能監視器中的診斷資料收集器集合工具（[SPA 的子物： Win2008 和其他的 AD 資料收集器集合](https://blogs.technet.com/b/askds/archive/2010/06/08/son-of-spa-ad-data-collector-sets-in-win2008-and-beyond.aspx)）

-   [Microsoft Server Performance Advisor](../../../server-performance-advisor/microsoft-server-performance-advisor.md)Active Directory Advisor 套件

-   使用 "（objectClass =）" 以外的任何篩選準則搜尋 \* ，其使用祖系索引。

### <a name="other-index-considerations"></a>其他索引考慮

-   確定在微調查詢已用盡為選項之後，建立索引是問題的正確解決方案。 適當地調整硬體大小十分重要。 只有在適當的修正是要為屬性編制索引，而不是嘗試模糊處理硬體問題時，才應新增索引。

-   索引會增加資料庫的大小，其最小值為要編制索引之屬性的總大小。 因此，可以藉由採用屬性中資料的平均大小，並乘以將填入屬性的物件數目，來評估資料庫成長的估計。 這通常大約是資料庫大小增加1%。 如需詳細資訊，請參閱[資料存放區的運作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜尋行為主要是在組織單位層級完成，請考慮編制容器化搜尋的索引。

-   元組索引比一般索引大，但估計大小的難度就愈多。 使用一般索引大小估計值作為成長的樓層，最大值為20%。 如需詳細資訊，請參閱[資料存放區的運作方式](https://technet.microsoft.com/library/cc772829.aspx)。

-   如果搜尋行為主要是在組織單位層級完成，請考慮編制容器化搜尋的索引。

-   需要元組索引，才能支援中詞搜尋字串和最終搜尋字串。 初始搜尋字串不需要元組索引。

    -   初始搜尋字串–（samAccountName = MYPC \* ）

    -   中詞搜尋字串-（samAccountName = \* MYPC \* ）

    -   最終搜尋字串–（samAccountName = \* MYPC $）

-   建立索引時將會產生磁片 i/o。 這會在優先順序較低的背景執行緒上完成，傳入要求會優先于索引組建。 如果環境的容量規劃已正確完成，這應該是透明的。 不過，大量寫入案例或網域控制站儲存體上負載不明的環境可能會降低用戶端體驗，而且應該在數小時後完成。

-   對複寫流量的影響很小，因為建立索引會在本機發生。

如需詳細資訊，請參閱下列各項：

-   [建立更有效率的 Microsoft Active Directory 啟用應用程式](https://msdn.microsoft.com/library/ms808539.aspx)

-   [在 Active Directory Domain Services 中搜尋](https://msdn.microsoft.com/library/aa746427.aspx)

-   [已編制索引的屬性](https://msdn.microsoft.com/library/windows/desktop/ms677112.aspx)

## <a name="additional-references"></a>其他參考

- [Active Directory 伺服器的效能調整](index.md)
- [硬體考量](hardware-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md)
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)