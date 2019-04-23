---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: AD FS 登入頁面的進階自訂
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ef30df61c28eb8302c94cf756ba8c8a7b5849520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850769"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登入頁面的進階自訂

>適用於：Windows Server 2016, Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>進階自訂的 AD FS 登\-頁面中  
Windows Server 2012 R2 中的 AD FS 提供內建\-中支援可用來自訂登\-體驗中。 對於大多數的這些案例中，內建\-在 Windows PowerShell cmdlet 都是所需。  建議您使用內建\-在 Windows PowerShell 命令，以自訂標準的項目適用於 AD FS 登入\-盡可能的經驗。  請參閱[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)如需詳細資訊。  
  
在某些情況下，AD FS 系統管理員可能想要提供額外登\-體驗不可以透過現有的 PowerShell 命令中的出貨\-搭配 AD FS 的方塊。 在某些情況下是可行\(內的下面提供的指導方針\)讓系統管理員自訂登\-體驗中，藉由新增其他邏輯來進一步**onload.js** ，AD FS 所提供，而且會執行所有 AD FS 網頁。  
  
## <a name="things-to-know-before-you-start"></a>在開始之前應該知道的事項  
  
-   不支援會影響重新導向流程，或修改 AD FS 搭配的通訊協定參數的任何變更。
  
-   原始 onload.js，其中隨附預設網頁佈景主題，包含處理不同的外型規格的網頁呈現的程式碼。 建議您使用未修改的原始 onload.js 內容，但只將您的程式碼附加至現有的 onload.js 處理自訂邏輯。  
  
-   AD FS 隨附內建\-中稱為預設網頁佈景主題。 您無法修改預設網頁佈景主題的 onload.js。 若要更新 onload.js，您必須建立並自訂網頁佈景主題用於 AD FS 登\-頁面中。  請參閱[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)如需如何建立自訂網頁佈景主題的資訊。  
  
-   相同 onload.js 會執行所有 ADFS 頁面\(ex。 表單\-為基礎的登入頁面、 主領域探索頁面以及等\)。 您必須確定您的指令碼中的程式碼僅取得執行它的設計，而且不會執行非預期地。  
  
-   當參考任何 HTML 項目，請確定您一律檢查的項目上採取行動之前的項目是否存在。 這提供穩定性，並確保不會不包含這個項目頁面上執行的自訂邏輯。 您只可以檢視的 HTML 原始檔，在 AD FS 登\-頁面以檢視現有的項目中。  
  
-   強烈建議以驗證您的自訂中的替代環境，並加以測試再放大到實際執行 AD FS 伺服器。 這會減少公開給這些自訂項目之前驗證使用者的機會。  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>自訂 AD FS 登\-使用 onload.js 的經驗  
自訂 AD FS 服務 onload.js 時，請使用下列步驟。  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>AD FS 服務的自訂 onload.js  
  
1.  若要將您的自訂邏輯新增至 onload.js 中，您需要先建立自訂網頁佈景主題。 已出貨的佈景主題\-的\-\-] 方塊中，稱為 [預設值。 您可以匯出預設佈景主題，並利用它加快開始的過程。 下列 cmdlet 會建立自訂網頁佈景主題，複製預設網頁佈景主題：  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  然後，您可以匯出自訂或預設網頁佈景主題，以取得 onload.js 檔案。 若要匯出網頁佈景主題，請使用下列 cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    您將 onload.js 指令碼 資料夾下的目錄中找到您在上述匯出 cmdlet 中指定，並將您的自訂邏輯加入至指令碼\(請參閱下面的範例 > 一節中的使用案例\)。  
  
3.  進行必要修改自訂 onload.js 根據您的需求。  
  
4.  更新已修改的 onload.js 佈景主題。 若要以自訂網頁佈景主題套用更新 onload.js 使用下列 cmdlet:  

     適用於 Windows Server 2012 R2 上的 AD FS:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    適用於 Windows Server 2016 上的 AD FS:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  若要將自訂網頁佈景主題套用至 AD FS 中，使用下列 cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>其他自訂範例  
以下是範例的自訂程式碼加入至不同的細緻的 onload.js\-微調之用。 當加入自訂的程式碼，請附加自訂程式碼的 onload.js 底部。  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>範例 1： 變更 「 使用組織帳戶登入 」 的字串  
預設的 AD FS 表單\-基礎登\-頁面中，請使用者輸入方塊上方有 「 登入您的組織帳戶 」 的標題。  
  
如果您想要使用您自己的字串來取代這個字串，可以以 onload.js 加入下列程式碼。  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>範例 2： 接受 SAM\-作為登入格式，在 AD FS 表單上的帳戶名稱\-基礎登\-頁面中  
預設 AD FS 表單\-基礎登\-頁面中支援的使用者主體名稱的登入格式\(Upn\) \(比方說， **johndoe@contoso.com** \)或網域限定 sam\-帳戶名稱\( **contoso\\johndoe**或是**contoso.com\\johndoe**\)。 所有的使用者是來自相同的網域，而且他們只知道 sam\-帳戶名稱，您可能要支援使用者可供登入的案例中運用 sam\-帳戶只有名稱。 您可以將下列程式碼新增至 onload.js 以支援此案例中，只要您想要使用的網域取代網域"contoso.com"中的範例如下。  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>其他參考資料 
[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)  
  

