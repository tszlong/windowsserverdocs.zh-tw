---
title: 資料包傳輸層安全性通訊協定
description: Windows Server 安全性
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 05/16/2018
ms.openlocfilehash: 1cf6c9b504a246f9b0abe344bc596434c0319c3a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640386"
---
# <a name="datagram-transport-layer-security-protocol"></a>資料包傳輸層安全性通訊協定

Windows Server (半年通道)、Windows Server 2016、Windows 10

IT 專業人員的本參考主題描述資料包傳輸層安全性 (DTLS) 通訊協定，這是安全通道安全性支援提供者 (SSP) 的一部分。

## <a name="BKMK_DTLS"></a>
DTLS 通訊協定是在 Windows Server 2012 和 Windows 8 的安全通道 SSP 中引進的，可提供資料包通訊協定的通訊隱私權。 如需 Windows 版本所支援之 DTLS 版本的相關資訊，請參閱 [TLS/SSL (SCHANNEL SSP) 中的通訊協定 ](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-)。 這個通訊協定讓用戶端和伺服器應用程式能夠以設計用來防止竊聽、竄改或訊息偽造的方式來通訊。 DTLS 通訊協定以傳輸層安全性 (TLS) 通訊協定為基礎，並提供對等的安全性保證，減少使用 IPsec 或設計自訂應用程式層安全性通訊協定的需要。

資料包常見於串流媒體，例如遊戲或受保護的視訊會議。 開發人員可以開發應用程式，在 Windows 驗證的安全性支援提供者介面的內容中使用 DTLS 通訊協定 (SSPI) 模型，以保護用戶端與伺服器之間的通訊。 DTLS 通訊協定是以使用者資料包協定為基礎， (UDP) 。 DTLS 的設計盡可能與 TLS 類似，可將新的安全性發明降至最低，並將程式碼和基礎結構重複使用的數量最大化。

適用于設定的加密套件會在您可以為 TLS 設定的加密套件之後進行。 不允許 RC4。 Schannel 會繼續使用新一代密碼編譯 (CNG) 。 這會利用 Windows Vista 引進的 FIPS 140 認證。

## <a name="additional-references"></a>其他參考資料

[IETF RFC 4347 資料包傳輸層安全性](http://tools.ietf.org/html/rfc4347)