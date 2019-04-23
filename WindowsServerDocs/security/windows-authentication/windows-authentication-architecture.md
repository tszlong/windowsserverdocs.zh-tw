---
title: Windows 驗證架構
description: Windows Server 安全性
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855859"
---
# <a name="windows-authentication-architecture"></a>Windows 驗證架構

>適用於：Windows Server （半年通道），Windows Server 2016

此概觀主題適用於 IT 專業人員說明 Windows 驗證的基本架構配置。

驗證是供系統會驗證使用者的登入或登入資訊的程序。 使用者的名稱和密碼是針對已授權的清單，而如果系統偵測到相符項目，授與該使用者的權限清單中指定的範圍內的存取權。

可延伸的架構的一部分，Windows Server 作業系統實作一組預設的驗證安全性支援提供者，包括交涉、 Kerberos 通訊協定、 NTLM、 安全通道 （安全通道），以及摘要。 這些提供者所使用的通訊協定啟用驗證的使用者、 電腦及服務，並在驗證程序可讓授權的使用者和服務以安全的方式存取資源。

在 Windows Server 中，應用程式會驗證使用者，藉由使用 SSPI 摘錄進行驗證的呼叫。 因此，開發人員不需要了解特定的驗證通訊協定的複雜性，或在其應用程式中建立驗證通訊協定。

Windows Server 作業系統包含一組構成 Windows 安全性模型的安全性元件。 這些元件會確保應用程式無法取得資源，而不需要驗證和授權的存取權。 下列各節描述驗證架構的元素。

### <a name="local-security-authority"></a>本機安全性授權
本機安全性授權 (LSA) 是一個受保護的子系統，會驗證並登入到本機電腦的使用者。 颾魤 ㄛ LSA 會維護各方面資訊的本機安全性群組的電腦上 （這些層面統稱為 「 本機安全性原則）。 它也提供各種服務的名稱和安全性識別碼 (Sid) 之間的轉譯。

安全性原則和電腦系統上的帳戶，會追蹤的安全性子系統。 在網域控制站，這些原則和帳戶就是作用中的網域控制站所在的網域。 這些原則和帳戶會儲存在 Active Directory 中。 LSA 子系統提供服務，用於驗證物件的存取權，檢查使用者權限，並產生稽核訊息。

### <a name="security-support-provider-interface"></a>安全性支援提供者介面
安全性支援提供者介面 (SSPI) 是取得整合式的安全性進行驗證、 訊息完整性、 訊息隱私性和安全性的服務品質的分散式應用程式中的任何通訊協定的服務的 API。

SSPI 是實作的一般安全性服務 API (GSSAPI)。 SSPI 提供的分散式應用程式可以呼叫其中一個安全性提供者，以取得詳細資料的安全性通訊協定的不知情的情況下驗證的連線的機制。

## <a name="see-also"></a>另請參閱

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [在 Windows 驗證的認證程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證技術概觀](https://technet.microsoft.com/library/dn169029.aspx)


