---
title: TLS/SSL 總覽（Schannel SSP）
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 51e3886339b7864951b322d210e1a05bef4dc1e1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403371"
---
# <a name="tlsssl-overview-schannel-ssp"></a>TLS/SSL 總覽（Schannel SSP）

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

本主題適用于 IT 專業人員，透過描述實際應用、Microsoft 的實行變更和軟體需求，在 Windows 仲介紹 TLS 和 SSL 的使用方式，以及Windows Server 2012 和 Windows 8 的其他資源。

## <a name="BKMK_OVER"></a>描述
安全通道是安全性支援提供者 (SSP)，可以實作安全通訊端層 (SSL) 和傳輸層安全性 (TLS) 網際網路標準驗證通訊協定。

安全性支援提供者介面 (SSPI) 是 Windows 系統所使用的 API，可以執行包含驗證在內的安全性相關功能。 SSPI 可作為數個 Ssp 的通用介面，包括 Schannel SSP。

TLS 版本1.0、1.1 和1.2、SSL 版本2.0 和3.0，以及資料包傳輸層安全性 \(DTLS\) 通訊協定版本1.0 和私人通訊傳輸 \(PCT\) 通訊協定是以公開金鑰密碼編譯為基礎。 安全通道驗證通訊協定組合提供這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。

## <a name="BKMK_APP"></a>應用程式
當您管理網路時遇到的一個問題是，要保護在未受信任網路的應用程式之間傳送的資料安全。 您可以使用 TLS 和 SSL 來驗證服務器和用戶端電腦，然後使用通訊協定來加密已驗證的合作物件之間的訊息。

例如，您可以針對下列項目使用 TLS/SSL：

-   與電子商務網站的 SSL 安全保護交易
-   已驗證的用戶端對於 SSL 安全保護網站的存取
-   遠端存取
-   SQL 存取
-   電子郵件

## <a name="BKMK_SOFT"></a>滿足
TLS 和 SSL 通訊協定會使用用戶端/伺服器模型，並以憑證驗證為基礎，這需要公開金鑰基礎結構。

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
執行 TLS、SSL 或 Schannel 不需要任何設定步驟。

## <a name="see-also"></a>請參閱 ##

-   [Schannel 安全性套件](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [安全通道](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel)
-   [傳輸層安全性通訊協定](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
