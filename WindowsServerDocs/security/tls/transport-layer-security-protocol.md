---
title: 傳輸層安全性通訊協定
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: justinha
ms.author: justinha
manager: brianlic-msft
ms.date: 05/16/2018
ms.openlocfilehash: 0a3b241fe0d2a61361d551b7f515507ad55d71cd
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284233"
---
# <a name="transport-layer-security-protocol"></a>傳輸層安全性通訊協定

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

本主題適用於 IT 專業人員說明傳輸層安全性 (TLS) 通訊協定的運作方式，並提供連結 IETF rfc TLS 1.0、 1.1 和 TLS 1.2。

TLS （和 SSL） 通訊協定是位於應用程式通訊協定層與 TCP/IP 層，它們可以在此保護，並將應用程式資料傳送至傳輸層之間。 因為通訊協定運作的應用程式層和傳輸層之間，TLS 和 SSL 可以支援多個應用程式層通訊協定。

TLS 和 SSL 會假設為連線導向傳輸，通常是 TCP，正在使用中。 通訊協定可讓用戶端和伺服器應用程式偵測到下列的安全性風險：

-   訊息遭到竄改

-   訊息的攔截

-   訊息偽造

TLS 和 SSL 通訊協定可以分成兩個層級。 第一層所組成的應用程式通訊協定和三個交握通訊協定： 交握通訊協定、 變更密碼規格通訊協定，以及警示的通訊協定。 第二層是記錄通訊協定。 下圖顯示各種層級和其項目。

**TLS 和 SSL 通訊協定層**


安全通道 SSP 實作 TLS 和 SSL 通訊協定，而不需修改。 SSL 通訊協定是專屬的但 Internet Engineering Task Force 會產生公用 TLS 規格。 哪些 TLS 或 SSL 版本支援 Windows 版本中的資訊，請參閱[TLS/ssl (安全通道 SSP) 的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出每個 TLS 版本規格。 每個規格包含下列資訊：

-   TLS 記錄通訊協定

-   TLS 交握通訊協定：\- 變更加密規格通訊協定\-警示通訊協定

-   密碼編譯計算

-   必要的加密套件

-   應用程式資料通訊協定

[RFC 5246-傳輸層安全性 (TLS) 通訊協定 1.2 版](http://tools.ietf.org/html/rfc5246)

[RFC 4346-傳輸層安全性 (TLS) 通訊協定 1.1 版](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 通訊協定 1.0 版](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>TLS 工作階段繼續
Windows Server 2012 R2 中引入，安全通道 SSP 實作 TLS 工作階段繼續的伺服器端部分。 Windows 8 中，已新增 RFC 5077 的用戶端端實作。

將 TLS 連接到伺服器的裝置經常需要重新連接。 TLS 工作階段繼續可降低建立 TLS 連線，因為繼續涉及縮寫的 TLS 信號交換的成本。 這有助於藉由使用一組 TLS 伺服器繼續彼此的 TLS 工作階段的更多繼續嘗試。 這項修改為支援 RFC 5077，包括 Windows Phone 和 Windows RT 裝置的任何 TLS 用戶端提供省下的下列：

-   減少伺服器上的資源使用量

-   降低頻寬，改善用戶端連線的效率

-   減少因繼續連線的 TLS 信號交換所花費的時間

如需無狀態 TLS 工作階段繼續的詳細資訊，請參閱 IETF 文件 [RFC 5077。](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>應用程式通訊協定交涉
 Windows Server 2012 R2 和 Windows 8.1 引進可讓用戶端 TLS 應用程式通訊協定交涉的支援。 應用程式可以利用 HTTP 2.0 標準開發中，一部分的通訊協定，使用者可以使用執行 SPDY 通訊協定的應用程式存取線上服務，例如 Google 和 Twitter。

如需應用程式通訊協定交涉運作方式的詳細資訊，請參閱[傳輸層安全性 (TLS) 應用程式層通訊協定交涉延伸](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="BKMK_SNI"></a>對於伺服器名稱指示延伸的 TLS 支援
伺服器名稱指示器 (SNI) 功能會延伸 SSL 和 TLS 通訊協定，當單一伺服器上有許多虛擬映像正在執行時，允許對伺服器進行適當的識別。 在虛擬裝載案例中，一部伺服器上裝載數個網域 （各有其本身可能完全不同的憑證）。 在此情況下，伺服器便無法預先得知哪個憑證傳送給用戶端。 SNI 允許用戶端通知目標網域，稍早的通訊協定，而這讓伺服器能夠正確地選取適當的憑證。

這個額外功能：

-   可讓您裝載在單一的網際網路通訊協定和連接埠 」 組合的多個 SSL 網站

-   在單一網頁伺服器上裝載多個 SSL 網站時，可以降低記憶體使用率

-   可讓多個使用者同時連線到 SSL 網站



