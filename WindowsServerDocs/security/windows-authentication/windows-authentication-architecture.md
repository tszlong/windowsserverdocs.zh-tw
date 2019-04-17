---
title: "Windows 驗證架構"
description: "Windows Server 安全性"
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-windows-auth
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: fd6e2cbc233a91b62e3c8c9caf1154f91c3863ac
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="windows-authentication-architecture"></a>Windows 驗證架構

>適用於：Windows Server（以每年次管道）、Windows Server 2016

適用於 IT 專業人員此概觀主題解釋 Windows 驗證的基本架構配置。

驗證是處理程序，讓系統驗證使用者登入或登入資訊。 使用者名稱和密碼比較授權的清單，以及存取權的使用者清單中指定的範圍授與系統偵測到相符項目。

延伸架構的一部分，在 Windows Server 作業系統實作驗證安全性支援提供者，包括交涉、Kerberos 通訊協定，NTLM、Schannel（安全通道），以及摘要的預設設定。 使用這些提供者通訊協定可讓使用者、電腦及服務的驗證，驗證程序可讓使用者的授權與服務存取資源在安全的方式。

在 Windows Server、應用程式使用驗證使用者 SSPI 提取電話驗證。 因此，開發人員不需要了解特定驗證通訊協定的複雜或驗證通訊協定建置到他們的應用程式。

Windows Server 作業系統整套組成 Windows 安全性模型安全性元件。 這些元件確保的應用程式無法獲得資源驗證而授權的存取權。 下列章節描述驗證架構的項目。

### <a name="local-security-authority"></a>本機安全性授權
本機安全性授權單位 (LSA) 是受保護的子系統的驗證並登入本機電腦的使用者。 此外，LSA 保留的相關資訊各方面的本機安全性在電腦上（這些層面統稱為本機安全性原則）。 它也會提供各種不同名稱和安全性識別碼 (Sid) 翻譯服務。

安全性子系統記錄的安全性原則和帳號的電腦系統上。 在網域控制器這些原則和帳號是指的網域控制站位於的網域才會生效。 這些原則和帳號儲存在 Active Directory 中。 LSA 子系統提供服務驗證物件的存取權，檢查使用者權限，以及產生稽核訊息。

### <a name="security-support-provider-interface"></a>安全性支援提供者介面
安全性支援提供者介面 (SSPI) 是取得整合式的安全性驗證，訊息完整性、 訊息隱私權，安全性品質服務分散式應用程式中的任何通訊協定服務的 API。

SSPI 是實作的一般安全性服務 API (GSSAPI)。 SSPI 提供分散式應用程式可以依據呼叫安全性提供者以取得驗證的連結，而不知道的通訊協定的安全性的詳細資訊的其中一個機制。

## <a name="see-also"></a>也了

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [在 Windows 驗證認證處理程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證技術概觀](https://technet.microsoft.com/library/dn169029.aspx)


