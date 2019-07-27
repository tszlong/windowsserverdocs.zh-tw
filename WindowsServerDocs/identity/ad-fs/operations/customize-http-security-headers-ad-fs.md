---
title: 使用 AD FS 自訂 HTTP 安全性回應標頭
description: 本檔 descirbes 如何自訂安全性標頭, 以防止安全性弱點。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3c497cbafb8f9313f988a1b892d2b8fef68115eb
ms.sourcegitcommit: f6503e503d8f08ba8000db9c5eda890551d4db37
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/26/2019
ms.locfileid: "68523907"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>使用 AD FS 2019 自訂 HTTP 安全性回應標頭 
 
為了防止常見的安全性弱點, 並讓系統管理員能夠利用以瀏覽器為基礎的保護機制的最新進展, AD FS 2019 新增了自訂 HTTP 安全性回應標頭的功能。由 AD FS 傳送。 這是透過引進兩個新的 Cmdlet 來完成`Get-AdfsResponseHeaders` : `Set-AdfsResponseHeaders`和。  

>[!NOTE]
>使用 Cmdlet 自訂 HTTP 安全性回應標頭 (CORS 標頭除外) 的功能`Get-AdfsResponseHeaders` : `Set-AdfsResponseHeaders`和是 backport 至 AD FS 2016。 您可以藉由安裝[KB4493473](https://support.microsoft.com/en-us/help/4493473/windows-10-update-kb4493473)和[KB4507459](https://support.microsoft.com/en-us/help/4507459/windows-10-update-kb4507459), 將功能新增至您的 AD FS 2016。 

在本檔中, 我們將討論常用的安全性回應標頭, 以示範如何自訂 AD FS 2019 所傳送的標頭。   
 
>[!NOTE]
>檔假設已安裝 AD FS 2019。  

 
在討論標頭之前, 讓我們先來看一些案例, 建立系統管理員自訂安全性標頭的需求 
 
## <a name="scenarios"></a>案例 
1. 系統管理員已啟用[**HTTP 嚴格傳輸安全性 (HSTS)** ](#http-strict-transport-security-hsts) (強制所有透過 HTTPS 加密的連線), 以保護可能使用 HTTP 從可能遭到駭客入侵的公用 wifi 存取點存取 web 應用程式的使用者。 她想要啟用子域的 HSTS 來進一步加強安全性。  
2. 系統管理員已設定[**X 框架選項**](#x-frame-options)回應標頭 (防止轉譯 iFrame 中的任何網頁), 以保護網頁不被 clickjacked。 不過, 她必須自訂標頭值, 這是因為新的商務需求, 會從具有不同來源 (網域) 的應用程式顯示資料 (在 iFrame 中)。
3. 系統管理員已啟用[**X-XSS 保護**](#x-xss-protection)(防止跨腳本攻擊), 以在瀏覽器偵測到跨腳本攻擊時, 淨化並封鎖頁面。 不過, 她必須自訂標頭, 以允許頁面在經過清理之後載入。  
4. 系統管理員必須啟用[**跨原始來源資源分享 (CORS)** ](#cross-origin-resource-sharing-cors-headers) , 並將 AD FS 上的來源 (網域) 設定為允許單一頁面應用程式存取具有另一個網域的 Web API。  
5. 系統管理員已啟用[**內容安全性原則 (CSP)** ](#content-security-policy-csp)標頭, 藉由禁止任何跨網域要求, 防止跨網站腳本和資料插入攻擊。 不過, 由於新的商務需求, 她必須自訂標頭, 以允許網頁從任何來源載入影像, 並將媒體限制為受信任的提供者。  

 
## <a name="http-security-response-headers"></a>HTTP 安全性回應標頭 
回應標頭會包含在 AD FS 傳送至網頁瀏覽器的傳出 HTTP 回應中。 您可以使用`Get-AdfsResponseHeaders` Cmdlet 來列出標頭, 如下所示。  

![標頭回應](media/customize-http-security-headers-ad-fs/header1.png)

上述`ResponseHeaders`螢幕擷取畫面中的屬性會識別每個 HTTP 回應中 AD FS 將包含的安全性標頭。 只有當`ResponseHeadersEnabled`設為`True` (預設值) 時, 才會傳送回應標頭。 此值可以設定為`False` , 以防止 AD FS 包括 HTTP 回應中的任何安全性標頭。 不過, 不建議這樣做。  若要這麼做, 請使用下列各項:

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-傳輸安全性 (HSTS) 
HSTS 是一種 web 安全性原則機制, 可協助減少同時具有 HTTP 和 HTTPS 端點之服務的通訊協定降級攻擊和 cookie 劫持。 它可讓網頁伺服器宣告網頁瀏覽器 (或其他符合使用者代理程式) 只能使用 HTTPS 與其互動, 而不是透過 HTTP 通訊協定。  
 
Web 驗證流量的所有 AD FS 端點都會以獨佔方式透過 HTTPS 開啟。 因此, AD FS 有效地降低 HTTP Strict 傳輸安全性原則機制所提供的威脅 (因為 HTTP 中沒有接聽程式, 所以預設不會降級至 HTTP)。 您可以藉由設定下列參數來自訂標頭 
 
- **最大壽命 =&lt; &gt;到期**時間–到期時間 (以秒為單位) 指定網站只能使用 HTTPS 存取的時間長度。 預設和建議值為31536000秒 (1 年)。  
- **includeSubDomains** –這是選擇性參數。 若已指定, HSTS 規則也會套用至所有子域。  
 
#### <a name="hsts-customization"></a>HSTS 自訂 
根據預設, 標頭會啟用並`max-age`設為1年; 不過, 系統管理員可以`max-age`修改 (不建議使用 [減少最大壽命] 值), 或透過`Set-AdfsResponseHeaders` Cmdlet 啟用子域的 HSTS。  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

根據預設, 標頭會包含在`ResponseHeaders`屬性中; 不過, 系統管理員可以`Set-AdfsResponseHeaders`透過 Cmdlet 移除標頭。  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X 框架-選項 
AD FS 預設不允許外部應用程式在執行互動式登入時使用 Iframe。 這麼做是為了防止特定的網路釣魚攻擊樣式。 請注意, 由於先前已建立的工作階段層級安全性, 無法透過 iFrame 執行非互動式登入。  
 
不過, 在某些罕見的情況下, 您可能會信任需要支援 iFrame 的特定應用程式, AD FS 登入頁面。 ' X 框架-選項 ' 標頭用於此用途。  
 
這個 HTTP 安全性回應標頭是用來與瀏覽器通訊, 不論它是否可以在&lt;框架&gt; / &lt;iframe&gt;中轉譯頁面。 標頭可設為下列其中一個值: 
 
- **拒絕**–畫面格中的頁面將不會顯示。 這是預設和建議的設定。  
- **sameorigin** –如果原點與網頁的來源相同, 則頁面只會顯示在框架中。 除非所有祖系也位於相同的來源, 否則選項不會非常有用。  
- [允許]-只有在原點 (例如,. ") https://www 時, 頁面才會顯示在畫面格中。 **<specified origin>** com) 符合標頭中的特定來源。 

#### <a name="x-frame-options-customization"></a>X 框架-選項自訂  
根據預設, 標頭會設定為拒絕;不過, 系統管理員可以透過`Set-AdfsResponseHeaders` Cmdlet 來修改此值。  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

根據預設, 標頭會包含在`ResponseHeaders`屬性中; 不過, 系統管理員可以`Set-AdfsResponseHeaders`透過 Cmdlet 移除標頭。  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
當瀏覽器偵測到跨網站腳本 (XSS) 攻擊時, 此 HTTP 安全性回應標頭可用來停止載入網頁。 這稱為 XSS 篩選。 標頭可設為下列其中一個值 
 
- **0** –停用 XSS 篩選。 不建議使用。  
- **1** –啟用 XSS 篩選。 如果偵測到 XSS 攻擊, browser 會淨化該頁面。   
- **1; 模式 = 封鎖**–啟用 XSS 篩選。 如果偵測到 XSS 攻擊, 瀏覽器將會防止轉譯頁面。 這是預設和建議的設定。  

#### <a name="x-xss-protection-customization"></a>X-XSS-保護自訂 
根據預設, 標頭會設定為 1;mode = block;不過, 系統管理員可以透過`Set-AdfsResponseHeaders` Cmdlet 來修改此值。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

根據預設, 標頭會包含在`ResponseHeaders`屬性中; 不過, 系統管理員可以`Set-AdfsResponseHeaders`透過 Cmdlet 移除標頭。 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>跨原始來源資源分享 (CORS) 標頭 
網頁瀏覽器安全性可防止網頁在腳本內起始跨原始來源要求。 不過, 有時您可能會想要存取其他來源 (網域) 中的資源。 CORS 是一種 W3C 標準, 可讓伺服器放寬相同的原始原則。 使用 CORS, 伺服器可以明確允許某些跨原始來源要求, 同時拒絕其他要求。  
 
為了進一步瞭解 CORS 要求, 讓我們逐步解說單一頁面應用程式 (SPA) 需要使用不同網域呼叫 Web API 的案例。 此外, 請考慮在 ADFS 2019 上設定 SPA 和 API, 且 AD FS 已啟用 CORS, 也就是 AD FS 可以識別 HTTP 要求中的 CORS 標頭、驗證標頭值, 並在回應中包含適當的 CORS 標頭 (詳細資料, 以瞭解如何啟用和在下面的 CORS 自訂區段中, 設定 AD FS 2019 上的 CORS)。 範例流程: 

1. 使用者透過用戶端瀏覽器存取 SPA, 並重新導向至 AD FS auth 端點進行驗證。 由於 SPA 是針對隱含授與流程所設定, 因此在成功驗證之後, 要求會將存取 + 識別碼權杖傳回至瀏覽器。  
2. 使用者驗證之後, SPA 中包含的前端 JavaScript 就會要求存取 Web API。 要求會被重新導向至具有下列標頭的 AD FS
    - 選項–描述目標資源的通訊選項 
    - 來源–包含 Web API 的來源
    - 存取控制-要求-方法-識別提出實際要求時所要使用的 HTTP 方法 (例如, DELETE) 
    - 存取控制-要求-標頭-識別提出實際要求時所要使用的 HTTP 標頭 
    
   >[!NOTE]
   >CORS 要求類似于標準 HTTP 要求, 不過, 原始標頭的存在表示連入要求與 CORS 相關。 
3. AD FS 會驗證標頭中所包含的 Web API 來源是否列于 AD FS 中設定的受信任來源 (詳細資料關於如何在 CORS 自訂中修改信任的來源一節)。 AD FS 接著會以下列標頭回應。  
    - 存取控制-允許-來源-與原始標頭中的值相同 
    - 存取控制--------方法標頭中的值與相同 
    - 存取控制-允許-標頭-值與在存取控制-要求標頭標題中相同 
4. 瀏覽器傳送包含下列標頭的實際要求 
    - HTTP 方法 (例如, DELETE) 
    - 來源–包含 Web API 的來源 
    - 包含在存取控制-Allow 標頭中的所有標頭 
5. 一旦通過驗證, AD FS 在「存取控制-允許原始回應」標頭中包含 Web API 網域 (來源) 來核准要求。  
6. 包含存取控制允許來源標頭, 可讓瀏覽器繼續呼叫所要求的 API。

#### <a name="cors-customization"></a>CORS 自訂 
根據預設, 將不會啟用 CORS 功能;不過, 系統管理員可以透過 AdfsResponseHeaders Cmdlet 來啟用此功能。  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

一個已啟用的系統管理員將能夠使用相同的 Cmdlet 來列舉受信任來源的清單。 例如, 下列命令會允許來自原始來源**HTTPs&#58;//example1.com**和 **&#58;HTTPs//example1.com**的 CORS 要求。 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> 系統管理員可以在受信任的來源清單中包含 "*", 以允許來自任何來源的 CORS 要求, 雖然因為安全性弱點, 不建議使用此方法, 如果選擇, 就會提供警告訊息。 

### <a name="content-security-policy-csp"></a>內容安全性原則 (CSP) 
這個 HTTP 安全性回應標頭是用來防止瀏覽器不小心執行惡意內容, 以避免跨網站腳本、點擊劫持和其他資料插入攻擊。 不支援 CSP 的瀏覽器只會忽略 CSP 回應標頭。  
 
#### <a name="csp-customization"></a>CSP 自訂 
CSP 標頭的自訂牽涉到修改安全性原則, 以允許載入網頁的資源瀏覽器。 預設的安全性原則為  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
**Default-src**指示詞是用來修改[-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)指示詞, 而不需要明確列出每個指示詞。 例如, 在下列範例中, 原則1與原則2相同。  

原則1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
原則2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

如果明確列出指示詞, 指定的值會覆寫為 default-src 提供的值。在下列範例中, img-src 會將值當做 ' * ' (允許從任何來源載入影像), 而其他-src 指示詞會將值視為「自我」 (限制為與網頁相同的來源)。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
您可以為預設的-src 原則定義下列來源 
 
- ' self ' –指定此項會限制內容的來源載入網頁的來源 
- 「unsafe-內嵌」–在原則中指定此項可允許使用內嵌的 JavaScript 和 CSS 
- ' unsafe-eval ' –在原則中指定此項, 允許將文字用於 JavaScript 等機制 (例如 eval) 
- ' none ' –指定此限制要載入的任何來源內容 
- 資料:-指定資料:Uri 可讓內容建立者內嵌內嵌在檔中的小型檔案。 不建議使用。  
 
>[!NOTE]
>AD FS 在驗證程式中使用 JavaScript, 因此在預設原則中包含 ' unsafe-inline ' 和 ' unsafe-eval ' 來源, 藉以啟用 JavaScript。  

### <a name="custom-headers"></a>自訂標頭 
除了上述的安全性回應標頭 (HSTS、CSP、X 框架選項、X-XSS-保護和 CORS) 之外, AD FS 2019 提供設定新標頭的功能。  
 
範例：若要設定新的標頭 "TestHeader", 並將值設為 "TestHeaderValue" 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

設定之後, 新的標頭會在 AD FS 回應中傳送 (以下的 fiddler 程式碼片段)。  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Web 瀏覽器相容性
使用下表和連結, 判斷哪些網頁瀏覽器與每個安全性回應標頭相容。

|HTTP 安全性回應標頭|瀏覽器相容性|
|-----|-----|
|HTTP Strict-傳輸安全性 (HSTS)|[HSTS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X 框架-選項|[X 框架-選項瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[X-XSS-保護瀏覽器相容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|跨原始來源資源分享 (CORS)|[CORS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|內容安全性原則 (CSP)|[CSP 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>下一個

- [使用 AD FS Help troublehshooting 指南](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑難排解](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
