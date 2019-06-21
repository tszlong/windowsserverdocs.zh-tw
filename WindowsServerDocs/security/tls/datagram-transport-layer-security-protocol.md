---
title: 資料包傳輸層安全性通訊協定
description: Windows Server 安全性
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
ms.date: 05/16/2018
ms.openlocfilehash: 6f8d7d10ccaace0f75bb470647e3f4571940d9c0
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284319"
---
# <a name="datagram-transport-layer-security-protocol"></a>資料包傳輸層安全性通訊協定

Windows Server （半年通道），Windows Server 2016 中，Windows 10

這個參考主題適合 IT 專業人員說明資料包傳輸層安全性 (DTLS) 通訊協定，也就是組件的安全通道安全性支援提供者 (SSP)。

## <a name="BKMK_DTLS"></a>
在安全通道 SSP 在 Windows Server 2012 和 Windows 8 中引進，DTLS 通訊協定提供通訊私密性為資料包通訊協定。 有關哪些 DTLS 版本支援 Windows 版本中的資訊，請參閱[TLS/ssl (安全通道 SSP) 的通訊協定](https://msdn.microsoft.com/library/windows/desktop/mt808159(v=vs.85).aspx)。 這個通訊協定讓用戶端和伺服器應用程式能夠以設計用來防止竊聽、竄改或訊息偽造的方式來通訊。 DTLS 通訊協定以傳輸層安全性 (TLS) 通訊協定為基礎，並提供對等的安全性保證，減少使用 IPsec 或設計自訂應用程式層安全性通訊協定的需要。

資料包常見於串流處理媒體，例如遊戲或安全的視訊會議。 開發人員可以開發應用程式使用 Windows 驗證安全性支援提供者介面 (SSPI) 模型的內容中的 DTLS 通訊協定來保護用戶端與伺服器之間的通訊。 DTLS 通訊協定已內建的使用者資料包通訊協定 (UDP) 之上。 DTLS 被設計為與 TLS 類似盡可能，為了減少新的安全性發明，並最大化程式碼和基礎結構的重複使用的數量。

可供設定的加密套件是以月份為模式之後，您可以設定 TLS。 不允許使用 RC4。 安全通道會繼續使用 Cryptography Next Generation (CNG)。 這會利用 FIPS 140 憑證，Windows Vista 中引進。

## <a name="see-also"></a>另請參閱

[IETF RFC 4347 資料包傳輸層安全性](http://tools.ietf.org/html/rfc4347)


