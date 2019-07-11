---
title: Active Directory 網域服務的容量規劃
description: 深入的討論要在 AD DS 的容量規劃考量的因素。
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: 5a9e2d39d4eedd1e8fdb4bfeaf267ad4cb4c596a
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799830"
---
# <a name="capacity-planning-for-active-directory-domain-services"></a>Active Directory 網域服務的容量規劃

本主題最初作者 Ken Brumfield，microsoft 資深工程師，並提供容量規劃 Active Directory 網域服務 (AD DS) 的建議。

## <a name="goals-of-capacity-planning"></a>容量規劃的目標

容量計劃不相同的效能事件疑難排解。 也就是密切相關，但很不一樣。 容量規劃的目標是：  

- 正確實作並操作環境 
- 最小化所花費的時間疑難排解效能問題。  
  
在 容量規劃，組織可能會有 40%的處理器使用率，以符合用戶端的效能需求，因而能夠在資料中心硬體升級所需的時間尖峰期間的基準目標。 然而，若要收到的效能異常事件，監視的警示閾值可能設 90 %5 分鐘間隔內。

其差異在於，持續超過容量管理臨界值時 （一次事件無關），新增容量 （也就，新增更多或更快的處理器） 是一個方案，或會擴充至多部伺服器的服務解決方案。 效能警示閾值，表示用戶端體驗目前發生問題，並需要立即的步驟來解決問題。

做個類比： 容量管理是有關防止汽車意外 （防禦性駕駛，並確定煞車正在正確等等） 而效能疑難排解是警察、 消防部門和緊急的醫療專業人員的工作之後發生的意外。 這是 [防禦開車]，關於 Active Directory 樣式。

過去幾年來，已大幅變更相應增加系統的容量規劃指引。 下列變更系統架構中的有受到質疑設計和調整服務的基本假設：

- 64 位元伺服器平台  
- 虛擬化  
- 功率耗用量增加的注意  
- SSD 儲存體  
- 雲端案例  

此外，方法從伺服器為基礎的容量規劃服務為基礎的容量，規劃練習的練習移位。 Active Directory 網域服務 (AD DS)、 成熟的分散式服務許多 Microsoft 和協力廠商產品做為後端，會變成一個正確規劃，以確保執行其他應用程式的必要容量最重要的產品。

### <a name="baseline-requirements-for-capacity-planning-guidance"></a>基準需求的容量規劃指引

在本文中，有下列的基本需求：

- 讀取器已閱讀並熟悉[效能微調指導方針適用於 Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85))。
- Windows Server 平台是 x64 型架構。 但即使您的 Active Directory 環境 （現在除了支援生命週期結尾） 的 Windows Server 2003 x86 上已安裝，並且可目錄資訊樹狀目錄 (DIT) 也就是減少大小的 1.5 GB，可以輕鬆地保留在記憶體中，從這個指導方針發行項是仍然適用。
- 容量規劃是連續的程序，您應該定期檢閱環境符合預期的程度。
- 最佳化會透過多個硬體生命週期與硬體的成本變更。 比方說，記憶體變得更便宜，每個核心的成本會降低，或不同的儲存體的價格選項的變更。
- 規劃在尖峰忙碌期間，在一天。 建議您看看這 30 分鐘或小時為間隔。 更高的任何項目可能會隱藏實際的尖峰流量和任何小於可能會扭曲的 「 暫時性尖峰。 」
- 適用於企業的硬體生命週期期間規劃成長。 這可能包括升級或新增硬體交錯的方式或完整的重新整理每三到五年中的策略。 如何更 Active Directory 上的負載會成長，這都需要"猜測"。 歷程記錄資料，如果收集，可協助使用這項評估中。 
- 規劃的容錯功能。 一次估計*N*衍生，針對包含的案例的計劃*N* &ndash; 1， *N* &ndash; 2 *N* &ndash; *x*。
  - 根據組織的需求，以確保遺失的單一或多個伺服器不會超過最大的尖峰容量估計值的其他伺服器中新增。
  - 也請考慮的成長計畫和錯誤容錯計劃需要整合。 比方說，如果一個 DC，才可支援的負載，但是估計值是負載將在下一個年度加倍，並需要兩個 Dc 總數，不會有足夠的容量，以支援容錯移轉。 解決方案會啟動三個網域控制站。 其中一個可能也會打算新增 3 個或 6 個月後的第三個 DC，如果預算是緊密。

    >[!NOTE]
    >新增 Active Directory 感知的應用程式可能會有明顯影響 DC 負載，是否負載來自應用程式伺服器或用戶端。

### <a name="three-step-process-for-the-capacity-planning-cycle"></a>容量規劃週期的三步驟程序

容量規劃，請先決定需要何種品質的服務。 比方說，核心資料中心支援較高層級的並行存取，而且需要更一致的經驗的使用者和取用端應用程式，需要更注意備援性，以及系統和基礎結構的瓶頸降至最低。 相反地，少數幾個使用者的附屬位置不需要相同的層級的並行處理或容錯功能。 因此，衛星辦公室可能不需要多注意力在基礎硬體和基礎結構，這可能會導致節省成本最佳化。 所有的建議和指導此處是達到最佳效能，而且可以是選擇性寬鬆的使用案例需求較低。

下一個問題是： 虛擬或實體嗎？ 容量規劃觀點而言，還有不正確或錯誤的答案;沒有僅不同組的好用的變數。 兩個選項的其中一個往虛擬化案例：

- 使用每一部主機 （虛擬化所在屬於抽象的實體硬體，從伺服器） 的一個來賓的 「 直接對應 」
- 「 共用的主機 」

測試及生產案例中指出，「 直接對應 」 案例可以視為相同到實體主機。 「 共用主機 」，不過，會帶來一些拼出更多詳細資料中稍後的考量。 「 共用的主機 」 案例表示 AD DS 也會競用資源，而且有負面影響和微調的考量，這項操作。

記住這些考量，容量規劃週期是反覆的三步驟程序：

1. 測量現有的環境，判斷系統瓶頸目前，而取得環境的基本功能所需計劃的所需的容量。
1. 決定根據步驟 1 中所述的準則所需的硬體。
1. 監視並驗證基礎結構進行實作，規格內運作。 在此步驟中收集一些資料成為基準容量規劃的下一個週期。

### <a name="applying-the-process"></a>套用此程序

若要最佳化效能，請確定正確選取和微調應用程式載入這些主要元件：

1. 記憶體
1. Network
1. 儲存體
1. 處理器
1. Net Logon

AD DS 的基本儲存體需求和適當撰寫的用戶端軟體的一般行為可讓具有多達 10000 到 20000 使用者容量規劃與實體硬體，幾乎任何現代的伺服器在放棄重大投資的環境類別系統處理負載。 話雖如此下, 表摘要說明如何評估現有的環境，就可以選取正確的硬體。 每個元件，就會分析在後續的章節來幫助評估他們使用的基準建議和環境特定主體的基礎結構的 AD DS 系統管理員的詳細資料。

一般情況下︰

- 任何目前的資料為基礎的調整大小才會正確地在目前的環境。
- 任何預估值，如預期需求成長的生命週期內的硬體。
- 判斷是否超過目前和擴大到較大的環境中，或增加容量的生命週期。
- 對虛擬化，完全相同容量規劃主體和方法來套用，不同之處在於虛擬化的負擔必須新增到任何項目相關的網域。
- 容量規劃，如同任何嘗試預測的項目不是精確的科學。 預期不會計算完美地與 100%的精確度的項目。 這裡的指引是 leanest 建議;增益集要更安全的容量，並持續驗證 環境保持在目標上。

### <a name="data-collection-summary-tables"></a>資料集合摘要資料表

#### <a name="new-environment"></a>新的環境

| Component | 估計值 |
|-|-|
|存放裝置] / [資料庫大小|40 KB 到每個使用者的 60 KB|
|RAM|資料庫大小<br />基底作業系統建議<br />第三方應用程式|
|Network|1 GB|
|CPU|每個核心的 1000 個並行使用者|

#### <a name="high-level-evaluation-criteria"></a>高層級的評估準則

| Component | 評估準則 | 規劃考量 |
|-|-|-|
|存放裝置] / [資料庫大小|一節 「 若要啟動的磁碟重組作業所釋放的磁碟空間的記錄 」 中[儲存體限制](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|儲存體 / 資料庫效能|<ul><li>「 邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Read，""LogicalDisk ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Write，""邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Transfer"</li><li>"LogicalDisk( *\<NTDS Database Drive\>* )\Reads/sec," "LogicalDisk( *\<NTDS Database Drive\>* )\Writes/sec," "LogicalDisk( *\<NTDS Database Drive\>* )\Transfers/sec"</li></ul>|<ul><li>儲存體的兩個考量需要解決<ul><li>可用空間，這與現今的基礎的磁針和 SSD 的大小基礎的儲存體無關的大部分 AD 環境。</li> <li>許多環境中 – 可用的輸入/輸出 (IO) 作業，這通常是被忽略。 請務必評估只環境，但其中沒有足夠的 RAM，將整個 NTDS 資料庫載入記憶體。</li></ul><li>儲存體可以是一個複雜的主題，並應包含適當的調整大小的硬體廠商專業知識。 特別是使用更複雜的案例，例如 SAN、 NAS 和 iSCSI 案例。 不過，在一般情況下，每 Gb 的儲存體的成本通常是在每次 IO 成本的直接反對者：<ul><li>RAID 5 低於 Raid 1 時，每 Gb 成本，但 Raid 1 有較低的成本，每次 IO</li><li>磁針為基礎的硬碟都具有較低的成本，每 Gb，但 Ssd 每個 IO 更低的成本</li></ul><li>重新啟動之後的電腦或 Active Directory 網域服務的服務，可延伸儲存引擎 (ESE) 快取是空的效能將會繫結而 warms 快取的磁碟。</li><li>在大多數環境中 AD 是讀取密集 I/O 到磁碟，模式隨機否定的許多快取的優點，並讀取最佳化策略。  此外，AD 有最大記憶體中快取比大多數的儲存體系統會快取。</li></ul>
|RAM|<ul><li>資料庫大小</li><li>基底作業系統建議</li><li>第三方應用程式</li></ul>|<ul><li>儲存體是最慢的元件的電腦中。 越多，可以是常駐在 RAM 中，不需要移至磁碟。</li><li>請確定配置足夠的 RAM 來儲存作業系統、 代理程式 （防毒、 備份、 監視），NTDS 資料庫並隨著時間成長。</li><li>其中最大化的 RAM 數量不是的環境的成本效益 （例如衛星位置） 或不可行 （DIT 是太大），以確保該儲存體的儲存體區段適當地調整大小的參考。</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>一般情況下，從 DC 到目前為止所傳送的流量超出傳送至 DC 的流量。</li><li>切換的乙太網路連線為全雙工，輸入和輸出網路流量，就必須獨立調整大小。</li><li>彙總數目 Dc 的會增加將針對每個網域控制站，傳送回應給用戶端要求所使用的頻寬量，但將會關閉以線性整個站台。</li><li>如果移除附屬位置網域控制站，別忘了，若要新增附屬 DC 的中樞網域控制站的頻寬，以及用來評估有多少 WAN 流量會。</li></ul>|
|CPU「 邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Read"descripti「 Process(lsass)\\' Processor Time"tors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>之後消除瓶頸的儲存體，處理所需的計算能力的量。</li><li>雖然不完全線性，（例如網站） 的特定範圍內的所有伺服器上耗用的處理器核心數目可以用於量測計多少個處理器都支援總計的用戶端負載所需。 加入最小必要範圍內的所有系統維護服務的目前層級。</li><li>處理器速度，包括電源管理中的變更相關的變更，衍生自目前環境的影響數字。 一般而言，就無法精確地評估如何前往 3 GHz 的處理器從 2.5 GHz 的處理器會減少所需的 Cpu 數目。</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon 的安全通道/MaxConcurrentAPI 只會影響使用 NTLM 驗證及/或 PAC 驗證的環境。 PAC 驗證是在預設會在 Windows Server 2008 之前的作業系統版本。 這會是用戶端設定，因此會受到影響的網域控制站，直到關閉此功能在所有用戶端系統上。</li><li>如果沒有適當調整大小，則環境具有重大跨信任驗證，其中包含內部樹系信任，有更大的風險。</li><li>伺服器彙總作業將會增加並行的跨信任層級驗證。</li><li>突增必須能容納的例如叢集容錯移轉，因為使用者重新驗證一併以新的叢集節點。</li><li>個別的用戶端系統 （例如叢集） 可能需要過調整。</li></ul>|

## <a name="planning"></a>規劃

長的時間，調整大小的 AD DS 的社群建議已將"放在與資料庫大小一樣多 RAM。 」 大部分的情況下，建議是，大部分的環境需要考慮。 但使用 AD DS 的生態系統就變得更大，因為有 AD DS 環境本身，自 1999 年問世以來。 雖然增加的計算能力，以及從 x86 轉換為 x64 架構所做的調整大小與執行 AD DS，在實體硬體上，虛擬化的成長的客戶一大組無關的效能有狡猾的層面的架構重新引入對比之前更大的觀眾調整的考量。

下列指導方針，因此是有關如何判斷和規劃需求的 Active Directory 做為服務，不論它是否部署在實體、 虛擬/實體混合時或完全虛擬化的案例。 我們將在此情況下，細分到四個主要元件的每個評估： 儲存體、 記憶體、 網路和處理器。 簡單地說，若要在 AD DS 上的效能最大化，目標是取得處理器繫結盡可能接近。

## <a name="ram"></a>RAM

只需、 越多，可以快取在 RAM 中，就越不就必須移至磁碟。 若要最大化伺服器延展性的最小 RAM 數量應目前的資料庫大小、 作業系統 SYSVOL 大小總計的總和建議 amount 和代理程式 （防毒、 監視、 備份、 等等的廠商建議嗎). 額外的工作量，應該將因應成長的存留期的伺服器。 這會是環保主觀為基礎的環境變更為基礎的資料庫成長的估計值。

其中最大化的 RAM 數量不是的環境的成本效益 （例如衛星位置） 或不可行 （DIT 是太大）、 儲存體 區段，以確保該儲存體已適當地設計的參考。

會出現在 一般內容調整記憶體中的必然結果分頁檔案的大小。 與所有其他項目相同的內容中記憶體相關，目標是要比速度變慢磁碟降到最低。 因此問題應為，"應該分頁檔要如何調整大小嗎？ 」 若要 「 多少 RAM 需要分頁降至最低？ 」 第二個問題的答案是本節的其餘部分中所述。 這會保留大多數的調整大小的分頁檔的一般作業系統建議，以及需要設定的系統記憶體傾印，也就是 AD DS 效能相關的領域來討論。

### <a name="evaluating"></a>評估

網域控制站 (DC) 需要的 RAM 數量是實際上複雜的練習，基於這些理由：

- 因為 LSASS 會修剪記憶體不足的壓力狀況下，以人為方式升空需要的方式回應，則需要高可能會嘗試使用現有的系統，以量測計多少 RAM 時發生錯誤。
- 主觀的事實，什麼是 「 有趣 」 給其用戶端快取，只需要個別的 DC。 這表示需要的站台與僅將 Exchange server 中的 DC 上進行快取的資料將會非常不同於需要只會驗證使用者的 DC 上進行快取的資料。
- 要評估的案例的基礎上每個 DC RAM 人力高昂，並變更環境變更時。
- 建議背後的準則是為了做出明智的決策： 
- 越多，可以快取在 RAM 中，不需要移至磁碟。 
- 儲存體是最慢的電腦元件。 存取以磁針為基礎的資料和 SSD 存放媒體是 1,000,000 x 低於在 RAM 中存取資料的順序。

因此，為了達到最高延展性的伺服器上，最小 RAM 數量資料目前的資料庫大小、 作業系統 SYSVOL 大小總計的總和，建議您使用 amount 和代理程式 （防毒軟體、 監視、 備份、 廠商的建議等等）。 新增額外的數量，以配合伺服器的存留期的成長。 這會是環保主觀為基礎的資料庫成長的估計值。 不過，附屬位置與一小組使用者，這些需求可放寬這些站台將不需要快取程度為大部分的要求提供服務。

其中最大化的 RAM 數量不是的環境的成本效益 （例如衛星位置） 或不可行 （DIT 是太大），以確保該儲存體的儲存體區段適當地調整大小的參考。

> [!NOTE]
> 必然結果而調整大小的記憶體分頁檔案的大小。 問題的目標是要比速度變慢磁碟降到最低，因為會從 「 應該分頁檔要如何調整大小嗎？ 」 若要 「 多少 RAM 需要分頁降至最低？ 」 第二個問題的答案是本節的其餘部分中所述。 這會保留大多數的調整大小的分頁檔的一般作業系統建議，以及需要設定的系統記憶體傾印，也就是 AD DS 效能相關的領域來討論。

### <a name="virtualization-considerations-for-ram"></a>RAM 的虛擬化注意事項

請避免在主機上的記憶體過度認可。 背後最佳化 RAM 容量的基本目標是時間的要移至磁碟所花費量降到最低。 在虛擬化案例中，記憶體過度認可的概念存在更多的 RAM 配置給來賓的位置，則存在於實體機器上。 這本身不是問題。 主動使用所有來賓的記憶體總計超過主機上的 RAM 數量，而基礎主機啟動分頁時，它就會變成問題。 效能會變成繫結磁碟所在的網域控制站要取得資料，NTDS.dit 或分頁檔，以取得資料，將網域控制站或主應用程式會取得客體視為在 RAM 中的資料磁碟的情況。

### <a name="calculation-summary-example"></a>計算摘要範例

|Component|估計的記憶體 （範例）|
|-|-|
|基底作業系統建議的 RAM (Windows Server 2008)|2 GB|
|LSASS 內部工作|200 MB|
|監視代理程式|100 MB|
|防毒|100 MB|
|資料庫 （通用類別目錄）|8.5 GB opravdu 篇|
|若要執行，系統管理員登入，而不會影響備份間隔|1 GB|
|總計|12 GB|

**建議使用：16 GB**

經過一段時間，假設您可以進行更多的資料將會加入至資料庫，伺服器可能會在 3 到 5 年的生產環境中。 根據 33%的估計，16 GB 是成長的合理的 RAM 將放在實體伺服器。 在虛擬機器，提供的簡化的可修改設定，和 RAM 可新增至 VM，與監視，並在未來升級計劃開始 12 GB 是合理的。

## <a name="network"></a>Network

### <a name="evaluating"></a>評估
此區段會小於評估有關複寫流量，其中著重於周遊 WAN 流量，並徹底涵蓋需求的相關[Active Directory 複寫流量](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10))、 有關評估總計大於包含用戶端查詢、 群組原則的應用程式等所需的頻寬和網路容量。 對於現有的環境，這可以收集使用效能計數器 」 網路介面 (\*) \Bytes Received/sec"和"網路介面 (\*) \Bytes Sent/sec。 」 在 15、 30 或 60 分鐘內的網路介面計數器的取樣間隔。 任何小於通常會為良好的度量，太 volatile更高的任何項目將會過平滑每日的窺視。

> [!NOTE]
> 一般而言，大部分的 DC 上的網路流量是輸出，因為 DC 回應用戶端查詢。 雖然建議也評估每個環境的輸入流量，這會是輸出流量，著重的原因。 同樣的方式可用來解決，並檢閱輸入的網路流量需求。 如需詳細資訊，請參閱知識庫文件[929851:在 Windows Vista 和 Windows Server 2008 中，TCP/IP 的預設動態通訊埠範圍已變更](http://support.microsoft.com/kb/929851)。

### <a name="bandwidth-needs"></a>頻寬需求

規劃網路延展性涵蓋兩種不同類別： 的流量和 CPU 負載的網路流量。 每個案例是相較於這篇文章中的其他主題的一些簡單易懂。

在評估時必須支援多少流量，有兩種唯一的網路流量方面的 AD DS 容量規劃。 第一個是網域控制站之間周遊，並徹底涵蓋參考中的複寫流量[Active Directory 複寫流量](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10))和仍與目前版本的 AD DS。 第二個是站台內用戶端對伺服器流量。 其中一個更簡單的案例，以規劃站台內的流量以接收來自相對於大量資料傳送回用戶端的用戶端的小型要求。 通常會在環境中最多 5,000 個使用者每個站台中的伺服器有足夠 100 MB。 支援建議針對以外的 5,000 位使用者使用 1 GB 網路介面卡和接收端調整 (RSS)。 若要驗證此案例中，特別是在伺服器合併案例的情況下查看 網路介面 (\*) \Bytes/sec 跨站台，在所有網域控制站將它們相加，並除以 網域控制站，以確保那里的目標數目是有足夠的容量。 若要這樣做最簡單的方式是使用 「 堆疊區域 」 檢視 Windows 可靠性和效能監視器 （先前稱為 Perfmon），並確定所有的計數器會調整相同。

請考慮下列範例 (也稱為說真的，變得很複雜的方式，來驗證的一般規則是適用於特定的環境)。 會進行下列假設：

- 目標是要盡可能減少到較少的伺服器的使用量。 在理想情況下，一部伺服器會執行負載和冗餘的部署額外的伺服器 (*N* + 1 的案例)。 
- 在此案例中，目前的網路介面卡支援僅 100 MB，並切換環境中。  
  最大目標的網路頻寬使用率為 N 案例 （DC 遺失） 中的 60%。
- 每部伺服器都有大約 10,000 個連線到它的用戶端。

從圖表中的資料得到的知識 (網路介面 (\*) \Bytes Sent/sec):

1. 工作日開始往下緩慢增加大約 5:30 和下午 7:00。
1. 在尖峰繁忙期間會從上午 8:00 為上午 8:15，有超過 25 個位元組傳送/秒的最忙碌的 DC 上。  
   > [!NOTE]
   > 所有效能資料都是歷程記錄。 因此在 8:15 的尖峰資料點表示從 8:00 將載入至 8:15。
1. 上午 4:00，長度不得少於 20 個位元組傳送/秒的最忙碌的 DC 上可能表示從不同的時區是負載或背景基礎結構的活動，例如備份之前，有尖峰。 在上午 8:00 的最大值超過此活動，因為它不相關。
1. 在站台中有五個網域控制站。
1. 最大的負載是連線的大約 5.5 MB/秒每 100 MB 44%的 DC。 使用這項資料，它可估計上午 8:00 和上午 8:15 之間所需的總頻寬是 28 MB/秒。
   >[!NOTE]
   >請謹慎使用的網路介面傳送/接收計數器以位元組為單位，以位元為單位的網路頻寬。 100MB &divide; 8 = 12.5 MB，1 GB &divide; 8 = 128 MB。
  
結論：

1. 此目前的環境符合 N + 1 的層級 60%的目標使用率在容錯移轉。 離線系統將會轉換每一部伺服器的頻寬從大約 5.5 MB/秒 （44%)大約 7 MB/秒 （56%)。
1. 這樣可同時為基礎的彙總到一部伺服器先前所述的目標，超過最大目標使用率和理論上可能利用的 100 MB 的連線。
1. 具有 1 GB 連線中，這將會代表 22%的總容量。
1. 在 標準模式下運作中的條件*N* + 1 的案例中，用戶端負載會相當平均散發在約 14 MB/s 每個伺服器或 11%的總容量。
1. 為了確保容量在 DC 無法使用時就已足夠，每一部伺服器的一般作業的目標就是大約 30%的網路使用率或 38 MB/s 每一部伺服器。 容錯移轉目標會是 60%的網路使用率或 72 MB/s 每一部伺服器。  
  
簡單地說，系統最終部署必須有 1 GB 網路介面卡，而且在連線到網路基礎結構將會支援該的負載。 進一步注意是指定產生的網路傳輸量，從網路通訊的 CPU 負載可以有顯著的影響，限制 AD DS 的最大的延展性。 同樣的程序可用來估計的 dc 的輸入通訊。 但假設相對於輸入流量的輸出流量佔逃避，學術大部分的環境下練習。 在有超過 5,000 個使用者每一部伺服器的環境中重要 RSS 確保硬體支援。 具有高網路流量的情況下，平衡的插斷負載可能會成為瓶頸。 這可以偵測到的處理器 (\*)\%並非平均地分散到 Cpu 插斷時間。 已啟用 RSS 的 Nic 可以解決這項限制及增加延展性。

> [!NOTE]
> 類似的方法可以用來估計所需的額外容量時合併資料中心，或淘汰的附屬位置的網域控制站。 只收集用戶端的傳出和傳入的流量，因此，現在會出現在 WAN 連結上的流量。  
>  
> 在某些情況下，您可能會遇到非預期行為，因為流量會較慢，例如當憑證檢查失敗時以符合在 WAN 上的彙總逾時的更多流量。 基於這個理由，WAN 的調整大小和使用率應該是反覆的、 進行中的程序。

### <a name="virtualization-considerations-for-network-bandwidth"></a>虛擬化網路頻寬考量

它很容易進行實體伺服器的建議：支援大於 5000 個使用者的伺服器需要 1 GB。 當多個來賓會開始共用基礎的虛擬交換器基礎結構時，多加注意需要，以確保主機有足夠的網路頻寬，可以在系統上，支援所有來賓，並因此而需要額外的指引。 這是只確保到主機電腦的網路基礎結構的擴充功能。 這是不論是否在網路不包含在主機上執行為虛擬機器的來賓與虛擬交換器，超過的網路流量的網域控制站是否直接連線至實體交換器。 虛擬交換器的只有一個詳細的元件需要支援的資料傳輸量上行連結的位置。 因此連結到交換器的實體主機實體網路介面卡應該能夠支援 DC 負載以及其他共用實體網路介面卡連線的虛擬交換器的所有來賓。

### <a name="calculation-summary-example"></a>計算摘要範例

|系統|尖峰頻寬|
|-|-|
DC 1|6.5 MB/秒|
DC 2|6.25 MB/秒|
|DC 3|6.25 MB/秒|
|DC 4|5.75 MB/秒|
|DC 5|4.75 MB/秒|
|總計|28.5 MB/秒|

**建議使用：72 MB/s** （28.5 MB/s 除以 40%）

|目標系統計數|（上述） 的總頻寬|
|-|-|
|2|28.5 MB/秒|
|產生的一般行為|28.5 &divide; 2 = 14.25 MB/秒|

一如往常，經過一段時間的假設可以進行用戶端負載會增加，這種成長應該加以規劃作為最佳。 建議的容量規劃可讓 50%的預估成長中的網路流量。

## <a name="storage"></a>儲存體

規劃儲存體構成兩個元件：

- 容量或儲存體大小
- 效能

在規劃容量，讓效能通常會完全忽略上花費大量時間和文件。 目前的硬體成本，大部分的環境不大夠任一這些是實際問題，並建議將"放在與資料庫大小一樣多 RAM 」 通常涵蓋其餘部分，但可以是中的衛星位置是否大材小用更大環境。

### <a name="sizing"></a>調整大小

#### <a name="evaluating-for-storage"></a>評估儲存體

相較於 13 年以前當 Active Directory 引進，已將最常見的磁碟機大小當 4 GB 與有 9 GB 的磁碟機的時間，Active directory 的調整大小不是偶數為所有的考量，但最大的環境。 與最小可用硬碟大小的 180 GB 範圍、 整個作業系統、 SYSVOL、 和 NTDS.dit 可以輕鬆地調整成一個磁碟機上。 因此，建議您取代這方面的重大投資。

只建議考量是確保 NTDS.dit 大小的 110%的可用性，以啟用重組。 此外，在硬體生命週期的成長加班費。

第一個也是最重要考量評估如何大型 NTDS.dit 以及 SYSVOL 會。 這些度量會造成縮放這兩個固定的磁碟和 RAM 的配置。 因為 （相對） 低成本的這些元件，而數學不需要嚴格且精確。 有關如何針對現有和新的環境可在評估此內容[資料存放區](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10))一系列的文章。 具體來說，請參閱下列文章：

- **針對現有的環境&ndash;** 文件中的區段標題為 < 若要啟用記錄的磁碟重組作業所釋放的磁碟空間[儲存體限制](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))。
- **新的環境&ndash;** 一文[Active Directory 使用者和組織單位的成長估計](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10))。

  > [!NOTE]
  > 這些文章根據發行版本的 Windows 2000 中的 Active Directory 時進行的資料大小估計值。 使用反映您環境中的物件的實際大小的物件大小。

當檢閱現有的環境具有多個網域，則可能的資料庫大小的變化。 當這種情況，使用 「 最小通用類別目錄 (GC) 和非 GC 大小。  

資料庫大小的作業系統版本而有所不同。 執行舊版作業系統，例如 Windows Server 2003 具備較小的資料庫大小比 DC，執行更新版本的作業系統，例如 Windows Server 2008 R2，特別是當功能這類的 Active Directory 資源回收筒或認證漫遊啟用的網域控制站。

> [!NOTE]  
  >
>- 對於新的環境，請注意在成長估計的 Active Directory 使用者和組織單位的估計值，指出 100,000 名使用者 （在相同的網域） 會取用大約 450 MB 的空間。 請注意，填入的屬性都有重大影響總數。 屬性會填入許多物件，由協力廠商和 Microsoft 產品，包括 Microsoft Exchange Server 和 Lync。 根據環境中的產品組合評估最好，但履行詳述出數學和測試所有的精確估計值，但最大的環境實際上可能不值得大量時間與精力。
>- 確定 110%的 NTDS.dit 大小會當做可用空間，以啟用離線磁碟重組、 規劃成長，透過三到五年硬體生命週期。 指定如何便宜的儲存體是，DIT 的大小，因為儲存體配置是安全地因應成長及可能需要離線重組的 300%，估計儲存體。

#### <a name="virtualization-considerations-for-storage"></a>虛擬化儲存體的考量

在多個虛擬硬碟 (VHD) 檔案在單一磁碟區正在配置位置的案例中使用固定大小磁碟至少 210%（100%的 DIT + 110%的可用空間) 以確保沒有足夠的空間保留 DIT 的大小。  

#### <a name="calculation-summary-example"></a>計算摘要範例

|從評估階段所收集的資料| |
|-|-|
|NTDS.dit 大小|35 GB|
|修飾詞，以便進行離線磁碟重組|2.1|
|所需的儲存體總計|73.5 GB|

> [!NOTE]
> 需要此儲存體是除了 SYSVOL、 作業系統、 分頁檔案、 暫存檔案、 本機快取的資料 （例如安裝程式檔案），以及應用程式所需的儲存體。

### <a name="storage-performance"></a>儲存體效能

#### <a name="evaluating-performance-of-storage"></a>評估儲存體的效能

做為任何電腦中最慢的元件，存放裝置可以在用戶端體驗有最大的負面影響。 針對這些大型環境足夠的 RAM 大小調整建議皆不可行，怎麼也發掘規劃儲存體效能的後果後果可能不堪設想。  此外的複雜性和各種儲存技術進一步增加失敗的風險的最佳做法的"put 作業的系統、 記錄檔，以及資料庫 」 在不同的實體磁碟上的長久相關性僅限於它的實用案例。  這是因為長久的最佳作法根據是 「 磁碟 」 是專用的磁針，這允許隔離的 I/O 的假設。  此假設將此設為 true，便不再相關的介紹：

- 新的儲存體類型和虛擬化及共用儲存體案例
- 共用磁針上存放區域網路 (SAN)
- 在 SAN 或網路連接儲存體的 VHD 檔案
- 固態硬碟
- 分層式儲存體架構 （也就是 SSD 儲存層快取較大的磁針基礎儲存體）

簡單地說，結束所有的儲存體效能工作，不論基礎儲存體架構和設計，旨在確保所需的輸入/輸出作業每秒數 (IOPS) 容量可用，並可接受的時間範圍內，發生這些 IOPS. 本節說明如何評估哪些 AD DS 需求的基礎儲存體，以確保儲存體解決方案的設計正確。  指定目前的儲存體技術的變化，最好是與存放裝置廠商合作，以確保有足夠的 IOPS。  這些案例使用本機連接的儲存體中，參考旓紵 C，如如何設計傳統的本機儲存體案例的基本概念。  此主體通常適用於更複雜的儲存層，並與支援後端存放裝置解決方案廠商也有助於在對話方塊中。

- 提供強大的可用儲存體選項，建議您使用參與的硬體支援小組或廠商，以確保特定的解決方案符合 AD DS 的需求的專業知識。 下列的數字是儲存專業人員提供的資訊。

太大，無法在 RAM 中保留資料庫所在的環境下，使用效能計數器來判斷多少 I/O 需要受到支援：

- 邏輯磁碟 (\*) \Avg Disk sec/Read (例如，如果 NTDS.dit 會儲存在 d: /磁碟機的完整路徑就是邏輯磁碟 （d:） \Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- 邏輯磁碟 (\*) \Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

這些應該取樣以建立基準的目前的環境需求 15/30/60 分鐘的時間間隔。

#### <a name="evaluating-the-results"></a>評估結果

> [!NOTE]
> 焦點是從資料庫讀取，因為這通常是最嚴苛的元件、 的相同邏輯可以套用至寫入記錄檔，以替代邏輯磁碟 ( *\<NTDS 記錄\>* ) \Avg Disk sec/Write與邏輯磁碟 ( *\<NTDS 記錄\>* ) \Writes/sec):
>  
> - 邏輯磁碟 ( *\<NTDS\>* ) \Avg Disk sec/Read 指出目前的儲存體是否會適當地調整大小。  如果結果是大致相等到磁碟存取時間的磁碟類型，邏輯磁碟 ( *\<NTDS\>* ) \Reads/sec 是有效的量值。  檢查後端上的存放裝置的製造商規格，但良好的範圍，如邏輯磁碟 ( *\<NTDS\>* ) \Avg Disk sec/Read 大約是：
>   - 7200 – 若要 12.5 9 毫秒 (ms)
>   - 10,000 – 6 到 10 毫秒
>   - 15,000 – 4 到 6 毫秒。  
>   - SSD – 1 到 3 毫秒  
>   - >[!NOTE]
>     >建議存在指出儲存體效能會降低在 15ms年至 20 毫秒 （視來源而定）。  上述的值與其他指導方針之間的差異是上述的值是正常的作業範圍內。  其他建議進行疑難排解指南，以識別用戶端體驗大幅降低，以及會變得明顯。  如需深入的說明參考旓紵 C。
> - 邏輯磁碟 ( *\<NTDS\>* ) \Reads/sec 是正在執行的 I/O 數量。
>   - 如果邏輯磁碟 ( *\<NTDS\>* ) 是後端儲存體，邏輯磁碟的最佳範圍內的 \Avg Disk sec/Read ( *\<NTDS\>* ) \Reads/直接向調整大小的儲存體可用秒。
>   - 如果邏輯磁碟 ( *\<NTDS\>* ) \Avg Disk sec/Read 不在後端儲存體的最佳範圍內，額外的 I/O 需要根據下列公式：
>     > (邏輯磁碟 ( *\<NTDS\>* ) \Avg Disk sec/Read) &divide; （實體媒體的磁碟存取時間） &times; (邏輯磁碟 ( *\<NTDS\>* )\Avg disk sec/Read)

考量：

- 請注意，是否伺服器具有次佳的 RAM 容量設定，這些值會針對規劃目的，不正確。  它們會錯誤地的高邊，並仍可用來當做最糟糕的情況。
- 特別是最佳化新增 RAM 」 將磁碟機讀取 I/O 數量減少 (邏輯磁碟 ( *\<NTDS\>* ) \Reads/Sec。 這表示儲存體解決方案可能沒有為健全一開始計算。  不幸的是，任何項目更多特定這個一般陳述式環保取決於用戶端負載和一般指引超出無法提供。  最佳選項是調整之後最佳化 RAM 的儲存體大小。

#### <a name="virtualization-considerations-for-performance"></a>虛擬化效能的考量

類似於所有先前的虛擬化討論，這裡的重點是確保基礎的共用基礎結構可否支援 DC 負載，以及使用基礎的其他資源共用的媒體和它的所有路徑。 這適用於實體的網域控制站是否共用相同的基礎媒體上的 SAN、 NAS 或 iSCSI 基礎結構做為其他伺服器或應用程式，無論是使用傳遞至共用的 SAN、 NAS 或 iSCSI 基礎結構存取來賓基礎媒體，或如果來賓使用 VHD 檔案位於本機的共用的媒體或 SAN、 NAS 或 iSCSI 的基礎結構上。 規劃的練習的重點並確定基礎媒體可以支援所有取用者的總負載。

另外，從來賓的觀點來看，因為有額外的程式碼路徑，必須周遊，還有對需要經歷主應用程式存取任何儲存體效能的影響。 不用多說，儲存體效能測試指出虛擬化有影響是主觀的主機系統的處理器使用率的輸送量 （請參閱附錄 a:CPU 調整大小準則），這會很明顯地受到客體所要求的主機資源。 這提供有關在虛擬化案例中的處理需求的虛擬化考量 (請參閱[處理的虛擬化考量](#virtualization-considerations-for-processing))。

使其更複雜，是有各種不同的儲存體時，可用選項都有不同的效能影響。 從實體移轉到虛擬時的安全估計，來調整不同的儲存體選項的虛擬化客體 HYPER-V，例如傳遞儲存體、 SCSI 介面卡或 IDE 使用 1.10 的乘數。 對於是否為本機儲存體、 SAN、 NAS 或 iSCSI 無關也是不同的儲存體案例之間傳輸時所需的調整。

#### <a name="calculation-summary-example"></a>計算摘要範例

判斷狀況良好的系統在正常作業情況下所需的 I/O 的數量：

- 邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) 在尖峰期間 15 分鐘的期間 \Transfers/sec 
- 若要判斷所需的儲存體超出的基礎儲存體容量的 I/O 數量：
  >*Needed IOPS* = (LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>* ) &times; LogicalDisk( *\<NTDS Database Drive\>* )\Read/sec

|計數器|值|
|-|-|
|實際的邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Transfer|.02 秒 （20 毫秒）|
|目標邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Transfer|0\.1 秒|
|變更可用的 IO 的乘數|0.02 &divide; 0.01 = 2|  
  
|值名稱|值|
|-|-|
|LogicalDisk( *\<NTDS Database Drive\>* )\Transfers/sec|400|
|變更可用的 IO 的乘數|2|
|在尖峰期間所需的 IOPS 總數|800|

若要判斷在快取且希望就緒的速率：

- 判斷準備快取的最大可接受的時間。 是，會從磁碟載入整個資料庫所花費的時間量，或在 RAM 中，無法載入整個資料庫的情況下，這會填滿 RAM 的最長時間。
- 判斷資料庫的大小，但不包括泛空白字元。  如需詳細資訊，請參閱 <<c0> [ 評估為儲存體](#evaluating-for-storage)。  
- 資料庫大小除以 8 KB;要載入資料庫所需的總 IOs。
- Total IOs 除以中定義的時間範圍內的秒數。

請注意，計算，而正確的速率將不會完全因為先前載入的頁面會收回如果 ESE 未設定為有固定的快取的大小，而且預設的 AD DS 會使用變數的快取大小。

|若要收集的資料點|值
|-|-|
|若要準備的最大可接受時間|10 分鐘 （600 秒）
|資料庫大小|2 GB|  
  
|計算步驟|公式|結果|
|-|-|-|
|計算頁面中的資料庫大小|(2 GB &times; 1024年&times;1024年) =*資料庫，以 kb 為單位的大小*|2,097,152 KB|
|計算資料庫中的頁數|2097152 KB &divide; 8KB =*頁數*|為 262,144 頁面|
|計算完全準備快取所需的 IOPS|為 262,144 頁面&divide;600 秒 =*所需的 IOPS*|437 IOPS|

## <a name="processing"></a>正在處理

### <a name="evaluating-active-directory-processor-usage"></a>評估 Active Directory 處理器使用量

對於大部分的環境中，儲存體、 RAM 和網路功能瑎堀規劃一節中所述之後管理的處理容量會值得多留意的元件。 評估所需的 CPU 容量有兩個挑戰：

- 在環境中的應用程式是否正在共用的服務基礎結構中的任何行為良好和本文中標題為 「 追蹤成本和效率不佳的搜尋 一節所述建立更有效率 Microsoft Active啟用目錄的應用程式或從舊版 SAM 移轉呼叫 LDAP 呼叫。  
  
  在較大的環境中，這點很重要的原因是不佳的自動程式化的應用程式可以磁碟機中的 CPU 負載的變動性、 「 竊取 」 的其他應用程式的 CPU 時間相當長、 以人為方式提高容量需求及平均地分散負載與針對網域控制站。  
- 因為 AD DS 是分散式的環境中使用各種潛在用戶端，估計所需的 「 單一用戶端 」 成本是因為使用模式的型別或利用 AD DS 的應用程式數量環保主觀的。 簡單地說，非常類似的網路區段，廣泛的適用性，這是更接近評估在環境中所需的總容量的觀點。

對於現有的環境，如先前討論的儲存體大小假設儲存體的大小現在正確，並因此處理器負載有關的資料是有效的。 再次提醒您，請務必確保系統的瓶頸不是儲存體的效能。 當有瓶頸存在，而且正在等候處理器時，有閒置狀態，就會消失之後瓶頸已移除。  隨著處理器等候狀態會遭到移除，根據定義，因為它不會再必須等候資料，也會增加 CPU 使用率。 因此，收集效能計數器 」 的邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Read"和"Process(lsass)\\%Processor Time"。 中的資料 」 Process(lsass)\\%Processor Time"將會減少以人為方式如果 「 邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Read"超過 10 到 15 毫秒，這是一般的臨界值，Microsoft 支援服務會使用儲存體相關的效能問題進行疑難排解。 同樣地，建議您使用取樣間隔是 15、 30 或 60 分鐘的時間。 任何小於通常會為良好的度量，太 volatile更高的任何項目將會過平滑每日的窺視。

### <a name="introduction"></a>簡介

若要規劃容量規劃的網域控制站，處理能力需要最多注意和了解。 在調整大小時以確保最大效能的系統，總是有瓶頸的元件和適當大小的網域控制站中，這會是處理器。

類似於環境的需求，檢閱站台的站台為基礎的網路區段，相同必須完成要求的計算容量。 不同於網路區段，其中提供的網路技術遠遠地超過正常的需求，需要支付多加留意，若要調整大小的 CPU 容量。  即使中等大小; 的任何環境任何項目上少數的數千個並行使用者可以將大量負載放在 CPU 上。

不幸的是，利用 AD 的用戶端應用程式很大的變化性，因為中的每一 CPU 的使用者一般預估為明白不適用於所有環境中。 具體來說，計算要求受限於使用者行為和應用程式設定檔。 因此，每個環境都需要個別調整大小。

#### <a name="target-site-behavior-profile"></a>目標站台的行為設定檔

如前所述，規劃整個站台的容量時，目標是為目標的設計，含有*N* + 1 個容量設計，例如在尖峰期間的一個系統的失敗可讓您的服務在合理的接續品質層級。 這表示，在 「*N*」 案例中，跨所有的方塊的負載應該小於 100%（最好是小於 80%）在尖峰期間。

此外，如果應用程式和網站中的用戶端使用最佳作法，找出網域控制站 (也就使用[DsGetDcName 函式](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx))，用戶端應該使用次要相當平均分散由於各種因素的暫時性尖峰。

在下一個範例中，會進行下列假設：

- 每個站台中的五個 Dc 都有四個 Cpu。
- 總目標上班時間期間的 CPU 使用量是在正常作業情況下 40%(「*N* + 1)，否則為 60%(「*N*")。 在非上班時間，目標 CPU 使用量是 80%的備份軟體及其他維護作業會取用所有可用的資源。

![CPU 使用量圖表](media/capacity-planning-considerations-cpu-chart.png)

分析圖表中的資料 (處理器 Information(_Total)\%處理器公用程式) 每個網域控制站：

- 大部分的情況下，負載是相當平均分佈也就是用戶端使用 DC 定位程式，也撰寫搜尋時就可預期。 
- 有數目的 10%，某些最大的五分鐘尖峰為 20%。 一般而言，除非它們會導致超過容量計劃目標，調查這些不是值回票價。  
- 適用於所有的系統在尖峰期間是大約上午 8:00 和上午 9:15 之間。 大約 5:00 AM 透過大約下午 5:00 平滑轉換，這是商務週期通常表示。 在下午 5:00 之間上午 4:00 方塊的案例上的 CPU 使用量更隨機的高峰會的容量規劃考量外。

  >[!NOTE]
  >管理完善的系統上，前述的特殊圖文集是可能的備份軟體執行完整系統的防毒掃描、 硬體或軟體清查、 軟體或修補程式的部署，等等。 因為其會落在尖峰使用者商務週期外，都不會超過目標。

- 為每個系統是大約 40%，且所有的系統有相同的 Cpu 數目，應一失敗或離線，剩餘的系統會執行在估計有 53%（系統 D 40%負載會平均分割，並加入到系統 A 和 C 系統的現有的 40%負載）。 對多種原因所造成，此線性的假設並不完全精確，但提供足夠的精確度量測計。  

  **替代案例 –** 執行 40%的兩個網域控制站：一個網域控制站失敗，剩餘的估計 CPU 就是預估的 80%。 這遠超過上面所述的容量計劃的臨界值，並且也開始嚴重限制的標頭空間的 10%到 20%的負載設定檔，這表示高峰會期間的 100%到 90%磁碟機 DC 中看到 「*N*」 案例，而且一定會降低回應能力。

### <a name="calculating-cpu-demands"></a>計算的 CPU 需求

「 程序\\處理器時間百分比 」 效能物件計數器會加總的總金額的時間的所有應用程式的執行緒在 CPU 上花費，並除以總金額的系統時間已超過。 效果是多重 CPU 系統上的多執行緒應用程式可能會超過 100 %cpu 時間，並會非常不同解譯比 「 處理器資訊\\%處理器公用程式 」。 在實務上 「 Process(lsass)\\%Processor Time"可視為執行會支援處理序的需求所需的 100%的 Cpu 數目。 值為 200%表示 2 個 Cpu，各為 100%，才能支援完整的 AD DS 負載。 雖然在 100%容量執行 CPU 是最具成本效益觀點的金額支出的 Cpu 和系統時，就會發生電源和能源消耗，將多個附錄 A 的較佳的回應能力，多執行緒的系統上所述的原因未執行 100%。

為了容納暫時性尖峰的用戶端負載，建議您使用目標尖峰期間的 CPU 40%到 60%的系統容量之間。 使用上述範例中，這表示 3.33 （60%的目標） 和 5 （40%的目標） 之間的 Cpu 會需要 AD DS （lsass 處理程序） 的負載。 應該加入額外的容量中，根據基礎作業系統和其他 （例如防毒、 備份、 監視、 等等） 所需的代理程式的需求。 雖然代理程式的影響，就需要進行評估以每個環境為基礎，就可以進行的估計值介於 5%到 10%的單一 CPU 之間。 在目前的範例中，這會建議，3.43 （60%的目標） 和 5.1 （40%的目標） 之間的 Cpu 時，需要在尖峰期間。

若要這樣做最簡單的方式是使用 「 堆疊區域 」 檢視 Windows 可靠性和效能監視器 (perfmon)，並確定所有的計數器會調整相同。

假設：

- 目標是要盡可能減少到較少的伺服器的使用量。 在理想情況下，一部伺服器會執行負載和冗餘的新增額外的伺服器 (*N* + 1 的案例)。

![Lsass 處理程序 （透過所有處理器） 的處理器時間圖表](media/capacity-planning-considerations-proc-time-chart.png)

從圖表中的資料得到的知識 (Process(lsass)\\%Processor Time):

- 工作日開始緩慢增加大約 7:00，並減少在下午 5:00。
- 在尖峰繁忙期間是上午 9:30 從上午 11:00。 
  > [!NOTE]
  > 所有效能資料都是歷程記錄。 9:15 的尖峰資料點表示從 9:00 將載入至 9:15。
- 上午 7:00 這可能表示來自不同時區的負載或是背景基礎結構的活動，例如備份之前，有尖峰。 上午 9:30 尖峰超過此活動，因為它不相關。
- 在站台中有三個網域控制站。

目前最大的負載，lsass 會取用大約 485%的 CPU 或 4.85 執行 100%的 Cpu。 依據數學計算更早版本，這表示站台需要大約 12.25 Cpu 適用於 AD DS。 在上述建議的 5%到 10%的背景處理序中新增，這表示立即取代伺服器需要大約 12.30 至 12.35 Cpu 支援相同的負載。 環境現在成長估計需要分解出來。

### <a name="when-to-tune-ldap-weights"></a>微調 LDAP 加權的時機

有幾個案例在 tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10))應該考慮。 在內容中的容量規劃，這會完成的應用程式或使用者的負載不平均地平衡，或就功能而言不平均分散的基礎系統時。 若要這樣做超過容量規劃的原因是這篇文章的範圍外。

有兩個常見的原因，來微調 LDAP 權數：

- PDC 模擬器是會影響哪些使用者或應用程式的載入行為並未平均分佈的每個環境的範例。 由於某些工具和動作目標的 PDC 模擬器，例如群組原則管理工具]、 [第二個嘗試在驗證失敗、 建立信任，並依此類推，PDC 模擬器上的 CPU 資源可能會比在其他地方偏向要求站台。
  - 才可調整此，如果沒有顯著的差異，CPU 使用率，以減少 PDC 模擬器上的負載，並提高其他網域控制站上的負載可讓負載更平均地分散。
  - 在此情況下，設定介於 50 到 75 LDAPSrvWeight PDC 模擬器。
- 伺服器的 Cpu （和速度） 網站中的不同計數。  例如，假設有兩個八核心伺服器和一個四核心伺服器。  最後一部伺服器有其他兩個伺服器的一半處理器。  這表示，也是分散式用戶端負載將增加大約兩倍的八核心方塊的四核心方塊上的平均 CPU 負載。
  - 比方說，40%會同時執行兩個八核心方塊，並會執行四核心的方塊百分之 80。
  - 此外，請考慮在此案例中，特別是四核心方塊現在會超載的事實中的一個八核心方塊遺失的影響。

#### <a name="example-1---pdc"></a>範例 1-PDC

| |使用預設值的使用率|New LdapSrvWeight|估計的新使用率|
|-|-|-|-|
|DC 1 （PDC 模擬器）|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

比較麻煩的部分是，如果 PDC 模擬器角色傳輸或抓取，特別是要在網站中，另一個網域控制站會在新的 PDC 模擬器上的大幅增加。

此範例使用區段[目標站台的行為設定檔](#target-site-behavior-profile)，假設已在站台中的所有三個網域控制站有四個 Cpu。 採取的動作，在正常情況下，如果其中一個網域控制站有八個 Cpu？ 會在 40%使用率的兩個網域控制站，另一個在 20%的使用率。 雖然這不是錯誤，還有以平衡負載稍微更好的機會。 利用 LDAP 權數，以完成這項作業。  會的示範案例：

#### <a name="example-2---differing-cpu-counts"></a>範例 2-不同的 CPU 計數

| |處理器資訊\\ %&nbsp;處理器 Utility(_Total)<br />使用預設值的使用率|New LdapSrvWeight|估計的新使用率|
|-|-|-|-|
|4 CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

不過是非常謹慎地處理這些案例。 可以是如上所示，數學看起來真的輕鬆又很在紙張上。 但這整篇文章，規劃 」*N* + 1"的案例是極其重要。 每個案例，必須先計算一個 DC 離線的影響。 在其中負載分佈是否平均，以確保 60%的前一個案例中，載入期間 」*N*」 案例中，跨所有伺服器上，平均地平衡負載分佈會正常比例保持一致的。 尋找在 PDC 模擬器調整案例中，且通常任何案例中，使用者或應用程式的負載不對稱，效果會非常不同：

| |調整的使用率|New LdapSrvWeight|估計的新使用率|
|-|-|-|-|
|DC 1 （PDC 模擬器）|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### <a name="virtualization-considerations-for-processing"></a>虛擬化處理考量

有兩個層的容量規劃需要在虛擬化環境中完成。 在主機層級，類似於網域控制站正在處理先前所述的商務週期的識別必須識別在尖峰期間的臨界值。 因為基礎主體都是相同的排程與實體機器上的 CPU 上取得 AD DS 執行緒在 CPU 上的客體執行緒的主機，建議使用相同的目標 40%至 60%的基礎主機上。 在下一層中，客體層，因為執行緒排程的主體未變更，在客體仍在 40%到 60%的範圍內的目標。

在直接對應案例中，每個主機、 一個客體到目前為止完成的所有容量規劃都需要加入基礎主機作業系統的需求 （RAM、 磁碟、 網路）。 在共用的主機案例中，測試指出基礎處理器的效率是 10%的影響。 這表示站台是否需要在目標為 40%，建議的所有配置的虛擬 Cpu 容量的 10 個 Cpu 」*N*"來賓是 11。 在網站中的實體伺服器與虛擬伺服器的混合式分配，修飾詞只適用於 Vm。 例如，如果站台具有"*N* + 1 」 案例中，具有 10 個 Cpu 的一個實體，或按一下 直接對應伺服器會是相當於一個客體與在主機上，具有 11 個 Cpu 保留網域控制站的 11 Cpu 相關。

在整個分析和計算所需的 CPU 數量來支援 AD DS 負載對應至項目可以購買的條款的實體硬體的 Cpu 數目不一定會對應完全。 虛擬化就不需要無條件進位。 虛擬化可減少計算容量新增至站台，提供的輕鬆與 CPU 可以新增至 VM 所需的投入時間。 也不能免除需要準確地評估需要使基礎硬體可供使用時要加入至來賓，需要額外的 Cpu 計算能力。  一如往常，請記得計劃和需求成長的監視。

### <a name="calculation-summary-example"></a>計算摘要範例

|系統|尖峰 CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|所使用的總 CPU|485%|  
  
|目標系統計數|（上述） 的總頻寬|
|-|-|
|在 40%的目標所需的 Cpu|4.85 &divide; .4 = 12.25|

重複的重要性，此點，因為*規劃成長，請記得*。 假設 50%的成長，在未來的三年，這種環境需要 18.375 Cpu (12.25 &times; 1.5) 三年標記處。 其他的計劃會檢閱第一年之後，並視需要新增額外的容量中。

### <a name="cross-trust-client-authentication-load-for-ntlm"></a>跨信任用戶端驗證負載的 NTLM

#### <a name="evaluating-cross-trust-client-authentication-load"></a>評估跨信任用戶端驗證負載

許多環境中可能有一或多個網域的信任連接。 需要周遊信任，使用另一個網域控制站在目的地網域或在路徑中的下一個網域的網域控制站的安全通道不會使用 Kerberos 驗證的另一個網域中的身分識別驗證要求目的地網域。 使用網域控制站可以對受信任網域的網域控制站的安全通道的同時呼叫數目會受到設定，稱為**MaxConcurrentAPI**。 網域控制站，確保安全的通道可處理的負載量透過兩種方法的其中一個： tuning **MaxConcurrentAPI**或內的樹系中，然後再建立捷徑信任。 若要跨個別的信任，量測軌的流量，請參閱[如何使用 MaxConcurrentApi 設定 NTLM 驗證的效能微調](https://support.microsoft.com/kb/2688798)。

在資料收集期間，如同所有其他的案例，必須收集在一天的有用資料的尖峰忙碌。

> [!NOTE]
> 樹系內和樹系間的案例可能會造成周遊多個信任的驗證，而且每個階段則需要進行微調。

#### <a name="planning"></a>規劃

有一些應用程式，根據預設，使用 NTLM 驗證，或在特定的組態案例中使用它。 應用程式伺服器的容量，以及服務越來越多的作用中用戶端。 另外還有用戶端有限的時間，讓工作階段保持開啟，並而重新連線定期執行 （例如電子郵件提取同步處理） 的趨勢。 為高 NTLM 負載的另一個常見範例是供網際網路存取需要驗證的 web proxy 伺服器。

這些應用程式可能會造成極大的負擔，NTLM 驗證，可以在網域控制站，放置重大的壓力，特別是當使用者和資源位於不同的網域。

有多個方法可以管理跨信任層級的負載，這實際上可以用搭配使用而不是以獨佔 / 或案例。 可能的選項包括：

- 找出使用者是位於相同網域中的使用者取用的服務，以減少跨信任用戶端驗證。
- 增加安全-可使用的通道數目。 這適用於樹系內和跨樹系的流量，並稱為捷徑信任。
- 微調的預設設定**MaxConcurrentAPI**。

用來微調**MaxConcurrentAPI**現有的伺服器上的方程式是：

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

如需詳細資訊，請參閱[2688798 的 KB 文章：如何使用 MaxConcurrentApi 設定 NTLM 驗證的效能微調](http://support.microsoft.com/kb/2688798)。

## <a name="virtualization-considerations"></a>虛擬考量

無，這是作業系統調整設定。

### <a name="calculation-summary-example"></a>計算摘要範例

|資料類型|值|
|-|-|
|號誌取得 （最低要求）|6,161|
|號誌取得 （最多）|6,762|
|號誌逾時|0|
|平均的號誌的等候時間|0.012|
|集合持續時間 （秒）|1:11 分鐘 （71 秒）|
|（從 KB 2688798) 的公式|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|最小值**MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

此系統的這段時間中，預設值為可接受的。

## <a name="monitoring-for-compliance-with-capacity-planning-goals"></a>容量規劃目標的合規性監視

在本文中，它已討論過規劃和調整移往使用率的目標。 以下是建議的臨界值必須以確保有足夠容量的臨界值內操作系統監視摘要圖表。 請記住，這些不是效能臨界值，但容量計劃的臨界值。 伺服器，這些閾值超過作業系統可行的但開始驗證所有的應用程式的行為會良好。 如果說過的應用程式會正常執行，就可以開始評估硬體升級或其他組態變更。

|Category|效能計數器|取樣間隔 /|目標|警告|
|-|-|-|-|-|
|處理器|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 or earlier)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long-Term Average Standby Cache Lifetime(s)|30 min|Must be tested|Must be tested|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 min|40%|60%|
|Storage|LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read<br /><br />LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write|60 min|10 ms|15 ms|
|AD Services|Netlogon(\*)\Average Semaphore Hold Time|60 min|0|1 second|

## Appendix A: CPU sizing criteria

### Definitions

**Processor (microprocessor) –** a component that reads and executes program instructions  

**CPU –** Central Processing Unit  

**Multi-Core processor –** multiple CPUs on the same integrated circuit  

**Multi-CPU –** multiple CPUs, not on the same integrated circuit  

**Logical Processor –** one logical computing engine from the perspective of the operating system  

This includes hyper-threaded, one core on multi-core processor, or a single core processor.  

As today’s server systems have multiple processors, multiple multi-core processors, and hyper-threading, this information is generalized to cover both scenarios. As such, the term logical processor will be used as it represents the operating system and application perspective of the available computing engines.

### Thread-level parallelism

Each thread is an independent task, as each thread has its own stack and instructions. Because AD DS is multi-threaded and the number of available threads can be tuned by using [How to view and set LDAP policy in Active Directory by using Ntdsutil.exe](http://support.microsoft.com/kb/315071), it scales well across multiple logical processors.

### Data-level parallelism

This involves sharing data across multiple threads within one process (in the case of the AD DS process alone) and across multiple threads in multiple processes (in general). With concern to over-simplifying the case, this means that any changes to data are reflected to all running threads in all the various levels of cache (L1, L2, L3) across all cores running said threads as well as updating shared memory. Performance can degrade during write operations while all the various memory locations are brought consistent before instruction processing can continue.

### CPU speed vs. multiple-core considerations

The general rule of thumb is faster logical processors reduce the duration it takes to process a series of instructions, while more logical processors means that more tasks can be run at the same time. These rules of thumb break down as the scenarios become inherently more complex with considerations of fetching data from shared-memory, waiting on data-level parallelism, and the overhead of managing multiple threads. This is also why scalability in multi-core systems is not linear.

Consider the following analogies in these considerations: think of a highway, with each thread being an individual car, each lane being a core, and the speed limit being the clock speed.

1. If there is only one car on the highway, it doesn’t matter if there are two lanes or 12 lanes. That car is only going to go as fast as the speed limit will allow.
1. Assume that the data the thread needs is not immediately available. The analogy would be that a segment of road is shutdown. If there is only one car on the highway, it doesn’t matter what the speed limit is until the lane is reopened (data is fetched from memory).
1. As the number of cars increase, the overhead to manage the number of cars increases. Compare the experience of driving and the amount of attention necessary when the road is practically empty (such as late evening) versus when the traffic is heavy (such as mid-afternoon, but not rush hour). Also, consider the amount of attention necessary when driving on a two-lane highway, where there is only one other lane to worry about what the drivers are doing, versus a six-lane highway where one has to worry about what a lot of other drivers are doing.
   > [!NOTE]
   > The analogy about the rush hour scenario is extended in the next section: Response Time/How the System Busyness Impacts Performance.

As a result, specifics about more or faster processors become highly subjective to application behavior, which in the case of AD DS is very environmentally specific and even varies from server to server within an environment. This is why the references earlier in the article do not invest heavily in being overly precise, and a margin of safety is included in the calculations. When making budget-driven purchasing decisions, it is recommended that optimizing usage of the processors at 40% (or the desired number for the environment) occurs first, before considering buying faster processors. The increased synchronization across more processors reduces the true benefit of more processors from the linear progression (2&times; the number of processors provides less than 2&times; available additional compute power).

> [!NOTE]
> Amdahl’s Law and Gustafson’s Law are the relevant concepts here.

### Response time/How the system busyness impacts performance

Queuing theory is the mathematical study of waiting lines (queues). In queuing theory, the Utilization Law is represented by the equation:

*U* k = *B* &divide; *T*

Where *U* k is the utilization percentage, *B* is the amount of time busy, and *T* is the total time the system was observed. Translated into the context of Windows, this means the number of 100-nanosecond (ns) interval threads that are in a Running state divided by how many 100-ns intervals were available in given time interval. This is exactly the formula for calculating % Processor Utility (reference [Processor Object](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10)) and [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10))).

Queuing theory also provides the formula: *N* = *U* k &divide; (1 &ndash; *U* k) to estimate the number of waiting items based on utilization ( *N* is the length of the queue). Charting this over all utilization intervals provides the following estimates to how long the queue to get on the processor is at any given CPU load.

![Queue length](media/capacity-planning-considerations-queue-length.png)

It is observed that after 50% CPU load, on average there is always a wait of one other item in the queue, with a noticeably rapid increase after about 70% CPU utilization.

Returning to the driving analogy used earlier in this section:

- The busy times of “mid-afternoon” would, hypothetically, fall somewhere into the 40% to 70% range. There is enough traffic such that one’s ability to pick any lane is not majorly restricted, and the chance of another driver being in the way, while high, does not require the level of effort to “find” a safe gap between other cars on the road.
- One will notice that as traffic approaches rush hour, the road system approaches 100% capacity. Changing lanes can become very challenging because cars are so close together that increased caution must be exercised to do so.

This is why the long term averages for capacity conservatively estimated at 40% allows for head room for abnormal spikes in load, whether said spikes transitory (such as poorly coded queries that run for a few minutes) or abnormal bursts in general load (the morning of the first day after a long weekend).

The above statement regards % Processor Time calculation being the same as the Utilization Law is a bit of a simplification for the ease of the general reader. For those more mathematically rigorous:  
- Translating the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = The number of 100-ns intervals “Idle” thread spends on the logical processor. The change in the “*X*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation
  - *T* = the total number of 100-ns intervals in a given time range. The change in the “*Y*” variable in the [PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10)) calculation.
  - *U* k = The utilization percentage of the logical processor by the “Idle Thread” or % Idle Time.  
- Working out the math:
  - *U* k = 1 – %Processor Time
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### Applying the concepts to capacity planning

The preceding math may make determinations about the number of logical processors needed in a system seem overwhelmingly complex. This is why the approach to sizing the systems is focused on determining maximum target utilization based on current load and calculating the number of logical processors required to get there. Additionally, while logical processor speeds will have a significant impact on performance, cache efficiencies, memory coherence requirements, thread scheduling and synchronization, and imperfectly balanced client load will all have significant impacts on performance that will vary on a server-by-server basis. With the relatively cheap cost of compute power, attempting to analyze and determine the perfect number of CPUs needed becomes more an academic exercise than it does provide business value.

Forty percent is not a hard and fast requirement, it is a reasonable start. Various consumers of Active Directory require various levels of responsiveness. There may be scenarios where environments can run at 80% or 90% utilization as a sustained average, as the increased wait times for access to the processor will not noticeably impact client performance. It is important to re-iterate that there are many areas in the system that are much slower than the logical processor in the system, including access to RAM, access to disk, and transmitting the response over the network. All of these items need to be tuned in conjunction. Examples:

- Adding more processors to a system running 90% that is disk-bound is probably not going to significantly improve performance. Deeper analysis of the system will probably identify that there are a lot of threads that are not even getting on the processor because they are waiting on I/O to complete.
- Resolving the disk-bound issues potentially means that threads that were previously spending a lot of time in a waiting state will no longer be in a waiting state for I/O and there will be more competition for CPU time, meaning that the 90% utilization in the previous example will go to 100% (because it can not go higher). Both components need to be tuned in conjunction.
  > [!NOTE]
  > Processor Information(*)\\% Processor Utility can exceed 100% with systems that have a "Turbo" mode.  This is where the CPU exceeds the rated processor speed for short periods.  Reference CPU manufacturers documentation and description of the counter for greater insight.  

Discussing whole system utilization considerations also brings into the conversation domain controllers as virtualized guests. [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) applies to both the host and the guest in a virtualized scenario. This is why in a host with only one guest, a domain controller (and generally any system) has near the same performance it does on physical hardware. Adding additional guests to the hosts increases the utilization of the underlying host, thereby increasing the wait times to get access to the processors as explained previously. In short, logical processor utilization needs to be managed at both the host and at the guest levels.

Extending the previous analogies, leaving the highway as the physical hardware, the guest VM will be analogized with a bus (an express bus that goes straight to the destination the rider wants). Imagine the following four scenarios:

- It is off hours, a rider gets on a bus that is nearly empty, and the bus gets on a road that is also nearly empty. As there is no traffic to contend with, the rider has a nice easy ride and gets there just as fast as if the rider had driven instead. The rider’s travel times are still constrained by the speed limit.
- It is off hours so the bus is nearly empty but most of the lanes on the road are closed, so the highway is still congested. The rider is on an almost-empty bus on a congested road. While the rider does not have a lot of competition in the bus for where to sit, the total trip time is still dictated by the rest of the traffic outside.
- It is rush hour so the highway and the bus are congested. Not only does the trip take longer, but getting on and off the bus is a nightmare because people are shoulder to shoulder and the highway is not much better. Adding more busses (logical processors to the guest) does not mean they can fit on the road any more easily, or that the trip will be shortened.
- The final scenario, though it may be stretching the analogy a little, is where the bus is full, but the road is not congested. While the rider will still have trouble getting on and off the bus, the trip will be efficient after the bus is on the road. This is the only scenario where adding more busses (logical processors to the guest) will improve guest performance.

From there it is relatively easy to extrapolate that there are a number of scenarios in between the 0%-utilized and the 100%-utilized state of the road and the 0%- and 100%-utilized state of the bus that have varying degrees of impact.

Applying the principals above of 40% CPU as reasonable target for the host as well as the guest is a reasonable start for the same reasoning as above, the amount of queuing.

## Appendix B: Considerations regarding different processor speeds, and the effect of processor power management on processor speeds

Throughout the sections on processor selection the assumption is made that the processor is running at 100% of clock speed the entire time the data is being collected and that the replacement systems will have the same speed processors. Despite both assumptions in practice being false, particularly with Windows Server 2008 R2 and later, where the default power plan is **Balanced**, the methodology still stands as it is the conservative approach. While the potential error rate may increase, it only increases the margin of safety as processor speeds increase.

- For example, in a scenario where 11.25 CPUs are demanded, if the processors were running at half speed when the data was collected, the more accurate estimate might be 5.125 &divide; 2.
- It is impossible to guarantee that doubling the clock speeds would double the amount of processing that happens for a given time period. This is due to the fact the amount of time that the processors spend waiting on RAM or other system components could stay the same. The net effect is that the faster processors might spend a greater percentage of the time idle while waiting on data to be fetched. Again, it is recommended to stick with the lowest common denominator, being conservative, and avoid trying to calculate a potentially false level of accuracy by assuming a linear comparison between processor speeds.

Alternatively, if processor speeds in replacement hardware are lower than current hardware, it would be safe to increase the estimate of processors needed by a proportionate amount. For example, it is calculated that 10 processors are needed to sustain the load in a site, and the current processors are running at 3.3 Ghz and replacement processors will run at 2.6 Ghz, this is a 21% decrease in speed. In this case, 12 processors would be the recommended amount.

That said, this variability would not change the Capacity Management processor utilization targets. As processor clock speeds will be adjusted dynamically based on the load demanded, running the system under higher loads will generate a scenario where the CPU spends more time in a higher clock speed state, making the ultimate goal to be at 40% utilization in a 100% clock speed state at peak. Anything less than that will generate power savings as CPU speeds will be throttled back during off peak scenarios.

> [!NOTE]
> An option would be to turn off power management on the processors (setting the power plan to **High Performance**) while data is collected. That would give a more accurate representation of the CPU consumption on the target server.

To adjust estimates for different processors, it used to be safe, excluding other system bottlenecks outlined above, to assume that doubling processor speeds doubled the amount of processing that could be performed.  Today, the internal architecture of processors is different enough between processors, that a safer way to gauge the effects of using different processors than data was taken from is to leverage the SPECint_rate2006 benchmark from Standard Performance Evaluation Corporation.

1. Find the SPECint_rate2006 scores for the processor that are in use and that plan to be used.
    1. On the website of the Standard Performance Evaluation Corporation, select **Results**, highlight **CPU2006**, and select **Search all SPECint_rate2006 results**.
    1. Under **Simple Request**, enter the search criteria for the target processor, for example **Processor Matches E5-2630 (baselinetarget)** and **Processor Matches E5-2650 (baseline)**.
    1. Find the server and processor configuration to be used (or something close, if an exact match is not available) and note the value in the **Result** and **# Cores** columns.
1. To determine the modifier use the following equation:
   >((*Target platform per-core score value*) &times; (*MHz per-core of baseline platform*)) &divide; ((*Baseline per-core score value*) &times; (*MHz per-core of target platform*))  

    Using the above example:
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. Multiply the estimated number of processors by the modifier.  In the above case to go from the E5-2650 processor to the E5-2630 processor multiply the calculated 11.25 CPUs &times; 0.92 = 10.35 processors needed.

## Appendix C: Fundamentals regarding the operating system interacting with storage

The queuing theory concepts outlined in [Response time/How the system busyness impacts performance](#response-timehow-the-system-busyness-impacts-performance) are also applicable to storage. Having a familiarity of how the operating system handles I/O is necessary to apply these concepts. In the Microsoft Windows operating system, a queue to hold the I/O requests is created for each physical disk. However, a clarification on physical disk needs to be made. Array controllers and SANs present aggregations of spindles to the operating system as single physical disks. Additionally, array controllers and SANs can aggregate multiple disks into one array set and then split this array set into multiple “partitions”, which is in turn presented to the operating system as multiple physical disks (ref. figure).

![Block spindles](media/capacity-planning-considerations-block-spindles.png)  

In this figure the two spindles are mirrored and split into logical areas for data storage (Data 1 and Data 2). These logical areas are viewed by the operating system as separate physical disks.

Although this can be highly confusing, the following terminology is used throughout this appendix to identify the different entities:

- **Spindle –** the device that is physically installed in the server.
- **Array –** a collection of spindles aggregated by controller.
- **Array partition –** a partitioning of the aggregated array
- **LUN –** an array, used when referring to SANs
- **Disk –** What the operating system observes to be a single physical disk.
- **Partition –** a logical partitioning of what the operating system perceives as a physical disk.

### Operating system architecture considerations

The operating system creates a First In/First Out (FIFO) I/O queue for each disk that is observed; this disk may be representing a spindle, an array, or an array partition. From the operating system perspective, with regard to handling I/O, the more active queues the better. As a FIFO queue is serialized, meaning that all I/Os issued to the storage subsystem must be processed in the order the request arrived. By correlating each disk observed by the operating system with a spindle/array, the operating system now maintains an I/O queue for each unique set of disks, thereby eliminating contention for scarce I/O resources across disks and isolating I/O demand to a single disk. As an exception, Windows Server 2008 introduces the concept of I/O prioritization, and applications designed to use the “Low” priority fall out of this normal order and take a back seat. Applications not specifically coded to leverage the “Low” priority default to “Normal.”

### Introducing simple storage subsystems

Starting with a simple example (a single hard drive inside a computer) a component-by-component analysis will be given. Breaking this down into the major storage subsystem components, the system consists of:

- **1 –** 10,000 RPM Ultra Fast SCSI HD (Ultra Fast SCSI has a 20 MB/s transfer rate)
- **1 –** SCSI Bus (the cable)
- **1 –** Ultra Fast SCSI Adapter
- **1 –** 32-bit 33 MHz PCI bus

Once the components are identified, an idea of how much data can transit the system, or how much I/O can be handled, can be calculated. Note that the amount of I/O and quantity of data that can transit the system is correlated, but not the same. This correlation depends on whether the disk I/O is random or sequential and the block size. (All data is written to the disk as a block, but different applications using different block sizes.) On a component-by-component basis:

- **The hard drive –** The average 10,000-RPM hard drive has a 7-millisecond (ms) seek time and a 3 ms access time. Seek time is the average amount of time it takes the read/write head to move to a location on the platter. Access time is the average amount of time it takes to read or write the data to disk, once the head is in the correct location. Thus, the average time for reading a unique block of data in a 10,000-RPM HD constitutes a seek and an access, for a total of approximately 10 ms (or .010 seconds) per block of data.

  When every disk access requires movement of the head to a new location on the disk, the read/write behavior is referred to as “random.” Thus, when all I/O is random, a 10,000-RPM HD can handle approximately 100 I/O per second (IOPS) (the formula is 1000 ms per second divided by 10 ms per I/O or 1000/10=100 IOPS).

  Alternatively, when all I/O occurs from adjacent sectors on the HD, this is referred to as sequential I/O. Sequential I/O has no seek time because when the first I/O is complete, the read/write head is at the start of where the next block of data is stored on the HD. Thus a 10,000-RPM HD is capable of handling approximately 333 I/O per second (1000 ms per second divided by 3 ms per I/O).

  >[!NOTE]
  >This example does not reflect the disk cache, where the data of one cylinder is typically kept. In this case, the 10 ms are needed on the first I/O and the disk reads the whole cylinder. All other sequential I/O is satisfied from the cache. As a result, in-disk caches might improve sequential I/O performance.
  
  So far, the transfer rate of the hard drive has been irrelevant. Whether the hard drive is 20 MB/s Ultra Wide or an Ultra3 160 MB/s, the actual amount of IOPS the can be handled by the 10,000-RPM HD is ~100 random or ~300 sequential I/O. As block sizes change based on the application writing to the drive, the amount of data that is pulled per I/O is different. For example, if the block size is 8 KB, 100 I/O operations will read from or write to the hard drive a total of 800 KB. However, if the block size is 32 KB, 100 I/O will read/write 3,200 KB (3.2 MB) to the hard drive. As long as the SCSI transfer rate is in excess of the total amount of data transferred, getting a “faster” transfer rate drive will gain nothing. See the following tables for comparison.

  | |7200 RPM 9ms seek, 4ms access|10,000 RPM 7ms seek, 3ms access|15,000 RPM 4ms seek, 2ms access
  |-|-|-|-|
  |Random I/O|80|100|150|
  |Sequential I/O|250|300|500|  
  
  |10,000 RPM drive|8 KB block size (Active Directory Jet)|
  |-|-|
  |Random I/O|800 KB/s|
  |Sequential I/O|2400 KB/s|

- **SCSI backplane (bus) –** Understanding how the “SCSI backplane (bus)”, or in this scenario the ribbon cable, impacts throughput of the storage subsystem depends on knowledge of the block size. Essentially the question would be, how much I/O can the bus handle if the I/O is in 8 KB blocks? In this scenario, the SCSI bus is 20 MB/s, or 20480 KB/s. 20480 KB/s divided by 8 KB blocks yields a maximum of approximately 2500 IOPS supported by the SCSI bus.

  >[!NOTE]
  >The figures in the following table represent an example. Most attached storage devices currently use PCI Express, which provides much higher throughput.  
  
  |I/O supported by SCSI bus per block size|2 KB block size|8 KB block size (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/s|10,000|2,500|
  |40 MB/s|20,000|5,000|
  |128 MB/s|65,536|16,384|
  |320 MB/s|160,000|40,000|

  As can be determined from this chart, in the scenario presented, no matter what the use, the bus will never be a bottleneck, as the spindle maximum is 100 I/O, well below any of the above thresholds.

  >[!NOTE]
  >This assumes that the SCSI bus is 100% efficient.
  
- **SCSI adapter –** For determining the amount of I/O that this can handle, the manufacturer’s specifications need to be checked. Directing I/O requests to the appropriate device requires processing of some sort, thus the amount of I/O that can be handled is dependent on the SCSI adapter (or array controller) processor.

  In this example, the assumption that 1,000 I/O can be handled will be made.

- **PCI bus –** This is an often overlooked component. In this example, this will not be the bottleneck; however as systems scale up, it can become a bottleneck. For reference, a 32 bit PCI bus operating at 33Mhz can in theory transfer 133 MB/s of data. Following is the equation:  
  > 32 bits &divide; 8 bits per byte &times; 33 MHz = 133 MB/s.  

  Note that is the theoretical limit; in reality only about 50% of the maximum is actually reached, although in certain burst scenarios, 75% efficiency can be obtained for short periods.

  A 66Mhz 64-bit PCI bus can support a theoretical maximum of (64 bits &divide; 8 bits per byte &times; 66 Mhz) = 528 MB/sec. Additionally, any other device (such as the network adapter, second SCSI controller, and so on) will reduce the bandwidth available as the bandwidth is shared and the devices will contend for the limited resources.

After analysis of the components of this storage subsystem, the spindle is the limiting factor in the amount of I/O that can be requested, and consequently the amount of data that can transit the system. Specifically, in an AD DS scenario, this is 100 random I/O per second in 8 KB increments, for a total of 800 KB per second when accessing the Jet database. Alternatively, the maximum throughput for a spindle that is exclusively allocated to log files would suffer the following limitations: 300 sequential I/O per second in 8 KB increments, for a total of 2400 KB (2.4 MB) per second.

Now, having analyzed a simple configuration, the following table demonstrates where the bottleneck will occur as components in the storage subsystem are changed or added.

|Notes|Bottleneck analysis|Disk|Bus|Adapter|PCI bus|
|-|-|-|-|-|-|
|This is the domain controller configuration after adding a second disk. The disk configuration represents the bottleneck at 800 KB/s.|Add 1 disk (Total=2)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|200 I/Os total<br />800 KB/s total.| | | |
|After adding 7 disks, the disk configuration still represents the bottleneck at 3200 KB/s.|**Add 7 disks (Total=8)**  <br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD|800 I/Os total.<br />3200 KB/s total| | | |
|After changing I/O to sequential, the network adapter becomes the bottleneck because it is limited to 1000 IOPS.|Add 7 disks (Total=8)<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD| | |2400 I/O sec can be read/written to disk, controller limited to 1000 IOPS| |
|After replacing the network adapter with a SCSI adapter that supports 10,000 IOPS, the bottleneck returns to the disk configuration.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade SCSI adapter (now supports 10,000 I/O)**|800 I/Os total.<br />3,200 KB/s total| | | |
|After increasing the block size to 32 KB, the bus becomes the bottleneck because it only supports 20 MB/s.|Add 7 disks (Total=8)<br /><br />I/O is random<br /><br />**32 KB block size**<br /><br />10,000 RPM HD| |800 I/Os total. 25,600 KB/s (25 MB/s) can be read/written to disk.<br /><br />The bus only supports 20 MB/s| | |
|After upgrading the bus and adding more disks, the disk remains the bottleneck.|**Add 13 disks (Total=14)**<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is random<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />**Upgrade to 320 MB/s SCSI bus**|2800 I/Os<br /><br />11,200 KB/s (10.9 MB/s)| | | |
|After changing I/O to sequential, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI Adapter with 14 disks<br /><br />**I/O is sequential**<br /><br />4 KB block size<br /><br />10,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|After adding faster hard drives, the disk remains the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />4 KB block size<br /><br />**15,000 RPM HD**<br /><br />Upgrade to 320 MB/s SCSI bus|14,000 I/Os<br /><br />56,000 KB/s<br /><br />(54.7 MB/s)| | | |
|After increasing the block size to 32 KB, the PCI bus becomes the bottleneck.|Add 13 disks (Total=14)<br /><br />Add second SCSI adapter with 14 disks<br /><br />I/O is sequential<br /><br />**32 KB block size**<br /><br />15,000 RPM HD<br /><br />Upgrade to 320 MB/s SCSI bus| | | |14,000 I/Os<br /><br />448,000 KB/s<br /><br />(437 MB/s) is the read/write limit to the spindle.<br /><br />The PCI bus supports a theoretical maximum of 133 MB/s (75% efficient at best).|

### Introducing RAID

The nature of a storage subsystem does not change dramatically when an array controller is introduced; it just replaces the SCSI adapter in the calculations. What does change is the cost of reading and writing data to the disk when using the various array levels (such as RAID 0, RAID 1, or RAID 5).

In RAID 0, the data is striped across all the disks in the RAID set. This means that during a read or a write operation, a portion of the data is pulled from or pushed to each disk, increasing the amount of data that can transit the system during the same time period. Thus, in one second, on each spindle (again assuming 10,000-RPM drives), 100 I/O operations can be performed. The total amount of I/O that can be supported is N spindles times 100 I/O per second per spindle (yields 100*N I/O per second).

![Logical d: drive](media/capacity-planning-considerations-logical-d-drive.png)

In RAID 1, the data is mirrored (duplicated) across a pair of spindles for redundancy. Thus, when a read I/O operation is performed, data can be read from both of the spindles in the set. This effectively makes the I/O capacity from both disks available during a read operation. The caveat is that write operations gain no performance advantage in a RAID 1. This is because the same data needs to be written to both drives for the sake of redundancy. Though it does not take any longer, as the write of data occurs concurrently on both spindles, because both spindles are occupied duplicating the data, a write I/O operation in essence prevents two read operations from occurring. Thus, every write I/O costs two read I/O. A formula can be created from that information to determine the total number of I/O operations that are occurring:  

> *Read I/O* + 2 &times; *Write I/O* = *Total available disk I/O consumed*  

When the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array:  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total IOPS*

RAID 1+ 0, behaves exactly the same as RAID 1 regarding the expense of reading and writing. However, the I/O is now striped across each mirrored set. If  

> *Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] = *Total I/O*  

in a RAID 1 set, when a multiplicity (*N*) of RAID 1 sets are striped, the Total I/O that can be processed becomes N &times; I/O per RAID 1 set:  

> *N* &times; {*Maximum IOPS per spindle* &times; 2 spindles &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 2 &times; *%Writes*)] } = *Total IOPS*

In RAID 5, sometimes referred to as *N* + 1 RAID, the data is striped across *N* spindles and parity information is written to the “+ 1” spindle. However, RAID 5 is much more expensive when performing a write I/O than RAID 1 or 1 + 0. RAID 5 performs the following process every time a write I/O is submitted to the array:

1. Read the old data
1. Read the old parity
1. Write the new data
1. Write the new parity

As every write I/O request that is submitted to the array controller by the operating system requires four I/O operations to complete, write requests submitted take four times as long to complete as a single read I/O. To derive a formula to translate I/O requests from the operating system perspective to that experienced by the spindles:  

> *Read I/O* + 4 &times; *Write I/O* = *Total I/O*  

Similarly in a RAID 1 set, when the ratio of reads to writes and the number of spindles are known, the following equation can be derived from the above equation to identify the maximum I/O that can be supported by the array (Note that total number of spindles does not include the “drive” lost to parity):  

> *IOPS per spindle* &times; (*Spindles* – 1) &times; [(*%Reads* + *%Writes*) &divide; (*%Reads* + 4 &times; *%Writes*)] = *Total IOPS*

### Introducing SANs

Expanding the complexity of the storage subsystem, when a SAN is introduced into the environment, the basic principles outlined do not change, however I/O behavior for all of the systems connected to the SAN needs to be taken into account. As one of the major advantages in using a SAN is an additional amount of redundancy over internally or externally attached storage, capacity planning now needs to take into account fault tolerance needs. Also, more components are introduced that need to be evaluated. Breaking a SAN down into the component parts:

- SCSI or Fibre Channel hard drive
- Storage unit channel backplane
- Storage units
- Storage controller module
- SAN switch(es)
- HBA(s)
- The PCI bus

When designing any system for redundancy, additional components are included to accommodate the potential of failure. It is very important, when capacity planning, to exclude the redundant component from available resources. For example, if the SAN has two controller modules, the I/O capacity of one controller module is all that should be used for total I/O throughput available to the system. This is due to the fact that if one controller fails, the entire I/O load demanded by all connected systems will need to be processed by the remaining controller. As all capacity planning is done for peak usage periods, redundant components should not be factored into the available resources and planned peak utilization should not exceed 80% saturation of the system (in order to accommodate bursts or anomalous system behavior). Similarly, the redundant SAN switch, storage unit, and spindles should not be factored into the I/O calculations.

When analyzing the behavior of the SCSI or Fibre Channel hard drive, the method of analyzing the behavior as outlined previously does not change. Although there are certain advantages and disadvantages to each protocol, the limiting factor on a per disk basis is the mechanical limitation of the hard drive.

Analyzing the channel on the storage unit is exactly the same as calculating the resources available on the SCSI bus, or bandwidth (such as 20 MB/s) divided by block size (such as 8 KB). Where this deviates from the simple previous example is in the aggregation of multiple channels. For example, if there are 6 channels, each supporting 20 MB/s maximum transfer rate, the total amount of I/O and data transfer that is available is 100 MB/s (this is correct, it is not 120 MB/s). Again, fault tolerance is a major player in this calculation, in the event of the loss of an entire channel, the system is only left with 5 functioning channels. Thus, to ensure continuing to meet performance expectations in the event of failure, total throughput for all of the storage channels should not exceed 100 MB/s (this assumes load and fault tolerance is evenly distributed across all channels). Turning this into an I/O profile is dependent on the behavior of the application. In the case of Active Directory Jet I/O, this would correlate to approximately 12,500 I/O per second (100 MB/s &divide; 8 KB per I/O).

Next, obtaining the manufacturer’s specifications for the controller modules is required in order to gain an understanding of the throughput each module can support. In this example, the SAN has two controller modules that support 7,500 I/O each. The total throughput of the system may be 15,000 IOPS if redundancy is not desired. In calculating maximum throughput in the case of failure, the limitation is the throughput of one controller, or 7,500 IOPS. This threshold is well below the 12,500 IOPS (assuming 4 KB block size) maximum that can be supported by all of the storage channels, and thus, is currently the bottleneck in the analysis. Still for planning purposes, the desired maximum I/O to be planned for would be 10,400 I/O.

When the data exits the controller module, it transits a Fibre Channel connection rated at 1 GB/s (or 1 Gigabit per second). To correlate this with the other metrics, 1 GB/s turns into 128 MB/s (1 GB/s &divide; 8 bits/byte). As this is in excess of the total bandwidth across all channels in the storage unit (100 MB/s), this will not bottleneck the system. Additionally, as this is only one of the two channels (the additional 1 GB/s Fibre Channel connection being for redundancy), if one connection fails, the remaining connection still has enough capacity to handle all the data transfer demanded.

En route to the server, the data will most likely transit a SAN switch. As the SAN switch has to process the incoming I/O request and forward it out the appropriate port, the switch will have a limit to the amount of I/O that can be handled, however, manufacturers specifications will be required to determine what that limit is. For example, if there are two switches and each switch can handle 10,000 IOPS, the total throughput will be 20,000 IOPS. Again, fault tolerance being a concern, if one switch fails, the total throughput of the system will be 10,000 IOPS. As it is desired not to exceed 80% utilization in normal operation, using no more than 8000 I/O should be the target.

Finally, the HBA installed in the server would also have a limit to the amount of I/O that it can handle. Usually, a second HBA is installed for redundancy, but just like with the SAN switch, when calculating maximum I/O that can be handled, the total throughput of *N* &ndash; 1 HBAs is what the maximum scalability of the system is.

### Caching considerations

Caches are one of the components that can significantly impact the overall performance at any point in the storage system. Detailed analysis about caching algorithms is beyond the scope of this article; however, some basic statements about caching on disk subsystems are worth illuminating:

- Caching does improved sustained sequential write I/O as it can buffer many smaller write operations into larger I/O blocks and de-stage to storage in fewer, but larger block sizes. This will reduce total random I/O and total sequential I/O, thus providing more resource availability for other I/O.
- Caching does not improve sustained write I/O throughput of the storage subsystem. It only allows for the writes to be buffered until the spindles are available to commit the data. When all the available I/O of the spindles in the storage subsystem is saturated for long periods, the cache will eventually fill up. In order to empty the cache, enough time between bursts, or extra spindles, need to be allotted in order to provide enough I/O to allow the cache to flush.

  Larger caches only allow for more data to be buffered. This means longer periods of saturation can be accommodated.

  In a normally operating storage subsystem, the operating system will experience improved write performance as the data only needs to be written to cache. Once the underlying media is saturated with I/O, the cache will fill and write performance will return to disk speed.

- When caching read I/O, the scenario where the cache is most advantageous is when the data is stored sequentially on the disk, and the cache can read-ahead (it makes the assumption that the next sector contains the data that will be requested next).
- When read I/O is random, caching at the drive controller is unlikely to provide any enhancement to the amount of data that can be read from the disk. Any enhancement is non-existent if the operating system or application-based cache size is greater than the hardware-based cache size.

  In the case of Active Directory, the cache is only limited by the amount of RAM.

### SSD considerations

SSDs are a completely different animal than spindle-based hard disks. Yet the two key criteria remain: “How many IOPS can it handle?” and “What is the latency for those IOPS?” In comparison to spindle-based hard disks, SSDs can handle higher volumes of I/O and can have lower latencies. In general and as of this writing, while SSDs are still expensive in a cost-per-Gigabyte comparison, they are very cheap in terms of cost-per-I/O and deserve significant consideration in terms of storage performance.

Considerations:

- Both IOPS and latencies are very subjective to the manufacturer designs and in some cases have been observed to be poorer performing than spindle based technologies. In short, it is more important to review and validate the manufacturer specs drive by drive and not assume any generalities.
- IOPS types can have very different numbers depending on whether it is read or write. AD DS services, in general, being predominantly read-based, will be less affected than some other application scenarios.
- “Write endurance” – this is the concept that SSD cells will eventually wear out. Various manufacturers deal with this challenge different fashions. At least for the database drive, the predominantly read I/O profile allows for downplaying the significance of this concern as the data is not highly volatile.

### Summary

One way to think about storage is picturing household plumbing. Imagine the IOPS of the media that the data is stored on is the household main drain. When this is clogged (such as roots in the pipe) or limited (it is collapsed or too small), all the sinks in the household back up when too much water is being used (too many guests). This is perfectly analogous to a shared environment where one or more systems are leveraging shared storage on an SAN/NAS/iSCSI with the same underlying media. Different approaches can be taken to resolve the different scenarios:

- A collapsed or undersized drain requires a full scale replacement and fix. This would be similar to adding in new hardware or redistributing the systems using the shared storage throughout the infrastructure.
- A “clogged” pipe usually means identification of one or more offending problems and removal of those problems. In a storage scenario this could be storage or system level backups, synchronized antivirus scans across all servers, and synchronized defragmentation software running during peak periods.

In any plumbing design, multiple drains feed into the main drain. If anything stops up one of those drains or a junction point, only the things behind that junction point back up. In a storage scenario, this could be an overloaded switch (SAN/NAS/iSCSI scenario), driver compatibility issues (wrong driver/HBA Firmware/storport.sys combination), or backup/antivirus/defragmentation. To determine if the storage “pipe” is big enough, IOPS and I/O size needs to be measured. At each joint add them together to ensure adequate “pipe diameter.”

## Appendix D - Discussion on storage troubleshooting - Environments where providing at least as much RAM as the database size is not a viable option

It is helpful to understand why these recommendations exist so that the changes in storage technology can be accommodated. These recommendations exist for two reasons. The first is isolation of IO, such that performance issues (that is, paging) on the operating system spindle do not impact performance of the database and I/O profiles. The second is that log files for AD DS (and most databases) are sequential in nature, and spindle-based hard drives and caches have a huge performance benefit when used with sequential I/O as compared to the more random I/O patterns of the operating system and almost purely random I/O patterns of the AD DS database drive. By isolating the sequential I/O to a separate physical drive, throughput can be increased. The challenge presented by today’s storage options is that the fundamental assumptions behind these recommendations are no longer true. In many virtualized storage scenarios, such as iSCSI, SAN, NAS, and Virtual Disk image files, the underlying storage media is shared across multiple hosts, thus completely negating both the “isolation of IO” and the “sequential I/O optimization” aspects. In fact these scenarios add an additional layer of complexity in that other hosts accessing the shared media can degrade responsiveness to the domain controller.

In planning storage performance, there are three categories to consider: cold cache state, warmed cache state, and backup/restore. The cold cache state occurs in scenarios such as when the domain controller is initially rebooted or the Active Directory service is restarted and there is no Active Directory data in RAM. Warm cache state is where the domain controller is in a steady state and the database is cached. These are important to note as they will drive very different performance profiles, and having enough RAM to cache the entire database does not help performance when the cache is cold. One can consider performance design for these two scenarios with the following analogy, warming the cold cache is a “sprint” and running a server with a warm cache is a “marathon.”

For both the cold cache and warm cache scenario, the question becomes how fast the storage can move the data from disk into memory. Warming the cache is a scenario where, over time, performance improves as more queries reuse data, the cache hit rate increases, and the frequency of needing to go to disk decreases. As a result the adverse performance impact of going to disk decreases. Any degradation in performance is only transient while waiting for the cache to warm and grow to the maximum, system-dependent allowed size. The conversation can be simplified to how quickly the data can be gotten off of disk, and is a simple measure of the IOPS available to Active Directory, which is subjective to IOPS available from the underlying storage. From a planning perspective, because warming the cache and backup/restore scenarios happen on an exceptional basis, normally occur off hours, and are subjective to the load of the DC, general recommendations do not exist except in that these activities be scheduled for non-peak hours.

AD DS, in most scenarios, is predominantly read IO, usually a ratio of 90% read/10% write. Read I/O often tends to be the bottleneck for user experience, and with write IO, causes write performance to degrade. As I/O to the NTDS.dit is predominantly random, caches tend to provide minimal benefit to read IO, making it that much more important to configure the storage for read I/O profile correctly.

For normal operating conditions, the storage planning goal is minimize the wait times for a request from AD DS to be returned from disk. This essentially means that the number of outstanding and pending I/O is less than or equivalent to the number of pathways to the disk. There are a variety of ways to measure this. In a performance monitoring scenario, the general recommendation is that LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read be less than 20 ms. The desired operating threshold must be much lower, preferably as close to the speed of the storage as possible, in the 2 to 6 millisecond (.002 to .006 second) range depending on the type of storage.

Example:

![Storage latency chart](media/capacity-planning-considerations-storage-latency.png)

Analyzing the chart:

- **Green oval on the left –** The latency remains consistent at 10 ms. The load increases from 800 IOPS to 2400 IOPS. This is the absolute floor to how quickly an I/O request can be processed by the underlying storage. This is subject to the specifics of the storage solution.
- **Burgundy oval on the right –** The throughput remains flat from the exit of the green circle through to the end of the data collection while the latency continues to increase. This is demonstrating that when the request volumes exceed the physical limitations of the underlying storage, the longer the requests spend sitting in the queue waiting to be sent out to the storage subsystem.

Applying this knowledge:

- **Impact to a user querying membership of a large group –** Assume this requires reading 1 MB of data from the disk, the amount of I/O and how long it takes can be evaluated as follows:
  - Active Directory database pages are 8 KB in size. 
  - A minimum of 128 pages need to be read in from disk. 
  - Assuming nothing is cached, at the floor (10 ms) this is going to take a minimum 1.28 seconds to load the data from disk in order to return it to the client. At 20 ms, where the throughput on storage has long since maxed out and is also the recommended maximum, it will take 2.5 seconds to get the data from disk in order to return it to the end user.  
- **At what rate will the cache be warmed –** Making the assumption that the client load is going to maximize the throughput on this storage example, the cache will warm at a rate of 2400 IOPS &times; 8 KB per IO. Or, approximately 20 MB/s per second, loading about 1 GB of database into RAM every 53 seconds.

> [!NOTE]
> It is normal for short periods to observe the latencies climb when components aggressively read or write to disk, such as when the system is being backed up or when AD DS is running garbage collection. Additional head room on top of the calculations should be provided to accommodate these periodic events. The goal being to provide enough throughput to accommodate these scenarios without impacting normal function.

As can be seen, there is a physical limit based on the storage design to how quickly the cache can possibly warm. What will warm the cache are incoming client requests up to the rate that the underlying storage can provide. Running scripts to “pre-warm” the cache during peak hours will provide competition to load driven by real client requests. That can adversely affect delivering data that clients need first because, by design, it will generate competition for scarce disk resources as artificial attempts to warm the cache will load data that is not relevant to the clients contacting the DC.處理器 Information(_Total)\\' 處理器公用程式 Domain Services
description: Detailed discussion of the factors to consider during capacity planning for AD DS.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; kenbrunf
author: Teresa-Motiv
ms.date: 7/3/2019
---

# Capacity planning for Active Directory Domain Services

This topic is originally written by Ken Brumfield, Senior Premier Field Engineer at Microsoft, and provides recommendations for capacity planning for Active Directory Domain Services (AD DS).

## Goals of capacity planning

Capacity planning is not the same as troubleshooting performance incidents. They are closely related, but quite different. The goals of capacity planning are:  

- Properly implement and operate an environment 
- Minimize the time spent troubleshooting performance issues.  
  
In capacity planning, an organization might have a baseline target of 40% processor utilization during peak periods in order to meet client performance requirements and accommodate the time necessary to upgrade the hardware in the datacenter. Whereas, to be notified of abnormal performance incidents, a monitoring alert threshold might be set at 90% over a 5 minute interval.

The difference is that when a capacity management threshold is continually exceeded (a one-time event is not a concern), adding capacity (that is, adding in more or faster processors) would be a solution or scaling the service across multiple servers would be a solution. Performance alert thresholds indicate that client experience is currently suffering and immediate steps are needed to address the issue.

As an analogy: capacity management is about preventing a car accident (defensive driving, making sure the brakes are working properly, and so on) whereas performance troubleshooting is what the police, fire department, and emergency medical professionals do after an accident. This is about “defensive driving,” Active Directory-style.

Over the last several years, capacity planning guidance for scale-up systems has changed dramatically. The following changes in system architectures have challenged fundamental assumptions about designing and scaling a service:

- 64-bit server platforms  
- Virtualization  
- Increased attention to power consumption  
- SSD storage  
- Cloud scenarios  

Additionally, the approach is shifting from a server-based capacity planning exercise to a service-based capacity planning exercise. Active Directory Domain Services (AD DS), a mature distributed service that many Microsoft and third-party products use as a backend, becomes one the most critical products to plan correctly to ensure the necessary capacity for other applications to run.

### Baseline requirements for capacity planning guidance

Throughout this article, the following baseline requirements are expected:

- Readers have read and are familiar with [Performance Tuning Guidelines for Windows Server 2012 R2](https://docs.microsoft.com/previous-versions//dn529133(v=vs.85)).
- The Windows Server platform is an x64 based architecture. But even if your Active Directory environment is installed on Windows Server 2003 x86 (now beyond the end of the support lifecycle) and has a directory information tree (DIT) that is less 1.5 GB in size and that can easily be held in memory, the guidelines from this article are still applicable.
- Capacity planning is a continuous process and you should regularly review how well the environment is meeting expectations.
- Optimization will occur over multiple hardware lifecycles as hardware costs change. For example, memory becomes cheaper, the cost per core decreases, or the price of different storage options change.
- Plan for the peak busy period of the day. It is recommended to look at this in either 30 minute or hour intervals. Anything greater may hide the actual peaks and anything less may be distorted by “transient spikes.”
- Plan for growth over the course of the hardware lifecycle for the enterprise. This may include a strategy of upgrading or adding hardware in a staggered fashion, or a complete refresh every three to five years. Each will require a “guess” as how much the load on Active Directory will grow. Historical data, if collected, will help with this assessment. 
- Plan for fault tolerance. Once an estimate *N* is derived, plan for scenarios that include *N* &ndash; 1, *N* &ndash; 2, *N* &ndash; *x*.
  - Add in additional servers according to organizational need to ensure that the loss of a single or multiple servers does not exceed maximum peak capacity estimates.
  - Also consider that the growth plan and fault tolerance plan need to be integrated. For example, if one DC is required to support the load, but the estimate is that the load will be doubled in the next year and require two DCs total, there will not be enough capacity to support fault tolerance. The solution would be to start with three DCs. One could also plan to add the third DC after 3 or 6 months if budgets are tight.

    >[!NOTE]
    >Adding Active Directory-aware applications might have a noticeable impact on the DC load, whether the load is coming from the application servers or clients.

### Three-step process for the capacity planning cycle

In capacity planning, first decide what quality of service is needed. For example, a core datacenter supports a higher level of concurrency and requires more consistent experience for users and consuming applications, which requires greater attention to redundancy and minimizing system and infrastructure bottlenecks. In contrast, a satellite location with a handful of users does not need the same level of concurrency or fault tolerance. Thus, the satellite office might not need as much attention to optimizing the underlying hardware and infrastructure, which may lead to cost savings. All recommendations and guidance herein are for optimal performance, and can be selectively relaxed for scenarios with less demanding requirements.

The next question is: virtualized or physical? From a capacity planning perspective, there is no right or wrong answer; there is only a different set of variables to work with. Virtualization scenarios come down to one of two options:

- “Direct mapping” with one guest per host (where virtualization exists solely to abstract the physical hardware from the server)
- “Shared host”

Testing and production scenarios indicate that the “direct mapping” scenario can be treated identically to a physical host. “Shared host,” however, introduces a number of considerations spelled out in more detail later. The “shared host” scenario means that AD DS is also competing for resources, and there are penalties and tuning considerations for doing so.

With these considerations in mind, the capacity planning cycle is an iterative three-step process:

1. Measure the existing environment, determine where the system bottlenecks currently are, and get environmental basics necessary to plan the amount of capacity needed.
1. Determine the hardware needed according to the criteria outlined in step 1.
1. Monitor and validate that the infrastructure as implemented is operating within specifications. Some data collected in this step becomes the baseline for the next cycle of capacity planning.

### Applying the process

To optimize performance, ensure these major components are correctly selected and tuned to the application loads:

1. Memory
1. Network
1. Storage
1. Processor
1. Net Logon

The basic storage requirements of AD DS and the general behavior of well written client software allow environments with as many as 10,000 to 20,000 users to forego heavy investment in capacity planning with regards to physical hardware, as almost any modern server class system will handle the load. That said, the following table summarizes how to evaluate an existing environment in order to select the right hardware. Each component is analyzed in detail in subsequent sections to help AD DS administrators evaluate their infrastructure using baseline recommendations and environment-specific principals.

In general:

- Any sizing based on current data will only be accurate for the current environment.
- For any estimates, expect demand to grow over the lifecycle of the hardware.
- Determine whether to oversize today and grow into the larger environment, or add capacity over the lifecycle.
- For virtualization, all the same capacity planning principals and methodologies apply, except that the overhead of the virtualization needs to be added to anything domain related.
- Capacity planning, like anything that attempts to predict, is NOT an accurate science. Do not expect things to calculate perfectly and with 100% accuracy. The guidance here is the leanest recommendation; add in capacity for additional safety and continuously validate that the environment remains on target.

### Data collection summary tables

#### New environment

| Component | Estimates |
|-|-|
|Storage/Database Size|40 KB to 60 KB for each user|
|RAM|Database Size<br />Base operating system recommendations<br />Third-party applications|
|Network|1 GB|
|CPU|1000 concurrent users for each core|

#### High-level evaluation criteria

| Component | Evaluation criteria | Planning considerations |
|-|-|-|
|Storage/Database size|The section entitled “To activate logging of disk space that is freed by defragmentation” in [Storage Limits](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10))| |
|Storage/ Database performance|<ul><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Write," "LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer"</li><li>"LogicalDisk(*\<NTDS Database Drive\>*)\Reads/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Writes/sec," "LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec"</li></ul>|<ul><li>Storage has two concerns to address<ul><li>Space available, which with the size of today’s spindle based and SSD based storage is irrelevant for most AD environments.</li> <li>Input/Output (IO) Operations available – In many environments, this is often overlooked. But it is important to evaluate only environments where there is not enough RAM to load the entire NTDS Database into memory.</li></ul><li>Storage can be a complex topic and should involve hardware vendor expertise for proper sizing. Particularly with more complex scenarios such as SAN, NAS, and iSCSI scenarios. However, in general, cost per Gigabyte of storage is often in direct opposition to cost per IO:<ul><li>RAID 5 has lower cost per Gigabyte than Raid 1, but Raid 1 has lower cost per IO</li><li>Spindle-based hard drives have lower cost per Gigabyte, but SSDs have a lower cost per IO</li></ul><li>After a restart of the computer or the Active Directory Domain Services service, the Extensible Storage Engine (ESE) cache is empty and performance will be disk bound while the cache warms.</li><li>In most environments AD is read intensive I/O in a random pattern to disks, negating much of the benefit of caching and read optimization strategies.  Plus, AD has a way larger cache in memory than most storage system caches.</li></ul>
|RAM|<ul><li>Database size</li><li>Base operating system recommendations</li><li>Third-party applications</li></ul>|<ul><li>Storage is the slowest component in a computer. The more that can be resident in RAM, the less it is necessary to go to disk.</li><li>Ensure enough RAM is allocated to store the operating system, Agents (antivirus, backup, monitoring), NTDS Database and growth over time.</li><li>For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.</li></ul>|
|Network|<ul><li>“Network Interface(\*)\Bytes Received/sec”</li><li>“Network Interface(\*)\Bytes Sent/sec”|<ul><li>In general, traffic sent from a DC far exceeds traffic sent to a DC.</li><li>As a switched Ethernet connection is full-duplex, inbound and outbound network traffic need to be sized independently.</li><li>Consolidating the number of DCs will increase the amount of bandwidth used to send responses back to client requests for each DC, but will be close enough to linear for the site as a whole.</li><li>If removing satellite location DCs, don’t forget to add the bandwidth for the satellite DC into the hub DCs as well as use that to evaluate how much WAN traffic there will be.</li></ul>|
|CPU|<ul><li>“Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read”</li><li>“Process(lsass)\\% Processor Time”</li></ul>|<ul><li>After eliminating storage as a bottleneck, address the amount of compute power needed.</li><li>While not perfectly linear, the number of processor cores consumed across all servers within a specific scope (such as a site) can be used to gauge how many processors are necessary to support the total client load. Add the minimum necessary to maintain the current level of service across all the systems within the scope.</li><li>Changes in processor speed, including power management related changes, impact numbers derived from the current environment. Generally, it is impossible to precisely evaluate how going from a 2.5 GHz processor to a 3 GHz processor will reduce the number of CPUs needed.</li></ul>|
|NetLogon|<ul><li>“Netlogon(\*)\Semaphore Acquires”</li><li>“Netlogon(\*)\Semaphore Timeouts”</li><li>“Netlogon(\*)\Average Semaphore Hold Time”</li></ul>|<ul><li>Net Logon Secure Channel/MaxConcurrentAPI only affects environments with NTLM authentications and/or PAC Validation. PAC Validation is on by default in operating system versions before Windows Server 2008. This is a client setting, so the DCs will be impacted until this is turned off on all client systems.</li><li>Environments with significant cross trust authentication, which includes intra-forest trusts, have greater risk if not sized properly.</li><li>Server consolidations will increase concurrency of cross-trust authentication.</li><li>Surges need to be accommodated, such as cluster fail-overs, as users re-authenticate en masse to the new cluster node.</li><li>Individual client systems (such as a cluster) might need tuning too.</li></ul>|

## Planning

For a long time, the community’s recommendation for sizing AD DS has been to “put in as much RAM as the database size.” For the most part, that recommendation is all that most environments needed to be concerned about. But the ecosystem consuming AD DS has gotten much bigger, as have the AD DS environments themselves, since its introduction in 1999. Although the increase in compute power and the switch from x86 architectures to x64 architectures has made the subtler aspects of sizing for performance irrelevant to a larger set of customers running AD DS on physical hardware, the growth of virtualization has reintroduced the tuning concerns to a larger audience than before.

The following guidance is thus about how to determine and plan for the demands of Active Directory as a service regardless of whether it is deployed in a physical, a virtual/physical mix, or a purely virtualized scenario. As such, we will break down the evaluation to each of the four main components: storage, memory, network, and processor. In short, in order to maximize performance on AD DS, the goal is to get as close to processor bound as possible.

## RAM

Simply, the more that can be cached in RAM, the less it is necessary to go to disk. To maximize the scalability of the server the minimum amount of RAM should be the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). An additional amount should be added to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth based on environmental changes.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage  section to ensure that storage is properly designed.

A corollary that comes up in the general context in sizing memory is sizing of the page file. In the same context as everything else memory related, the goal is to minimize going to the much slower disk. Thus the question should go from, “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Evaluating

The amount of RAM that a domain controller (DC) needs is actually a complex exercise for these reasons:

- High potential for error when trying to use an existing system to gauge how much RAM is needed as LSASS will trim under memory pressure conditions, artificially deflating the need.
- The subjective fact that an individual DC only needs to cache what is “interesting” to its clients. This means that the data that needs to be cached on a DC in a site with only an Exchange server will be very different than the data that needs to be cached on a DC that only authenticates users.
- The labor to evaluate RAM for each DC on a case-by-case basis is prohibitive and changes as the environment changes.
- The criteria behind the recommendation will help to make informed decisions: 
- The more that can be cached in RAM, the less it is necessary to go to disk. 
- Storage is by far the slowest component of a computer. Access to data on spindle-based and SSD storage media is on the order of 1,000,000x slower than access to data in RAM.

Thus, in order to maximize the scalability of the server, the minimum amount of RAM is the sum of the current database size, the total SYSVOL size, the operating system recommended amount, and the vendor recommendations for the agents (antivirus, monitoring, backup, and so on). Add additional amounts to accommodate growth over the lifetime of the server. This will be environmentally subjective based on estimates of database growth. However, for satellite locations with a small set of end users, these requirements can be relaxed as these sites will not need to cache as much to service most of the requests.

For environments where maximizing the amount of RAM is not cost effective (such as a satellite locations) or not feasible (DIT is too large), reference the Storage section to ensure that storage is properly sized.

> [!NOTE]
> A corollary while sizing memory is sizing of the page file. Because the goal is to minimize going to the much slower disk, the question goes from “how should the page file be sized?” to “how much RAM is needed to minimize paging?” The answer to the latter question is outlined in the rest of this section. This leaves most of the discussion for sizing the page file to the realm of general operating system recommendations and the need to configure the system for memory dumps, which are unrelated to AD DS performance.

### Virtualization considerations for RAM

Avoid memory over-commit at the host. The fundamental goal behind optimizing the amount of RAM is to minimize the amount of time spent going to disk. In virtualization scenarios, the concept of memory over-commit exists where more RAM is allocated to the guests then exists on the physical machine. This in and of itself is not a problem. It becomes a problem when the total memory actively used by all the guests exceeds the amount of RAM on the host and the underlying host starts paging. Performance becomes disk-bound in cases where the domain controller is going to the NTDS.dit to get data, or the domain controller is going to the page file to get data, or the host is going to disk to get data that the guest thinks is in RAM.

### Calculation summary example

|Component|Estimated memory (example)|
|-|-|
|Base operating system recommended RAM (Windows Server 2008)|2 GB|
|LSASS internal tasks|200 MB|
|Monitoring agent|100 MB|
|Antivirus|100 MB|
|Database (Global Catalog)|8.5 GB are you sure ???|
|Cushion for backup to run, administrators to log on without impact|1 GB|
|Total|12 GB|

**Recommended: 16 GB**

Over time, the assumption can be made that more data will be added to the database and the server will probably be in production for 3 to 5 years. Based on an estimate of growth of 33%, 16 GB would be a reasonable amount of RAM to put in a physical server. In a virtual machine, given the ease with which settings can be modified and RAM can be added to the VM, starting at 12 GB with the plan to monitor and upgrade in the future is reasonable.

## Network

### Evaluating
This section is less about evaluating the demands regarding replication traffic, which is focused on traffic traversing the WAN and is thoroughly covered in [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)), than it is about evaluating total bandwidth and network capacity needed, inclusive of client queries, Group Policy applications, and so on. For existing environments, this can be collected by using performance counters “Network Interface(\*)\Bytes Received/sec,” and “Network Interface(\*)\Bytes Sent/sec.” Sample intervals for Network Interface counters in either 15, 30, or 60 分鐘utes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

> [!NOTE]
> Generally, the majority of network traffic on a DC is outbound as the DC responds to client queries. This is the reason for the focus on outbound traffic, though it is recommended to evaluate each environment for inbound traffic also. The same approaches can be used to address and review inbound network traffic requirements. For more information, see Knowledge Base article [929851: The default dynamic port range for TCP/IP has changed in Windows Vista and in Windows Server 2008](http://support.microsoft.com/kb/929851).

### Bandwidth needs

Planning for network scalability covers two distinct categories: the amount of traffic and the CPU load from the network traffic. Each of these scenarios is straight-forward compared to some of the other topics in this article.

In evaluating how much traffic must be supported, there are two unique categories of capacity planning for AD DS in terms of network traffic. The first is replication traffic that traverses between domain controllers and is covered thoroughly in the reference [Active Directory Replication Traffic](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742457(v=technet.10)) and is still relevant to current versions of AD DS. The second is the intrasite client-to-server traffic. One of the simpler scenarios to plan for, intrasite traffic predominantly receives small requests from clients relative to the large amounts of data sent back to the clients. 100 MB will generally be adequate in environments up to 5,000 users per server, in a site. Using a 1 GB network adapter and Receive Side Scaling (RSS) support is recommended for anything above 5,000 users. To validate this scenario, particularly in the case of server consolidation scenarios, look at Network Interface(\*)\Bytes/sec across all the DCs in a site, add them together, and divide by the target number of domain controllers to ensure that there is adequate capacity. The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (formerly known as Perfmon), making sure all of the counters are scaled the same.

Consider the following example (also known as, a really, really complex way to validate that the general rule is applicable to a specific environment). The following assumptions are made:

- The goal is to reduce the footprint to as few servers as possible. Ideally, one server will carry the load and an additional server is deployed for redundancy (*N* + 1 scenario). 
- In this scenario, the current network adapter supports only 100 MB and is in a switched environment.  
  The maximum target network bandwidth utilization is 60% in an N scenario (loss of a DC).
- Each server has about 10,000 clients connected to it.

Knowledge gained from the data in the chart (Network Interface(\*)\Bytes Sent/sec):

1. The business day starts ramping up around 5:30 and winds down at 7:00 PM.
1. The peak busiest period is from 8:00 AM to 8:15 AM, with greater than 25 Bytes sent/sec on the busiest DC.  
   > [!NOTE]
   > All performance data is historical. So the peak data point at 8:15 indicates the load from 8:00 to 8:15.
1. There are spikes before 4:00 AM, with more than 20 Bytes sent/sec on the busiest DC, which could indicate either load from different time zones or background infrastructure activity, such as backups. Since the peak at 8:00 AM exceeds this activity, it is not relevant.
1. There are five Domain Controllers in the site.
1. The max load is about 5.5 MB/s per DC, which represents 44% of the 100 MB connection. Using this data, it can be estimated that the total bandwidth needed between 8:00 AM and 8:15 AM is 28 MB/s.
   >[!NOTE]
   >Be careful with the fact that Network Interface sent/receive counters are in bytes and network bandwidth is measured in bits. 100 MB &divide; 8 = 12.5 MB, 1 GB &divide; 8 = 128 MB.
  
Conclusions:

1. This current environment does meet the N+1 level of fault tolerance at 60% target utilization. Taking one system offline will shift the bandwidth per server from about 5.5 MB/s (44%) to about 7 MB/s (56%).
1. Based on the previously stated goal of consolidating to one server, this both exceeds the maximum target utilization and theoretically the possible utilization of a 100 MB connection.
1. With a 1 GB connection this will represent 22% of the total capacity.
1. Under normal operating conditions in the *N* + 1 scenario, client load will be relatively evenly distributed at about 14 MB/s per server or 11% of total capacity.
1. To ensure that capacity is adequate during unavailability of a DC, the normal operating targets per server would be about 30% network utilization or 38 MB/s per server. Failover targets would be 60% network utilization or 72 MB/s per server.  
  
In short, the final deployment of systems must have a 1 GB network adapter and be connected to a network infrastructure that will support said load. A further note is that given the amount of network traffic generated, the CPU load from network communications can have a significant impact and limit the maximum scalability of AD DS. This same process can be used to estimate the amount of inbound communication to the DC. But given the predominance of outbound traffic relative to inbound traffic, it is an academic exercise for most environments. Ensuring hardware support for RSS is important in environments with greater than 5,000 users per server. For scenarios with high network traffic, balancing of interrupt load can be a bottleneck. This can be detected by Processor(\*)\% Interrupt Time being unevenly distributed across CPUs. RSS enabled NICs can mitigate this limitation and increase scalability.

> [!NOTE]
> A similar approach can be used to estimate the additional capacity necessary when consolidating data centers, or retiring a domain controller in a satellite location. Simply collect the outbound and inbound traffic to clients and that will be the amount of traffic that will now be present on the WAN links.  
>  
> In some cases, you might experience more traffic than expected because traffic is slower, such as when certificate checking fails to meet aggressive time-outs on the WAN. For this reason, WAN sizing and utilization should be an iterative, ongoing process.

### Virtualization considerations for network bandwidth

It is easy to make recommendations for a physical server: 1 GB for servers supporting greater than 5000 users. Once multiple guests start sharing an underlying virtual switch infrastructure, additional attention is necessary to ensure that the host has adequate network bandwidth to support all the guests on the system, and thus requires the additional rigor. This is nothing more than an extension of ensuring the network infrastructure into the host machine. This is regardless whether the network is inclusive of the domain controller running as a virtual machine guest on a host with the network traffic going over a virtual switch, or whether connected directly to a physical switch. The virtual switch is just one more component where the uplink needs to support the amount of data being transmitted. Thus the physical host physical network adapter linked to the switch should be able to support the DC load plus all other guests sharing the virtual switch connected to the physical network adapter.

### Calculation summary example

|System|Peak bandwidth|
|-|-|
DC 1|6.5 MB/s|
DC 2|6.25 MB/s|
|DC 3|6.25 MB/s|
|DC 4|5.75 MB/s|
|DC 5|4.75 MB/s|
|Total|28.5 MB/s|

**Recommended: 72 MB/s** (28.5 MB/s divided by 40%)

|Target system(s) count|Total bandwidth (from above)|
|-|-|
|2|28.5 MB/s|
|Resulting normal behavior|28.5 &divide; 2 = 14.25 MB/s|

As always, over time the assumption can be made that client load will increase and this growth should be planned for as best as possible. The recommended amount to plan for would allow for an estimated growth in network traffic of 50%.

## Storage

Planning storage constitutes two components:

- Capacity, or storage size
- Performance

A great amount of time and documentation is spent on planning capacity, leaving performance often completely overlooked. With current hardware costs, most environments are not large enough that either of these is actually a concern, and the recommendation to “put in as much RAM as the database size” usually covers the rest, though it may be overkill for satellite locations in larger environments.

### Sizing

#### Evaluating for storage

Compared to 13 years ago when Active Directory was introduced, a time when 4 GB and 9 GB drives were the most common drive sizes, sizing for Active Directory is not even a consideration for all but the largest environments. With the smallest available hard drive sizes in the 180 GB range, the entire operating system, SYSVOL, and NTDS.dit can easily fit on one drive. As such, it is recommended to deprecate heavy investment in this area.

The only recommendation for consideration is to ensure that 110% of the NTDS.dit size is available in order to enable defrag. Additionally, accommodations for growth over the life of the hardware should be made.

The first and most important consideration is evaluating how large the NTDS.dit and SYSVOL will be. These measurements will lead into sizing both fixed disk and RAM allocation. Due to the (relatively) low cost of these components, the math does not need to be rigorous and precise. Content about how to evaluate this for both existing and new environments can be found in the [Data Storage](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/cc961771(v=technet.10)) series of articles. Specifically, refer to the following articles:

- **For existing environments &ndash;** The section titled “To activate logging of disk space that is freed by defragmentation” in the article [Storage Limits](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961769(v=technet.10)).
- **For new environments &ndash;** The article titled [Growth Estimates for Active Directory Users and Organizational Units](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc961779(v=technet.10)).

  > [!NOTE]
  > The articles are based on data size estimates made at the time of the release of Active Directory in Windows 2000. Use object sizes that reflect the actual size of objects in your environment.

When reviewing existing environments with multiple domains, there may be variations in database sizes. Where this is true, use the smallest global catalog (GC) and non-GC sizes.  

The database size can vary between operating system versions. DCs that run earlier operating systems such as Windows Server 2003 has a smaller database size than a DC that runs a later operating system such as Windows Server 2008 R2, especially when features such Active Directory Recycle Bin or Credential Roaming are enabled.

> [!NOTE]  
  >
>- For new environments, notice that the estimates in Growth Estimates for Active Directory Users and Organizational Units indicate that 100,000 users (in the same domain) consume about 450 MB of space. Please note that the attributes populated can have a huge impact on the total amount. Attributes will be populated on many objects by both third-party and Microsoft products, including Microsoft Exchange Server and Lync. An evaluation based on the portfolio of the products in the environment is preferred, but the exercise of detailing out the math and testing for precise estimates for all but the largest environments may not actually be worth significant time and effort.
>- Ensure that 110% of the NTDS.dit size is available as free space in order to enable offline defrag, and plan for growth over a three to five year hardware lifespan. Given how cheap storage is, estimating storage at 300% the size of the DIT as storage allocation is safe to accommodate growth and the potential need for offline defrag.

#### Virtualization considerations for storage

In a scenario where multiple Virtual Hard Disk (VHD) files are being allocated on a single volume use a fixed sized disk of at least 210% (100% of the DIT + 110% free space) the size of the DIT to ensure that there is adequate space reserved.  

#### Calculation summary example

|Data collected from evaluation phase| |
|-|-|
|NTDS.dit size|35 GB|
|Modifier to allow for offline defrag|2.1|
|Total storage needed|73.5 GB|

> [!NOTE]
> This storage needed is in addition to the storage needed for SYSVOL, operating system, page file, temporary files, local cached data (such as installer files), and applications.

### Storage performance

#### Evaluating performance of storage

As the slowest component within any computer, storage can have the biggest adverse impact on client experience. For those environments large enough for which the RAM sizing recommendations are not feasible, the consequences of overlooking planning storage for performance can be devastating.  Also, the complexities and varieties of storage technology further increase the risk of failure as the relevance of long standing best practices of “put operating system, logs, and database” on separate physical disks is limited in it’s useful scenarios.  This is because the long standing best practice is based on the assumption that is that a “disk” is a dedicated spindle and this allowed I/O to be isolated.  This assumptions that make this true are no longer relevant with the introduction of:

- New storage types and virtualized and shared storage scenarios
- Shared spindles on a Storage Area Network (SAN)
- VHD file on a SAN or network-attached storage
- Solid State Drives
- Tiered storage architectures (i.e. SSD storage tier caching larger spindle based storage)

In short, the end goal of all storage performance efforts, regardless of underlying storage architecture and design, is to ensure the needed amount of Input/Output Operations per Second (IOPS) are available and that those IOPS happen within an acceptable time frame. This section covers how to evaluate what AD DS demands of the underlying storage in order to ensure storage solutions are properly designed.  Given the variability of today’s storage technologies, it is best to work with the storage vendors to ensure adequate IOPS.  For those scenarios with locally attached storage, reference the Appendix C for the basics in how to design traditional local storage scenarios.  This principals are generally applicable to more complex storage tiers and will also help in dialog with the vendors supporting backend storage solutions.

- Given the wide breadth of storage options available, it is recommended to engage the expertise of hardware support teams or vendors to ensure that the specific solution meets the needs of AD DS. The following numbers are the information that would be provided to the storage specialists.

For environments where the database is too large to be held in RAM, use the performance counters to determine how much I/O needs to be supported:

- LogicalDisk(\*)\Avg Disk sec/Read (for example, if NTDS.dit is stored on the D:/ drive, the full path would be LogicalDisk(D:)\Avg Disk sec/Read)
- LogicalDisk(\*)\Avg Disk sec/Write
- LogicalDisk(\*)\Avg Disk sec/Transfer
- LogicalDisk(\*)\Reads/sec
- LogicalDisk(\*)\Writes/sec
- LogicalDisk(\*)\Transfers/sec

These should be sampled in 15/30/60 minute intervals to benchmark the demands of the current environment.

#### Evaluating the results

> [!NOTE]
> The focus is on reads from the database as this is usually the most demanding component, the same logic can be applied to writes to the log file by substituting LogicalDisk(*\<NTDS Log\>*)\Avg Disk sec/Write and LogicalDisk(*\<NTDS Log\>*)\Writes/sec):
>  
> - LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read indicates whether or not the current storage is adequately sized.  If the results are roughly equal to the Disk Access Time for the disk type, LogicalDisk(*\<NTDS\>*)\Reads/sec is a valid measure.  Check the manufacturer specifications for the storage on the back end, but good ranges for LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read would roughly be:
>   - 7200 – 9 to 12.5 milliseconds (ms)
>   - 10,000 – 6 to 10 ms
>   - 15,000 – 4 to 6 ms  
>   - SSD – 1 to 3 ms  
>   - >[!NOTE]
>     >Recommendations exist stating that storage performance is degraded at 15ms to 20ms (depending on source).  The difference between the above values and the other guidance is that the above values are the normal operating range.  The other recommendations are troubleshooting guidance to identify when client experience significantly degrades and becomes noticeable.  Reference Appendix C for a deeper explanation.
> - LogicalDisk(*\<NTDS\>*)\Reads/sec is the amount of I/O that is being performed.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is within the optimal range for the backend storage, LogicalDisk(*\<NTDS\>*)\Reads/sec can be used directly to size the storage.
>   - If LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read is not within the optimal range for the backend storage, additional I/O is needed according to the following formula:
>     > (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read) &divide; (Physical Media Disk Access Time) &times; (LogicalDisk(*\<NTDS\>*)\Avg Disk sec/Read)

Considerations:

- Note that if the server is configured with a sub-optimal amount of RAM, these values will be inaccurate for planning purposes.  They will be erroneously on the high side and can still be used as a worst case scenario.
- Adding/Optimizing RAM specifically will drive a decrease in the amount of read I/O (LogicalDisk(*\<NTDS\>*)\Reads/Sec.  This means the storage solution may not have to be as robust as initially calculated.  Unfortunately, anything more specific than this general statement is environmentally dependent on client load and general guidance cannot be provided.  The best option is to adjust storage sizing after optimizing RAM.

#### Virtualization considerations for performance

Similar to all of the preceding virtualization discussions, the key here is to ensure that the underlying shared infrastructure can support the DC load plus the other resources using the underlying shared media and all pathways to it. This is true whether a physical domain controller is sharing the same underlying media on a SAN, NAS, or iSCSI infrastructure as other servers or applications, whether it is a guest using pass through access to a SAN, NAS, or iSCSI infrastructure that shares the underlying media, or if the guest is using a VHD file that resides on shared media locally or a SAN, NAS, or iSCSI infrastructure. The planning exercise is all about making sure that the underlying media can support the total load of all consumers.

Also, from a guest perspective, as there are additional code paths that must be traversed, there is a performance impact to having to go through a host to access any storage. Not surprisingly, storage performance testing indicates that the virtualizing has an impact on throughput that is subjective to the processor utilization of the host system (see Appendix A: CPU Sizing Criteria), which is obviously influenced by the resources of the host demanded by the guest. This contributes to the virtualization considerations regarding processing needs in a virtualized scenario (see [Virtualization considerations for processing](#virtualization-considerations-for-processing)).

Making this more complex is that there are a variety of different storage options that are available that all have different performance impacts. As a safe estimate when migrating from physical to virtual, use a multiplier of 1.10 to adjust for different storage options for virtualized guests on Hyper-V, such as pass-through storage, SCSI Adapter, or IDE. The adjustments that need to be made when transferring between the different storage scenarios are irrelevant as to whether the storage is local, SAN, NAS, or iSCSI.

#### Calculation summary example

Determining the amount of I/O needed for a healthy system under normal operating conditions:

- LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec during the peak period 15 minute period 
- To determine the amount of I/O needed for storage where the capacity of the underlying storage is exceeded:
  >*Needed IOPS* = (LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read &divide; *\<Target Avg Disk sec/Read\>*) &times; LogicalDisk(*\<NTDS Database Drive\>*)\Read/sec

|Counter|Value|
|-|-|
|Actual LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.02 seconds (20 milliseconds)|
|Target LogicalDisk(*\<NTDS Database Drive\>*)\Avg Disk sec/Transfer|.01 seconds|
|Multiplier for change in available IO|0.02 &divide; 0.01 = 2|  
  
|Value name|Value|
|-|-|
|LogicalDisk(*\<NTDS Database Drive\>*)\Transfers/sec|400|
|Multiplier for change in available IO|2|
|Total IOPS needed during peak period|800|

To determine the rate at which the cache is desired to be warmed:

- Determine the maximum acceptable time to warm the cache. It is either the amount of time that it should take to load the entire database from disk, or for scenarios where the entire database cannot be loaded in RAM, this would be the maximum time to fill RAM.
- Determine the size of the database, excluding white space.  For more information, see [Evaluating for storage](#evaluating-for-storage).  
- Divide the database size by 8 KB; that will be the total IOs necessary to load the database.
- Divide the total IOs by the number of seconds in the defined time frame.

Note that the rate calculated, while accurate, will not be exact because previously loaded pages are evicted if ESE is not configured to have a fixed cache size, and AD DS by default uses variable cache size.

|Data points to collect|Values
|-|-|
|Maximum acceptable time to warm|10 minutes (600 seconds)
|Database size|2 GB|  
  
|Calculation step|Formula|Result|
|-|-|-|
|Calculate size of database in pages|(2 GB &times; 1024 &times; 1024) = *Size of database in KB*|2,097,152 KB|
|Calculate number of pages in database|2,097,152 KB &divide; 8 KB = *Number of pages*|262,144 pages|
|Calculate IOPS necessary to fully warm the cache|262,144 pages &divide; 600 seconds = *IOPS needed*|437 IOPS|

## Processing

### Evaluating Active Directory processor usage

For most environments, after storage, RAM, and networking are properly tuned as described in the Planning section, managing the amount of processing capacity will be the component that deserves the most attention. There are two challenges in evaluating CPU capacity needed:

- Whether or not the applications in the environment are being well-behaved in a shared services infrastructure, and is discussed in the section titled “Tracking Expensive and Inefficient Searches” in the article Creating More Efficient Microsoft Active Directory-Enabled Applications or migrating away from down-level SAM calls to LDAP calls.  
  
  In larger environments, the reason this is important is that poorly coded applications can drive volatility in CPU load, “steal” an inordinate amount of CPU time from other applications, artificially drive up capacity needs, and unevenly distribute load against the DCs.  
- As AD DS is a distributed environment with a large variety of potential clients, estimating the expense of a “single client” is environmentally subjective due to usage patterns and the type or quantity of applications leveraging AD DS. In short, much like the networking section, for broad applicability, this is better approached from the perspective of evaluating the total capacity needed in the environment.

For existing environments, as storage sizing was discussed previously, the assumption is made that storage is now properly sized and thus the data regarding processor load is valid. To reiterate, it is critical to ensure that the bottleneck in the system is not the performance of the storage. When a bottleneck exists and the processor is waiting, there are idle states that will go away once the bottleneck is removed.  As processor wait states are removed, by definition, CPU utilization increases as it no longer has to wait on the data. Thus, collect performance counters “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” and “Process(lsass)\\% Processor Time”. The data in “Process(lsass)\\% Processor Time” will be artificially low if “Logical Disk(*\<NTDS Database Drive\>*)\Avg Disk sec/Read” exceeds 10 to 15 ms, which is a general threshold that Microsoft support uses for troubleshooting storage-related performance issues. As before, it is recommended that sample intervals be either 15, 30, or 60 minutes. Anything less will generally be too volatile for good measurements; anything greater will smooth out daily peeks excessively.

### Introduction

In order to plan capacity planning for domain controllers, processing power requires the most attention and understanding. When sizing systems to ensure maximum performance, there is always a component that is the bottleneck and in a properly sized Domain Controller this will be the processor.

Similar to the networking section where the demand of the environment is reviewed on a site-by-site basis, the same must be done for the compute capacity demanded. Unlike the networking section, where the available networking technologies far exceed the normal demand, pay more attention to sizing CPU capacity.  As any environment of even moderate size; anything over a few thousand concurrent users can put significant load on the CPU.

Unfortunately, due to the huge variability of client applications that leverage AD, a general estimate of users per CPU is woefully inapplicable to all environments. Specifically, the compute demands are subject to user behavior and application profile. Therefore, each environment needs to be individually sized.

#### Target site behavior profile

As mentioned previously, when planning capacity for an entire site, the goal is to target a design with an *N* + 1 capacity design, such that failure of one system during the peak period will allow for continuation of service at a reasonable level of quality. That means that in an “*N*” scenario, load across all the boxes should be less than 100% (better yet, less than 80%) during the peak periods.

Additionally, if the applications and clients in the site are using best practices for locating domain controllers (that is, using the [DsGetDcName function](http://msdn.microsoft.com/en-us/library/windows/desktop/ms675983(v=vs.85).aspx)), the clients should be relatively evenly distributed with minor transient spikes due to any number of factors.

In the next example, the following assumptions are made:

- Each of the five DCs in the site has four of CPUs.
- Total target CPU usage during business hours is 40% under normal operating conditions (“*N* + 1”) and 60% otherwise (“*N*”). During non-business hours, the target CPU usage is 80% because backup software and other maintenance are expected to consume all available resources.

![CPU usage chart](media/capacity-planning-considerations-cpu-chart.png)

Analyzing the data in the chart (Processor Information(_Total)\% Processor Utility) for each of the DCs:

- For the most part, the load is relatively evenly distributed which is what would be expected when clients use DC locator and have well written searches. 
- There are a number of five-minute spikes of 10%, with some as large as 20%. Generally, unless they cause the capacity plan target to be exceeded, investigating these is not worthwhile.  
- The peak period for all systems is between about 8:00 AM and 9:15 AM. With the smooth transition from about 5:00 AM through about 5:00 PM, this is generally indicative of the business cycle. The more randomized spikes of CPU usage on a box-by-box scenario between 5:00 PM and 4:00 AM would be outside of the capacity planning concerns.

  >[!NOTE]
  >On a well-managed system, said spikes are might be backup software running, full system antivirus scans, hardware or software inventory, software or patch deployment, and so on. Because they fall outside the peak user business cycle, the targets are not exceeded.

- As each system is about 40% and all systems have the same numbers of CPUs, should one fail or be taken offline, the remaining systems would run at an estimated 53% (System D's 40% load is evenly split and added to System A's and System C’s existing 40% load). For a number of reasons, this linear assumption is NOT perfectly accurate, but provides enough accuracy to gauge.  

  **Alternate scenario –** Two domain controllers running at 40%: One domain controller fails, estimated CPU on the remaining one would be an estimated 80%. This far exceeds the thresholds outlined above for capacity plan and also starts to severely limit the amount of head room for the 10% to 20% seen in the load profile above, which means that the spikes would drive the DC to 90% to 100% during the “*N*” scenario and definitely degrade responsiveness.

### Calculating CPU demands

The “Process\\% Processor Time” performance object counter sums the total amount of time that all of the threads of an application spend on the CPU and divides by the total amount of system time that has passed. The effect of this is that a multi-threaded application on a multi-CPU system can exceed 100% CPU time, and would be interpreted VERY differently than “Processor Information\\% Processor Utility”. In practice the “Process(lsass)\\% Processor Time” can be viewed as the count of CPUs running at 100% that are necessary to support the process’s demands. A value of 200% means that 2 CPUs, each at 100%, are needed to support the full AD DS load. Although a CPU running at 100% capacity is the most cost efficient from the perspective of money spent on CPUs and power and energy consumption, for a number of reasons detailed in Appendix A, better responsiveness on a multi-threaded system occurs when the system is not running at 100%.

To accommodate transient spikes in client load, it is recommended to target a peak period CPU of between 40% and 60% of system capacity. Working with the example above, that would mean that between 3.33 (60% target) and 5 (40% target) CPUs would be needed for the AD DS (lsass process) load. Additional capacity should be added in according to the demands of the base operating system and other agents required (such as antivirus, backup, monitoring, and so on). Although the impact of agents needs to be evaluated on a per environment basis, an estimate of between 5% and 10% of a single CPU can be made. In the current example, this would suggest that between 3.43 (60% target) and 5.1 (40% target) CPUs are necessary during peak periods.

The easiest way to do this is to use the “Stacked Area” view in Windows Reliability and Performance Monitor (perfmon), making sure all of the counters are scaled the same.

Assumptions:

- Goal is to reduce footprint to as few servers as possible. Ideally, one server would carry the load and an additional server added for redundancy (*N* + 1 scenario).

![Processor time chart for lsass process (over all processors)](media/capacity-planning-considerations-proc-time-chart.png)

Knowledge gained from the data in the chart (Process(lsass)\\% Processor Time):

- The business day starts ramping up around 7:00 and decreases at 5:00 PM.
- The peak busiest period is from 9:30 AM to 11:00 AM. 
  > [!NOTE]
  > All performance data is historical. The peak data point at 9:15 indicates the load from 9:00 to 9:15.
- There are spikes before 7:00 AM which could indicate either load from different time zones or background infrastructure activity, such as backups. Because the peak at 9:30 AM exceeds this activity, it is not relevant.
- There are three domain controllers in the site.

At maximum load, lsass consumes about 485% of one CPU, or 4.85 CPUs running at 100%. As per the math earlier, this means the site needs about 12.25 CPUs for AD DS. Add in the above suggestions of 5% to 10% for background processes and that means replacing the server today would need approximately 12.30 to 12.35 CPUs to support the same load. An environmental estimate for growth now needs to be factored in.

### When to tune LDAP weights

There are several scenarios where tuning [LdapSrvWeight](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc957291(v=technet.10)) should be considered. Within the context of capacity planning, this would be done when the application or user loads are not evenly balanced, or the underlying systems are not evenly balanced in terms of capability. Reasons to do so beyond capacity planning are outside of the scope of this article.

There are two common reasons to tune LDAP Weights:

- The PDC emulator is an example that affects every environment for which user or application load behavior is not evenly distributed. As certain tools and actions target the PDC emulator, such as the Group Policy management tools, second attempts in the case of authentication failures, trust establishment, and so on, CPU resources on the PDC emulator may be more heavily demanded than elsewhere in the site.
  - It is only useful to tune this if there is a noticeable difference in CPU utilization in order  to reduce the load on the PDC emulator and increase the load on other domain controllers will allow a more even distribution of load.
  - In this case, set LDAPSrvWeight between 50 and 75 for the PDC emulator.
- Servers with differing counts of CPUs (and speeds) in a site.  For example, say there are two eight-core servers and one four-core server.  The last server has half the processors of the other two servers.  This means that a well distributed client load will increase the average CPU load on the four-core box to roughly twice that of the eight-core boxes.
  - For example, the two eight-core boxes would be running at 40% and the four-core box would be running at 80%.
  - Also, consider the impact of loss of one eight-core box in this scenario, specifically the fact that the four-core box would now be overloaded.

#### Example 1 - PDC

| |Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|53%|57|40%|
|DC 2|33%|100|40%|
|DC 3|33%|100|40%|

The catch here is that if the PDC emulator role is transferred or seized, particularly to another domain controller in the site, there will be a dramatic increase on the new PDC emulator.

Using the example from the section [Target site behavior profile](#target-site-behavior-profile), an assumption was made that all three domain controllers in the site had four CPUs. What should happen, under normal conditions, if one of the domain controllers had eight CPUs? There would be two domain controllers at 40% utilization and one at 20% utilization. While this is not bad, there is an opportunity to balance the load a little bit better. Leverage LDAP weights to accomplish this.  An example scenario would be:

#### Example 2 - Differing CPU counts

| |Processor Information\\ %&nbsp;Processor Utility(_Total)<br />Utilization with defaults|New LdapSrvWeight|Estimated new utilization|
|-|-|-|-|
|4-CPU DC 1|40|100|30%|
|4-CPU DC 2|40|100|30%|
|8-CPU DC 3|20|200|30%|

Be very careful with these scenarios though. As can be seen above, the math looks really nice and pretty on paper. But throughout this article, planning for an “*N* + 1” scenario is of paramount importance. The impact of one DC going offline must be calculated for every scenario. In the immediately preceding scenario where the load distribution is even, in order to ensure a 60% load during an “*N*” scenario, with the load balanced evenly across all servers, the distribution will be fine as the ratios stay consistent. Looking at the PDC emulator tuning scenario, and in general any scenario where user or application load is unbalanced, the effect is very different:

| |Tuned Utilization|New LdapSrvWeight|Estimated New Utilization|
|-|-|-|-|
|DC 1 (PDC emulator)|40%|85|47%|
|DC 2|40%|100|53%|
|DC 3|40%|100|53%|

### Virtualization considerations for processing

There are two layers of capacity planning that need to be done in a virtualized environment. At the host level, similar to the identification of the business cycle outlined for the domain controller processing previously, thresholds during the peak period need to be identified. Because the underlying principals are the same for a host machine scheduling guest threads on the CPU as for getting AD DS threads on the CPU on a physical machine, the same goal of 40% to 60% on the underlying host are recommended. At the next layer, the guest layer, since the principals of thread scheduling have not changed, the goal within the guest remains in the 40% to 60% range.

In a direct mapped scenario, one guest per host, all the capacity planning done to this point needs to be added to the requirements (RAM, disk, network) of the underlying host operating system. In a shared host scenario, testing indicates that there is 10% impact on the efficiency of the underlying processors. That means if a site needs 10 CPUs at a target of 40%, the recommended amount of virtual CPUs to allocate across all the “*N*” guests would be 11. In a site with a mixed distribution of physical servers and virtual servers, the modifier only applies to the VMs. For example, if a site has an “*N* + 1” scenario, one physical or direct-mapped server with 10 CPUs would be about equivalent to one guest with 11 CPUs on a host, with 11 CPUs reserved for the domain controller.

Throughout the analysis and calculation of the CPU quantities necessary to support AD DS load, the numbers of CPUs that map to what can be purchased in terms physical hardware do not necessarily map cleanly. Virtualization eliminates the need to round up. Virtualization decreases the effort necessary to add compute capacity to a site, given the ease with which a CPU can be added to a VM. It does not eliminate the need to accurately evaluate the compute power needed so that the underlying hardware is available when additional CPUs need to be added to the guests.  As always, remember to plan and monitor for growth in demand.

### Calculation summary example

|System|Peak CPU|
|-|-|-|
|DC 1|120%|
|DC 2|147%|
|Dc 3|218%|
|Total CPU being used|485%|  
  
|Target system(s) count|Total bandwidth (from above)|
|-|-|
|CPUs needed at 40% target|4.85 &divide; .4 = 12.25|

Repeating due to the importance of this point, *remember to plan for growth*. Assuming 50% growth over the next three years, this environment will need 18.375 CPUs (12.25 &times; 1.5) at the three-year mark. An alternate plan would be to review after the first year and add in additional capacity as needed.

### Cross-trust client authentication load for NTLM

#### Evaluating cross-trust client authentication load

Many environments may have one or more domains connected by a trust. An authentication request for an identity in another domain that does not use Kerberos authentication needs to traverse a trust using the domain controller’s secure channel to another domain controller either in the destination domain or the next domain in the path to the destination domain. The number of concurrent calls using the secure channel that a domain controller can make to a domain controller in a trusted domain is controlled by a setting known as **MaxConcurrentAPI**. For domain controllers, ensuring that the secure channel can handle the amount of load is accomplished by one of two approaches: tuning **MaxConcurrentAPI** or, within a forest, creating shortcut trusts. To gauge the volume of traffic across an individual trust, refer to [How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](https://support.microsoft.com/kb/2688798).

During data collection, this, as with all the other scenarios, must be collected during the peak busy periods of the day for the data to be useful.

> [!NOTE]
> Intraforest and interforest scenarios may cause the authentication to traverse multiple trusts and each stage would need to be tuned.

#### Planning

There are a number of applications that use NTLM authentication by default, or use it in a certain configuration scenario. Application servers grow in capacity and service an increasing number of active clients. There is also a trend that clients keep sessions open for a limited time and rather reconnect on a regular basis (such as email pull sync). Another common example for high NTLM load is web proxy servers that require authentication for Internet access.

These applications can cause a significant load for NTLM authentication, which can put significant stress on the DCs, especially when users and resources are in different domains.

There are multiple approaches to managing cross-trust load, which in practice are used in conjunction rather than in an exclusive either/or scenario. The possible options are:

- Reduce cross-trust client authentication by locating the services that a user consumes in the same domain that the user is resident in.
- Increase the number of secure-channels available. This is relevant to intraforest and cross-forest traffic and are known as shortcut trusts.
- Tune the default settings for **MaxConcurrentAPI**.

For tuning **MaxConcurrentAPI** on an existing server, the equation is:

> *New_MaxConcurrentApi_setting* &ge; (*semaphore_acquires* + *semaphore_time-outs*) &times; *average_semaphore_hold_time* &divide; *time_collection_length*

For more information, see [KB article 2688798: How to do performance tuning for NTLM authentication by using the MaxConcurrentApi setting](http://support.microsoft.com/kb/2688798).

## Virtualization considerations

None, this is an operating system tuning setting.

### Calculation summary example

|Data type|Value|
|-|-|
|Semaphore Acquires (Minimum)|6,161|
|Semaphore Acquires (Maximum)|6,762|
|Semaphore Timeouts|0|
|Average Semaphore Hold Time|0.012|
|Collection Duration (seconds)|1:11 minutes (71 seconds)|
|Formula (from KB 2688798)|((6762 &ndash; 6161) + 0) &times; 0.012 /|
|Minimum value for **MaxConcurrentAPI**|((6762 &ndash; 6161) + 0) &times; 0.012 &divide; 71 = .101|

For this system for this time period, the default values are acceptable.

## Monitoring for compliance with capacity planning goals

Throughout this article, it has been discussed that planning and scaling go towards utilization targets. Here is a summary chart of the recommended thresholds that must be monitored to ensure the systems are operating within adequate capacity thresholds. Keep in mind that these are not performance thresholds, but capacity planning thresholds. A server operating in excess of these thresholds will work, but is time to start validating that all the applications are well behaved. If said applications are well behaved, it is time to start evaluating hardware upgrades or other configuration changes.

|Category|Performance counter|Interval/Sampling|Target|Warning|
|-|-|-|-|-|
|Processor|Processor Information(_Total)\\% Processor Utility|60 min|40%|60%|
|RAM (Windows Server 2008 R2 或更早版本)|Memory\Available MB|< 100 MB|N/A|< 100 MB|
|RAM (Windows Server 2012)|Memory\Long 字詞的平均待命的快取 Lifetime(s)|30 分鐘|必須進行測試|必須進行測試|
|Network|Network Interface(\*)\Bytes Sent/sec<br /><br />Network Interface(\*)\Bytes Received/sec|30 分鐘|40%|60%|
|儲存體|LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Read<br /><br />LogicalDisk( *\<NTDS Database Drive\>* )\Avg Disk sec/Write|60 分鐘|10 毫秒|15 毫秒|
|AD 服務|Netlogon(\*)\Average Semaphore Hold Time|60 分鐘|0|1 秒|

## <a name="appendix-a-cpu-sizing-criteria"></a>附錄 A：CPU 調整大小準則

### <a name="definitions"></a>定義

**處理器 （微處理器） –** 讀取和執行程式指令的元件  

**CPU-** 中央處理器  

**多核心處理器 –** 相同的整合式驗證線路上的多個 Cpu  

**多重 CPU –** 不在相同的整合式驗證線路上的多個 Cpu  

**邏輯處理器 –** 從作業系統的觀點來看的單一邏輯運算引擎  

這包括超執行緒，多核心處理器或在單一核心處理器上的一個核心。  

因為現今的伺服器系統有多個處理器、 多個的多核心處理器和超執行緒，這項資訊會一般化為涵蓋這兩種案例。 因此，因為其代表的作業系統和應用程式的可用運算引擎的觀點來看，將使用詞彙的邏輯處理器。

### <a name="thread-level-parallelism"></a>執行緒層級平行處理原則

每個執行緒都會獨立的工作，因為每個執行緒都有它自己的堆疊和指示。 因為 AD DS 是多執行緒和可用的執行緒數目都可以使用微調[如何檢視和設定 Active Directory 中的 LDAP 原則使用 Ntdsutil.exe](http://support.microsoft.com/kb/315071)，它也在多個邏輯處理器間調整。

### <a name="data-level-parallelism"></a>資料層級平行處理原則

這牽涉到跨多個執行緒 （如果是單獨的 AD DS 程序） 的其中一個處理序內和跨多個處理序中的多個執行緒 （一般） 共用資料。 使用過度簡化的案例的問題，這表示資料的任何變更會反映所有核心的快取 (L1，L2，L3) 的各種不同的層級中所有執行中的執行緒來執行前述的執行緒，以及更新共用的記憶體。 效能可能會降低在寫入作業，而所有不同的記憶體位置會一致，才能繼續處理指示。

### <a name="cpu-speed-vs-multiple-core-considerations"></a>CPU 速度與多核心的考量

一般的經驗法則是更快速的邏輯處理器縮短處理一系列的指示，在多個邏輯處理器表示可以同時執行更多的工作時所需的持續時間。 這些規則的經驗法則細分，如案例變得原本就是更複雜，但有共用記憶體，等候資料層級平行處理原則，以及管理多個執行緒的額外負荷擷取資料的考量事項。 這也是為什麼在多核心系統的延展性不是線性的。

請考慮在這些考量下列的基礎： 把高速公路，其中每個執行緒正在個別汽車的核心，且正在的時脈速度速度限制每個通道。

1. 如果高速公路上只有一部車，無論是否有兩個通道或 12 的通道。 只會執行速度很快，因為速度限制會允許該汽車。
1. 假設執行緒需要的資料不是立即可用。 路段圖的區段是關機時，會比較這兩者。 如果在高速公路上只有一部車，並不重要速度限制的是之前重新開啟通道時 （從記憶體擷取資料）。
1. Cars 的數目增加時，會增加管理的額外負荷。 比較的駕駛體驗，並注意所需的數量時 road 幾乎是空的 （例如，深夜） 與當天交通很繁重 （例如中間的午後，但不是尖峰時刻）。 此外，請考慮注意數量必要時在兩個通道高速公路，其中只有一個其他通道擔心驅動程式在做什麼，六個通道高速公路其中一個必須擔心哪些許多其他驅動程式與執行。
   > [!NOTE]
   > 在下一節中擴充需尖峰時刻案例比喻：回應時間/系統繁忙程度如何影響效能。

如此一來，更多或更快的處理器相關的特定資訊變得高度主觀應用程式的行為，這在 AD DS 的情況下是環保很特定，甚至會在伺服器環境內而有所不同。 這就是安全性的為什麼稍早在本文中的參考執行不大量投資在過度精確，並在計算中包含邊界。 預算為導向的採購決策時，建議，最佳化的 40%（或環境所需的數目） 的處理器使用量，就會發生第一次，再考慮購買更快的處理器。 增加的同步處理，跨多個處理器，可減少從直線的更多處理器的真正優勢 (2&times;處理器的數目提供小於 2&times;提供額外的計算能力)。

> [!NOTE]
> 受控於安達爾定律和 Gustafson 的法律所相關的概念。

### <a name="response-timehow-the-system-busyness-impacts-performance"></a>回應時間/系統繁忙程度執行效能的影響

佇列的理論是等候線 （佇列） 的數學的研究。 佇列的理論上，使用率法律是以方程式表示：

*U* k = *B* &divide; *T*

其中*U* k 是使用率百分比*B*忙線中的時間量，以及*T*系統發現的總時間。 轉譯至 Windows 的內容，這表示 100 奈秒 (ns) 數目除以多少的 100-ns 間隔所指定的時間間隔中可用的執行狀態的間隔執行緒。 這是完全的公式計算 %處理器公用程式 (參考[處理器物件](https://docs.microsoft.com/previous-versions/ms804036(v=msdn.10))並[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/previous-versions/windows/embedded/ms901169(v=msdn.10)))。

佇列的理論上也會提供公式：*N* = *U* k &divide; (1 &ndash; *U* k) 來估計的使用量為基礎的等候項目數 ( *N*是佇列的長度）。 此圖表在所有的使用率間隔提供下列的估計值，來取得處理器上佇列的時間在任何指定的 CPU 負載。

![佇列長度](media/capacity-planning-considerations-queue-length.png)

它觀察到之後 50%的 CPU 負載, 平均總是有其他項目在佇列中等候，與明顯快速之後會增加大約 70%的 CPU 使用率。

回到駕駛的比喻，本節中稍早使用：

- 「 中間下午 」 的忙碌時間會假設情況下，屬於某處 40%到 70%的範圍。 沒有足夠的流量，讓某人可以挑選任何通道不 majorly 限制，和另一個的方式，而高、 中的驅動程式的機會不需要投入時間量來 「 尋找 」 道路上其他車輛之間有安全的間隙。
- 其中一個會注意到流量接近尖峰時刻，road 系統接近 100%的容量。 變更通道可以成為非常具挑戰性，因為汽車是非常接近在一起，增加必須謹慎若要這樣做。

這就是為什麼長期來看平均負載，無論說尖峰暫時性 （例如效能不佳的自動程式化的查詢執行的幾分鐘的時間） 或異常的突發一般負載 （早上的保守估計為 40%的容量可讓異常暴增的前端聊天室第一天漫長週末之後）。

上述陳述式將如使用率法律一點以便簡化一般讀取器正在相同的 %處理器時間計算。 針對這些數學方式更嚴格：  
- 轉譯[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))
  - *B* = 的邏輯處理器的 「 閒置 」 執行緒花費在 100 奈秒間隔數。 中的變更 」*X*」 中使用者定義變數[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))計算
  - *T* = 100 奈秒間隔在指定的時間範圍內的總數。 中的變更 」*Y*」 中使用者定義變數[PERF_100NSEC_TIMER_INV](https://docs.microsoft.com/en-us/previous-versions/windows/embedded/ms901169(v=msdn.10))計算。
  - *U* k = 「 閒置執行緒 」 或 %邏輯處理器的使用率百分比閒置的時間。  
- 處理數學：
  - *U* k = 1-%處理器時間
  - %Processor Time = 1 – *U* k
  - %Processor Time = 1 – *B* / *T*
  - %Processor Time = 1 – *X1* – *X0* / *Y1* – *Y0*

### <a name="applying-the-concepts-to-capacity-planning"></a>將概念套用至容量規劃

上述的數學運算可能會使系統所需的邏輯處理器數目做出看起來十分複雜。 這就是為什麼調整系統大小的方法著重於判斷根據目前的最大目標使用率載入和計算那里取得所需的邏輯處理器的數目。 此外，邏輯處理器的速度將會嚴重影響效能、 快取效率、 記憶體一致性需求、 執行緒排程和同步處理，而負載平衡優於用戶端將所有對顯著影響在每個伺服器的伺服器而異的效能。 透過計算能力的成本相對較低廉的成本，嘗試分析，並判斷所需的 Cpu 完美數目變成更多學術比它提供商業價值。

40%不是很難和快速的需求，它是一個合理的起點。 Active Directory 的各種不同的取用者需要各種層級的回應性。 因為存取權的處理器增加的等候時間不會明顯影響用戶端效能時，則可能是其中 80%或 90%的使用率持續的平均值，可以執行環境的案例。 請務必重新逐一查看在系統中，包括 RAM 的存取會遠低於邏輯處理器的系統中有許多方面，存取磁碟，並透過網路傳輸的回應。 所有這些項目需要一起進行微調。 範例：

- 新增更多的處理器來執行磁碟繫結的 90%的系統可能不會大幅改善效能。 更深入的分析系統可能會識別有很多，甚至還未處理器上因為它們要等待 I/O 完成執行緒。
- 解決磁碟繫結問題可能表示之前花許多時間在等候狀態的執行緒將不再會處於等候狀態的 I/O，並會有多個競爭 CPU 時間，也就是說，在舊版的 90%使用率範例將會移到 100%（因為它不可以進入更高版本）。 這兩個元件需要一起進行微調。
  > [!NOTE]
  > 處理器 Information(*)\\%處理器公用程式可能會超過 100%，與具有 「 渦輪 」 模式的系統。  這是 CPU 超過評分最高的處理器速度期限很短的位置。  參考 CPU 製造商的文件和更高的深入解析的計數器的描述。  

討論整個系統使用率考量也會帶入交談的網域控制站作為虛擬化來賓。 [回應時間/系統繁忙程度執行效能的影響](#response-timehow-the-system-busyness-impacts-performance)適用於主機和客體的虛擬化案例中。 這也是為什麼在只有一個客體與主機中，網域控制站 （與通常任何系統） 都有幾乎相同的效能，它會在實體硬體。 將其他來賓新增至主機時，會增加基礎主機，藉此增加等候時間，以取得存取權的處理器，如先前所述的使用量。 簡單地說，邏輯處理器使用率必須在這兩個主機和客體層級受到管理。

擴充先前的對比、 離開高速公路與實體硬體，也就是客體 VM 將 analogized 含匯流排 （會直接移至目的地 rider express 匯流排想） 中。 假設有下列四個案例：

- 它是下班時間 rider 取得幾乎是空白，匯流排上，在匯流排取得也幾乎是空白的道路上。 因為沒有任何流量都努力應付著，rider 有好用的簡單好好體驗吧，而且那里取得只快速如同 rider 必須改為導向。 Rider 的行進時間仍會受限於速度限制。
- 它是下班時間讓匯流排幾乎是空白，但大部分的道路上通道已關閉，因此高速公路仍壅塞。 Rider 是擁塞的道路上幾乎空白匯流排上。 雖然 rider 沒有要坐著匯流排中的競爭很多，總車程時間仍取決於外部流量的其餘部分。
- 它會是尖峰時刻，因此高速公路和匯流排都壅塞。 不會爲旅程會更久，只取得開啟和關閉匯流排是惡夢一場，因為大家都以 shoulder shoulder 和高速公路不是更好。 新增更多的匯流排上 （客體的邏輯處理器） 並不表示他們可以符合道路上任何更容易，或是該趟車程支付將被截斷。
- 最後一個案例中，但它可能會有點，延伸比較這兩者其中匯流排已滿，但 road 不壅塞。 Rider 仍會無法取得開啟和關閉匯流排，而該趟車程支付有效率的方式是在路上匯流排之後。 這是唯一的案例，其中加入更多的匯流排上 （客體的邏輯處理器） 將可改善客體效能。

從該處就相對較容易進行推斷，有一些案例之間使用 0%-] 和 [路段圖的 100%使用狀態和具有不同程度的影響匯流排 0%和 100%使用狀態。

套用上述的 40%的主體做為主機，以及客體合理的目標 CPU 會是合理的起點，如上述的佇列數量的相同原因。

## <a name="appendix-b-considerations-regarding-different-processor-speeds-and-the-effect-of-processor-power-management-on-processor-speeds"></a>附錄 B：考量不同的處理器速度和處理器速度上的處理器電源管理的效果

處理器的選取範圍上各節假設，處理器正在以 100%的時脈速度的一直所收集的資料，而取代系統必須在相同的速度處理器。 儘管實際上正在其中的預設電源計劃 false，特別是使用 Windows Server 2008 R2 和更新版本，這兩個假設**平衡**，仍會方法代表，因為它是保守的作法。 可能的錯誤速率可能會增加，而它只會增加處理器速度增加安全性的邊界。

- 比方說，在此案例中，11.25 Cpu 權，如果處理器執行一半的速度將資料收集時，更精確的估計值可能是 5.125 &divide; 2。
- 它是處理的很難保證，加倍時脈速度會兩倍的所指定的期間發生量。 這是因為處理器花費在 RAM 的等候的時間量或其他系統元件無法保持不變。 最後的結果會是時間的更快的處理器可能會花更高的百分比表示處於閒置狀態，等候要擷取的資料。 同樣地，建議您使用固定使用最小公分母，正在保守，並避免嘗試假設處理器速度之間的線性比較計算可能，則為 false 的精確度層級。

或者，如果在取代硬體的處理器速度會低於目前的硬體，可放心將增加的處理器所需的按比例的量估計值。 比方說，它會計算，10 個處理器來承受負載在網站中，需要並在 3.3 Ghz 執行目前的處理器並取代處理器會在 2.6 Ghz 執行，這是 21%降低速度。 在此情況下，12 處理器會建議的數量。

話雖如此，變化性就不會變更的容量管理處理器使用率的目標。 為處理器時脈速度將會根據較高的負載下執行系統的負載需求，動態調整將會產生其中 CPU 花費在更多時間在更高的時鐘速度狀態，使最終的目標必須為 40%的使用率，在 100%的案例時鐘的速度在尖峰時段的狀態。 任何小於將產生的電源節約，因為 CPU 速度將遭受節流處置回復期間關閉尖峰情況。

> [!NOTE]
> 選項會關閉在處理器上的電源管理 (若要設定的電源計劃**高效能**) 時收集資料。 可讓 CPU 耗用量的更精確表示，在目標伺服器上。

若要調整為不同處理器的估計值，它會用來安全，但不包括其他系統的瓶頸上述，假設兩個處理器速度兩倍的可能執行的處理量。  目前處理器的內部架構是不同的量測計的使用不同的處理器，比從取得資料的效果更安全的方式是利用標準的效能評估 SPECint_rate2006 基準測試的處理器之間Corporation。

1. 找到的處理器，正在使用中，要使用的計劃 SPECint_rate2006 分數。
    1. 在標準的效能評估 Corporation 網站，選取**結果**，反白顯示**CPU2006**，然後選取**搜尋所有 SPECint_rate2006 結果**。
    1. 底下**簡單要求**，例如目標處理器，並輸入搜尋準則**處理器相符項目 E5-2630 (baselinetarget)** 和**處理器相符項目 E5-2650 （基準）** .
    1. 尋找要使用的伺服器和處理器的組態 （或項目關閉，如果沒有完全相符），並記下的值**結果**並 **# 核心**資料行。
1. 若要判斷修飾詞會使用下列方程式：
   >((*為目標平台的每一核心分數值*) &times; (*MHz 每一核心的基準平台*)) &divide; ((*基準每一核心分數值*) &times;(*MHz 每一核心的目標平台*))  

    使用上述範例中：
   >(35.83 &times; 2000) &divide; (33.75 &times; 2300) = 0.92
1. 估計的處理器數目乘以修飾詞。  在上述案例中，從 E5 2650 移至處理器 E5 2630 處理器相乘的導出的 11.25 Cpu &times; 0.92 = $10.35 所需的處理器。

## <a name="appendix-c-fundamentals-regarding-the-operating-system-interacting-with-storage"></a>附錄 C：有關作業系統與儲存體互動的基本概念

中所述的佇列的理論概念[回應時間/系統繁忙程度執行效能的影響](#response-timehow-the-system-busyness-impacts-performance)也適用於儲存體。 需要瞭解的情況下，作業系統控制代碼 I/O，才能套用這些概念的方式。 在 Microsoft Windows 作業系統中，要保存的 I/O 要求的佇列會建立每個實體磁碟。 不過，實體磁碟上的釐清需要進行。 陣列控制站和 San 提供的作業系統的磁針的彙總為單一的實體磁碟。 此外，陣列控制器和 San 可以彙總成一個的陣列設定的多個磁碟，然後分割此集中多個 「 資料分割 」，這會接著向作業系統為多個實體磁碟 （參考圖） 的陣列。

![區塊主軸](media/capacity-planning-considerations-block-spindles.png)  

在此圖中兩個磁針鏡像，並分成邏輯區域 （Data 1 和 Data 2） 的資料儲存體上。 這些邏輯區域是由作業系統視為個別的實體磁碟。

雖然這可能是高令人困惑，就是整個本附錄以下術語用來識別不同的實體：

- **磁針 –** 實際安裝在伺服器中的裝置。
- **Array-** 控制器所彙總的磁針的集合。
- **陣列資料分割-** 資料分割的彙總的陣列
- **LUN –** 陣列，使用參考時，San
- **磁碟 –** 觀察是單一的實體磁碟的作業系統。
- **資料分割-** 的作業系統察覺到實體磁碟的邏輯資料分割。

### <a name="operating-system-architecture-considerations"></a>作業系統架構考量

作業系統會建立觀察到; 每個磁碟的第一個 In/First 出 (FIFO) I/O 佇列磁針、 陣列或陣列的資料分割，則可能代表此磁碟。 作業系統的觀點而言，關於處理 I/O，多個作用中佇列愈好。 因為序列化 FIFO 佇列時，這表示必須處理在順序中的所有發行至儲存體子系統的 I/o 要求到達。 藉由關聯的主軸/陣列使用作業系統所觀察到的每個磁碟，作業系統現在維護每一組唯一的磁碟，藉此在磁碟排除很少的 I/O 資源的爭用和隔離至單一的 I/O 要求 I/O 佇列，磁碟。 例外狀況，Windows Server 2008 導入了 I/O 優先順序的概念，並設計為可使用 「 低 」 優先順序的應用程式超出此一般順序後坐。 不是明確地編碼中的應用程式，以便利用 「 低 」 優先權預設值為"Normal"。

### <a name="introducing-simple-storage-subsystems"></a>引進簡單的儲存子系統

從簡單的範例 （單一硬碟機電腦中） 將會提供元件的分析。 分成這主要儲存體子系統元件，系統所組成：

- **1 –** 10,000 RPM 超級快速 SCSI HD （超級快速的 SCSI 有 20 MB/秒的傳輸速率）
- **1 –** （纜線） 的 SCSI 匯流排
- **1 –** 超級快速的 SCSI 介面卡
- **1 –** 33 MHz 32 位元的 PCI 匯流排

一旦識別出的元件，就可以計算的資料量來傳輸系統，或可以處理多少 I/O，了解。 請注意，則相互關聯的 I/O 數量和來傳輸系統的資料數量，但不是相同。 此相互關聯，取決於磁碟 I/O 是否隨機或循序和區塊大小。 （所有資料都會都寫入至磁碟做為區塊中，但使用不同的不同應用程式區塊大小）。依元件的元件：

- **硬碟機 –** 平均 10,000 RPM 硬碟具有 7 位數毫秒 (ms) 搜尋時間 」 和 「 3 毫秒的存取時間。 搜尋時的平均讀寫頭，若要移到某個位置，在磁碟上所花費的時間量。 存取時的平均讀取或正確的位置標頭之後將資料寫入磁碟時，花費的時間量。 因此，讀取唯一 10,000 RPM HD 中的資料區塊的平均時間每個資料區塊構成搜尋和存取，總共大約 10 毫秒 （或.010 秒為單位）。

  當每個磁碟存取要求移動到新的位置，在磁碟上的標頭時，讀取/寫入行為稱為 「 隨機 」。 因此，當是隨機的所有 I/O，都則 10,000 RPM HD 可以處理大約 100 每秒 I/O (IOPS) (公式都是除以 10 毫秒的每個 I/O 或 1000年/10 每秒 1000 毫秒 = 100 IOPS)。

  或者，從相鄰的磁區 HD 上所有 I/O 時，這稱為循序 I/O。 循序 I/O 不含任何搜尋時間，因為第一個 I/O 完成時，讀寫頭開頭的下一個資料區塊上 HD 的儲存位置。 因此 10,000 RPM HD 就能夠處理大約 333 I/O 每秒 （1000 秒除以 3 毫秒，每次 I/O 毫秒）。

  >[!NOTE]
  >此範例不會反映磁碟快取，其中通常會保留一個磁柱的資料。 在此情況下，需要 10 毫秒的第一個 I/O 和磁碟讀取整個磁柱。 所有其他的循序 I/O 會從快取來滿足。 如此一來，在磁碟快取可能會改善循序 I/O 效能。
  
  到目前為止，硬碟機傳輸速率一直不相關。 硬碟機是否為 20 MB/s 強力寬或 Ultra3 160 MB/秒，實際數量的 IOPS 可由 10,000 RPM HD 是 ~ 100 個隨機或 ~ 300 循序 I/O。 區塊大小變更為基礎的應用程式寫入磁碟機，是不同的每次 I/O 提取的資料量。 比方說，如果區塊大小為 8 KB，100 個 I/O 作業將會讀取或寫入硬碟機 800 的 KB 總數。 不過，如果區塊大小為 32 KB，100 I/O 將讀取/寫入降到 3,200 KB (3.2 MB) 到硬碟機。 只要 SCSI 傳輸速率超出所傳輸的資料總量，取得 「 快 」 的傳輸速率磁碟機，即可執行任何動作。 請參閱下的表比較。

  | |7200 RPM 9ms 搜尋，4ms 存取|10,000 RPM 7 毫秒搜尋，3 毫秒存取|15,000 RPM 4ms 搜尋，2 毫秒存取
  |-|-|-|-|
  |隨機 I/O|80|100|150|
  |循序 I/O|250|300|500|  
  
  |10,000 RPM 的磁碟機|8 KB 的區塊大小 (Active Directory Jet)|
  |-|-|
  |隨機 I/O|800 KB/秒|
  |循序 I/O|2400 KB/秒|

- **SCSI 後擋板 （匯流排） –** 了解如何 「 SCSI 後擋板 （匯流排） 」，或在此案例的功能區纜線中，會影響輸送量的儲存子系統，取決於區塊大小的知識。 本質上的問題是，I/O 多少可以匯流排控制代碼，如果 I/O 是以 8 KB 區塊？ 在此案例中，在 SCSI 匯流排會是 20 MB/秒或 20480 KB/秒。 20480 KB/s 除以 8 KB 的區塊會產生大約有 2500 IOPS 的 SCSI 匯流排所支援的最大值。

  >[!NOTE]
  >下表中的數字代表範例。 最附加的存放裝置目前使用 PCI Express 中，可提供更高的輸送量。  
  
  |SCSI 匯流排，每個區塊大小所支援的 I/O|2 KB 的區塊大小|8 KB 的區塊大小 (AD Jet) (SQL Server 7.0/SQL Server 2000)
  |-|-|-|
  |20 MB/秒|10,000|2,500|
  |40 MB/秒|20,000|5,000|
  |128 MB/秒|65,536|16,384|
  |320 MB/秒|160,000|40,000|

  可以決定於所謂的使用，無論匯流排絕對不會是瓶頸，從這個圖表，在案例中，會出現磁針為最大值為 100 I/O，遠低於任何上述的臨界值。

  >[!NOTE]
  >這是假設的 SCSI 匯流排是有效率的 100%。
  
- **SCSI 配接器 –** 判斷這可以處理的 I/O 數量，製造商的規格，需要檢查。 將導向至適當的裝置的 I/O 要求需要某種類型的處理，因此可以處理的 I/O 數量是依存於 SCSI 介面卡 （或陣列控制器） 處理器。

  在此範例中，假設 1,000 I/O 可處理會進行。

- **PCI 匯流排 –** 這是經常被忽略的元件。 在此範例中，這並不會造成瓶頸。不過，當系統相應增加，它可以成為瓶頸。 如需參考，運作於 33 Mhz 32 位元 PCI 匯流排可以在理論上傳輸 133 MB/s 的資料。 以下是此方程式：  
  > 32 位元&divide;每個位元組的 8 位元&times;33 MHz = 133 MB/s。  

  請注意，理論上的限制;事實上只有約 50%的最大值是實際達到，雖然在某些高載情況下，可以取得 75%的驚人效率期限很短。

  66 Mhz 64 位元 PCI 匯流排可支援的理論最大值 (64 位元&divide;每個位元組的 8 位元&times;66 mhz 為單位) = 528 MB/秒。 此外，任何其他裝置 （例如網路介面卡、 第二個 SCSI 控制器，等等） 將會減少可用的頻寬因為頻寬已共用，且裝置將有限的資源爭用。

在分析後此儲存體子系統的元件，磁針是數量可以要求的 I/O，因此可以傳輸系統的資料量的限制因素。 具體來說，在 AD DS 案例中，這是 8 KB 的增量方式每秒 100 隨機 I/O 總計每個第二個 when 800 KB 存取 Jet 資料庫。 或者，專門配置給記錄檔的主軸的最大輸送量就會發生下列限制：每秒 8 KB 的增量，總共 2400 KB (2.4 MB) 每秒的 300 循序 I/O。

現在，讓分析簡單的設定下, 表示範變更或新增的儲存子系統的元件時將會發生瓶頸的位置。

|注意|分析瓶頸|Disk|Bus|介面卡|PCI 匯流排|
|-|-|-|-|-|-|
|這是加入第二個磁碟之後，網域控制站設定。 磁碟組態可表示在 800 KB/秒的瓶頸。|將加入 1 個磁碟 (總計 = 2)<br /><br />是隨機的 I/O<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD|200 的 I/o 總數<br />800 KB/秒總計。| | | |
|在新增之後 7 的磁碟，磁碟設定仍代表在 3200 KB/秒的瓶頸。|**新增 7 磁碟 (總計 = 8)**  <br /><br />是隨機的 I/O<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD|800 的 I/o 總數。<br />3200 KB/秒總計| | | |
|在之後變更循序 I/O，網路介面卡會成為瓶頸，因為它是限制為 1000 IOPS。|新增 7 磁碟 (總計 = 8)<br /><br />**I/O 是循序**<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD| | |2400 I/O 秒可以讀取/寫入至磁碟，限制為 1000 IOPS 的控制站| |
|之後取代支援 10,000 IOPS 的 SCSI 介面卡的網路介面卡，瓶頸會傳回磁碟組態。|新增 7 磁碟 (總計 = 8)<br /><br />是隨機的 I/O<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD<br /><br />**升級的 SCSI 介面卡 (現在支援 10,000 I/O)**|800 的 I/o 總數。<br />降到 3,200 KB/秒總計| | | |
|之後增加至 32 KB 的區塊大小，此匯流排會成為瓶頸，因為它只支援 20 MB/秒。|新增 7 磁碟 (總計 = 8)<br /><br />是隨機的 I/O<br /><br />**32 KB 的區塊大小**<br /><br />10,000 RPM HD| |800 的 I/o 總數。 25600 KB/s （25 MB/秒） 可以是讀取/寫入至磁碟。<br /><br />匯流排僅支援 20 MB/秒| | |
|升級匯流排，並新增更多磁碟之後, 該磁碟仍然會造成瓶頸。|**新增 13 磁碟 (總數 = 14)**<br /><br />新增具有 14 磁碟的第二個 SCSI 介面卡<br /><br />是隨機的 I/O<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD<br /><br />**升級到 320 MB/s SCSI 匯流排**|2800 I/Os<br /><br />11,200 KB/秒 （10.9 MB/秒）| | | |
|在之後變更循序 I/O，該磁碟仍然會造成瓶頸。|新增 13 磁碟 (總數 = 14)<br /><br />新增具有 14 磁碟的第二個 SCSI 介面卡<br /><br />**I/O 是循序**<br /><br />4 KB 的區塊大小<br /><br />10,000 RPM HD<br /><br />升級到 320 MB/s SCSI 匯流排|8,400 I/Os<br /><br />33,600 KB\s<br /><br />(32.8 MB\s)| | | |
|在新增之後更快的硬碟，該磁碟仍然會造成瓶頸。|新增 13 磁碟 (總數 = 14)<br /><br />新增具有 14 磁碟的第二個 SCSI 介面卡<br /><br />I/O 是循序<br /><br />4 KB 的區塊大小<br /><br />**15,000 RPM HD**<br /><br />升級到 320 MB/s SCSI 匯流排|14000 I/o<br /><br />56000 KB/秒<br /><br />（54.7 MB/秒）| | | |
|之後增加至 32 KB 的區塊大小，PCI 匯流排會成為瓶頸。|新增 13 磁碟 (總數 = 14)<br /><br />新增具有 14 磁碟的第二個 SCSI 介面卡<br /><br />I/O 是循序<br /><br />**32 KB 的區塊大小**<br /><br />15,000 RPM HD<br /><br />升級到 320 MB/s SCSI 匯流排| | | |14000 I/o<br /><br />448,000 KB/秒<br /><br />（437 MB/秒） 的讀取/寫入限制是以磁針。<br /><br />PCI 匯流排 133 MB/秒 （在最有效率 75%) 的理論最多可支援。|

### <a name="introducing-raid"></a>簡介 RAID

引進陣列控制器; 時不大幅變更儲存體子系統的本質它只會在計算中的 SCSI 介面卡。 功能會變更為讀取和寫入資料到磁碟上，使用不同的陣列層級 （例如 RAID 0、 RAID 1 或 RAID 5) 時的成本。

在 RAID 0，是等量分散的磁碟，在 RAID 中設定所有的資料。 這表示在讀取或寫入作業時，資料的一部分，是取自或在推送至每個磁碟，增加可以在相同的時間期間內傳輸系統的資料量。 因此，在一秒，（再次假設 10,000 RPM 的磁碟機），每個磁針上 100 個 I/O 作業可以執行。 可支援的 I/O 總數是 N 磁針 100 次每個磁針 (每秒產生 100 * N I/O) 每秒的 I/O。

![邏輯的 d： 磁碟機](media/capacity-planning-considerations-logical-d-drive.png)

在 RAID 1 中，資料會鏡像處理 （重複） 跨一組備援的磁針。 因此，執行讀取的 I/O 作業時，資料可以從這兩個磁針，集合中讀取。 這會使得從這兩個磁碟的 I/O 容量可用的讀取作業期間。 需要注意的是，在 RAID 1 中撰寫作業獲得任何效能優勢。 這是因為相同的資料需要寫入兩個為了備援的磁碟機。 雖然它不會超過此時間的資料寫入發生時同時在這兩個磁針，因為這兩個磁針佔用複製資料，則寫入 I/O 作業基本上可防止發生的兩個讀取的作業。 因此，每個寫入 I/O 成本兩個會讀取 I/O。 您可以從該資訊來判斷發生的 I/O 作業總數中建立公式：  

> *讀取 I/O* + 2 &times; *寫入 I/O* = *總耗用的可用磁碟 I/O*  

當寫入讀數的比例，並且已知的磁針數，下列方程式可以衍生自上述的方程式，以識別陣列必須支援的最大 I/O:  

> *最大 IOPS，每個磁針* &times; 2 個磁針&times;[( *%讀取* +  *%寫入*) &divide; ( *%讀取*+ 2 &times; *%寫入*)] = *IOPS 總數*

RAID 1 + 0、 讀取和寫入的費用相關的行為與 RAID 1 完全相同。 不過，在 I/O 現在顆每個鏡像集。 如果  

> *最大 IOPS，每個磁針* &times; 2 個磁針&times;[( *%讀取* +  *%寫入*) &divide; ( *%讀取*+ 2 &times; *%寫入*)] =*總 I/O*  

在 RAID 1 集中，當多重性 (*N*) 的 RAID 1 會等量集，可以處理的總 I/O 會變成 N &times; I/O，每個 RAID 1 集：  

> *N* &times; {*每個磁針的最大值 IOPS* &times; 2 個磁針&times;[( *%讀取* +  *%寫入*) &divide;( *%的讀取*+ 2 &times; *%寫入*)]} = *IOPS 總數*

RAID 5 中，有時稱為*N* + 1 的 RAID，資料在多顆*N*磁針和同位資訊會寫入至"+ 1"的主軸。 然而，RAID 5 時，花費更多執行比 RAID 1 或 1 + 0 寫入 I/O。 每次寫入 I/O 送出至的陣列，RAID 5 就會執行下列程序：

1. 讀取舊的資料
1. 讀取舊的同位檢查
1. 撰寫新的資料
1. 撰寫新的同位檢查

作業系統所送出至陣列控制器，每個寫入 I/O 要求要求四個 I/O 作業完成時，寫入要求送出會花上四次以完成單一讀取 I/O。 若要衍生的公式是要轉譯從作業系統的觀點來看的 I/O 要求，所經歷的磁針：  

> *讀取 I/O* + 4 &times; *寫入 I/O* = *總 I/O*  

同樣地在 RAID 1 組中，當已知讀取與寫入和磁針數的比率，下列方程式可以衍生自上述方程式來識別支援的陣列 （請注意，總計的磁針數不包含的最大 I/Oe"drive"遺失同位）：  

> *每個磁針的 IOPS* &times; (*磁針*– 1) &times; [( *%讀取* +  *%寫入*) &divide; ( *%的讀取*+ 4 &times; *%寫入*)] = *IOPS 總數*

### <a name="introducing-sans"></a>簡介 San

擴充的儲存子系統，複雜度，SAN 導入環境時，所述的基本準則不會變更，不過 I/O 行為的所有系統都連接到 SAN 列入考量。 在使用 SAN 的主要優點之一是從內部或外部連結的儲存體上額外的備援數量，容量規劃現在必須考慮到帳戶容錯需求。 此外，導入更多元件，必須進行評估。 上分解成元件部分 SAN:

- SCSI 或光纖通道的硬碟機
- 儲存體單位通道後擋板
- 儲存體單位
- 儲存體控制器模組
- SAN 這些參數
- Hba
- PCI 匯流排

當設計冗餘的任何系統時，其他元件的以因應可能會失敗。 當它是非常重要的是，容量規劃，以排除重複的元件，從可用的資源。 例如，如果 SAN 中有兩個控制器模組，一個控制器模組的 I/O 容量就應該使用提供給系統的總 I/O 輸送量。 這是因為如果一個控制器發生故障，所有連接的系統所要求的整個 I/O 負載必須由其餘的控制器來處理。 所有容量規劃，都為了尖峰使用量，備援元件不會納入可用的資源和計劃的尖峰使用量 （以配合高載或異常的系統不應該超過 80%的系統的飽和度行為）。 同樣地，多餘的 SAN 交換器、 儲存體單位和磁針上應該不納入 I/O 計算。

當分析的 SCSI 或光纖通道的硬碟機的行為，分析行為，如所述的方法之前不會變更。 雖然有特定的優點和缺點，每個通訊協定，在每個磁碟上的限制因素是機械硬碟機的限制。

分析通道上的儲存體單位是完全相同計算除以 （例如 8 KB) 的區塊大小的頻寬 （例如 20 MB/秒） 的 SCSI 匯流排上的可用資源。 這是衍生自上一個範例是在多個通道的彙總。 比方說，如果有 6 個通道，各自支援 20 MB/秒的最大傳輸速率，總數是 可用的 I/O 和資料傳輸是 100 MB/秒 （這是正確的它是不 120 MB/秒）。 同樣地，容錯是主要的播放程式，在此計算中，將整個通道耗損，系統是唯一的左邊，具有 5 個正常運作的通道。 因此，為了確保持續符合發生失敗時的效能期望，所有的儲存體通道的總輸送量不應該超過 100 MB/秒 （這假設負載及容錯功能會平均分配所有頻道）。 開啟這個功能到 I/O 設定檔是取決於應用程式的行為。 這會在 Active Directory Jet I/O 的情況下相互關聯約 12,500 每秒 I/O (100 MB/秒&divide;每次 I/O 的 8 KB)。

接下來，取得的控制器模組的製造商規格，才能了解如何在每個模組都能支援的輸送量。 在此範例中，SAN 會有兩個控制器模組的支援 7500 I/O 每個。 如果不想要使用備援系統的總輸送量可能 15,000 的 IOPS。 計算最大的輸送量，在失敗的情況下，限制會是一個控制站，或 7500 IOPS 輸送量。 此臨界值會完全低於 12,500 （假設 4 KB 的區塊大小） 的 IOPS 上限的所有儲存體通道，可以支援，因此，目前在分析中的瓶頸。 仍在進行規劃用途，是規劃所需的最大 I/O 會 10400 I/O。

當資料存在時將控制器模組時，它會傳輸 1 GB/秒 （或每秒 1 Gigabit） 評等的光纖通道連線。 若要將此相互關聯與其他度量，1 GB/s 會變成 128 MB/秒 (1 GB/秒&divide;8 位元/位元組)。 因為這超過總頻寬是跨所有通路中的儲存體單位 （100 MB/秒），這將不會造成瓶頸的系統。 此外，因為這是其中一個的兩個通道 （額外 1 GB/s 光纖通道連線正在進行備援），如果其中一個連線失敗，剩餘的連線仍會有足夠的容量來處理要求的所有資料傳輸。

路由到伺服器，也就是資料很可能會傳輸 SAN 開關。 SAN 交換器必須處理內送的 I/O 要求及轉寄出適當的連接埠，此參數會有可以處理的 I/O 數量限制，不過，製造商規格將會需要該限制會決定。 比方說，如果有兩個參數，而且每個參數都可以處理 10,000 IOPS，輸送量總計會是 20,000 IOPS。 同樣地，容錯所需要考量，如果交換器失敗，系統的總輸送量 10,000 IOPS。 想要它不能超過 80%使用量在正常作業中，使用超過 8000 I/O 應該是目標。

最後，在伺服器中安裝 HBA 也必須可以處理的 I/O 數量限制。 通常，第二個 HBA 已安裝的備援，但計算最大可以處理的總輸送量的 I/O 時，就像使用 SAN 參數中， *N* &ndash; 1 的 Hba，正是系統的最大的延展性。

### <a name="caching-considerations"></a>快取的考量

快取是可能會嚴重影響整體效能，在任何時間點儲存體系統中的元件之一。 有關快取演算法的詳細的分析已超出範圍的此篇文章：不過，有關快取磁碟子系統上的某些基本陳述式是值得理解：

- 快取會改善持續的循序寫入 I/O，因為它可以緩衝處理許多較小到較大的 I/O 區塊的寫入作業，並改存到較少但較大的區塊大小的儲存體。 這會降低總隨機 I/O 和總計的循序 I/O，因此能提供更多的資源可用性的其他 I/O。
- 快取無法改善持續的寫入的儲存子系統的 I/O 輸送量。 它只允許對寫入的磁針提供認可資料之前進行緩衝處理。 當磁針的儲存子系統的所有可用的 I/O 已飽和長時間時，快取會最後會填滿。 若要清空快取，足夠的時間量暴增或額外的磁針之間，必須配置以便提供足夠的 I/O，以允許快取排清。

  較大的快取只允許進行緩衝處理的詳細資料。 這表示可以容納較長的飽和度。

  在正常作業的儲存體子系統中，作業系統會發生資料才要寫入至快取改善的寫入效能。 一旦基礎的媒體而飽和 I/O，快取將會填滿，寫入效能將會回到磁碟速度。

- 當快取讀取 I/O 時，快取最有利的案例時，在磁碟上，依序儲存資料，快取可以預先讀取 （它會假設下一步 的磁區包含將下一個要求的資料）。
- 當讀取 I/O 是隨機的在磁碟機控制器快取不提供任何增強功能，可以從磁碟讀取的資料量。 任何的增強功能是不存在的作業系統或應用程式為基礎的快取大小是否大於硬體為基礎的快取的大小。

  在 Active Directory 中，快取只受限於 RAM 數量。

### <a name="ssd-considerations"></a>SSD 的考量

SSDs 會完全不同的動物比磁針為基礎的硬碟。 還會保留兩個關鍵條件：「 多少 IOPS 可以它處理？ 」 和 「 這些 IOPS 的延遲為何？ 」 相較於磁針為基礎的硬碟 Ssd 可以處理較大量的 I/O，並可以有較低的延遲。 一般而言，撰寫本文時，每 Gb 成本相較之下仍十分昂貴 Ssd 時，他們就成本每-I/o 而言非常便宜，值得儲存體效能方面的重要考量。

考量：

- IOPS 和延遲會製造廠商設計很主觀，並在某些情況下觀察到是導致能比磁針基礎的技術。 簡單地說，它是檢閱及驗證的磁碟機的磁碟機的製造商規格，並假設任何 generalities 更重要。
- IOPS 類型可以有非常不同的數字，根據它是讀取或寫入。 AD DS 服務，在一般情況下，要以讀取為主，將會比其他應用程式案例較不受影響。
- 這是概念的 SSD 儲存格將最後出 「 寫入耐力 – 」。這項挑戰不同的方式應付各種不同的製造商。 至少針對資料庫磁碟機中，以讀取的 I/O 設定檔允許 downplaying 這個問題的重要性，因為資料不具有高易變性。

### <a name="summary"></a>總結

考慮儲存體的方法之一 picturing 家庭的配管。 假設資料儲存在媒體的 IOPS 是家庭的主要清空。 當這已經堵塞 （例如管道中的根憑證），或有限制 （它是摺疊或太小），在備份時在太多 water 家庭成員的所有接收使用 （太多來賓）。 這是非常類似於共用環境中其中一個或多個系統會利用 SAN/NAS/iSCSI 使用相同的基礎媒體上的共用存放裝置。 不同的方法可以採取來解決不同的案例：

- 摺疊或過小清空需要完整的小數位數取代型和修正程式。 這看起來會像在新的硬體中新增或轉散發使用整個基礎結構的共用存放裝置的系統。
- 一個以上的違規問題的識別和移除這些問題，通常表示"阻塞"管道。 在儲存體的案例中，這可能是儲存體或系統層級備份，跨所有伺服器，以及在尖峰期間的同步處理的磁碟重組軟體執行同步處理的防毒掃描。

在任何配管設計中，多個清空後會饋送至主要清空。 如果任何項目停止了其中一個這些清空後 」 或 「 連接點，該連接點背後的事物點備份。 在儲存體案例中，這可能是多載的參數 （SAN/NAS/iSCSI 案例）、 驅動程式相容性問題 (錯誤驅動程式/HBA Firmware/storport.sys 組合) 或磁碟重組備份/防毒軟體。 若要判斷是否夠大的儲存體的 「 管道 」，必須待測 IOPS 和 I/O 大小。 在每個聯結在一起，以確保有足夠新增 「 管道直徑。 」

## <a name="appendix-d---discussion-on-storage-troubleshooting---environments-where-providing-at-least-as-much-ram-as-the-database-size-is-not-a-viable-option"></a>附錄 D-討論有關儲存體疑難排解-環境，其中提供至少會盡可能 RAM 因為資料庫大小不可行的選項

最好先了解這些建議存在，因此可以容納在儲存體技術變更的原因。 這些建議會存在兩個原因。 第一種是隔離的 IO，使得作業系統磁針上的效能問題 （也就，分頁） 不會影響效能的資料庫和 I/O 設定檔。 第二個是 AD DS （以及大部分資料庫） 的記錄檔會循序在本質上，而且磁針為基礎的硬碟機和快取有很大的效能優勢時搭配循序 I/O，相較於更隨機的 IO 模式的作業系統和幾乎AD ds 的純粹隨機 IO 模式的資料庫磁碟機。 藉由隔離到個別的實體磁碟機的循序 I/O，您可以增加輸送量。 現今的儲存體選項所呈現的挑戰是這些建議背後的基本假設不再成立。 在許多虛擬化儲存體的案例，例如 iSCSI SAN、 NAS 和虛擬磁碟映像檔、 基礎儲存體跨多部主機共用的媒體，因此完全否定 「 隔離的 IO 中 」 和 「 循序 I/O 最佳化 」 方面。 事實上這些案例會新增一層額外的複雜性，因為其他主機存取的共用的媒體可能會降低網域控制站的回應性。

在規劃中的儲存體效能，有要考慮的三個類別： 冷快取狀態、 已準備快取狀態，以及備份/還原。 冷快取狀態，就會發生在案例，例如當網域控制站開始重新啟動或重新啟動 Active Directory 服務，並在 RAM 中沒有 Active Directory 資料。 其中的網域控制站處於穩定狀態和快取資料庫暖的快取狀態。 這些是要特別注意它們將磁碟機非常不同的效能設定檔，以及有足夠的 RAM 來快取整個資料庫不會協助效能時，快取是冷項目。 我們可以把這兩種情況，使用下列的比喻，準備冷快取中的效能設計 「 sprint 」 且暖快取執行伺服器的 「 marathon"。

冷快取和暖的快取案例中，問題就變成速度的儲存體移動資料從磁碟載入記憶體。 正在準備快取是其中一段時間，可改善效能以及更多查詢重複使用的資料、 快取點擊率增加，需要移至磁碟的頻率會減少的案例。 如此一來會降低磁碟所要的效能產生負面影響。 等候要準備，以及擴充到最大值、 系統而定的允許大小的快取時，才暫時性任何效能降低。 交談可以簡化成如何快速取得資料，從磁碟，而是 Active directory、 可用的 IOPS 的簡單量值也就是主觀 IOPS 提供從基礎儲存體。 規劃的觀點而言，因為準備中快取和備份/還原的情況下會發生例外狀況為基礎，通常就會發生停機的情況下，而且主觀的 DC，載入的一般建議不存在除外，因為這些活動排程非尖峰時間排列。

AD DS，在大部分情況下，主要是唯讀的 IO，通常的 90 %read/10%寫入比率。 讀取 I/O 通常多半會是使用者體驗的瓶頸，並寫入 IO 原因撰寫效能降低。 因為是以隨機 I/O NTDS.dit，則快取通常會提供最少的好處，讀取 IO，讓它更為重要，若要設定的儲存體 I/O 設定檔正確讀取。

正常作業情況下，針對儲存體的規劃目標會是從 AD DS 的要求，會傳回從磁碟等候時間降至最低。 這基本上表示的未處理和暫止 I/O 數目小於或等於多元化的磁碟數目。 有各種不同的測量這種方式。 監視案例的效能，一般建議是該邏輯磁碟 ( *\<NTDS 資料庫磁碟機\>* ) \Avg Disk sec/Read 會小於 20 毫秒。 所需的操作臨界值必須是低很多，最好是為接近儲存體的速度越好，根據儲存體的類型 2 到 6 毫秒 （至.006.002 秒） 範圍中。

範例：

![存放裝置延遲圖表](media/capacity-planning-considerations-storage-latency.png)

分析圖表：

- **綠色 oval 左邊 –** 延遲保持一致，在 10 毫秒。 在負載增加時從 800 的 IOPS 來 2400 IOPS。 這是如何快速地處理 I/O 要求，由基礎儲存體絕對最低限度值。 這受限於儲存體解決方案的詳細資訊。
- **其中顯示酒 oval 右邊 –** 輸送量持平透過綠色圓形的結束資料集合的結尾時延遲會持續增加。 這示範，當要求量超過實際的基礎儲存體限制時，更長的要求費用坐在佇列中等待傳送至儲存子系統。

套用這項知識：

- **影響使用者查詢成員資格的大型群組 –** 假設這需要讀取 1 MB 的資料磁碟的 I/O 數量，並花多少時間可以評估，如下所示：
  - Active Directory 資料庫頁面的大小是 8 KB。 
  - 至少 128 個分頁就必須從磁碟讀取的。 
  - 假設不是在快取，這要花最少 1.28 秒從磁碟載入資料，以傳回至用戶端 floor （10 毫秒）。 在 20 毫秒，其中儲存體上的輸送量很超量而也是建議的最大值，將需要 2.5 秒，以取得從磁碟的資料，才能將它傳回給使用者。  
- **在何種速率會快取就緒 –** 並假設用戶端負載即將在本例中儲存體輸送量最大化，快取會準備的 2400 IOPS 速率&times;每次 IO 的 8 KB。 或者，約 20 MB/秒第二個，載入約 1 GB 的資料庫進入 RAM 每隔 53 秒。

> [!NOTE]
> 這是正常的短的時間，以觀察延遲爬元件積極地讀取或寫入磁碟，例如當系統正在進行備份，或當 AD DS 正在執行記憶體回收時。 其他前端的空間，在計算之上應該提供以容納這些週期的事件。 目標所提供足夠的輸送量來滿足這些案例，而不會影響正常的函式。

可以看出沒有實際限制，可能是多快可以準備快取的儲存體設計為基礎。 什麼會準備快取是連入用戶端要求，最多可以提供基礎儲存區的速率。 執行 「 預先準備 「 快取在尖峰時段的指令碼會提供一個競賽即可載入實際的用戶端要求所導向。 可造成負面的影響，因為根據設計，它會產生很少的磁碟資源競用的人嘗試準備快取將會載入不會連絡 DC 的用戶端與相關的資料，用戶端第一次需要提供資料。
