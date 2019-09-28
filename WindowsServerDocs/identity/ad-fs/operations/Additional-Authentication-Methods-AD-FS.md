---
title: AD FS 2019 中的其他驗證方法
description: 本檔說明 AD FS 2019 中的新驗證方法。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8a53a4cfca4f34459102b8edc8e6af82f36be70d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358375"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>將協力廠商驗證提供者設定為 AD FS 2019 中的主要驗證


組織遭遇攻擊，試圖透過傳送密碼型驗證要求，以暴力破解、入侵或鎖定使用者帳戶。  為了協助保護組織免于遭到入侵，AD FS 引進了外部網路「智慧型」鎖定和以 IP 位址為基礎的封鎖等功能。  

不過，這些緩和措施是被動的。  為了提供主動的方法，若要降低這些攻擊的嚴重性，AD FS 可以在收集密碼之前提示您輸入非密碼因素。  

例如，AD FS 2016 引進了 Azure MFA 做為主要驗證，因此可以使用來自驗證器應用程式的 OTP 代碼作為第一個因素。
以 AD FS 2019 為基礎，您可以將外部驗證提供者設定為主要的驗證因素。

這有兩個主要的案例可達成：

## <a name="scenario-1-protect-the-password"></a>案例1：保護密碼
先提示額外的外部因素，以保護以密碼為基礎的登入，防止暴力密碼破解攻擊和鎖定。  只有在成功完成外部驗證後，使用者才會看到密碼提示。  這可避免攻擊者嘗試入侵或停用帳戶的便利方式。

此案例是由兩個元件所組成：
- 提示 Azure MFA 或外部驗證因素做為主要驗證
- 使用者名稱和密碼做為 AD FS 中的額外驗證

## <a name="scenario-2-password-free"></a>案例2：密碼-免費！
完全排除密碼，但在 AD FS 中使用完全不以密碼為基礎的方法來完成強大的多重要素驗證
- 使用驗證器應用程式的 Azure MFA
- Windows 10 Hello 企業版
- 憑證驗證
- 外部驗證提供者

## <a name="concepts"></a>概念
**主要驗證**的意義是，它是使用者第一次出現的方法，在其他因素之前。  先前 AD FS 中唯一可用的主要方法，就是 Active Directory 或 Azure MFA 或其他 LDAP 驗證存放區的內建方法。  外部方法可以設定為「額外」驗證，這會在主要驗證成功完成後進行。

在 AD FS 2019 中，作為主要功能的外部驗證是指在 AD FS 伺服器陣列上註冊的任何外部驗證提供者（使用 Register-adfsauthenticationprovider），都可供主要驗證和「其他」使用authentication. 它們的啟用方式與內建提供者（例如表單驗證和憑證驗證）相同，適用于內部網路和/或外部網路使用。

![驗證](media/Additional-Authentication-Methods-AD-FS/auth1.png)

一旦為外部網路、內部網路或兩者啟用外部提供者，就可供使用者使用。  如果已啟用一個以上的方法，使用者會看到一個選擇頁面，而且可以選擇主要方法，就像是進行其他驗證一樣。

## <a name="pre-requisites"></a>先決條件
將外部驗證提供者設定為主要之前，請確定您已備妥下列先決條件
- AD FS 伺服器陣列行為層級（FBL）已提升為 ' 4 ' （此值會轉譯為 AD FS 2019）
    - 這是新 AD FS 2019 伺服器陣列的預設 FBL 值
    - 針對以 Windows Server 2012 R2 或2016為基礎的 AD FS 伺服器陣列，可以使用 PowerShell commandlet AdfsFarmBehaviorLevelRaise 來引發 FBL。  如需有關升級 AD FS 伺服器陣列的詳細資訊，請參閱 SQL 伺服器陣列或 WID 伺服器陣列的伺服器陣列升級一文。 
    - 您可以使用 Cmdlet AdfsFarmInformation 來檢查 FBL 值
- AD FS 2019 伺服器陣列已設定為使用新的2019「編頁」使用者面向頁面
    - 這是新 AD FS 2019 伺服器陣列的預設行為
    - 針對從 Windows Server 2012 R2 或2016升級的 AD FS 伺服器陣列，當外部驗證為主要（本檔中所述的功能）時，會自動啟用分頁流程，如下所述。

## <a name="enable-external-authentication-methods-as-primary"></a>啟用外部驗證方法作為主要
驗證必要條件之後，有兩種方式可以將 AD FS 其他驗證提供者設定為主要：

### <a name="using-powershell"></a>使用 PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


在啟用或停用其他驗證做為主要複本之後，必須重新開機 AD FS 服務。

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理主控台
在 [AD FS 管理主控台] 的 [**服務** -> **驗證方法**] 底下，按一下 [**主要驗證方法**] 底下的 [編輯]

按一下 [**允許其他驗證提供者作為主要**] 核取方塊。

在啟用或停用其他驗證做為主要複本之後，必須重新開機 AD FS 服務。

## <a name="enable-username-and-password-as-additional-authentication"></a>啟用使用者名稱和密碼做為額外的驗證
若要完成「保護密碼」案例，請使用 PowerShell 或 AD FS 管理主控台，啟用使用者名稱和密碼做為額外的驗證
### <a name="using-powershell"></a>使用 PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理主控台
在 [AD FS 管理主控台] 的 [**服務** -> **驗證方法**] 底下，按一下 [**其他驗證方法**] 底下的 [**編輯**]

按一下 [**表單驗**證] 核取方塊，以啟用使用者名稱和密碼做為額外的驗證。
