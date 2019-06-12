---
title: 自訂 HTTP 安全性回應標頭與 AD FS
description: 此文件 descirbes 如何自訂安全性標頭，以防止安全性弱點。
author: billmath
ms.author: billmath
manager: daveba
ms.reviewer: akgoel23
ms.date: 02/19/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 231c8783032f51f607565922d90ea7f7eb877cfd
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444695"
---
# <a name="customize-http-security-response-headers-with-ad-fs-2019"></a>自訂 AD FS 2019 的 HTTP 安全性回應標頭 
 
若要防範常見的安全性弱點，並讓系統管理員能夠充分利用最新的瀏覽器為基礎的保護機制，AD FS 2019 新增自訂 HTTP 安全性回應標頭的功能AD FS 所傳送。 這透過引進的兩個新的 cmdlet 來完成：`Get-AdfsResponseHeaders`和`Set-AdfsResponseHeaders`。  
 
通常，我們將討論這份文件中使用安全性回應標頭，示範如何自訂 AD FS 2019 所傳送的標頭。   
 
>[!NOTE]
>文件會假設已安裝 AD FS 2019。  

 
我們會討論標頭之前，我們來看看幾個案例建立自訂安全性標頭的系統管理員的需求 
 
## <a name="scenarios"></a>案例 
1. 系統管理員已啟用[ **HTTP Strict-傳輸層安全性 (HSTS)** ](#http-strict-transport-security-hsts) （強制透過 HTTPS 加密的所有連線） 來保護可能會存取公用 wifi 存取來自使用 HTTP 的 web 應用程式的使用者可能會遭竊取的點。 她想要進一步加強安全性，藉由啟用 HSTS 為子網域。  
2. 系統管理員已設定[ **X 框架選項**](#x-frame-options) （可避免轉譯在 iFrame 中的任何網頁上） 的回應標頭來防止 clickjacked 保護網頁。 不過，她必須自訂標頭值，因為新的商業需求，以顯示資料 （在 iFrame) 從其他來源 （網域） 的應用程式。
3. 系統管理員已啟用[ **X XSS 防護**](#x-xss-protection) （可避免跨指令碼攻擊） 處理，並封鎖頁面上，如果瀏覽器偵測到跨指令碼攻擊。 不過，她必須自訂標頭，以允許載入頁面一次 」 妥善處理。  
4. 系統管理員必須啟用[**跨原始資源共用 (CORS)** ](#cross-origin-resource-sharing-cors-headers)和 AD FS，以允許單一頁面應用程式存取 web API 與另一個網域上設定原點 （網域）。  
5. 系統管理員已啟用[**內容安全性原則 (CSP)** ](#content-security-policy-csp)不允許任何跨網域要求的標頭，以防止跨網站指令碼和資料插入式攻擊。 不過，因為新的商業需求而需要自訂標頭，以允許從任何來源載入影像，並限制受信任的提供者的媒體的網頁。  

 
## <a name="http-security-response-headers"></a>HTTP 安全性回應標頭 
回應標頭會包含在外寄傳送至 HTTP 回應由 AD FS 的網頁瀏覽器。 可以使用列出的標頭`Get-AdfsResponseHeaders`cmdlet，如下所示。  

![標頭回應](media/customize-http-security-headers-ad-fs/header1.png)

`ResponseHeaders`上面的螢幕擷取畫面中的屬性會識別將會包含在每個 HTTP 回應中的 AD fs 的安全性標頭。 將會傳送回應標頭，只有當`ResponseHeadersEnabled`設為`True`（預設值）。 值可以設定為`False`以避免包括任何安全性標頭的 HTTP 回應中的 AD FS。 不過這不建議。  若要執行這使用下列項目：

```PowerShell
Set-AdfsResponseHeaders -EnableResponseHeaders $false
```
 
### <a name="http-strict-transport-security-hsts"></a>HTTP Strict-傳輸層安全性 (HSTS) 
HSTS 是 web 安全性原則機制，可協助降低通訊協定降級攻擊和 cookie 攔截有 HTTP 和 HTTPS 端點的服務。 它可讓 web 伺服器，來宣告，網頁瀏覽器 （或其他 complying 使用者代理程式） 應該僅互動與它使用 HTTPS，並永遠不會透過 HTTP 通訊協定。  
 
適用於 web 驗證流量的所有 AD FS 端點會透過 HTTPS 以獨佔方式都開啟。 如此一來，AD FS 有效地降低 HTTP Strict Transport Security 原則機制提供的威脅 （依預設沒有任何降級至 HTTP 由於 HTTP 有沒有接聽程式）。 藉由設定下列參數，可以自訂標頭 
 
- **最大壽命 =&lt;到期時間&gt;** – 到期時間 （以秒為單位） 可讓您指定多久網站應該只能存取使用 HTTPS。 預設的建議的值是 31536000 秒 （1 年）。  
- **includeSubDomains** – 這是選擇性參數。 如果指定，HSTS 規則適用於所有的子網域。  
 
#### <a name="hsts-customization"></a>HSTS 自訂 
根據預設，標頭已啟用並`max-age`設為 1 年; 不過，系統管理員可以修改`max-age`（降低最大壽命值不建議） 或子網域透過啟用 HSTS `Set-AdfsResponseHeaders` cmdlet。  
 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=<seconds>; includeSubDomains" 
``` 

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Strict-Transport-Security" -SetHeaderValue "max-age=31536000; includeSubDomains" 
 ```

根據預設，在包含標頭`ResponseHeaders`屬性; 不過，系統管理員可以移除透過標頭`Set-AdfsResponseHeaders`cmdlet。  
 
```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "Strict-Transport-Security" 
```

### <a name="x-frame-options"></a>X-Frame-Options 
預設的 AD FS 不允許外部應用程式時所要使用 Iframe 執行互動式登入。 這是為了防止特定樣式的網路釣魚攻擊。 請注意，可透過 iFrame，因為先前的工作階段已建立的層級安全性而執行非互動式登入。  
 
不過，在某些罕見的情況下，您可能信任需要 iFrame 支援互動式的 AD FS 登入頁面的特定應用程式。 針對此目的，使用 ' X-框架-Options' 標頭。  
 
此 HTTP 安全性回應標頭用來傳達給瀏覽器是否呈現在頁面&lt;框架&gt;/&lt;iframe&gt;。 標頭可以設定下列值之一： 
 
- **拒絕**– 將不會顯示在框架中的頁面。 這是預設且建議的設定。  
- **sameorigin 所**– 頁面將才會顯示在框架中的來源是網頁的原始來源相同。 此選項不是很有用的除非所有上階也位於相同的來源。  
- **允許從<specified origin>**  -頁面則只會顯示在框架中如果來源 (例如， https://www。 」。com) 比對特定的來源，以標頭。 

#### <a name="x-frame-options-customization"></a>X 框架選項自訂  
根據預設，標頭會設定為拒絕;不過，系統管理員可以修改透過值`Set-AdfsResponseHeaders`cmdlet。  
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "<deny/sameorigin/allow-from<specified origin>>" 
 ```

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-Frame-Options" -SetHeaderValue "allow-from https://www.example.com" 
 ```

根據預設，在包含標頭`ResponseHeaders`屬性; 不過，系統管理員可以移除透過標頭`Set-AdfsResponseHeaders`cmdlet。  

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-Frame-Options" 
```

### <a name="x-xss-protection"></a>X-XSS-Protection 
此 HTTP 安全性回應標頭用來停止從瀏覽器所偵測到跨網站指令碼 (XSS) 攻擊時載入的網頁。 這稱為 XSS 篩選。 標頭可以設為其中一個下列的值 
 
- **0** – 停用的 XSS 篩選。 不建議使用。  
- **1** – 可讓 XSS 篩選。 如果偵測到 XSS 攻擊時，瀏覽器將會淨化頁面。   
- **1;模式 = 區塊**– 可讓 XSS 篩選。 如果偵測到 XSS 攻擊時，瀏覽器會使頁面的轉譯。 這是預設且建議的設定。  

#### <a name="x-xss-protection-customization"></a>X XSS 保護自訂 
根據預設，標頭會設定為 1。模式 = 應用程式區塊。不過，系統管理員可以修改透過值`Set-AdfsResponseHeaders`cmdlet。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "<0/1/1; mode=block/1; report=<reporting-uri>>" 
``` 

範例： 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "X-XSS-Protection" -SetHeaderValue "1" 
 ```

根據預設，在包含標頭`ResponseHeaders`屬性; 不過，系統管理員可以移除透過標頭`Set-AdfsResponseHeaders`cmdlet。 

```PowerShell
Set-AdfsResponseHeaders -RemoveHeaders "X-XSS-Protection" 
```

### <a name="cross-origin-resource-sharing-cors-headers"></a>跨原始資源共用 (CORS) 標頭 
網頁瀏覽器安全性可防止網頁進行跨原始來源要求從起始指令碼中。 不過，有時候您可能想要存取其他來源 （網域） 中的資源。 CORS 是 W3C 標準，可讓伺服器放寬同源原則。 使用 CORS，伺服器可以明確允許某些跨源要求並拒絕其他。  
 
若要進一步了解 CORS 要求，讓我們逐步解說的案例，其中單一頁面應用程式 (SPA) 需要呼叫 web API 與不同的網域。 此外，我們來看一下 SPA 和 API 已在 2019 ADFS，AD FS 已啟用 CORS 也就是 AD FS 可以識別在 HTTP 要求的 CORS 標頭、 驗證標頭值，以及在回應中包含適當的 CORS 標頭 (如何啟用的詳細資訊和設定 CORS AD FS 2019 CORS 自訂一節中）。 範例流程： 

1. 使用者會存取透過用戶端瀏覽器的 SPA，並會重新導向至 AD FS 進行驗證的驗證端點。 由於 SPA 設定隱含授與流程，要求存取 + 識別碼傳回至瀏覽器的語彙基元，在成功驗證之後。  
2. 使用者驗證之後，前端的 JavaScript SPA 中包含會提出要求，以存取 web API。 已重新導向至 AD FS 包含下列標頭
    - 選項 – 描述的目標資源的通訊選項 
    - 來源 – 包含來源的 web API
    - 存取控制-要求-方法-識別的 HTTP 方法 （例如，刪除） 提出實際要求時要使用 
    - 存取控制-access-control-request-headers 標-識別提出實際要求時要使用的 HTTP 標頭 
    
   >[!NOTE]
   >CORS 要求類似於標準的 HTTP 要求，不過，origin 標頭的存在表示傳入要求是 CORS 相關。 
3. AD FS 驗證 web API 的標頭中包含的原始來源會列在 AD FS （如何修改在 CORS 自訂一節中的受信任的來源的詳細資料） 中設定信任的原始來源。 AD FS，然後會回應包含下列標頭。  
    - 存取控制-允許-原始 – 如同 Origin 標頭值 
    - 存取控制-允許-方法-如所示的存取控制-要求方法標頭值 
    - 存取控制-允許-標頭-如所示的存取控制-access-control-request-headers 標標頭值 
4. 瀏覽器會傳送實際要求，包括下列標頭 
    - HTTP 方法 （例如，刪除） 
    - 來源 – 包含來源的 web API 
    - 存取控制-允許-標頭的回應標頭中包含的所有標頭 
5. 驗證之後，AD FS 會核准要求的存取控制-允許-原始回應標頭中包含的 web API 的網域 （來源）。  
6. 存取控制-允許-原始標頭包含可讓瀏覽器以繼續進行呼叫要求的 API。

#### <a name="cors-customization"></a>CORS 自訂 
根據預設，將不會啟用 CORS 功能;不過，系統管理員可以啟用透過集 AdfsResponseHeaders cmdlet 的功能。  

```PowerShell 
Set-AdfsResponseHeaders -EnableCORS $true 
 ```

其中一個已啟用，系統管理員將能夠列舉的受信任的來源使用相同的 cmdlet 清單。 比方說，下列命令會讓 CORS 要求來自原點**https&#58;//example1.com**並**https&#58;//example1.com**。 
 
```PowerShell
Set-AdfsResponseHeaders -CORSTrustedOrigins https://example1.com,https://example2.com 
 ```

> [!NOTE]
> 系統管理員可以允許來自任何來源的 CORS 要求，方式包括 「 * 」 清單中的受信任的來源，但由於安全性弱點，以及一則警告訊息，不建議這種方法提供如果他們選擇。 

### <a name="content-security-policy-csp"></a>內容安全性原則 (CSP) 
此 HTTP 安全性回應標頭用來防止跨網站指令碼、 點閱綁架和其他資料的資料隱碼攻擊，藉由防止瀏覽器不小心執行惡意內容。 不支援 CSP 的瀏覽器只會忽略 CSP 回應標頭。  
 
#### <a name="csp-customization"></a>CSP 自訂 
CSP 標頭的自訂牽涉到修改定義資源瀏覽器的安全性原則允許載入的網頁。 預設安全性原則  
 
`Content-Security-Policy: default-src ‘self’ ‘unsafe-inline’ ‘’unsafe-eval’; img-src ‘self’ data:;` 
 
**預設 src**指示詞用來修改[-src 指示詞](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)而不需要明確列出每個指示詞。 比方說，在以下原則範例 1 是與原則 2 相同的。  

原則 1 
```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src 'self'" 
```
 
原則 2
```PowerShell 
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "script-src ‘self’; img-src ‘self’; font-src 'self';  
frame-src 'self'; manifest-src 'self'; media-src 'self';" 
```

如果明確列出指示詞，指定的值會覆寫預設 src 為指定的值。在下列範例中，i m g src 要做為值 ' *' （允許從任何來源載入的映像） 而其他-src 指示詞會採用的值為 '自我' （婞鎏磭懘網頁限制）。  

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "Content-Security-Policy" -SetHeaderValue "default-src ‘self’; img-src *" 
```
下列來源可定義的預設 src 原則 
 
- 'self' – 指定此會限制要載入到網頁的原始伺服器的內容的原點 
- ' unsafe-inline' – 指定此原則中允許使用內嵌 JavaScript 和 CSS 
- ' unsafe-eval' – 指定此原則中允許使用文字等機制的 javascript eval 
- 'none' – 指定此限制來自任何來源載入的內容 
- 資料:-指定的資料：Uri 可讓內嵌的小型檔案內嵌在文件中的內容建立者。 不建議使用方式。  
 
>[!NOTE]
>AD FS 驗證程序中使用 JavaScript，並因此可讓 JavaScript 包括 'unsafe inline' 及' unsafe eval' 來源在預設原則。  

### <a name="custom-headers"></a>自訂標頭 
除了上述所列的安全性回應標頭 (HSTS，CSP，X 框架選項、 X XSS 防護和 CORS)，AD FS 2019 讓您能夠設定新的標頭。  
 
範例：若要將新標頭 」 TestHeader"與值設定為"TestHeaderValue 」 

```PowerShell
Set-AdfsResponseHeaders -SetHeaderName "TestHeader" -SetHeaderValue "TestHeaderValue" 
 ```

設定之後，新的標頭在回應時傳送 AD FS （fiddler 下列程式碼片段）。  
 
![Fiddler](media/customize-http-security-headers-ad-fs/header2.png)

## <a name="web-browswer-compatibility"></a>Web 瀏覽器相容性
若要判斷哪些 web 瀏覽器相容與每個安全性回應標頭使用下列資料表和連結。

|HTTP 安全性回應標頭|瀏覽器相容性|
|-----|-----|
|HTTP Strict-傳輸層安全性 (HSTS)|[HSTS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/Strict-Transport-Security#Browser_compatibility)|
|X-Frame-Options|[X 框架選項瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/Headers/X-Frame-Options#Browser_compatibility)| 
|X-XSS-Protection|[X XSS 保護瀏覽器相容性](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-XSS-Protection#Browser_compatibility)| 
|跨原始資源共用 (CORS)|[CORS 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CORS#Browser_compatibility) 
|內容安全性原則 (CSP)|[CSP 瀏覽器相容性](https://developer.mozilla.org/docs/Web/HTTP/CSP#Browser_compatibility) 

## <a name="next"></a>下一個

- [使用 AD FS 說明 troublehshooting 輔助線](https://aka.ms/adfshelp/troubleshooting )
- [AD FS 疑難排解](../../ad-fs/troubleshooting/ad-fs-tshoot-overview.md)
