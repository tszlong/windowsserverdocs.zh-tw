---
title: Windows 驗證架構
description: Windows Server 安全性
ms.topic: article
ms.assetid: 07c9d6bb-9b03-407d-89b6-97c7551b256b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 7f2ad45a12f8542e4af9a77a9dc76a9596df2143
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638720"
---
# <a name="windows-authentication-architecture"></a>Windows 驗證架構

>適用於：Windows Server (半年度管道)、Windows Server 2016

IT 專業人員的本總覽主題說明 Windows 驗證的基本架構配置。

驗證是系統用來驗證使用者登入或登入資訊的程式。 使用者的名稱和密碼會與授權清單進行比較，如果系統偵測到相符項，則會將存取權授與給該使用者的許可權清單中指定的範圍。

在可延伸的架構中，Windows Server 作業系統會採用一組預設的驗證安全性支援提供者，包括 Negotiate、Kerberos 通訊協定、NTLM、Schannel (安全通道) 和摘要。 這些提供者使用的通訊協定可讓使用者、電腦和服務進行驗證，而驗證程式可讓授權的使用者和服務以安全的方式存取資源。

在 Windows Server 中，應用程式會使用 SSPI 來對驗證進行抽象呼叫，以驗證使用者。 因此，開發人員不需要瞭解特定驗證通訊協定的複雜性，也不需要在應用程式中建立驗證通訊協定。

Windows Server 作業系統包含組成 Windows 安全性模型的一組安全性元件。 這些元件可確保應用程式無法在不需要驗證和授權的情況下取得資源的存取權。 下列各節說明驗證架構的元素。

### <a name="local-security-authority"></a>本機安全性授權
 (LSA) 的「本地安全機構」是受保護的子系統，可驗證使用者並將其登入本機電腦。 此外，LSA 也會維護電腦上本機安全性的所有層面的相關資訊 (這些層面統稱為本機安全性原則) 。 它也提供各種服務，可在名稱與安全識別碼之間轉譯 (Sid) 。

安全性子系統會持續追蹤電腦系統上的安全性原則和帳戶。 在網域控制站的情況下，這些原則和帳戶是對網域控制站所在網域有效的原則和帳戶。 這些原則和帳戶會儲存在 Active Directory 中。 LSA 子系統提供的服務可驗證物件的存取權、檢查使用者權限，以及產生審核訊息。

### <a name="security-support-provider-interface"></a>安全性支援提供者介面
安全性支援提供者介面 (SSPI) 是一種 API，可針對任何分散式應用程式協定取得驗證、訊息完整性、訊息隱私權和安全性服務品質的整合式安全性服務。

SSPI 是 (GSSAPI) 的一般安全性服務 API 的實作為。 SSPI 提供一種機制，讓分散式應用程式可以呼叫其中一個安全性提供者來取得已驗證的連線，而不需要知道安全性通訊協定的詳細資料。

## <a name="additional-references"></a>其他參考資料

-   [安全性支援提供者介面架構](security-support-provider-interface-architecture.md)

-   [Windows 驗證中的認證程序](credentials-processes-in-windows-authentication.md)

-   [Windows 驗證技術概觀](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dn169029(v=ws.10))