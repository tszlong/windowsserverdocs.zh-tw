---
title: "資料流 Tls 通訊協定"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-tls-ssl
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 31c1cf1f3218c0511a4407b560be30d0c6f86233
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# 資料流 Tls 通訊協定

>適用於：Windows Server（以每年次管道）、Windows Server 2016

這適用於 IT 專業人員的參考主題描述資料流傳輸層級的安全性 (DTLS) 通訊協定，也就是部分 Schannel 安全性支援提供者 (SSP)。

## <a name="BKMK_DTLS"></a>
在 Windows Server 2012 和 Windows 8 中 Schannel SSP 導入了，DTLS 通訊協定提供通訊隱私權，資料流通訊協定。 適用於 Windows 版本相關的 DTLS 支援的版本資訊，請查看[中 TLS SSL (Schannel SSP) 通訊協定](https://msdn.microsoft.com/en-us/library/windows/desktop/mt808159(v=vs.85).aspx)。 通訊協定允許 client 和伺服器應用程式的方式設計用來防止竊取、竄改或冒名訊息通訊。 DTLS 通訊協定為基礎的傳輸層級的安全性 (TLS) 通訊協定，並提供相同的安全保證減少使用 IPsec 或設計的應用程式自訂層安全性通訊協定。

資料包都有的串流媒體，例如遊戲或受保護的影片會議。 開發人員可以開發戶端與伺服器間通訊使用中的 Windows 驗證安全性支援提供者介面 (SSPI) 模型 DTLS 通訊協定的應用程式。 DTLS 通訊協定建置上方使用者資料流通訊協定 (UDP)。 DTLS 被設計為類似 TLS 度將新的安全性發明最小化，並放到最大的驗證碼與基礎結構重複使用。

您可以設定 TLS 這些的模式密碼套件可供設定。 不允許 RC4。 若要使用密碼編譯下一代 (CNG) 持續 Schannel。 這會利用 FIPS 140 認證，在 Windows Vista 中推出。

## 也了

[IETF RFC 4347 資料流 Tls](http://tools.ietf.org/html/rfc4347)


