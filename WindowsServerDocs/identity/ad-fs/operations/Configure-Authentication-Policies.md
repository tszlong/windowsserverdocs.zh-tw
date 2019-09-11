---
ms.assetid: 8e7015bc-c489-4ec7-8b6e-3ece90f72317
title: 使用驗證原則
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5bd53022646b554e4a6cc09925607c66cead21e9
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865903"
---
# <a name="configure-authentication-policies"></a>使用驗證原則

在 AD FS 的 Windows Server 2012 R2 中，存取控制和驗證機制都會以包含使用者、裝置、位置和驗證資料的多項因素加以增強。 這些增強功能可讓您透過使用者介面或 Windows PowerShell 來管理，透過多重\- \-要素存取控制授與存取權限給 AD FS 安全應用程式的風險，以及多個\-根據使用者身分識別或群組成員資格、網路位置、加入工作地點\-的裝置資料，以及多重\-要素驗證\(MFA時的驗證狀態的因素驗證\)已執行。  

如需 Windows Server 2012 R2 Active Directory 同盟服務\- \(AD FS\) MFA 和多重要素存取控制的詳細資訊，請參閱下列主題：  


-   [從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)

-   [使用條件式存取控制管理風險](../../ad-fs/operations/Manage-Risk-with-Conditional-Access-Control.md)

-   [透過其他多重要素驗證管理機密應用程式的風險](../../ad-fs/operations/Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)

## <a name="configure-authentication-policies-via-the-ad-fs-management-snap-in"></a>透過 AD FS 管理嵌入式管理單元\-來設定驗證原則  
若要完成這些程序，至少需要本機電腦上之 **Administrators** 群組的成員資格或同等權限。  請參閱[本機與網域的預設群組](https://go.microsoft.com/fwlink/?LinkId=83477)中關於使用適當帳戶和群組成員資格的詳細資料。   

在 AD FS 的 Windows Server 2012 R2 中，您可以指定全域範圍的驗證原則，而該領域適用于 AD FS 保護的所有應用程式和服務。 您也可以針對依賴合作物件信任且受到 AD FS 保護的特定應用程式和服務，設定驗證原則。 針對每個信賴憑證者信任指定特定應用程式的驗證原則，並不會覆寫全域驗證原則。 如果全域或每一信賴憑證者信任驗證原則需要 MFA，則會在使用者嘗試向此信賴憑證者信任進行驗證時觸發 MFA。 全域驗證原則是一種信賴憑證者信任，適用于沒有特定已設定驗證原則的應用程式和服務。 

## <a name="to-configure-primary-authentication-globally-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 中全域設定主要驗證 

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在 AD FS\-嵌入式管理單元中，按一下 **驗證原則**。  

3.  在 [**主要驗證**] 區段中，按一下 [**全域設定**] 旁的 [**編輯**]。 您也可以用\-滑鼠右鍵按一下 [**驗證原則**]，選取 [**編輯全域主要驗證**]，或在 [**動作**] 窗格下選取 [**編輯全域主要驗證**]。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy1.png)

4.  在 [**編輯全域驗證原則**] 視窗的 [**主要**] 索引標籤上，您可以將下列設定設為全域驗證原則的一部分：  

    -   要用於主要驗證的驗證方法。 您可以選取**外部**網路和**內部**網路底下可用的驗證方法。  

    -   透過 [**啟用裝置驗證**] 核取方塊進行裝置驗證。 如需詳細資訊，請參閱[從任何裝置加入工作地點網路，並在公司的各個應用程式提供 SSO 和無縫式的次要因素驗證](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy2.png)  

## <a name="to-configure-primary-authentication-per-relying-party-trust"></a>若要設定每個信賴憑證者信任的主要驗證  

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在 AD FS\-嵌入式管理單元中，按一下 **每個信賴**憑證者信任的**驗證原則**\\，然後按一下您要設定驗證原則的信賴憑證者信任。  

3.  以滑鼠\-按右鍵您要設定驗證原則的信賴憑證者信任，然後選取 [**編輯自訂主要驗證**]，或在 [**動作**] 窗格下選取 [**編輯自訂主要]驗證**。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy5.png)   

4.  在 [**編輯 < 信賴\_ \_憑證者信任\_名稱的驗證原則 >** ] 視窗的 [**主要**] 索引標籤底下，您可以將下列設定設為**每個信賴**憑證者信任的一部分驗證原則：  

    -   使用者是否需要在每次登入時透過使用者提供其\-認證，每 **\-次登入時都必須提供其**認證核取方塊。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy6.png) 

## <a name="to-configure-multi-factor-authentication-globally"></a>全域設定多重要素驗證  

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在 AD FS\-嵌入式管理單元中，按一下 **驗證原則**。  

3.  在 [**多重\-要素驗證**] 區段中，按一下 [**全域設定**] 旁的 [**編輯**]。 您也可以用\-滑鼠右鍵按一下 [**驗證原則**]，然後選取 [**編輯全域多重\-要素驗證**]，或在 [**動作**] 窗格下選取 [**編輯全域多重\-要素驗證]** .  
![驗證原則](media/Configure-Authentication-Policies/authpolicy8.png)   

4.  在 [**編輯全域驗證原則**] 視窗的 [**多重\-要素**] 索引標籤底下，您可以將下列設定設為全域\-多重要素驗證原則的一部分：  

    -   透過 [**使用者\/群組**]、[**裝置**] 和 [**位置**] 區段下的可用選項來進行 MFA 的設定或條件。  

    -   若要針對上述任一設定啟用 MFA，您必須至少選取一個額外的驗證方法。 **憑證驗證**是預設的可用選項。 您也可以設定其他自訂的其他驗證方法，例如 Windows Azure Active Authentication。 如需詳細資訊， [請參閱逐步解說指南：透過其他多因素驗證管理機密應用程式](../../ad-fs/operations/Walkthrough-Guide--Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)的風險。  

> [!WARNING]  
> 您只能以全域方式設定其他驗證方法。  
![驗證原則](media/Configure-Authentication-Policies/authpolicy9.png)  

## <a name="to-configure-multi-factor-authentication-per-relying-party-trust"></a>若要設定\-每個信賴憑證者信任的多重要素驗證  

1.  在伺服器管理員中，按一下 [**工具**]，然後選取 [ **AD FS 管理**]。  

2.  在 AD FS\-嵌入式管理單元中，按一下 **每個信賴**憑證者信任的**驗證原則**\\，然後按一下您要設定 MFA 的信賴憑證者信任。  

3.  以滑鼠\-按右鍵您要設定 MFA 的信賴憑證者信任，然後選取 [**編輯自訂多重\-要素驗證**]，或在 [**動作**] 窗格下選取 [**編輯\-自訂多個]要素驗證**。  

4.  在 [ **<\_信賴\_憑證者信任\_名稱 >** ] 視窗的 [編輯驗證原則] 中，您可以在 [**多重\-要素**] 索引標籤下，將下列設定設為每個\-信賴憑證者信任驗證原則：  

    -   透過 [**使用者\/群組**]、[**裝置**] 和 [**位置**] 區段下的可用選項來進行 MFA 的設定或條件。  

## <a name="configure-authentication-policies-via-windows-powershell"></a>透過 Windows PowerShell 設定驗證原則  
Windows PowerShell 可讓您更有彈性地使用各種不同的存取控制因素，以及 Windows Server 2012 R2 AD FS 中可用的驗證機制，以設定所需的驗證原則和授權規則。針對 AD FS \-保護的資源，執行真正的條件式存取。  

若要完成這些程式，至少需要本機電腦上系統管理員的成員資格或同等許可權。  如需使用適當帳戶和群組成員資格的詳細資料，請參閱[本機與網域預設群組](https://go.microsoft.com/fwlink/?LinkId=83477) \(HTTP：\/ \/go.microsoft.com\/fwlink\/？LinkId\=83477\)。   

### <a name="to-configure-an-additional-authentication-method-via-windows-powershell"></a>透過 Windows PowerShell 設定其他驗證方法  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
`Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider CertificateAuthentication  `
~~~


> [!WARNING]  
> 若要確認是否已順利執行此命令，可以執行 `Get-AdfsGlobalAuthenticationPolicy` 命令。  

### <a name="to-configure-mfa-per-relying-party-trust-that-is-based-on-a-users-group-membership-data"></a>根據使用者的群組\-成員資格資料，設定每個信賴憑證者信任的 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，並執行下列命令：  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!WARNING]  
> 請務必以您的信賴憑證者信任名稱取代 *< 信賴\_ \_憑證者信任 >* 。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  


~~~
$MfaClaimRule = “c:[Type == ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'”, Value =~ ‘“^(?i) <group_SID>$'”] => issue(Type = ‘“https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'”, Value ‘“https://schemas.microsoft.com/claims/multipleauthn'”);” 

Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules $MfaClaimRule
~~~


> [!NOTE]  
> 請務必將 < 群組\_SID > 取代為 Active Directory \(AD\)群組的安全\(識別碼\) SID 值。  

### <a name="to-configure-mfa-globally-based-on-users-group-membership-data"></a>根據使用者的群組成員資格資料，全域設定 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid'", Value == ‘"group_SID'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> 請務必將 *< 群組\_sid >* 取代為您 AD 群組的 sid 值。  

### <a name="to-configure-mfa-globally-based-on-users-location"></a>根據使用者的位置，全域設定 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
$MfaClaimRule = “c:[Type == ‘" https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == ‘"true_or_false'"]  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");”  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~



> [!NOTE]  
> 請務必以`true`或 *\_取代 <\_true 或 false >。* `false` 此值取決於您的特定規則條件，其依據是存取要求是否來自外部網路或內部網路。  

### <a name="to-configure-mfa-globally-based-on-users-device-data"></a>根據使用者的裝置資料，全域設定 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
$MfaClaimRule = "c:[Type == ‘" https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == ‘"true_or_false"']  
 => issue(Type = ‘"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value = ‘"https://schemas.microsoft.com/claims/multipleauthn'");"  

Set-AdfsAdditionalAuthenticationRule $MfaClaimRule  
~~~


> [!NOTE]  
> 請務必以`true`或 *\_取代 <\_true 或 false >。* `false` 此值取決於您根據裝置是否已加入工作地點\-而定的特定規則條件。  

### <a name="to-configure-mfa-globally-if-the-access-request-comes-from-the-extranet-and-from-a-non-workplace-joined-device"></a>若要在存取要求來自外部網路和未\-加入工作場所\-的裝置時，全域設定 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
`Set-AdfsAdditionalAuthenticationRule "c:[Type == '"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser'", Value == '"true_or_false'"] && c2:[Type == '"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork'", Value == '" true_or_false '"] => issue(Type = '"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod'", Value ='"https://schemas.microsoft.com/claims/multipleauthn'");" ` 
~~~


> [!NOTE]  
> 請務必將 *< true\_ \_或 false >* `true`的兩個實例都取代`false`為或，這取決於您的特定規則條件。 規則條件取決於裝置是否已加入工作地點\-，以及存取要求是否來自外部網路或內部網路。  

### <a name="to-configure-mfa-globally-if-access-comes-from-an-extranet-user-that-belongs-to-a-certain-group"></a>若要在存取來自屬於特定群組的外部網路使用者時，全域設定 MFA  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
Set-AdfsAdditionalAuthenticationRule "c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value == `"group_SID`"] && c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value== `"true_or_false`"] => issue(Type = `"https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`", Value =`"https://schemas.microsoft.com/claims/
~~~

> [!NOTE]  
> 請務必以群組 sid 的值取代 *<\_群組 sid >* ，並 *< true\_或\_false >* 使用`true`或`false`，這取決於您的特定規則條件是以存取要求是否來自外部網路或內部網路為基礎。  

### <a name="to-grant-access-to-an-application-based-on-user-data-via-windows-powershell"></a>根據使用者資料，透過 Windows PowerShell 授與應用程式的存取權  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> 請務必將 *< 信賴\_ \_憑證者信任 >* 取代為信賴憑證者信任的值。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  

   ```  

     $GroupAuthzRule = "@RuleTemplate = `“Authorization`” @RuleName = `"Foo`" c:[Type == `"https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`", Value =~ `"^(?i)<group_SID>$`"] =>issue(Type = `"https://schemas.microsoft.com/authorization/claims/deny`", Value = `"DenyUsersWithClaim`");"  
   Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –IssuanceAuthorizationRules $GroupAuthzRule  
   ```  

> [!NOTE]  
> > 請務必將 *< 群組\_sid >* 取代為您 AD 群組的 sid 值。  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-this-users-identity-was-validated-with-mfa"></a>授與受 AD FS 保護之應用程式的存取權，只有在使用 MFA 驗證過此使用者的身分識別時  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> 請務必將 *< 信賴\_ \_憑證者信任 >* 取代為信賴憑證者信任的值。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  

   ```  
   $GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
   @RuleName = `"PermitAccessWithMFA`"  
   c:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)https://schemas\.microsoft\.com/claims/multipleauthn$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = ‘“PermitUsersWithClaim'");"  

   ```  

### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-the-user"></a>授與受 AD FS 保護之應用程式的存取權，只有在存取要求是來自已加入\-工作場所的裝置時，才會向使用者註冊  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  

    ```  
    $rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust  

    ```  

> [!NOTE]  
> 請務必將 *< 信賴\_ \_憑證者信任 >* 取代為信賴憑證者信任的值。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"PermitAccessFromRegisteredWorkplaceJoinedDevice`"  
c:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");  
~~~



### <a name="to-grant-access-to-an-application-that-is-secured-by-ad-fs-only-if-the-access-request-comes-from-a-workplace-joined-device-that-is-registered-to-a-user-whose-identity-has-been-validated-with-mfa"></a>若要授與受 AD FS 保護之應用程式的存取權，只有在存取要求是來自\-已加入工作場所的裝置，且其身分識別已通過 MFA 驗證之後才會進行  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust ` 
~~~


> [!NOTE]  
> 請務必將 *< 信賴\_ \_憑證者信任 >* 取代為信賴憑證者信任的值。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  

   ```  
   $GroupAuthzRule = ‘@RuleTemplate = “Authorization”  
   @RuleName = “RequireMFAOnRegisteredWorkplaceJoinedDevice”  
   c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
   c2:[Type == `"https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser`", Value =~ `"^(?i)true$”] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  

   ```  

### <a name="to-grant-extranet-access-to-an-application-secured-by-ad-fs-only-if-the-access-request-comes-from-a-user-whose-identity-has-been-validated-with-mfa"></a>只有在存取要求來自其身分識別已通過 MFA 驗證的使用者時，才將外部網路存取權授與 AD FS 保護的應用程式  

1.  在您的同盟伺服器上，開啟 Windows PowerShell 命令視窗，然後執行下列命令。  


~~~
`$rp = Get-AdfsRelyingPartyTrust –Name relying_party_trust`  
~~~


> [!NOTE]  
> 請務必將 *< 信賴\_ \_憑證者信任 >* 取代為信賴憑證者信任的值。  

2. 在相同的 Windows PowerShell 命令視窗中，執行下列命令。  


~~~
$GroupAuthzRule = "@RuleTemplate = `"Authorization`"  
@RuleName = `"RequireMFAForExtranetAccess`"  
c1:[Type == `"https://schemas.microsoft.com/claims/authnmethodsreferences`", Value =~ `"^(?i)http://schemas\.microsoft\.com/claims/multipleauthn$`"] &&  
c2:[Type == `"https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`", Value =~ `"^(?i)false$`"] => issue(Type = `"https://schemas.microsoft.com/authorization/claims/permit`", Value = `"PermitUsersWithClaim`");"  
~~~

## <a name="additional-references"></a>其他參考資料  

[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)
