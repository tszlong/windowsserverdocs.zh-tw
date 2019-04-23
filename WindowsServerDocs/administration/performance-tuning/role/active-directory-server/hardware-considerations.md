---
title: 在 AD 效能微調的硬體考量
description: 在 AD 效能微調的硬體考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: TimWi; ChrisRob; HerbertM; KenBrumf;  MLeary; ShawnRab
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 0f1aa1e3c07c5cb9238a332156abfec248e74176
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866089"
---
# <a name="hardware-considerations-in-adds-performance-tuning"></a>在 ADDS 效能微調的硬體考量 

>[!Important]
> 以下是金鑰的建議和考量，來最佳化 Active Directory 中的更深入的討論的工作負載的伺服器硬體的摘要[Active Directory 網域服務的容量計劃](https://go.microsoft.com/fwlink/?LinkId=324566)文章。 讀取器會檢閱我們極力[Active Directory 網域服務的容量計劃](https://go.microsoft.com/fwlink/?LinkId=324566)更高的技術知識和這些建議的影響。

## <a name="avoid-going-to-disk"></a>避免在兩到磁碟

因為記憶體可讓 active Directory 會快取的資料庫。 從記憶體擷取頁面是倍的速度比媒體是磁針還是 SSD 為基礎的實體媒體上，依序。 新增更多記憶體到磁碟 I/O 最小化。

-   Active Directory 的最佳做法，建議將足夠的 RAM 整個 DIT 載入記憶體，再加上容納作業系統和其他已安裝的應用程式，例如防毒、 備份軟體來監視、 等等。

    -   如需舊版的平台的限制，請參閱[Lsass.exe 程序，在執行 Windows Server 2003 或 Windows 2000 Server 的網域控制站上的記憶體使用量](https://support.microsoft.com/kb/308356)。

    -   使用記憶體\\長期平均待機快取存留期 （秒） &gt; 30 分鐘的時間效能計數器。

-   將作業系統、 記錄和資料庫放在不同的磁碟區上。 如果要將所有或大部分的 DIT 可以快取，當準備快取，而且在穩定狀態下，這會變成較不相關，並提供較多的彈性，在儲存體配置。 在無法快取整個 DIT 的情況下，作業系統、 記錄和個別磁碟區上的資料庫進行分割的重要性就變得更重要。

-   一般來說，要 DIT 的 I/O 比例是大約 90%讀取和寫入 10%。 寫入 I/O 數量大幅超過 10%-20%的案例會視為寫入繁重。 大量寫入的情況下不會大大受益於 Active Directory 快取。 若要保證寫入目錄資料的交易持久性，Active Directory 不會執行磁碟寫入快取。 相反地，它會認可到磁碟的所有寫入作業之前它會傳回成功的完成狀態作業中，除非有明確要求不要這樣做。 因此，快速磁碟 I/O 是 Active directory 的寫入作業的效能很重要的。 以下是可能會改善效能，這些案例的硬體建議：

    -   硬體 RAID 控制器

    -   增加低延遲/高 RPM 裝載的 DIT 和記錄檔的磁碟的數目

    -   寫入快取的控制站上

-   檢閱個別為每個磁碟區的磁碟子系統效能。 大部分的 Active Directory 案例主要以讀取為基礎，因此裝載 DIT 的磁碟區上的統計資料是最重要檢查。 不過，請勿忽略監視磁碟機，包括作業系統在內的其餘部分和記錄檔磁碟機。 若要判斷網域控制站已正確設定以避免儲存體正在效能的瓶頸，請參考標準儲存體建議在存放子系統上的區段。 在許多環境中，原理是以確保前端的空間可容納激增或尖峰負載。 這些閾值會警告臨界值的預留空間以容納激增或大量載入的位置會變成限制，並將用戶端回應速度降低。 簡單地說，超過這些閾值不在短期內 （5 到 15 分鐘一天多次） 不正確，但執行持續使用這種統計資料的系統不完整快取資料庫和，可能會耗盡，應該加以調查。

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Database Reads Averaged Latency &lt; 15ms

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Database Reads/sec &lt; 10

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Log Writes Averaged Latency &lt; 10ms

    -   Database ==&gt; Instances(lsass/NTDSA)\\I/O Log Writes/sec – informational only.

        若要維持資料一致性，所有的變更必須寫入記錄檔。 不正確或不正確數字這裡，它是在支援儲存體數量的量值。

-   規劃非核心磁碟 I/O 負載，例如，備份與防毒軟體掃描，以非尖峰負載時段列入。 此外，使用備份與防毒支援引入的低優先順序 I/O 功能來減少 I/O 需求，Active Directory 的競賽的 Windows Server 2008 中的解決方案。

## <a name="dont-over-tax-the-processors"></a>不超過稅務處理器

沒有足夠的循環的處理器可能造成長時間的等待時間取得入處理器的執行緒執行。 在許多環境中，原理是為了確保前端的空間可容納激增或尖峰負載，在這些案例中的用戶端回應能力的影響降到最低。 簡單地說，超過閾值以下不是錯誤在短期內 （5 到 15 分鐘為一天多次），不過執行持續使用這種統計資料的系統並不提供任何前端容納異常負載騰出空間，並可以輕鬆地將放入使用量情況下得逞 s所在的案例。 消費臨界值的持續的時間的系統應該調查如何降低處理器負載。

-   如需如何選取 [處理器] 的詳細資訊，請參閱[伺服器硬體的效能微調](../../hardware/index.md)。

-   新增硬體、 最佳化的負載，將其他位置，用戶端導向或移除環境，以降低 CPU 負載的負載。

-   使用的處理器資訊 (\_總計)\\%Processor Utilization &lt; 60%的效能計數器。

## <a name="avoid-overloading-the-network-adapter"></a>避免多載的網路介面卡

就像處理器，過多的網路介面卡使用情形會導致等候時間很長的連上網路的輸出流量。 Active Directory 常會有小型的傳入的要求並相當明顯較大量的資料傳回給用戶端系統。 傳送的資料遠超過接收的資料。 在許多環境中，原理是以確保前端的空間可容納激增或尖峰負載。 此臨界值是預留空間以容納激增或尖峰負載的其中的警告臨界值會變成限制，並將用戶端回應速度降低。 簡單地說，超過這些閾值不在短期內 （5 到 15 分鐘一天多次） 不正確，但執行持續使用這種統計資料的系統是透過情況下得逞，應該加以調查。

-   如需如何調整網路子系統的詳細資訊，請參閱[網路子系統的效能微調](../../../../networking/technologies/network-subsystem/net-sub-performance-top.md)。

-   使用比較網路介面 (\*)\\Bytes Sent/Sec 與網路介面 (\*)\\目前頻寬的效能計數器。 比例應該小於 60%時。

## <a name="see-also"></a>另請參閱
- [效能微調 Active Directory 伺服器](index.md)
- [LDAP 的考量](ldap-considerations.md)
- [獲得正確位置的網域控制站和站台考量](site-definition-considerations.md)
- [ADDS 效能疑難排解](troubleshoot.md) 
- [Active Directory 網域服務的容量規劃](https://go.microsoft.com/fwlink/?LinkId=324566)