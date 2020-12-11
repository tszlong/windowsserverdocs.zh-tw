---
description: 深入瞭解： AD FS 單一 Sign-On 設定
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: AD FS 2016 單一登入設定
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.openlocfilehash: 0a6596805107e728417da99496de565a549ae7cc
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041916"
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 單一 Sign-On 設定

單一 Sign-On (SSO) 讓使用者只需驗證一次，就可以存取多個資源，而不會提示您提供其他認證。  本文描述 SSO 的預設 AD FS 行為，以及可讓您自訂此行為的設定。

## <a name="supported-types-of-single-sign-on"></a>支援的單一 Sign-On 類型

AD FS 支援數種類型的單一 Sign-On 體驗：

-   **會話 SSO**

     系統會為已驗證的使用者撰寫會話 SSO cookie，以在使用者于特定會話期間切換應用程式時，消除進一步的提示。 但是，如果特定的會話結束，則會再次提示使用者輸入認證。

     如果未註冊使用者的裝置，AD FS 預設會設定會話 SSO cookie。 如果瀏覽器會話已結束且已重新開機，則會刪除此會話 cookie，且不再有效。

-   **持續性 SSO**

     持續性 SSO cookie 是針對已驗證的使用者所撰寫，只要持續的 SSO cookie 有效，就會在使用者切換應用程式時排除進一步的提示。 持續性 SSO 和會話 SSO 之間的差異在於可以跨不同會話維護持續性 SSO。

     如果裝置已註冊，AD FS 將會設定持續性的 SSO cookie。 如果使用者選取 [讓我保持登入] 選項，AD FS 也會設定持續性 SSO cookie。 如果持續性 SSO cookie 無效，將會遭到拒絕並刪除。

-   **應用程式特定的 SSO**

     在 OAuth 案例中，會使用重新整理權杖來維護特定應用程式範圍內的使用者 SSO 狀態。

     如果裝置已註冊，AD FS 將會根據已註冊裝置的持續 SSO cookie 存留期來設定重新整理權杖的到期時間，預設為7天（AD FS 2012R2），而如果使用裝置在14天內使用其裝置來存取 AD FS 資源，則最多可達 90 2016 AD FS 天。

如果裝置未註冊，但使用者選取 [讓我保持登入] 選項，重新整理權杖的到期時間會等於「讓我保持登入」的持續 SSO cookie 存留期（預設為1天，最多7天）。 否則，重新整理權杖存留期等於會話 SSO cookie 存留期，預設為8小時

 如先前所述，除非停用持續性 SSO，否則已註冊裝置上的使用者一律會取得持續性 SSO。 針對未註冊的裝置，您可以啟用「讓我保持登入」 (KMSI) 功能來達成持續性 SSO。

 針對 Windows Server 2012 R2，若要啟用「讓我保持登入」案例的 PSSO，您必須安裝此修正 [程式](https://support.microsoft.com/kb/2958298/) ，這也是 [2014 年8月更新彙總套件的一部分，適用于 Windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/kb/2975719)。

Task | PowerShell | 描述
------------ | ------------- | -------------
啟用/停用持續性 SSO | ```` Set-AdfsProperties –EnablePersistentSso <Boolean> ````| 預設會啟用持續性 SSO。 如果已停用，將不會寫入任何 PSSO cookie。
「啟用/停用「讓我保持登入」 | ```` Set-AdfsProperties –EnableKmsi <Boolean> ```` | 預設會停用 [讓我保持登入] 功能。 若已啟用，終端使用者會在 AD FS 登入頁面上看到 [讓我保持登入] 選項



## <a name="ad-fs-2016---single-sign-on-and-authenticated-devices"></a>AD FS 2016-單一 Sign-On 和已驗證的裝置
AD FS 2016 會在要求者從已註冊的裝置進行驗證，而增加至90天，但在14天內需要驗證， (裝置使用時間範圍) 來變更 PSSO。
第一次提供認證之後，已註冊裝置的使用者預設會在90天的最長期間內取得單一 Sign-On，但前提是他們使用裝置至少每14天可存取 AD FS 資源一次。  如果使用者在提供認證之後等候15天，系統會再次提示使用者輸入認證。

預設會啟用持續性 SSO。 如果已停用，將不會寫入任何 PSSO cookie。 |

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```

[裝置使用量] 視窗預設 (14 天) 由 AD FS 屬性 **DeviceUsageWindowInDays** 控管。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```
依預設，單一 Sign-On 期間的最大值 (90 天) 由 AD FS 屬性 **PersistentSsoLifetimeMins** 控管。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>讓我保持登入未經過驗證的裝置
針對未註冊的裝置，單一登入期間是由 [ **讓我保持登入] (KMSI)** 功能設定所決定。  KMSI 預設為停用，而且可以藉由將 AD FS 屬性 KmsiEnabled 設定為 True 來啟用。

``` powershell
Set-AdfsProperties -EnableKmsi $true
```

停用 KMSI 時，預設的單一登入期限為8小時。  您可以使用屬性 SsoLifetime 來設定這個屬性。  屬性是以分鐘為單位來測量，因此其預設值為480。

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\>
```

在啟用 KMSI 的情況下，預設的單一登入期限為24小時。  您可以使用屬性 KmsiLifetimeMins 來設定這個屬性。  屬性是以分鐘為單位來測量，因此其預設值為1440。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\>
```

## <a name="multi-factor-authentication-mfa-behavior"></a> (MFA) 行為的多重要素驗證
請務必注意，雖然提供的單一登入時間相當長，但 AD FS 會在先前的登入是以主要認證而非 MFA 時，提示輸入額外的驗證 (多重要素驗證) ，但目前的登入需要 MFA。  這與 SSO 設定無關。 AD FS，當它收到驗證要求時，會先判斷是否有 SSO 內容 (（例如 cookie) ），然後如果需要 MFA (例如，如果要求是來自) 外部，則會評估 SSO 內容是否包含 MFA。  如果沒有，則會提示 MFA。



## <a name="psso-revocation"></a>PSSO 撤銷
 為了保護安全性，AD FS 會在符合下列條件時，拒絕先前發出的任何持續性 SSO cookie。 這會要求使用者提供其認證，才能再次向 AD FS 進行驗證。

- 使用者變更密碼

- 在 AD FS 中停用持續性 SSO 設定

- 系統管理員在遺失或遭竊的情況下停用裝置

- AD FS 收到針對已註冊使用者發出的持續性 SSO cookie，但使用者或裝置不再註冊

- AD FS 為已註冊的使用者接收持續的 SSO cookie，但使用者已重新登錄

- AD FS 會收到由於「讓我保持登入」而發出的持續性 SSO cookie，但 AD FS 中已停用 [讓我保持登入] 設定。

- AD FS 收到針對已註冊使用者發出的持續性 SSO cookie，但在驗證期間遺失或改變了裝置憑證

- AD FS 系統管理員已設定持續性 SSO 的截止時間。 設定此項之後，AD FS 將會拒絕此時間之前發出的任何持續性 SSO cookie

  若要設定截止時間，請執行下列 PowerShell Cmdlet：


``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```

## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>啟用 PSSO for Office 365 使用者以存取 SharePoint Online
 在 AD FS 中啟用並設定 PSSO 之後，AD FS 會在使用者經過驗證之後，寫入持續性 cookie。 當使用者下一次出現時，如果持續性 cookie 仍然有效，使用者就不需要提供認證來重新驗證。 您也可以在 AD FS 中設定下列兩個宣告規則，以觸發 Microsoft Azure AD 和 SharePoint Online 的持續性，以避免 Office 365 和 SharePoint Online 使用者的額外驗證提示。  若要讓 Office 365 使用者的 PSSO 存取 SharePoint online，您必須安裝此修正 [程式](https://support.microsoft.com/kb/2958298/) ，這也是 [2014 年8月更新彙總套件的一部分，適用于 Windows RT 8.1、Windows 8.1 和 Windows Server 2012 R2](https://support.microsoft.com/kb/2975719)。

 傳遞 InsideCorporateNetwork 宣告的發行轉換規則

```
@RuleTemplate = "PassThroughClaims"
@RuleName = "Pass through claim - InsideCorporateNetwork"
c:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork"]
=> issue(claim = c);
A custom Issuance Transform rule to pass through the persistent SSO claim
@RuleName = "Pass Through Claim - Psso"
c:[Type == "https://schemas.microsoft.com/2014/03/psso"]
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
    <th>YES</th>
    <th></th>
    <th>否</th>
    <th>否，但 KMSI</th>
    <th>YES</th>
  </tr>
 <tr align="center">
    <td>SSO = &gt; 設定重新整理權杖 =&gt;</td>
    <td>8小時</td>
    <td>N/A</td>
    <td>N/A</td>
    <th></th>
    <td>8小時</td>
    <td>N/A</td>
    <td>N/A</td>
  </tr>

 <tr align="center">
    <td>PSSO = &gt; Set Refresh Token =&gt;</td>
    <td>N/A</td>
    <td>24小時</td>
    <td>7天</td>
    <th></th>
    <td>N/A</td>
    <td>24小時</td>
    <td>最多14天的90天時間範圍</td>
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

**已註冊的裝置？** 您會取得 PSSO/持續性 SSO <br>
**未註冊裝置？** 您會取得 SSO <br>
**未註冊的裝置，但 KMSI？** 您會取得 PSSO/持續性 SSO <p>
如果：
 - [x] 系統管理員已啟用 KMSI 功能 [和]
 - [x] 使用者按一下表單登入頁面上的 [KMSI] 核取方塊

  
只有當較新的重新整理權杖的有效性超過先前的權杖時，ADFS 才會發出新的重新整理權杖。 權杖的最長存留期為84天，但 AD FS 可讓權杖在14天的滑動時間範圍內保持有效。 如果重新整理權杖的有效時間為8小時（一般 SSO 時間），則不會發出新的重新整理權杖。
 

**好的須知：** <br>
未同步處理 **LastPasswordChangeTimestamp** 屬性的同盟使用者會發出會話 cookie，並重新整理具有 **最大存留期值12小時的** 權杖。<br>
發生這種情況是因為 Azure AD 無法判斷何時要撤銷與舊認證相關的權杖 (例如) 變更的密碼。 因此，Azure AD 必須更頻繁地進行檢查，以確保使用者和相關聯的權杖仍處於良好的地位。
