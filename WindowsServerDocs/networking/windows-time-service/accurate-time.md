---
ms.assetid: 72a90d00-56ee-48a9-9fae-64cbad29556c
title: Windows 2016 正確的時間
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 3/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: f4b61dbf07fbc21820dd7b9326bbc990e4db3602
ms.sourcegitcommit: fb4e2ace2e0a29e0f6b028f1cb945cab6aa6ee2c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/05/2018
---
# <a name="windows-server-2016-accurate-time"></a>Windows Server 2016 正確的時間

>適用於：Windows Server 2016

Windows Server 2016 同步處理時間精確度已經大幅，改善維持向前全 NTP 較舊的 Windows 版本的相容性。 您可以在合理操作條件維護 1 ms 正確性 UTC-10 或 Windows Server 2016 和 Windows 10 年度更新版網域成員更好。

Windows 時間服務是使用外掛程式模型 client 和伺服器的時間同步處理提供者的元件。  在 Windows 上，有兩個建 client 提供者和有第三方插可供使用。 一個提供者使用[NTP (RFC 1305)](https://tools.ietf.org/html/rfc1305)或[MS-NTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx)來同步處理本機系統時間 NTP 和/或 MS-NTP 相容參考伺服器。 其他提供者是 HYPER-V 和同步處理至 HYPER-V 主機虛擬電腦 (VM)。  多個提供者存在，當 Windows 將會挑選最佳的提供者先，使用組織層級後面根延遲，根起始，並最後時間時差。

>[!NOTE]
>Windows 時間服務的快速概述，看看這個[高階概觀視訊](https://aka.ms/WS2016TimeVideo)。

<!-- Not sure what to do with the following -->
本主題中，我們討論...下列主題與相關讓正確的時間： 

- 改進
- 測量
- 最佳做法

>[!IMPORTANT]
> Windows 2016 正確時間文章參考增補可以下載[在此](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)。  本文件會提供我們的測試及度量單位方法有關更多詳細資料。



> [!NOTE] 
> Windows 時間提供者增益集模型是[記載 TechNet 在](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx)。
<!-- -->





## <a name="domain-hierarchy"></a>網域層
網域和獨立設定以不同方式運作。

- 網域成員使用安全 NTP 通訊協定，以確保的安全性與的時間參考的真確性使用驗證。  由網域階層和評分系統時鐘主要網域成員同步。  在網域中，還有階層時間 stratums，讓每個俠指向家長俠更加準確的時間組織層的層級。  階層解析 PDC 或根、 樹系 DC DC GTIMESERV 網域旗標，代表告訴您一個好時間伺服器的網域。  查看[指定本機可靠時間服務使用 GTIMESERV](#GTIMESERV)一節。

- 獨立電腦設定預設為使用 time.windows.com。  此名稱解析 isp，應該要指向 Microsoft 擁有資源。  所有遠端位於的時間參考網路中斷，例如可能會導致同步處理。  網路流量載入和對稱網路路徑可能降低同步處理時間準確度。  1 ms 正確性，您無法依賴遠端時間來源。

HYPER-V 來賓將會有可以選擇在至少兩個 Windows 時間提供者，因為的主機時間和 NTP，您可能會看到不同的行為，與網域或獨立來賓以執行。

> [!NOTE] 
> 如網域階層和評分系統的相關詳細資訊，請查看["What's Windows 時間服務」？](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) 部落格文章。

> [!NOTE]
> 組織層的概念用於 NTP 和 HYPER-V 提供者，且其表示階層時鐘位置。  保留組織 1 層的 error 時鐘]，和階層 0 保留被視為精確的硬體，而且少或不延遲關聯。  組織層 2 詢問真人專員組織 1 層的伺服器，階層 3 到階層 2 等等。  而較低的組織層通常表示更加準確的時鐘，就可以找到不一致。  同時，W32time 只接受階層 15 從或下方的時間。  若要查看的 client 階層，使用*w32tm /query /status*。

## <a name="critical-factors-for-accurate-time"></a>重要因素正確的時間
在每個案例中為正確的時間，您有三個重要比例：

1. **實心來源時鐘**-來源時鐘在您的網域需求穩定且準確。 這通常表示安裝 GPS 裝置或指向到階層 1 來源，考慮 #3。 會，如果您有兩個的水，而且您在嘗試測量相較於到另一個的高度類比，您的準確度是最好的作法如果來源拉梅斯是非常穩定，不移動。 相同的時間會，如果您的來源時鐘無法穩定，然後整個鏈結同步時鐘的影響，放大每個階段。 它也必須存取因為中斷連接中的會干擾同步處理時間。 而且最後，必須安全。 如果維護參考不正確的時間，或可能是惡意派對由，您可能會公開您的網域型時間攻擊。
2. **穩定 client 時鐘**-穩定 client 時鐘確保 oscillator 的自然積雪是 containable。  NTP 條件和訓練您本機電腦的時鐘使用多個範例潛在多個 NTP 伺服器。  它不步驟時間會變更，但而不速度變慢或 NTP 要求之間加速的方法的正確時間快速本機時鐘和保持準確。  不過，如果 client 電腦時鐘 oscillator 不穩定，就會發生空行調整更多變動，然後 Windows 用來條件時鐘演算法無法正確運作。  有時候，可能需要韌體更新的正確時間。
3. **對稱式 NTP 通訊**-很重要的 NTP 通訊連接是對稱。  NTP 調整時間假設網路修補程式是對稱用於計算。  如果路徑 NTP 封包需要移至伺服器需要在不同的一段時間，以返回、 正確性會受到影響。  例如，路徑可能會變更因為變更拓撲網路或傳送到裝置有不同的介面速度封包。


電池供電的裝置的行動裝置版，可移植，您必須考慮不同策略。  根據我們的建議，會 secure 一秒，與時鐘更新的頻率相關聯的時鐘需要保留正確的時間。 這些設定將會使用更多電池電力的非預期和可能會干擾省電模式可在 Windows 中，這類裝置。 電池供電的裝置也已停止所有應用程式，會干擾訓練時鐘和維護正確的時間 W32time 的能力，某些電源模式。 此外，在行動裝置版裝置時鐘可能無法非常準確開始。  環境環境條件影響時鐘的正確性和行動裝置版的裝置可以從環境條件移動到下一步，可能會干擾持續時間精確的能力。  因此，Microsoft 不建議您在高正確性設定的設定可移植電池供電的裝置。 

## <a name="why-is-time-important"></a>為何很重要的時間？  


## <a name="windows-server-2016-improvements"></a>Windows Server 2016 的改進
### <a name="windows-time-service-and-ntp"></a>Windows 時間服務和 NTP
Windows Server 2016 已經改進用來修正時間和條件時鐘與 UTC-10 同步處理本機的演算法。  NTP 使用 4 值計算時間時差，根據時間戳記 client 要求日回應和伺服器要求回應。  不過的網路都是吵，而且有突然在 NTP 網路塞車，以及其他因素影響延遲網路，因為資料。  Windows 2016 演算法平均出使用幾種不同的技術，會導致穩定和準確時鐘此雜音。  此外，來源我們會使用正確的時間參考改進的 API，讓我們更高的解析度。  這些改進我們目前無法達到 1 ms 正確性有關 UTC 跨網域。

### <a name="hyper-v"></a>Hyper-v
Windows 2016 已改善 HYPER-V TimeSync 服務。 改進上 VM [開始] 畫面或 VM 還原中斷延遲修正 w32time 提供的範例包括更加準確的初始時間。  這項改良功能可讓我們的單元 10µs RMS、（根表示平方，這表示差異），以的主機保持在 50µs，甚至在載入 75%的電腦上。

> [!NOTE]
> 查看這篇文章[HYPER-V 架構](https://msdn.microsoft.com/library/cc768520.aspx)如需詳細資訊。

> [!NOTE]
> 載入建立使用 prime95 基準使用平衡設定檔。

此外，主機報告 guest 組織層級也更清楚。  先前主機會顯示為 2，無論正確性修正的階層。  Windows Server 2016 中的變更，以主機報告階層大於主機階層，導致的 virtual 來賓更好的時間。  主機組織層是透過一般的方式根據其原始檔時間 w32time 來判斷。  Windows 2016 最精確時鐘]，而非預設為主機，將會尋找來賓加入網域。  基於這個原因，我們建議您手動停用 HYPER-V 時間提供設定參與 Windows 2012R2 和下方網域的電腦是。

### <a name="monitoring"></a>監視
已新增效能監視器。  這些基準，可讓您監視和疑難排解時間準確度。  這些計數器包括：

計數器|描述|
----- | ----- |
計算時差時間|   計算 W32Time 服務毫秒位移系統時鐘和選擇的時間來源，之間絕對時間。 新的有效範例可使用時，會更新計算的時間時間時差範例所示。 這是本機時鐘的實際時間時差。 W32time 初始使用此時差時鐘修正，並需要套用至本機時鐘剩餘的時間時差更新計算的時間空行範例。 可以使用此效能計數器低輪詢間隔追蹤時鐘正確性 (例如： 256 秒或較少)，並尋找想要的時鐘正確性限制小於計數器值。|
時鐘頻率調整| 本機系統時鐘由 W32Time billion 每個部分絕對時鐘頻率調整。 這個計數器協助視覺化來 W32time 正在執行的動作。|
NTP 往返延遲|    在收到來自伺服器的回應毫秒 NTP Client 遇到的最新來回延遲。 這是次經過 NTP client 之間傳輸到 NTP 伺服器要求和有效的回應收到來自伺服器上。 這個計數器協助描述 NTP client 遇到的延遲。 變大或不同往返可以新增雜音 NTP 時間計算，依序可能會影響透過 NTP 同步處理時間準確度。|
NTP Client 來源計數|    使用中的 [由 NTP Client NTP 的時間來源數目。 這是計數的作用中的不同的回應此 client 的要求的時間伺服器的 IP 位址。 放大或縮小比設定等，根據 DNS 解析度等名稱與目前的範圍功能，可能會這個號碼。|
伺服器 NTP 收到的要求|   要求 NTP （要求/秒） 伺服器接收到的號碼。|
NTP 伺服器傳出回應|  要求回答 NTP 伺服器 （回應/秒） 數目。|

第一次 3 計數器為目標，以取得疑難排解正確性問題案例。  疑難排解時間正確性和 NTP 區段下，在[最佳做法](#BestPractices)，有更多詳細資料。
最後一次 3 計數器 NTP 伺服器案例實體鍵盤保護蓋，並會很有幫助時判斷基準與載入您目前的效能。

### <a name="configuration-updates-per-environment"></a>每個環境組態更新
下列告訴您的變更，在舊版 Windows 2016 之間的預設設定為每個角色。  Windows Server 2016 和 Windows 10 年度 Update(build 14393) 的設定是現在獨特的原因有會顯示為不同的欄。 

|角色|設定|Windows Server 2016|Windows 10 版本 1607|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**獨立日 Nano Server**||||
| |*時間伺服器*|time.windows.com|NA|time.windows.com|
| |*輪詢頻率*|64 位 1024 秒|NA|一個星期了一次|
| |*時鐘更新的頻率*|第二個一次|NA|一小時|
|**獨立 Client**||||
| |*時間伺服器*|NA|time.windows.com|time.windows.com|
| |*輪詢頻率*|NA|一天|一個星期了一次|
| |*時鐘更新的頻率*|NA|一天|一個星期了一次|
|**網域控制站**||||
| |*時間伺服器*|GTIMESERV PDC 日|NA|GTIMESERV PDC 日|
| |*輪詢頻率*|64-1024 秒數|NA|1024-32768 秒|
| |*時鐘更新的頻率*|一天|NA|一個星期了一次|
|**網域成員伺服器**||||
| |*時間伺服器*|DC|NA|DC|
| |*輪詢頻率*|64-1024 秒數|NA|1024-32768 秒|
| |*時鐘更新的頻率*|第二個一次|NA|在每個 5 分鐘|
|**網域成員 Client**||||
| |*時間伺服器*|NA|DC|DC|
| |*輪詢頻率*|NA|1204-32768 秒|1024-32768 秒|
| |*時鐘更新的頻率*|NA|在每個 5 分鐘|在每個 5 分鐘|
|**HYPER-V 來賓**||||
| |*時間伺服器*|選擇最佳的選項，根據主機與的時間伺服器的組織層|選擇最佳的選項，根據主機與的時間伺服器的組織層|預設為主機|
| |*輪詢頻率*|根據上方的角色|根據上方的角色|根據上方的角色|
| |*時鐘更新的頻率*|根據上方的角色|根據上方的角色|根據上方的角色|

>[!NOTE]
>適用於 Linux HYPER-V 中，請查看[允許 Linux 使用 HYPER-V 主機時間](#AllowingLinux)一節。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>影響提升的輪詢及時鐘更新的頻率
為了提供更多正確的時間，可讓我們經常進行微調增加輪詢的頻率及時鐘更新的預設值。  這會讓更多 UDP 日 NTP 流量、 不過，這些封包小，您應該會有一些很或不會影響到寬頻連結。 好處，不過，是這次應該會更多種的硬體及環境較佳。

電池備份裝置的頻繁輪詢可能會造成問題。  電池裝置無法儲存時間時關閉。  當他們繼續時，它可能需要時鐘常用的修正。  輪詢頻繁會導致變得不穩定的時鐘，您也可以使用更多電力。  Microsoft 建議您不要變更預設 client 的設定。

網域控制站應該受影響最小即使有公式提升更新從 NTP AD 網域中的效果。  NTP 有較小資源消耗相較於其他通訊協定與臨界的影響。  您將能更到之前 Windows Server 2016 提高的設定會受到限制其他網域功能。  Active Directory 會使用安全 NTP，這通常會以較不精確比簡單 NTP 同步處理時間，但我們已經確認它將會縮放用兩戶端階層 PDC 原位。

保守計劃，為您應該會保留 100 NTP 要求核心每秒。  例如，組成 4 網域控制站的 4 個核心網域中，您應該可以服務 1600 NTP 要求秒。  如果您已經設定同步處理時間在每個 64 秒到 10 k 戶端要求隨著時間收到一致，您會看到 [64 10000 日或約 160 要求秒分散在所有的網域控制站。  這個落輕鬆我們 1600 NTP 秒要求此範例中為基礎。  這些都是保守計劃的建議，當然有大型相依性在您的網路，處理器速度和載入，如往常，基準並測試您的環境中。

也很重要，請注意，如果您的網域控制站執行，仍的 CPU 載入，超過 40%，這將會幾乎歷經各種暴風雨雜音加入 NTP 回應會影響您的時間準確度在您的網域。  同樣地，您必須測試結果實際了解您的環境中。

## <a name="time-accuracy-measurements"></a>時間正確性度量單位
### <a name="methodology"></a>方法
若要針對 Windows Server 2016 測量時間正確性，我們使用各種不同的工具，方法環境。  您可以使用這些技術測量和調整您的環境並判斷正確性結果是否符合您需求。 

我們網域來源時鐘所組成 GPS 硬體兩部高精確 NTP 伺服器。  我們也會使用不同參考測試電腦的度量單位，這也是從不同的製造商所安裝的高精確式 GPS 硬體。  部分的測試，您將需要使用做為您的網域時鐘來源除了參考準確且可靠的時鐘來源。

我們用來測量正確性實體和虛擬電腦的四個不同的方法。 多個方法提供獨立的方式來驗證結果。


1. 測量本機時鐘]，這由 w32tm 條件，針對我們有另一個 GPS 硬體的參考測試電腦。  
2.  測量 NTP ping 從 NTP 伺服器 W32tm 」 stripchart 」 用來
3.  測量 NTP ping 從 client NTP 伺服器使用 W32tm 」 stripchart 」
4.  測量 HYPER-V 結果的使用時間戳記計數器 (TSC) guest 主機。  這兩個磁碟分割中這個計數器共用之間的磁碟分割和系統時間。  我們計算主機和 client 時間的差在一樣。  然後我們會使用 TSC 時鐘後的度量單位不會發生在此同時，請插入來賓，從主機時間。  同時，我們會使用延遲和延遲 TSV 時鐘隔離 API 中。

W32tm 建，但我們在我們測試期間所使用的其他工具會為您的測試及使用開放原始碼適用於 Microsoft 在 GitHub 存放庫。  在存放庫 WIKI 有如何使用工具來執行測量的詳細資訊。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

測試結果顯示，以下是我們的測試環境的所做的度量單位子集。  它們闡述正確性維護時間階層和結尾的時間階層子女網域 client 的開頭。  這是相較於在 2012 根據拓撲相同電腦進行比較。

### <a name="topology"></a>拓撲
如需比較，我們測試的 Windows Server 2012R2 與 Windows Server 2016 根據拓撲。  這兩個拓撲包含兩個實體的 HYPER-V 主機上安裝 GPS 時鐘硬體參考 Windows Server 2016 的電腦。  每個主機執行 3 加入網域 windows 來賓，這根據下列拓撲排列。  行代表時間階層，以及使用通訊協定日傳輸。

![Windows 時間](media/Windows-2016-Accurate-Time/topology1.png)

![Windows 時間](media/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>圖形結果概觀
下列兩個圖形代表根據上述拓撲網域中的兩個特定成員時間準確度。  每個圖形顯示 2016年結果顯示和 Windows Server 2012R2 的視覺示範改進。  正確性是測量的位在主機相較於來賓電腦。  表示我們已完成的測試整組子集圖形資料，並顯示的最佳和最差的案例。  

![Windows 時間](media/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根網域 PDC 的效能
根 PDC 同步處理至 HYPER-V 主機 （使用 VMIC） 也就是 Windows Server 2016 GPS 硬體準確且穩定證明。  這是嚴重 1 ms 正確性，會顯示為遺漏灰色區域的需求。

![Windows 時間](media/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子女網域 Client 的效能
子女網域 Client 已連接到子女網域 PDC 的根 PDC 進行通訊。  時間也可在 1 ms 需求。

![Windows 時間](media/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>測試左上方
下表比較 1 virtual 網路躍點 」 來與 Windows Server 2016 6 實體網路躍點。  有兩個圖表是覆蓋彼此透明度顯示重疊的資料。  增加躍表示更高版本延遲和較大的時間偏差。  圖表是放大和如此 1 ms 範圍中，由遺漏] 區域中，會放大。  您可以看到是仍在 1 ms 使用多個躍點。  它會負面移，這證明了網路為中心不對稱。  當然，每個網路不同，且測量許多環境因素而定。

![Windows 時間](media/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>準確 timekeeping 最佳做法
### <a name="solid-source-clock"></a>實心來源時鐘
只有一樣來源時鐘與進行同步電腦的時間。  為了實現 1 ms 的正確性，您將需要 GPS 硬體或時間應用裝置為主要來源時鐘您參考您網路上。  使用預設的 time.windows.com，可能無法提供穩定與當地的時間來源。  此外，當您遠離來源時鐘、 網路影響準確度。  有一個主要來源時鐘中每個資料中心] 是必要的最佳準確度。

### <a name="hardware-gps-options"></a>硬體 GPS] 選項
有各種不同的正確時間提供硬體方案。  一般而言，方案今天根據 GPS 天線。  也有電台及使用專用的行撥號數據機方案。  它們連接到您的網路為應用裝置，或是插入電腦的執行個體透過 PCIe 或 USB 裝置的 Windows。  其他不同選項將會提供的正確性、 的不同層級，如往常，結果而定您的環境。  變數影響的正確性，包括 GPS 可用性、 網路穩定性和載入和電腦的硬體。  這些是所有重要因素時選擇來源時鐘]，如我們之前聲明中所，是穩定和正確時間的需求。

### <a name="domain-and-synchronizing-time"></a>網域和同步處理時間
網域成員使用網域層判斷的電腦同步處理時間使用做為來源。  每個網域成員找到另一部電腦同步的並將它儲存為的時鐘來源。  每種類型的網域成員時鐘來源尋找同步處理時間才能依照一組規則。  在 [樹系根 PDC 是所有網域預設時鐘來源。  以下列出的不同的角色，以及高層級描述尋找來源的方式：


- **使用 PDC 角色網域控制站**– 這台電腦是網域的授權的時間來源。 它會網域中有提供最正確時間，必須以外於同步 DC 父系網域中的其中[GTIMESERV](#GTIMESERV)的角色支援。 
- **任何其他網域控制站**– 這台電腦做為的時間來源戶端和成員網域中的伺服器。 DC 可以 PDC 的自己網域，或其家長網域中的任何俠同步。
- **戶端/成員伺服器]** – 這台電腦可以同步的任何俠或自己網域，或俠 PDC PDC 家長網域中的。

根據可用的候選項目，要尋找的最佳的時間來源使用評分系統。  系統會考慮的時間來源及的相對位置的可靠性。  此選項出現時之後與的時間何時開始服務。  如果您需要有進一步控制如何同步處理時間，您可以在特定位置新增對的時機伺服器，或新增冗餘。  查看[指定本機可靠時間服務使用 GTIMESERV](#GTIMESERV)區段，如需詳細資訊。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>作業系統混合的環境 （Win2012R2 和 Win2008R2）
時所需的最佳正確性純真 Windows Server 2016 網域環境，仍有權益在混合的環境中。  部署 Windows Server 2016 HYPER-V Windows 2012 網域中將前提來賓，因為我們上述，但僅限如果來賓也是 Windows Server 2016 的改良功能。  Windows Server 2016 PDC，將無法提供更加準確的時間，因為它將會改進的演算法更穩定的來源。  以取代您 PDC 可能不是選項，您可以改用新增與 Windows Server 2016 DC [GTIMESERV](#GTIMESERV)相簿設定為在您的網域正確性升級的。  Windows Server 2016 DC 可以下游時間戶端以提供更好的時間，不過，只有來源 NTP 時間一樣。

還，如上述，時鐘輪詢並重新整理頻率經過修改以 Windows Server 2016。  這些可以手動變更您的舊版 Dc 或透過群組原則套用。  我們尚未測試這些設定，而這些應該也中 Win2008R2 Win2012R2 行為，以及提供一些權益。

Windows Server 2016 有多個問題保留正確的時間讓，會導致系統在進行調整之後會立即時間變動之前的版本。  因為經常取得時間範例準確 NTP 來源和條件本機時鐘與資料會導致系統時鐘在較小積雪內取樣期間，導致更好的時間維持在舊版的作業系統版本中。 觀察到的最佳正確性是約 5 ms 當 Windows Server 2012R2 NTP Client、 高不正確設定，以設定同步準確 Windows 2016 NTP 伺服器的時間。

有關來賓網域控制站某些案例中，在 HYPER-V TimeSync 範例可中斷網域時間同步處理。  這應該不會再 Server 2016 來賓 Server 2016 HYPER-V 主機上執行的問題。

若要停用 HYPER-V TimeSync 服務無法提供以 w32time 範例，設定下列來賓機碼：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>讓使用 HYPER-V 主機時間 Linux
適用於 Linux 來賓 HYPER-V 中執行，通常是針對 NTP 伺服器同步處理時間使用的 NTP 精靈設定戶端。  如果 Linux distribution 支援 TimeSync 版本 4 通訊協定 Linux 來賓已支援 「 TimeSync 整合服務，它將會同步處理針對主機時間。 這可能會導致保留是否支援這兩種方法是一致的時間。

若要同步專屬針對主機時，建議停用 NTP 時間同步處理被：

- 停用 ntp.conf 檔案中的任何 NTP 伺服器
- 或停用的 NTP 精靈

此設定，時間伺服器參數為這個主機。  它輪詢的頻率 5 秒，時鐘更新頻率也且 5 秒鐘。

若要同步到 NTP 專屬，建議停用來賓 TimeSync 整合服務。

> [!NOTE]
> 注意： Linux 來賓的正確時間支援需要最新上游 Linux 核心僅限支援的功能，它不常使用的所有 Linux distros 上尚未的項目。 請參考[適用於 windows HYPER-V 支援 Linux 和 FreeBSD 虛擬機器](https://technet.microsoft.com/en-us/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)的支援散發有關更多詳細資料。

#### <a name="GTIMESERV"></a>指定本機可靠的時間服務使用 GTIMESERV
您可以指定一或多個網域控制站做精確的來源時鐘使用 GTIMESERV，好的時間伺服器，旗標。  例如，配備 GPS 硬體特定網域控制站可以標示 GTIMESERV。  這可確保您的網域參考時鐘根據 GPS 硬體。

> [!NOTE]
> 在 [找到詳細資訊網域旗標[MS-ADTS 通訊協定的文件](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一個相關的網域服務旗標表示授權目前的電腦是否，它可以變更 DC 失去連接。  在這種狀態 DC 將 「 未知階層 「 透過 NTP 查詢時。  嘗試多次之後, 將 DC 登入系統事件時間服務事件 36。

如果您想要為 GTIMESERV 設定 DC，這可以使用下列命令，以手動方式來設定。  在這種情形下 DC 做為主要時鐘使用另一部電腦。  這可能是應用裝置或專用的電腦。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 如需詳細資訊，請查看[設定 Windows 時間服務](https://technet.microsoft.com/library/cc731191.aspx)

如果 DC 已安裝的 GPS 硬體，您需要使用這些步驟來停用 NTP client 以及 NTP 伺服器。

[開始]，來停用 NTP Client 以及 NTP 伺服器使用這些登錄重要變更。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下來，重新開機 Windows 時間服務

    net stop w32time && net start w32time

最後，表示這部電腦已使用可靠的時間來源。
   
    w32tm /config /reliable:yes /update

若要查看已正確完成所做的變更，您可以執行下列命令，影響結果如下所示。 

    w32tm /query /configuration

值。|預期的設定|
----- | ----- |
AnnounceFlags|  5 （本機）|
NtpServer   |（本機）|
DllName |C:\WINDOWS\SYSTEM32\w32time。DLL （本機）|
支援 |1 （本機）|
NtpClient|  （本機）|

    w32tm /query /status /verbose

值。|  預期的設定|
----- | ----- |
組織層|    1 （主要參考-syncd 廣播時鐘）|
ReferenceId|    0x4C4F434C (來源名稱: 「 本機 」)|
來源| 本機 CMOS 時鐘|
階段時差|   0.0000000s|
伺服器角色|    576 （可靠的時間服務）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>Windows Server 2016 上 3 派對 Virtual 平台
Windows 擬化檔案，預設 Hypervisor 時，提供時間負責。  但加入網域成員需要要同步的網域控制站的 Active Directory 正常運作。  最好來賓和任何 3 廠商 virtual 平台主機間的任何時間模擬停用。

#### <a name="discovering-the-hierarchy"></a>探索階層
因為的時間階層主要時鐘來源，而且動態網域中交涉，您需要查詢狀態的時間來源，而且鏈結主要來源時鐘了解的出處電腦。  這可協助診斷階段同步處理的問題。

提供您想要進行疑難排解的特定 client;使用這個 w32tm 命令來了解它的時間來源是第一個步驟。

    w32tm /query /status

結果顯示其他項目之間的來源。  來源表示與您同步網域中的時間。  這是此電腦的時間階層的第一個步驟。
接著會使用上述來源項目，並使用 /StripChart 參數鏈結中找到的下一步的時間來源。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

也有很有用，下列命令列出每個找到指定網域中的網域控制站與列印可讓您判斷每個協力廠商的結果。  這個命令，將會包含電腦，可以手動設定。
    
    w32tm /monitor /domain:my_domain

使用清單，您可以透過網域結果沿著湖邊繪製，並階層為時間時差每個步驟以了解。  找出點時間時差取得大幅糟位置，您可以找出根的正確時間。  您可以嘗試了解為何當時是正確打開從[w32tm 登入](#W32Logging)。 

#### <a name="using-group-policy"></a>使用群組原則
您可以使用群組原則來完成更嚴格準確度，例如，指派戶端使用特定 NTP 伺服器或控制如何舊版作業系統的設定時擬化檔案。  
以下是可能案例和相關的群組原則設定的清單：

**網域擬化檔案**-以 Windows 2012R2 控制擬化檔案網域控制站，讓他們與他們網域同步處理時間，而非與 HYPER-V 主機，您可以停用這個登錄項目。   Pdc，您不想要為 HYPER-V 主機，將提供最穩定的時間來源，停用的項目。  變更後重新開機 w32time 服務會要求登錄項目。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**正確性機密載入**-時間正確性機密工作負載，您可以設定群組的電腦設定的 NTP 伺服器，任何相關的使用時間設定，例如輪詢和時鐘更新的頻率。  這通常會由網域，但更多的控制，您可以針對特定電腦直接指向主要時鐘。

群組原則設定|   新值。|
----- | ----- |
NtpServer|  ClockMasterName 0x8|
MinPollInterval|    6 – 64 秒|
MaxPollInterval|    6|
UpdateInterval| 100 – 第二個每一次|
EventLogFlags|  3-所有特殊時間登入|

> [!NOTE]
> NtpServer 和 EventLogFlags 設定位於 System\Widows 時間 Service\Time 提供者使用的 Windows 設定 NTP Client 設定。  其他 3 位於 System\Windows 時間服務使用全球設定。

**遠端正確性機密載入遠端**– 適用於執行個體零售和付款信用卡 Industry (PCI) 分支網域中的系統，Windows 會使用目前的網站資訊，並尋找 [本機 DC，除非另有手動 NTP DC 定位的時間來源的設定。  此環境需要的正確性、 使用正確的時間來更快速地聚合 1 第二部分。  此選項可讓向後移動時鐘 w32time 服務。  如果這是可接受並符合您需求，您可以建立下列原則。   在任何環境] 中，以確保測試及基準您的網路。 

群組原則設定|   新值。|
----- | ----- |
MaxAllowedPhaseOffset|  1 超過在第二個，是否為時鐘正確的時間。|

MaxAllowedPhaseOffset 設定位於 System\Windows 時間服務使用全球設定。

> [!NOTE]
> 如需在群組原則和項目相關的資訊，請查看[Windows 時間服務工具](windows-time-service-tools-and-settings.md)設定參考 TechNet 上的文章。

#### <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 考量

##### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 一樣： Active Directory Domain Services
執行 Active Directory Domain Services Azure VM 是否現有先部分 Active Directory 森林，然後 TimeSync(VMIC) 應該停用。 這是為了讓所有網域控制站實體和 virtual，森林中的使用單一時間同步階層。 請參考最佳做法白皮書[「 執行網域控制站在 HYPER-V 」](https://technet.microsoft.com/en-us/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

##### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 一樣： 加入網域的電腦
如果您主控也就是現有的 Active Directory 樹系加入網域的電腦，virtual 或實體，最好的做法是 TimeSync 來賓停用，並確保 W32Time 已進行同步處理與設定的時間型透過為網域控制站 = NTP5

##### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 一樣： 獨立群組的電腦
如果 Azure VM 未加入網域，也不是網域控制站，建議持續時間預設設定，並讓 VM 與主機同步處理。

### <a name="windows-application-requiring-accurate-time"></a>Windows 應用程式要求正確的時間
#### <a name="time-stamp-api"></a>頻率 API
有關 UTC，並不時間的經過正確性最大的程式應該使用[GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  這樣可確保您的應用程式可取得 Windows 時間服務會條件系統時間。

#### <a name="udp-performance"></a>UDP 效能
如果您有連接埠篩選引擎的基底使用 UDP 通訊的交易，它的重要最小化延遲，有一些有關登錄項目，您可以使用它們來設定連接埠，以排除一系列應用程式。  將提升兩個延遲並提高您處理能力。  不過，應該限於經驗系統管理員登錄的變更。  此外，這個因應措施排除連接埠防火牆受保護。  查看下列的文章參考如需詳細資訊。

適用於 Windows Server 2012 和 Windows Server 2008，您必須先安裝 Hotfix。  您可以參考此知識庫文章：[當您在 Windows 8 和 Windows Server 2012 中執行多點收件者的應用程式的資料流遺失](https://support.microsoft.com/en-us/kb/2808584)

#### <a name="update-network-drivers"></a>更新網路驅動程式
某些網路廠商有驅動程式更新，以改善效能有關延遲驅動程式和緩衝 UDP 封包。  請連絡您的網路廠商，看看是否有更新可協助 UDP 處理能力。

### <a name="logging-for-auditing-purposes"></a>稽核進行登入
為遵守用時間追蹤法規您以手動方式可以保存 w32tm 登、 事件登和效能監視器資訊。  之後，封存的資訊可以用於證明過的特定時間的相容性。  下列因素用來指出準確度。


1. 使用時間計算位移效能監視器計數器時鐘準確度。  這會顯示的時鐘與中您想要準確度。
2.  時鐘尋找 「 等回應從 「 w32tm 登入的來源。   下列訊息文字是 VMIC 描述的時間來源，下一步] 中的參考時鐘鏈結驗證的 IP 位址。
3.  時鐘條件狀態使用 w32tm 登驗證的 「 ClockDispl 訓練： \*SKEW\*TIME\* 」 會發生。  這表示該 w32tm 使用時間。

#### <a name="event-logging"></a>事件登入
若要取得完成故事，您也會需要事件登入資訊。  收集系統事件登入並篩選上時間伺服器、 Microsoft Windows 核心-開機、 Microsoft-Windows-核心-一般，您可以探索是否有變更時，例如，第三方其他影響。  這些登可能需要排除外部干擾。  群組原則可能影響到的事件登寫入登入。  使用群組原則看到上述的一節，以取得詳細資訊。

#### <a name="W32Logging"></a>W32time 偵錯登入
若要讓 w32tm 稽核用途，下列命令可讓所顯示的時鐘定期更新，並指出來源時鐘登入。  重新開機，可讓新的登入服務。  

如需詳細資訊，請查看[如何關閉登入 Windows 時間服務偵錯在](https://support.microsoft.com/en-us/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

#### <a name="performance-monitor"></a>效能的監視器
Windows Server 2016 Windows 時間服務公開效能計數器用於收集稽核登入。  這些可在本機或遠端電腦上，登入。  您可以在電腦的時間位移和來回延遲計數器記錄。  
與任何計數器，例如您可以從遠端監視它們，並建立使用 System Center Operations Manager 警示。  例如，您可以使用警示時所需的正確性從時間位移 drifts 警示您。  [系統中心管理組件](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)有更多的資訊。

#### <a name="windows-traceability-example"></a>Windows 利用範例
從 w32tm 登入檔案，您將會要驗證兩個項資訊。  首先，登入檔案目前條件時鐘的指示。  這證明，已成為您時鐘 Windows 時間服務容許爭議時間。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

主要是您看到的訊息，這是年齡 w32time ClockDispln 訓練加上與您的系統時鐘互動。
 
接下來，您需要登入找到上次的報告之前爭議報告做為參考時鐘目前正在使用的來源電腦的時間。  這可能是 IP 位址、 電腦名稱或 VMIC 提供者，表示它已同步的 HYPER-V 主機。  下列範例提供 10.197.216.105 IPv4 位址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

既然您已經驗證參考時間鏈結中的第一次系統，您需要調查參考資料的時間來源登入檔案，並重複相同的步驟。  直到像是 GPS 或已知的時間來源，例如 NIST 實體時鐘]，以繼續。  如果參考時鐘 GPS 硬體，然後從製造登也可能需要。

### <a name="network-considerations"></a>網路注意事項
NTP 通訊協定演算法對稱您網路上有相依性。  當您增加躍數目的機率為中心不對稱增加。  有，很難預測您特定的環境中將會看到精確度的類型。 

效能監視器和 Windows Server 2016 中的新 Windows 時間計數器可用於評估您的環境準確度和建立的基準。 此外，您可以執行疑難排解，來判斷時差目前的任何電腦上您的網路。

有兩個一般標準正確的時間在網路上。  PTP ([精確度時間通訊協定-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) 網路基礎結構擁有更緊密需求，但通常可以提供子微秒準確度。  NTP ([網路時間通訊協定 – RFC 1305](https://tools.ietf.org/html/rfc1305)) 上看起來各種不同的網路及的環境中，讓它變得更容易管理的運作方式。 

Windows 預設非網域連接電腦的支援簡單 NTP (RFC2030)。  針對加入網域的電腦，我們會使用安全 NTP 稱為[MS-SNTP](https://msdn.microsoft.com/en-us/library/cc246877.aspx)，使用網域交涉可提供 RFC1305 RFC5905 中所述驗證 NTP 管理優點。   

網域與非網域結合通訊協定要求 UDP 連接埠 123。  如需 NTP 最佳做法的詳細資訊，請參考[網路時間通訊協定最佳目前做法 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>為了硬體時鐘 (RTC)
Windows 會不步驟時間，除非特定範圍超過，但而 disciplines 時鐘。  這表示 w32tm 調整頻率時鐘的頻率，使用時鐘更新頻率設定，以在第二與 Windows Server 2016 的預設值。  如果時鐘之後，它加速的頻率並繼續時，是否會變慢的頻率。  不過，之間時鐘頻率調整當時硬體時鐘是控制。  如果有時鐘硬體與韌體的問題，在電腦上的時間可以變得較不精確。

這是另一個原因，您需要測試及您的環境中的基準。  如果 」 計算時間位移 「 效能計數器不會在您的目標的正確性穩定，您可能想要驗證您的韌體是最新狀態。  與其他測試，您可以查看重複硬體重現相同的問題。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>疑難排解時間正確性和 NTP
您可以使用發掘階層上面了解不正確的時間來源。  查看時間時差、 階層時間位置出現其 NTP 來源最中找到點。  了解階層之後, 您會想要嘗試並了解為何該特定時間來源不會收到正確的時間。  

將焦點放在不同時間使用的系統上，您可以使用這些工具下列以收集更多資訊，以協助您判斷這個問題並尋找解析度。  下方，UpstreamClockSource 參考是時鐘發現使用 「 w32tm /config /status 」。


- 系統事件登
- 讓登入使用： w32tm 登-w32tm /debug 情況下 /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- w32Time 登錄 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本機網路追蹤
- 正在 （從本機或 UpstreamClockSource）
- W32tm /stripchart /computer:UpstreamClockSource
- 了解延遲和躍點來源數目 PING UpstreamClockSource
- Tracert UpstreamClockSource

問題|    症狀|   解析度|
----- | ----- | ----- |
本機 TSC 時鐘不穩定。| 使用實體電腦 – 效能同步時鐘穩定時鐘]，但您仍然會看到每個 1-2 分鐘的時間的幾個 100us。 |   更新韌體或驗證不同的硬體不會顯示相同的問題。|
網路延遲|    w32tm stripchart 顯示為超過 10 ms RoundTripDelay。  在延遲接受造成一樣大的來回時間，例如只是一個方向延遲 ½ 雜音。</br></br>UpstreamClockSource 為多個躍點，PING 所示。  TTL 應該接近 128。</br></br>使用 Tracert 在每個躍點尋找延遲。    | 尋找更仔細地時鐘來源的時間。  一個方案是安裝在同一個區段的來源時鐘或手動指向來源的地理位置靠近時鐘。  加入網域案例中，GTimeServ 角色一部電腦。 |  
不會可靠地瑞曲之戰 NTP 來源|    W32tm /stripchart 間歇性傳回 」 要求逾時]    |NTP 來源無法回應|
NTP 來源無法回應|    檢查效能計數器 NTP Client 來源計數、 NTP 伺服器傳入的要求，NTP 伺服器傳出回應，並判斷您使用相較於您的基準。|    使用伺服器效能計數器，判斷是否載入已變更您的基準對照。</br></br>有網路壅塞問題嗎？|
未使用的最精確時鐘網域控制站|    變更拓撲或最近新增的主要時間時鐘中。|   w32tm /resync /rediscover|
變動 client 時鐘| 服務時間事件 36 系統事件登入和/或文字登入檔案，描述: 「 NTP Client 的時間來源計數 「 計數器 1 前往 0|疑難排解上游來源，並了解是否正在執行的效能問題。|

### <a name="baselining-time"></a>設定基準時間
使您第一次，了解的效能和準確度您的網路，並比較基準未來發生問題時，設定基準很重要。  您想要基準根 PDC 或任何電腦標示 GTIMESRV。  我們也建議您基準 PDC 每個森林中。  最後挑選重要的網域控制站或電腦已有趣特性，例如距離或許多與高和基準這些。

也很有幫助基礎 Windows Server 2016 與 2012 R2，不過您只需要 w32tm /stripchart 為比較，因為 Windows Server 2012R2 不會有計數器效能，您可以使用此工具。  您應該選擇相同的特性兩部電腦或升級電腦，並比較更新後的結果。  Windows 時間度量單位增補有更多有關如何詳細的度量單位之間 2016年 2012年。

使用所有 w32time 效能計數器，至少一個星期了收集的資料。  這樣可確保您擁有的不同隨著時間網路中的參考及執行，以提供您的時間準確度能穩定信賴的。

### <a name="ntp-server-redundancy"></a>NTP 伺服器冗餘
手動 NTP 伺服器設定與非網域中加入的電腦或 PDC 搭配使用，有一個以上的伺服器是告訴您一個好冗餘測量在可用性。  它也可能會提供更好的正確性、 假設所有來源都精確與穩定。  不過，如果拓撲無法運作的設計，或不穩定的時間來源，結果正確性可能更糟，小心謹慎。  伺服器 w32time 可以手動參考支援階段的上限是 10。 

## <a name="leap-seconds"></a>閏秒
地球旋轉期間變化時，造成 climatic 和 geological 事件。 一般而言，可接受約一秒每隔幾年。 只要從不可時間成長太大，一秒（向上或向下）的校正插入，稱為閏秒。 這是一種不同的永遠不會超過 0.9 秒。 此修正是宣布實際修正六個月。 Windows Server 2016 之前 Microsoft 時間服務不知道閏秒鐘，，但依賴外部時間處理這些服務。 與 Windows Server 2016 的時間正確性，Microsoft 正常運作的更多適合用於閏第二個問題。

## <a name="secure-time-seeding"></a>安全播種的時間
W32time Server 2016 中的包含安全時間播種的功能。 這項功能會判斷出 SSL 來自大約目前的時間。  此時間值用來監視本機系統時鐘和修正總錯誤。 您可以朗讀更多有關功能的[的這篇部落格文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)。  部署可靠的時間來源與包含監視時間位移也監控的電腦，您可以選擇不使用 [安全時間播種的功能，改為使用您現有的基礎結構。 

您可以停用的功能與下列步驟：

1.  0 UtilizeSSLTimeData 登錄的設定值設特定電腦上：

    reg 新增 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t 呼叫完成 /d 0 /f


2.  如果因為某些原因立即重新開機，您可以通知 W32time 服務有關的組態更新。 這將會阻止階段監視和執法的時間資料收集從 SSL 連接。 

    W32tm.exe /config /update

3.  電腦重新開機一次可設定有效立即，也會造成停止 SSL 來自收集的任何資料的時間。  第二部分非常小的負荷，而且不應該效能問題。

4.  若要將此設定套用到整部網域中，請將 UtilizeSSLTimeData 值設定為 0 W32time 群組原則設定中，並發行設定。  設定取貨透過群組原則 Client，W32time 服務會收到通知，並時間監視與執法使用 SSL 時間資料，它將會停止。 每一部電腦重新開機時，將會停止 SSL 時間資料收集。 如果您的網域有膝上型電腦平板可移植纖薄和其他裝置，您可能想要排除這類的電腦，這項原則變更。 這些裝置最後會面臨電池電力流失和需要安全時間播種功能要他們的使用時間。














 













 




