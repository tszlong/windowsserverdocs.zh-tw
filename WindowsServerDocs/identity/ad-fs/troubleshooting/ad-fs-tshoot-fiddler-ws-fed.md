---
title: 疑難排解-Fiddler-WS-同盟的 AD FS
description: 本文件說明 WS-同盟 exchange 與 AD FS 的深入的追蹤
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846899"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>疑難排解-Fiddler-WS-同盟的 AD FS
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>步驟 1 和 2
這是我們追蹤的開頭。  在此框架中我們看到下列項目： ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

要求：

- HTTP GET 為我們信賴憑證者的合作對象 （ http://sql1.contoso.com/SampApp)

回應：

- 回應是 HTTP 302 （重新導向）。  傳輸資料，回應標頭中的顯示為 （重新導向的位置 https://sts.contoso.com/adfs/ls)
- 重新導向 URL 包含 wa = wsignin 1.0 告訴我們，我們的 RP 應用程式有沒有針對我們所建置的 WS-同盟登入要求，如此傳送至 AD FS 的 adfs/oauth2//ls/端點。  這稱為重新導向繫結。
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>步驟 3 和 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

要求：

- HTTP GET 至我們的 AD FS server(sts.contoso.com)

回應：

- 回應是認證的提示。  這表示，我們使用表單進行
- 按一下 web 回應的檢視中，您可以看到提示的認證。
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>步驟 5 和 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

要求：

- 我們的使用者名稱和密碼的 HTTP POST。  
- 我們會提供我們的認證。  藉由查看要求的原始資料中，我們可以看到認證

回應：

- 發現和 MSIAuth 加密的 cookie 會建立並傳回回應。  這用來驗證用戶端所產生的 SAML 判斷提示。  這就是所謂的 「 驗證 cookie"，並將只會出現 AD FS 時的 Idp。


## <a name="step-7-and-8"></a>步驟 7 和 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

要求：

- 既然我們已通過驗證我們 AD FS 伺服器的另一個 HTTP GET 作業，並提供我們的驗證權杖

回應：

- 回應是 HTTP OK 表示 AD FS 已經驗證讓使用者根據提供的認證
- 此外，我們設定了 3 個 cookie 傳回給用戶端
    - MSISAuthenticated 包含用戶端驗證時的 base64 編碼的時間戳記值。
    - 在同盟伺服器無限重新導向迴圈中有最後停止用戶端的 AD FS 無限迴圈偵測機制會使用 MSISLoopDetectionCookie。 Cookie 資料是 base64 編碼的時間戳記。
    - MSISSignout 用來追蹤的 IdP，而且所有 RPs 瀏覽都過的 SSO 工作階段。 WS-同盟登出會叫用時，會使用此 cookie。 您可以看到使用 base64 解碼器此 cookie 的內容。
    
## <a name="step-9-and-10"></a>步驟 9 和 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) 要求：

- HTTP POST

回應：

- 回應是找到

## <a name="step-11-and-12"></a>步驟 11 和 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) 要求：

- HTTP GET

回應：

- 回應是 [確定]

## <a name="next-steps"></a>後續步驟

- [疑難排解 AD FS](ad-fs-tshoot-overview.md)