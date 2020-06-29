---
title: TLS/SSL 總覽（Schannel SSP）
description: Windows Server 安全性
ms.prod: windows-server
ms.technology: security-tls-ssl
ms.topic: article
ms.assetid: 1b7b0432-1bef-4912-8c9a-8989d47a4da9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 0d963116fc9f22482398b38482f0c3c49f4be505
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475515"
---
# <a name="tlsssl-overview-schannel-ssp"></a>TLS/SSL 總覽（Schannel SSP）

>適用於：Windows Server (半年通道)、Windows Server 2016、Windows 10

本主題適用于 IT 專業人員，透過描述實際應用、Microsoft 的實行變更和軟體需求，以及 Windows Server 2012 和 Windows 8 的其他資源，來介紹 Windows 中的 TLS 和 SSL 的使用安全。

## <a name="description"></a><a name="BKMK_OVER"></a>描述
安全通道是安全性支援提供者 (SSP)，可以實作安全通訊端層 (SSL) 和傳輸層安全性 (TLS) 網際網路標準驗證通訊協定。

安全性支援提供者介面 (SSPI) 是 Windows 系統所使用的 API，可以執行包含驗證在內的安全性相關功能。 SSPI 可作為數個 Ssp 的通用介面，包括 Schannel SSP。

TLS 版本1.0、1.1 和1.2、SSL 版本2.0 和3.0，以及資料包傳輸層安全性 \( DTLS \) 通訊協定版本1.0 和私人通訊傳輸 \( 百分比 \) 通訊協定是以公開金鑰密碼編譯為基礎。 安全通道驗證通訊協定組合提供這些通訊協定。 所有的安全通道通訊協定都使用用戶端/伺服器模型。

## <a name="applications"></a><a name="BKMK_APP"></a>應用程式
當您管理網路時，其中一個問題就是保護在不受信任網路上的應用程式之間傳送的資料。 您可以使用 TLS 和 SSL 來驗證服務器和用戶端電腦，然後使用通訊協定來加密已驗證的合作物件之間的訊息。

例如，您可以針對下列項目使用 TLS/SSL：

-   與電子商務網站的 SSL 安全保護交易
-   已驗證的用戶端對於 SSL 安全保護網站的存取
-   遠端存取
-   SQL 存取
-   電子郵件

## <a name="requirements"></a><a name="BKMK_SOFT"></a>滿足
TLS 和 SSL 通訊協定會使用用戶端/伺服器模型，並以憑證驗證為基礎，這需要公開金鑰基礎結構。

## <a name="server-manager-information"></a><a name="BKMK_INSTALL"></a>伺服器管理員資訊
執行 TLS、SSL 或 Schannel 不需要任何設定步驟。

## <a name="additional-references"></a>其他參考 ##

-   [安全通道安全性封裝](https://docs.microsoft.com/windows/desktop/com/schannel)
-   [安全通道](https://docs.microsoft.com/windows/desktop/SecAuthN/secure-channel) \(英文\)
-   [傳輸層安全性通訊協定](https://docs.microsoft.com/windows/desktop/SecAuthN/transport-layer-security-protocol)
