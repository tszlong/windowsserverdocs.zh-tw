---
title: 調整 IIS 10.0
description: 效能微調 recommmendations IIS 10.0 web 伺服器，在 Windows Server 16
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 16ea5c857d99e8a69f528e81178911236341dbb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869849"
---
# <a name="tuning-iis-100"></a>調整 IIS 10.0

使用 Windows Server 2016 包含 Internet Information Services (IIS) 10.0。 它會使用類似於 IIS 8.5 和 IIS 7.0 的處理序模型。 核心模式網路驅動程式 (http.sys) 接收和路由傳送 HTTP 要求，並從其回應快取來滿足。 背景工作處理序註冊 URL subspaces，http.sys 會將要求路由至適當程序 （或組的應用程式集區的程序）。

HTTP.sys 是負責連線管理和要求處理。 可以從 HTTP.sys 快取的要求，或傳遞至背景工作處理序以供進一步處理。 可以設定多個背景工作處理序，以提供隔離，降低成本。 如需詳細資訊如何要求處理運作方式，請參閱下圖：

![在 iis 10.0 中處理的要求](../../media/perftune-guide-iis-request-handling.png)

HTTP.sys 會包含回應快取。 當要求符合回應快取中的項目時，HTTP.sys 會直接從核心模式傳送快取回應。 某些 web 應用程式平台，例如 ASP.NET、 提供機制，以啟用任何快取中的核心模式快取的動態內容。 IIS 10.0 中的靜態檔案處理常式會自動快取經常要求的檔案，在 http.sys 中。

因為網頁伺服器核心模式和使用者模式元件，這兩個元件必須加以調整以獲得最佳效能。 因此，針對特定的工作負載調整 IIS 10.0，包括下列設定：

-   HTTP.sys 和相關聯的核心模式快取

-   背景工作處理序和使用者模式的 IIS，包括應用程式集區設定

-   影響效能的特定微調參數

下列各節討論如何設定 IIS 10.0 的核心模式和使用者模式的層面。

## <a name="kernel-mode-settings"></a>核心模式設定

效能相關的 HTTP.sys 設定分成兩大類： 快取管理以及連接和要求的管理。 所有的登錄設定會儲存在下列登錄項目：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Http\Parameters
```

**附註** 如果 HTTP 服務已在執行中，您必須重新啟動，讓變更生效。

Â 

## <a name="cache-management-settings"></a>快取管理設定

HTTP.sys 所提供的其中一個優點是核心模式快取。 如果回應是核心模式快取中，您就可以滿足完全從核心模式中，大幅降低 CPU 成本的處理要求的 HTTP 要求。 不過，IIS 10.0 的核心模式快取為基礎的實體記憶體，而且項目的成本是它會佔用的記憶體。

只有在使用時，快取中的項目是很有幫助。 不過，項目一律會耗用的實體記憶體，不論是否正在使用的項目。 您也必須考慮可用的資源 （CPU 和記憶體） 和工作負載之項目的存留期內評估的實用性的快取 （無法從快取中將其提供折扣），而且其成本 （所佔用的實體記憶體） 中的項目需求。 HTTP.sys 會嘗試保留僅有用的主動存取項目，快取，但是您可以藉由微調 HTTP.sys 快取中的特定工作負載增加 web 伺服器的效能。

以下是一些有用的設定 HTTP.sys 核心模式快取：

-   **UriEnableCache**預設值：1

    為非零的值會啟用核心模式回應和片段快取。 對於大部分的工作負載，快取應保持啟用。 請考慮停用快取，如果您預期的非常短的回應和片段快取。

-   **UriMaxCacheMegabyteCount**預設值：0

    非零值，指定所使用的核心模式快取的最大記憶體。 預設值為 0，可讓系統自動調整至快取的記憶體數量可用。

    **請注意**指定大小設定上限，以及系統可能不允許成長到設定最大大小的快取。

    Â 

-   **UriMaxUriBytes**預設值： 262144 位元組 (256 KB)

    核心模式快取中項目的大小上限。 回應或片段較大的值不會快取。 如果您有足夠的記憶體，可考慮加大此限制。 如果記憶體有限，且大型的項目會擠在一塊出較小的它可能有助於降低限制。

-   **UriScavengerPeriod**預設值：120 秒

    清除處理，定期掃描 HTTP.sys 快取並不會清除掃描間隔存取的項目會移除。 將清除處理期間設為較高的值減少清除處理掃描的次數。 不過，因為較舊且較不常存取的項目保留在快取中，可能會增加快取記憶體使用量。 設定期間太低會導致更頻繁清除處理掃描時，它可以和導致過多的排清快取變換。

## <a name="request-and-connection-management-settings"></a>要求與連線管理設定

在 Windows Server 2016 中，HTTP.sys 會自動管理連線。 無法再使用下列登錄設定：

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

在本節中的設定會影響 IISÂ 10.0 背景工作處理序行為。 大部分的這些設定可在下列的 XML 組態檔中：

%SystemRoot%\\system32\\inetsrv\\config\\applicationHost.config

使用 Appcmd.exe，IIS 10.0 管理主控台、 WebAdministration 或適 PowerShell Cmdlet 來變更它們。 大部分的設定會自動偵測，並不需要重新啟動 IIS 10.0 背景工作處理序或 web 應用程式伺服器。 如需詳細的 applicationHost.config 檔案的詳細資訊，請參閱[ApplicationHost.config 簡介](http://www.iis.net/learn/get-started/planning-your-iis-architecture/introduction-to-applicationhostconfig)。


## <a name="ideal-cpu-setting-for-numa-hardware"></a>理想的 NUMA 硬體的 CPU 設定

從 Windows 2016 開始，IIS 10.0 支援自動提升效能和延展性，在 NUMA 硬體上的其執行緒集區執行緒的理想 CPU 指派。 這項功能預設為啟用，並可以透過下列登錄機碼來設定：

``` syntax
HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\InetInfo\Parameters\ThreadPoolUseIdealCpu
```

啟用此功能，IIS 執行緒管理員會對其平均分散到所有其目前的負載為基礎的 NUMA 節點中的所有 Cpu IIS 的執行緒集區的最大努力。 一般情況下，建議您保留此設定保持不變的 NUMA 硬體的預設值。

**附註** 理想的 CPU 設定與不同的背景工作處理序 NUMA 節點指派設定 （numaNodeAssignment 和 numaNodeAffinityMode） 中導入[應用程式集區的 CPU 設定](https://www.iis.net/configreference/system.applicationhost/applicationpools/add/cpu)。 理想的 CPU 設定會影響如何 IIS 發佈其執行緒集區，而背景工作處理序 NUMA 節點指派設定可讓您判斷哪一個 NUMA 節點上啟動背景工作處理序。

## <a name="user-mode-cache-behavior-settings"></a>使用者模式快取行為設定

本章節說明影響 IISÂ 10.0 中的快取行為的設定。 使用者模式快取會實作為接聽全域快取所引發之事件的整合式管線的模組。 若要完全停用使用者模式快取，請移除 FileCacheModule (cachfile.dll) 模組從 applicationHost.config 中 system.webServer/globalModules 組態區段中的已安裝模組的清單。

**system.webServer/caching**

|屬性|描述|預設|
|--- |--- |--- |
|Enabled|停用 IIS 使用者模式的快取已設定為時**False**。 快取點擊率時很小，您可以停用快取完全避免快取程式碼路徑相關聯的額外負荷。 停用使用者模式快取不會停用核心模式快取。|True|
|enableKernelCache|停用時設為核心模式快取**False**。|True|
|maxCacheSize|IIS 使用者模式快取大小以 mb 為單位指定的大小限制。 IIS 會調整，根據可用記憶體的預設值。 選擇仔細經常根據一組的大小值存取的檔案以及 RAM 或 IIS 處理序位址空間的數量。|0|
|maxResponseSize|會快取檔案，但不超過指定的大小。 實際的值取決於可用的 RAM 與資料集中的最大檔案大小與數量。 快取大型、 經常要求的檔案可以減少 CPU 使用量、 磁碟存取，以及相關的延遲時間。|262144|

## <a name="compression-behavior-settings"></a>壓縮行為設定

IIS 7.0 中啟動依預設，壓縮靜態內容。 此外，將壓縮動態內容的啟用根據預設，安裝 DynamicCompressionModule 時。 壓縮可減少頻寬使用量，但是會增加 CPU 使用量。 壓縮的內容會快取中的核心模式快取的話。 8.5 從開始，IIS 可讓獨立控制靜態和動態內容壓縮。 靜態內容通常是指不會變更，例如 GIF 或 HTM 檔案的內容。 動態內容通常會產生指令碼或在伺服器上，也就是 ASP.NET 網頁的程式碼。 您可以自訂任何特定的擴充功能，做為靜態或動態的分類。

若要完全停用壓縮，請從 applicationHost.config 中的 [system.webServer/globalModules] 區段中的模組清單中移除 StaticCompressionModule 和 DynamicCompressionModule。

**system.webServer/httpCompression**

|屬性|描述|預設|
|--- |--- |--- |
|staticCompression-EnableCpuUsage<br><br>staticCompression-DisableCpuUsage<br><br>dynamicCompression-EnableCpuUsage<br><br>dynamicCompression-DisableCpuUsage|啟用或停用壓縮，如果目前的 CPU 使用量百分比高於或低於指定的限制。<br><br>從 IIS 7.0 開始，壓縮會自動停用如果穩定狀態 CPU 可以自行停用臨界值。 當 CPU 低於啟用臨界值時，會啟用壓縮。|50、 100、 50 和 90 分別|
|目錄|指定在其中壓縮的版靜態檔案會暫時儲存和快取的目錄。 請考慮移至此目錄系統磁碟機，如果經常存取。|%SystemDrive%\inetpub\temp\IIS temporary Compressed 的 Files|
|doDiskSpaceLimiting|指定是否限制存在多少磁碟空間的所有壓縮的檔案可以佔用。 壓縮的檔案會儲存在所指定的壓縮目錄**directory**屬性。|True|
|maxDiskSpaceUsage|壓縮目錄中指定的壓縮的檔案可以佔用的磁碟空間的位元組數目。<br><br>這項設定可能需要增加如果所有的壓縮內容的總大小太大。|100 MB|

**system.webServer/urlCompression**

|屬性|描述|預設|
|--- |--- |--- |
|doStaticCompression|指定是否要壓縮靜態內容。|True|
|doDynamicCompression|指定是否要壓縮動態內容。|True|

**請注意**對於伺服器執行 IIS 10.0 有低的平均 CPU 使用率，請考慮啟用壓縮動態內容，特別是如果回應是大型。 這第一次應該在測試環境中評估從基準線的 CPU 使用量影響。


### <a name="tuning-the-default-document-list"></a>調整預設文件清單

預設文件的模組會處理 HTTP 要求目錄的根目錄，並將它們轉譯成特定的檔案，例如 Default.htm 或 Index.htm 的要求。 平均而言，aroundÂ 25%，在網際網路上的所有要求的經歷預設文件路徑。 這個大幅變動針對個別站台。 當 HTTP 要求未指定檔案名稱時，預設文件的模組會搜尋每個名稱的允許的預設文件清單檔案系統中。 這可能會影響效能，特別是當觸達內容需要進行網路來回行程或 touching 磁碟。

選擇性地停用預設文件，並藉由減少或排序的文件清單，您可以避免額外負荷。 使用預設文件的網站，您應該只有預設文件類型，可縮減清單。 此外，順序清單，使它的開頭最常存取的預設文件檔案名稱。

藉由自訂 applicationHost.config 中的位置標記中的設定，或直接在內容的目錄中插入的 web.config 檔案，可以選擇性地在特定 Url 上設定預設文件的行為。 這可讓混合式方法，讓只有其中兩者就會視需要，將清單以正確的檔案名稱的每個 URL 設定的預設文件。

若要完全停用預設文件，請從 applicationHost.config 中的 [system.webServer/globalModules] 區段中的模組清單中移除 DefaultDocumentModule。

**system.webServer/defaultDocument**

|屬性|描述|預設|
|--- |--- |--- |
|enabled|指定啟用預設文件。|True|
|&lt;檔案&gt;項目|指定檔案名稱設定為預設文件。|提供 Default.htm，Default.asp、 Index.htm Index.html、 Iisstart.htm 和 Default.aspx 的預設清單。|

## <a name="central-binary-logging"></a>集中式二進位記錄

當伺服器工作階段有多個 URL 群組，其下的時，建立數百個格式化為個別的 URL 群組的記錄檔和記錄檔資料寫入磁碟的程序快速消耗寶貴的 CPU 和記憶體資源，藉此建立效能和延展性問題。 集中式二進位記錄降至最低記錄，在同時提供詳細的記錄資料的組織需要它時所使用的系統資源的量。 剖析的二進位檔格式記錄檔需要後置處理的工具。

您可以藉由將 centralLogFileMode 屬性 CentralBinary 以及設定啟用集中式二進位記錄**啟用**屬性設定為 **，則為 True**。 請考慮移中央的記錄檔關閉系統磁碟分割和到為了避免系統活動與記錄活動之間的爭用的專用的記錄磁碟機的位置。

**system.applicationHost/log**

|屬性|描述|預設|
|--- |--- |--- |
|centralLogFileMode|指定伺服器的記錄模式。 此值變更為 CentralBinary 啟用集中式二進位記錄。|站台|

**system.applicationHost/log/centralBinaryLogFile**

|屬性|描述|預設|
|--- |--- |--- |
|enabled|指定是否啟用集中式二進位記錄。|False|
|目錄|指定記錄項目會寫入其中的目錄。|%SystemDrive%\inetpub\logs\LogFiles|


## <a name="application-and-site-tunings"></a>應用程式和網站 tunings

應用程式集區和網站 tunings 與相關的下列設定。

**system.applicationHost/applicationPools/applicationPoolDefaults**

|屬性|描述|預設|
|--- |--- |--- |
|queueLength|向 HTTP.sys 指出多少要求已排入佇列的應用程式集區之前未來的要求會遭到拒絕。 當超過這個屬性的值時，IIS 會拒絕具有 503 錯誤的後續要求。<br><br>請考慮增加這與高延遲後端資料存放區進行通訊，如果發現 503 錯誤的應用程式。|1000|
|enable32BitAppOnWin64|若為 True，可讓具有 64 位元處理器的電腦上執行的 32 位元應用程式。<br><br>請考慮啟用 32 位元模式下，如果記憶體耗用量是個問題。 因為指標大小和指令的大小較小，32 位元應用程式會使用較少的記憶體比 64 位元應用程式。 若要在 64 位元電腦上執行 32 位元應用程式的缺點是限制為 4 GB 使用者模式位址空間。|False|

**system.applicationHost/sites/VirtualDirectoryDefault**

|屬性|描述|預設|
|--- |--- |--- |
|allowSubDirConfig|指定 IIS 會尋找低於目前的內容目錄中的 web.config 檔案層級 (True) 或不會尋找低於目前的層級 (False) 的內容目錄中的 web.config 檔案。 藉由加強是簡單的限制，允許只能在虛擬目錄中的設定，IISÂ 10.0 可以知道，除非 **/&lt;名稱&gt;.htm**是虛擬目錄，它不應該尋找組態檔。 略過的其他檔案作業，可以大幅改善效能，有非常大量的隨機存取的靜態內容的網站。|True|

## <a name="managing-iis-100-modules"></a>管理 IIS 10.0 模組

IIS 10.0 具有已重整到多個使用者可延伸的模組，以支援模組化的結構。 這個分解具有少許花費。 每個模組整合式的管線必須呼叫每個事件與模組相關的模組。 此時不論是否模組必須在其中執行任何工作。 您可以移除不相關的特定網站的所有模組，以節省 CPU 週期和記憶體。

微調簡單的靜態檔案的網頁伺服器可能只包括下列五個模組：UriCacheModule、 HttpCacheModule、 StaticFileModule、 AnonymousAuthenticationModule 和 HttpLoggingModule。

若要從 applicationHost.config 中移除模組，請移除模組 system.webServer/handlers 和 system.webServer/modules 除了 system.webServer/globalModules 中移除模組宣告區段中的所有參考。

## <a name="classic-asp-settings"></a>傳統的 ASP 設定

處理傳統的 ASP 要求的主要成本包括初始化指令碼引擎，編譯 ASP 範本中，要求的 ASP 指令碼和指令碼引擎上執行的範本。 雖然範本的執行成本取決於所要求的 ASP 指令碼的複雜度，但 IIS 傳統 ASP 模組可以快取在記憶體和磁碟中的記憶體與快取範本中的指令碼引擎 （只有在記憶體內部範本快取溢位），大幅提升效能CPU 繫結案例。

下列設定來設定傳統的 ASP 範本快取和指令碼引擎快取，並不會影響 ASP.NET 設定。

**system.webServer/asp/cache**

|屬性|描述|預設|
|--- |--- |--- |
|diskTemplateCacheDirectory|ASP 用到記憶體中快取溢位時儲存已編譯的範本之目錄的名稱。<br><br>建議：設為大量不是目錄使用，比方說，不會與作業系統、 IIS 記錄檔中，共用的磁碟機或其他經常存取的內容。|%SystemDrive%\inetpub\temp\ASP 編譯範本|
|maxDiskTemplateCacheFiles|在磁碟上，指定可以快取的已編譯 ASP 範本數目上限。<br><br>建議：設定為 0x7FFFFFFF 的最大值。|2000|
|scriptFileCacheSize|這個屬性會指定可以快取的已編譯 ASP 範本數目上限，在記憶體中。<br><br>建議：在設定最多達經常要求 ASP 指令碼由應用程式集區的數目。 可能的話，請設定為多個 ASP 範本，因為記憶體限制允許。|500|
|scriptEngineCacheMax|指定將保留在記憶體中快取的指令碼引擎數目上限。<br><br>建議：在設定最多達經常要求 ASP 指令碼由應用程式集區的數目。 可能的話，請設定為多個指令碼引擎的記憶體限制允許。|250|

**system.webServer/asp/limits**

|屬性|描述|預設|
|--- |--- |--- |
|processorThreadMax|指定每個處理器可以建立 ASP 的背景工作執行緒的數目上限。 如果目前的設定是權限不足無法處理負載，它在提供要求時，會導致錯誤或造成的 CPU 資源不足的使用量，增加。|25|

**system.webServer/asp/comPlus**

|屬性|描述|預設|
|--- |--- |--- |
|executeInMta|設定為 **，則為 True**如果 IIS 服務 ASP 內容時偵測到錯誤或失敗。 這可能是，比方說，當裝載多個隔離的網站，在每個站台執行它自己的背景工作處理序。 從 COM + 事件檢視器中，通常會回報錯誤。 此設定可讓 ASP 在多執行緒的 apartment 模型。|False|


## <a name="aspnet-concurrency-setting"></a>ASP.NET 並行處理設定

### <a name="aspnet-35"></a>ASP.NET 3.5
根據預設，ASP.NET 會限制要求並行存取，以減少伺服器上的穩定狀態的記憶體耗用量。 高並行存取的應用程式可能需要調整某些設定，以改善整體效能。 您可以變更此設定中 aspnet.config 檔案：

``` syntax
<system.web>
  <applicationPool maxConcurrentRequestsPerCPU="5000"/>
</system.web>
```

下列設定是可完全在系統上使用資源：

-   **maxConcurrentRequestPerCpu**預設值：5000

    此設定會限制並行執行的系統上的 ASP.NET 要求的最大數目。 預設值是保守減少的 ASP.NET 應用程式的記憶體耗用量。 請考慮增加此限制，在執行應用程式的執行長，同步 I/O 作業的系統上。 否則使用者可以因為佇列或要求失敗，因為超過佇列限制在高負載下的，會使用預設設定時遇到高度延遲。

### <a name="aspnet-46"></a>ASP.NET 4.6
除了 maxConcurrentRequestPerCpu 設定中，ASP.NET 4.7 也會提供設定以改善重度依賴非同步作業的應用程式的效能。 可以在 aspnet.config 檔中變更設定。

``` syntax
<system.web>
  <applicationPool percentCpuLimit="90" percentCpuLimitMinActiveRequestPerCpu="100"/>
</system.web>
```

-   **percentCpuLimit**預設值：大量負載 （除了的硬體功能） 會放在這種情況時，90 的非同步要求就會有一些延展性問題。 問題是由於非同步案例配置的本質。 在這些情況下，當非同步作業啟動時，會發生配置，而且會在完成時使用。 在該時間，是 s 很可能物件都已移至層代 1 或 2 的 GC。 當發生這種情況時，增加的負載將會顯示增加每秒 (rps) 到點為止的要求。 一旦我們通過該點，在 GC 中花費的時間就會開始變得有問題，而 rps 將開始 dip，需要調整產生負面影響。 若要修正此問題，，cpu 使用量超過 percentCpuLimit 設定時，要求會傳送至 ASP.NET 的原生佇列中。
-   **percentCpuLimitMinActiveRequestPerCpu**預設值：不以 100 CPU 節流 （percentCpuLimit 設定），要求的數目，但它們的昂貴程度。 如此一來，可能導致備份無法清空除了連入要求的原生佇列中的少數需要大量 CPU 的要求。 若要解決此 problme，percentCpuLimitMinActiveRequestPerCpu 可用來確保節流會在前所提供的要求的最小數目。

## <a name="worker-process-and-recycling-options"></a>背景工作處理序，與回收選項

您可以設定循環使用 IIS 工作者處理序的選項，並提供實用方案及嚴重狀況或事件不需要介入的情況下，或重設服務或電腦。 這類情況下，事件包含記憶體流失、 增加的記憶體載入或沒有回應或閒置的工作者處理序。 一般情況下，回收選項可能不需要和回收可以關閉或系統可以設定很少回收。

您可以讓處理序回收特定應用程式，藉由將屬性加入**回收/periodicRestart**項目。 可以觸發回收事件，包括記憶體使用量、 固定的數目的要求，以及一段固定的時間的幾個事件。 回收工作者處理序時，即會清空已排入佇列並執行要求，並才能服務新要求，同時啟動新的處理序。 **回收/periodicRestart**項目是每個應用程式，這表示在下表中的每個屬性針對每個應用程式分割。

**system.applicationHost/applicationPools/ApplicationPoolDefaults/recycling/periodicRestart**

|屬性|描述|預設|
|--- |--- |--- |
|memory|啟用處理序回收，如果虛擬記憶體耗用量超過指定的限制，以 kb 為單位。 這是很有用的設定的 32 位元電腦，小型組織、 2 GB 的位址空間。 它可協助避免因記憶體不足錯誤而失敗的要求。|0|
|privateMemory|如果私用記憶體配置超過指定的限制，以 kb 為單位可讓處理序回收。|0|
|requests|啟用處理序回收之後要求數目。|0|
|time|啟用處理序回收一段時間之後。|29:00:00|


## <a name="dynamic-worker-process-page-out-tuning"></a>微調動態背景工作處理序移出分頁

從 Windows Server 2012 R2 開始，IIS 會提供設定背景工作處理序的選項，若要暫停之後已閒置一段時間 （除了終止，因為 IIS 7 存在的選項）。

閒置工作者處理序頁面外和閒置工作者處理序終止功能的主要目的是記憶體的為了節省記憶體使用率的伺服器上，因為站台可以耗用大量，即使它記憶體的只記憶體的坐在那裡，接聽。 根據站台上使用的技術 (靜態內容與 ASP.NET 與其他架構)，使用的記憶體可以是任何一處大約 10 MB 到數百個 mb 為單位，也就是說，如果您的伺服器已與多站台，找出最有效的設定您的網站可以大幅會改善效能的作用中和擱置的網站。

在進入細節之前，我們必須請記住，如果不有任何記憶體條件約束，則可能是最佳只要設定 永遠不會擱置或終止的網站。 畢竟，本期 s 低價值終止背景工作角色處理如果它是唯一的電腦上。

**附註** 站台執行不穩定的程式碼，例如記憶體流失，程式碼或否則不穩定，設定站台上終止閒置可能問題的修正程式碼的應急替代項目。 這不是建議，但在處理中，可能是最好的運作方式的更多的永久性解決方案時，使用這項功能做為清除機制。\]

Â 

要考量的另一個因素是，如果站台會使用大量記憶體，然後暫止處理序本身會採用影響，因為電腦上已寫入至磁碟的工作者處理序所使用的資料。 如果背景工作處理序正在使用大量記憶體，然後在其上暫止可能比不必等候它開始備份的成本昂貴。

若要讓最好的背景工作處理序暫止 」 功能，您需要檢閱您的網站，在每個應用程式集區，並決定應該暫停，這應該會終止，它應該是作用中無限期。 如需每個動作和每個站台，您需要找出理想的逾時期限。

在理想情況下，您會設定暫止或終止的網站是指訪客每一天，但仍不足以保證會讓它保持使用中的所有時間。 這些通常是每天大約 20 唯一的訪客的站台或更少。 您可以分析的流量模式，使用站台的記錄檔，並計算平均每日流量。

請記住，一旦特定的使用者連線至該網站，就會通常維持在其上至少一段時間，提出其他要求，並因此只會計算每日的要求可能不會正確反映實際的流量模式。 若要取得更精確的讀取，您也可以使用的工具，例如 Microsoft Excel 中，來計算要求之間的平均時間。 例如: 

||要求 URL|要求時間|差異|
|--- |--- |--- |--- |
|1|/SourceSilverLight/Geosource.web/grosource.html|10:01||
|2|/SourceSilverLight/Geosource.web/sliverlight.js|10:10|0:09|
|3|/SourceSilverLight/Geosource.web/clientbin/geo/1.aspx|10:11|0:01|
|4|/lClientAccessPolicy.xml|10:12|0:01|
|5|/ SourceSilverLight/GeosourcewebService/Service.asmx|10:23|0:11|
|6|/ SourceSilverLight/Geosource.web/GeoSearchServer...¦.|11:50|1:27|
|7|/rest/Services/CachedServices/Silverlight_load_la...¦|12:50|1:00|
|8|/rest/Services/CachedServices/Silverlight_basemap...¦.|12:51|0:01|
|9|rest/Services/DynamicService/Silverlight_basemap...正確 」。|12:59|0:08|
|10|/rest/Services/CachedServices/Ortho_2004_cache.as...|13:40|0:41|
|11|/rest/Services/CachedServices/Ortho_2005_cache.js|13:40|0:00|
|12|/rest/Services/CachedServices/OrthoBaseEngine.aspx|13:41|0:01|

不過，困難的部分，找出要套用至有意義的設定為何。 在我們的案例中，站台會從使用者取得一連串的要求上, 表顯示總計的 4 個唯一的工作階段發生在 4 小時內。 應用程式集區的背景工作處理序暫止的預設設定，預設逾時的 20 分鐘的時間，這表示每個這些使用者所遇到的站台啟動週期之後，就會終止站台。 這使得適合背景工作處理序暫停，因為對於大部分的時間，網站處於閒置狀態，所以暫停它會節省資源，並讓使用者幾乎可立即連線到站台。

最後，也是非常重要的注意事項這是磁碟的效能是重要的這項功能。 因為寫入和讀取大量資料到硬碟機，牽涉到的暫止 」 和 「 喚醒程序，我們強烈建議使用這個的快速的磁碟。 固態硬碟 (Ssd) 理想且強烈建議這麼做，而且您應該要確定 Windows 分頁檔會儲存在其上 （如果 SSD 上未安裝作業系統本身，設定要將分頁檔移動到其中的作業系統）。

是否使用 SSD 與否，我們也建議修正的分頁檔大小以配合對其寫入移出分頁資料，但不調整檔案大小。 調整分頁檔大小可能會發生作業系統需要將資料儲存在分頁檔，因為根據預設，Windows 會設定自動調整其大小，根據需要。 藉由設定大小固定的一個，您可以防止調整大小，並很多改善效能。

若要設定預先固定的分頁檔大小，您要計算其理想大小，這取決於您的幾個站台將會暫停，以及它們所耗用多少記憶體。 如果平均作用中背景工作處理序為 200 MB，而且您有 500 的站台，將會暫止，在伺服器上則分頁檔應該至少 (200 \* 500) 的基底大小 MB 數的分頁檔 (因此基底 + 100 GB，在我們的範例)。

**請注意**站台會暫止時，它們會耗用大約是 6 MB，因此在我們的案例中，如果所有站台會暫止的記憶體使用量會大約 3 GB。 事實上，不過，您可能不會讓它們全部在同一時間暫止重新。

 
## <a name="transport-layer-security-tuning-parameters"></a>傳輸層安全性調整參數

使用傳輸層安全性 (TLS) 會施加額外的 CPU 成本。 TLS 的成本最高元件就是建立工作階段建立時，因為它牽涉到完整的交握的成本。 重新連線、 加密及解密也增加成本。 為了更好的 TLS 效能，執行下列作業：

-   針對 TLS 工作階段中啟用 HTTP 持續作用。 這會排除的工作階段建立成本。

-   重複使用工作階段時適當情況下，特別是使用保持連線的流量。

-   選擇性地加密僅適用於頁面或組件，而是要為整個站台的站台。

**注意**
-   較大的金鑰提供更高的安全性，但它們也會使用更多的 CPU 時間。

-   所有的元件可能不需要加密。 不過，一般的 HTTP 和 HTTPS 混用可能會導致警告頁面上的不是所有內容都安全的快顯視窗。

 
## <a name="internet-server-application-programming-interface-isapi"></a>網際網路伺服器應用程式發展介面 (ISAPI)

ISAPI 應用程式不需要任何特殊的微調參數。 如果您撰寫的私用的 ISAPI 延伸模組，請確定它會寫入效能與資源使用。

## <a name="managed-code-tuning-guidelines"></a>Managed 程式碼微調方針

IIS 10.0 中的整合式的管線模型可讓較高程度的彈性和擴充性。 在原生或 managed 程式碼中實作的自訂模組可以插入管線中，或可以取代現有的模組。 雖然這個擴充性模型提供了便利性] 和 [為了簡單起見，您應該要小心的情況，先將連結的新受管理的模組插入全域事件。 新增全域受管理的模組可讓您表示必須接觸所有要求，包括靜態檔案要求 managed 程式碼。 自訂模組都很容易事件，例如記憶體回收。 此外，自訂模組會加入重大的 CPU 成本，因為原生和 managed 程式碼之間封送處理資料。 可能的話，您應該設定前置條件為 managedHandler 受管理模組。

若要取得更佳的冷啟動效能，請確定您先行編譯 ASP.NET 網站或利用 IIS 應用程式初始化功能準備應用程式。

如果不需要工作階段狀態，請確定您關閉它針對每個頁面。

如果有許多的 I/O 繫結作業，請試著使用非同步版本的相關的 Api，可讓您更好的延展性。

也正確地使用輸出快取時，並也會大幅提升您網站的效能。

當您執行包含隔離模式 （一個應用程式集區每個站台） 中的 ASP.NET 指令碼的多部主機時，監視記憶體使用量。 請確定伺服器具有足夠的 RAM，同時執行應用程式集區預期的數目。 請考慮使用多個應用程式定義域，而不多個獨立的處理序。


## <a name="other-issues-that-affect-iis-performance"></a>其他問題會影響 IIS 效能

下列問題可能會影響 IIS 效能：

-   不知道此快取的篩選器的安裝

    安裝不是 HTTP 快取-感知的篩選條件會導致 IIS 完全停用快取，會導致效能不佳。 IISÂ 6.0 之前所撰寫的 ISAPI 篩選器可能會造成這種行為。

-   通用閘道介面 (CGI) 要求

    基於效能考量，來處理要求的 CGI 應用程式的使用建議不要使用 IIS。 經常建立和刪除 CGI 處理序牽涉到相當大的負擔。 好的替代方案包括使用 FastCGI，ISAPI 應用程式指令碼和 ASP 或 ASP.NET 的指令碼。 隔離是適用於每個選項。

# <a name="see-also"></a>另請參閱
- [Web 伺服器的效能微調](index.md) 
- [HTTP 1.1/2 微調](http-performance.md)
