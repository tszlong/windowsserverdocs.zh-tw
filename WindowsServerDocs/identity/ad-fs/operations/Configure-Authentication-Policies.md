---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: 使用驗證原則
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7faffb7ccbb4b0ea3c65329d18f915d7dafcd46f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861789"
---
# <a name="configure-authentication-policies"></a>使用驗證原則

>適用於：Windows Server 2012 R2

在 Windows Server 2012 r2 中的 AD FS 中存取控制項，及增強以包括使用者、 裝置、 位置和驗證資料的多重要素驗證機制。 這些增強功能可讓您透過使用者介面或透過 Windows PowerShell，來管理存取權限授與 AD FS 的風險\-保護應用程式，透過多重\-因素存取控制和多重\-根據使用者的身分識別或群組的成員資格、 網路位置、 工作場所的裝置資料的 authentication\-加入，並驗證狀態時多\-authentication \(MFA\)執行。  
  
如需 MFA 和多重\-因素中 Active Directory Federation Services 的存取控制\(AD FS\)在 Windows Server 2012 R2，請參閱下列主題：  


-   [從任何裝置加入工作地點網路提供 SSO 和無縫式次要因素驗證，跨公司應用程式](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [使用條件式存取控制管理風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [透過機密應用程式的其他多因素驗證管理風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>設定驗證原則，透過 AD FS 管理嵌入式管理單元\-中  
若要完成這些程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   
  
在 Windows Server 2012 R2 中的 AD FS，您可以指定適用於所有的應用程式和受保護的 AD FS 服務的全域範圍的驗證原則。 您也可以設定特定的應用程式和服務會依賴信任和受保護的 AD FS 的驗證原則。 指定驗證原則特定的應用程式，每個信賴憑證者信任不覆寫全域驗證原則。 如果全域或每個信賴憑證者信任驗證原則需要 MFA，MFA 會在使用者嘗試向此信賴憑證者信任時觸發。 全域驗證原則是信賴憑證者信任的應用程式和服務，並沒有特定的設定的驗證原則的後援。 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>若要在 Windows Server 2012 R2 中的全域設定主要驗證 
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在 AD FS 嵌入式管理單元\-中，按一下**驗證原則**。  
  
3.  在 **主要驗證**區段中，按一下**編輯**旁**全域設定**。 您也可以滑鼠右鍵\-按一下 **驗證原則**，然後選取**編輯全域主要驗證**，或者，在**動作**窗格中，選取**編輯全域主要驗證**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy1.png)
  
4.  在 [**編輯全域驗證原則**] 視窗，請在**主要**索引標籤上，您可以設定下列設定為全域驗證原則的一部分：  
  
    -   要用於主要驗證的驗證方法。 您可以選取可用的驗證方法，底下**外部網路**並**內部網路**。  
  
    -   透過裝置驗證**啟用裝置驗證**核取方塊。 如需詳細資訊，請參閱 [Join to Workplace from Any Device for SSO and Seamless Second Factor Authentication Across Company Applications](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。  
![auth policies](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>若要設定主要驗證每個信賴憑證者信任  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在 AD FS 嵌入式管理單元\-中，按一下**驗證原則**\\**每個信賴憑證者信任**，然後按一下 信賴憑證者信任，您想要設定驗證原則。  
  
3.  直接\-，請按一下您要設定驗證原則，然後選取信賴憑證者的合作對象信任**編輯自訂主要驗證**，或者，在**動作**] 窗格中，選取 [**編輯自訂主要驗證**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  在 **編輯驗證原則 < 信賴憑證者\_合作對象\_信任\_名稱 >** 視窗下**主要**索引標籤上，您可以設定下列設定做為一部分**每個信賴憑證者信任**驗證原則：  
  
    -   使用者是否需要提供其認證，每次登\-中透過**使用者必須提供其認證，每次登\-在**核取方塊。  
![auth policies](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>若要全域設定多重要素驗證  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在 AD FS 嵌入式管理單元\-中，按一下**驗證原則**。  
  
3.  在 **多重\-因素驗證**區段中，按一下**編輯**旁**全域設定**。 您也可以滑鼠右鍵\-按一下 **驗證原則**，然後選取**編輯全域多重\-因素驗證**，或在**動作**窗格中，選取**編輯全域多重\-因素驗證**。  
![auth policies](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  在**編輯全域驗證原則** 視窗底下**多\-因素**索引標籤上，您可以設定下列設定的全域多重一部分\-因素驗證原則：  
  
    -   設定或透過可用的選項，在 mfa 條件**使用者\/群組**，**裝置**，和**位置**區段。  
  
    -   若要啟用 MFA 的任一種設定，您必須選取至少一個其他驗證方法。 **憑證驗證**是預設可用的選項。 您也可以設定其他自訂的其他驗證方法，例如，Windows Azure Active Authentication。 如需詳細資訊，請參閱[逐步解說指南：透過機密應用程式的其他多因素驗證管理風險](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)。  
  
> [!WARNING]  
> 您只可以全域設定其他驗證方法。  
![auth policies](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>若要設定多\-要素驗證，每個信賴憑證者信任  
  
1.  在 [伺服器管理員] 中，按一下**工具**，然後選取**AD FS 管理**。  
  
2.  在 AD FS 嵌入式管理單元\-中，按一下**驗證原則**\\**每個信賴憑證者信任**，然後按一下 信賴憑證者信任，您想要設定 MFA。  
  
3.  直接\-，請按一下您要設定 MFA，然後選取信賴憑證者的合作對象信任**編輯自訂多\-因素驗證**，或者，在**動作**] 窗格中，選取 [**編輯自訂多\-因素驗證**。  
  
4.  在**編輯驗證原則 < 信賴憑證者\_合作對象\_信任\_名稱 >** 視窗下方**多\-因素**索引標籤上，您可以設定下列設定的一部分每\-信賴憑證者信任驗證原則：  
  
    -   設定或透過可用的選項，在 mfa 條件**使用者\/群組**，**裝置**，和**位置**區段。  

## <a name="configure-authentication-policies-via-windows-powershell"></a>設定驗證原則，透過 Windows PowerShell  
Windows PowerShell 可讓更大的彈性來使用各種因素的存取控制和驗證機制，可在設定驗證原則和授權的 Windows Server 2012 R2 中的 AD FS 規則，所需適用於 AD FS 實作，則為 true 的條件式存取\-保護的資源。  
  
完成這些程序的最小需求的系統管理員，或同等權限，在本機電腦上的成員資格。  檢閱詳細使用適當帳戶和群組成員[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(http:\/\/go.microsoft.com\/fwlink\/嗎？LinkId\=83477\)。   
  
### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>若要設定其他驗證方法，透過 Windows PowerShell  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  

    `Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `

  
> [!WARNING]  
> 若要確認是否已順利執行此命令，可以執行 `Get-AdfsGlobalAuthenticationPolicy` 命令。  
  
### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>若要設定每個 MFA\-信賴憑證者信任，根據使用者的群組成員資格資料  
  
1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，並執行下列命令：  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
  
  
> [!WARNING]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的名稱。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  
 
    $MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’”, Value =~ ‘“^(?i) <group_SID>$’”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn’”);” 
    
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
  
  
> [!NOTE]  
> 請務必取代 < 群組\_SID > 與安全性識別項的值\(SID\) Active directory \(AD\)群組。  
  
### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>若要設定全域使用者的群組成員資格資料為基礎的 MFA  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  

    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid’", Value == ‘"group_SID’"]  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  
      
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 請務必取代 *< 群組\_SID >* AD 群組的 sid 值。  
  
### <a name="to-configure-mfa-globally-based-on-users-location"></a>若要設定全域使用者的位置為基礎的 MFA  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
 
    $MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork’", Value == ‘"true_or_false’"]  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");”  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
  

  
> [!NOTE]  
> 請務必取代 *< true\_或\_false >* 使用`true`或`false`。 值取決於您根據是否存取要求來自外部網路或內部網路的特定規則條件。  
  
### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>若要設定全域使用者的裝置資料為基礎的 MFA  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  

    $MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser’", Value == ‘"true_or_false"']  
     => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod’", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn’");"  
  
    Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  

  
> [!NOTE]  
> 請務必取代 *< true\_或\_false >* 使用`true`或`false`。 值取決於您的特定規則條件，根據裝置是否 workplace\-加入與否。  
  
### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>如果存取要求是從外部網路和非全域設定 MFA\-workplace\-已加入的裝置  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
 
    `Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
 
  
> [!NOTE]  
> 取代兩個執行個體，請務必 *< true\_或\_false >* 使用`true`或`false`，這取決於您特定的規則條件。 規則條件會根據裝置是否為 workplace\-聯結或存取要求不以及是否來自外部網路或內部網路。  
  
### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>如果存取來自外部網路的使用者屬於特定群組之全域設定 MFA  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  

    Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
      
> [!NOTE]  
> 請務必取代 *< 群組\_SID >* 的群組 SID 值和 *<，則為 true\_或\_false >* 使用`true`或`false`，這取決於您的特定規則條件，根據是否存取要求來自外部網路或內部網路。  
  
### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>若要授與存取權的使用者資料透過 Windows PowerShell 為基礎的應用程式  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的值。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
  
      $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
    Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
    ```  
  
> [!NOTE]  
> > 請務必取代 *< 群組\_SID >* AD 群組的 sid 值。  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>若要授與存取權會受到 AD FS 才這位使用者的身分識別的應用程式已通過 MFA 驗證  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  

    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的值。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessWithMFA`"  
    c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim’");"  
  
    ```  
  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>若要授與存取權的應用程式，會受到 AD FS 才存取要求來自 workplace\-加入的使用者已註冊的裝置  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  
  
    ```  
  
> [!NOTE]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的值。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
    c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
  

  
### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授與存取權的應用程式，會受到 AD FS 才存取要求來自 workplace\-加入其身分識別已通過 MFA 驗證的使用者已註冊的裝置  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
 
  
> [!NOTE]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的值。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  
    ```  
    $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
    @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
  
    ```  
  
### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授與存取要求來自身分識別已通過 MFA 驗證的使用者時，才由 AD FS 保護的應用程式的外部網路存取  
  
1.  在您的同盟伺服器上開啟 Windows PowerShell 命令視窗並執行下列命令。  
  
 
    `$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  

  
> [!NOTE]  
> 請務必取代 *< 信賴憑證者\_合作對象\_信任 >* 您信賴憑證者信任的值。  
  
2.  在相同的 Windows PowerShell 命令視窗中，執行下列命令。  
  

    $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
    @RuleName = `"RequireMFAForExtranetAccess`"  
    c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
    c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

## <a name="additional-references"></a>其他參考資料  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
