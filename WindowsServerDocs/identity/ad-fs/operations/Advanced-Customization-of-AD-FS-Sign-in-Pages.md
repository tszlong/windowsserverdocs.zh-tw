---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: AD FS 登入頁面的自訂
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a4a70632ea4c1db39c020327bbe135f4798e6970
ms.sourcegitcommit: 2cc251eb5bc3069bf09bc08e06c3478fcbe1f321
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/03/2020
ms.locfileid: "84333889"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登入頁面的自訂

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登入頁面的自訂 \-  
Windows Server 2012 R2 中的 AD FS 提供 \- 自訂登入體驗的內建支援 \- 。 在大部分的情況下， \- 都需要內建的 Windows PowerShell Cmdlet。  建議您盡可能使用內建的 \- Windows PowerShell 命令，針對 AD FS 登入體驗自訂標準元素 \- 。  如需詳細資訊，請參閱[AD-FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)。  
  
在某些情況下，AD FS 系統管理員可能會想要 \- 透過 AD FS 隨附的現有 PowerShell 命令，提供無法進行的其他登入體驗 \- 。 在某些情況下，在下面提供的指導方針中，這是可行的， \( \) 讓系統管理員可以藉 \- 由將額外邏輯新增至 AD FS 所提供的**onload .js** ，進一步自訂登入體驗，並將在所有 AD FS 頁面上執行。  
  
## <a name="things-to-know-before-you-start"></a>開始之前要知道的事項  
  
-   不支援任何會影響重新導向流程或修改 AD FS 使用之通訊協定參數的變更。
  
-   原始的 onload，這是預設 web 主題隨附的，其中包含處理不同外型規格之頁面轉譯的程式碼。 建議您不要修改原始的 onload 內容，只是將您的程式碼附加至現有的 onload 來處理自訂邏輯。  
  
-   AD FS 隨附內建的 \- web 主題，稱為「預設」。 您無法修改預設 web 主題的 onload。 若要更新 onload，您必須建立並使用 AD FS 登入頁面的自訂 web 主題 \- 。  如需如何建立自訂 web 主題的相關資訊，請參閱[AD-FS 使用者登入自訂](AD-FS-user-sign-in-customization.md)。  
  
-   相同的 onload 會在所有 ADFS 頁面上執行， \( 例如 以表單為 \- 基礎的登入頁面、主領域探索頁面等等 \) 。 您必須確保腳本中的程式碼只會在設計時執行，而且不會意外執行。  
  
-   參考任何 HTML 專案時，請務必先檢查項目是否存在，然後再對元素採取行動。 這會提供健全性，並確保不會在不包含此元素的頁面上執行自訂邏輯。 您可以直接在 [AD FS 登入] 頁面上查看 HTML 來源 \- ，以查看現有的元素。  
  
-   強烈建議您在替代環境中驗證您的自訂，並在將其推出至生產 AD FS 伺服器之前先進行測試。 這可降低使用者在驗證之前暴露于這些自訂的機會。  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>使用 onload 自訂 AD FS 登 \- 入體驗  
自訂 AD FS 服務的 onload 時，請使用下列步驟。  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>為 AD FS 服務自訂 onload  
  
1.  若要將您的自訂邏輯新增至 onload，您必須先建立自訂的 web 主題。 現成推出的主題 \- \- \- 稱為預設值。 您可以匯出預設佈景主題，並利用它加快開始的過程。 下列 Cmdlet 會建立可複製預設 web 主題的自訂 web 主題：  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  接著，您可以匯出自訂或預設網頁主題，以取得 onload 檔案。 若要匯出 web 主題，請使用下列 Cmdlet：  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    您會在上述匯出 Cmdlet 中指定的目錄中，找到 [腳本] 資料夾底下的 [onload]，並將您的自訂邏輯新增至腳本， \( 請參閱下面的範例一節中的使用案例 \) 。  
  
3.  進行必要的修改，以根據您的需求自訂 onload。  
  
4.  使用修改過的 onload 更新主題。 使用下列 Cmdlet 將更新 onload 套用至自訂 web 主題：  

     針對 Windows Server 2012 R2 上的 AD FS：  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}  
  
    ```  
    針對 Windows Server 2016 上的 AD FS：

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\theme\script\onload.js"   
  
    ```  
  
5.  若要將自訂網頁主題套用至 AD FS，請使用下列 Cmdlet：  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>其他自訂範例  
以下是針對不同微調用途新增至 onload 的自訂程式碼範例 \- 。 新增自訂程式碼時，請一律將您的自訂程式碼附加至 onload 的底部。  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>範例1：變更「使用組織帳戶登入」字串  
預設 AD FS 表單型登 \- \- 入頁面的標題為 [使用您的組織帳戶登入]，上方的使用者輸入方塊。  
  
如果您想要將此字串取代為您自己的字串，您可以將下列程式碼加入至 onload。  
  
```  
// Sample code to change "Sign in with organizational account" string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>範例2： \- 在 AD FS 形式的登入頁面上，接受 SAM 帳戶名稱作為登 \- \- 入格式  
預設的 AD FS 表單型登 \- \- 入頁面支援使用者主要名稱 upn 的登入格式 \( \) \( ，例如， <strong>johndoe@contoso.com</strong> \) 或網域限定的 sam \- 帳戶名稱 \( **contoso \\ johndoe**或**contoso.com \\ johndoe** \) 。 如果您的所有使用者都是來自相同的網域，而他們只知道 sam \- 帳戶名稱，您可能會想要支援案例，讓使用者只能使用 sam \- 帳戶名稱登入。 您可以將下列程式碼新增至 onload 以支援此案例，只需將下列範例中的網域 "contoso.com" 取代為您想要使用的網域。  
  
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
  

