---
title: Windows 驗證架構
description: Windows Server 安全性
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 4a9deef6481c1f7dacb56e8166584de1c59d613c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403275"
---
# <a name="windows-authentication-architecture"></a>Windows 驗證架構

>適用於：Windows Server (半年度管道)、Windows Server 2016

這個適用于 IT 專業人員的總覽主題說明 Windows 驗證的基本架構配置。

驗證是系統用來驗證使用者登入或登入資訊的程式。 使用者的名稱和密碼會與授權清單相比較，如果系統偵測到相符的情況，就會將存取權授與給該使用者的許可權清單中指定的範圍。

作為可擴充架構的一部分，Windows Server 作業系統會執行一組預設的驗證安全性支援提供者，其中包括 Negotiate、Kerberos 通訊協定、NTLM、Schannel （安全通道）和摘要。 這些提供者所使用的通訊協定可以驗證使用者、電腦和服務，而驗證程式可讓授權的使用者和服務以安全的方式存取資源。

在 Windows Server 中，應用程式會使用 SSPI 對驗證進行抽象呼叫來驗證使用者。 因此，開發人員不需要瞭解特定驗證通訊協定的複雜性，或在其應用程式中建立驗證通訊協定。

Windows Server 作業系統包含組成 Windows 安全性模型的一組安全性元件。 這些元件可確保應用程式無法在沒有驗證和授權的情況下取得資源的存取權。 下列各節說明驗證架構的元素。

### <a name="local-security-authority"></a>本機安全性授權
「本地安全機構」（LSA）是一個受保護的子系統，可驗證和登入本機電腦的使用者。 此外，LSA 也會維護電腦上本機安全性所有層面的相關資訊（這些層面統稱為「本機安全性原則」）。 它也會提供各種服務，以便在名稱和安全識別碼（Sid）之間進行轉譯。

安全性子系統會追蹤安全性原則和電腦系統上的帳戶。 在網域控制站的情況下，這些原則和帳戶就是網域控制站所在網域的作用中。 這些原則和帳戶會儲存在 Active Directory 中。 LSA 子系統提供用來驗證物件存取權、檢查使用者權限，以及產生 audit 訊息的服務。

### <a name="security-support-provider-interface"></a>安全性支援提供者介面
安全性支援提供者介面（SSPI）是針對任何分散式應用程式通訊協定取得整合式安全性服務以進行驗證、訊息完整性、訊息隱私權和安全性服務品質的 API。

SSPI 是一般安全性服務 API （GSSAPI）的執行。 SSPI 提供一種機制，讓分散式應用程式可以呼叫其中一個安全性提供者來取得已驗證的連線，而不需要知道安全性通訊協定的詳細資料。

## <a name="see-also"></a>另請參閱

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [Windows 驗證中的認證程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證技術概觀](https://technet.microsoft.com/library/dn169029.aspx)


