---
title: 調整 IIS 10.0
description: Windows Server 16 上 IIS 10.0 web 伺服器的效能微調 recommmendations
ms.topic: landing-page
ms.author: ericam
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 5ee33ad0c5246efa40263bcdba8c4380efb2e2bf
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078085"
---
# <a name="tuning-iis-100"></a>調整 IIS 10.0

Internet Information Services (IIS) 10.0 隨附于 Windows Server 2016。 它會使用類似于 IIS 8.5 和 IIS 7.0 的進程模型。 核心模式 web 驅動程式 ( # A0) 接收並路由傳送 HTTP 要求，並滿足其回應快取的要求。 背景工作進程會註冊 URL subspaces，http.sys 將要求路由至適當的進程， (或) 應用程式集區的處理常式。

HTTP.sys 負責連接管理和要求處理。 您可以從 HTTP.sys 快取提供要求，或傳遞至背景工作進程以進行進一步的處理。 您可以設定多個背景工作進程，以降低成本提供隔離。 如需要求處理如何運作的詳細資訊，請參閱下圖：

![iis 10.0 中的要求處理](../../media/perftune-guide-iis-request-handling.png)

HTTP.sys 包含回應快取。 當要求符合回應快取中的專案時，HTTP.sys 會直接從核心模式傳送快取回應。 有些 web 應用程式平臺（例如 ASP.NET）會提供機制，讓任何動態內容都能在核心模式快取中快取。 IIS 10.0 中的靜態檔案處理常式會自動快取 http.sys 中經常要求的檔案。

因為網頁伺服器具有核心模式和使用者模式元件，所以必須調整這兩個元件以獲得最佳效能。 因此，針對特定工作負載調整 IIS 10.0 包括設定下列各項：

-   HTTP.sys 以及相關聯的核心模式快取

-   背景工作進程和使用者模式 IIS，包括應用程式集區設定

-   會影響效能的特定微調參數

下列各節將討論如何設定 IIS 10.0 的核心模式和使用者模式的層面。

## <a name="kernel-mode-settings"></a>核心模式設定

與效能相關的 HTTP.sys 設定分為兩大類：快取管理和連線和要求管理。 所有登錄設定會儲存在下列登錄專案下：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**注意**  如果 HTTP 服務已在執行中，您必須重新開機它，變更才會生效。

## <a name="cache-management-settings"></a>快取管理設定

HTTP.sys 提供的其中一個優點是核心模式快取。 如果回應是在核心模式快取中，則您可以完全從核心模式滿足 HTTP 要求，這會大幅降低處理要求的 CPU 成本。 但是，IIS 10.0 的核心模式快取是以實體記憶體為基礎，而專案的成本則是它所佔用的記憶體。

快取中的專案只有在使用時才有説明。 不過，無論專案是否正在使用，專案一律都會耗用實體記憶體。 您必須評估快取中某個專案的實用性 (可從快取) 中提供服務的節省成本，以及在專案存留期內，藉由考慮可用資源 (CPU 和實體記憶體) 和工作負載需求，來節省 (物理) 記憶體的成本。 HTTP.sys 會嘗試只在快取中保留有用且主動存取的專案，但您可以藉由微調特定工作負載的 HTTP.sys 快取，來提高網頁伺服器的效能。

以下是 HTTP.sys 核心模式快取的一些實用設定：

-   **UriEnableCache** 預設值：1

    非零值會啟用核心模式回應和片段快取。 針對大部分的工作負載，快取應該保持啟用狀態。 如果您預期會有非常低的回應和片段快取，請考慮停用快取。

-   **UriMaxCacheMegabyteCount** 預設值：0

    非零值，指定核心模式快取可用的最大記憶體。 預設值為0，可讓系統自動調整快取可使用的記憶體數量。

    **注意** 指定大小只會設定最大值，而系統可能無法讓快取成長到設定的大小上限。

    Â 

-   **UriMaxUriBytes** 預設值：262144位元組 (256 KB) 

    核心模式快取中專案的大小上限。 不會快取大於此的回應或片段。 如果您有足夠的記憶體，請考慮增加限制。 如果記憶體有限，且大型專案根本較小，則降低限制可能很有説明。

-   **UriScavengerPeriod** 預設值：120秒

    清除程式會定期掃描 HTTP.sys 快取，而不會移除清除程式掃描之間的專案。 將清除程式期間設定為較高的值，可減少清除程式掃描的次數。 不過，快取記憶體使用量可能會增加，因為較舊且較不常存取的專案可能會保留在快取中。 將期間設定得太低會造成更頻繁的清除和快取變換。

## <a name="request-and-connection-management-settings"></a>要求和連接管理設定

在 Windows Server 2016 中，HTTP.sys 會自動管理連接。 以下是不再使用的登錄設定：

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

本節中的設定會影響 IISÂ10.0 背景工作進程的行為。 您可以在下列 XML 設定檔中找到大部分的設定：

% SystemRoot% \\ system32 \\ inetsrv \\ config \\applicationHost.config

使用 Appcmd.exe、IIS 10.0 管理主控台、WebAdministration 或 IISAdministration PowerShell Cmdlet 來變更它們。 系統會自動偵測到大部分的設定，而且不需要重新開機 IIS 10.0 背景工作進程或 web 應用程式伺服器。 如需 applicationHost.config 檔案的詳細資訊，請參閱 [ApplicationHost.config簡介 ](https://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)。


## <a name="ideal-cpu-setting-for-numa-hardware"></a>NUMA 硬體的理想 CPU 設定

從 Windows 2016 開始，IIS 10.0 可針對其執行緒集區執行緒支援自動理想的 CPU 指派，以增強 NUMA 硬體的效能和擴充性。 預設會啟用這項功能，並可透過下列登錄機碼設定：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

啟用這項功能時，IIS 執行緒管理員會根據目前的負載，盡力將 IIS 執行緒集區執行緒平均分配到所有 NUMA 節點中的所有 Cpu。 一般情況下，建議您將此預設設定保留為不變更 NUMA 硬體。

**注意**  理想的 CPU 設定與背景工作進程 NUMA 節點指派設定不同 (numaNodeAssignment 和 numaNodeAffinityMode) 在[應用程式集區的 CPU 設定](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu)中引進。 理想的 CPU 設定會影響 IIS 如何散發其執行緒集區執行緒，而背景工作進程 NUMA 節點指派設定則決定背景工作進程啟動的 NUMA 節點。

## <a name="user-mode-cache-behavior-settings"></a>使用者模式快取行為設定

本節說明在 IISÂ10.0 中影響快取行為的設定。 使用者模式快取會實作為模組，以接聽整合管線所引發的全域快取事件。 若要完全停用使用者模式快取，請從 applicationHost.config 的 System.webserver/globalModules configuration 區段中的已安裝模組清單中移除 FileCacheModule ( # A0) 模組。

**System.webserver/caching**

| 屬性 | 描述 | 預設 |
|--|--|--|
| 啟用 | 當設定為 **False**時，會停用使用者模式 IIS 快取。 當快取命中率很少時，您可以完全停用快取，以避免與快取程式碼路徑相關聯的額外負荷。 停用使用者模式快取並不會停用核心模式快取。 | 是 |
| enableKernelCache | 當設定為 **False**時，停用核心模式快取。 | 是 |
| maxCacheSize | 將 IIS 使用者模式快取大小限制為指定的大小（以 Mb 為單位）。 IIS 會根據可用記憶體來調整預設值。 根據經常存取之檔案的大小和 RAM 數量或 IIS 進程位址空間，謹慎地選擇值。 | 0 |
| maxResponseSize | 快取最多指定大小的檔案。 實際的值取決於資料集中最大的檔案數目和大小，以及可用的 RAM。 快取大型、經常要求的檔案可能會降低 CPU 使用量、磁片存取以及相關聯的延遲。 | 262144 |

## <a name="compression-behavior-settings"></a>壓縮行為設定

從7.0 開始的 IIS 預設會壓縮靜態內容。 此外，在安裝 DynamicCompressionModule 時，預設會啟用動態內容的壓縮。 壓縮可減少頻寬使用量，但會增加 CPU 使用量。 如果可能的話，會在核心模式快取中快取已壓縮的內容。 從8.5 開始，IIS 可讓您單獨針對靜態和動態內容控制壓縮。 靜態內容通常是指不會變更的內容，例如 GIF 或 HTM 檔案。 動態內容通常是由伺服器上的腳本或程式碼所產生，也就是 ASP.NET 網頁。 您可以自訂任何特定延伸模組的分類為靜態或動態。

若要完全停用壓縮，請從 applicationHost.config 的 System.webserver/globalModules 區段中的模組清單中移除 StaticCompressionModule 和 DynamicCompressionModule。

**System.webserver/HTTPCompression**

| 屬性 | 描述 | 預設 |
|--|--|--|
| staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage | 如果目前百分比 CPU 使用量超過或低於指定的限制，則啟用或停用壓縮。<br><br>從 IIS 7.0 開始，如果穩定狀態 CPU 增加超過停用臨界值，就會自動停用壓縮。 如果 CPU 低於啟用閾值，則會啟用壓縮。 | 分別是50、100、50和90 |
| directory | 指定暫時儲存和快取壓縮版本之靜態檔案的目錄。 如果經常存取此目錄，請考慮將它移出系統磁片磁碟機。 | %SystemDrive%\inetpub\temp\IIS Temporary Compressed Files |
| doDiskSpaceLimiting | 指定所有壓縮檔案可佔用的磁碟空間是否有限制。 壓縮檔案會儲存在 **目錄** 屬性所指定的壓縮目錄中。 | 是 |
| maxDiskSpaceUsage | 指定壓縮檔案可在壓縮目錄中佔用的磁碟空間位元組數目。<br><br>如果所有壓縮內容的總大小過大，則可能需要增加這項設定。 | 100 MB |

**System.webserver/檔案 >urlcompression>**

| 屬性 | 描述 | 預設 |
|--|--|--|
| doStaticCompression | 指定是否壓縮靜態內容。 | 是 |
| doDynamicCompression | 指定是否壓縮動態內容。 | 是 |

> [!NOTE]
> 針對執行具有低平均 CPU 使用率之 IIS 10.0 的伺服器，請考慮啟用動態內容的壓縮，特別是當回應很大時。 這應該會先在測試環境中完成，以評估基準中 CPU 使用量的影響。

### <a name="tuning-the-default-document-list"></a>調整預設檔案清單

預設的檔模組會處理目錄根目錄的 HTTP 要求，並將其轉譯為特定檔案的要求，例如 Default.htm 或 Index.htm。 平均來說，aroundÂ網際網路上所有要求的25% 會通過預設檔路徑。 針對個別網站，這會有很大的差異。 當 HTTP 要求未指定檔案名時，預設的檔模組會在檔案系統中搜尋每個名稱的允許預設檔案清單。 這可能會對效能造成不良影響，特別是當到達內容需要進行網路往返或觸及磁片時。

您可以選擇性地停用預設檔，以及減少或排序檔案清單，以避免額外負荷。 針對使用預設檔的網站，您應該將清單縮減為僅限使用的預設檔案類型。 此外，請排序清單，使其開頭為最常存取的預設檔檔案名。

您可以在 applicationHost.config 中自訂位置標記內的設定，或直接在內容目錄中插入 web.config 檔案，以選擇性地設定特定 Url 的預設檔行為。 這允許混合式方法，只在必要時才啟用預設檔，並將清單設定為每個 URL 的正確檔案名。

若要完全停用預設檔，請從 applicationHost.config 的 System.webserver/globalModules 區段中的模組清單中移除 DefaultDocumentModule。

**System.webserver/defaultDocument**

| 屬性 | 描述 | 預設 |
|--|--|--|
| 已啟用 | 指定啟用預設檔。 | 是 |
| `<files>` 項目 | 指定設定為預設檔的檔案名。 | 預設清單是 Default.htm、預設的 asp、Index.htm、Index.html、Iisstart.htm 和 default.aspx。 |

## <a name="central-binary-logging"></a>集中式二進位記錄

當伺服器會話中有多個 URL 群組時，為個別的 URL 群組建立數百個格式化記錄檔，並將記錄資料寫入磁片的程式，可快速取用寶貴的 CPU 和記憶體資源，進而建立效能和擴充性問題。 集中式二進位記錄可將用於記錄的系統資源量降至最低，同時也能為需要的組織提供詳細的記錄資料。 剖析二進位格式的記錄檔需要進行後置處理工具。

您可以藉由將 centralLogFileMode 屬性設定為 CentralBinary，並將 [ **enabled** ] 屬性設定為 [ **True**]，來啟用中央二進位記錄。 請考慮將中央記錄檔的位置從系統磁碟分割移至專用的記錄磁片磁碟機，以避免系統活動與記錄活動之間的爭用。

**Applicationhost.config/記錄檔**

| 屬性 | 描述 | 預設 |
|--|--|--|
| centralLogFileMode | 指定伺服器的記錄模式。 將此值變更為 CentralBinary，以啟用中央二進位記錄。 | 網站 |

**Applicationhost.config/log/centralBinaryLogFile**

| 屬性 | 描述 | 預設 |
|--|--|--|
| 已啟用 | 指定是否啟用中央二進位記錄。 | 否 |
| directory | 指定寫入記錄專案的目錄。 | %SystemDrive%\inetpub\logs\LogFiles |

## <a name="application-and-site-tunings"></a>應用程式和網站並

下列設定與應用程式集區和網站並相關。

**Applicationhost.config/applicationPools/applicationPoolDefaults**

| 屬性 | 描述 | 預設 |
|--|--|--|
| queueLength | 表示在拒絕未來要求之前，HTTP.sys 應用程式集區的佇列要求數。 當超過這個屬性的值時，IIS 會拒絕後續的要求，並出現503錯誤。<br><br>如果觀察到503錯誤，請考慮為與高延遲後端資料存放區通訊的應用程式增加這一點。 | 1000 |
| enable32BitAppOnWin64 | 若為 True，可讓32位應用程式在具有64位處理器的電腦上執行。<br><br>如果需要考慮記憶體耗用量，請考慮啟用32位模式。 因為指標大小和指令大小較小，所以32位應用程式所使用的記憶體比64位應用程式少。 在64位電腦上執行32位應用程式的缺點是使用者模式位址空間的限制為 4 GB。 | 否 |

**Applicationhost.config/sites/VirtualDirectoryDefault**

| 屬性 | 描述 | 預設 |
|--|--|--|
| allowSubDirConfig | 指定 IIS 是否要在內容目錄中尋找低於目前層級的 web.config 檔案 (True) 或找不到低於目前層級 (False) 之內容目錄中的 web.config 檔。 藉由提供簡單的限制（只允許虛擬目錄中的設定），IISÂ10.0 可以知道，除非** / &lt; &gt; .htm**是虛擬目錄，否則不應該尋找設定檔。 略過其他檔案作業可大幅改善擁有一組非常龐大隨機存取靜態內容之網站的效能。 | 是 |

## <a name="managing-iis-100-modules"></a>管理 IIS 10.0 模組

IIS 10.0 已分解為多個使用者可延伸的模組，以支援模組化結構。 此分解的成本較低。 針對每個模組，整合式管線必須針對與模組相關的每個事件呼叫模組。 無論模組是否必須執行任何工作，都會發生這種情況。 您可以藉由移除與特定網站無關的所有模組，來節省 CPU 週期和記憶體。

針對簡單靜態檔案調整的 web 伺服器，可能只會包含下列五個模組： UriCacheModule、HttpCacheModule、StaticFileModule、AnonymousAuthenticationModule 和 HttpLoggingModule。

若要從 applicationHost.config 移除模組，除了移除 system.webserver/globalModules 中的模組宣告之外，請從 system.webserver/處理常式和 system.webserver/module 區段中移除模組的所有參考。

## <a name="classic-asp-settings"></a>傳統 ASP 設定

處理傳統 ASP 要求的主要成本包括初始化腳本引擎、將要求的 ASP 腳本編譯成 ASP 範本，以及在腳本引擎上執行範本。 雖然範本執行成本取決於要求的 ASP 腳本的複雜度，但 IIS 傳統 ASP 模組可將腳本引擎快取在記憶體和磁片中的記憶體和快取範本 (只有在記憶體內部) 範本快取溢位時，才會將腳本引擎快取在記憶體和磁片中，以提升 CPU 界限案例的效能

下列設定可用來設定傳統 ASP 範本快取和腳本引擎快取，而不會影響 ASP.NET 設定。

**System.webserver/asp/cache**

| 屬性 | 描述 | 預設 |
|--|--|--|
| diskTemplateCacheDirectory | 當記憶體中的快取溢位時，ASP 用來儲存已編譯範本的目錄名稱。<br><br>建議：設定為未使用的目錄（例如，未與作業系統、IIS 記錄檔或其他經常存取的內容共用的磁片磁碟機）。 | %SystemDrive%\inetpub\temp\ASP Compiled Templates |
| maxDiskTemplateCacheFiles | 指定可在磁片上快取之已編譯 ASP 範本的最大數目。<br><br>建議：設定為0x7FFFFFFF 的最大值。 | 2000 |
| scriptFileCacheSize | 這個屬性會指定可在記憶體中快取之已編譯 ASP 範本的最大數目。<br><br>建議：至少設定為應用程式集區所提供的經常要求 ASP 腳本數目。 可能的話，請將設定為記憶體限制允許的 ASP 範本數目。 | 500 |
| scriptEngineCacheMax | 指定將在記憶體中保留快取的腳本引擎數目上限。<br><br>建議：至少設定為應用程式集區所提供的經常要求 ASP 腳本數目。 可能的話，請盡可能將設定為記憶體限制所允許的腳本引擎數目。 | 250 |

**System.webserver/asp/限制**

| 屬性 | 描述 | 預設 |
|--|--|--|
| processorThreadMax | 指定 ASP 可建立的每個處理器背景工作執行緒數目上限。 如果目前的設定不足以處理負載，可能會在服務要求時造成錯誤，或導致 CPU 資源無法使用時，增加。 | 25 |

**System.webserver/asp/comPlus**

| 屬性 | 描述 | 預設 |
|--|--|--|
| executeInMta | 如果在 IIS 提供 ASP 內容時偵測到錯誤或失敗，則設定為 **True** 。 例如，裝載多個隔離的網站時，會在每個網站的背景工作進程下執行。 錯誤通常是從事件檢視器中的 COM + 回報。 這項設定會啟用 ASP 中的多執行緒單元模型。 | 否 |

## <a name="aspnet-concurrency-setting"></a>ASP.NET 並行設定

### <a name="aspnet-35"></a>ASP.NET 3.5
依預設，ASP.NET 會限制要求平行存取，以減少伺服器上的穩定狀態記憶體耗用量。 高平行存取應用程式可能需要調整某些設定，以改善整體效能。 您可以在 aspnet.config 檔案中變更此設定：

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

下列設定適用于完整地使用系統上的資源：

-   **maxConcurrentRequestPerCpu** 預設值：5000

    這項設定會限制系統上同時執行之 ASP.NET 要求的最大數目。 預設值是保守的，可減少 ASP.NET 應用程式的記憶體耗用量。 請考慮在執行執行長時間同步 i/o 作業的應用程式的系統上增加此限制。 否則，使用者可能會因為佇列或要求失敗而遇到高延遲，因為在使用預設設定時，會因為高負載而超出佇列限制。

### <a name="aspnet-46"></a>ASP.NET 4。6
除了 maxConcurrentRequestPerCpu 設定之外，ASP.NET 4.7 也提供設定，以改善大量依賴非同步作業的應用程式效能。 您可以在 aspnet.config 檔案中變更此設定。

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit** 預設值：90當大量載入 (超過硬體功能) 的情況下，非同步要求會有一些擴充性問題。 問題是因為非同步案例的配置本質所致。 在這些情況下，會在非同步作業啟動時進行配置，並在完成時使用。 屆時，itâs 很可能會將物件移至第1代或第2代的 GC。 發生這種情況時，增加負載將會顯示每秒要求增加 (rps) 到點為止。 一旦通過這個時間點，花費在 GC 的時間就會開始成為問題，而 rps 會開始 dip，並有負面的調整效果。 若要修正此問題，當 cpu 使用量超過 percentCpuLimit 設定時，就會將要求傳送至 ASP.NET 原生佇列。
-   **percentCpuLimitMinActiveRequestPerCpu** 預設值： 100 CPU 節流 (percentCpuLimit 設定) 不是以要求數目為基礎，而是以要求的成本為基礎。 如此一來，就可能只有幾個需要大量 CPU 的要求會導致原生佇列中的備份，而不會在連入要求旁邊清空。 若要解決此 problme，percentCpuLimitMinActiveRequestPerCpu 可以用來確保在節流開始之前，會提供最少的要求數目。

## <a name="worker-process-and-recycling-options"></a>背景工作進程和回收選項

您可以設定回收 IIS 工作者進程的選項，並在不需要介入或重設服務或電腦的情況下，為銳的情況或事件提供實際的解決方案。 這類情況和事件包括記憶體流失、增加記憶體負載，或沒有回應或閒置的工作者進程。 在一般情況下，可能不需要回收選項，而且回收可以關閉，或者系統可以設定為不常回收。

您可以將屬性新增至 **回收/periodicRestart** 元素，以啟用特定應用程式的進程回收。 回收事件可由數個事件觸發，包括記憶體使用量、固定的要求數目，以及固定的時間週期。 當背景工作進程回收時，已排入佇列和執行中的要求會被清空，並同時啟動新的進程來服務新的要求。 **回收/periodicRestart**元素是個別應用程式，這表示下表中的每個屬性都會以每個應用程式為基礎進行分割。

**Applicationhost.config/applicationPools/ApplicationPoolDefaults/回收/periodicRestart**

| 屬性 | 描述 | 預設 |
|--|--|--|
| 記憶體 | 如果虛擬記憶體耗用量超過指定的限制（以 kb 為單位），則啟用進程回收。 對於具有小型、2 GB 位址空間的32位電腦而言，這是很有用的設定。 這有助於避免因為記憶體不足錯誤而失敗的要求。 | 0 |
| privateMemory | 如果私人記憶體配置超過指定的限制（以 kb 為單位），則啟用進程回收。 | 0 |
| requests | 在特定數目的要求之後，啟用進程回收。 | 0 |
| time | 在指定的時間週期後啟用進程回收。 | 29:00:00 |

## <a name="dynamic-worker-process-page-out-tuning"></a>動態背景工作進程頁面時微調

從 Windows Server 2012 R2 開始，IIS 提供選項，讓背景工作進程在閒置一段時間之後暫停， (除了在 IIS 7) 之後存在的 terminate 選項之外。

閒置背景工作進程頁面輸出和閒置背景工作進程終止功能的主要目的，是為了節省伺服器上的記憶體使用量，因為網站可能會耗用大量的記憶體，即使它剛剛坐在接聽。 根據在網站上使用的技術 (靜態內容和其他架構) ，所使用的記憶體可能會從大約 10 MB 到數百 MB，而且這表示如果您的伺服器設定了許多網站，則為網站找出最有效的設定，可大幅提升作用中和已擱置網站的效能。

在我們進入詳細資訊之前，我們必須記住，如果沒有記憶體限制，則最好只將網站設定為永不暫停或終止。 畢竟，如果在電腦上是唯一的，則在終止工作者進程時，thereâs 很少的價值。

> [!NOTE]
> 如果網站執行不穩定的程式碼（例如記憶體流失或不穩定的程式碼），將網站設定為在閒置時終止可能是修正程式碼錯誤的一種快速又中途的替代方案。 這並不是我們建議的做法，但在 crunch 中，使用這項功能做為清除機制可能會比較好，而更永久的解決方案則在運作中。\]

另一個要考慮的因素是，如果網站使用大量的記憶體，則暫停程式本身會收取費，因為電腦必須將工作者進程使用的資料寫入磁片。 如果背景工作進程使用大量的記憶體，則暫停它的成本可能會比必須等待它開始備份的成本更高。

若要使背景工作進程暫止的功能發揮最大效果，您需要在每個應用程式集區中檢查您的網站，並決定應該暫停的網站、應終止的位置，以及應該無限期地使用的網站。 針對每個動作和每個網站，您需要找出理想的超時時間。

在理想的情況下，您將設定暫停或終止的網站是每天都有訪客的網站，但不足以保證隨時保持在作用中狀態。 這些通常是大約20個唯一訪客在一天以內的網站。 您可以使用網站的記錄檔來分析流量模式，並計算每日平均流量。

請記住，一旦特定使用者連線到網站，他們通常至少會保留一段時間，提出額外的要求，因此只要計算每日要求，就可能無法正確反映實際的流量模式。 若要取得更精確的閱讀，您也可以使用工具（例如 Microsoft Excel）來計算要求之間的平均時間。 例如：

| Number | 要求 URL | 要求時間 | 差異 |
|--|--|--|--|
| 1 | /SourceSilverLight/Geosource.web/grosource.html | 10:01 |  |
| 2 | /SourceSilverLight/Geosource.web/sliverlight.js | 10:10 | 0:09 |
| 3 | /SourceSilverLight/Geosource.web/clientbin/geo/1.aspx | 10:11 | 0:01 |
| 4 | /lClientAccessPolicy.xml | 10:12 | 0:01 |
| 5 | /SourceSilverLight/GeosourcewebService/服務 .asmx | 10:23 | 0:11 |
| 6 | /SourceSilverLight/Geosource. web/GeoSearchServer ... ¦。 | 11:50 | 1:27 |
| 7 | /rest/Services/CachedServices/Silverlight_load_la ... ¦ | 12:50 | 1:00 |
| 8 | /rest/Services/CachedServices/Silverlight_basemap ... ¦。 | 12:51 | 0:01 |
| 9 | /rest/Services/DynamicService/Silverlight_basemap ... ¦。 | 12:59 | 0:08 |
| 10 | /rest/Services/CachedServices/Ortho_2004_cache .。。 | 13:40 | 0:41 |
| 11 | /rest/Services/CachedServices/Ortho_2005_cache.js | 13:40 | 0:00 |
| 12 | /rest/Services/CachedServices/OrthoBaseEngine.aspx | 13:41 | 0:01 |

不過，困難的部分是找出要套用的設定有意義的。 在我們的案例中，網站會取得來自使用者的一堆要求，而上表顯示在4小時期間內，總共有4個唯一會話發生。 使用應用程式集區之背景工作進程暫止的預設設定時，網站會在預設的時間超過20分鐘之後終止，這表示每一位使用者都會經歷網站的啟動迴圈。 這讓它成為工作者進程暫止的理想候選，因為在大部分的情況下，該網站會閒置，因此暫停它會節省資源，並讓使用者幾乎可以立即存取網站。

最後一點很重要的一點是，磁片效能對於這項功能很重要。 由於暫止和喚醒程式牽涉到將大量資料寫入和讀取硬碟，因此強烈建議您使用快速磁片來進行這項操作。 固態硬碟 (Ssd) 是理想的選擇，而且強烈建議您這麼做，因為在 SSD 上未安裝作業系統本身，因此您應該確定 Windows 分頁檔已儲存在該磁片上 (如果該作業系統本身未安裝在 SSD 上，請設定作業系統將分頁檔移至) 。

無論您使用的是 SSD，我們也建議您修正分頁檔案的大小，以配合將頁面輸出資料寫入其中，而不需要進行檔案調整大小。 當作業系統需要將資料儲存在分頁檔時，可能會發生分頁檔調整大小，因為根據預設，Windows 會設定為根據需求自動調整其大小。 藉由將大小設定為固定值，您可以避免調整大小及改善效能。

若要設定預先修正的頁面檔案大小，您必須計算其理想大小，這取決於您要暫停的網站數量，以及它們所使用的記憶體數量。 如果使用中的背景工作進程的平均為 200 MB，而且您在即將暫停的伺服器上有500個網站，則分頁檔案的大小至少應為 (200 \* 500) MB， (因此範例) 中的基底 + 100 GB。

> [!NOTE]
> 當網站暫止時，每個網站都會耗用大約 6 MB，因此在我們的案例中，如果所有網站都已暫止，記憶體使用量大約是 3 GB。 但事實上，youâre 可能永遠不會同時暫停它們。

## <a name="transport-layer-security-tuning-parameters"></a>傳輸層安全性微調參數

使用傳輸層安全性 (TLS) 會造成額外的 CPU 成本。 最昂貴的 TLS 元件是建立會話建立的成本，因為它牽涉到完整的交握。 重新連接、加密和解密也會增加成本。 為了提供更好的 TLS 效能，請執行下列動作：

-   針對 TLS 會話啟用 HTTP 保持連接。 這可消除會話建立成本。

-   在適當的情況下重複使用會話，特別是在非 keep-alive 流量的情況下。

-   選擇性地將加密套用至需要它的網站頁面或部分，而不是整個網站。

> [!NOTE]
> - 較大的索引鍵可提供更多的安全性，但也會使用更多的 CPU 時間。
> - 所有元件可能不需要加密。 不過，混合純 HTTP 和 HTTPS 可能會導致快顯警告，指出頁面上的所有內容都不是安全的。

## <a name="internet-server-application-programming-interface-isapi"></a> (ISAPI) 的網際網路伺服器應用程式設計介面

ISAPI 應用程式不需要任何特殊的微調參數。 如果您撰寫私人 ISAPI 延伸模組，請確定它是針對效能和資源使用所撰寫的。

## <a name="managed-code-tuning-guidelines"></a>Managed 程式碼微調指導方針

IIS 10.0 中的整合式管線模型可提供高度的彈性和擴充性。 在原生或 managed 程式碼中執行的自訂模組可以插入管線中，也可以取代現有的模組。 雖然此擴充性模型提供便利性和簡單性，但是在插入新的受管理模組以連結到全域事件之前，您應該先小心。 新增全域受控模組表示所有要求（包括靜態檔案要求）都必須接觸 managed 程式碼。 自訂模組很容易發生垃圾收集之類的事件。 此外，自訂模組因為封送處理原生和 managed 程式碼之間的資料，而增加了大量的 CPU 成本。 可能的話，您應該針對受管理的模組設定 managedHandler 的先決條件。

若要取得更好的冷啟動效能，請務必先行編譯 ASP.NET 網站，或利用 IIS 應用程式初始化功能來準備應用程式。

如果不需要會話狀態，請確定您已針對每個頁面關閉它。

如果有許多 i/o 系結作業，請嘗試使用相關 Api 的非同步版本，這可提供您更好的擴充性。

此外，也會適當地使用輸出快取，以提升網站的效能。

當您在隔離模式中執行包含 ASP.NET 腳本的多部主機時 (每個網站) 一個應用程式集區，請監視記憶體使用量。 請確定伺服器具有足夠的 RAM，可滿足所需的同時執行應用程式集區數目。 請考慮使用多個應用程式域，而不是多個獨立的進程。

## <a name="other-issues-that-affect-iis-performance"></a>影響 IIS 效能的其他問題

下列問題可能會影響 IIS 效能：

-   安裝不是快取感知的篩選

    安裝不是 HTTP 快取感知的篩選會導致 IIS 完全停用快取，這會導致效能不佳。 在 IISÂ6.0 之前撰寫的 ISAPI 篩選器可能會導致此行為。

-   一般閘道介面 (CGI) 要求

    基於效能的考慮，建議您不要在 IIS 中使用 CGI 應用程式來提供要求。 經常建立和刪除 CGI 進程需要大量的負擔。 更好的替代方案包括使用 FastCGI、ISAPI 應用程式腳本和 ASP 或 ASP.NET 腳本。 每個選項都可以使用隔離。

## <a name="additional-references"></a>其他參考資料

- [Web 服務器效能調整](index.md)
- [HTTP 1.1/2 調整](http-performance.md)
