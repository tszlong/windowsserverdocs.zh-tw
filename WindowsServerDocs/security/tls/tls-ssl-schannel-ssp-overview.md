---
title: "TLS-SSL (Schannel SSP) 概觀"
description: "Windows Server 安全性"
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
ms.date: 10/12/2016
ms.openlocfilehash: afd0b70264dba1e720f95e40d3d201c2c5bf1c64
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="tls---ssl-schannel-ssp-overview"></a>TLS-SSL (Schannel SSP) 概觀

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題適用於 IT 專業人員導入 TLS 日 SSL 實作使用 Schannel 安全性服務提供者 (SSP)，描述實用的應用程式，Microsoft 實作，與軟體需求，以及其他資源變更適用於 Windows Server 2012 和 Windows 8 的 windows。

**您表示：**

-   [Schannel 安全性套件](https://msdn.microsoft.com/library/ms678421.aspx)

-   [安全通道](https://msdn.microsoft.com/library/windows/desktop/aa380123.aspx)

-   [傳輸層安全性通訊協定](https://msdn.microsoft.com/library/windows/desktop/aa380516.aspx)

## <a name="BKMK_OVER"></a>TLS\SSL \(Schannel\) 描述
Schannel 是實作的安全通訊端層 \(SSL\) 和 Tls \(TLS\) 安全性支援提供者 \(SSP\) 網際網路標準驗證通訊協定。

安全性支援提供者介面 \(SSPI\) 是用來執行 security\ 相關的功能包括驗證 windows API。 做為數個安全性支援提供者 \(SSPs\)，包括 Schannel SSP.常見介面 SSPI 函式

1.0，1.1、1.2 安全通訊端層 \(SSL\) 通訊協定，2.0 和 3.0 資料流 Tls \(DTLS\) 1.0 版，並私人通訊傳輸 \(PCT\) 通訊協定的 Tls \(TLS\) 通訊協定版本根據公開加密。 安全性通道 \(Schannel\) 驗證通訊協定提供這些通訊協定。 所有 Schannel 通訊協定都使用 client\ 日伺服器模型。

## <a name="BKMK_APP"></a>實用的應用程式
有一個問題，您可以管理網路時保護資料未受信任的網路上的應用程式之間傳送。 您可以使用 TLS\SSL 驗證伺服器和 client 電腦，然後使用通訊協定加密已驗證的對象之間的訊息。

例如，您可以使用適用於 TLS\SSL:

-   SSL\ 保護交易 e\ 商務網站

-   證存取 SSL\ 安全的網站

-   遠端存取

-   SQL 存取

-   E\ 郵件

## <a name="BKMK_SOFT"></a>軟體需求
使用 client\server 模型 TLS\SSL 通訊協定，並為基礎的憑證驗證，這需要公開基礎結構。

## <a name="BKMK_INSTALL"></a>伺服器管理員資訊
實作 TLS、SSL 或 Schannel 所需的任何設定步驟。

