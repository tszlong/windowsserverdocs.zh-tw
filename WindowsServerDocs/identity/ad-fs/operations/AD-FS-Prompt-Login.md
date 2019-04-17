---
title: "AD FS 提示 = 登入"
description: "Ad FS 2016 常見問題集"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory 同盟服務提示 = 登入參數支援
下列文件告訴您的原生支援提示 = 登入參數，可在 AD FS。

## <a name="what-is-promptlogin"></a>提示 = 登入？  

某些（使用現代化驗證支援）的 Office 365 應用程式傳送提示 = 登入參數 Azure ad 為每個驗證要求的一部分。  根據預設，Azure AD 轉譯這兩個參數：<code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>，並<code><b>wfresh</b>=0</code>。

這可能會造成內部網路與多因素驗證案例需要驗證類型以外的使用者名稱和密碼的問題。  

AD FS 在 2016 年 7 月更新彙總套件與 Windows Server 2012 R2 推出提示 = 登入參數原生的支援。  這表示您現在可以設定 Azure AD 的選擇傳送此參數-Azure AD 的一部分就是您 AD FS 服務和 Office 365 驗證要求。

### <a name="ad-fs-versions-that-support-promptlogin"></a>AD FS 版本支援提示 = 登入
以下是 AD FS 版本支援提示 = 參數登入的清單。

- AD FS 年 7 月 2016 年與 Windows Server 2012 R2 的更新彙總套件

- 在 Windows Server 2016 AD FS

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>如何設定傳送提示您 Azure AD 承租人 = AD FS 登入

使用 Azure AD PowerShell 模組設定。

> [!NOTE]
> （PromptLoginBehavior 屬性支援）的提示 = 登入功能是目前只適用於[' 版本 1.0' Azure AD Powershell 模組](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，cmdlet 中已包含」Msol」，例如 Set-MsolDomainFederationSettings 名稱。  不透過目前可用 ' 版本 2.0' Azure AD PowerShell 模組，其 cmdlet 有名稱等」設定-AzureAD\ *」。

若要設定 [命令提示字元中 = 登入的行為，下列 cmdlet 語法：

範例 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

範例 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

範例 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 檢視 cmdlet 的輸出找到 PreferredAuthenticationProtocol、SupportsMfa，以及 PromptLoginBehavior 屬性的值：![取得-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> 根據預設，執行 Get-MsolDomainFederationSettings 時，某些屬性，不會顯示在主機中。  若要檢視建議您使用這些參數 |fl * 強制的所有物件的屬性輸出。


以下是 PromptLoginBehavior 參數，其設定的相關詳細資訊。
   
   - <b>TranslateToFreshPasswordAuth</b>表示預設 Azure AD 行為傳送的<b>wauth</b>和<b>wfresh</b>以而非提示 AD FS = 登入
   - <b>NativeSupport</b> = 提示登入參數會傳送到 AD FS 表示
   - <b>停用</b>表示不會傳送給 AD FS

