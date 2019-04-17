---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: "AD FS 在 Windows Server 2016 中的自訂項目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>AD FS 在 Windows Server 2016 中的自訂項目

>適用於：Windows Server 2016

使用 AD FS 組織的意見反應回應，我們已經新增額外的工具，來自訂使用者登入個人 AD FS 受保護的應用程式的體驗。  
除了指定每個應用程式網頁描述文字和的連結，現在可以指定整個 web 主題每個應用程式。  這包括商標、圖、樣式凍結，或是整個 onload.js 檔案。  
  
## <a name="global-settings"></a>通用設定    
一般全球設定您可以參考[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)，隨附在 Windows Server 2012 R2 AD FS。  
  
## <a name="pre-requisites"></a>必要條件  
下列必要條件，需要先本文件概述的程序。  
  
-   AD FS 在 Windows Server 2016 TP4 或更新版本  
  
## <a name="configure-ad-fs-relying-parties"></a>設定 AD FS 可以派對  
Web 登入信賴廠商每項目和主題：您可以設定使用 PowerShell 範例如下：  
  
### <a name="customize-messages"></a>來自訂郵件  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>自訂公司名稱、商標，以及影像  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>自訂整個頁面  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>自訂主題：和進階自訂的主題  
  
請參考自訂主題中的[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)和[進階自訂 AD FS 登入頁面。](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>指派自訂 web 主題每資源點數  
  
若要指定自訂主題每次資源點數使用下列程序：  
  
1. 做為預設值，AD FS 中的全域主題複製建立新的主題  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  匯出自訂的主題<code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.在您最愛的編輯器自訂主題檔案的影像、客服支援（onload.js）-或更換檔案 4。 匯入 AD FS（目標新主題）檔案自訂的檔案系統<code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.適用於特定資源點數（或資源點數的）的新的自訂主題
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>家用領域探索  
家用領域 dicovery 自訂看到[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="updated-password-page"></a>已更新的密碼頁面  
自訂更新密碼頁面上資訊的查看[[自訂頁面 AD FS 登入](https://technet.microsoft.com/library/dn280950.aspx)。  
  
## <a name="customizing-and-alternate-ids"></a>自訂及其他 Id  
使用者可以登入 Active Directory 同盟 Services (AD FS)-的應用程式使用任何形式的使用者識別碼接受 Active Directory Domain Services (AD DS)。 其中包括使用者主體名稱 (Upn) (johndoe@contoso.com) 或網域完整坡-account 名稱（contoso\johndoe 或 contoso.com\johndoe）。  如需有關這個查看[id 設定的其他登入。](Configuring-Alternate-Login-ID.md)  
  
您可能會此外想要自訂 AD FS 登入頁面，讓使用者一些有關其他登入收到提示 您可以新增更多的資訊查看自訂的登入頁面描述來執行[自訂 AD FS 登入頁面。](https://technet.microsoft.com/library/dn280950.aspx)   
  
您也可以執行此動作，自訂的使用者名稱] 欄位上述的「登入組織 account「字串。  查看此資訊的[進階自訂項目 AD FS 登入頁面的](https://technet.microsoft.com/library/dn636121.aspx)。  

## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
