

## <a name="windows-server-2016-improvements"></a>Windows Server 2016 的增強功能
### <a name="windows-time-service-and-ntp"></a>Windows 時間服務及 NTP
Windows Server 2016 已經改良，它會使用來更正時間和條件的本機時鐘同步處理與 UTC 的演算法。  NTP 使用 4 個值來計算時間位移，用戶端要求/回應 」 和 「 伺服器要求/回應的時間戳記為基礎。  不過，網路內容十分龐雜，而且可以將資料從 NTP 由於網路壅塞和其他因素會影響網路延遲尖峰。  Windows 2016 演算法平均數，這種情況使用多個不同的技術，這會產生穩定且精確的時鐘。  此外，來源我們會使用正確的時間參考會提供更好的解析度提升 API。  利用這些增強功能我們就能達到 1 毫秒精確度與 UTC 跨網域項目。

### <a name="hyper-v"></a>Hyper-V
Windows 2016 已改善對 HYPER-V TimeSync 服務。 增強功能包括更精確的初始時間在 VM 開始或 VM 還原，然後插斷延遲修正提供給 w32time 的範例。  這項改善可讓我們保持對 10µs （根平均平方，這表示變異數），rms，主機的 50µs，即使在 75%負載的機器上。 如需詳細資訊，請參閱 < [HYPER-V 架構](https://msdn.microsoft.com/library/cc768520.aspx)。

> [!NOTE]
> 負載已建立使用 prime95 基準測試使用平衡的設定檔。

此外，主機會向客體 stratum 層級是更透明的。  先前的主機會呈現為 2，不論其精確度的固定的 stratum。  使用 Windows Server 2016 中的變更，主應用程式會報告 stratum 一必須大於主機 stratum，導致虛擬客體的更佳的時間。  主機 stratum 取決於 w32time 經由一般方法，根據其來源的時間。  加入網域的 Windows 2016 來賓會尋找最精確的時鐘，而不是預設為主機。  它是基於這個原因，我們建議您以手動方式停用 HYPER-V 時間提供者設定為加入網域 Windows 2012R2 或其下的機器。

### <a name="monitoring"></a>監視
已新增效能監視器計數器。  這些基準，可讓您監視和疑難排解的時間精確度。  這些計數器包含：

計數器|描述|
----- | ----- |
計算時間位移|   絕對的時間位移系統時鐘之間的所選的時間來源，如 W32Time 服務以微秒為單位計算。 使用新的有效範例時，此範例所指定的時間位移會更新計算的時間。 這是本機電腦時鐘的實際時間位移。 W32time 起始使用此位移的時鐘更正，並更新必須套用至本機電腦時鐘的剩餘時間位移範例之間的計算的時間。 可以使用較低的輪詢間隔的這個效能計數器追蹤時鐘精確度 (例如： 256 的秒數小於或等於)，並尋找小於所需的時鐘精確度限制的計數器值。|
時脈頻率調整| 絕對的時脈頻率調整 W32Time billion 每個組件中所做的本機系統時鐘。 此計數器可協助視覺化 W32time 所要採取的動作。|
NTP Roundtrip Delay|    經歷 NTP 中的用戶端從伺服器接收回應，以微秒為單位的最新的來回延遲。 這是次經過 NTP 用戶端之間傳輸的 NTP 伺服器的要求和接收來自伺服器的有效回應。 此計數器可協助描繪 NTP 用戶端所經歷的延遲。 較大或變動的往返作業可以加入雜訊 NTP 時間計算，接著可能會影響透過 NTP 時間同步處理的精確度。|
NTP 用戶端來源計數|    作用中數目 NTP 用戶端所使用的 NTP 時間來源的詳細資訊。 這是此用戶端要求沒有回應的時間伺服器作用中的不同 IP 位址的計數。 這個數目可能大於或小於設定的對等，視 DNS 解析的對等名稱，以及目前的觸達能力而定。|
NTP 伺服器連入要求|   NTP 伺服器 （要求/秒） 收到的要求數目。|
NTP 伺服器連出回應|  NTP 伺服器 （回應/秒） 來回應的要求數目。|

前 3 個計數器以精確度問題進行疑難排解的案例為目標。  疑難排解的時間精確度和 NTP 區段下方，底下[最佳做法](#BestPractices)，有更多詳細資料。
前 3 個計數器涵蓋 NTP 伺服器案例，並會很有幫助時，判斷設定基準的負載目前的效能。

### <a name="configuration-updates-per-environment"></a>每個環境的組態更新
以下說明在 Windows 2016 和舊版之間的預設組態，每個角色中的變更。  設定 Windows Server 2016 和 Windows 10 年度更新版 （組建 14393），現在會唯一這也是為什麼有會顯示為個別的資料行。 

|[角色]|設定|Windows Server 2016|Windows 10|Windows Server 2012 R2</br>Windows Server 2008 R2</br>Windows 10|
|---|---|---|---|---|
|**獨立/Nano 伺服器**||||
| |*Time Server*|time.windows.com|NA|time.windows.com|
| |*輪詢頻率*|64-1024 的秒數|NA|一週一次|
| |*時鐘更新頻率*|在每秒|NA|每小時一次|
|**獨立用戶端**||||
| |*Time Server*|NA|time.windows.com|time.windows.com|
| |*輪詢頻率*|NA|一天一次|一週一次|
| |*時鐘更新頻率*|NA|一天一次|一週一次|
|**網域控制站**||||
| |*Time Server*|PDC/GTIMESERV|NA|PDC/GTIMESERV|
| |*輪詢頻率*|64-1024 秒|NA|1024-32768 秒|
| |*時鐘更新頻率*|一天一次|NA|一週一次|
|**網域成員伺服器**||||
| |*Time Server*|DC|NA|DC|
| |*輪詢頻率*|64-1024 秒|NA|1024-32768 秒|
| |*時鐘更新頻率*|在每秒|NA|每 5 分鐘|
|**網域成員用戶端**||||
| |*Time Server*|NA|DC|DC|
| |*輪詢頻率*|NA|1204-32768 秒|1024-32768 秒|
| |*時鐘更新頻率*|NA|每 5 分鐘|每 5 分鐘|
|**HYPER-V 客體**||||
| |*Time Server*|選擇最佳的選項根據 stratum 的主機和時間的伺服器|選擇最佳的選項根據 stratum 的主機和時間的伺服器|至主機的預設值|
| |*輪詢頻率*|根據上述的角色|根據上述的角色|根據上述的角色|
| |*時鐘更新頻率*|根據上述的角色|根據上述的角色|根據上述的角色|

>[!NOTE]
>針對 Linux 在 HYPER-V 中，請參閱[可讓 Linux 使用 HYPER-V 主機時間](#AllowingLinux)下一節。

### <a name="impact-of-increased-polling-and-clock-update-frequency"></a>增加的輪詢和時鐘更新頻率的影響
為了提供更準確的時間，輪詢頻率和時鐘更新的預設值會增加，讓我們更頻繁地進行小幅度的調整。  這會導致更多的 UDP/NTP 流量; 不過，這些封包小型因此應該很少或不會影響透過寬頻的連結。 好處，不過，是時間應該更廣的硬體和環境上。

適用於備份電池裝置，增加輪詢頻率可能會造成問題。  電池裝置不會儲存已關閉時的時間。  當他們繼續時，它可能需要經常更正的時鐘。  增加輪詢頻率會變得不穩定的時鐘，也可以使用更強大的功能。  Microsoft 建議您不要變更的用戶端的預設設定。

網域控制站應該最少受到影響，即使使用 NTP 的 AD 網域中的用戶端的增加更新乘效果。  NTP 有相較於其他通訊協定和臨界影響的許多較小的資源耗用量。  您會更容易受到增加的設定，適用於 Windows Server 2016 之前達到限制的其他網域的功能。  Active Directory 會使用安全的 NTP，通常會比簡單的 NTP 較不精確地同步處理時間，但我們已經確認它會相應增加用戶端兩者 stratum 遠離 PDC。

保守的計畫，您應該保留每個核心每秒 100 個 NTP 要求。  比方說，網域中具有 4 個核心的 4 個 Dc 所組成，您應該能夠處理每秒 1600 NTP 要求。  如果您有 10 個 k 用戶端設定為同步處理時間，之後每隔 64 秒，而且經過一段時間一致的方式接收要求，您會看到 10000/64 或大約 160 要求/秒，分散到所有網域控制站。  輕鬆地落在我們 1600 NTP 要求數/秒根據此範例中。  這些是保守規劃建議，而且當然會有大型的相依性，在您的網路、 處理器速度與載入，因此如往常基準和測試您的環境中。

務必也要注意，是否您的 Dc 正在執行具有相當大的 CPU 負載，大於 40%，這將會幾乎肯定雜訊 NTP 回應加上會影響您在網域中的時間精確度。  同樣地，您要測試您的環境，以了解實際的結果中。

## <a name="time-accuracy-measurements"></a>時間精確度的度量單位
### <a name="methodology"></a>方法
若要測量的時間精確度，適用於 Windows Server 2016，我們會使用各種不同的工具、 方法和環境。  您可以使用這些技巧來測量地調整您的環境，並判斷是否傳回精確度結果符合您的需求。 

我們網域來源時鐘是由兩個有效位數很高 NTP 伺服器與 GPS 硬體所組成。  我們也會使用個別的參考測試機器的度量，還必須從不同的製造商所安裝的有效位數很高 GPS 硬體。  針對某些測試，您必須做為參考，除了您網域的時鐘來源使用的精確又可靠的時脈來源。

我們可以用四種不同方法來測量精確度，但實體和虛擬機器。 多個方法會提供獨立的方式，來驗證結果。


1. 針對我們參照測試機器具有個別的 GPS 硬體中測量的本機時鐘，w32tm 的條件式。  
2.  從 NTP 伺服器 」 使用 W32tm"stripchart"的用戶端中的量值 NTP ping
3.  量值 NTP ping 從用戶端使用 W32tm"stripchart 」 的 NTP 伺服器
4.  量值的 HYPER-V 會產生從主機到客體使用時間戳記計數器 (TSC)。  此計數器是磁碟分割與系統時間之間共用，在兩個資料分割中。  我們計算虛擬機器的主機和用戶端時間的差異。  然後我們會使用 TSC 時鐘來插入客體，主應用程式時間，因為度量不會發生在相同的時間。  此外，我們會使用延遲和延遲 TSV 時鐘分解 API 中。

W32tm 是內建功能，但我們在我們的測試期間使用的其他工具可供 Microsoft 存放庫在 GitHub 上為您的測試和使用量的開放原始碼。  存放庫上的 WIKI 有說明如何使用工具來執行度量的詳細資訊。

> [https://github.com/Microsoft/Windows-Time-Calibration-Tools](https://github.com/Microsoft/Windows-Time-Calibration-Tools)

如下所示的測試結果都是我們在其中一個測試環境中所做的度量單位的子集。  這些逐步解說說明維護的時間階層和子網域的用戶端的時間階層結尾開頭的精確度。  這是相較於 2012年拓樸中在同一部電腦進行比較。

### <a name="topology"></a>拓撲
進行比較，我們測試的 Windows Server 2012 r2 和 Windows Server 2016 型拓樸。  這兩種拓撲是由參考與 GPS 時鐘硬體安裝的 Windows Server 2016 電腦的兩個實體 HYPER-V 主機電腦所組成。  每一部主機會執行 3 個已加入網域 windows 客體，這會根據下列拓撲排列。  線條則代表的時間階層，與使用通訊協定/傳輸。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology1.png)

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology2.png)

### <a name="graphical-results-overview"></a>圖形化結果概觀
下列兩個圖形代表的時間精確度針對上述拓撲為基礎的網域中的兩個特定成員。  每個圖形顯示 Windows Server 2012 r2 和 2016年結果重疊，以視覺化方式示範改善項目。  相較於主應用程式在客體機器時，已從內測量精確度。  圖形化的資料代表整個我們所做的測試集合的子集，並顯示最好的情況下，嚴重的案例狀況。  

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/topology3.png)

### <a name="performance-of-the-root-domain-pdc"></a>根網域的 PDC 的效能
根 PDC 會同步處理至 HYPER-V 主機 （使用 VMIC） 也就是使用經證明是精確且穩定的 GPS 硬體的 Windows Server 2016。  這是 1 毫秒精確度，會顯示為綠色陰影區域是關鍵需求。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart1.png)

### <a name="performance-of-the-child-domain-client"></a>子網域用戶端效能
子網域用戶端會附加至子網域的 PDC 到根 PDC 通訊。  時間也位於 1 ms 需求。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart2.png)


### <a name="long-distance-test"></a>長途電話測試
下圖比較 Windows Server 2016 的 6 個實體網路躍點 1 的虛擬網路躍點。  兩個圖表會彼此重疊，透明度，以顯示重疊的資料。  增加的網路躍點表示較高的延遲和較大時間偏差。  圖表是放大顯示，所以 1 ms 範圍中，由 [綠色] 區域中，是較大。  如您所見，時間是 1 內仍與多個躍點的毫秒。  它會產生負面已改變，這證明了網路上的不對稱。  當然，每個網路都不同，而度量取決於許多環境因素。

![Windows Time](../media/Windows-Time-Service/Windows-2016-Accurate-Time/chart3.png)

## <a name="BestPractices"></a>精確 timekeeping 的最佳作法
### <a name="solid-source-clock"></a>Solid 來源時鐘
機器時間只會顯示為來源時鐘，它會與同步。  為了達到 1 毫秒的精確度，您將需要 GPS 硬體或時間設備您參考的主要來源時鐘在網路上。  使用預設值 time.windows.com，可能無法提供穩定且本地時間來源。  此外，當您遠離來源時鐘，網路會影響精確度。  具有主要來源時鐘在每個資料中心是為了最佳的精確度。

### <a name="hardware-gps-options"></a>硬體的 GPS 選項
有各種不同的硬體解決方案可以提供準確的時間。  一般情況下，解決方案現在會根據 GPS 天線。  也有選項和撥號數據機使用的專用的線路的解決方案。  它們會附加至您為的網路應用裝置，或插入電腦時，例如透過 PCIe 或 USB 裝置的 Windows。  不同的選項會提供不同層級的精確度，並如往常，結果會取決於您的環境。  這會影響精確度的變數包括 GPS 可用性、 網路穩定性和負載，以及電腦硬體。  選擇來源時鐘，如我們所述，是穩定且精確的時間的需求時，這些是所有重要的因素。

### <a name="domain-and-synchronizing-time"></a>網域和同步處理時間
網域成員使用網域階層來判斷哪一部機器它們用以做為來源同步處理時間。  每個網域的成員會找到另一部電腦做為它的時鐘來源同步並儲存它。  網域成員的每個型別會遵循一組不同的規則，以便找出時間同步處理時鐘來源。  在樹系根 PDC 是所有網域預設時脈來源。  以下列出不同的角色和概略說明它們如何尋找來源：


- **網域控制站與 PDC 角色**– 這台電腦是網域的授權時間來源。 它會在網域中，有最準確的時間，而必須與父系網域中的 DC 除了在中同步處理的情況下所在[GTIMESERV](#GTIMESERV)啟用角色。 
- **任何其他網域控制站**– 這部電腦將做為時間來源，用戶端和網域中的成員伺服器。 DC 可以使用它自己網域的 PDC 或與其父系網域中的任何 DC 同步處理。
- **成員用戶端/伺服器**– 這部電腦可以與任何 DC 或它自己的網域，或 DC 的 PDC 或在父系網域的 PDC 同步。

根據可用的候選項目，評分系統用來找出最佳的時間來源。  此系統會考量的時間來源和其相對位置的可靠性。  會發生這種情況是一旦所啟動的服務時的時間。  如果您需要能夠更精細控制如何同步處理時間，您可以在特定位置中新增適合的時間伺服器，或新增備援。  請參閱[指定本機可靠時間服務使用 GTIMESERV](#GTIMESERV)節的詳細資訊。

#### <a name="mixed-os-environments-win2012r2-and-win2008r2"></a>混合的作業系統環境 （Win2012R2 和 Win2008R2）
為了最佳精確度的純虛擬的 Windows Server 2016 網域環境時，仍有優點的混合式環境中。  部署 Windows Server 2016 HYPER-V 在 Windows 2012 網域中，將會因為我們先前所述，但只有如果來賓也是 Windows Server 2016 的改進獲益的來賓。  Windows Server 2016 PDC，可以傳遞更準確的時間，因為有改良過的演算法，它將會更穩定的來源。  做為取代您 PDC 可能不是選項，您可以改為加入與 Windows Server 2016 DC [GTIMESERV](#GTIMESERV)向前復原集是在精確度為您的網域的升級。  Windows Server 2016 DC 可以傳遞給下游的時間用戶端的最佳時機，不過，它只會顯示為其來源 NTP 時間。

如上所述，也與 Windows Server 2016 已經過修改時鐘輪詢和重新整理頻率。  這些可以變更為您下層網域控制站的手動或透過群組原則套用。  雖然我們尚未測試這些設定，它們應該在 Win2008R2 和 Win2012R2 中正常運作，並提供一些優點。

Windows Server 2016 必須保持準確的時間保持因而導致系統時間離開，緊接著進行調整的多個問題之前的版本。  因為這個緣故，經常從正確的 NTP 來源取得時間範例和條件的本機時鐘的資料導致較小的漂移，在其系統時鐘的內部取樣期間，導致更好的時間將維持在舊版作業系統版本上。 Windows Server 2012R2 NTP 設定的用戶端，以高精確度設定，同步處理其時間從正確的 Windows 2016 NTP 伺服器時，觀察到的最佳精確度會為大約 5 毫秒。

在某些情況下，牽涉到客體的網域控制站，HYPER-V TimeSync 範例可能會干擾網域的時間同步處理。  這應該不會再 Server 2016 HYPER-V 主機上執行的 Server 2016 客體的問題。

若要停用 HYPER-V TimeSync 服務無法提供 w32time 的範例，請設定下列客體登錄機碼：

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider 
    "Enabled"=dword:00000000

#### <a name="AllowingLinux"></a>允許使用 HYPER-V 主機時間的 Linux
在 HYPER-V 中執行的 Linux 客體，通常是用於針對 NTP 伺服器的時間同步處理的 NTP 精靈設定用戶端。  如果 Linux 散發套件支援 TimeSync 第 4 版通訊協定，而且 Linux 客體已啟用 TimeSync 整合服務，它會同步處理與主應用程式的時間。 這可能會導致不一致的時間保留，如果您啟用這兩種方法。

若要以獨佔方式與主機時間同步處理，建議停用 NTP 時間同步處理透過下列方式：

- 停用在 ntp.conf 檔案中任何 NTP 伺服器
- 或停用的 NTP 服務精靈

在此組態中，時間伺服器參數會是此主機。  其輪詢頻率為 5 秒和時鐘更新頻率也是 5 秒。

若要以獨佔方式透過 NTP 同步處理，建議停用客體中的 TimeSync 整合服務。

> [!NOTE]
> 注意：支援 Linux 來賓的精確時間要求的功能，只有在最新上游 Linux 核心中，而且它不是可廣泛提供所有 Linux 散發版本尚未。 請參閱[支援的 Linux 和 FreeBSD 虛擬機器，在 Windows 上的 hyper-v](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows)如需詳細資訊支援的散發套件。

#### <a name="GTIMESERV"></a>指定使用 GTIMESERV 本機可靠的時間服務
您可以指定一或多個網域控制站，以正確的來源時鐘使用 GTIMESERV，良好的時間伺服器，加上旗標。  比方說，配備 GPS 硬體的特定網域控制站可標示為 GTIMESERV。  這會確保您的網域參考根據 GPS 硬體時鐘。

> [!NOTE]
> 網域旗標的詳細資訊可在[MS ADTS 通訊協定文件](https://msdn.microsoft.com/library/mt226583.aspx)。

TIMESERV 是另一個相關的網域服務旗標表示是否是目前授權的機器，它可以變更，如果 DC 失去連線。  此狀態中的 DC 會傳回 「 未知 Stratum 」 時透過 NTP 查詢。  之後嘗試多次，DC 會記錄系統事件的時間服務事件 36。

如果您想要將 DC 設定為 GTIMESERV，這可以使用下列命令，以手動方式來設定。  在此情況下 DC 也正在使用另一部電腦，作為主要的時鐘。  這可能是專用的電腦的設備。

    w32tm /config /manualpeerlist:”master_clock1,0x8 master_clock2,0x8” /syncfromflags:manual /reliable:yes /update

> [!NOTE]
> 如需詳細資訊，請參閱[設定 Windows 時間服務](https://technet.microsoft.com/library/cc731191.aspx)

如果 DC 已安裝的 GPS 硬體，您需要使用下列步驟來停用 NTP 用戶端，並啟用 NTP 伺服器。

開始停用 NTP 用戶端，並啟用 使用這些登錄機碼變更的 NTP 伺服器。

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpClient /v Enabled /t REG_DWORD /d 0 /f

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\TimeProviders\NtpServer /v Enabled /t REG_DWORD /d 1 /f

接下來，重新啟動 Windows Time 服務

    net stop w32time && net start w32time

最後，您會指定這台電腦擁有的可靠時間來源，使用。
   
    w32tm /config /reliable:yes /update

若要檢查已正確完成變更，您可以執行下列命令會影響結果如下所示。 

    w32tm /query /configuration

值|預期的設定|
----- | ----- |
AnnounceFlags|  5 （本機）|
NtpServer   |（本機）|
DllName |C:\WINDOWS\SYSTEM32\w32time.DLL (Local)|
Enabled |1 （本機）|
NtpClient|  （本機）|

    w32tm /query /status /verbose

值|  預期的設定|
----- | ----- |
Stratum|    1 （主要參考-syncd 廣播時間）|
ReferenceId|    0x4C4F434C (來源名稱：「 區域 」）|
Source| 本機 CMOS 時鐘|
階段位移|   0.0000000s|
伺服器角色|    576 （可靠的時間服務）|

#### <a name="windows-server-2016-on-3rd-party-virtual-platforms"></a>在第 3 方虛擬平台上的 Windows Server 2016
虛擬化 Windows 時，預設 Hypervisor 時負責提供時間。  但已加入網域的成員必須是為了讓正常運作的 Active Directory 的網域控制站的同步處理。  最好的方式是停用客體和主機的任何第 3 方虛擬平台之間的任何時間虛擬化。

#### <a name="discovering-the-hierarchy"></a>探索階層
因為主要時鐘來源的時間階層鏈結會在網域中，動態交涉，您必須查詢特定的電腦，以了解它的時間來源，鏈結的主要來源時鐘的狀態。  這可協助診斷時間同步處理問題。

指定您要疑難排解特定的用戶端;第一個步驟是透過此 w32tm 命令來了解它的時間來源。

    w32tm /query /status

結果會顯示來源以及其他項目。  來源會指出與您的時間同步在網域中。  這是此機器時間階層的第一個步驟。
接下來使用上述的來源項目，並使用 /StripChart 參數來尋找下一個時間來源，鏈結中。

    w32tm /stripchart /computer:MySourceEntry /packetinfo /samples:1

也很有用，下列命令會列出指定的網域中可找到每個網域控制站，並會列印結果可讓您判斷每個夥伴。  此命令會包含手動設定的機器。
    
    w32tm /monitor /domain:my_domain

使用清單時，您就可以追蹤透過網域結果，並了解階層，以及在每個步驟的時間位移。  藉由找出其中的時間位移取得明顯較差的點，您可以找出不正確的時間的根目錄。  從該處，您可以嘗試了解為何該時間不正確開啟[w32tm 記錄](#W32Logging)。 

#### <a name="using-group-policy"></a>使用群組原則
您可以使用群組原則來完成更嚴格的精確度，比方說，使用特定的 NTP 伺服器，或設定控制項如何舊版 OS 的虛擬化時，將用戶端的指派。  
以下是可能的案例以及相關的群組原則設定的清單：

**虛擬化網域**-為了控制在 Windows 2012 r2 中的虛擬網域控制站，以便他們使用其網域時，同步處理時間，而不是與 HYPER-V 主機，您可以停用此登錄項目。   Pdc，您不想要停用該項目，因為 HYPER-V 主機將會提供最穩定的時間來源。  登錄項目需要變更後，重新啟動 w32time 服務。

    [HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\VMICTimeProvider]
    "Enabled"=dword:00000000

**精確度機密載入**-時間精確度機密工作負載，您可以設定的設定 NTP 伺服器的機器群組和任何相關時間設定，例如輪詢和時鐘更新頻率。  這通常由網域，但更多的控制中，您可以鎖定特定機器直接指向主要的時鐘。

群組原則設定|   [新值]|
----- | ----- |
NtpServer|  ClockMasterName 0x8|
MinPollInterval|    6 – 64 的秒數|
MaxPollInterval|    6|
UpdateInterval| 100 – 每秒一次|
EventLogFlags|  3 – 所有的特殊的時間登入|

> [!NOTE]
> NtpServer 和 EventLogFlags 設定位於 System\Windows 時間 Service\Time 提供者使用的設定 Windows NTP 用戶端設定。  其他 3 位於 System\Windows 時間服務使用的全域組態設定。

**遠端精確度機密載入遠端**– 在執行個體零售和付款信用卡產業 (PCI) 的分支網域中的系統，Windows 會使用目前的站台資訊並 DC 定位程式以尋找本機 DC，除非發生手動 NTP 時間來源設定。  這種環境需要 1 秒的精確度，會使用更快速的聚合成正確的時間。  此選項可讓 w32time 服務以向後移動時鐘。  如果這是可以接受的而且符合您的需求，您可以建立下列原則。   如同任何環境，可確保測試和基準您的網路。 

群組原則設定|   [新值]|
----- | ----- |
MaxAllowedPhaseOffset|  1，如果多個在第二個設定時鐘為正確的時間。|

MaxAllowedPhaseOffset 設定位於 System\Windows 時間服務使用的全域組態設定。

> [!NOTE]
> 如需有關群組原則和相關項目的詳細資訊，請參閱[Windows 時間服務工具](windows-time-service-tools-and-settings.md)和 TechNet 上的設定發行項。

## <a name="azure-and-windows-iaas-considerations"></a>Azure 和 Windows IaaS 的考量

### <a name="azure-virtual-machine-active-directory-domain-services"></a>Azure 虛擬機器：Active Directory Domain Services
如果執行 Active Directory 網域服務的 Azure VM 屬於現有內部部署 Active Directory 樹系，然後 TimeSync(VMIC)，應該停用。 這是為了允許所有網域控制站的樹系中，實體和虛擬的使用一次同步處理階層。 最佳的作法 （英文） 白皮書是指["執行網域控制站在 HYPER-V"](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv.aspx)

### <a name="azure-virtual-machine-domain-joined-machine"></a>Azure 虛擬機器：已加入網域的電腦
如果您裝載的電腦已加入到現有的 Active Directory 樹系的網域，虛擬或實體，最佳做法是停用客體 TimeSync，並確認 W32Time 已設定為使用其網域控制站，透過設定時間同步處理類型 = NTP5

### <a name="azure-virtual-machine-standalone-workgroup-machine"></a>Azure 虛擬機器：獨立工作群組機器
如果 Azure VM 未加入網域，也不是網域控制站，建議保留預設時間組態，並讓同步處理與主機的 VM。

## <a name="windows-application-requiring-accurate-time"></a>Windows 應用程式需要準確的時間
### <a name="time-stamp-api"></a>時間戳記 API
程式需要的最大精確度與 UTC，並不經過的時間，應該使用[GetSystemTimePreciseAsFileTime API](https://msdn.microsoft.com/library/windows/desktop/Hh706895.aspx)。  如此可確保您的應用程式取得系統時間，由 Windows 時間服務的條件式。

### <a name="udp-performance"></a>UDP 效能
如果您有應用程式會使用 UDP 通訊交易和它的一定要盡量減少延遲，有一些相關的登錄項目可用來設定要排除的連接埠範圍連接埠篩選引擎的基底。  這會改善延遲並增加您的輸送量。  不過，登錄變更應該限制為有經驗的系統管理員。  此外，這個因應措施會排除從所保護的防火牆連接埠。  請參閱以下文件參考，如需詳細資訊。

如需 Windows Server 2012 和 Windows Server 2008，您必須先安裝 Hotfix。  您可以參考這份知識庫文章：[當您執行多點傳送的接收者應用程式在 Windows 8 和 Windows Server 2012 中的資料包遺失](https://support.microsoft.com/kb/2808584)

### <a name="update-network-drivers"></a>更新網路驅動程式
有些網路廠商有改善的效能與驅動程式延遲和緩衝 UDP 封包的驅動程式更新。  請連絡您的網路廠商，以查看是否有更新，以協助使用 UDP 輸送量。

## <a name="logging-for-auditing-purposes"></a>稽核記錄
時間追蹤法規遵循您可以手動封存 w32tm 記錄、 事件記錄檔和效能監視器資訊。  更新版本中，已封存的資訊可用來證明在特定時間在過去的合規性。  下列因素用來指示的精確度。


1. 使用的計算時間位移效能監視器計數器的時鐘精確度。  這會顯示動畫的時鐘使用所需的精確度。
2.  「 對等個體回應從 」 w32tm 記錄檔中尋找的時脈來源。   下列的訊息文字是 VMIC，描述的時間來源，以及參考時鐘鏈結中的下一個来驗證的 IP 位址。
3.  時鐘來驗證使用 w32tm 記錄檔的條件狀態 」 ClockDispl 專業領域：\*扭曲\*時間\*」 會發生。  這表示該 w32tm 在作用中的時間。

### <a name="event-logging"></a>事件記錄
若要取得完整的劇本，您也將需要事件記錄檔資訊。  藉由收集系統事件記錄檔，並篩選時間伺服器，Microsoft Windows-核心開機]、 [Microsoft-Windows-核心-一般，您可以探索是否有其他會影響已變更時，比方說，第三方。  這些記錄可能需要排除外部干擾。  群組原則可能會影響哪些事件記錄檔會寫入記錄檔。  如需詳細資訊，使用群組原則上看到上的一節。

### <a name="W32Logging"></a>W32time 偵錯記錄
若要啟用 w32tm 作為稽核用途，下列命令會啟用記錄，顯示時鐘的定期更新，並指出來源時鐘。  重新啟動該服務以啟用新的記錄。  

如需詳細資訊，請參閱 <<c0> [ 如何開啟偵錯記錄在 Windows 時間服務](https://support.microsoft.com/kb/816043)。

    w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-73,103,107,110

### <a name="performance-monitor"></a>效能監視器
Windows Server 2016 的 Windows 時間服務會公開可用來收集的稽核記錄效能計數器。  這些可以在本機或遠端記錄。  您可以記錄的電腦時間位移和來回行程延遲計數器。  
和任何效能計數器，例如，您可以從遠端監視它們並建立使用 System Center Operations Manager 的警示。  比方說，以便警示時間位移偏離所需的精確度時警示您。  [System Center 管理組件](https://social.technet.microsoft.com/wiki/contents/articles/15251.system-center-management-pack-authoring-guide.aspx)有詳細資訊。

### <a name="windows-traceability-example"></a>Windows 可追蹤性範例
從 w32tm 記錄檔，您要驗證兩項資訊。  第一個是記錄檔的目前條件時鐘的指示。  這證明，您的時鐘已正在的條件式 Windows 時間服務在有爭議的階段。

    151802 20:18:32.9821765s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:223 CR:156250 UI:100 phcT:65 KPhO:14307
    151802 20:18:33.9898460s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:64 KPhO:41
    151802 20:18:44.1090410s - ClockDispln Discipline: *SKEW*TIME* - PhCRR:1 CR:156250 UI:100 phcT:65 KPhO:38

主要的重點是，您就會看見訊息加上 ClockDispln 專業領域，也就是證明 w32time 正在您的系統時鐘與互動。
 
接著，您必須有爭議的時間就會報告目前正在使用以參考制來源電腦之前，找到記錄檔中的最後一份報表。  這可能是 IP 位址、 電腦名稱或 VMIC 提供者，這表示它同步處理與 hyper-v 主機。  下列範例提供 10.197.216.105 IPv4 位址。

    151802 20:18:54.6531515s - Response from peer 10.197.216.105,0x8 (ntp.m|0x8|0.0.0.0:123->10.197.216.105:123), ofs: +00.0012218s

既然您已驗證的參考時間鏈結中第一個系統，您要調查的參考時間來源的記錄檔，並重複相同的步驟。  這會繼續直到到達實體時鐘，例如 GPS 或已知的時間來源，例如 NIST。  如果參考時鐘 GPS 硬體，然後從製造的記錄檔可能會需要。

## <a name="network-considerations"></a>網路考量
NTP 通訊協定演算法會有相依性網路的對稱。  隨著您增加網路躍點數目，會增加不對稱的機率。  有，它很難預測您會看到在您特定的環境中的哪些類型的精確度。 

效能監視器 」 和 「 Windows Server 2016 中新的 Windows 時間計數器可用來評估您環境的精確度，並建立基準。 此外，您可以執行疑難排解，來判斷您的網路上的任何機器的目前位移。

有兩個一般標準準確的時間在網路上。  PTP ([有效位數時間通訊協定-IEEE 1588](https://www.nist.gov/el/intelligent-systems-division-73500/introduction-ieee-1588)) 更緊密的需求對網路基礎結構，但通常可以提供子微秒的精確度。  NTP ([網路時間通訊協定 – RFC 1305](https://tools.ietf.org/html/rfc1305)) 適用於較多種類的網路和環境，如此可以讓您更容易管理。 

Windows 預設的非網域聯結的機器支援簡單的 NTP (RFC2030)。  已加入網域的機器，我們會使用稱為安全 NTP [MS SNTP](https://msdn.microsoft.com/library/cc246877.aspx)，它會利用提供 RFC1305 和 RFC5905 中所述的驗證 NTP 管理優勢的交涉的網域密碼。   

網域與非網域聯結通訊協定需要 UDP 連接埠 123。  如需有關 NTP 最佳作法的詳細資訊，請參閱[網路時間通訊協定最佳目前作法 IETF 草稿](https://tools.ietf.org/html/draft-ietf-ntp-bcp-00)。

### <a name="reliable-hardware-clock-rtc"></a>可靠的硬體時鐘 (RTC)
Windows 會執行不是逐步時間，除非特定的範圍超過，但而 disciplines 時鐘。  這表示 w32tm 調整時鐘的頻率，以固定間隔，使用時鐘更新頻率設定，一次第二個 Windows Server 2016 的預設值。  如果時鐘是後面，它會加速頻率，如果繼續，會變慢頻率。  不過，在這段時間之間時脈頻率調整，硬體時鐘會在控制項中。  如果沒有發生問題的韌體或硬體時鐘，機器上的時間可能會變得較不精確。

這是您要測試的另一個原因，您的環境中的基準。  如果在您的目標準確性不穩定的 「 計算時間位移 」 效能計數器，您可能想要確認您的韌體是最新狀態。  另一項測試，您可以看到如果重複的硬體發生相同問題。

### <a name="troubleshooting-time-accuracy-and-ntp"></a>疑難排解的時間精確度和 NTP
您可以使用發掘更新版本，以了解不正確的時間來源的 [階層] 區段。  尋找於時間位移，找到其中時間分歧發揮其 NTP 來源階層中的點。  一旦您了解階層，您會想要試用，並了解為何該特定的時間來源不會收到正確的時間。  

將焦點放在系統狀態的時間，您可以使用下列這些工具來收集更多資訊來協助您判斷問題並找出解決方法。  下面的 UpstreamClockSource 參考是使用 「 w32tm /config /status 「 探索到的時鐘。


- 系統事件記錄檔
- 啟用記錄使用： w32tm 記錄檔-w32tm /debug /enable /file:C:\Windows\Temp\w32time-test.log /size:10000000 /entries:0-300
- w32Time Registry key HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time
- 本機網路追蹤
- （從本機電腦或 UpstreamClockSource） 的效能計數器
- W32tm /stripchart /computer:UpstreamClockSource
- 若要了解延遲和躍點數以來源 PING UpstreamClockSource
- Tracert UpstreamClockSource

問題|    問題|   解析度|
----- | ----- | ----- |
本機 TSC 時鐘可能會不穩定。| 使用 Perfmon-實體電腦 – 同步處理時鐘穩定時鐘，但您仍發現的數個 100us 每隔 1-2 分鐘。 |   更新韌體或驗證不同的硬體不會顯示相同的問題。|
網路延遲|    w32tm stripchart 顯示 RoundTripDelay 的多個 10 毫秒。  延遲的變化會導致雜訊的來回時間，例如僅以單一方向是延遲的 ½ 一樣大。</br></br>UpstreamClockSource 是多個躍點，以偵測。  TTL 應該接近 128。</br></br>您可以使用 Tracert 來尋找每個躍點的延遲。    | 找到接近時鐘時間。  一個解決方案是安裝在相同的區段的來源時鐘或以手動方式指向來源時鐘地理位置較接近。  網域的案例中，新增 GTimeServ 角色的電腦。 |  
無法可靠地連線到 NTP 來源|    W32tm /stripchart 間歇地傳回 「 要求已逾時 」    |NTP 來源未回應|
NTP 來源未回應|    檢查 NTP 用戶端來源計數 NTP 伺服器連入要求、 NTP 伺服器的傳出回應的 Perfmon 計數器，並判斷您的使用量，相較於您的基準。|    使用伺服器效能計數器，來判斷負載是否已變更您的基準參考。</br></br>是否有網路阻塞的問題？|
未使用的最精確的時鐘的網域控制站|    最近新增主要時鐘的拓撲中的變更。|   w32tm /resync /rediscover|
用戶端時鐘會離開| 時間服務事件 36 系統事件記錄檔和/或文字中描述的記錄檔中：「 NTP 用戶端的時間來源計數 」 計數器會從 1 設為 0|疑難排解上游來源，並了解它遇到效能問題。|

### <a name="baselining-time"></a>基準化時間
因此，您第一次，可以了解效能和精確度，您的網路，並與基準未來比較，當發生問題時，基準化很重要。  您會想要比較基準根 PDC 或 GTIMESRV 標示任何機器。  我們也會建議您基準每個樹系中的 PDC。  最後挑選任何重要的網域控制站或其有趣的特性，例如距離或高負載和基準電腦這些。

它也很有用到 Windows Server 2016 基準 vs 2012 R2 中，但您只需要 w32tm /stripchart 作為工具，您可以用來比較，因為 Windows Server 2012 r2 沒有效能計數器。  您應該挑選兩部機器以相同的特性，或將電腦升級和更新之後比較的結果。  Windows Time 測量數據增補有如何之間 2016年和 2012年的詳細的度量的詳細資訊。

使用所有 w32time 效能計數器，至少一週收集資料。  這會確保您有足夠的各種在經過一段時間的網路帳戶的參考和足夠執行以提供您的時間精確度是穩定的信心。

### <a name="ntp-server-redundancy"></a>NTP 伺服器備援
搭配非網域聯結的機器或 PDC 手動 NTP 伺服器設定，讓一個以上的伺服器是發生可用性的備援性良好量值。  它也可能提供較佳的精確度，假設所有來源都都精確且穩定。  不過，如果拓撲沒有良好的設計，或不穩定的時間來源，產生的精確度可能會較差因此請謹慎。  支援伺服器 w32time 可以手動參考的時間限制為 10。 

## <a name="leap-seconds"></a>閏秒
在地球輪換期間隨著 climatic 和地質事件所造成的時間。 一般而言，變化是約一秒鐘每隔幾年來。 每當從不可部分完成的時間變化成長得太大，為一秒 （向上或向下） 的更正插入，稱為 「 閏秒。 這是一種差異絕不會超出 0.9 的秒數。 這項更正宣佈已實際修正之前的六個月。 之前 Windows Server 2016 中，Microsoft 時間服務無法察覺潤秒，但依賴外部時間服務，若要解決此問題。 Windows Server 2016 較多的時間精確度，Microsoft 正在努力閏秒問題更合適的解決方案。

## <a name="secure-time-seeding"></a>安全時間植入
在 Server 2016 中的 W32time 包含安全時間植入功能。 此功能會判斷目前的大約時間，來自傳出的 SSL 連線。  此時間值用來監視本機系統時鐘，並更正任何毛額的錯誤。 您可以深入了解中的功能[此部落格文章](https://blogs.msdn.microsoft.com/w32time/2016/09/28/secure-time-seeding-improving-time-keeping-in-windows/)。  在部署中使用可靠的時間來源和也受監視的機器，包括 時間位移的監視，您可以選擇不使用安全時間植入功能，而改為依賴您現有的基礎結構。 

您可以停用此功能，進行下列步驟：

1.  設定為 0 的 UtilizeSSLTimeData 登錄設定值，在特定電腦上：

    reg add HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\w32time\Config /v UtilizeSslTimeData /t REG_DWORD /d 0 /f


2.  如果您無法立即因某些原因而重新開機時，您可以通知 W32time 服務有關的組態更新。 這會停止時間監視和強制執行時間的資料收集從 SSL 連線。 

    W32tm.exe /config /update

3.  可讓設定生效立即重新啟動電腦，並也會使它停止會收集任何時間的 SSL 連線。  後半部非常小的額外負荷，而且不應該是效能問題。

4.  若要套用此設定，在整個網域，請將 UtilizeSSLTimeData 值設定為 0 的 W32time 群組原則設定中，並發佈設定。  時設定由群組原則用戶端，W32time 服務會收到通知，它會停止時間監視和強制使用 SSL 時間資料。 每個機器重新開機時，將會停止 SSL 時間資料集合。 如果您的網域有可攜式輕型的膝上型電腦/平板電腦和其他裝置，您可能想要排除這類機器，這項原則變更。 這些裝置最後會面臨電池清空，並需要安全時間植入功能來啟動時間。


