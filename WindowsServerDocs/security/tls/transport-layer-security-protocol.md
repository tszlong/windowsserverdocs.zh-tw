---
title: "傳輸層級的安全性通訊協定"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: de510bb0-a9f6-4bbe-8f8a-8dd7473bbae8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 94c81a79e5f3fd8fc22eafde3de8bd3d0bdd73f0
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# 傳輸層級的安全性通訊協定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員告訴您如何運作傳輸層級的安全性 (TLS) 通訊協定，並提供 IETF Rfc TLS 1.0，TLS 1.1、TLS 1.2 的連結。

您可以找到之間應用程式的通訊協定層級和 TCP/IP 層級，它們可以在此安全，並將應用程式資料傳送至傳輸層的 TLS（和 SSL）通訊協定。 因為應用程式層之間傳輸層上工作通訊協定，TLS 及 SSL 可支援多個應用程式通訊協定。

TLS 和 SSL 假設連接方向傳輸，通常 TCP 正在使用中。 通訊協定允許偵測以下安全性風險 client 和伺服器應用程式：

-   訊息竄改

-   訊息攔截

-   訊息偽造

將 TLS 及 SSL 通訊協定可分成兩種層級。 第一次層和所組成的應用程式通訊協定的三個交握通訊協定：交換通訊協定，變更密碼規格通訊協定，警示通訊協定。 第二層是記錄通訊協定。 下圖顯示不同層和他們的項目。

**TLS 和 SSL 通訊協定層級**


Schannel SSP 實作修改 TLS 和 SSL 通訊協定。 SSL 通訊協定專屬，但網際網路工程設計工作人員產生公用 TLS 規格。 適用於 Windows 版本的 TLS 或 SSL 支援的版本資訊，請查看[中 TLS SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx)。 下表列出每個 TLS 版本規格。 每個規格包含下列資訊：

-   使用碼表進行 TLS 通訊協定

-   TLS 交握通訊協定: \-變更密碼規格通訊協定 \-警示通訊協定

-   密碼編譯計算

-   管轄密碼套件

-   應用程式資料通訊協定

[RFC 5246-傳輸層級的安全性 (TLS) 通訊協定版本 1.2](http://tools.ietf.org/html/rfc5246)

[RFC 4346-傳輸層級的安全性 (TLS) 通訊協定 1.1 版](http://tools.ietf.org/html/rfc4346)

[RFC 2246-TLS 通訊協定 1.0](http://tools.ietf.org/html/rfc2246)

## <a name="BKMK_SessionResumption"></a>TLS 工作階段回復
在 Windows Server 2012 R2 推出，Schannel SSP 實作 TLS 工作階段回復伺服器端部分。 Windows 8 中加入的 RFC 5077 client 端實作。

經常 TLS 連接到伺服器的裝置需要重新連接。 TLS 工作階段回復降低因為回復包含縮寫的 TLS 交換建立 TLS 連接。 這更多回復嘗試幫助您允許的 TLS 伺服器繼續彼此的 TLS 工作階段群組。 這個修改提供任何支援 RFC 5077，包括 Windows Phone 和 Windows RT 裝置 TLS client 下列節省：

-   在伺服器上資源減少的使用量

-   減少的頻寬，可改善 client 連接的效率

-   減少的因為 resumptions 連接的 TLS 交換所花費的時間

無狀態 TLS 工作階段回復的相關資訊，會看到 IETF 文件[RFC 5077。](http://www.ietf.org/rfc/rfc5077)

## <a name="BKMK_AppProtocolNego"></a>應用程式通訊協定交涉
 Windows Server 2012 R2 和 Windows 8.1 導入了可 client 端 TLS 應用程式通訊協定交涉的支援。 應用程式可如何利用通訊協定 HTTP 2.0 標準開發的一部分，使用者可以使用應用程式執行 SPDY 通訊協定存取 online services Google 和 Twitter。

適用於應用程式通訊協定交涉的運作方式的相關資訊，請查看[傳輸層級的安全性 (TLS) 應用程式層通訊協定交涉延伸](http://tools.ietf.org/search/draft-ietf-tls-applayerprotoneg-05)。

## <a name="BKMK_SNI"></a>TLS 伺服器名稱指示擴充功能的支援
[伺服器名稱指示 (SNI) 功能延伸 SSL 和 TLS 通訊協定，允許伺服器的適當驗證時單一伺服器上執行許多虛擬映像。 在 virtual 裝載案例中，數個（每一個都有它自己可能不同的憑證）的網域裝載一部伺服器上。 在這種情形下，伺服器必須事先得知 client 來傳送的憑證。 SNI 可 client 通知目標網域稍早的通訊協定，和這可以讓伺服器正確選取適當的憑證。

此額外的功能：

-   可讓您在主機上的單一網際網路通訊協定和連接埠組合多 SSL 網站

-   在單一網頁伺服器裝載多 SSL 網站時，可減少記憶體使用量

-   可讓使用者更多同時連接 SSL 網站



