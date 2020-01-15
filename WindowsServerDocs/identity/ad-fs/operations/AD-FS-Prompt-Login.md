---
title: AD FS 提示 = 登入
description: AD FS 2016 的常見問題
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cb91bb61adf97fee6f157ca44eb657e20670a1e7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948688"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 同盟服務提示 = 登入參數支援

下列檔描述 AD FS 中提供之 prompt = login 參數的原生支援。

## <a name="what-is-promptlogin"></a>什麼是提示 = 登入？

當應用程式需要向 Azure AD 要求全新驗證時，表示他們需要 Azure AD 重新驗證使用者，即使使用者已通過驗證，他們也可以將 `prompt=login` 參數傳送到 Azure AD 做為驗證要求的一部分。

當此要求適用于同盟使用者時，Azure AD 需要通知 IdP （例如 AD FS）要求是否為全新驗證。

根據預設，Azure AD 會在傳送此類型的驗證要求給同盟 IdP 時，將 `prompt=login` 轉譯為 `wfresh=0` 和 `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`。

這些參數表示：

- `wfresh=0`：進行全新驗證
- `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`：使用使用者名稱/密碼進行全新的驗證要求

這可能會造成公司內部網路和多重要素驗證案例的問題，其中 `wauth` 參數所要求的驗證類型不是使用者名稱和密碼。  

Windows Server 2012 R2 中的 AD FS 與7月2016更新彙總套件引進了 `prompt=login` 參數的原生支援。 這表示現在 Azure AD 可以在 Azure AD 和 Office 365 驗證要求中，AD FS 服務的方式傳送此參數。

## <a name="ad-fs-versions-that-support-promptlogin"></a>支援 prompt = login 的 AD FS 版本

以下是支援 `prompt=login` 參數的 AD FS 版本清單。

- Windows Server 2012 R2 中的 AD FS 加上2016年7月更新彙總套件
- Windows Server 2016 中的 AD FS

## <a name="how-to-configure-a-federated-domain-to-send-promptlogin-to-ad-fs"></a>如何設定同盟網域來傳送 prompt = 登入 AD FS

使用 Azure AD PowerShell 模組來進行設定。

> [!NOTE]
> `prompt=login` 功能（由 `PromptLoginBehavior` 屬性啟用）目前僅適用于[1.0 版的 Azure AD Powershell 模組](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，其中 Cmdlet 的名稱會包含 "Msol"，例如 set-msoldomainfederationsettings。  目前無法透過 Azure AD PowerShell 模組的 ' version 2.0 ' 取得，其 Cmdlet 的名稱類似 "AzureAD\*"。

1. 藉由執行下列 PowerShell 命令，先取得同盟網域的 `PreferredAuthenticationProtocol`、`SupportsMfa`和 `PromptLoginBehavior` 的目前值：

```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | Format-List *
```

> [!NOTE]
> `Get-MsolDomainFederationSettings` 的輸出預設不會在主控台中顯示特定屬性。 若要查看所有屬性，您應該以管線（`|`）將其輸出傳送至 `Format-List *`，以強制執行物件所有屬性的輸出。

![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)

> [!NOTE]
> 如果 `PreferredAuthenticationMethod` 屬性是空的（`$null`），則表示 `TranslateToFreshPasswordAuth`的預設行為。

2. 執行下列命令，以設定所需的 `PromptLoginBehavior` 值：

```powershell
    Set-MsolDomainFederationSettings –DomainName <your_domain_name> -PreferredAuthenticationProtocol <current_value_from_step1> -SupportsMfa <current_value_from_step1> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

以下是可能的 `PromptLoginBehavior` 參數值及其意義：

- **TranslateToFreshPasswordAuth**：表示將 `prompt=login` 轉譯為 `wauth=https://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` 和 `wfresh=0`的預設 Azure AD 行為。
- **NativeSupport**：表示 `prompt=login` 參數將會以 AD FS 的方式傳送。 如果 AD FS 是在 2012 2016 年7月更新彙總套件或更高版本的 Windows Server R2 中，這是建議的值。
- **Disabled**：表示只有 `wfresh=0` 會傳送至 AD FS。
