---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: "設定驗證原則"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configure-authentication-policies"></a>設定驗證原則

>適用於： Windows Server 2012 R2

AD FS，在 Windows Server 2012 R2 中, 存取控制和驗證機制增強了多因素包含使用者、 裝置、 位置及驗證資料。 這些調節可讓您，透過使用者介面或透過 Windows PowerShell，來管理的廣告 FS\ 保護的應用程式透過 multi\ 雙因素存取控制和 multi\ 雙因素驗證為基礎的使用者身分或群組成員資格網路位置，workplace\ 加入的裝置資料的存取權限授與和驗證狀態 multi\ 雙因素驗證 \(MFA\) 是時執行。  
  
如需有關在 Windows Server 2012 R2 在 Active Directory 同盟服務 \(AD FS\) MFA 和 multi\ 雙因素存取控制的詳細資訊，請下列主題：  


-   [加入的任何裝置 SSO 和順暢的第二個工作地點因數驗證跨公司應用程式](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [管理條件存取控制的風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>AD FS 管理 snap\ 中透過驗證原則的設定  
資格在**系統管理員**，或相當於、 在本機電腦上的最低需求才能完成這些程序。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)。   
  
AD FS，在 Windows Server 2012 R2，您可以指定在適用於所有應用程式與服務都會受到 AD FS 全域範圍驗證原則。 您也可以設定驗證原則的特定應用程式和服務，依賴廠商信任受到 AD FS。 指定特定應用程式可以依據驗證原則廠商信任不覆寫全球驗證原則。 如果全球或每個信賴驗證原則需要 MFA，MFA 廠商信任使用者嘗試這個信賴廠商信任驗證時觸發。 全球驗證原則是後援信賴廠商信任的應用程式和服務，不需要特定的設定的驗證原則。 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>若要在 Windows Server 2012 R2 全球設定主要驗證 
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在 [AD FS snap\ 中，按一下 [**驗證原則**。  
  
3.  在**主要驗證**區段中，按**編輯**旁邊**通用設定**。 您也可以按一下 right\**驗證原則**，然後選取 [**編輯全球主要驗證**，或在**動作**] 窗格中，選取**編輯全球主要驗證**。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  在**編輯全球驗證原則**視窗中，在**主要**索引標籤上，您可以全球驗證原則的一部分來進行下列設定：  
  
    -   用於主要驗證的驗證方法。 您可以選擇在可用的驗證方法**外部網路**和**內部**。  
  
    -   裝置驗證透過**讓裝置驗證**核取方塊。 如需詳細資訊，請查看[SSO 和順暢第二個因數驗證在公司應用程式加入的工作地點，從任何裝置](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>若要設定可以依據主要驗證廠商信任  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在 AD FS snap\ 中，按一下 [**驗證原則**\\**每可以廠商信任**，然後按一下您想要設定 [驗證原則的依賴廠商信任。  
  
3.  Right\-按一下信賴信任的您想要設定 [驗證原則，然後選取 [**編輯自訂主要驗證**，或在**執行**] 窗格中，選取**編輯自訂主要驗證**。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  在**編輯驗證原則 < relying\_party\_trust\_name >**視窗中，在**主要**索引標籤上，您可以設定的下列設定的一部分**每可以方信任**驗證原則：  
  
    -   使用者是否需要提供認證 sign\ 在每次透過**使用者的所提供的認證 sign\ 在每次的**核取方塊。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>若要設定多因素驗證全球  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在 [AD FS snap\ 中，按一下 [**驗證原則**。  
  
3.  在**Multi\ 雙因素驗證**區段中，按**編輯**旁邊**通用設定**。 您也可以按一下 right\**驗證原則**，並選取**全球編輯 Multi\ 雙因素驗證**，或在**動作**] 窗格中，選取**全球編輯 Multi\ 雙因素驗證**。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  在**編輯全球驗證原則**視窗中，在**Multi\ 雙因素**索引標籤上，您可以全球 multi\ 雙因素驗證原則的一部分來進行下列設定：  
  
    -   設定或條件 MFA 可用的選項，在透過**群組 Users\ 日**，**裝置**，並**位置**區段。  
  
    -   若要讓 MFA 適用於這些設定，您必須選取至少一個額外的驗證方法。 **憑證驗證**會預設使用的選項。 您也可以設定，例如 Windows Azure Active 驗證其他自訂額外的驗證方法。 如需詳細資訊，請查看[逐步解說快速入門： 管理其他多因素驗證敏感的應用程式的風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
> [!WARNING]  
> 您只能全球設定額外的驗證方法。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>若要設定可以依據 multi\ 雙因素驗證廠商信任  
  
1.  在伺服器管理員中，按一下**工具**，然後選取 [ **AD FS 管理**。  
  
2.  在 AD FS snap\ 中，按一下 [**驗證原則**\\**每可以廠商信任**，然後按一下您想要設定 MFA 信賴廠商信任。  
  
3.  Right\-按一下信賴信任的您想要設定 MFA，然後選取 [**編輯自訂 Multi\ 雙因素驗證**，或在**執行**] 窗格中，選取**編輯自訂 Multi\ 雙因素驗證**。  
  
4.  在**編輯驗證原則 < relying\_party\_trust\_name >**視窗中，在**Multi\ 雙因素**索引標籤上，您可以進行下列設定的一部分 per\ 信賴信任驗證原則：  
  
    -   設定或條件 MFA 可用的選項，在透過**群組 Users\ 日**，**裝置**，並**位置**區段。  

## <a name="configure-authentication-policies-via-windows-powershell"></a>設定 Windows PowerShell 透過驗證原則  
Windows PowerShell 中使用各種不同因素存取控制的更多彈性並的驗證方式，會提供 AD FS 進行驗證原則和授權的 Windows Server 2012 R2 中規則，所需實作 AD FS \-secured 資源條件為 true 存取。  
  
系統管理員或等本機電腦上的成員資格是才能完成這些程序的最低需求。  檢視詳細資料使用適當的帳號，並群組成員資格，[本機和網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)\ (go.microsoft.com\ fwlink\ 方式 http:///\/ # / 嗎？LinkId\ = 83477\)。   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>若要設定額外的驗證方式，透過 Windows PowerShell  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> 若要確認已順利執行這個命令時，您可以執行`Get-AdfsGlobalAuthenticationPolicy`命令。  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>若要設定 MFA per\ 可以廠商信任，使用者的群組成員資格資料會根據  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令：  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的名稱。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    $MfaClaimRule =「c: [輸入 = '」https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'」，值 = ~'」^(?i) < group_SID >$ '」] = > 問題 (輸入 ='」https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'」，值 '」https://schemas.microsoft.com/claims/multipleauthn'「);」 
    
    設定-AdfsRelyingPartyTrust – TargetRelyingParty $rp – AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> 更換 < group\_SID > 確保安全性識別碼的值與 \(SID\) 的 Active Directory \(AD\) 群組。  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>若要設定 MFA 全球根據使用者群組成員資格資料  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $MfaClaimRule =「c: [輸入 = '「https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'」，值 ='「group_SID'「]  
     = > 問題 (類型 = '」https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'」，值 ='」https://schemas.microsoft.com/claims/multipleauthn'」)。」  
      
    設定 AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 確定要取代*< group\_SID >*的值為您的廣告群組的 SID。  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>若要 MFA 全球使用者的位置為基礎的設定  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    $MfaClaimRule =「c: [輸入 = '「https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'」，值 ='「true_or_false'「]  
     = > 問題 (類型 = '」https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'」，值 ='」https://schemas.microsoft.com/claims/multipleauthn'」)。」  
  
    設定 AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> 確定要取代*< true\_or\_false >*使用`true`或`false`。 值特定規則條件是否存取要求來自外部網路或內部為基礎而定。  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>若要設定 MFA 全球根據使用者的裝置資料  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $MfaClaimRule =「c: [輸入 = '「https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'」，值 =」true_or_false「]  
     = > 問題 (類型 = '」https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'」，值 ='」https://schemas.microsoft.com/claims/multipleauthn'」)。」  
  
    設定 AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 確定要取代*< true\_or\_false >*使用`true`或`false`。 值而定，根據該裝置是否 workplace\ 加入特定規則條件。  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>若要設定 MFA 全球如果要求存取其中的外部裝置或從 non\ workplace\ 加入裝置  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> 確定要取代的兩個實例*< true\_or\_false >*使用`true`或`false`，它會隨著特定規則條件。 規則條件為基礎的裝置是否 workplace\ 加入和是否存取要求來自外部網路或內部網路。  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>如果您存取來自某些群組外部網路使用者全球設定 MFA  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  

    Set-AdfsAdditionalAuthenticationRule「c: [類型 = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`「，值 = `"group_SID`」] 與與 c2: [輸入 = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`」，值 = `"true_or_false`」] = > 問題 (類型 = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`」，值 ='」https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> 確定要取代*< group\_SID >*群組 SID 的值與和*< true\_or\_false >*的其中`true`或`false`，而定特定規則條件為基礎是否存取要求來自外部網路或內部網路。  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>若要權限授與應用程式的使用者資料是透過 Windows PowerShell  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的值。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > 確定要取代*< group\_SID >*的值為您的廣告群組的 SID。  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>授與應用程式，這位使用者的身分，AD FS 才安全的存取權是以驗證 MFA  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的值。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>授與僅 AD FS 受保護的應用程式的存取權的使用者如果存取要求是來自係 workplace\ 加入裝置  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的值。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $GroupAuthzRule = 」@RuleTemplate = `"Authorization`「  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c: [輸入 = `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`「，值 = ~ `"^(?i)true$`「] = > 問題 (輸入 = `"https://schemas.microsoft.com/authorization/claims/permit`」，值 = `"PermitUsersWithClaim`」);  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>以授與的存取權，僅 AD FS 受保護的應用程式已驗證的身分 MFA 的使用者如果存取要求是來自係 workplace\ 加入裝置  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的值。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>若要權限授與外部網路存取要求來自的使用者 MFA 的已驗證的身分，才 AD FS 受保護的應用程式  
  
1.  在您聯盟的伺服器，開放 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> 確定要取代*< relying\_party\_trust >*您信賴的派對信任的值。  
  
2.  在同一個 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $GroupAuthzRule = 」@RuleTemplate = `"Authorization`「  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1: [輸入 = `"https://schemas.microsoft.com/claims/authnmethodsreferences`「，值 = ~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`「] 與與  
    c2: [輸入 = `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`「，值 = ~ `"^(?i)false$`「] = > 問題 (輸入 = `"https://schemas.microsoft.com/authorization/claims/permit`」，值 = `"PermitUsersWithClaim`」); 」  

## <a name="additional-references"></a>其他參考資料  

[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)
