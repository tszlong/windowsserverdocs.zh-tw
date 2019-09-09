---
title: Windows Server 2016 改良功能
description: Windows Server 2016 已改善用來更正時間和條件的演算法，以與 UTC 同步處理本機時鐘。
author: dcuomo
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 34d05a8058db366714c0ff4fed0b7d80b9150aa4
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2019
ms.locfileid: "69626301"
---
## <a name="windows-server-2016-improvements"></a>Windows Server 2016 改良功能

### <a name="windows-time-service-and-ntp"></a>Windows 時間服務和 NTP
Windows Server 2016 已改善用來更正時間和條件的演算法，以與 UTC 同步處理本機時鐘。  NTP 會根據用戶端要求/回應和伺服器要求/回應的時間戳記，使用4個值來計算時間位移。  不過，網路會產生雜訊，而且因為網路擁塞和其他影響網路延遲的因素，所以來自 NTP 的資料可能會尖峰。  Windows 2016 演算法會使用數種不同的技術來平均地排除這項雜訊，這會導致穩定且精確的時鐘。  此外，我們用來精確時間的來源會參考改良的 API，讓我們更能解決問題。  有了這些改良功能，我們就能夠在跨網域的 UTC 方面達到1毫秒的精確度。

### <a name="hyper-v"></a>Hyper-V
Windows 2016 已改善 Hyper-v TimeSync 服務。 改進功能包括更精確的 VM 啟動或 VM 還原初始時間，以及提供給 w32time 的範例中斷延遲修正。  這項改善可讓我們以 RMS （代表變異數）的10μs 主機，即使在具有 75% 負載的電腦上，也能保持在50μs 的情況下。 如需詳細資訊，請參閱[hyper-v 架構](https://msdn.microsoft.com/library/cc768520.aspx)。

> [!NOTE]
> 負載是使用 prime95 基準測試來建立，並使用平衡的設定檔。

此外，主控制項向訪客回報的階層層級更為透明。  之前，主機會顯示2的固定層次，而不論其正確性為何。  隨著 Windows Server 2016 中的變更，主機會報告一個大於主機層次的層次，這會導致虛擬來賓的時間更好。  主機層次是由 w32time 根據其來源時間以標準方式決定。  已加入網域的 Windows 2016 來賓會找出最精確的時鐘，而不是預設為主機。  基於這個理由，我們建議您針對參與 Windows 2012R2 和以下網域的機器，手動停用 Hyper-v 時間提供者設定。

### <a name="monitoring"></a>監視
已新增效能監視器計數器。  這些可讓您對時間精確度進行基準、監視和疑難排解。  這些計數器包括：

計數器|描述|
----- | ----- |
計算的時間位移|   W32Time 服務所計算的系統時鐘與所選時間來源之間的絕對時間位移（以微秒為單位）。 當有新的有效範例可供使用時，會以樣本所指示的時間位移來更新計算的時間。 這是本機時鐘的實際時間位移。 W32time 會使用此位移起始時鐘更正，並以需要套用至本機時鐘的剩餘時間位移，來更新樣本之間的計算時間。 您可以使用此效能計數器搭配低輪詢間隔（例如：256秒或更少）來追蹤時鐘精確度，並尋找計數器值是否小於所需的時鐘精確度限制。|
時鐘頻率調整| 每十億個部分中，W32Time 對本機系統時鐘所進行的絕對時鐘頻率調整。 此計數器有助於視覺化 W32time 所採取的動作。|
NTP 往返延遲|    NTP 用戶端從伺服器接收回應所經歷的最新來回延遲（以毫秒為單位）。 這是在傳送要求到 NTP 伺服器，並從伺服器接收有效回應之間，NTP 用戶端所經過的時間。 此計數器有助於描述 NTP 用戶端所遇到的延遲。 較大或不同的往返可能會增加 NTP 時間計算的雜訊，而這可能會影響透過 NTP 同步處理的時間準確度。|
NTP 用戶端來源計數|    NTP 用戶端使用的 NTP 時間來源總數。 這是回應此用戶端要求之時間伺服器的作用中、相異 IP 位址計數。 此數目可能大於或小於設定的對等，視對等名稱的 DNS 解析和目前的觸達能力而定。|
NTP 伺服器連入要求|   NTP 伺服器接收的要求數（每秒要求）。|
NTP 伺服器傳出回應|  NTP 伺服器回答的要求數（回應/秒）。|

前3個計數器的目標是針對精確度問題進行疑難排解的案例。  [下面的](#BestPractices)「疑難排解時間精確度」和「NTP」一節會有更多詳細資料。
最後3個計數器涵蓋 NTP 伺服器案例，在判斷負載並將目前的效能基準化時，會很有説明。

### <a name="configuration-updates-per-environment"></a>每個環境的設定更新
以下說明 Windows 2016 與先前版本的每個角色在預設設定中的變更。  Windows Server 2016 和 Windows 10 年度更新版（組建14393）的設定現在是唯一的，這就是為什麼會顯示為個別的資料行。 

|Role|設定|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**獨立/Nano 伺服器**||||
| |*時間伺服器*|time.windows.com|NA|time.windows.com|
| |*輪詢頻率*|64-1024 秒|NA|一週一次|
| |*時鐘更新頻率*|每秒一次|NA|一小時一次|
|**獨立用戶端**||||
| |*時間伺服器*|NA|time.windows.com|time.windows.com|
| |*輪詢頻率*|NA|一天 1 次|一週一次|
| |*時鐘更新頻率*|NA|一天 1 次|一週一次|
|**網域控制站**||||
| |*時間伺服器*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*輪詢頻率*|64-1024 秒|NA|1024-32768 秒|
| |*時鐘更新頻率*|一天 1 次|NA|一週一次|
|**網域成員伺服器**||||
| |*時間伺服器*|DC|NA|DC|
| |*輪詢頻率*|64-1024 秒|NA|1024-32768 秒|
| |*時鐘更新頻率*|每秒一次|NA|每5分鐘一次|
|**網域成員用戶端**||||
| |*時間伺服器*|NA|DC|DC|
| |*輪詢頻率*|NA|1204-32768 秒|1024-32768 秒|
| |*時鐘更新頻率*|NA|每5分鐘一次|每5分鐘一次|
|**Hyper-v 來賓**||||
| |*時間伺服器*|根據主機和時間伺服器的層次選擇最佳選項|根據主機和時間伺服器的層次選擇最佳選項|預設為主機|
| |*輪詢頻率*|根據上述角色|根據上述角色|根據上述角色|
| |*時鐘更新頻率*|根據上述角色|根據上述角色|根據上述角色|

>[!NOTE]
>針對 Hyper-v 中的 Linux，請參閱下面的[允許 linux 使用 Hyper-v 主機時間](#AllowingLinux)一節。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加輪詢和時鐘更新頻率的影響
為了提供更精確的時間，會增加輪詢頻率和時鐘更新的預設值，讓我們更頻繁地進行小型調整。  這會造成更多 UDP/NTP 流量，不過，這些封包很小，因此應該不會影響寬頻連結。 不過，它的優點是，在更廣泛的硬體和環境上，時間應該更好。

針對備有電池的裝置，增加輪詢頻率可能會造成問題。  電池裝置不會在關閉時儲存時間。  當他們繼續時，可能需要經常更正時鐘。  增加輪詢頻率會導致時鐘變得不穩定，而且也可以使用更多的電源。  Microsoft 建議您不要變更用戶端的預設設定。

網域控制站應該會受到最低限度的影響，即使是從 AD 網域中的 NTP 用戶端增加更新的效果也一樣。  與其他通訊協定相較之下，NTP 具有更小的資源耗用量，而且會產生極大的影響。  在 Windows Server 2016 的增加設定受到影響之前，您更有可能達到其他網域功能的限制。  Active Directory 不會使用安全 NTP，這通常會比簡單 NTP 的同步處理時間更短，但我們已驗證它會相應增加至遠離 PDC 的兩個用戶端。

就保守的計畫而言，每個核心應保留每秒 100 NTP 要求。  例如，由4個 Dc 組成的網域，每個都有4個核心，您應該能夠服務每秒 1600 NTP 要求。  如果您的用戶端設定為每64秒一次同步處理一次，而且要求會在一段時間內一致地收到，則您會看到 10000/64 或大約每秒160個要求，並分散到所有 Dc。  這會根據此範例，輕鬆地在 1600 NTP 要求/秒內。  這些都是保守的規劃建議，當然也會對您的網路、處理器速度和負載有很大的相依性，因此在您的環境中一律會進行基準和測試。

也請務必注意，如果您的 Dc 是以相當多的 CPU 負載執行，大於 40%，這幾乎肯定會將雜訊新增到 NTP 回應，並影響您網域中的時間精確度。  同樣地，您需要在您的環境中測試，以瞭解實際的結果。

## <a name="time-accuracy-measurements"></a>時間精確度測量
### <a name="methodology"></a>付諸實施
為了測量 Windows Server 2016 的時間準確度，我們使用了各種不同的工具、方法和環境。  您可以使用這些技術來測量和調整您的環境，並判斷精確度結果是否符合您的需求。 

我們的網域來源時鐘是由兩個具有 GPS 硬體的高精確度 NTP 伺服器所組成。  我們也使用個別的參照測試電腦進行測量，也就是從不同的製造商安裝高精確度的 GPS 硬體。  在進行某些測試時，除了您的網域時鐘來源以外，您還需要正確且可靠的時鐘來源做為參考。

我們使用四種不同的方法來測量實體和虛擬機器的精確度。 有多個方法提供獨立的方式來驗證結果。


1. 針對具有個別 GPS 硬體的參考測試電腦，測量由 w32tm 所保護的本機時鐘。  
2.  使用 W32tm "stripchart"，測量 ntp 伺服器對用戶端的 NTP ping
3.  使用 W32tm "stripchart"，將來自用戶端的 NTP ping 測量到 NTP 伺服器
4.  使用時間戳計數器（TSC）來測量從主機到來賓的 Hyper-v 結果。  這個計數器會在兩個數據分割的資料分割和系統時間之間共用。  我們計算了主機時間與虛擬機器中用戶端時間的差異。  然後，我們會使用 TSC 時鐘，從來賓插補主機時間，因為度量不會同時發生。  此外，我們會在 API 中使用 TSV 時鐘因數 out 延遲和延遲。

W32tm 是內建的，但我們在測試期間使用的其他工具，適用于 GitHub 上的 Microsoft 存放庫，做為您測試和使用的開放原始碼。  儲存機制上的 WIKI 有詳細資訊，說明如何使用這些工具來進行測量。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

下面顯示的測試結果，是我們在其中一個測試環境中所做的測量子集。  其中說明在時間階層開始時所維護的精確度，以及時間階層結尾的子網域用戶端。  這會與以2012為基礎的拓撲中的相同電腦比較。

### <a name="topology"></a>拓撲
為了進行比較，我們同時測試了 Windows Server 2012R2 和 Windows Server 2016 拓撲。  這兩種拓撲都是由兩部實體 Hyper-v 主機電腦所組成，這些機器會參照已安裝 GPS 時鐘硬體的 Windows Server 2016 電腦。  每部主機都會執行3個已加入網域的 windows 來賓，並根據下列拓撲進行排列。  這幾行代表時間階層，以及所使用的通訊協定/傳輸。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>圖形化結果總覽
下列兩個圖形根據上述拓撲，呈現定義域中兩個特定成員的時間精確度。  每個圖表都會顯示重迭的 Windows Server 2012R2 和2016結果，以視覺化方式呈現改良功能。  與主機相比，在來賓機器中，精確度是從和進行測量。  圖形化資料代表我們完成之整組測試的子集，並顯示最佳案例和最糟的案例。  

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根域 PDC 的效能
根 PDC 會與 Hyper-v 主機同步處理（使用 VMIC），這是一種 Windows Server 2016，其中已證明是精確且穩定的 GPS 硬體。  這是1毫秒精確度的關鍵需求，會顯示為綠色陰影區域。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子域用戶端的效能
子域用戶端會連接到與根 PDC 通訊的子域 PDC。  It 時間也會在1毫秒的需求內。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>長途電話測試
下圖比較1個虛擬網路躍點與 Windows Server 2016 的6個實體網路躍點。  兩個圖表會彼此重迭，以顯示重複的資料。  增加網路躍點表示較高的延遲，以及較長的時間偏差。  圖表會放大，因此1個 ms 界限（以綠色區域表示）會變大。  如您所見，時間仍在具有多個躍點的1毫秒內。  它會有負面的改變，這會示範網路上的不對稱。  當然，每個網路都不同，而測量則取決於許多環境因素。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>精確 timekeeping 的最佳做法
### <a name="solid-source-clock"></a>穩固的來源時鐘
機器時間只是與同步處理的來源時鐘一樣好。  為了達到1毫秒的精確度，您需要在您的網路上使用 GPS 硬體或時間設備，做為主要來源時鐘。  使用預設的 time.windows.com，可能無法提供穩定和當地時間來源。  此外，當您從來源時鐘進一步取得時，網路會影響精確度。  必須在每個資料中心擁有主要來源時鐘，才能獲得最佳準確度。

### <a name="hardware-gps-options"></a>硬體 GPS 選項
有各種硬體解決方案可提供精確的時間。  一般來說，現今的解決方案是以 GPS 天線為基礎。  此外，也有使用專用線路的無線電和撥號數據機解決方案。  它們會以設備的形式連接到您的網路，或插入電腦，例如透過 PCIe 或 USB 裝置的 Windows。  不同的選項會提供不同層級的精確度，而且一律會根據您的環境而定。  影響精確度的變數包含 GPS 可用性、網路穩定性和負載，以及 PC 硬體。  在選擇來源時鐘（如我們所述）時，這些都是很重要的因素，是穩定和精確時間的需求。

### <a name="domain-and-synchronizing-time"></a>網域和同步處理時間
網域成員會使用網域階層來決定要將哪一台機器當做來源來同步處理時間。  每個網域成員都會尋找要同步處理的另一部電腦，並將它儲存為時鐘來源。  每種類型的網域成員會遵循一組不同的規則，以便尋找時間同步處理的時鐘來源。  樹系根目錄中的 PDC 是所有網域的預設時鐘來源。  以下列出不同的角色，以及其如何尋找來源的高階描述：


- **具有 PDC 角色的網域控制站**–這部機器是網域的授權時間來源。 在網域中，它會有最精確的可用時間，而且必須與父系網域中的 DC 同步，除非啟用[GTIMESERV](#GTIMESERV)角色的情況除外。 
- **任何其他網域控制站**–這部電腦將作為網域中用戶端和成員伺服器的時間來源。 DC 可以與其本身網域的 PDC 或其父系網域中的任何 DC 同步。
- **用戶端/成員伺服器**–這部電腦可以與自己網域的任何 DC 或 pdc 或父系網域中的 DC 或 pdc 同步。

根據可用的候選項目，會使用評分系統來尋找最佳的時間來源。  此系統會考慮時間來源和其相對位置的可靠性。  當時間為服務啟動時，就會發生這種情況。  如果您需要更精確地控制時間同步化的方式，您可以在特定位置新增適當的時間伺服器，或新增冗余。  如需詳細資訊，請參閱[使用 GTIMESERV 指定本機可靠時間服務](#GTIMESERV)一節。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合 OS 環境（Win2012R2 和 Win2008R2）
雖然純粹的 Windows Server 2016 網域環境是最佳準確度的必要條件，但在混合環境中仍然有其優點。  在 Windows 2012 網域中部署 Windows Server 2016 Hyper-v 將會讓來賓受益，因為我們先前提到的改進，但前提是來賓也是 Windows Server 2016。  Windows Server 2016 PDC 將能夠提供更精確的時間，因為改良的演算法會是更穩定的來源。  取代您的 PDC 可能不是一個選項，您可以改為新增 Windows Server 2016 DC 與[GTIMESERV](#GTIMESERV)復原集，這會是您網域的精確度升級。  Windows Server 2016 DC 可以提供更好的時間給下游時間用戶端，不過，它的來源 NTP 時間也一樣好。

同樣地，如上所述，時鐘輪詢和重新整理頻率已修改為 Windows Server 2016。  這些可以手動變更為下層的 Dc，或透過群組原則套用。  雖然我們尚未測試這些設定，但它們在 Win2008R2 和 Win2012R2 方面的表現良好，並提供一些優點。

Windows Server 2016 之前的版本有多個問題保留正確的時間，這會導致系統時間在進行調整後立即離開。  因此，經常從正確的 NTP 來源取得時間樣本，並以資料來調節本機時鐘，會導致在取樣期間發生系統時鐘較小的漂移，進而縮短舊版的作業系統版本。 以高準確度設定設定的 Windows Server 2012R2 NTP 用戶端會從正確的 Windows 2016 NTP 伺服器同步處理時間時，最佳觀察到的精確度大約是5毫秒。

在涉及來賓網域控制站的某些情況下，Hyper-v TimeSync 範例可能會中斷網域時間同步處理。  對於在伺服器 2016 Hyper-v 主機上執行的伺服器2016來賓，這應該不再是問題。

若要停用 Hyper-v TimeSync 服務，使其無法提供範例給 w32time，請設定下列來賓登錄機碼：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>允許 Linux 使用 Hyper-v 主機時間
針對在 Hyper-v 中執行的 Linux 來賓，通常會將用戶端設定為使用 NTP 背景程式，以針對 NTP 伺服器進行時間同步處理。  如果 Linux 散發套件支援 TimeSync 第4版通訊協定，且 Linux 來賓已啟用 TimeSync 整合服務，則會針對主機時間進行同步處理。 如果這兩種方法都已啟用，這可能會導致不一致的時間保留。

若要以獨佔方式同步處理主機時間，建議您停用 NTP 時間同步處理，方法是：

- 停用 ntp 檔案中的任何 NTP 伺服器
- 或停用 NTP daemon

在此設定中，時間伺服器參數為 [此主機]。  其輪詢頻率為5秒，而時鐘更新頻率也是5秒。

若要以獨佔方式透過 NTP 進行同步處理，建議停用來賓中的 TimeSync 整合服務。

> [!NOTE]
> 注意：支援 Linux 來賓的精確時間需要最新的上游 Linux 核心才支援的功能，而且目前尚未在所有 Linux 散發版本上廣泛提供。 如需有關支援散發的詳細資訊，請參閱[Windows 上的 Hyper-v 支援的 Linux 和 FreeBSD 虛擬機器](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)。

#### <a name="GTIMESERV"></a>使用 GTIMESERV 指定本機可靠時間服務
您可以使用 GTIMESERV、良好的時間伺服器、旗標，將一或多個網域控制站指定為正確的來源時鐘。  比方說，配備 GPS 硬體的特定網域控制站可以標示為 GTIMESERV。  這可確保您的網域根據 GPS 硬體參考時鐘。

> [!NOTE]
> 如需有關網域旗標的詳細資訊，請參閱[ADTS 通訊協定檔](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一個相關的網域服務旗標，指出電腦目前是否為授權的，如果 DC 失去連線，這可能會變更。  此狀態的 DC 會在透過 NTP 查詢時傳回「未知的層次」。  嘗試多次之後，DC 會記錄系統事件時間-服務事件36。

如果您想要將 DC 設定為 GTIMESERV，可以使用下列命令手動設定。  在此情況下，DC 會使用另一部電腦做為主要時鐘。  這可能是設備或專用的機器。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 如需詳細資訊，請參閱[設定 Windows 時間服務](https://technet.microsoft.com/library/cc731191.aspx)

如果 DC 已安裝 GPS 硬體，您必須使用下列步驟來停用 NTP 用戶端並啟用 NTP 伺服器。

一開始請先停用 NTP 用戶端，然後使用這些登錄機碼變更來啟用 NTP 伺服器。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下來，重新開機 Windows Time 服務

    net stop w32time && net start w32time

最後，您會使用來表示這部電腦具有可靠的時間來源。
   
    w32tm /config /reliable:yes /update

若要檢查變更是否已正確完成，您可以執行下列命令，這會影響如下所示的結果。 

    w32tm /query /configuration

值|預期的設定|
----- | ----- |
AnnounceFlags|  5（本機）|
NtpServer   |本機|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL （本機）|
Enabled |1（本機）|
NtpClient|  本機|

    w32tm /query /status /verbose

值|  預期的設定|
----- | ----- |
上層|    1（主要參考-依無線電時鐘 syncd）|
ReferenceId|    0x4C4F434C （來源名稱："LOCAL"）|
Source| 本機 CMOS 時鐘|
階段位移|   0.0000000 s|
伺服器角色|    576（可靠時間服務）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>協力廠商虛擬平臺上的 Windows Server 2016
當 Windows 已虛擬化時，根據預設，管理者會負責提供時間。  但是加入網域的成員必須與網域控制站同步，才能讓 Active Directory 正常運作。  最好是在任何協力廠商虛擬平臺的來賓與主機之間，停用任何時間虛擬化。

#### <a name="discovering-the-hierarchy"></a>探索階層
由於時間階層與主要時鐘來源的鏈在網域中是動態的，因此，您必須查詢特定電腦的狀態，以瞭解它是時間來源，並會與主要來源時鐘連鎖。  這有助於診斷時間同步處理的問題。

假設您想要針對特定用戶端進行疑難排解;第一個步驟是使用此 w32tm 命令來瞭解其時間來源。

    w32tm /query /status

結果會顯示其他專案中的來源。  來源會指出您在網域中同步處理時間的目標。  這是此電腦時間階層的第一個步驟。
接下來，使用上述的來源專案，並使用/StripChart 參數來尋找鏈中的下一個時間來源。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

此外，下列命令也會列出它可以在指定的網域中找到的每個網域控制站，並列印可讓您判斷每個夥伴的結果。  此命令將包含已手動設定的電腦。
    
    w32tm /monitor /domain:my_domain

使用此清單，您可以透過定義域追蹤結果，並瞭解階層，以及每個步驟的時間位移。  藉由找出時差明顯較差的時間點，您可以找出不正確時間的根目錄。  您可以透過開啟[w32tm 記錄](#W32Logging)，嘗試瞭解該時間不正確的原因。 

#### <a name="using-group-policy"></a>使用群組原則
例如，您可以使用群組原則，將用戶端指派給使用特定 NTP 伺服器，或控制在虛擬化時，如何設定下層 OS 的，藉以達到更嚴格的精確度。  
以下是可能的案例清單，以及相關的群組原則設定：

**虛擬化網域**-為了控制 Windows 2012R2 中的虛擬網域控制站，讓它們與網域同步處理時間，而不是使用 hyper-v 主機，您可以停用此登錄專案。   針對 PDC，您不想要停用此專案，因為 Hyper-v 主機將會提供最穩定的時間來源。  登錄專案會要求您必須在變更之後重新開機 w32time 服務。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**精確度敏感性負載**-針對時間準確度敏感的工作負載，您可以設定機器群組來設定 NTP 伺服器和任何相關的時間設定，例如輪詢和時鐘更新頻率。  這通常是由網域來處理，但若要進行更多控制，您可以將特定電腦的目標設為直接指向主要時鐘。

群組原則設定|   [新值]|
----- | ----- |
NtpServer|  ClockMasterName，0x8|
MinPollInterval|    6–64秒|
MaxPollInterval|    6|
UpdateInterval| 100–每秒一次|
EventLogFlags|  3-所有特殊時間記錄|

> [!NOTE]
> [NtpServer] 和 [EventLogFlags] 設定位於 [System\Windows Time Service\Time Provider] 底下，使用 [設定 Windows NTP 用戶端設定]。  其他3則位於使用全域設定的 System\Windows Time 服務下。

遠端**精確度敏感載入遠端**–適用于實例零售和付款信用業（PCI）的分公司系統，Windows 會使用目前的網站資訊和 DC 定位器來尋找本機 DC，除非已設定手動 NTP 時間來源.  此環境需要1秒的精確度，其使用更快速的聚合至正確的時間。  此選項允許 w32time 服務向後移動時鐘。  如果這是可接受且符合您的需求，您可以建立下列原則。   如同任何環境，請務必測試您的網路並為其建立基準。 

群組原則設定|   [新值]|
----- | ----- |
MaxAllowedPhaseOffset|  1，如果超過秒，請設定 clock 以更正時間。|

MaxAllowedPhaseOffset 設定位於使用全域設定的 System\Windows Time 服務下。

> [!NOTE]
> 如需群組原則和相關專案的詳細資訊，請參閱 TechNet 上的[Windows 時間服務工具](windows-time-service-tools-and-settings.md)和設定一文。

## <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 考慮

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虛擬機器：Active Directory Domain Services
如果執行 Active Directory Domain Services 的 Azure VM 是現有內部部署 Active Directory 樹系的一部分，則應該停用 TimeSync （VMIC）。 這是為了讓樹系（實體和虛擬）中的所有 Dc 都使用單一時間同步階層。 請參閱最佳做法白皮書「[在 hyper-v 中執行網域控制站](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)」

### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虛擬機器：已加入網域的電腦
如果您要裝載的電腦已加入現有的 Active Directory 樹系（虛擬或實體），最佳做法是停用來賓的 TimeSync，並確保 W32Time 已設定為與其網域控制站進行同步處理，方法是設定時間類型 = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虛擬機器：獨立工作組機器
如果 Azure VM 未加入網域，也不是網域控制站，建議您保留預設時間設定，並讓 VM 與主機同步處理。

## <a name="windows-application-requiring-accurate-time"></a>需要精確時間的 Windows 應用程式
### <a name="time-stamp-api"></a>時間戳記 API
針對 UTC （而不是時間）需要最高精確度的程式，應該使用[GETSYSTEMTIMEPRECISEASFILETIME API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  這可確保您的應用程式取得系統時間，這是 Windows 時間服務的條件。

### <a name="udp-performance"></a>UDP 效能
如果您的應用程式使用 UDP 通訊來處理交易，而且要將延遲降到最低，則有一些相關的登錄專案可用來設定要排除在基底篩選引擎外的埠範圍。  這會改善延遲並增加您的輸送量。  不過，對登錄所做的變更應該限制為有經驗的系統管理員。  此外，這項解決方法是排除防火牆不受保護的埠。  如需詳細資訊，請參閱下面的文章參考。

對於 Windows Server 2012 和 Windows Server 2008，您必須先安裝一個修補程式。  您可以參考這篇知識庫文章：[當您在 Windows 8 和 Windows Server 2012 中執行多播接收者應用程式時，資料包遺失](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>更新網路驅動程式
有些網路廠商有驅動程式更新，可以改善驅動程式延遲和緩衝處理 UDP 封包的效能。  請洽詢您的網路廠商，以查看是否有可協助進行 UDP 輸送量的更新。

## <a name="logging-for-auditing-purposes"></a>記錄以供審核之用
為了符合時間追蹤規定，您可以手動封存 w32tm 記錄、事件記錄檔和效能監視器資訊。  之後，封存的資訊可以用來證明過去特定時間的合規性。  下列因素是用來指出精確度。


1. 使用 [計算時間位移] 效能監視器計數器的時鐘精確度。  這會以所需的精確度顯示時鐘。
2.  在 w32tm 記錄中尋找「對等回應」的時鐘來源。   郵件內文後面會有 IP 位址或 VMIC，其描述要驗證之參考時鐘鏈中的時間來源和下一個。
3.  使用 w32tm 記錄來驗證「ClockDispl 專業領域」的時鐘條件狀態：\*發生\*扭曲\*時間」。  這表示目前正在使用 w32tm。

### <a name="event-logging"></a>事件記錄
若要取得完整的案例，您也需要事件記錄檔資訊。  藉由收集系統事件記錄檔，以及篩選時間伺服器、Microsoft-Windows 核心開機、Microsoft Windows 核心-一般，您可以探索是否有其他影響已變更時間（例如，協力廠商）。  這些記錄可能需要排除外部干擾。  群組原則可能會影響寫入記錄檔的事件記錄檔。  如需詳細資訊，請參閱上面有關使用群組原則的一節。

### <a name="W32Logging"></a>W32time Debug 記錄
若要啟用 w32tm 以進行審核，下列命令會啟用記錄以顯示時鐘的定期更新，並指出來源時鐘。  重新開機服務，以啟用新的記錄。  

如需詳細資訊，請參閱[如何在 Windows Time 服務中開啟偵錯工具記錄](https://support.microsoft.com/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>效能監視器
Windows Server 2016 Windows 時間服務會公開效能計數器，可用來收集用於審核的記錄。  這些可以在本機或遠端登入。  您可以記錄「電腦時間位移」和「來回行程延遲」計數器。  
和任何效能計數器一樣，您可以從遠端監視它們，並使用 System Center Operations Manager 來建立警示。  例如，您可以使用警示，在時間位移從所需的精確度偏離時提醒您。  [System Center 管理元件](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)有詳細資訊。

### <a name="windows-traceability-example"></a>Windows 追蹤性範例
從 w32tm 記錄檔中，您會想要驗證兩個資訊片段。  第一個指示是記錄檔目前是條件時鐘。  這證明您的時鐘在爭議時間是由 Windows 時間服務所條件地進行。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

重點是，您會看到前面加上 ClockDispln 專業領域的訊息，也就是 w32time 與您的系統時鐘互動的證明。
 
接下來，您必須在爭議時間之前的記錄檔中尋找最後一份報告，其會報告目前正作為參考時鐘的來源電腦。  這可以是 IP 位址、電腦名稱稱或 VMIC 提供者，這表示它會與 Hyper-v 的主機進行同步處理。  下列範例會提供10.197.216.105 的 IPv4 位址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

現在您已驗證參考時間鏈中的第一個系統，您需要調查參考時間來源上的記錄檔，並重複相同的步驟。  這會繼續進行，直到您進入實體時鐘，例如 GPS 或已知的時間來源（例如 NIST）。  如果參考時鐘是 GPS 硬體，則可能也需要來自製造的記錄。

## <a name="network-considerations"></a>網路考慮
NTP 通訊協定演算法與您的網路的對稱有相依關係。  當您增加網路躍點的數目時，不對稱的機率也會增加。  在此情況下，很難以預測您會在特定環境中看到的精確度類型。 

Windows Server 2016 中的效能監視器和新的 Windows 時間計數器可用來評估您的環境精確度並建立基準。 此外，您可以執行疑難排解來判斷網路上任何電腦的目前位移。

透過網路進行精確的時間，有兩個一般標準。  PTP （[精確度時間通訊協定-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)）對網路基礎結構有更緊密的需求，但通常可以提供子圖元準確度。  NTP （[網路時間通訊協定– RFC 1305](https://tools.ietf.org/html/rfc1305)）適用于更多不同的網路和環境，可讓您更輕鬆地進行管理。 

Windows 預設針對未加入網域的電腦支援簡單的 NTP （RFC2030）。  針對加入網域的機器，我們使用名為[MS-SNTP](https://msdn.microsoft.com/library/cc246877.aspx)的安全 NTP，它會利用網域協商的秘密，這會透過 RFC1305 和 RFC5905 中所述的驗證 NTP 提供管理優勢。   

網域和未加入網域的通訊協定都需要 UDP 埠123。  如需 NTP 最佳做法的詳細資訊，請參閱[網路時間通訊協定最佳最新實務-IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠的硬體時鐘（RTC）
除非超出特定界限，否則 Windows 不會逐步執行時間，而是會以頻率為限。  這表示 w32tm 會使用 [時鐘更新頻率] 設定（預設為 Windows Server 2016 每秒一次），定期調整時鐘的頻率。  如果時鐘落後，它會加速頻率，如果過了，則會使頻率變慢。  不過，在這段時間內，時鐘頻率調整期間，硬體時鐘就會受到控制。  如果固件或硬體時鐘發生問題，電腦上的時間可能會變得較不精確。

這是您需要在環境中測試和基準的另一個原因。  如果「計算的時間位移」效能計數器未在您設為目標的精確度穩定，則您可能會想要確認您的固件為最新狀態。  另一種測試是，您可以看到重複的硬體重現相同的問題。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>疑難排解時間精確度和 NTP
您可以使用上述的「探索階層」一節來瞭解不正確時間的來源。  查看時間位移，尋找階層中時間從其 NTP 來源分歧最多的點。  一旦您瞭解階層之後，您會想要試著瞭解為什麼該特定時間來源不會收到精確的時間。  

將焦點放在系統上，您可以使用下列工具來收集詳細資訊，以協助您判斷問題並尋找解決方式。  下面的 UpstreamClockSource 參考，是使用 "w32tm/config/status" 探索到的時鐘。


- 系統事件記錄檔
- 使用下列內容啟用記錄： w32tm 記錄-w32tm/debug/enable/file： C:\Windows\Temp\w32time-test.log/size： 10000000/entries： 0-300
- w32Time 登錄機碼 HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本機網路追蹤
- 效能計數器（從本機電腦或 UpstreamClockSource）
- W32tm/stripchart/computer： UpstreamClockSource
- 偵測 UpstreamClockSource 以瞭解來源的延遲和躍點數目
- Tracert UpstreamClockSource

問題|    問題|   解析度|
----- | ----- | ----- |
本機 TSC 時鐘不穩定。| 使用 Perfmon-實體電腦–同步處理時鐘穩定時鐘，但您仍然會看到數個100us 的每1-2 分鐘。 |   更新固件或驗證不同的硬體並不會顯示相同的問題。|
網路延遲|    w32tm stripchart 顯示超過10毫秒的 RoundTripDelay。  延遲變化會導致來回時間的1/2，例如，只在一個方向的延遲。</br></br>UpstreamClockSource 是多個躍點，如 PING 所示。  TTL 應接近128。</br></br>使用 Tracert 來尋找每個躍點的延遲。    | 尋找時間較接近的時鐘來源。  其中一個解決方案是在相同的區段上安裝來源時鐘，或手動指向地理位置較近的來源時鐘。  若為網域案例，請新增具有 GTimeServ 角色的電腦。 |  
無法可靠地到達 NTP 來源|    W32tm/stripchart 間歇性地傳回「要求超時」    |NTP 來源沒有回應|
NTP 來源沒有回應|    檢查適用于 NTP 用戶端來源計數、NTP 伺服器連入要求、NTP 伺服器傳出回應的 Perfmon 計數器，並決定您的使用方式（與您的基準相比）。|    使用伺服器效能計數器，判斷您的基準參考中的負載是否已變更。</br></br>有網路擁塞問題嗎？|
網域控制站未使用最精確的時鐘|    拓撲中的變更或最近新增的主要時間時鐘。|   w32tm/resync/rediscover|
用戶端時鐘是離開| 系統事件記錄檔中的時間服務事件36和/或記錄檔中的文字描述：「NTP 用戶端時間來源計數」計數器從1到0|針對上游來源進行疑難排解，並瞭解其是否遇到效能問題。|

### <a name="baselining-time"></a>基準時間
基準非常重要，因此您可以先瞭解網路的效能和精確度，並與未來發生問題時的基準進行比較。  您會想要將根 PDC 或任何標示為 GTIMESRV 的機器做為基準。  我們也建議您對每個樹系中的 PDC 進行基準。  最後，挑選具有有趣特性的任何重要 Dc 或電腦，例如距離或高負載，以及基準那些。

對於 Windows Server 2016 與 2012 R2 的基準也很有用，不過您只有 w32tm/stripchart 是您可以用來比較的工具，因為 Windows Server 2012R2 沒有效能計數器。  您應該挑選兩部具有相同特性的機器，或升級機器並比較更新後的結果。  Windows 時間測量增補包含如何在2016與2012之間進行詳細測量的詳細資訊。

使用所有的 w32time 效能計數器，至少收集一周的資料。  這會確保您有足夠的時間可在網路上的各種不同的情況下，針對網路中的各項參考，並可讓您確信時間精確度穩定。

### <a name="ntp-server-redundancy"></a>NTP 伺服器冗余
對於與未加入網域的電腦或 PDC 搭配使用的手動 NTP 伺服器設定，擁有一部以上的伺服器在可用性情況下是不錯的冗余量值。  它也可能會提供更好的精確度，假設所有來源都是精確且穩定的。  不過，如果拓撲未妥善設計，或時間來源不穩定，則所產生的精確度可能會變差，因此建議您小心。  W32time 可以手動參考的支援時間伺服器的限制為10。 

## <a name="leap-seconds"></a>閏秒數
地球的旋轉期間會隨著時間而改變，原因是 climatic 和地質局事件。 一般來說，每幾年會有一秒的變化。 每當不可部分完成的時間變得太大時，就會插入一秒（向上或向下）更正，稱為「閏秒」。 這項作業的完成方式是讓差異不會超過0.9 秒。 這項更正會在實際更正的六個月前宣佈。 在 Windows Server 2016 之前，Microsoft 時間服務不知道閏秒，但依賴外部時間服務來處理這一點。 隨著 Windows Server 2016 的時間準確度增加，Microsoft 正致力於處理更適合第二個問題的解決方案。

## <a name="secure-time-seeding"></a>安全時間植入
伺服器2016中的 W32time 包含安全時間植入功能。 這項功能會判斷來自傳出 SSL 連線的大約目前時間。  這個時間值是用來監視本機系統時鐘，並更正所有的總錯誤。 您可以在[這篇 blog 文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)中閱讀更多關於此功能的資訊。  在具有可靠時間來源的部署和受監視的電腦（包含時間位移的監視）中，您可以選擇不要使用安全時間植入功能，而改為依賴現有的基礎結構。 

您可以使用下列步驟來停用此功能：

1.  在特定電腦上將 UtilizeSSLTimeData 登錄設定值設為0：

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config/v UtilizeSslTimeData/t REG_DWORD/d 0/f


2.  如果您因為某些原因而無法立即重新開機電腦，您可以通知 W32time 服務關於設定更新。 這會根據從 SSL 連線收集的時間資料，停止監視和強制執行時間。 

    W32tm/config/update

3.  重新開機電腦會使設定立即生效，同時也會使其停止從 SSL 連線收集任何時間資料。  後面的部分會有極小的負荷，而且不應該是效能方面的顧慮。

4.  若要將這項設定套用到整個網域，請將 W32time 群組原則設定中的 UtilizeSSLTimeData 值設定為0，併發佈設定。  當群組原則用戶端挑選此設定時，會通知 W32time 服務，並使用 SSL 時間資料停止監視和強制執行時間。 當每部機器重新開機時，SSL 時間資料收集將會停止。 如果您的網域具有可移植的超薄筆記本電腦/平板電腦和其他裝置，您可能會想要排除這類機器不受此原則變更。 這些裝置最終會面臨電池耗盡的情況，而且需要安全的時間植入功能來啟動其時間。


