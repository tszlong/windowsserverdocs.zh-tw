---
title: AD FS 提示 = 登入
description: 適用於 AD FS 2016 的常見問題集
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824629"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Active Directory Federation Services 提示 = 登入參數支援
下列文件描述 prompt = 登入參數可在 AD FS 中的原生支援。

## <a name="what-is-promptlogin"></a>什麼是 prompt = login？  

某些 Office 365 應用程式 （已啟用新式驗證） 會將 prompt = login 參數傳送至每個驗證要求的一部分的 Azure AD。  根據預設，Azure AD 轉譯這兩個參數： <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>，以及<code> <b> wfresh </b> =0 </code>.

這可能會造成公司內部網路與使用者名稱和密碼以外的驗證類型所需的 multi-factor authentication 案例的問題。  

AD FS 2016 年 7 月更新彙總套件的 Windows Server 2012 R2 中引進了 prompt = login 參數的原生支援。  這表示您現在可以選擇設定 Azure AD，將這個參數當做-為您的 AD FS 服務，做為一部分的 Azure AD 和 Office 365 驗證要求。

### <a name="ad-fs-versions-that-support-promptlogin"></a>支援提示字元中的 AD FS 版本 = 登入
以下是一份支援 prompt = login 參數的 AD FS 版本。

- AD FS 2016 年 7 月的 Windows Server 2012 R2 更新彙總套件

- Windows Server 2016 中的 AD FS

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>如何設定 Azure AD 租用戶傳送 prompt = 登入 AD FS

使用 Azure AD PowerShell 模組來進行設定。

> [!NOTE]
> （啟用 PromptLoginBehavior 屬性） 的提示 = 登入功能是目前只適用於[' 版本 1.0' Azure AD Powershell 模組](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)，在其中是 cmdlet 擁有包含"Msol"，例如的名稱Set-msoldomainfederationsettings。  不是目前可透過 ' 版本 2.0' Azure AD PowerShell 模組，其 cmdlet 有名稱類似"集 AzureAD\*"。

若要設定提示字元 = 登入行為，下列 cmdlet 語法：

範例 1：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

範例 2：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

範例 3：
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 PreferredAuthenticationProtocol、 SupportsMfa 和 PromptLoginBehavior 屬性值可在檢視 cmdlet 的輸出：![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> 根據預設，執行 Get-msoldomainfederationsettings 時，某些屬性不會顯示在主控台中。  若要檢視建議，您會使用這些參數 |fl * 以強制輸出的所有物件的屬性。


以下是 PromptLoginBehavior 參數和其設定的詳細資訊。
   
   - <b>TranslateToFreshPasswordAuth</b>表示傳送的預設 Azure AD 行為<b>wauth</b>並<b>wfresh</b>而不是提示字元中的 AD fs = 登入
   - <b>NativeSupport</b> prompt = login 參數將會依原狀傳送至 AD FS 的表示
   - <b>已停用</b>表示不會傳送至 AD FS

