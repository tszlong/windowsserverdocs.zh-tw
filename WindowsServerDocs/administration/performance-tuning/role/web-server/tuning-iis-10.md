---
title: 調整 IIS 10。0
description: Windows Server 16 上 IIS 10.0 網頁伺服器的效能微調 recommmendations
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 8b86bf779a4ea9d67f959dacf125a98a8e26a729
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75947129"
---
# <a name="tuning-iis-100"></a>調整 IIS 10。0

Internet Information Services （IIS）10.0 隨附在 Windows Server 2016 中。 它會使用類似于 IIS 8.5 和 IIS 7.0 的進程模型。 核心模式 web 驅動程式（HTTP.sys）會接收並路由傳送 HTTP 要求，並滿足來自其回應快取的要求。 工作者進程會註冊 URL subspaces，而 HTTP.sys 會將要求路由傳送至適當的進程（或應用程式集區的一組進程）。

Http.sys 負責連接管理和要求處理。 您可以從 HTTP.SYS 快取提供要求，或傳遞至背景工作進程以進行進一步的處理。 可以設定多個背景工作進程，以降低成本提供隔離。 如需有關要求處理如何運作的詳細資訊，請參閱下圖：

![iis 10.0 中的要求處理](../../media/perftune-guide-iis-request-handling.png)

Http.sys 包含回應快取。 當要求符合回應快取中的專案時，HTTP.sys 會直接從核心模式傳送快取回應。 某些 web 應用程式平臺（例如 ASP.NET）提供的機制，可讓任何動態內容在核心模式快取中進行快取。 IIS 10.0 中的靜態檔案處理常式會自動快取 HTTP.sys 中經常要求的檔案。

因為 web 伺服器具有核心模式和使用者模式元件，所以必須微調這兩個元件以獲得最佳效能。 因此，針對特定的工作負載調整 IIS 10.0 包括設定下列各項：

-   Http.sys 和相關聯的核心模式快取

-   工作者進程和使用者模式 IIS，包括應用程式集區設定

-   會影響效能的特定調整參數

下列各節將討論如何設定 IIS 10.0 的核心模式和使用者模式方面。

## <a name="kernel-mode-settings"></a>核心模式設定

與效能相關的 HTTP.SYS 設定分成兩個廣泛的類別：快取管理和連線和要求管理。 所有登錄設定會儲存在下列登錄專案底下：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**請注意**  如果 HTTP 服務已在執行中，您必須重新開機它，變更才會生效。

&nbsp; 

## <a name="cache-management-settings"></a>快取管理設定

HTTP.SYS 提供的一項優點是核心模式快取。 如果回應是在核心模式快取中，您可以完全從核心模式滿足 HTTP 要求，這樣會大幅降低處理要求的 CPU 成本。 不過，IIS 10.0 的核心模式快取是以實體記憶體為基礎，而專案的成本則是它所佔用的記憶體。

快取中的專案只有在使用時才有用。 不過，不論專案是否正在使用，專案一律會取用實體記憶體。 您必須考慮可用的資源（CPU 和實體記憶體）和工作負載，評估快取中某個專案的實用性（從快取提供服務的節省量）及其成本（已佔用的實體記憶體）滿足. Http.sys 會嘗試只在快取中保留實用、主動存取的專案，但您可以藉由調整特定工作負載的 HTTP.SYS 快取來增加 web 伺服器的效能。

以下是 HTTP.SYS 核心模式快取的一些實用設定：

-   **UriEnableCache**預設值：1

    非零值會啟用核心模式回應和片段快取。 對於大部分的工作負載，快取應該保持啟用狀態。 如果您預期會有非常低的回應和片段快取，請考慮停用快取。

-   **UriMaxCacheMegabyteCount**預設值：0

    非零值，指定可供核心模式快取使用的最大記憶體。 預設值為0，可讓系統自動調整快取可用的記憶體數量。

    **注意**指定大小只會設定最大值，而且系統可能不會讓快取成長到設定的大小上限。

    &nbsp; 

-   **UriMaxUriBytes**預設值：262144個位元組（256 KB）

    核心模式快取中專案的大小上限。 大於這個的回應或片段不會進行快取。 如果您有足夠的記憶體，請考慮增加限制。 如果記憶體受到限制，且大型專案的根本較小，則降低限制可能會很有説明。

-   **UriScavengerPeriod**預設值：120秒

    清除程式會定期掃描 HTTP.SYS 快取，而且不會移除清理程式掃描之間的專案。 將 [清理時間] 設定為 [高] 值會減少清除清除的次數。 不過，快取記憶體使用量可能會增加，因為較舊、較不常存取的專案可能會保留在快取中。 將 [期間] 設得太低會導致清除更頻繁的清除，而且可能會導致太多排清和快取變換。

## <a name="request-and-connection-management-settings"></a>要求和連接管理設定

在 Windows Server 2016 中，HTTP.sys 會自動管理連接。 已不再使用下列登錄設定：

-   **MaxConnections**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\MaxConnections
    ```

-   **IdleConnectionsHighMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsHighMark
    ```

-   **IdleConnectionsLowMark**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleConnectionsLowMark
    ```

-   **IdleListTrimmerPeriod**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\IdleListTrimmerPeriod
    ```

-   **RequestBufferLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\RequestBufferLookasideDepth
    ```

-   **InternalRequestLookasideDepth**

    ``` syntax
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters\InternalRequestLookasideDepth
    ```


## <a name="user-mode-settings"></a>使用者模式設定

本節中的設定會影響 IISÂ10.0 背景工作進程的行為。 大部分的設定都可以在下列 XML 設定檔中找到：

% SystemRoot%\\system32\\inetsrv administration.config\\config\\Applicationhost.config

使用 Appcmd.exe、IIS 10.0 管理主控台、WebAdministration 或 IISAdministration PowerShell Cmdlet 來變更它們。 大部分的設定都會自動偵測到，而且不需要重新開機 IIS 10.0 背景工作進程或 web 應用程式伺服器。 如需 Applicationhost.config 檔案的詳細資訊，請參閱[Applicationhost.config 簡介](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)。


## <a name="ideal-cpu-setting-for-numa-hardware"></a>NUMA 硬體的理想 CPU 設定

從 Windows 2016 開始，IIS 10.0 針對其執行緒集區執行緒支援自動理想的 CPU 指派，以增強 NUMA 硬體的效能和擴充性。 此功能預設為啟用，而且可以透過下列登錄機碼來設定：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

啟用這項功能後，IIS 執行緒管理員就能根據目前的負載，在所有 NUMA 節點的所有 Cpu 上平均散發 IIS 執行緒集區執行緒。 一般情況下，建議您針對 NUMA 硬體將此預設設定保持不變。

**請注意**  [理想的 CPU] 設定不同于[應用程式集區的 cpu 設定](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu)中引進的工作者進程 NUMA 節點指派設定（numaNodeAssignment 和 numaNodeAffinityMode）。 理想的 CPU 設定會影響 IIS 散發其執行緒集區執行緒的方式，而工作者進程 NUMA 節點指派設定則會決定工作者進程啟動所在的 NUMA 節點。

## <a name="user-mode-cache-behavior-settings"></a>使用者模式快取行為設定

本節說明在 IISÂ10.0 中會影響快取行為的設定。 使用者模式快取會實作為模組，以接聽整合管線所引發的全域快取事件。 若要完全停用使用者模式快取，請從 Applicationhost.config 中的 System.webserver/globalModules configuration 區段的已安裝模組清單中移除 FileCacheModule （cachfile）模組。

**System.webserver/caching**

|屬性|說明|Default|
|--- |--- |--- |
|Enabled|當設定為**False**時，會停用使用者模式 IIS 快取。 當快取點擊率非常少時，您可以完全停用快取，以避免與快取程式碼路徑相關聯的額外負荷。 停用使用者模式快取並不會停用核心模式快取。|True|
|enableKernelCache|當設定為**False**時，會停用核心模式快取。|True|
|maxCacheSize|將 IIS 使用者模式快取大小限制為指定的大小（以 Mb 為單位）。 IIS 會根據可用的記憶體調整預設值。 根據經常存取的檔案集合大小與 RAM 或 IIS 進程位址空間的數量，仔細選擇值。|0|
|maxResponseSize|快取檔案，最多可達指定的大小。 實際值取決於資料集內最大檔案的數目和大小與可用的 RAM。 快取大型、經常要求的檔案可以減少 CPU 使用量、磁片存取和相關聯的延遲。|262144|

## <a name="compression-behavior-settings"></a>壓縮行為設定

從7.0 開始的 IIS 預設會壓縮靜態內容。 此外，在安裝 DynamicCompressionModule 時，預設會啟用動態內容的壓縮。 壓縮可減少頻寬使用量，但會增加 CPU 使用量。 如果可能的話，會在核心模式快取中快取已壓縮的內容。 從8.5 開始，IIS 可讓您獨立控制靜態和動態內容的壓縮。 靜態內容通常是指不會變更的內容，例如 GIF 或 HTM 檔案。 動態內容通常是由伺服器上的腳本或程式碼所產生，也就是 ASP.NET 網頁。 您可以自訂任何特定延伸模組的分類為靜態或動態。

若要完全停用壓縮，請從 Applicationhost.config 中的 System.webserver/globalModules 區段的模組清單中移除 StaticCompressionModule 和 DynamicCompressionModule。

**System.webserver/HTTPCompression**

|屬性|說明|Default|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|如果目前的 CPU 使用量百分比高於或低於指定的限制，則啟用或停用壓縮。<br><br>從 IIS 7.0 開始，如果穩定狀態 CPU 的增加高於停用閾值，則會自動停用壓縮。 如果 CPU 低於啟用閾值，則會啟用壓縮。|分別為50、100、50和90|
|directory|指定臨時儲存和快取靜態檔案壓縮版本的目錄。 如果經常存取此目錄，請考慮將它移出系統磁片磁碟機。|%SystemDrive%\inetpub\temp\IIS 暫存的壓縮檔案|
|doDiskSpaceLimiting|指定所有壓縮檔案可佔用的磁碟空間大小是否有限制。 壓縮檔案會儲存在**目錄**屬性所指定的壓縮目錄中。|True|
|maxDiskSpaceUsage|指定壓縮檔案在壓縮目錄中可佔用的磁碟空間位元組數。<br><br>如果所有壓縮內容的總大小太大，則此設定可能需要增加。|100 MB|

**System.webserver/urlCompression**

|屬性|說明|Default|
|--- |--- |--- |
|doStaticCompression|指定是否壓縮靜態內容。|True|
|doDynamicCompression|指定是否壓縮動態內容。|True|

**注意**對於執行 IIS 10.0 但平均 CPU 使用率很低的伺服器，請考慮啟用動態內容的壓縮，特別是當回應很大時。 您應該先在測試環境中完成這項作業，以評估從基準對 CPU 使用量的影響。


### <a name="tuning-the-default-document-list"></a>微調預設檔案清單

預設檔模組會處理目錄根目錄的 HTTP 要求，並將它們轉譯成特定檔案的要求，例如，預設的 .htm 或 index.htm。 平均來說，aroundÂ網際網路上所有要求的25% 會經歷預設的檔路徑。 這在個別網站上會有很大的差異。 當 HTTP 要求未指定檔案名時，預設的檔模組會搜尋檔案系統中每個名稱允許的預設檔案清單。 這可能會對效能造成負面影響，特別是當到達內容需要進行網路往返或觸及磁片時。

您可以選擇性地停用預設檔，並藉由減少或排序檔案清單來避免額外負荷。 針對使用預設檔的網站，您應該將清單縮減為僅限使用的預設檔案類型。 此外，請排序清單，使其開頭為最常存取的預設檔案名稱。

您可以選擇性地在 Applicationhost.config 中自訂位置標記內的設定，或直接在內容目錄中插入 web.config 檔案，以在特定 Url 上設定預設檔行為。 這允許混合式方法，只在必要時才啟用預設檔，並將清單設定為每個 URL 的正確檔案名。

若要完全停用預設檔，請從 Applicationhost.config 中的 System.webserver/globalModules 區段的模組清單中移除 DefaultDocumentModule。

**System.webserver/defaultDocument**

|屬性|說明|Default|
|--- |--- |--- |
|enabled|指定啟用預設檔。|True|
|&lt;檔案&gt; 元素|指定設定為預設檔的檔案名。|預設清單為預設的 .htm、預設值 .asp、index.htm、Iisstart.htm、.html、.htm 和 default.aspx。|

## <a name="central-binary-logging"></a>中央二進位記錄

當伺服器會話中有多個 URL 群組時，為個別的 URL 群組建立數百個格式化的記錄檔，並將記錄資料寫入磁片的程式，可以快速地耗用寶貴的 CPU 和記憶體資源，進而建立效能和擴充性問題。 集中式二進位記錄可減少用於記錄的系統資源數量，同時為需要它的組織提供詳細的記錄資料。 剖析二進位格式的記錄檔需要後置處理工具。

您可以藉由將 centralLogFileMode 屬性設定為 CentralBinary，並將**enabled**屬性設定為**True**來啟用中央二進位記錄。 請考慮將中央記錄檔的位置從系統磁碟分割移至專用的記錄磁片磁碟機，以避免系統活動與記錄活動之間的競爭。

**system.web/log**

|屬性|說明|Default|
|--- |--- |--- |
|centralLogFileMode|指定伺服器的記錄模式。 將此值變更為 CentralBinary，以啟用中央二進位記錄。|站台|

**system.web/log/centralBinaryLogFile**

|屬性|說明|Default|
|--- |--- |--- |
|enabled|指定是否啟用中央二進位記錄。|False|
|directory|指定寫入記錄專案的目錄。|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>應用程式和網站 tunings

下列設定與應用程式集區和網站 tunings 相關。

**system.webserver/applicationPools/applicationPoolDefaults**

|屬性|說明|Default|
|--- |--- |--- |
|queueLength|向 HTTP.SYS 表示在未來的要求遭到拒絕之前，應用程式集區的佇列要求數。 當超過這個屬性的值時，IIS 會拒絕後續的要求，並出現503錯誤。<br><br>如果觀察到503錯誤，請考慮為與高延遲後端資料存放區進行通訊的應用程式增加這種情況。|1000|
|enable32BitAppOnWin64|當為 True 時，可讓32位應用程式在具有64位處理器的電腦上執行。<br><br>如果記憶體耗用量是問題，請考慮啟用32位模式。 由於指標大小和指令大小較小，因此32位應用程式所使用的記憶體會比64位應用程式少。 在64位電腦上執行32位應用程式的缺點是，使用者模式位址空間限制為 4 GB。|False|

**system.web/sites/VirtualDirectoryDefault**

|屬性|說明|Default|
|--- |--- |--- |
|allowSubDirConfig|指定 IIS 是否會尋找低於目前層級（True）之內容目錄中的 web.config 檔案，或不會尋找低於目前層級（False）之內容目錄中的 web.config 檔案。 藉由強加簡單的限制，僅允許在虛擬目錄中進行設定，IISÂ10.0 可以知道，除非 **/&lt;名稱&gt;.htm**是虛擬目錄，否則不應尋找設定檔。 略過額外的檔案作業，可以大幅改善具有一組大量隨機存取之靜態內容的網站效能。|True|

## <a name="managing-iis-100-modules"></a>管理 IIS 10.0 模組

IIS 10.0 已分解成多個使用者可擴充的模組，以支援模組化結構。 此分解的成本很小。 針對每個模組，整合式管線必須針對與模組相關的每個事件呼叫模組。 無論模組是否必須執行任何工作，都會發生這種情況。 您可以藉由移除與特定網站無關的所有模組，來節省 CPU 週期和記憶體。

針對簡單靜態檔案微調的 web 伺服器可能只包含下列五個模組： UriCacheModule、HttpCacheModule、StaticFileModule、AnonymousAuthenticationModule 和 HttpLoggingModule。

若要從 Applicationhost.config 移除模組，除了移除 system.webserver/globalModules 中的模組宣告之外，從 System.webserver/handler 和 system.webserver/模組區段移除模組的所有參考。

## <a name="classic-asp-settings"></a>傳統 ASP 設定

處理傳統 ASP 要求的主要成本包括初始化腳本引擎、將要求的 ASP 腳本編譯成 ASP 範本，以及在腳本引擎上執行範本。 雖然範本執行成本取決於所要求 ASP 腳本的複雜性，但 IIS 傳統 ASP 模組可以將腳本引擎快取在記憶體中，並在記憶體和磁片中快取範本（僅限於記憶體內部範本快取溢位），以提升效能CPU 系結案例。

下列設定是用來設定傳統 ASP 範本快取和腳本引擎快取，而且不會影響 ASP.NET 設定。

**System.webserver/asp/cache**

|屬性|說明|Default|
|--- |--- |--- |
|diskTemplateCacheDirectory|當記憶體內部快取溢位時，ASP 用來儲存已編譯之範本的目錄名稱。<br><br>建議：將設定為不常使用的目錄，例如，未與作業系統、IIS 記錄檔或其他經常存取的內容共用的磁片磁碟機。|%SystemDrive%\inetpub\temp\ASP 編譯的範本|
|maxDiskTemplateCacheFiles|指定可在磁片上快取的已編譯 ASP 範本數目上限。<br><br>建議：設定為0x7FFFFFFF 的最大值。|2000|
|scriptFileCacheSize|這個屬性會指定可在記憶體中快取的已編譯 ASP 範本數目上限。<br><br>建議：至少設定為與應用程式集區所服務的常用 ASP 腳本數目一樣多。 可能的話，請將設定為與記憶體限制允許的數目一樣多的 ASP 範本。|500|
|scriptEngineCacheMax|指定將在記憶體中保持快取的腳本引擎數目上限。<br><br>建議：至少設定為與應用程式集區所服務的常用 ASP 腳本數目一樣多。 可能的話，請將設定為與記憶體限制所允許的數目一樣多的腳本引擎。|250|

**System.webserver/asp/限制**

|屬性|說明|Default|
|--- |--- |--- |
|processorThreadMax|指定 ASP 可以建立的每個處理器的背景工作執行緒最大數目。 如果目前的設定不足以處理負載，則增加，這可能會在服務要求或導致 CPU 資源使用量過低時造成錯誤。|25|

**System.webserver/asp/comPlus**

|屬性|說明|Default|
|--- |--- |--- |
|executeInMta|如果在 IIS 提供 ASP 內容時偵測到錯誤或失敗，則設為**True** 。 例如，裝載多個隔離的網站（其中每個網站都在自己的背景工作進程下執行）時，就會發生這種情況。 錯誤通常會從事件檢視器中的 COM + 報告。 此設定會啟用 ASP 中的多執行緒單元模型。|False|


## <a name="aspnet-concurrency-setting"></a>ASP.NET 並行設定

### <a name="aspnet-35"></a>ASP.NET 3.5
根據預設，ASP.NET 會限制要求並行處理，以減少伺服器上的穩定狀態記憶體耗用量。 高平行存取應用程式可能需要調整某些設定，以改善整體效能。 您可以在 aspnet .config 檔案中變更此設定：

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

下列設定適用于完整使用系統上的資源：

-   **maxConcurrentRequestPerCpu**預設值：5000

    此設定會限制系統上同時執行之 ASP.NET 要求的最大數目。 預設值是保守地減少 ASP.NET 應用程式的記憶體耗用量。 請考慮在執行長時間同步 i/o 作業之應用程式的系統上增加此限制。 否則，使用者可能會因為佇列或要求失敗而發生高延遲，因為使用預設設定時，在高負載下超出佇列限制。

### <a name="aspnet-46"></a>ASP.NET 4.6
除了 maxConcurrentRequestPerCpu 設定之外，ASP.NET 4.7 也提供設定，以改善高度依賴非同步作業之應用程式的效能。 您可以在 aspnet .config 檔案中變更此設定。

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit**預設值90：當大量負載（超過硬體功能）進入這類案例時，非同步要求會有一些擴充性問題。 問題是因為在非同步案例上配置的本質。 在這些情況下，當非同步作業開始時，將會進行配置，並在完成時使用。 在這段時間，itâs 非常可能的物件已由 GC 移至第1代或第2代。 發生這種情況時，增加負載將會顯示每秒要求數（rps）的增加，直到某個點為止。 一旦通過該點，垃圾收集所花費的時間就會開始變成一個問題，而 rps 會開始 dip，並具有負面的縮放效果。 若要修正此問題，當 cpu 使用量超過 percentCpuLimit 設定時，會將要求傳送至 ASP.NET 原生佇列。
-   **percentCpuLimitMinActiveRequestPerCpu**預設值： 100 CPU 節流（percentCpuLimit 設定）不是以要求數為基礎，而是以其為代價。 因此，可能只會有幾個需要大量 CPU 的要求導致原生佇列中的備份，而無法從傳入要求中清空。 若要解決此 problme，可以使用 percentCpuLimitMinActiveRequestPerCpu 來確保在進行節流之前，會提供最少的要求數。

## <a name="worker-process-and-recycling-options"></a>工作者進程和回收選項

您可以設定回收 IIS 工作者進程的選項，並在不需要介入或重設服務或電腦的情況下，為銳的情況或事件提供實用的解決方案。 這類情況和事件包括記憶體流失、增加記憶體負載，或沒有回應或閒置的工作者進程。 在一般情況下，可能不需要回收選項，而且可以關閉回收，或將系統設定成非常不常回收。

您可以藉由將屬性新增至**回收/periodicRestart**元素，來啟用特定應用程式的進程回收。 回收事件可以由數個事件觸發，包括記憶體使用量、固定的要求數，以及固定的時間週期。 回收背景工作進程時，會清空已排入佇列和執行中的要求，同時也會同時啟動新的進程來服務新的要求。 **回收/periodicRestart**元素是每個應用程式，這表示下表中的每個屬性都是以每個應用程式為基礎進行分割。

**system.webserver/applicationPools/ApplicationPoolDefaults/回收/periodicRestart**

|屬性|說明|Default|
|--- |--- |--- |
|memory|如果虛擬記憶體耗用量超過指定的限制（以 kb 為單位），則啟用進程回收。 對於具有小型、2 GB 位址空間的32位電腦而言，這是很有用的設定。 這有助於避免因為記憶體不足的錯誤而導致失敗的要求。|0|
|privateMemory|如果私用記憶體配置超過指定的限制（以 kb 為單位），則啟用進程回收。|0|
|requests|在特定數目的要求之後，啟用進程回收。|0|
|時間|在指定的時間週期之後啟用進程回收。|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>動態背景工作-進程頁面超微調

從 Windows Server 2012 R2 開始，IIS 提供了設定背景工作進程在閒置一段時間後暫停的選項（除了 [終止] 的選項，因為 IIS 7 之後就會存在）。

「閒置工作者進程」分頁和「閒置工作者進程終止」功能的主要目的，是為了節省伺服器上的記憶體使用量，因為網站可能會耗用大量的記憶體，即使只是坐在那裡也是接聽。 根據網站上所使用的技術（靜態內容與 ASP.NET 與其他架構），使用的記憶體可以從大約 10 MB 到數百 Mb 的任何位置，這表示如果您的伺服器設定了許多網站，則會找出最有效率的設定針對您的網站，可以大幅提升作用中和已擱置網站的效能。

在我們進入細節之前，我們必須記住，如果沒有記憶體限制，最好只將網站設定為永不暫停或終止。 畢竟，如果是電腦上唯一的工作者進程，thereâs 就不會有太大的價值。

**請注意** ，萬一網站執行不穩定的程式碼（例如發生記憶體流失的程式碼，或不穩定的程式碼），將網站設定為 [閒置時終止] 可能是修正程式碼 bug 的快速又有可能的替代方式。 這不是我們建議的做法，但在借助電腦中，使用這項功能做為清除機制可能會比較好，而更永久性的解決方案則在運作中。\]

&nbsp; 

另一個要考慮的因素是，如果網站確實使用海量儲存體，則暫停進程本身會收取付費，因為電腦必須將工作者進程所使用的資料寫入磁片。 如果工作者進程正在使用大量的記憶體，則暫停它的成本可能會比必須等待它啟動備份的代價高。

若要充分發揮工作者進程暫止的功能，您必須在每個應用程式集區中檢查您的網站，並決定應暫停哪些，應加以終止，而哪些應該是無限期地作用中。 針對每個動作和每個網站，您需要找出理想的超時時間。

在理想的情況下，您將設定為暫停或終止的網站是每天都有訪客的網站，但不足以保證隨時保持作用中狀態。 這些通常是一天左右約20名唯一訪客的網站。 您可以使用網站的記錄檔來分析流量模式，並計算平均每日流量。

請記住，一旦特定使用者連線到網站之後，通常至少會保留一段時間、提出額外的要求，因此只計算每日要求可能無法正確反映實際的流量模式。 若要取得更精確的閱讀，您也可以使用工具（例如 Microsoft Excel）來計算要求之間的平均時間。 例如：

||要求 URL|要求時間|Delta|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/SourceSilverLight/GeosourcewebService/服務 .asmx|10:23|0:11|
|6|/SourceSilverLight/Geosource. web/GeoSearchServer ... ¦。|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la ... ¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap ... ¦。|12:51|0:01|
|9|/rest/Services/DynamicService/Silverlight_basemap ... ¦。|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache 。|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache .js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

不過，困難的部分是找出要套用的設定有何意義。 在我們的案例中，網站會從使用者取得一堆要求，而上表顯示在4小時內總共發生4個唯一會話。 使用應用程式集區的背景工作進程暫止的預設設定時，網站會在預設超時20分鐘後終止，這表示每個使用者都會經歷網站微調迴圈。 這讓它成為工作者進程暫止的理想候選，因為在大部分的情況下，網站都處於閒置狀態，因此暫停它會節省資源，並允許使用者幾乎立即到達網站。

最後一點很重要的一點是，磁片效能對這項功能很重要。 由於暫停和喚醒程式牽涉到將大量資料寫入硬碟，因此強烈建議使用快速磁片來進行這項操作。 固態硬碟（Ssd）非常理想，而且強烈建議您這麼做，而且您應該確定 Windows 分頁檔案儲存在該磁片上（如果作業系統本身並未安裝在 SSD 上，請設定作業系統將分頁檔案移到它）。

無論您是否使用 SSD，我們也建議您修正分頁檔案的大小，以配合在不進行檔案調整大小的情況下將分頁資料寫入其中。 當作業系統需要在分頁檔案中儲存資料時，可能會發生頁面檔案大小調整，因為根據預設，Windows 會設定為根據需求自動調整其大小。 藉由將大小設為固定一，您可以避免調整規模並改善效能。

若要設定預先固定的頁面檔案大小，您必須計算其理想大小，這取決於您要暫停的網站數目，以及它們所使用的記憶體數量。 如果使用中工作者進程的平均為 200 MB，而您在即將暫停的伺服器上有500個網站，則分頁檔案的大小至少應為（200 \* 500） MB （在我們的範例中為基底 + 100 GB）。

**注意**當網站暫止時，它們會耗用大約 6 MB，因此在我們的案例中，如果所有網站都已暫止，則記憶體使用量將會約 3 GB。 不過事實上，youâre 可能永遠都不會同時全部暫停。

 
## <a name="transport-layer-security-tuning-parameters"></a>傳輸層安全性調整參數

使用傳輸層安全性（TLS）會造成額外的 CPU 成本。 TLS 的最昂貴元件是建立會話建立的成本，因為它牽涉到完整的交握。 重新連接、加密和解密也會增加成本。 如需更好的 TLS 效能，請執行下列動作：

-   針對 TLS 會話啟用 HTTP 保持連接。 這可排除會話的建立成本。

-   適當時重複使用會話，特別是在非 keep-alive 流量的情況下。

-   選擇性地將加密套用到需要它的網頁或部分，而不是整個網站。

**注意**
-   較大的索引鍵可提供更高的安全性，但也會使用較多的 CPU 時間。

-   所有元件可能不需要加密。 不過，混合純 HTTP 和 HTTPS 可能會導致快顯警告，而不是網頁上的所有內容都是安全的。

 
## <a name="internet-server-application-programming-interface-isapi"></a>網際網路伺服器應用程式開發介面（ISAPI）

ISAPI 應用程式不需要任何特殊的調整參數。 如果您撰寫私人 ISAPI 延伸模組，請確定它是針對效能和資源使用所撰寫的。

## <a name="managed-code-tuning-guidelines"></a>Managed 程式碼調整方針

IIS 10.0 中的整合式管線模型可提供高程度的彈性和擴充性。 在原生或 managed 程式碼中執行的自訂模組可以插入管線中，或可以取代現有的模組。 雖然這種擴充性模型提供了便利且簡單的功能，但在插入連結到全域事件的新受控模組之前，您應該先小心。 新增全域受管理的模組表示所有要求（包括靜態檔案要求）都必須觸控 managed 程式碼。 自訂模組易受垃圾收集之類的事件影響。 此外，自訂模組會因為在原生和 managed 程式碼之間封送處理資料，而增加了大量的 CPU 成本。 可能的話，您應該將 [managedHandler] 設定為 [受控模組的先決條件]。

若要取得更好的冷啟動效能，請確定您先行編譯 ASP.NET 網站，或利用 IIS 應用程式初始化功能來準備應用程式。

如果不需要會話狀態，請確定您已針對每個頁面關閉它。

如果有許多 i/o 系結作業，請嘗試使用相關 Api 的非同步版本，這可提供您更好的擴充性。

此外，適當地使用輸出快取也會提升網站的效能。

當您在隔離模式下執行多個包含 ASP.NET 腳本的主機時（每個網站一個應用程式集區），請監視記憶體使用量。 請確定伺服器有足夠的 RAM 可滿足所需的同時執行應用程式集區數目。 請考慮使用多個應用程式域，而不是多個隔離的進程。


## <a name="other-issues-that-affect-iis-performance"></a>其他會影響 IIS 效能的問題

下列問題可能會影響 IIS 效能：

-   安裝不是快取感知的篩選器

    安裝不是 HTTP 快取感知的篩選器會導致 IIS 完全停用快取，這會導致效能不佳。 在 IISÂ6.0 之前寫入的 ISAPI 篩選器可能會導致此行為。

-   通用閘道介面（CGI）要求

    基於效能考慮，IIS 不建議使用 CGI 應用程式來處理要求。 經常建立和刪除 CGI 進程牽涉到相當大的負擔。 更好的替代方案包括使用 FastCGI、ISAPI 應用程式腳本和 ASP 或 ASP.NET 腳本。 每個選項都可以使用隔離。

## <a name="see-also"></a>請參閱
- [Web 服務器效能調整](index.md) 
- [HTTP 1.1/2 調整](http-performance.md)
