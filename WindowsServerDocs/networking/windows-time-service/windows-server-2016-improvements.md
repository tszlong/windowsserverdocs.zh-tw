---
title: Windows Server 2016 的時間準確度改善
description: Windows Server 2016 已改善本機時鐘用來更正時間和條件以與 UTC 同步。
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.openlocfilehash: 7c0644d88e158050b83873f4398fe7ee87b120d5
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87997393"
---
# <a name="time-accuracy-improvements-for-windows-server-2016"></a>Windows Server 2016 的時間準確度改善

Windows Server 2016 已改善本機時鐘用來更正時間和條件以與 UTC 同步。 NTP 會根據用戶端要求/回應和伺服器要求/回應的時間戳記，使用 4 個值來計算時間位移。 不過，網路會產生雜訊，而且因為網路擁塞和其他影響網路延遲的因素，因此來自 NTP 的資料可能會激增。 Windows 2016 演算法會使用數種不同的技術平均地排除雜訊，讓時鐘運作穩定且準確。 此外，我們校正時間的來源會參考改進的 API，以便提供更佳的解決方案。 有了這些改進功能，我們就能夠達到跨網域 UTC 1 毫秒的準確度。

## <a name="hyper-v"></a>Hyper-V

Windows 2016 改進了 Hyper-v TimeSync 服務。 改進功能包括更準確的 VM 啟動或 VM 還原初始時間，以及提供給 w32time 的樣本中斷延遲修正。 這項改進可讓我們透過 RMS (均方根，表示代表變異數)，讓主機保持在 10μs；即使在具有 75% 負載的電腦上，也能保持在 50μs 的情況。 如需詳細資訊，請參閱 [Hyper-V 架構](https://msdn.microsoft.com/library/cc768520.aspx)。

> [!NOTE]
> 負載是使用 prime95 基準測試來建立，並使用平衡的設定檔。

此外，主機向客體回報的組織層級也更為透明。 之前，無論正確性為何，主機都會將固定組織層顯示為 2。 隨著 Windows Server 2016 中功能的變更，主機會回報大於主機組織層的層級，這會讓虛擬客體的時間更準確。 主機組織層是由 w32time 根據其來源時間以標準方式決定。 已加入網域的 Windows 2016 客體會找出最準確的時鐘，而不是主機預設的時鐘。 因此，建議您針對參與 Windows 2012R2 和以下網域的機器，手動停用 Hyper-V 時間提供者設定。

## <a name="monitoring"></a>監視

新增了效能監視計數器計數器。 這些計數器可讓您了解時間準確度的基準，並加以監視和進行疑難排解。 這些計數器包括：

|計數器|說明|
|----- | ----- |
|計算的時間位移| 系統時鐘與所選時間來源間的絕對時間位移，由 W32Time 服務計算 (以毫秒為單位)。 當有新的有效樣本可供使用時，會以樣本所指示的時間位移來更新計算的時間。 這是本機時鐘的實際時間位移。 W32time 會使用此位移起始時鐘更正作業，並以需要套用至本機時鐘的剩餘時間位移，更新樣本之間的計算時間。 您可以使用此效能計數器搭配低輪詢間隔 (例如 256 秒或更少) 來追蹤時鐘準確度，並尋找小於所需的時鐘準確度限制的計數器值。|
|時鐘頻率調整| W32Time 以每十億為單位，對本機系統時鐘進行絕對時鐘頻率調整。 此計數器有助於將 W32time 採取的動作以視覺化方式呈現。|
|NTP 來回延遲| NTP 用戶端最近從伺服器接收回應所經歷的來回延遲 (以毫秒為單位)。 這是在傳送要求到 NTP 伺服器，並從伺服器接收有效回應之間，NTP 用戶端所經過的時間。 此計數器有助於描述 NTP 用戶端所遇到的延遲特性。 較大或不同的來回作業可能會增加 NTP 時間計算的雜訊，這可能會影響透過 NTP 同步時間的準確度。|
|NTP 用戶端來源計數| NTP 用戶端使用的作用中 NTP 時間來源總數。 這是作用中、相異 IP 位址的時間伺服器計數，會回應此用戶端的要求。 數目可能大於或小於設定的同儕節點，視同儕節點名稱的 DNS 解析和目前的觸達能力而定。|
|NTP 伺服器連入要求| NTP 伺服器接收的要求數 (要求/每秒)。|
|NTP 伺服器連出回應| NTP 伺服器回答的要求數 (回應/每秒)。|

前 3 個計數器的目標是針對準確度問題進行疑難排解的案例。 最佳做法下的「時間準確度 NTP 疑難排解」一節會有更多詳細資料。
最後 3 個計數器涵蓋各種 NTP 伺服器案例，在判斷負載並將目前效能基準化時十分實用。

## <a name="configuration-updates-per-environment"></a>依環境更新組態

以下說明 Windows 2016 與先前版本的每個角色在預設組態中的變更。 目前 Windows Server 2016 和 Windows 10 年度更新版 (組建14393) 的設定是獨一無二的，因此會顯示為個別的資料行。

|[角色]|設定|Windows Server 2016|Windows 10|Windows Server 2012 R2<br>Windows Server 2008 R2<br>Windows 10|
|---|---|---|---|---|
|**獨立/Nano 伺服器**||||
| |時間伺服器|time.windows.com|NA|time.windows.com|
| |輪詢頻率|64 - 1024 秒|NA|一週一次|
| |時鐘更新頻率|每秒一次|NA|一小時一次|
|**獨立用戶端**||||
| |時間伺服器|NA|time.windows.com|time.windows.com|
| |輪詢頻率|NA|一天一次|一週一次|
| |時鐘更新頻率|NA|一天一次|一週一次|
|**網域控制站**||||
| |時間伺服器|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |輪詢頻率|64 -1024 秒|NA|1024 - 32768 秒|
| |時鐘更新頻率|一天一次|NA|一週一次|
|**網域成員伺服器**||||
| |時間伺服器|DC|NA|DC|
| |輪詢頻率|64 -1024 秒|NA|1024 - 32768 秒|
| |時鐘更新頻率|每秒一次|NA|每 5 分鐘一次|
|**網域成員用戶端**||||
| |時間伺服器|NA|DC|DC|
| |輪詢頻率|NA|1204 - 32768 秒|1024 - 32768 秒|
| |時鐘更新頻率|NA|每 5 分鐘一次|每 5 分鐘一次|
|**Hyper-V 客體**||||
| |時間伺服器|根據主機和時間伺服器的組織層選擇最佳選項|根據主機和時間伺服器的組織層選擇最佳選項|預設為主機|
| |輪詢頻率|根據上述角色|根據上述角色|根據上述角色|
| |時鐘更新頻率|根據上述角色|根據上述角色|根據上述角色|

>[!NOTE]
>針對 Hyper-V 中的 Linux，請參閱下方的「允許 Linux 使用 Hyper-V」主機時間一節。

## <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加輪詢和時鐘更新頻率的影響

為了提供更準確的時間，系統會增加輪詢頻率和時鐘更新的預設值，以便更頻繁地進行小幅度調整。 這會造成更多 UDP/NTP 流量；不過，這些封包很小，應該不會影響寬頻連結。 然而優點是，在更廣泛的硬體和環境中，應該能提供更準確的時間。

針對備有電池的裝置，增加輪詢頻率可能會產生問題。 電池裝置不會在關閉時儲存時間。 當裝置繼續時，可能需要經常更正時鐘。 增加輪詢頻率會導致時鐘變得不穩定，也可能更耗電。 Microsoft 建議您不要變更用戶端的預設設定。

即使從 AD 網域中的 NTP 用戶端增加更新會產生乘數效應，但網域控制站應該會受到最低限度的影響。 相較於其他通訊協定，NTP 耗用的資源量更小，而且會產生極大的影響。 在受到 Windows Server 2016 增加的設定影響之前，您更有可能先達到其他網域功能的限制。 Active Directory 會使用安全 NTP；相較於簡單 NTP，它的時間同步準確度不足，但比起 PDC，我們確定安全 NTP 會在用戶端中相應增加兩個組織層。

保守估計，您應該為每個核心應保留每秒 100 個 NTP 要求。 例如，由 4 個 DC 組成的網域，每個都有 4 個核心，如此每秒應該能服務 1600 個 NTP 要求。 如果 10000 個用戶端均設定為每 64 秒同步時間一次，而且隨著時間增加會收到一致的要求，則您會看到 10,000/64 或大約每秒 160 個要求分散到所有 DC 中。 此數值輕鬆地落在此範例中顯示的每秒 1600 個 NTP 要求內。 這些都是保守的規劃建議，而您的網路、處理器速度和負載都會影響實際結果，因此請務必在您的環境中設定基準並進行測試。

也請務必注意，如果您的 DC 執行相當多的 CPU 負載 (大於 40%)，則幾乎肯定會在 NTP 回應中增加雜訊，並影響網域中的時間準確度。 同樣地，您需要在環境中進行測試，以了解實際的結果。

## <a name="time-accuracy-measurements"></a>時間準確度測量

### <a name="methodology"></a>方法

為了測量 Windows Server 2016 的時間準確度，我們使用了各種不同的工具、方法和環境。 您可以使用這些技術來測量和調整環境，並判斷準確度結果是否符合需求。

我們的網域來源時鐘是由兩個具有 GPS 硬體的高準確度 NTP 伺服器所組成。 我們也使用個別的參照測試電腦進行測量，不同的電腦製造商也安裝了高準確度的 GPS 硬體。 在進行某些測試時，除了您的網域時鐘來源以外，還需要正確且可靠的時鐘來源做為參考。

我們使用四種不同的方法來測量實體和虛擬機器的準確度。 這些方法各自提供了獨立的方式來驗證結果。

1. 針對具有個別 GPS 硬體的參考測試電腦，測量由 w32tm 調整的本機時鐘。

2. 使用 W32tm “stripchart” 測量 NTP 伺服器對用戶端的 NTP 偵測

3. 使用 W32tm “stripchart” 測量用戶端對 NTP 伺服器的 NTP 偵測

4. 使用時間戳計數器 (TSC) 來測量從主機到客體的 Hyper-V 結果。 這個計數器會在兩個磁碟分割的分割區和系統時間之間共用。 我們計算了主機時間與虛擬機器中用戶端時間的差異。 接著使用 TSC 時鐘，從客體插入主機時間，因為不會同時進行測量。 此外，我們會在 API 中使用 TSV 時鐘找出延遲資訊。

系統內建 W32tm，但我們在測試期間使用的其他工具均為開放原始碼，並可在 GitHub 上的 Microsoft 存放庫中找到，以供您測試和使用。 如需如何使用工具來測量的詳細資訊，請參閱 [Windows 時間校正工具 Wiki](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

下面顯示的測試結果，是我們在其中一個測試環境中所做的測量子集。 這些結果說明在時間階層開始時維持的準確度，以及時間階層結束時的子網域用戶端。 這會與以 2012 為基礎的拓撲中的相同電腦進行比較。

### <a name="topology"></a>拓撲

為了進行比較，我們同時測試了 Windows Server 2012R2 和 Windows Server 2016 拓撲。 這兩種拓撲都是由兩部實體 Hyper-V 主機電腦所組成，並會參照已安裝 GPS 時鐘硬體的 Windows Server 2016 電腦。 每部主機都會執行 3 個已加入網域的 Windows 客體，並根據下列拓撲進行排列。 下方的線條代表時間階層，以及所使用的通訊協定/傳輸。

![Windows 時間拓撲圖表](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Windows 時間拓撲圖表](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>圖形化結果概觀

下列兩張圖表根據上述拓撲，呈現網域中兩個特定成員的時間準確度。 每張圖表都會顯示重疊的 Windows Server 2012R2 和 2016 結果，並以視覺化方式呈現改進項目。 相較於主機，準確度是從客體電腦中進行測量。 圖形化資料代表我們完成之整組測試的子集，並顯示最佳案例和最糟的情況。

![Windows 時間拓撲圖表](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根網域 PDC 的效能

根 PDC 會與 Hyper-V 主機同步 (使用 VMIC)，將 Windows Server 2016 搭配已經實證為準確且穩定的 GPS 硬體。 這是事關 1 毫秒準確度的關鍵需求，會顯示為綠色陰影區域。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子網域用戶端的效能

子網域用戶端會與根 PDC 通訊的子網域 PDC 連結， 其時間也在 1 毫秒的需求內。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)

### <a name="long-distance-test"></a>長距離測試

下圖比較 1 個虛擬網路躍點與 Windows Server 2016 的 6 個實體網路躍點。 兩張圖表會彼此重疊，以顯示重複的資料。 增加的網路躍點表示較高的延遲，以及較長的時間偏差。 圖表會放大以便於檢視，因此 1 毫秒的界限也會變大 (以綠色區域表示)。 如您所見，多個躍點的時間仍然在 1 毫秒內， 且朝負數移動，表示網路的不對稱。 當然，每個網路都不同，而測量結果會取決於許多環境因素。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="best-practices-for-accurate-timekeeping"></a>準確保留時間的最佳做法

### <a name="solid-source-clock"></a>穩固的來源時鐘

電腦時間與其同步的來源時鐘一樣準確。 為了達到 1 毫秒的準確度，您需要在參考的網路上使用 GPS 硬體或時間設備，做為主要來源時鐘。 使用預設的 time.windows.com 可能無法提供穩定的本機時間來源。 此外，當您離來源時鐘越遠，網路會影響準確度。 您必須在每個資料中心擁有主要來源時鐘，才能獲得最佳準確度。

### <a name="hardware-gps-options"></a>硬體 GPS 選項

有各種硬體解決方案可提供準確的時間。 一般來說，現今的解決方案是以 GPS 天線為基礎。 此外，也有使用專用線路的收音機和撥號數據機解決方案。 它們會以設備的形式與您的網路連結，或插入電腦，例如透過 PCIe 或 USB 裝置的 Windows。 不同的選項會提供不同層級的準確度，而且一律會根據您的環境而定。 影響準確度的變數包含 GPS 可用性、網路穩定性和負載，以及 PC 硬體。 如上所述，在選擇來源時鐘時，這些都是很重要的因素，是提供穩定和準確時間的必要條件。

### <a name="domain-and-synchronizing-time"></a>網域和同步時間

網域成員會使用網域階層來決定要將哪一台電腦當做同步時間的來源。 每個網域成員都會尋找要與之同步的另一部電腦，並將它儲存為時鐘來源。 每種類型的網域成員會遵循一組不同的規則，以便尋找同步時間的時鐘來源。 樹系根中的 PDC 是所有網域的預設時鐘來源。 以下列出不同的角色，以及這些角色如何尋找來源的高階描述：

- **具有 PDC 角色的網域控制站** - 這部電腦是網域的授權時間來源。 在網域中，它會有最準確的可用時間，而且必須與父系網域中的 DC 同步，除非啟用了 GTIMESERV 角色。
- **任何其他網域控制站** - 這部電腦將作為網域中用戶端和成員伺服器的時間來源。 DC 可以與自己網域的 PDC，或其父系網域中的任何 DC 同步。
- **用戶端/成員伺服器** - 這部電腦可以與自己網域的任何 DC 或 PDC，或父系網域中的 DC 或 PDC 同步。

根據這些可用的候選項目，系統會使用評分系統尋找最佳的時間來源。 此系統會考慮時間來源和其相對位置的可靠性。 當時間啟動服務時，就會發生這種情況。 如果您需要更準確地控制時間同步的方式，可以在特定位置新增適當的時間伺服器，或新增冗餘。 請參閱「使用 GTIMESERV 指定本機可靠時間服務」一節以取得詳細資訊。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合 OS 環境 (Win2012R2 和 Win2008R2)

雖然純粹的 Windows Server 2016 網域環境能是提供最佳準確度，但在混合環境中仍然有其優點。 根據先前提到的改進項目，在 Windows 2012 網域中部署 Windows Server 2016 Hyper-V 會讓客體受益，但前提是客體也是 Windows Server 2016。 Windows Server 2016 PDC 能夠提供更準確的時間，因為改進的演算法會是更穩定的來源。 您不一定要換掉 PDC，但可以改為新增具備 GTIMESERV 復原集的 Windows Server 2016 DC，這會讓您網域的準確度升級。 Windows Server 2016 DC 可以提供下游時間用戶端更準確的時間，但是最多與來源 NTP 時間相同。

同樣如上所述，時鐘輪詢和重新整理頻率已修改為 Windows Server 2016。 這些可以手動變更為下層的 DC，或透過群組原則套用。 雖然我們尚未測試這些組庇，但它們在 Win2008R2 和 Win2012R2 中的表現良好，也帶來了一些優點。

Windows Server 2016 之前的版本在保留正確的時間方面有著不少問題，這會導致系統時間在進行調整後立即偏離。 因此，經常從準確的 NTP 來源取得時間樣本，並以資料來調節本機時鐘，會導致系統時鐘在取樣期間發生些微的偏離，進而讓下層 OS 版本更能保證準確的時間。 我們觀察到的最佳準確度大約是 5 毫秒，會在以高準確度設定值設定的 Windows Server 2012R2 NTP 用戶端從準確的 Windows 2016 NTP 伺服器同步時間時發生。

在某些涉及客體網域控制站的情況下，Hyper-V TimeSync 樣本可能會中斷網域時間同步。 對於在伺服器 2016 Hyper-V 主機上執行的伺服器 2016 客體而言，這應該不再構成威脅。

若要停用 Hyper-V TimeSync 服務，使其無法提供樣本給 w32time，請設定下列客體登錄機碼：`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider
 "Enabled"=dword:00000000`

#### <a name="allowing-linux-to-use-hyper-v-host-time"></a>允許 Linux 使用 Hyper-V 主機時間

針對在 Hyper-V 中執行的 Linux 客體，系統通常會將用戶端設定為使用 NTP 精靈，以針對 NTP 伺服器進行時間同步。 如果 Linux 散發套件支援 TimeSync 第 4 版通訊協定，且 Linux 客體已啟用 TimeSync 整合服務，則系統會針對主機時間進行同步。 如果同時啟用了這兩種方法，可能會導致不一致的時間保留。

若要以獨佔方式同步主機時間，建議您使用下列其中一種方法停用 NTP 時間同步：

- 停用 ntp.conf 檔案中的任何 NTP 伺服器

- 或停用 NTP 精靈

在此組態中，時間伺服器參數為此主機。 其輪詢頻率為 5 秒，而時鐘更新頻率也是 5 秒。

若要以獨佔方式透過 NTP 進行同步，建議停用客體中的 TimeSync 整合服務。

> [!NOTE]
> 注意：支援 Linux 客體的準確時間需要最新的上游 Linux 核心才支援的功能，而且目前尚未在所有 Linux 散發版本上廣泛提供。 如需有關支援散發版本的詳細資訊，請參閱 [Windows 上支援的 Linux 和 FreeBSD 虛擬機器](../../virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)。

#### <a name="specify-a-local-reliable-time-service-using-gtimeserv"></a>使用 GTIMESERV 指定本機可靠時間服務

您可以使用 GTIMESERV、準確時間伺服器和旗標，將一或多個網域控制站指定為正確的來源時鐘。 比方說，配備 GPS 硬體的特定網域控制站可以標示為 GTIMESERV。 這可確保您的網域根據 GPS 硬體參考時鐘。

> [!NOTE]
> 如需有關網域旗標的詳細資訊，請參閱 [ADTS 通訊協定文件](/openspecs/windows_protocols/ms-winerrata/fe563333-6e4f-4198-9bf5-741a523cd0d7)。

TIMESERV 是另一個相關的網域服務旗標，指出電腦目前是否為授權狀態；如果 DC 失去連線，狀態可能會變更。 此狀態的 DC 會在透過 NTP 查詢時傳回「未知的組織層」。 經過多次嘗試後，DC 會記錄系統事件時間 - 服務事件 36。

如果您想要將 DC 設定為 GTIMESERV，可以使用下列命令手動設定。 在此情況下，DC 會使用另一部電腦做為主要時鐘。 這可能是設備或專用電腦。

```
w32tm /config /manualpeerlist:"master_clock1,0x8 master_clock2,0x8" /syncfromflags:manual /reliable:yes /update
```

> [!NOTE]
> 如需詳細資訊，請參閱[設定 Windows 時間 服務](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731191(v=ws.10))

如果 DC 已安裝 GPS 硬體，您必須使用下列步驟來停用 NTP 用戶端，並啟用 NTP 伺服器。

一開始請先停用 NTP 用戶端，然後使用這些登錄機碼變更來啟用 NTP 伺服器。

```
reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f
```

接著，重新啟動 Windows Time 服務

```
net stop w32time && net start w32time
```

最後，指出這部電腦具有可靠的時間來源。

```
w32tm /config /reliable:yes /update
```

若要檢查變更是否已正確完成，可以執行下列命令，這會影響如下所示的結果。

```
w32tm /query /configuration

Value|Expected Setting|
----- | ----- |
AnnounceFlags| 5 (Local)|
NtpServer |(Local)|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 (Local)|
NtpClient| (Local)|

w32tm /query /status /verbose

Value| Expected Setting|
----- | ----- |
Stratum| 1 (primary reference - syncd by radio clock)|
ReferenceId| 0x4C4F434C (source name: "LOCAL")|
Source| Local CMOS Clock|
Phase Offset| 0.0000000s|
Server Role| 576 (Reliable Time Service)|
```

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>第三方虛擬平台上的 Windows Server 2016

當 Windows 已虛擬化時，根據預設，Hypervisor 會負責提供時間。 但是加入網域的成員必須與網域控制站同步，才能讓 Active Directory 正常運作。 最好在所有第三方虛擬平台的客體與主機之間，停用所有時間虛擬化。

#### <a name="discovering-the-hierarchy"></a>探索階層

由於時間階層與主要時鐘來源之間的鏈結在網域中是動態且可交涉的，因此您必須查詢特定電腦的狀態，才能知道它是時間來源，並會與主要來源時鐘鏈結。 這有助於診斷時間同步的問題。

假設您想要針對特定用戶端進行疑難排解；第一個步驟是使用此 W32tm 命令來了解其時間來源。

```
w32tm /query /status
```

結果會顯示其他項目中的來源。 來源會指出您在網域中與之同步時間的目標。 這是此電腦時間階層的第一個步驟。
接下來，使用上述的來源項目以及 /StripChart 參數來尋找鏈結中的下一個時間來源。

```
w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1
```

此外，下列命令也會列出可以在指定網域中找到的每個網域控制站，並列印結果供您判斷每個夥伴。 此命令包含已手動設定的電腦。

 w32tm /monitor /domain:my_domain

使用此清單，您可以透過網域追蹤結果並了解階層，以及每個步驟的時間位移。 藉由找出時間位移明顯較差的時間點，您可以指出不正確時間的根。 接著，您可以開啟 w32tm 記錄，嘗試了解該時間不正確的原因。

#### <a name="using-group-policy"></a>使用群組原則
例如，您可以使用群組原則，將用戶端指派給使用特定 NTP 伺服器，或控制在虛擬化時如何設定下層 OS，藉以達到更嚴格的準確度。
以下是可能發生的案例清單，以及相關的群組原則設定：

**虛擬化網域** - 為了控制 Windows 2012R2 中的虛擬網域控制站並與網域同步時間，而不是與 Hyper-V 主機同步時間，您可以停用此登錄項目。  請不要停用 PDC 項目，因為 Hyper-V 主機將會提供最穩定的時間來源。 登錄項目會要求您必須在變更之後重新啟動 w32time 服務。

 [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider] "Enabled"=dword:00000000

**準確度敏感負載** - 針對時間準確度敏感的工作負載，您可以設定電腦群組來設定 NTP 伺服器和任何相關的時間設定，例如輪詢和時鐘更新頻率。 這通常是由網域來處理，但若要進行更多控制，您可以將特定電腦的目標設為直接指向主要時鐘。

群組原則設定| [新值]|
----- | ----- |
NtpServer| ClockMasterName,0x8|
MinPollInterval| 6 – 64 秒|
MaxPollInterval| 6|
UpdateInterval| 100 – 每秒一次|
EventLogFlags| 3 - 所有特殊時間記錄|

> [!NOTE]
> NtpServer 和 EventLogFlags 設定位於 System\Windows Time Service\Time Providers 下，使用 [設定 Windows NTP 用戶端] 設定。 其他 3 個則位於使用全域組態設定的 System\Windows Time Service 下。

**遠端準確度敏感性負載遠端** – 針對執行個體零售和支付信用產業 (PCI) 的分公司系統，Windows 會使用目前的網站資訊和 DC 定位器來尋找本機 DC，除非已設定手動 NTP 時間來源。 此環境需要 1 秒的準確度，其使用更快速的正確時間聚合。 此選項允許 W32time 服務以逆時鐘方向移動。 如果可接受且符合您的需求，可以建立下列原則。  在任何環境中，請務必測試您的網路並為其建立基準。

群組原則設定| [新值]|
----- | ----- |
MaxAllowedPhaseOffset| 1，如果超過 1 秒，請設定時鐘以更正時間。|

MaxAllowedPhaseOffset 設定位於使用全域組態設定的 System\Windows Time Service 下。

> [!NOTE]
> 如需群組原則和相關項目的詳細資訊，請參閱 [Windows Time 服務工具](windows-time-service-tools-and-settings.md)和 TechNet 上的設定文章。

## <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 的考量

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虛擬機器：Active Directory 網域服務
如果執行 Active Directory Domain Services 的 Azure VM 是現有內部部署 Active Directory 樹系的一部分，則應該停用 TimeSync(VMIC)。 這是為了讓樹系 (實體和虛擬) 中的所有 DC 都使用單一時間同步階層。 請參閱最佳做法白皮書[在 Hyper-V 中執行網域控制站](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd363553(v=ws.10))

### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虛擬機器：加入網域的電腦
如果您要裝載的電腦已加入現有的 Active Directory 樹系 (虛擬或實體) 網域，最佳做法是停用客體的 TimeSync，並確保 W32Time 已設定為透過設定時間為 Type=NTP5，與其網域控制站進行同步

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虛擬機器：獨立工作群組電腦
如果 Azure VM 未加入網域，也不是網域控制站，建議您保留預設時間組態，並讓 VM 與主機同步。

## <a name="windows-application-requiring-accurate-time"></a>需要準確時間的 Windows 應用程式
### <a name="time-stamp-api"></a>時間戳記 API
若程式需要最高準確度的 UTC 而非時間段落，應該使用 [GetSystemTimePreciseAsFileTime API](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getsystemtimepreciseasfiletime)。 這可確保您的應用程式取得系統時間，這是由 Windows Time 服務調整的。

### <a name="udp-performance"></a>UDP 效能
如果您的應用程式使用 UDP 通訊來處理交易，而且要將延遲降到最低，則有一些相關的登錄項目可用來設定要排除在基準篩選引擎外的連接埠範圍。 這會改善延遲並增加輸送量。 不過，應該請有經驗的系統管理員才能變更登錄。 此外，這項解決方法排除了不受防火牆保護的連接埠。 如需詳細資訊，請參閱下面的文章參考。

對於 Windows Server 2012 和 Windows Server 2008，您必須先安裝 Hotfix。 您可以參考這篇知識庫文章：[在 Windows 8 和 Windows Server 2012 中執行多點傳送接收者應用程式時遺失資料包](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>更新網路驅動程式
有些網路廠商會提供驅動程式更新，可以改善驅動程式延遲和緩衝處理 UDP 封包的效能。 請洽詢您的網路廠商，了解是否有可協助增加 UDP 輸送量的更新。

## <a name="logging-for-auditing-purposes"></a>供稽核之用的記錄
為了符合時間追蹤規定，您可以手動封存 w32tm 記錄、事件記錄和效能監視器資訊。 之後，封存的資訊可以用來證明過去特定時間的合規性。 下列因素可用來指出準確度。

1. 使用計算時間位移效能監視器計數器的時鐘準確度。 這會以所需的準確度顯示時鐘。
2. 在 w32tm 記錄中尋找「對等回應來源」的時鐘來源。  訊息文字之後會有 IP 位址或 VMIC，其描述要驗證之參考時鐘鏈結中的時間來源和下一個來源。
3. 使用 w32tm 記錄來驗證 “ClockDispl Discipline:\*SKEW\*TIME\*” 正在進行。 這表示目前正在使用 W32tm。

### <a name="event-logging"></a>事件記錄
若要取得完整的脈絡，您也需要事件記錄資訊。 藉由收集系統事件記錄，以及篩選 Time-Server、Microsoft-Microsoft-Windows-Kernel-Boot 和 Microsoft-Windows-Kernel-General，您可以探索是否有其他因素變更了時間 (例如，第三方)。 您需要這些記錄以排除外部干擾。 群組原則可能會影響寫入記錄的事件記錄檔。 如需詳細資訊，請參閱上述有關使用群組原則一節。

### <a name="w32time-debug-logging"></a><a name="W32Logging"></a>W32time 偵錯記錄
若要啟用 w32tm 以進行稽核，下列命令會啟用記錄以顯示時鐘的定期更新，並指出來源時鐘。 重新啟動服務，以啟用新的記錄。

如需詳細資訊，請參閱[如何開啟 Windows T時間服務中的偵錯記錄](https://support.microsoft.com/kb/816043)。

 w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>效能監視器
Windows Server 2016 Windows Time 服務會公開效能計數器，可用來收集用於稽核的記錄。 這些計數器可以在本機或遠端登入。 您可以記錄電腦時間位移和來回延遲計數器。
和所有效能計數器一樣，您可以從遠端監視，並使用 System Center Operations Manager 來建立警示。 例如，您可以使用警示，在時間位移從所需的準確度偏離時提醒您。 [System Center 管理組件](https://www.microsoft.com/download/details.aspx?id=44231)中包含更詳細的資訊。

### <a name="windows-traceability-example"></a>Windows 追蹤範例
建議您在 w32tm 記錄檔中驗證兩個資訊片段。 第一個指出記錄檔目前是條件時鐘。 這證明您的時鐘在發生爭議的時間是由 Windows Time 服務進行調整。

 151802 20:18:32.9821765s - ClockDispln Discipline:*SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307 151802 20:18:33.9898460s - ClockDispln Discipline:*SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41 151802 20:18:44.1090410s - ClockDispln Discipline:*SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

重點是，您會看到前面加上 ClockDispln Discipline 的訊息，也就是 w32time 與您的系統時鐘互動的證明。

接下來，您必須在發生爭議時間之前的記錄中尋找最後一份報告，內容說明目前正作為參考時鐘的來源電腦。 這可以是 IP 位址、電腦名稱或 VMIC 提供者，這表示系統與 Hyper-V 的主機進行同步。 下列範例會提供 10.197.216.105 的 IPv4 位址。

 151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

現在您已驗證參考時間鏈結中的第一個系統，接著需要調查參考時間來源上的記錄檔，並重複相同的步驟。 直到您取得實體時鐘前 (例如 GPS 或已知的時間來源，例如 NIST)，此作業會繼續進行。 如果參考時鐘是 GPS 硬體，則可能也需要來自製造商的記錄。

## <a name="network-considerations"></a>網路考量
NTP 通訊協定演算法與您的網路對稱有相依關係。 增加網路躍點的數目時，也會提高不對稱的機率。 在此情況下，很難預測您在特定環境中會看到的準確度類型。

Windows Server 2016 中的效能監視器和新的 Windows 時間計數器可用來評估環境準確度並建立基準。 此外，您可以執行疑難排解來判斷網路上任何電腦的目前位移。

有兩種通用標準可以透過網路取得準確的時間。 PTP ([準確時間通訊協定 - IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) 在網路基礎結構上有較緊密的需求，但通常可以提供子毫秒的準確度。 NTP ([網路時間通訊協定 - RFC 1305](https://tools.ietf.org/html/rfc1305)) 適用於更多不同的網路和環境，可讓您更輕鬆地進行管理。

依預設，Windows 會針對未加入網域的電腦支援簡單的 NTP (RFC2030)。 針對加入網域的機器，我們使用名為 [MS-SNTP](/openspecs/windows_protocols/ms-sntp/8106cb73-ab3a-4542-8bc8-784dd32031cc) 的安全 NTP，這會利用網域交涉的祕密，透過 RFC1305 和 RFC5905 中所述的驗證 NTP 提供管理優勢。

網域和未加入網域的通訊協定都需要 UDP 連接埠 123。 如需有關 NTP 最佳做法的詳細資訊，請參閱[網路時間通訊協定最新最佳作法 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠的硬體時鐘 (RTC)

除非超出特定界限，否則 Windows 不會將時間分段，而是會調整時鐘。 這表示 w32tm 會使用時鐘更新頻率設定 (預設為使用 Windows Server 2016 每秒一次)，定期調整時鐘的頻率。 如果時鐘落後，則會加速頻率；如果超前，則會將頻率調慢。 不過，在時鐘頻率調整期間，硬體時鐘就會掌握控制權。 如果軔體或硬體時鐘發生問題，電腦上的時間可能會變得較不準確。

這是您需要在環境中進行測試和設定基準的另一個原因。 如果「計算的時間位移」效能計數器並未針對您設為目標的準確度進行穩定化，建議您確認軔體是否為最新狀態。 另一種測試是，您可以看到重複的硬體是否會重現相同的問題。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>時間準確度和 NTP 疑難排解

您可以使用上述的「探索階層」一節，了解為何會產生不正確的時間。 查看時間位移，在階層中尋找時間從其 NTP 來源分歧最多的點。 一旦了解階層後，也會想要試著了解為什麼該特定時間來源不會收到準確的時間。

將焦點放在系統中分歧的時間上，您可以使用下列工具來收集詳細資訊，以協助您判斷問題並尋找解決方式。 下面的 UpstreamClockSource 參考是使用 “w32tm /config /status” 探索到的時鐘。

- 系統事件記錄
- 使用 w32tm logs - w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300 啟用記錄
- w32Time Registry key HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 區域網路追蹤
- 效能計數器 (從本機電腦或 UpstreamClockSource)
- W32tm /stripchart /computer:UpstreamClockSource
- 偵測 UpstreamClockSource 以了解來源的延遲和躍點數目
- Tracert UpstreamClockSource

問題| 徵兆| 解決方法|
----- | ----- | ----- |
| 本機 TSC 時鐘不穩定。| 使用 Perfmon - Physical Computer – Sync clock 穩定時鐘，但每 1-2 分鐘仍然會看到數個 100us。 | 更新軔體或驗證不同的硬體並不會顯示相同的問題。|
| 網路延遲| w32tm stripchart 顯示超過 10 毫秒的 RoundTripDelay。 延遲變化會導致相當於來回時間二分之一的雜訊；例如只往一個方向的延遲。<br><br>UpstreamClockSource 是多個躍點，可透過偵測顯示。 TTL 應接近 128。<br><br>使用 Tracert 來尋找每個躍點的延遲。  | 尋找時間較接近的時鐘來源。 其中一個解決方案是在相同的區段上安裝來源時鐘，或手動指向地理位置較近的來源時鐘。 若為網域案例，請新增具有 GTimeServ 角色的電腦。 |
無法可靠地觸及 NTP 來源| W32tm /stripchart 間歇性地傳回「要求逾時」 |NTP 來源沒有回應|
NTP 來源沒有回應| 檢查適用於 NTP 用戶端來源計數、NTP 伺服器連入要求、NTP 伺服器連出回應的 Perfmon 計數器，並決定您的使用方式 (與您的基準相比)。| 使用伺服器效能計數器，判斷基準參考中的負載是否已變更。<br><br>有網路擁塞問題嗎？|
網域控制站未使用最準確的時鐘| 拓撲中的變更或最近新增了主要時間時鐘。| w32tm /resync /rediscover|
用戶端時鐘偏離| 系統事件記錄中的 Time-Service 事件 36 和/或記錄檔中的文字描述：「NTP 用戶端時間來源計數」計數器從 1 變成 0|針對上游來源進行疑難排解，並了解是否遇到效能問題。|

### <a name="baselining-time"></a>基準時間

基準非常重要，可讓您先了解網路的效能和準確度，並與未來發生問題時的基準進行比較。 建議您將根 PDC 或任何標示為 GTIMESRV 的機器做為基準。 我們也建議您對每個樹系中的 PDC 設定基準。 最後，挑選具有有趣特性的關鍵 DC 或電腦，例如距離或高負載，以及針對這些項目設定基準。

這對設定 Windows Server 2016 與 2012 R2 的基準而言也很實用；不過您只能使用 w32tm/stripchart 當做進行比較的工具，因為 Windows Server 2012R2 沒有效能計數器。 您應該挑選兩部具有相同特性的電腦，或升級電腦並比較更新後的結果。 Windows 時間測量增補包含更多如何在 2016 與 2012 間進行詳細測量的資訊。

使用所有的 w32time 效能計數器，並至少收集一星期的資料。 這會確保您能隨著時間，在網路上的各種不同的情況下取得各項參考，並確信時間準確度十分穩定。

### <a name="ntp-server-redundancy"></a>NTP 伺服器冗餘

對於與未加入網域的電腦或 PDC 搭配使用的手動 NTP 伺服器組態，擁有一部以上的伺服器在可用性情況下是不錯的冗餘量值。 它也可能會提供更好的準確度，前提是所有來源都是準確且穩定的。 不過，如果拓撲未妥善設計，或時間來源不穩定，則所產生的準確度可能會變差，因此建議您務必謹慎處理。 支援時間伺服器 w32time 可以手動參考的支援限制為 10。

## <a name="leap-seconds"></a>閏秒數

氣候和地質事件會讓地球的自轉週期隨著時間而改變。 一般來說，每幾年會有一秒的變化。 每當原子時間的變化太大時，就會插入一秒 (向上或向下) 來更正，稱為「閏秒」。 如此一來，差異不會超過 0.9 秒。 這項更正會在實際發生的六個月前宣佈。 在 Windows Server 2016 之前，Microsoft 時間服務不會感知閏秒，只能依賴外部時間服務來處理這個問題。 隨著 Windows Server 2016 的時間準確度增加，Microsoft 正致力於打造更能妥善處理閏秒的解決方案。

## <a name="secure-time-seeding"></a>安全時間植入

Server 2016 中的 W32time 包含安全時間植入功能。 這項功能會判斷來自連出 SSL 連線的約略目前時間。 這個時間值用來監視本機系統時鐘，並更正所有的嚴重錯誤。 若要深入了解此功能，請參閱[此部落格文章](/archive/blogs/w32time/secure-time-seeding-improving-time-keeping-in-windows)。 在具有可靠時間來源的部署和受監視的電腦 (包含時間位移的監視) 中，您可以選擇不要使用安全時間植入功能，而改為依賴現有的基礎結構。

您可以使用下列步驟來停用此功能：

1. 在特定電腦上將 UtilizeSSLTimeData 登錄組態值設為 0：

    ```
    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f
    ```

2. 如果您因為某些原因而無法立即將電腦重新開機，可以通知 W32time 服務組態更新的相關資訊。 這會根據從 SSL 連線收集的時間資料，停止監視和強制執行時間。

    ```
    W32tm.exe /config /update
    ```

3. 將電腦重新開機會使設定立即生效，同時也會讓電腦停止從 SSL 連線收集任何時間資料。 後者會產生極小的負荷，應該不會有效能方面的顧慮。

4. 若要將這項設定套用到整個網域，請將 W32time 群組原則設定中的 UtilizeSSLTimeData 值設定為 0，並發佈設定。 當群組原則用戶端挑選此設定時，會通知 W32time 服務，並使用 SSL 時間資料停止監視和強制執行時間。 當每部電腦重新開機時，就會停止 SSL 時間資料收集作業。 如果您的網域中包含可攜式的超薄膝上型電腦/平板電腦和其他裝置，建議您排除這類機器以免受此原則變更影響。 這些裝置最終會面臨電池耗盡的情況，而且需要安全時間植入功能來啟動其時間。