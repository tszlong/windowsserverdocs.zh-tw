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
ms.openlocfilehash: 1a2e70251837c88c3220c3d7593108eba5afb1ca
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869434"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 單一登入設定

單一登入（SSO）可讓使用者驗證一次並存取多個資源，而不會提示您輸入其他認證。  本文說明 SSO 的預設 AD FS 行為，以及可讓您自訂此行為的設定。  

## <a name="supported-types-of-single-sign-on"></a>支援的單一登入類型

AD FS 支援數種類型的單一登入體驗：  
  
-   **會話 SSO**  
  
     會話 SSO cookie 是針對已驗證的使用者所撰寫，這會在使用者于特定會話期間切換應用程式時，排除進一步的提示。 不過，如果特定的會話結束，系統會提示使用者重新輸入其認證。  
  
     如果未註冊使用者的裝置，AD FS 預設會設定會話 SSO cookie。 如果瀏覽器會話已結束且重新開機，則會刪除此會話 cookie，而且不再有效。  
  
-   **持續 SSO**  
  
     持續性 SSO cookie 是針對已驗證的使用者所撰寫，在使用者切換應用程式時，只要持續的 SSO cookie 有效，就可以排除進一步的提示。 持續性 SSO 和會話 SSO 之間的差異在於持續性 SSO 可以跨不同的會話進行維護。  
  
     如果裝置已註冊，AD FS 將會設定持續性 SSO cookie。 如果使用者選取 [讓我保持登入] 選項，AD FS 也會設定持續性 SSO cookie。 如果持續性 SSO cookie 無效，則會遭到拒絕和刪除。  
  
-   **應用程式特定 SSO**  
  
     在 OAuth 案例中，重新整理權杖是用來在特定應用程式的範圍內維護使用者的 SSO 狀態。  
  
     如果裝置已註冊，AD FS 會根據已註冊裝置的持續 SSO cookie 存留期，設定重新整理權杖的到期時間（預設為7天，AD FS 2012R2，最多90天，如果使用其裝置，則為 AD FS 2016）在14天的時間範圍記憶體取 AD FS 資源。 

如果裝置未註冊，但使用者選取 [讓我保持登入] 選項，重新整理權杖的到期時間將會等於 [讓我保持登入] 的持續 SSO cookie 存留期，預設為1天，最多7天。 否則，重新整理權杖存留期等於會話 SSO cookie 存留期，預設為8小時  
  
 如前所述，已註冊裝置上的使用者一律會取得持續性 SSO，除非已停用持續性 SSO。 針對未註冊的裝置，您可以藉由啟用「讓我保持登入」（KMSI）功能來達到持續性 SSO。 
 
 針對 Windows Server 2012 R2，若要啟用「讓我保持登入」案例的 PSSO，您必須安裝此[修補程式](https://support.microsoft.com/en-us/kb/2958298/)，這也是[windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2 的2014年8月更新彙總套件](https://support.microsoft.com/en-us/kb/2975719)的一部分。   

工作 | PowerShell | 描述
------------ | ------------- | -------------
啟用/停用持續性 SSO | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| 預設會啟用持續性 SSO。 如果已停用，則不會寫入任何 PSSO cookie。
[啟用/停用] [讓我保持登入] | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | 預設會停用 [讓我保持登入] 功能。 如果已啟用，終端使用者會在 AD FS 登入頁面上看到 [讓我保持登入] 選項



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-單一登入和已驗證的裝置
當要求者從已註冊的裝置進行驗證時，AD FS 2016 變更 PSSO 90，但在14天的期間內需要驗證（[裝置使用量] 視窗）。
第一次提供認證之後，根據預設，已註冊裝置的使用者會在90天的最長期間內取得單一登入，但前提是它們會使用裝置至少每14天存取 AD FS 資源一次。  如果它們在提供認證後等待15天，使用者會再次收到認證的提示。  

預設會啟用持續性 SSO。 如果已停用，則不會寫入任何 PSSO cookie。 |  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
[裝置使用時間] 視窗（預設為14天）由 [AD FS] 屬性**DeviceUsageWindowInDays**控管。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
單一登入期間上限（預設為90天）是由 AD FS 屬性**PersistentSsoLifetimeMins**所控管。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>讓我保持登入未驗證的裝置 
針對未註冊的裝置，單一登入期間是由 [**讓我保持登入（KMSI）** ] 功能設定來決定。  KMSI 預設為停用，而且可以藉由將 AD FS 屬性 KmsiEnabled 設定為 True 來啟用。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

停用 KMSI 之後，預設的單一登入期間為8小時。  這可以使用屬性 SsoLifetime 來設定。  屬性是以分鐘為單位來測量，因此其預設值為480。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

啟用 KMSI 時，預設的單一登入期間為24小時。  這可以使用屬性 KmsiLifetimeMins 來設定。  屬性是以分鐘為單位來測量，因此其預設值為1440。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多重要素驗證（MFA）行為  
請務必注意，雖然提供較長的單一登入週期，但當先前的登入是以主要認證為基礎，而不是 MFA 時，AD FS 會提示您進行額外的驗證（多重要素驗證），但目前的登入需要 MFA。  無論 SSO 設定為何，都是如此。 AD FS，當它收到驗證要求時，會先判斷是否有 SSO 內容（例如 cookie），如果需要 MFA （例如，如果要求是來自外部，則會評估 SSO 內容是否包含 MFA）。  如果不是，則會提示 MFA。  


  
## <a name="psso-revocation"></a>PSSO 撤銷  
 為了保護安全性，AD FS 會在符合下列條件時，拒絕先前發出的任何持續性 SSO cookie。 這會要求使用者提供其認證，以便再次向 AD FS 進行驗證。 
  
- 使用者變更密碼  
  
- 已停用持續性 SSO 設定 AD FS  
  
- 系統管理員已在遺失或遭竊的情況下停用裝置  
  
- AD FS 收到為已註冊的使用者發出的持續性 SSO cookie，但使用者或裝置未再註冊  
  
- AD FS 收到已註冊使用者的持續 SSO cookie，但使用者已重新註冊  
  
- AD FS 會收到持續性 SSO cookie，其會因為「讓我保持登入」而發出，但已停用 [讓我保持登入] 設定 AD FS  
  
- AD FS 收到針對已註冊使用者發出的持續性 SSO cookie，但在驗證期間遺失或變更了裝置憑證  
  
- AD FS 系統管理員已設定持續性 SSO 的截止時間。 當此設定時，AD FS 將會拒絕在這段時間之前發出的任何持續性 SSO cookie  
  
  若要設定截止時間，請執行下列 PowerShell Cmdlet：  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>啟用 PSSO for Office 365 使用者以存取 SharePoint Online  
 一旦在 AD FS 中啟用並設定 PSSO 之後，AD FS 會在使用者經過驗證之後寫入持續性 cookie。 使用者下次進入時，如果持續性 cookie 仍然有效，使用者就不需要提供認證來重新驗證。 您也可以在 AD FS 中設定下列兩個宣告規則，以觸發 Microsoft Azure AD 和 SharePoint Online 的持續性，以避免 Office 365 和 SharePoint Online 使用者的額外驗證提示。  若要讓 PSSO for Office 365 使用者存取 SharePoint online，您必須安裝此[修補程式](https://support.microsoft.com/en-us/kb/2958298/)，這也是[windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2 的2014更新彙總套件](https://support.microsoft.com/en-us/kb/2975719)的一部分。  
  
 要通過 InsideCorporateNetwork 宣告的發行轉換規則  
  
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
    <th colspan="3">ADFS 2012 R2 <br> 裝置是否已註冊？</th>
        <th colspan="1"></th>
    <th colspan="3">ADFS 2016 <br> 裝置是否已註冊？</th>
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
    <td>SSO =&gt;設定重新整理權杖 =&gt;</td>
    <td>8小時</td>
    <td>N/A</td>
    <td>N/A</td>
    <th></th>
    <td>8小時</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>

 <tr align="center">
    <td>PSSO =&gt;設定重新整理權杖 =&gt;</td>
    <td>N/A</td>
    <td>24小時</td>
    <td>7天</td>
    <th></th>
    <td>N/A</td>
    <td>24小時</td>
    <td>最大90天（含14天）視窗</td>
  </tr>

 <tr align="center">
    <td>權杖存留期</td>
    <td>1小時</td>
    <td>1小時</td>
    <td>1小時</td>
    <th></th>
    <td>1小時</td>
    <td>1小時</td>
    <td>1小時</td>
  </tr>
</table>

**已註冊的裝置？** 您會取得 PSSO/持續性的 SSO <br>
**未註冊的裝置？** 您會取得 SSO <br>
**未註冊的裝置，但 KMSI？** 您會取得 PSSO/持續性的 SSO <p>
只有
 - [x] 系統管理員已啟用 KMSI 功能 [和]
 - [x] 使用者按一下表單登入頁面上的 [KMSI] 核取方塊
 
**好知道：** <br>
未同步處理**LastPasswordChangeTimestamp**屬性的同盟使用者會發行會話 cookie，並將**最大存留期值為12小時**的重新整理權杖。<br>
這是因為 Azure AD 無法判斷何時撤銷與舊認證相關的權杖（例如已變更的密碼）。 因此，Azure AD 必須更頻繁地檢查，以確定使用者和相關聯的權杖仍在良好的地位。
