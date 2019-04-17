---
ms.assetid: 1a443181-7ded-4912-8e40-5aa447faf00c
title: "AD FS 2016 單一登入設定"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/17/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e8c24399949efc1b8d0b1782e299593c02374c62
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="ad-fs-single-sign-on-settings"></a>AD FS 單一登入設定

>適用於：Windows Server 2016、Windows Server 2012 R2

單一登入 (SSO) 可讓使用者一次驗證並不會提示您輸入其他認證存取多個資源。  此文章將描述預設 AD FS 行為 SSO，以及設定可讓您來自訂此行為。  

## <a name="supported-types-of-single-sign-on"></a>支援的類型的單一登入

AD FS 支援單一登入體驗數種的類型：  
  
-   **工作階段 SSO**  
  
     工作階段 SSO cookie 寫入已驗證使用者，當使用者切換應用程式特定的工作階段期間不接下來的提示。 不過，如果在特定的工作階段結束時，使用者將會提示他們認證再試一次。  
  
     AD FS 可工作階段 SSO cookie 預設設定如果使用者的裝置會不登記完畢。 如果您在瀏覽器工作階段結束後，重新啟動此工作階段 cookie 刪除且不正確任何更多。  
  
-   **持續 SSO**  
  
     排除進一步提示時，使用者切換應用程式，只要將持續 SSO cookie 是有效的已驗證使用者寫入持續 SSO cookie。 持續 SSO 和工作階段 SSO 不同的是，可跨不同的工作階段維護持續 SSO。  
  
     如果裝置係 AD FS 將持續 SSO cookie。 AD FS 也會將設定持續性的 SSO cookie 如果使用者可選取 [[讓我保持登入] 的選項。 如果持續 SSO cookie 任何不正確，它會拒絕且刪除。  
  
-   **應用程式特定 SSO**  
  
     在 OAuth 案例中，重新整理預付碼用來維護 SSO 狀態範圍特定應用程式中的使用者。  
  
     如果裝置係，AD FS 將設定到期時間的預設為基礎的且已裝置 7 天持續 SSO cookie 期間重新整理預付碼。 如果使用者可選取 [[讓我保持登入] 的選項，到期的時間重新整理預付碼將會等號 [保留我登入「持續 SSO cookie 期間這是最多 7 天預設 1 天。 否則，請重新整理權杖期間等工作階段 SSO cookie 期間這是預設 8 小時  
  
 如上所述，且已裝置上的使用者會看到持續 SSO 除非持續 SSO 已停用。 可藉由讓 [保留我登入「達成持續 SSO 未且已裝置的功能 (KMSI)。 
 
 針對 Windows Server 2012 R2，以便 PSSO「[讓我保持登入「案例中，您需要安裝這個[hotfix](https://support.microsoft.com/en-us/kb/2958298/)也是部分的[2014 年 8 月更新彙總套件適用於 Windows RT 8.1，Windows 8.1、Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719)。   
 
  
## <a name="single-sign-on-and-authenticated-devices"></a>單一登入與驗證的裝置  
之後提供的認證第一次，預設的且已裝置的使用者取得單一登入期間最大的 90 天，他們可以使用裝置存取 AD FS 資源至少一次每個 14 天提供。  如果他們等待 15 日之後提供的認證，使用者將會提示輸入認證再試一次。  

預設會讓持續 SSO。 如果已停用，將不 PSSO cookie 寫入。|  

``` powershell
Set-AdfsProperties –EnablePersistentSso <Boolean\>
```     
  
裝置使用視窗（14 天預設）由 AD FS 屬性**DeviceUsageWindowInDays**。

``` powershell
Set-AdfsProperties -DeviceUsageWindowInDays
```   
最大單一登入期間（預設的 90 天）由 AD FS 屬性**PersistentSsoLifetimeMins**。

``` powershell
Set-AdfsProperties -PersistentSsoLifetimeMins
```    

## <a name="keep-me-signed-in-for-unauthenticated-devices"></a>保留我的裝置未驗證登入 
適用於非登記裝置單一登入期間由**繼續我登入 (KMSI)**設定的功能。  KMSI 預設停用，並可設定為 True AD FS 屬性 KmsiEnabled。

``` powershell
Set-AdfsProperties -EnableKmsi $true  
```    

使用 KMSI 停用，預設單一登入期間是 8 小時的時間。  這可以使用屬性 SsoLifetime 設定。  預設值是 480 屬性被以分鐘。  

``` powershell
Set-AdfsProperties –SsoLifetime <Int32\> 
```   

與支援 KMSI，預設單一登入期間為 24 小時。  這可以使用屬性 KmsiLifetimeMins 設定。  預設值是 1440年屬性被以分鐘。

``` powershell
Set-AdfsProperties –KmsiLifetimeMins <Int32\> 
```   

## <a name="multi-factor-authentication-mfa-behavior"></a>多因素驗證 (MFA) 問題  
請務必注意，雖然提供較長的單一登入，AD FS 會提示您輸入其他驗證 （多重因數驗證） 上的登入根據主要認證，並不 MFA，但目前登入需要 MFA。  這是無論 SSO 設定。 AD FS 收到的驗證要求時，第一次判斷是否是 （例如 cookie) SSO 操作，然後、 MFA 是否需要 (例如如果要求來自之外) 它將會評估是否 SSO 操作包含 MFA。  如果不行，系統會提示 MFA。  


  
## <a name="psso-revocation"></a>PSSO 撤銷  
 為保護安全性，AD FS 將拒絕先前發行符合下列條件時任何持續 SSO cookie。 這將會要求提供認證為了再次使用 AD FS 進行驗證使用者。 
  
-   使用者變更密碼  
  
-   AD FS 中停用持續 SSO 設定  
  
-   遺失或遭竊案例中的系統管理員裝置已停用  
  
-   AD FS 接收持續發出的使用者且已，但使用者的 SSO cookie 或不會再登記裝置  
  
-   AD FS 接收持續 SSO cookie 且已使用者，但使用者重新登記。  
  
-   AD FS 接收持續發出根據」[讓我保持登入」，但「[讓我保持登入] 的 SSO cookie AD FS 中停用設定  
  
-   AD FS 接收持續發出且已使用者的 SSO cookie，但裝置憑證會在驗證期間遺失或已變更  
  
-   AD FS 管理員已經設定為 SSO 持續的時間點。 AD FS 此設定時，將會拒絕發行之前這次任何持續 SSO cookie  
  
 若要設定時間點，執行下列 PowerShell cmdlet:  
  

``` powershell
Set-AdfsProperties -PersistentSsoCutoffTime <DateTime>
```
  
## <a name="enable-psso-for-office-365-users-to-access-sharepoint-online"></a>讓 PSSO Office 365 使用者存取 SharePoint Online  
 一旦 PSSO 支援，AD FS 中設定 AD FS 將寫入持續 cookie 使用者驗證。 如果持續 cookie 仍然有效，包含，使用者在下一次使用者不必提供認證來驗證再試一次。 您也可以避免額外的驗證命令提示字元中的 Office 365，並藉由設定以下兩個 SharePoint Online 使用者宣告 AD FS，Microsoft Azure AD 和 SharePoint Online 觸發程序保存在規則。  您需要安裝這個要使用的 Office 365 使用者存取 SharePoint online PSSO，請[hotfix](https://support.microsoft.com/en-us/kb/2958298/)也是部分的[2014 年 8 月更新彙總套件適用於 Windows RT 8.1，Windows 8.1、Windows Server 2012 R2](https://support.microsoft.com/en-us/kb/2975719)。  
  
 通過 InsideCorporateNetwork 理賠要求發行轉換規則  
  
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
  
  
    


