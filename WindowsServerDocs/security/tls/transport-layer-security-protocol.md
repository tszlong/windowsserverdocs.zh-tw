---
title: 傳輸層安全性通訊協定
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic
ms.date: 05/16/2018
ms.openlocfilehash: 3884d80d1d2f5465e5f3daf708af57b35fab6bd8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853601"
---
# <a name="transport-layer-security-protocol"></a>傳輸層安全性通訊協定

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

本主題適用于 IT 專業人員，說明傳輸層安全性（TLS）通訊協定的運作方式，並提供適用于 TLS 1.0、TLS 1.1 和 TLS 1.2 的 IETF Rfc 連結。

TLS （和 SSL）通訊協定位於應用程式協定層和 TCP/IP 層之間，可在其中保護和傳送應用程式資料至傳輸層。 因為通訊協定會在應用層和傳輸層之間運作，所以 TLS 和 SSL 可以支援多個應用層通訊協定。

TLS 和 SSL 會假設連接導向傳輸（通常是 TCP）正在使用中。 此通訊協定可讓用戶端和伺服器應用程式偵測下列安全性風險：

-   訊息篡改

-   訊息攔截

-   偽造訊息

TLS 和 SSL 通訊協定可以分成兩層。 第一層是由應用程式通訊協定和三個握手通訊協定所組成：交握通訊協定、變更密碼規格通訊協定，以及警示通訊協定。 第二層是「記錄」通訊協定。 下圖說明各種圖層及其元素。

**TLS 和 SSL 通訊協定層**


Schannel SSP 會在不修改的情況下，執行 TLS 和 SSL 通訊協定。 SSL 通訊協定是專屬的，但網際網路工程任務推動小組會產生公用 TLS 規格。 如需 Windows 版本支援哪些 TLS 或 SSL 版本的相關資訊，請參閱[TLS/ssl （安全通道 SSP）中的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出每個 TLS 版本的規格。 每個規格都包含下列資訊：

-   TLS 記錄通訊協定

-   TLS 信號交換通訊協定： \- 變更加密規格通訊協定 \- 警示通訊協定

-   密碼編譯計算

-   強制加密套件

-   應用程式資料通訊協定

[RFC 5246-傳輸層安全性（TLS）通訊協定版本1。2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-傳輸層安全性（TLS）通訊協定版本1。1](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 通訊協定版本1。0](http://tools.ietf.org/html/rfc2246)

## <a name="tls-session-resumption"></a><a name="BKMK_SessionResumption"></a>TLS 會話繼續
在 Windows Server 2012 R2 中引進，Schannel SSP 會執行 TLS 會話繼續的伺服器端部分。 Windows 8 已新增 RFC 5077 的用戶端執行。

將 TLS 連接到伺服器的裝置經常需要重新連接。 TLS 會話繼續會降低建立 TLS 連線的成本，因為繼續牽涉到縮寫的 TLS 交握。 藉由允許一組 TLS 伺服器繼續彼此的 TLS 會話，這有助於更多的恢復嘗試。 這項修改為任何支援 RFC 5077 （包括 Windows Phone 和 Windows RT 裝置）的 TLS 用戶端提供下列節約：

-   減少伺服器上的資源使用量

-   降低頻寬，改善用戶端連線的效率

-   減少因連線繼續而花費的 TLS 交握時間

如需無狀態 TLS 工作階段繼續的詳細資訊，請參閱 IETF 文件 [RFC 5077。](http://www.ietf.org/rfc/rfc5077)

## <a name="application-protocol-negotiation"></a><a name="BKMK_AppProtocolNego"></a>應用程式協定協調
 Windows Server 2012 R2 和 Windows 8.1 引進了允許用戶端 TLS 應用程式協定協調的支援。 應用程式可以利用通訊協定作為 HTTP 2.0 標準開發的一部分，而使用者可以使用執行 SPDY 通訊協定的應用程式來存取 Google 和 Twitter 之類的線上服務。

如需應用程式通訊協定交涉運作方式的詳細資訊，請參閱[傳輸層安全性 (TLS) 應用程式層通訊協定交涉延伸](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="tls-support-for-server-name-indication-extensions"></a><a name="BKMK_SNI"></a>伺服器名稱指示擴充功能的 TLS 支援
伺服器名稱指示器 (SNI) 功能會延伸 SSL 和 TLS 通訊協定，當單一伺服器上有許多虛擬映像正在執行時，允許對伺服器進行適當的識別。 在虛擬裝載案例中，會在一部伺服器上裝載數個網域（各自具有自己的可能相異憑證）。 在此情況下，伺服器無法事先知道要將哪個憑證傳送至用戶端。 SNI 可讓用戶端在通訊協定中稍早通知目標網域，這讓伺服器能夠正確地選取適當的憑證。

這個額外功能：

-   可讓您在單一網際網路通訊協定和埠組合上裝載多個 SSL 網站

-   在單一網頁伺服器上裝載多個 SSL 網站時，可以降低記憶體使用率

-   允許更多使用者同時連接至 SSL 網站



