---
title: AD 效能微調中的硬體考慮
description: AD 效能微調中的硬體考慮
ms.topic: article
ms.author: timwi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: b01df0d8bcef7dc49ca1a27833abb1022fcf0d90
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077346"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>新增效能微調中的硬體考慮

>[!Important]
> 以下是針對 [Active Directory Domain Services 文章容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566) 中更深入瞭解 Active Directory 工作負載的重要建議和考慮的摘要。 我們強烈建議讀者複習 [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566) ，以獲得更好的技術理解以及這些建議的含意。

## <a name="avoid-going-to-disk"></a>避免進入磁片

Active Directory 快取與記憶體允許的相同數量的資料庫。 如果媒體是以主軸或 SSD 為基礎，從記憶體中提取頁面的速度會比前往實體媒體更快。 新增更多的記憶體，以將磁片 i/o 最小化。

-   Active Directory 最佳作法建議將足夠的 RAM 載入至記憶體中，然後容納作業系統和其他已安裝的應用程式，例如防毒軟體、備份軟體、監視等等。

    -   如需舊版平臺的限制，請參閱執行 [Windows Server 2003 或 windows 2000 Server 之網域控制站上的 Lsass.exe 進程的記憶體使用量](https://support.microsoft.com/kb/308356)。

    -   使用「記憶體 \\ 長期平均待命快取存留期 (s) &gt; 30 分鐘」效能計數器。

-   將作業系統、記錄檔和資料庫放在不同的磁片區上。 如果所有或大部分的 DIT 都可以快取，則在準備就緒快取並處於穩定狀態時，這會變得較不相關，並在儲存體配置中提供更大的彈性。 在無法快取整個 DIT 的案例中，將作業系統、記錄檔和資料庫分割在不同磁片區上的重要性將變得更重要。

-   一般而言，對 DIT 的 i/o 比率是大約90% 的讀取和10% 寫入。 寫入 i/o 磁片區大幅超過 10%-20% 的案例會被視為寫入繁重。 大量寫入案例不會大幅受益于 Active Directory 快取。 為了保證寫入目錄之資料的交易持久性，Active Directory 不會執行磁片寫入快取。 相反地，它會在傳回作業的成功完成狀態之前認可磁片的所有寫入作業，除非有明確的要求不這麼做。 因此，快速磁片 i/o 對 Active Directory 寫入作業的效能很重要。 以下是可能改善這些案例效能的硬體建議：

    -   硬體 RAID 控制器

    -   增加裝載 DIT 和記錄檔的低延遲/高 RPM 磁片數目

    -   在控制器上寫入快取

-   針對每個磁片區個別檢查磁片子系統的效能。 大部分的 Active Directory 案例主要是以讀取為基礎，因此裝載 DIT 的磁片區上的統計資料是最重要的檢查。 不過，請勿忽略監視其餘的磁片磁碟機，包括作業系統和記錄檔磁片磁碟機。 若要判斷是否已正確設定網域控制站，以避免儲存體成為效能的瓶頸，請參考儲存子系統上的一節，以取得標準儲存體建議。 在許多環境中，原理是要確保有足夠的前端空間來容納突然或負載尖峰。 這些臨界值是警告閾值，其中可容納尖峰或負載尖峰的空間會變得受限，且用戶端回應會降低。 簡單來說，在一天的時間內，超過這些臨界值並不是 (5 至15分鐘的) ，不過，使用這類統計資料持續執行的系統並不會完全快取資料庫，且應該進行調查。

    -   Database = = &gt; (lsass/NTDSA 的實例) \\ I/o 資料庫讀取平均延遲 &lt; 15ms

    -   Database = = &gt; 實例 (lsass/NTDSA) \\ I/o 資料庫讀取/秒 &lt; 10

    -   Database = = &gt; 實例 (lsass/NTDSA) \\ i/o 記錄檔寫入平均延遲 &lt; 10 毫秒

    -   Database = = &gt; 實例 (lsass/NTDSA) \\ I/o Log Writes/sec –僅供參考。

        為了維持資料的一致性，所有變更都必須寫入記錄檔中。 這裡沒有任何良好或不良的數位，它只是儲存體所支援的量值。

-   針對非尖峰載入期間，規劃非核心磁片 i/o 負載，例如備份和防病毒掃描。 此外，您也可以使用支援 Windows Server 2008 中的低優先順序 i/o 功能的備份與防毒軟體解決方案，以 Active Directory 的 i/o 需求減少競爭。

## <a name="dont-over-tax-the-processors"></a>處理器不含稅

沒有足夠的可用迴圈的處理器可能會導致等候執行緒進入處理器執行的等候時間很長。 在許多環境中，原理是要確保有足夠的前端空間來容納尖峰或負載尖峰，以將在這些案例中對用戶端回應的影響降到最低。 簡單來說，在一天的時間內，超過下列臨界值並不是 (5 至15分鐘的) ，不過使用這些統計資料來持續執行的系統不會提供任何現成的空間來容納異常的負載，而且可以輕鬆地放入大量的案例中。 您應調查閾值超過閾值的系統，以瞭解如何減少處理器負載。

-   如需如何選取處理器的詳細資訊，請參閱 [伺服器硬體的效能微調](../../hardware/index.md)。

-   新增硬體、優化負載、將用戶端導向至其他位置，或從環境中移除負載以降低 CPU 負載。

-   使用處理器資訊 (\_ Total) \\ % Processor 使用率 &lt; 60% 效能計數器。

## <a name="avoid-overloading-the-network-adapter"></a>避免網路介面卡超載

就像處理器一樣，過度的網路介面卡使用率將會導致輸出流量等候更長的等候時間，以連到網路。 Active Directory 通常會有少量的輸入要求，且相對於傳回給用戶端系統的資料量會大幅增加。 傳送的資料遠超過接收的資料。 在許多環境中，原理是要確保有足夠的前端空間來容納突然或負載尖峰。 此臨界值是一種警告閾值，其中用於容納尖峰或負載尖峰的空間會變得受限，且用戶端回應會降低。 簡單地說，在一天的時間內，超過這些臨界值並不是 (5 至15分鐘的) ，不過，使用這些統計資料來持續執行的系統會過度使用，因此應該加以調查。

-   如需如何微調網路子系統的詳細資訊，請參閱 [網路子系統的效能微調](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md)。

-   使用「比較 NetworkInterface (\*) \\ 傳送的位元組數/秒」與「 \* 目前的頻寬效能計數器」 NetworkInterface () \\ 。 比例應小於使用的60%。

## <a name="additional-references"></a>其他參考資料
- [Active Directory 伺服器的效能調整](index.md)
- [LDAP 考量](ldap-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md)
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)