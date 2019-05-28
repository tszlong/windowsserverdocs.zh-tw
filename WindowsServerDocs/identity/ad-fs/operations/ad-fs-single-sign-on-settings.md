---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: AD FS 2016 單一登入設定
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f3719277c80eae2bf2a4d923146920d17546601d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188726"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 的單一登入設定

單一登入 (SSO) 可讓使用者進行一次驗證及存取多個資源，而不需要額外的認證的提示。  這篇文章描述 SSO，以及可讓您自訂此行為的組態設定的預設 AD FS 的行為。  

## <a name="supported-types-of-single-sign-on"></a>支援的單一登入的類型

AD FS 支援數種類型的單一登入體驗：  
  
-   **工作階段 SSO**  
  
     工作階段的 SSO cookie 會寫入已驗證之使用者的使用者特定的工作階段期間切換應用程式時，就不接下來的提示。 不過，如果特定的工作階段結束時，將會再次提示使用者輸入其認證。  
  
     AD FS 會設定工作階段的 SSO cookie 預設當使用者的裝置未註冊。 如果瀏覽器工作階段已結束，且重新啟動，此工作階段 cookie 會刪除而且不是有效了。  
  
-   **永續性 SSO**  
  
     這樣會消除進一步的提示時使用者切換適用於應用程式，只要持續性的 SSO cookie 為有效的已驗證使用者寫入永續性 SSO cookie。 永續性 SSO 與工作階段 SSO 之間的差異是，可以在不同的工作階段之間維護永續性 SSO。  
  
     如果註冊的裝置，AD FS 將設定持續性的 SSO cookie。 如果使用者選取 [保留我登入] 選項，AD FS 也會設定永續性的 SSO cookie。 如果持續性的 SSO cookie 無效了，它將會遭到拒絕，而且刪除。  
  
-   **應用程式特定的 SSO**  
  
     在 OAuth 案例中，重新整理權杖用來維護 SSO 狀態之範圍內的特定應用程式的使用者。  
  
     如果註冊裝置時，AD FS 會設定已註冊的裝置，也就是 7 天預設的 AD FS 2012 r2 和最大值與 AD FS 2016 的 90 天，如果使用者使用其裝置的永續性 SSO cookie 存留時間為基礎的重新整理權杖的到期時間存取一 14 天的時間內的 AD FS 資源。 

如果裝置未註冊但使用者選取 [保留我登入] 選項，重新整理權杖的到期時間將會等於 「 讓我保持登中 」 的永續性 SSO cookie 存留期這會是 1 天，預設最大值為 7 天。 否則，請重新整理權杖存留期等於工作階段 SSO cookie 存留期即預設為 8 小時  
  
 如先前所述，使用者已註冊的裝置上將一律可以取得永續性 SSO 除非永續性 SSO 已停用。 取消註冊裝置，永續性 SSO 可藉由啟用 「 讓我保持登中 」 (KMSI) 」 功能。 
 
 Windows Server 2012 r2，若要啟用 PSSO 「 保持登入我 」 案例中，您必須安裝這[hotfix](https://support.microsoft.com/en-us/kb/2958298/)也是屬於的[2014 年 8 月 Windows RT 8.1、 Windows 8.1 和 Windows Server 2012 更新彙總套件R2](https://support.microsoft.com/en-us/kb/2975719)。   

工作 | PowerShell | 描述
------------ | ------------- | -------------
啟用/停用持續性的 SSO | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| 預設會啟用永續性 SSO。 如果已停用，則會不寫入任何 PSSO cookie。
啟用/停用 「 讓我保持登入 」 | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | 預設會停用 [保留我登入] 功能。 如果已啟用，使用者會看到 讓我登入保持 「 選擇 AD FS 登入頁面



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-單一登入和驗證的裝置
要求者會從已註冊的裝置增加最大的 90 天內，但需要 authenticvation 14 天內 （裝置使用方式 視窗） 進行驗證時，AD FS 2016 變更 PSSO。
提供認證之後第一次，依預設具有已註冊裝置的使用者取得單一登入最大的一段 90 天，提供他們用來存取 AD FS 資源至少一次每隔 14 天的裝置。  如果他們提供認證之後的 15 天，將會提示使用者輸入認證一次。  

預設會啟用永續性 SSO。 如果已停用，將會寫入任何 PSSO cookie。 |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
裝置使用方式 視窗 （預設值 14 天） 由 AD FS 屬性**DeviceUsageWindowInDays**。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
最大單一登入期間 （預設值 90 天） 由 AD FS 屬性**PersistentSsoLifetimeMins**。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>保留未驗證裝置的 我登入 
對於未註冊的裝置，單一登入期間有取決**讓我帶正負號中 (KMSI)** 功能設定。  KMSI 會預設為停用，而且可由 AD FS 屬性 KmsiEnabled 設為 True。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

KMSI 停用，使用的預設單一登入期間會為 8 小時。  這可以使用 SsoLifetime; 屬性設定。  屬性是以分鐘為單位測量，因此它的預設值是 480。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

啟用 KMSI，與預設單一登入期間為 24 小時。  這可以使用 KmsiLifetimeMins; 屬性設定。  屬性是以分鐘為單位測量，因此它的預設值為 1440年。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多重要素驗證 (MFA) 行為  
請務必請注意，同時提供相對較長的單一登入，AD FS 會提示您輸入其他的驗證 （多重要素驗證） 當先前的登入根據主要認證並不是使用 MFA，但目前登入需要 MFA。  這是不論 SSO 組態。 AD FS 中，當它收到的驗證要求時，首先會決定是否沒有 SSO 內容 （例如 cookie)，然後，如果需要 MFA (例如，如果要求來自外部) 它將會評估是否 SSO 內容包含 MFA。  如果沒有，則 MFA 系統會提示。  


  
## <a name="psso-revocation"></a>PSSO 撤銷  
 若要保護的安全性，AD FS 將會拒絕任何符合下列條件時，先前發行的永續性 SSO cookie。 這將需要提供其認證，才能再次使用 AD FS 驗證使用者。 
  
-   使用者變更密碼  
  
-   在 AD FS 中停用持續性的 SSO 設定  
  
-   系統管理員在遺失或遭竊的情況下停用裝置  
  
-   AD FS 收到永續性的 SSO cookie 發行給已註冊的使用者，但使用者或裝置未再註冊  
  
-   AD FS 收到永續性 SSO cookie 的已註冊的使用者，但使用者重新加以註冊  
  
-   AD FS 收到永續性的 「 讓我保持登 」，但 「 讓我保持登 」 發出的 SSO cookie 設定為停用 AD FS 中  
  
-   AD FS 收到永續性的 SSO cookie 發行給已註冊的使用者，但裝置憑證在驗證期間會遺失或已變更  
  
-   AD FS 系統管理員已經設定為永續性 SSO 的截止時間。 當此設定時，AD FS 會拒絕任何此時間之前發出的永續性 SSO cookie  
  
 若要設定截止時間，請執行下列 PowerShell cmdlet:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>啟用 Office 365 使用者，以存取 SharePoint Online 的 PSSO  
 一旦 PSSO 已啟用，並設定 AD FS 中，AD FS 會在使用者驗證之後寫入永續性 cookie。 下次使用者傳入時，如果永續性 cookie 仍然有效，使用者不必提供認證，才能再次進行驗證。 您也可以避免額外的驗證提示適用於 Office 365 和 SharePoint Online 的使用者，藉由設定下列兩個宣告在 Microsoft Azure AD 與 SharePoint Online 觸發程序持續性的 AD FS 中的規則。  若要啟用 Office 365 使用者，以存取 SharePoint online 的 PSSO，必須先安裝這[hotfix](https://support.microsoft.com/en-us/kb/2958298/)也是屬於的[2014 年 8 月更新彙總套件，Windows RT 8.1、 Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719).  
  
 若要通過 InsideCorporateNetwork 宣告發行轉換規則  
  
```  
@RuleTemplate = "PassThroughClaims"  
@RuleName = "Pass through claim - InsideCorporateNetwork"  
c:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]  
=> issue(claim = c);   
A custom Issuance Transform rule to pass through the persistent SSO claim  
@RuleName = "Pass Through Claim - Psso"  
c:[Type == "http://schemas.microsoft.com/2014/03/psso"]  
=> issue(claim = c);  
  
```
  
總結：
<table>
  <tr>
    <th colspan="1">單一登入體驗</th>
    <th colspan="3">ADFS 2012 R2 <br> 已註冊的裝置</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> 已註冊的裝置</th>
  </tr>

  <tr align="center">
    <th></th>
    <th>否</th>
    <th>否，但 KMSI</th>
    <th>是</th>
    <th></th>
    <th>否</th>
    <th>否，但 KMSI</th>
    <th>是</th>
  </tr>
 <tr align="center">
    <td>SSO=>set Refresh Token=></td>
    <td>8 小時</td>
    <td>N/A</td>
    <td>N/A</td>
    <th></th>
    <td>8 小時</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>

 <tr align="center">
    <td>PSSO=>set Refresh Token=></td>
    <td>N/A</td>
    <td>24 小時</td>
    <td>7 天</td>
    <th></th>
    <td>N/A</td>
    <td>24 小時</td>
    <td>最大值與 14 天 視窗的 90 天</td>
  </tr>

 <tr align="center">
    <td>權杖存留期</td>
    <td>1 小時</td>
    <td>1 小時</td>
    <td>1 小時</td>
    <th></th>
    <td>1 小時</td>
    <td>1 小時</td>
    <td>1 小時</td>
  </tr>
</table>

**已註冊的裝置嗎？** 取得 PSSO / 永續性 SSO <br>
**未註冊的裝置嗎？** 取得 SSO <br>
**未註冊的裝置，但 KMSI 嗎？** 取得 PSSO / 永續性 SSO <p>
如果：
 - [x] 管理員已啟用 KMSI 功能 [AND]
 - [x] 使用者按一下表單登入頁面上 KMSI 核取方塊
 
**最好知道：** <br>
同盟使用者不需要**LastPasswordChangeTimestamp**同步處理的屬性會發出工作階段 cookie 並重新整理權杖具有**12 小時的最大壽命 」 值**。<br>
這是因為 Azure AD 無法判斷何時要撤銷舊的認證 （例如密碼已變更） 相關的權杖。 因此，Azure AD 必須更頻繁地檢查，請確定使用者和相關聯的權杖是仍在有效。
