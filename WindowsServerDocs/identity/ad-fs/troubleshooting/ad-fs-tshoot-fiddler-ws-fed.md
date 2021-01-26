---
title: AD FS 疑難排解-Fiddler-WS-Federation
description: 本檔顯示 WS-Federation 交換與 AD FS 的深入追蹤
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.openlocfilehash: 96d202a0c4379d2d52e41bdfb0c750e0e64ffaf2
ms.sourcegitcommit: 6717decb5839aa340c81811d6fde020aabaddb3b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2021
ms.locfileid: "98781926"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>AD FS 疑難排解-Fiddler-WS-Federation

![AD FS 和 Windows Server 同盟圖表](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>步驟1和2

這是我們的追蹤開始。  在此畫面中，我們會看到下列內容：

![Fiddler 追蹤的開頭](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

要求：

- HTTP GET 給我們的信賴憑證者 (http://sql1.contoso.com/SampApp)

回應：

- 回應是 HTTP 302 (重新導向) 。  回應標頭中的傳輸資料會顯示重新導向至 (https://sts.contoso.com/adfs/ls)
- 重新導向 URL 包含 wa = wsignin1.0 1.0，告訴我們 RP 應用程式已為我們建立 WS-Federation 登入要求，並將其傳送至 AD FS 的/adfs/ls/端點。  這就是所謂的重新導向系結。

![回應標頭中的傳輸資料](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>步驟3和4

![接續 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

要求：

- AD FS server 的 HTTP GET (sts.contoso.com) 

回應：

- 回應會提示您輸入認證。  這表示我們使用表單驗證
- 藉由按一下回應的 Web 程式，您可以看到認證提示。

![顯示認證提示之回應的 web view 螢幕擷取畫面。](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>步驟5和6

![提示輸入認證提示畫面的 [Web 程式] 索引標籤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

要求：

- 使用我們的使用者名稱和密碼進行 HTTP POST。
- 我們會提供認證。  藉由查看要求中的原始資料，我們可以看到認證

回應：

- 找到回應，並會建立並傳回 MSIAuth 加密 cookie。  這是用來驗證用戶端所產生的 SAML 判斷提示。  這也稱為「驗證 cookie」，只有在 AD FS 為 Idp 時才會出現。

## <a name="step-7-and-8"></a>步驟7和8

![Fiddler 追蹤的螢幕擷取畫面，其中顯示 H T T P Get 要求以及對該要求的回應。](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

要求：

- 既然我們已驗證過，我們會對 AD FS 伺服器進行另一個 HTTP GET，並出示驗證權杖

回應：

- 回應是 HTTP OK，表示 AD FS 已根據提供的認證來驗證使用者
- 此外，我們會將3個 cookie 設定回用戶端
    - MSISAuthenticated 包含驗證用戶端時的 base64 編碼時間戳記值。
    - AD FS 的無限迴圈偵測機制會使用 MSISLoopDetectionCookie，來停止在無限重新導向迴圈中最後的用戶端到同盟伺服器。 Cookie 資料是以 base64 編碼的時間戳記。
    - MSISSignout 是用來追蹤 IdP 和針對 SSO 會話所造訪的所有 RPs。 叫用 WS-Federation 登出時，就會使用此 cookie。 您可以使用 base64 解碼器來查看此 cookie 的內容。

## <a name="step-9-and-10"></a>步驟9和10

![Fiddler 追蹤的螢幕擷取畫面，其中顯示 H T T Post 要求和對該要求的回應。](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png)

要求：

- HTTP POST

回應：

- 已找到回應

## <a name="step-11-and-12"></a>步驟11和12

![終止 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png)

要求：

- HTTP GET

回應：

- 回應正常

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)