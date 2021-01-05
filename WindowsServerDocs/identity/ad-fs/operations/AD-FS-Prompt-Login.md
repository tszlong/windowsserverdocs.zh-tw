---
title: AD FS prompt = login
description: 瞭解 AD FS 中可用之 prompt = login 參數的原生支援。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.openlocfilehash: 4ef4c832dab13f156b5a0bf5b7dc8ea2a07d36c5
ms.sourcegitcommit: 9e19436bd8b20af60284071ab512405aebfbec83
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/29/2020
ms.locfileid: "97811435"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 同盟服務提示=登入參數支援

下列檔說明 AD FS 中可用之 prompt = login 參數的原生支援。

## <a name="what-is-promptlogin"></a>什麼是 prompt = login？

當應用程式需要要求 Azure AD 的全新驗證時，也就是需要 Azure AD 重新驗證使用者，即使使用者已通過驗證，他們仍可將 `prompt=login` 參數傳送至 Azure AD 作為驗證要求的一部分。

當此要求適用于同盟使用者時，Azure AD 必須通知 IdP （例如 AD FS），要求是進行全新驗證。

根據預設，Azure AD 會 `prompt=login` `wfresh=0` 在傳送 `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` 此類型的驗證要求至同盟 IdP 時轉譯。

這些參數代表：

- `wfresh=0`：進行全新驗證
- `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`：使用使用者名稱/密碼進行全新的驗證要求

這可能會導致公司內部網路和多重要素驗證案例發生問題，而使用者名稱和密碼以外的驗證類型 `wauth` 是必要的。

Windows Server 2012 R2 中的 AD FS （含7月2016更新彙總套件）引進了參數的原生支援 `prompt=login` 。 這表示現在 Azure AD 可以將此參數依原樣傳送給 AD FS 服務，作為 Azure AD 和 Office 365 驗證要求的一部分。

## <a name="ad-fs-versions-that-support-promptlogin"></a>支援 prompt = login 的 AD FS 版本

以下是支援參數的 AD FS 版本清單 `prompt=login` 。

- Windows Server 2012 R2 中的 AD FS （含7月2016更新彙總套件）
- Windows Server 2016 中的 AD FS

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>如何設定同盟網域以傳送 prompt = login 給 AD FS

使用 Azure AD PowerShell 模組來設定此設定。

> [!NOTE]
> `prompt=login`屬性) 所啟用的功能 (`PromptLoginBehavior` 目前僅適用于[Azure AD Powershell 模組的1.0 版](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，其中 Cmdlet 的名稱包含 "Msol"，例如 get-msoldomainfederationsettings。  目前無法透過 Azure AD PowerShell 模組的 ' version 2.0 ' 取得，其 Cmdlet 的名稱如 "AzureAD \* "。

1. 藉 `PreferredAuthenticationProtocol` `SupportsMfa` `PromptLoginBehavior` 由執行下列 PowerShell 命令，先取得同盟網域的目前值：

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> 依預設，的輸出不 `Get-MsolDomainFederationSettings` 會在主控台中顯示某些屬性。 若要查看所有屬性，您應該傳送管線 (`|`) 它的輸出到， `Format-List *` 以強制物件的所有屬性輸出。

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> 如果屬性的值 `PromptLoginBehavior` 是空的 (`$null`) 會使用的行為 `TranslateToFreshPasswordAuth` 。

2. 藉 `PromptLoginBehavior` 由執行下列命令來設定所需的值：

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

以下是參數的可能值 `PromptLoginBehavior` 及其意義：

- **TranslateToFreshPasswordAuth**：表示轉譯為和的預設 Azure AD `prompt=login` 行為 `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` `wfresh=0` 。
- **NativeSupport**：表示 `prompt=login` 參數將依原樣傳送給 AD FS。 如果 AD FS 在 Windows Server 2012 R2 中，並具有7月2016更新彙總套件或更高版本，則建議使用此值。
- **Disabled**：表示只 `wfresh=0` 傳送至 AD FS。
