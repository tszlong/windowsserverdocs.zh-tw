---
title: 資料包傳輸層安全性通訊協定
description: Windows Server 安全性
ms.topic: article
ms.assetid: 57b8873a-ad9c-4f2c-93e0-a2af352c6965
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/16/2018
ms.openlocfilehash: 2390fc9c688f60d6e4887ff9ee75183ae4333aa2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87994587"
---
# <a name="datagram-transport-layer-security-protocol"></a>資料包傳輸層安全性通訊協定

Windows Server (半年通道)、Windows Server 2016、Windows 10

這個適用于 IT 專業人員的參考主題說明資料包傳輸層安全性 (DTLS) 通訊協定，這是安全通道安全性支援提供者 (SSP) 的一部分。

## <a name="BKMK_DTLS"></a>
DTLS 通訊協定是在 Windows Server 2012 和 Windows 8 中的安全通道 SSP 中引進的，可為資料包通訊協定提供通訊隱私權。 如需 Windows 版本中支援哪些 DTLS 版本的相關資訊，請參閱[TLS/SSL (SCHANNEL SSP) 中的通訊協定](/windows/win32/secauthn/protocols-in-tls-ssl--schannel-ssp-)。 這個通訊協定讓用戶端和伺服器應用程式能夠以設計用來防止竊聽、竄改或訊息偽造的方式來通訊。 DTLS 通訊協定以傳輸層安全性 (TLS) 通訊協定為基礎，並提供對等的安全性保證，減少使用 IPsec 或設計自訂應用程式層安全性通訊協定的需要。

資料包在串流媒體中很常見，例如遊戲或安全的視訊會議。 開發人員可以開發應用程式，在 Windows 驗證安全性支援提供者介面的內容中使用 DTLS 通訊協定， (SSPI) 模型來保護用戶端與伺服器之間的通訊。 DTLS 通訊協定是以使用者資料包通訊協定為基礎， (UDP) 。 DTLS 的設計是盡可能與 TLS 相似，以將新的安全性家發明降到最低，並將程式碼和基礎結構重複使用的數量最大化。

可供設定使用的加密套件，會在您可以針對 TLS 進行設定的程式之後進行配置。 不允許 RC4。 Schannel 會繼續使用新一代密碼編譯 (CNG) 。 這會利用 Windows Vista 中引進的 FIPS 140 認證。

## <a name="additional-references"></a>其他參考資料

[IETF RFC 4347 資料包傳輸層安全性](http://tools.ietf.org/html/rfc4347)