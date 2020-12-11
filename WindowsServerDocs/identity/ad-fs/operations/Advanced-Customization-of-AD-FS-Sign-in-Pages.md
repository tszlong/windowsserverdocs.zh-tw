---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: AD FS 登入頁面的 Advanced 自訂
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.openlocfilehash: 296d3e310818dcaee8443810b922ce6bf0389537
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97040256"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登入頁面的 Advanced 自訂


## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>AD FS 登入頁面的 Advanced 自訂 \-
Windows Server 2012 R2 中的 AD FS 提供 \- 內建支援來自訂登 \- 入體驗。 在大部分的情況下，內建的 \- Windows PowerShell Cmdlet 都是必要的。  建議您盡可能使用內建的 \- Windows PowerShell 命令自訂標準元素，以 AD FS 登 \- 入體驗。  如需詳細資訊，請參閱 [AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 。

在某些情況下，AD FS 系統管理員可能會想要提供其他登 \- 入體驗，而這些體驗無法透過隨附于 box 的現有 PowerShell 命令 \- 與 AD FS。 在某些情況下，為了 \( \) 讓系統管理員可以進一步自訂登入體驗，讓系統管理員可以進一步自訂登 \- 入體驗，方法是將額外的邏輯新增至 AD FS 所提供的 **onload.js** ，並將在所有 AD FS 頁面上執行。

## <a name="things-to-know-before-you-start"></a>開始之前要知道的事項

-   不支援任何影響重新導向流程的變更，或修改 AD FS 運作的通訊協定參數。

-   原始 onload.js （預設 web 主題隨附的）包含的程式碼會處理不同外型規格的頁面呈現。 建議您不要修改原始 onload.js 內容，但只將您的程式碼附加至處理自訂邏輯的現有 onload.js。

-   AD FS 隨附內建的 \- web 主題（稱為預設值）。 您無法修改預設 web 主題的 onload.js。 若要更新 onload.js，您必須建立並使用 AD FS 登入頁面的自訂 web 主題 \- 。  如需如何建立自訂 web 主題的詳細資訊，請參閱 [AD FS 使用者登入自訂](AD-FS-user-sign-in-customization.md) 。

-   相同的 onload.js 將會在所有 ADFS 頁面上執行， \( 例如 以表單為 \- 基礎的登入頁面、主領域探索頁面 \) 等等。 您必須確保腳本中的程式碼只會在設計時執行，而且不會意外執行。

-   參考任何 HTML 專案時，請務必先檢查元素是否存在，然後再對專案採取行動。 這可提供穩定性，並確保不會在不包含此專案的頁面上執行自訂邏輯。 您可以直接在 AD FS 登入頁面上查看 HTML 來源 \- ，以查看現有的元素。

-   強烈建議您在替代環境中驗證您的自訂，並先測試它們，然後再將其推出至生產環境 AD FS 伺服器。 這可降低使用者在驗證之前公開至這些自訂的機會。

## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>使用 onload.js 自訂 AD FS 登 \- 入體驗
自訂 AD FS 服務的 onload.js 時，請使用下列步驟。

#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>自訂 AD FS 服務的 onload.js

1.  若要將您的自訂邏輯新增至 onload.js，您必須先建立自訂的 web 主題。 現成推出的主題 \- \- \- 稱為預設值。 您可以匯出預設佈景主題，並利用它加快開始的過程。 下列 Cmdlet 會建立自訂的 web 主題，這會複製預設的 web 主題：

    ```
    New-AdfsWebTheme –Name custom –SourceName default

    ```

2.  然後，您可以匯出自訂或預設的 web 主題，以取得 onload.js 的檔案。 若要匯出 web 主題，請使用下列 Cmdlet：

    ```
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme

    ```

    您會在上述 export Cmdlet 中指定之目錄的腳本資料夾下找到 onload.js，並將您的自訂邏輯新增至腳本， \( 請參閱下方範例一節中的使用案例 \) 。

3.  進行必要的修改，以根據您的需求自訂 onload.js。

4.  使用修改過的 onload.js 來更新主題。 使用下列 Cmdlet 將更新 onload.js 套用至自訂 web 主題：

     若為 Windows Server 2012 R2 上的 AD FS：

    ```
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}

    ```
    若為 Windows Server 2016 上的 AD FS：

     ```
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\theme\script\onload.js"

    ```

5.  若要將自訂網頁主題套用至 AD FS，請使用下列 Cmdlet：

    ```
    Set-AdfsWebConfig -ActiveThemeName custom
    ```

## <a name="additional-customization-examples"></a>其他自訂範例
以下是新增至 onload.js 的自訂程式碼範例，以進行不同的微調 \- 用途。 新增自訂程式碼時，請一律將自訂程式碼附加至 onload.js 的底部。

### <a name="example-1-change-sign-in-with-organizational-account-string"></a>範例1：變更「使用組織帳戶登入」字串
預設 AD FS 表單登 \- \- 入頁面的使用者輸入方塊上方具有「使用您的組織帳戶登入」的標題。

如果您想要使用您自己的字串來取代這個字串，可以將下列程式碼新增至 onload.js。

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

### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>範例2：以 \- 登入 AD FS 表單的登入頁面上的登入格式接受 SAM 帳戶名稱 \- \-
預設 AD FS 以表單 \- 為基礎的 \- 登入頁面支援使用者主體名稱 upn 的登入格式 \( \) \( ，例如， <strong>johndoe@contoso.com</strong> \) 或網域限定的 sam \- 帳戶名稱 \( **contoso \\ johndoe** 或 **contoso.com \\ johndoe** \) 。 如果您所有的使用者都來自相同的網域，而且他們只知道 sam \- 帳戶名稱，您可能會想要支援使用者只能使用 sam 帳戶名稱登入的案例 \- 。 您可以將下列程式碼新增至 onload.js 來支援此案例，只要將下列範例中的網域 "contoso.com" 取代為您想要使用的網域即可。

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


