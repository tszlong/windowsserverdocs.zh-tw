---
title: TLS/SSL 概觀 (安全通道 SSP)
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: a6571e5e06e07fd62ad4cf39bab322b45c90a9f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848599"
---
# <a name="tlsssl-overview-schannel-ssp"></a>TLS/SSL 概觀 (安全通道 SSP)

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows 10

本主題適用於 IT 專業人員介紹透過描述實際應用，使用安全通道安全性服務提供者 (SSP) 的 Windows 中 TLS 和 SSL 實作的變更在 Microsoft 實作中，與軟體需求，再加上Windows Server 2012 和 Windows 8 的其他資源。

## <a name="BKMK_OVER"></a>描述
安全通道是安全性支援提供者 (SSP)，可以實作安全通訊端層 (SSL) 和傳輸層安全性 (TLS) 網際網路標準驗證通訊協定。

安全性支援提供者介面 (SSPI) 是 Windows 系統所使用的 API，可以執行包含驗證在內的安全性相關功能。 SSPI 可以做幾個 Ssp，包括安全通道 SSP 的通用介面

TLS 1.0、 1.1 和 1.2 版，SSL 2.0 和 3.0 版，以及資料包傳輸層安全性\(DTLS\)通訊協定版本 1.0，以及私人通訊傳輸\(PCT\)通訊協定為基礎公開金鑰密碼編譯。 安全通道驗證通訊協定組合提供這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。

## <a name="BKMK_APP"></a>應用程式
當您管理網路時遇到的一個問題是，要保護在未受信任網路的應用程式之間傳送的資料安全。 您可以使用 TLS 和 SSL 來驗證伺服器和用戶端電腦，然後使用通訊協定加密已驗證對象之間的訊息。

例如，您可以針對下列項目使用 TLS/SSL：

-   與電子商務網站的 SSL 安全保護交易
-   已驗證的用戶端對於 SSL 安全保護網站的存取
-   遠端存取
-   SQL 存取
-   電子郵件

## <a name="BKMK_SOFT"></a>需求
TLS 和 SSL 通訊協定會使用用戶端/伺服器模型，並以憑證驗證，而這需要公開金鑰基礎結構為基礎。

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
沒有實作 TLS、 SSL 或安全通道所需的組態步驟。

## <a name="see-also"></a>另請參閱 ##

-   [安全通道安全性封裝](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [安全通道](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [傳輸層安全性通訊協定](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
