---
title: 傳輸層安全性通訊協定
description: 瞭解傳輸層安全性 (TLS) 通訊協定的運作方式，以及提供適用于 TLS 1.0、TLS 1.1 和 TLS 1.2 之 IETF Rfc 的連結。
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: 634045d6e513157e555290875f44b416729944c8
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113344"
---
# <a name="transport-layer-security-protocol"></a>傳輸層安全性通訊協定

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

本主題適用于 IT 專業人員，說明傳輸層安全性 (TLS) 通訊協定的運作方式，以及提供適用于 TLS 1.0、TLS 1.1 和 TLS 1.2 之 IETF Rfc 的連結。

TLS (和 SSL) 通訊協定位於應用程式協定層和 TCP/IP 層之間，可在其中保護和傳送應用程式資料至傳輸層。 因為通訊協定在應用層和傳輸層之間運作，因此 TLS 和 SSL 可支援多個應用層通訊協定。

TLS 和 SSL 假設連接導向傳輸（通常是 TCP）正在使用中。 此通訊協定可讓用戶端和伺服器應用程式偵測下列安全性風險：

-   訊息篡改

-   訊息攔截

-   訊息偽造

TLS 和 SSL 通訊協定可以分為兩層。 第一層是由應用程式協定和三種信號交換通訊協定所組成：交握通訊協定、變更加密規格通訊協定，以及警示通訊協定。 第二層是記錄通訊協定。 下圖說明各種圖層和其元素。

**TLS 和 SSL 通訊協定層**


安全通道 SSP 會在不修改的情況下，執行 TLS 和 SSL 通訊協定。 SSL 通訊協定是專屬的，但網際網路工程任務推動小組會產生公用 TLS 規格。 如需 Windows 版本所支援的 TLS 或 SSL 版本的詳細資訊，請參閱 [tls/ssl (SCHANNEL SSP) 中的通訊協定 ](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-)。 下表列出每個 TLS 版本的規格。 每個規格都包含下列資訊：

-   TLS 記錄通訊協定

-   TLS 信號交換通訊協定： \- 變更加密規格通訊協定 \- 警示通訊協定

-   密碼編譯計算

-   強制加密套件

-   應用程式資料通訊協定

[RFC 5246 - 傳輸層安全性 (TLS) 通訊協定 1.2 版](http://tools.ietf.org/html/rfc5246)

[RFC 4346 - 傳輸層安全性 (TLS) 通訊協定 1.1 版](http://tools.ietf.org/html/rfc4346)

[RFC 2246 - TLS 通訊協定 1.0 版](http://tools.ietf.org/html/rfc2246)

## <a name="tls-session-resumption"></a><a name="BKMK_SessionResumption"></a>TLS 工作階段繼續
安全通道 SSP 在 Windows Server 2012 R2 中引進，它會在 TLS 會話繼續執行伺服器端部分。 RFC 5077 的用戶端實作已加入 Windows 8 中。

將 TLS 連接到伺服器的裝置經常需要重新連接。 TLS 會話繼續可降低建立 TLS 連線的成本，因為恢復牽涉到縮寫的 TLS 交握。 這可讓一組 TLS 伺服器繼續彼此的 TLS 會話，以促進更多繼續嘗試。 這項修改可為任何支援 RFC 5077 的 TLS 用戶端節省下列費用，包括 Windows Phone 和 Windows RT 裝置：

-   減少伺服器上的資源使用量

-   降低頻寬，改善用戶端連線的效率

-   因為連線繼續而縮短 TLS 交握所花費的時間

如需無狀態 TLS 會話繼續的詳細資訊，請參閱 IETF 檔 [RFC 5077。](http://www.ietf.org/rfc/rfc5077)

## <a name="application-protocol-negotiation"></a><a name="BKMK_AppProtocolNego"></a>應用程式通訊協定交涉
 Windows Server 2012 R2 和 Windows 8.1 引進了允許用戶端 TLS 應用程式協定協商的支援。 應用程式可以在 HTTP 2.0 標準開發中利用通訊協定，而使用者可以使用執行 SPDY 通訊協定的應用程式來存取 Google 和 Twitter 等線上服務。

如需應用程式通訊協定交涉運作方式的詳細資訊，請參閱[傳輸層安全性 (TLS) 應用程式層通訊協定交涉延伸](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="tls-support-for-server-name-indication-extensions"></a><a name="BKMK_SNI"></a>伺服器名稱指示擴充功能的 TLS 支援
伺服器名稱指示器 (SNI) 功能會延伸 SSL 和 TLS 通訊協定，當單一伺服器上有許多虛擬映像正在執行時，允許對伺服器進行適當的識別。 在虛擬裝載案例中，有數個網域 (各自的可能相異憑證) 裝載于一部伺服器上。 在此情況下，伺服器無法事先知道要傳送給用戶端的憑證。 SNI 可讓用戶端在通訊協定中稍早通知目標網域，如此一來，伺服器就能正確地選取適當的憑證。

這個額外功能：

-   可讓您在單一網際網路通訊協定和埠組合上裝載多個 SSL 網站

-   在單一網頁伺服器上裝載多個 SSL 網站時，可以降低記憶體使用率

-   可讓更多使用者同時連線至 SSL 網站