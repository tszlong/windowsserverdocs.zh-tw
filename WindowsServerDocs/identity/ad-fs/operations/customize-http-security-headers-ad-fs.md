---
title: 使用 AD FS 自訂 HTTP 安全性回應標頭
description: 本檔 descirbes 如何自訂安全性標頭，以防止安全性弱點。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.openlocfilehash: ecb70f0b200ee1f143219fb6c88a15c5dc58a7d9
ms.sourcegitcommit: 00406560a665a24d5a2b01c68063afdba1c74715
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91716919"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>使用 AD FS 2019 自訂 HTTP 安全性回應標頭

為了防止常見的安全性弱點，並讓系統管理員能夠充分利用瀏覽器型保護機制的最新進展，AD FS 2019 新增了自訂 AD FS 所傳送之 HTTP 安全性回應標頭的功能。 這是透過引進兩個新的 Cmdlet 來達成： `Get-AdfsResponseHeaders` 和 `Set-AdfsResponseHeaders` 。

> [!NOTE]
> 自訂 HTTP 安全性回應標頭的功能 (使用指令程式) 的 CORS 標頭除外： `Get-AdfsResponseHeaders` 和 `Set-AdfsResponseHeaders` 已 backport 至 AD FS 2016。 您可以藉由安裝 [KB4493473](https://support.microsoft.com/help/4493473/windows-10-update-kb4493473) 和 [KB4507459](https://support.microsoft.com/help/4507459/windows-10-update-kb4507459)，將功能新增至您的 AD FS 2016。

在本檔中，我們將討論常用的安全性回應標頭，以示範如何自訂 AD FS 2019 傳送的標頭。

> [!NOTE]
> 本檔假設已安裝 AD FS 2019。


在討論頁首之前，讓我們看看幾個案例，讓系統管理員必須自訂安全性標頭

## <a name="scenarios"></a>案例
1. 系統管理員已啟用 [**HTTP Strict-傳輸-安全性 (HSTS) **](#http-strict-transport-security-hsts) (強制所有透過 HTTPS) 加密的連線，以使用可能遭到駭客攻擊的公用 wifi 存取點的 HTTP，來保護可能存取 web 應用程式的使用者。 他們想要藉由啟用子域的 HSTS，進一步加強安全性。
2. 系統管理員已設定 [**X 框架選項**](#x-frame-options) 回應標頭 (防止在 iFrame 中轉譯任何網頁，) 防止 clickjacked 網頁。 不過，他們需要自訂標頭值，因為有新的商務需求，可將 iFrame 中的資料 (顯示在具有不同來源 (網域) 的應用程式) 。
3. 系統管理員已啟用 [**X-XSS 保護**](#x-xss-protection) (防止跨腳本攻擊) 在瀏覽器偵測到跨腳本攻擊的情況下，淨化和封鎖網頁。 不過，他們必須自訂標頭，以允許頁面在經過清理後載入。
4. 系統管理員必須啟用 [**跨原始來源資源分享 (CORS) **](#cross-origin-resource-sharing-cors-headers) ，並在 AD FS 上設定來源 (網域) ，以允許單一頁面應用程式存取具有另一個網域的 web API。
5. 系統管理員已啟用 [**內容安全性原則 (CSP) **](#content-security-policy-csp) 標頭，藉由不允許任何跨網域要求來防止跨網站腳本和資料插入式攻擊。 不過，由於新的商務需求，他們必須自訂標頭，以允許網頁從任何來源載入影像，並將媒體限制為受信任的提供者。


## <a name="http-security-response-headers"></a>HTTP 安全性回應標頭
回應標頭會包含在 AD FS 傳送至網頁瀏覽器的連出 HTTP 回應中。 您可以使用 Cmdlet 來列出標頭， `Get-AdfsResponseHeaders` 如下所示。

![標頭回應](media/customize-http-security-headers-ad-fs/header1.png)

`ResponseHeaders`上述螢幕擷取畫面中的屬性會識別在每個 HTTP 回應中 AD FS 將包含的安全性標頭。 只有當 `ResponseHeadersEnabled` 設為 `True` (預設值) 時，才會傳送回應標頭。 值可以設定為 `False` ，以防止 AD FS 包括 HTTP 回應中的任何安全性標頭。 不過，這不是建議的做法。  若要這樣做，請使用下列各項：

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```

### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-傳輸-安全性 (HSTS) 
HSTS 是一種 web 安全性原則機制，可協助減輕同時具有 HTTP 和 HTTPS 端點之服務的通訊協定降級攻擊和 cookie 劫持。 其可讓網頁伺服器宣告：網頁瀏覽器 (或其他相符的使用者代理程式) 只能使用 HTTPS (而不能透過 HTTP 通訊協定) 與其互動。

Web 驗證流量的所有 AD FS 端點都會以獨佔方式透過 HTTPS 開啟。 如此一來，AD FS 可以有效地降低 HTTP Strict 傳輸安全性原則機制提供 (的威脅，因為 HTTP) 中沒有任何接聽程式，所以預設不會降級至 HTTP。 您可以藉由設定下列參數來自訂標頭：

- **最大壽命 = &lt; 到期 &gt; **時間–到期時間 (（秒）) 指定應僅使用 HTTPS 存取網站的時間長度。 預設值和建議值為31536000秒 (1 年) 。
- **includeSubDomains** –這是選擇性參數。 如果有指定，HSTS 規則也會套用至所有子域。

#### <a name="hsts-customization"></a>HSTS 自訂
依預設，標頭會啟用並 `max-age` 設定為1年; 不過，系統管理員可以修改 `max-age` (降低最大壽命值) 或透過 Cmdlet 啟用子域的 HSTS `Set-AdfsResponseHeaders` 。

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains"
```

範例：

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains"
 ```

依預設，標頭會包含在屬性中， `ResponseHeaders` 不過，系統管理員可以透過 Cmdlet 移除標頭 `Set-AdfsResponseHeaders` 。

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security"
```

### <a name="x-frame-options"></a>X 框架-選項
AD FS 預設不允許外部應用程式在執行互動式登入時使用 Iframe。 這樣做的目的是為了避免特定的網路釣魚攻擊樣式。 請注意，因為先前已建立的工作階段層級安全性，所以無法透過 iFrame 執行非互動式登入。

不過，在某些罕見的情況下，您可能會信任需要 iFrame 的互動式 AD FS 登入頁面的特定應用程式。 「X 框架-選項」標頭用於此用途。

此 HTTP 安全性回應標頭是用來與瀏覽器進行通訊，無論它是否可以在 &lt; 框架 iframe 中轉譯頁面 &gt; / &lt; &gt; 。 標頭可以設定為下列其中一個值：

- **拒絕** –畫面格中的頁面不會顯示。 這是預設值且為建議的設定。
- **sameorigin** –如果原點與網頁的原點相同，則頁面只會顯示在框架中。 除非所有上階也位於相同的來源，否則此選項不會很有用。
- **允許來源 <specified origin> **-只有在來源 (（例如，）時，頁面才會顯示在框架中 https://www 。com) 符合標頭中的特定來源。 受限制的瀏覽器可能會支援這種情況。

#### <a name="x-frame-options-customization"></a>X 框架-選項自訂
依預設，標頭會設定為 [拒絕];不過，系統管理員可以透過 Cmdlet 修改該值 `Set-AdfsResponseHeaders` 。
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>"
 ```

範例：

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com"
 ```

依預設，標頭會包含在屬性中， `ResponseHeaders` 不過，系統管理員可以透過 Cmdlet 移除標頭 `Set-AdfsResponseHeaders` 。

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options"
```

### <a name="x-xss-protection"></a>X-XSS-保護
當瀏覽器偵測到跨網站腳本 (XSS) 攻擊時，會使用此 HTTP 安全性回應標頭來防止載入網頁。 這就是所謂的 XSS 篩選。 標頭可以設定為下列其中一個值：

- **0** -停用 XSS 篩選。 不建議使用。
- **1** –啟用 XSS 篩選。 如果偵測到 XSS 攻擊，瀏覽器將會淨化頁面。
- **1; 模式 = 區塊** –啟用 XSS 篩選。 如果偵測到 XSS 攻擊，瀏覽器將會防止頁面呈現。 這是預設值且為建議的設定。

#### <a name="x-xss-protection-customization"></a>X-XSS-保護自訂
依預設，標頭會設為 1;mode = block;不過，系統管理員可以透過 Cmdlet 修改該值 `Set-AdfsResponseHeaders` 。

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>"
```

範例：

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1"
 ```

依預設，標頭會包含在屬性中， `ResponseHeaders` 不過，系統管理員可以透過 Cmdlet 移除標頭 `Set-AdfsResponseHeaders` 。

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection"
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>跨原始資源分享 (CORS) 標頭
Web 瀏覽器安全性可防止網頁從腳本內起始跨原始來源要求。 不過，有時候您可能會想要存取 (網域) 的其他來源中的資源。 CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。 使用 CORS，伺服器可以明確允許某些跨源要求，然而拒絕其他要求。

為了進一步瞭解 CORS 要求，我們將逐步解說一個案例，其中的單一頁面應用程式 (SPA) 需要使用不同的網域來呼叫 web API。 此外，讓我們考慮在 ADFS 2019 上設定 SPA 和 API，並將 AD FS 啟用 CORS，也就是 AD FS 可以識別 HTTP 要求中的 CORS 標頭、驗證標頭值，並在回應中包含適當的 CORS 標頭 (詳細資料，以瞭解如何在以下) 的 CORS 自訂區段中啟用和設定 2019 AD FS CORS。 範例流程：

1. 使用者透過用戶端瀏覽器存取 SPA，並重新導向至 AD FS auth endpoint 進行驗證。 因為 SPA 是針對隱含授與流程設定的，所以在成功驗證之後，要求會將存取 + 識別碼權杖傳回給瀏覽器。
2. 在使用者驗證之後，SPA 中包含的前端 JavaScript 會要求存取 web API。 要求會被重新導向至具有下列標頭的 AD FS：
    - 選項–描述目標資源的通訊選項
    - 來源–包含 web API 的來源
    - 存取控制-要求-方法-識別 HTTP 方法 (例如，在進行實際要求時要使用的刪除) 
    - 存取控制-要求-標頭-識別進行實際要求時要使用的 HTTP 標頭

   > [!NOTE]
   > CORS 要求類似于標準 HTTP 要求，不過，原始標頭的存在表示連入要求是 CORS 相關的。
3. AD FS 確認標頭中所含的 web API 來源已列于 [AD FS 中所設定的受信任來源 (詳細資料，說明如何修改 CORS 自訂中的信任來源] 區段) 。 AD FS 接著會以下列標頭回應：
    - 存取控制-允許-來源-與原始標頭中的值相同
    - 存取控制-------值與在存取控制-要求-方法標頭中的值相同
    - 存取-------------------value 與
4. 瀏覽器會傳送包含下列標頭的實際要求：
    - HTTP 方法 (例如，刪除) 
    - 來源–包含 web API 的來源
    - 包含在存取控制-Allow 標頭回應標頭中的所有標頭
5. 經過驗證後，AD FS 在存取控制-允許原始的回應標頭中包含 web API 網域 (原始) 來核准要求。
6. 包含存取控制-允許原始的標頭可讓瀏覽器繼續呼叫要求的 API。

#### <a name="cors-customization"></a>CORS 自訂
根據預設，CORS 功能將不會啟用;不過，系統管理員可以透過 AdfsResponseHeaders Cmdlet 來啟用此功能。

```PowerShell
Set-AdfsResponseHeaders -EnableCORS $true
 ```

若已啟用，系統管理員將能夠使用相同的 Cmdlet 來列舉受信任的來源清單。 例如，下列命令會允許來自來源 HTTPs 的 CORS 要求 **&#58;//example1.com** 和 **HTTPs&#58;//example1.com**。

```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com
 ```

> [!NOTE]
> 系統管理員可以在受信任的來源清單中包含 "*"，以允許來自任何來源的 CORS 要求，但由於安全性弱點，因此不建議使用此方法，如果有安全性弱點，則會提供警告訊息。

### <a name="content-security-policy-csp"></a> (CSP) 的內容安全性原則
此 HTTP 安全性回應標頭是用來防止瀏覽器不小心執行惡意內容，以防止跨網站腳本、點擊劫持和其他資料插入式攻擊。 不支援 CSP 的瀏覽器只會忽略 CSP 回應標頭。

#### <a name="csp-customization"></a>CSP 自訂
CSP 標頭的自訂牽涉到修改安全性原則，該原則會定義可針對網頁載入的資源瀏覽器。 預設安全性原則為

`Content-Security-Policy: default-src ‘self' ‘unsafe-inline' ‘'unsafe-eval'; img-src ‘self' data:;`

**預設的 src**指示詞可用來修改[-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)指示詞，而不會明確列出每個指示詞。 例如，在下列範例中，原則1和原則2相同。

原則 1
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'"
```

原則 2
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self'; img-src ‘self'; font-src 'self';
frame-src 'self'; manifest-src 'self'; media-src 'self';"
```

如果明確列出指示詞，則指定的值會覆寫針對預設 src 提供的值。 在下列範例中，img src 會將值視為 ' * ' (允許從任何) 來源載入影像，而其他-src 指示詞則會將值設為 ' self ' (限制為與網頁) 相同的來源。

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self'; img-src *"
```
您可以針對預設 src 原則定義下列來源：

- ' self '-指定此項會限制內容的來源，以載入至網頁的來源
- ' unsafe-inline '-在原則中指定此項可允許使用內嵌 JavaScript 和 CSS
- ' unsafe-eval '-在原則中指定此項，可允許使用文字至 JavaScript 機制，例如 eval
- ' none '-指定這會限制來自任何來源的內容以載入
- data：-指定資料： Uri 可讓內容建立者內嵌內嵌于檔中的小型檔案。 不建議使用。

> [!NOTE]
> AD FS 在驗證程式中使用 JavaScript，因此可以在預設原則中包含 ' unsafe-inline ' 和 ' unsafe-eval ' 來源來啟用 JavaScript。

### <a name="custom-headers"></a>自訂標頭
除了上述所列的安全性回應標頭 (HSTS、CSP、X 框架選項、X-XSS 保護和 CORS) ，AD FS 2019 提供設定新標頭的功能。

範例：設定新的標頭 "TestHeader"，並將值設定為 "TestHeaderValue"

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue"
 ```

一旦設定之後，新的標頭會在) 的 AD FS 回應 (fiddler 程式碼片段中傳送。

![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browser-compatibility"></a>Web 瀏覽器相容性
使用下表和連結，判斷哪些網頁瀏覽器與每個安全性回應標頭相容。

|HTTP 安全性回應標頭|瀏覽器相容性|
|-----|-----|
|HTTP Strict-傳輸-安全性 (HSTS) |[HSTS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X 框架-選項|[X 框架-選項瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)|
|X-XSS-保護|[X-XSS-保護瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)|
|跨原始資源分享 (CORS) |[CORS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility)
| (CSP) 的內容安全性原則|[CSP 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility)

## <a name="next"></a>下一個

- [使用 AD FS 協助疑難排解指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑難排解](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
