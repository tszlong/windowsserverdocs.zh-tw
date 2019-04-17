---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: "進階 AD FS 登入頁面的自訂項目"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>進階 AD FS 登入頁面的自訂項目

>適用於：Windows Server 2016、Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS Sign\ 在網頁中的進階的自訂項目  
在 Windows Server 2012 R2 AD FS 提供 built\ 中支援自訂 sign\ 中的體驗。 針對大部分在這些案例中，built\ 中的 Windows PowerShell cmdlet 是所需的所有。  建議使用 built\ 中的 Windows PowerShell 命令自訂 AD FS sign\ 中體驗盡可能標準項目。  查看[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)如需詳細資訊。  
  
有時候，AD FS 管理員可能會想要提供其他 sign\ 中體驗未可以透過出貨 in\ 隨 AD FS 使用的現有 PowerShell 命令。 在某些案例中，會 \ （在提供指導方針 below\) sign\ 中體驗進一步自訂透過新增額外的邏輯以系統管理員的**onload.js** ，AD FS 提供，並將會執行的所有 AD FS 頁面上。  
  
## <a name="things-to-know-before-you-start"></a>在您開始之前必須知道的事項  
  
-   不支援影響的重新導向流程或修改通訊協定參數 AD FS 使用的任何變更。
  
-   原始 onload.js，其中隨附的預設網頁主題，包含處理的不同尺寸規格頁面轉譯的程式碼。 最好不是修改原始 onload.js content，但僅限處理自訂邏輯現有 onload.js 附加程式碼。  
  
-   AD FS 隨附 built\ 中 web 主題稱為預設值。 您無法修改 onload.js 的預設網頁主題。 若要更新 onload.js，您必須建立和自訂 web 主題使用 AD FS sign\ 在網頁。  查看[AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)如需如何建立自訂 web 主題的資訊。  
  
-   在所有 ADFS 頁面 \(ex.來執行相同 onload.js form\ 登入頁面，家用領域探索頁面等。 \)。 您必須先確認指令碼的程式碼僅限取得執行的設計目的是並不會意外地執行。  
  
-   當參考 HTML 的任何項目，請確定您總是檢查有差的項目上做之前的項目。 這提供穩定性，並確保自訂邏輯不包含此項目網頁上並不會執行。 您只是可以檢視 HTML 來源 AD FS sign\ 在頁面上，檢視現有的項目。  
  
-   建議來驗證您的備用環境中的自訂項目及復原出到 production AD FS 伺服器前進行測試。 這樣可以降低公開驗證之前這些自訂給終端使用者的機會。  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>自訂體驗 sign\ 中 AD FS 使用 onload.js  
自訂 onload.js AD FS 服務時，請使用下列步驟。  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>自訂 onload.js AD FS 服務  
  
1.  若要加入 onload.js 您自訂的邏輯，您需要先建立自訂 web 主題。 預設稱為「已推出的 out\ of\-the\-方塊的主題。 您可以匯出預設的主題，並使用它，以便您可以快速開始。 下列 cmdlet 建立自訂 web 主題，請重複的預設網頁主題：  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  然後，您可以匯出自訂或 web 主題取得 onload.js 檔案的預設值。 若要匯出 web 主題，請使用下列 cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    您會找到 onload.js 指令碼資料夾中的 [directory 您指定匯出 cmdlet 上述，並加入您的自訂邏輯指令碼 \ (查看使用案例中範例區段 below\)。  
  
3.  進行需要修改自訂 onload.js 根據您的需求。  
  
4.  已修改 onload.js 更新的主題。 使用下列 cmdlet 更新 onload.js 自訂 web 主題適用於：  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  AD FS 来套用的自訂 web 主題，請使用下列 cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>其他自訂範例  
以下是新增到不同的 fine\ 調整供 onload.js 自訂的程式碼範例。 新增時自訂的程式碼，請附加自訂您的驗證碼至 onload.js 底部。  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>變更 「 使用 account 組織的登入] 字串，例如 1:  
預設 AD FS form\ 型 sign\ 在頁面上有 「 登入您的組織帳號 「 標題上述方塊中輸入的使用者。  
  
如果您想要這個字串取代您自己的字串，您可以加入 onload.js 下列程式碼。  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>範例 2： 接受 SAM\ 帳號 AD FS form\ 型 sign\ 在頁面上的登入格式  
預設的使用者主體名稱 \(UPNs\) \(for example, ** johndoe@contoso.com **\) 或網域 AD FS form\ 型 sign\ 中頁面支援登入格式完整 sam\-account 名稱 \ (**contoso\\johndoe**或**contoso.com\\johndoe**\)。 以方便您使用者的所有來自相同的網域，只有關於 sam\-account 名稱知道您可能想要支援可以在登入的使用者案例中使用只 sam\-account 名稱。 您可以新增下列程式碼來 onload.js 支援此案例，只是想要使用的網域取代網域 」 contoso.com 」 在範例如下。  
  
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
  

