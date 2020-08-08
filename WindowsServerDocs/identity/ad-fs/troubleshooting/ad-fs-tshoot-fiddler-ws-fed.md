---
title: AD FS 疑難排解-Fiddler-WS-FEDERATION
description: 本檔顯示 WS-同盟交換與 AD FS 的深入追蹤
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.openlocfilehash: f30b66bd80b3c5002bdc2c66a1d89718cdb1ee39
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956205"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>AD FS 疑難排解-Fiddler-WS-FEDERATION

![AD FS 和 Windows Server 同盟圖表](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>步驟1和2

這是我們追蹤的開端。  在此畫面中，我們看到下列內容：

![開始 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

要求：

- 信賴憑證者的 HTTP GET (http://sql1.contoso.com/SampApp)

回應：

- 回應是 HTTP 302 (重新導向) 。  回應標頭中的傳輸資料會顯示要重新導向到的位置 (https://sts.contoso.com/adfs/ls)
- [重新導向 URL] 包含 wa = wsignin1.0 1.0，告訴我們 RP 應用程式已為我們建立 WS-同盟登入要求，並將其傳送至 AD FS 的/adfs/ls/端點。  這就是所謂的重新導向系結。

![在回應標頭中傳輸資料](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>步驟3和4

![接續 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

要求：

- AD FS 伺服器的 HTTP GET (sts.contoso.com) 

回應：

- 回應是認證的提示。  這表示我們使用表單 authnetication
- 按一下回應的 [Web 工作]，您就可以看到認證提示。

![接續 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>步驟5和6

![[提示認證提示] 畫面的 [Web 工作] 索引標籤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

要求：

- 包含使用者名稱和密碼的 HTTP POST。
- 我們會提供認證。  藉由查看要求中的原始資料，我們可以看到認證

回應：

- 找到回應，並會建立並傳回 MSIAuth 加密的 cookie。  這是用來驗證用戶端所產生的 SAML 判斷提示。  這也稱為「驗證 cookie」，只有在 AD FS 為 Idp 時才會出現。

## <a name="step-7-and-8"></a>步驟7和8

![接續 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

要求：

- 現在我們已驗證過，我們會對 AD FS 伺服器執行另一個 HTTP GET，並出示我們的驗證權杖

回應：

- 回應是 HTTP OK，表示 AD FS 已根據提供的認證來驗證使用者
- 此外，我們也將3個 cookie 設定回用戶端
    - MSISAuthenticated 包含以 base64 編碼的時間戳記值，適用于用戶端已驗證時。
    - AD FS 的無限迴圈偵測機制會使用 MSISLoopDetectionCookie，以停止在同盟伺服器上最後進入無限重新導向迴圈的用戶端。 Cookie 資料是以 base64 編碼的時間戳記。
    - MSISSignout 是用來追蹤 IdP 和所有已造訪 SSO 會話的 RPs。 叫用 WS-同盟登出時，會使用此 cookie。 您可以使用 base64 解碼器來查看此 cookie 的內容。

## <a name="step-9-and-10"></a>步驟9和10

![接續 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png)

要求：

- HTTP POST

回應：

- 回應是找到的

## <a name="step-11-and-12"></a>步驟11和12

![結束 Fiddler 追蹤](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png)

要求：

- HTTP GET

回應：

- 回應正常

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)