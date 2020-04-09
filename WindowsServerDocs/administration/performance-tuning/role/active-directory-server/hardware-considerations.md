---
title: AD 效能微調中的硬體考慮
description: AD 效能微調中的硬體考慮
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: timwi; chrisrob; herbertm; kenbrumf;  mleary; shawnrab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: c40faca06668adf6fd29a5e4e753e5790b8104b7
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851911"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>中的硬體考慮新增效能微調 

>[!Important]
> 以下是針對 Active Directory Domain Services 文章的[容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)中更深入的 Active Directory 工作負載優化伺服器硬體的重要建議和考慮的摘要。 我們強烈鼓勵讀者複習[Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)，以獲得更深入的技術理解及這些建議的影響。

## <a name="avoid-going-to-disk"></a>避免進入磁片

Active Directory 會在記憶體允許的情況中快取大部分的資料庫。 從記憶體中提取分頁的速度，比進入實體媒體更快，不論媒體是以主軸還是 SSD 為基礎。 新增更多記憶體以將磁片 i/o 降至最低。

-   Active Directory 最佳做法建議放入足夠的 RAM，將整個 DIT 載入記憶體中，並配合作業系統和其他已安裝的應用程式，例如防毒軟體、備份軟體、監視等等。

    -   如需舊版平臺的限制，請參閱在執行[Windows Server 2003 或 Windows 2000 伺服器的網域控制站上，lsass.exe 進程的記憶體使用量](https://support.microsoft.com/kb/308356)。

    -   使用 記憶體\\長期待命快取存留期 &gt; 30 分鐘 效能計數器。

-   將作業系統、記錄和資料庫放在不同的磁片區上。 如果可以快取所有或大部分的 DIT，一旦快取準備就緒並處於穩定狀態，這就會變得比較不相關，並在儲存體配置中提供更大的彈性。 在無法快取整個 DIT 的案例中，將作業系統、記錄和資料庫分割在不同的磁片區上的重要性變得更重要。

-   一般來說，DIT 的 i/o 比率大約是90% 的讀取和10% 的寫入。 寫入 i/o 磁片區明顯超過 10%-20% 的案例會視為大量寫入。 大量寫入的案例不會大幅受益于 Active Directory 快取。 為保證寫入目錄之資料的交易持久性，Active Directory 不會執行磁片寫入快取。 相反地，它會在傳回作業的成功完成狀態之前，先認可磁片的所有寫入作業，除非有明確的要求不會這麼做。 因此，快速磁片 i/o 對於 Active Directory 寫入作業的效能很重要。 以下是可能會改善這些案例效能的硬體建議：

    -   硬體 RAID 控制器

    -   增加裝載 DIT 和記錄檔的低延遲/高 RPM 磁片數目

    -   在控制器上寫入快取

-   個別檢查每個磁片區的磁片子系統效能。 大部分的 Active Directory 案例主要是以讀取為基礎，因此裝載 DIT 的磁片區上的統計資料是最重要的檢查。 不過，請勿忽略監視其餘的磁片磁碟機，包括作業系統和記錄檔磁片磁碟機。 若要判斷是否已正確設定網域控制站，以避免存放裝置成為效能瓶頸，請參考儲存子系統上的一節，以取得標準儲存體建議。 在許多環境中，其原理在於確保有足夠的前端空間來容納負載中的電湧或尖峰。 這些臨界值是警告臨界值，可容納尖峰或負載突然增加的空間會受到限制，而且用戶端回應會降低。 簡言之，在短期內，超過這些臨界值並不會有任何錯誤（5到15分鐘一天），不過，使用這些統計資料持續執行的系統並不會完全快取資料庫，而且可能會被調查。

    -   Database = =&gt; 實例（lsass/NTDSA）\\i/o 資料庫讀取平均延遲 &lt; 15ms

    -   Database = =&gt; 實例（lsass/NTDSA）\\i/o 資料庫讀取/秒 &lt; 10

    -   Database = =&gt; 實例（lsass/NTDSA）\\i/o 記錄寫入平均延遲 &lt; 10 毫秒

    -   Database = =&gt; 實例（lsass/NTDSA）\\i/o 記錄檔寫入/秒–僅供參考。

        為了維持資料的一致性，所有變更都必須寫入記錄檔。 此處沒有任何好或壞的數位，只是儲存體所支援的量值。

-   針對非尖峰負載期間，規劃非核心磁片 i/o 負載，例如備份和防毒軟體掃描。 此外，使用備份和防毒軟體解決方案，支援 Windows Server 2008 中引進的低優先順序 i/o 功能，以降低對 Active Directory 的 i/o 需求的競賽。

## <a name="dont-over-tax-the-processors"></a>不要對處理器進行稅金

沒有足夠的可用週期的處理器，可能會導致在對處理器取得執行緒的等候時間很長，而無法執行。 在許多環境中，其原理是要確保有足夠的前端空間來容納激增或負載突然增加，以在這些情況下將對用戶端回應的影響降至最低。 簡單地說，在短期內超過下列臨界值不會很錯誤（一天5到15分鐘的時間），不過，使用這些統計資料持續執行的系統並不會提供任何標頭空間來容納異常負載，而且可以輕鬆地放入過度計的案例中。 系統會將超過閾值的持續時間進行調查，以瞭解如何減少處理器負載。

-   如需如何選取處理器的詳細資訊，請參閱[伺服器硬體的效能微調](../../hardware/index.md)。

-   新增硬體、將負載優化、將用戶端導向其他地方，或從環境中移除負載以降低 CPU 負載。

-   使用 [處理器資訊（\_總計）]\\[% Processor 使用率] &lt; 60% 效能計數器。

## <a name="avoid-overloading-the-network-adapter"></a>避免多載網路介面卡

就像處理器一樣，過多的網路介面卡使用率會導致輸出流量長時間等候網路。 Active Directory 通常會有小型的輸入要求，而相對於傳回給用戶端系統的資料量會大幅增加。 傳送的資料遠超過接收的資料。 在許多環境中，其原理在於確保有足夠的前端空間來容納負載中的電湧或尖峰。 此臨界值是一項警告臨界值，其中的標頭空間會受到限制，而且用戶端回應會降低。 簡單地說，在短期內，超過這些臨界值並不會有太多錯誤（5到15分鐘一天執行一次），不過，使用這些統計資料持續執行的系統會過度計，而且應該進行調查。

-   如需如何微調網路子系統的詳細資訊，請參閱[網路子系統的效能微調](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md)。

-   使用 [比較 NetworkInterface （\*）\\Bytes Sent/Sec] 與 [NetworkInterface （\*）]\\[目前的頻寬] 效能計數器。 比率應小於使用的60%。

## <a name="see-also"></a>另請參閱
- [Active Directory 伺服器的效能微調](index.md)
- [LDAP 考量](ldap-considerations.md)
- [適當地放置網域控制站與站台考量](site-definition-considerations.md)
- [針對 ADDS 效能問題進行疑難排解](troubleshoot.md) 
- [Active Directory Domain Services 的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)