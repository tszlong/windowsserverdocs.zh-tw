---
title: 在 AD FS 2019 中的其他驗證方法
description: 本文件說明新的驗證方法，在 AD FS 2019。
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b0d5754a4622df9ca26a80bd4e32c355dda0f684
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190059"
---
# <a name="configure-3rd-party-authentication-providers-as-primary-authentication-in-ad-fs-2019"></a>將第 3 方驗證提供者設定為 ad FS 2019 的主要驗證


組織會遇到試圖暴力密碼破解攻擊、 入侵，或否則鎖定使用者帳戶，藉由傳送密碼型驗證要求。  為了協助保護組織免於遭到入侵，AD FS 引進了功能，例如外部網路的 「 智慧 」 鎖定和封鎖的 IP 位址。  

不過，這些防護功能為反應式行動。  若要提供主動的方式，來降低這些攻擊的嚴重性 AD FS 會具有非密碼因素之前收集密碼提示的能力。  

比方說，AD FS 2016 引進了 Azure MFA 作為主要驗證，以便從 Authenticator 應用程式的 OTP 程式碼可以使用做為第一個因素。
使用 AD FS 2019 建置，您可以設定外部驗證提供者為主要驗證因素。

有兩個重要的案例，這可讓：

## <a name="scenario-1-protect-the-password"></a>案例 1： 保護密碼
保護免於暴力攻擊和鎖定的密碼型登入，會先提示額外的外部因素。  外部驗證已成功完成時，才會使用者再看到密碼的提示。  這排除了一個便利的方式，攻擊者已嘗試入侵，或停用的帳戶。

此案例中是由兩個元件所組成：
- 提示為主要驗證 Azure MFA 或外部驗證因素
- 使用者名稱和密碼作為 AD FS 中的其他驗證

## <a name="scenario-2-password-free"></a>案例 2： 免用密碼 ！
完全消除密碼，但完成強式，使用完全非密碼的多重要素驗證型的 AD FS 中的方法
- Azure Authenticator 應用程式的 MFA
- Windows 10 windows hello 企業版
- 憑證驗證
- 外部驗證提供者

## <a name="concepts"></a>概念
什麼**主要驗證**方法其實是它是第一個，其他因素之前的方法會提示使用者。  先前只有主要 AD fs 的方法內建的方法為 Active Directory 或 Azure MFA 或其他 LDAP 驗證存放區。  無法設定外部方法，為主要驗證成功完成後，會發生的 「 其他 」 驗證。

在 AD FS 2019 中，主要功能的外部驗證表示 （使用 Register-adfsauthenticationprovider） 的 AD FS 伺服器陣列上註冊的任何外部驗證提供者會成為適用於主要驗證，以及 「 其他 」驗證。 他們可以啟用的內建的提供者，例如表單驗證和憑證驗證內部網路和/或外部網路使用相同的方式。

![驗證](media/Additional-Authentication-Methods-AD-FS/auth1.png)

一旦外部提供者啟用外部網路的內部網路或兩者，它會變成可供使用者使用。  如果已啟用一個以上的方法，使用者會看到 [選擇] 頁面，並能夠選擇主要的方法，就如同其他驗證。

## <a name="pre-requisites"></a>必要條件
在之前設定為主要的外部驗證提供者，請確定您已備妥下列必要條件
- AD FS 伺服器陣列行為層級 (FBL) 已提升至 '4' （此值會轉譯為 AD FS 2019）
    - 這是新的 AD FS 2019 伺服器陣列的預設 FBL 值
    - 根據 Windows Server 2012 R2 或 2016年的 AD FS 伺服器陣列，以 FBL，都可以使用 PowerShell commandlet Invoke AdfsFarmBehaviorLevelRaise 會引發。  如需升級 AD FS 伺服器陣列的詳細資訊，請參閱伺服器陣列升級為 SQL 伺服器陣列或 WID 伺服器陣列的文件 
    - 您可以檢查使用 Get AdfsFarmInformation cmdlet FBL 值
- AD FS 2019 伺服陣列設定為使用新 2019年 '分頁' 使用者面向網頁
    - 這是新的 AD FS 2019 伺服器陣列的預設行為
    - 從 Windows Server 2012 R2 或 2016年升級 AD FS 伺服器陣列，，設為主要的外部驗證 （本文件中描述的功能），如下所述啟用時，會自動啟用分頁的流程。

## <a name="enable-external-authentication-methods-as-primary"></a>啟用外部的驗證方法設為主要
一旦您確認必要條件後，有兩種方式可設定為主要的 AD FS 的其他驗證提供者：

### <a name="using-powershell"></a>使用 PowerShell


```powershell
PS C:\> Set-AdfsGlobalAuthenticationPolicy -AllowAdditionalAuthenticationAsPrimary $true
``` 


AD FS 服務必須重新啟動之後啟用或停用作為主要的其他驗證。

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理主控台
在 AD FS 管理主控台中，在**服務** -> **驗證方法**下**主要驗證方法**，按一下 [編輯]

按一下核取方塊**允許額外的驗證提供者為主要**。

AD FS 服務必須重新啟動之後啟用或停用作為主要的其他驗證。

## <a name="enable-username-and-password-as-additional-authentication"></a>啟用使用者名稱和密碼作為其他驗證
若要完成的 「 保護密碼 」 的案例，啟用作為使用 PowerShell 或 AD FS 管理主控台的其他驗證的使用者名稱和密碼
### <a name="using-powershell"></a>使用 PowerShell



```powershell
PS C:\> $providers = (Get-AdfsGlobalAuthenticationPolicy).AdditionalAuthenticationProvider

PS C:\>$providers = $providers + "FormsAuthentication"

PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider $providers
``` 

### <a name="using-the-ad-fs-management-console"></a>使用 AD FS 管理主控台
在 AD FS 管理主控台中，在**服務** -> **驗證方法**下**其他驗證方法**，按一下  **編輯**

按一下核取方塊**表單驗證**若要啟用 使用者名稱和密碼作為其他驗證。
