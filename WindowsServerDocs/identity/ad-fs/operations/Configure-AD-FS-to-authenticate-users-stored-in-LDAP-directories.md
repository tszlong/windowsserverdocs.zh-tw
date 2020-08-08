---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: 設定 AD FS 驗證 LDAP 目錄中儲存的使用者
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 95dfeb427aa67bce56c3f87f2c356eff34aac6c4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962502"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories-in-windows-server-2016-or-later"></a>設定 AD FS 以驗證儲存在 Windows Server 2016 或更新版本中 LDAP 目錄中的使用者

下列主題說明讓您的 AD FS 基礎結構驗證使用者，其身分識別儲存在輕量型目錄存取協定 (LDAP) v3 相容目錄所需的設定。

在許多組織中，身分識別管理解決方案都是由 Active Directory、AD LDS 或協力廠商 LDAP 目錄的組合所組成。 藉由新增 AD FS 支援來驗證儲存在 LDAP v3 相容目錄中的使用者，無論您的使用者身分識別儲存在何處，您都可以受益于整個企業級 AD FS 功能集。 AD FS 支援任何與 LDAP v3 相容的目錄。

> [!NOTE]
> 部分 AD FS 功能包括單一登入 (SSO) 、裝置驗證、彈性的條件式存取原則、透過與 Web 應用程式 Proxy 的整合，從任何地方進行工作的支援，以及與 Azure AD 的緊密同盟，讓您和您的使用者能夠利用雲端，包括 Office 365 和其他 SaaS 應用程式。  如需詳細資訊，請參閱[Active Directory 同盟服務總覽](../ad-fs-overview.md)。

為了讓 AD FS 從 LDAP 目錄驗證使用者，您必須建立**本機宣告提供者信任**，將此 LDAP 目錄連接到您的 AD FS 伺服器陣列。  本機宣告提供者信任是一個信任物件，代表您 AD FS 伺服器陣列中的 LDAP 目錄。 本機宣告提供者信任物件由各種識別碼、名稱和規則所組成，可將此 LDAP 目錄識別到本機 federation service。

藉由新增多個**本機宣告提供者信任**，您可以在相同的 AD FS 伺服器陣列中，支援多個 LDAP 目錄，每個都有自己的設定。 此外，AD FS 所在的樹系不信任的 AD DS 樹系也可以模型化為本機宣告提供者信任。 您可以使用 Windows PowerShell 來建立本機宣告提供者信任。

 (本機宣告提供者信任的 LDAP 目錄，) 可以與 AD 目錄共存， (宣告提供者信任) 在同一個 AD FS 伺服器陣列中，因此，AD FS 的單一實例能夠驗證和授權同時儲存在 AD 和非 AD 目錄中的使用者存取權。

僅支援以表單為基礎的驗證來驗證 LDAP 目錄中的使用者。 驗證 LDAP 目錄中的使用者時，不支援以憑證為基礎和整合式 Windows 驗證。

AD FS 所支援的所有被動授權通訊協定（包括 SAML、WS-同盟和 OAuth）也支援儲存在 LDAP 目錄中的身分識別。

WS-TRUST active authorization 通訊協定也支援儲存在 LDAP 目錄中的身分識別。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>設定 AD FS 來驗證 LDAP 目錄中儲存的使用者
若要設定您的 AD FS 伺服器陣列以驗證 LDAP 目錄中的使用者，您可以完成下列步驟：

1. 首先，使用**AdfsLdapServerConnection** Cmdlet 設定與 LDAP 目錄的連線：

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > 建議您為想要連接的每個 LDAP 伺服器建立新的連線物件。 AD FS 可以連接到多個複本 LDAP 伺服器，並在特定的 LDAP 伺服器關閉時自動故障切換。 針對這種情況，您可以為每個複本 LDAP 伺服器建立一個 AdfsLdapServerConnection，然後使用**AdfsLocalClaimsProviderTrust**指令程式的-**LdapServerConnection**參數來新增連線物件的陣列。

   **注意：** 您嘗試使用 Get-Credential，並輸入用來系結至 LDAP 實例的 DN 和密碼，可能會導致失敗，因為特定輸入格式的使用者介面需求，例如，網域 \ （或） user@domain.tld 。 您可以改為使用 Convertto-html-SecureString Cmdlet，如下所示 (範例假設 uid = admin，ou = system 作為用來系結至 LDAP 實例) 認證的 DN：

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   然後輸入 uid = admin 的密碼，並完成其餘的步驟。

2. 接下來，您可以使用**AdfsLdapAttributeToClaimMapping** Cmdlet，執行將 LDAP 屬性對應至現有 AD FS 宣告的選擇性步驟。 在下列範例中，您會將 givenName、姓氏和 CommonName LDAP 屬性對應至 AD FS 宣告：

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   這項對應的目的是為了讓 LDAP 存放區中的屬性可做為 AD FS 中的宣告，以便在 AD FS 中建立條件式存取控制規則。 它也可讓 AD FS 在 LDAP 存放區中使用自訂架構，方法是提供簡單的方法，將 LDAP 屬性對應至宣告。

3. 最後，您必須使用**AdfsLocalClaimsProviderTrust** Cmdlet，以本機宣告提供者信任的 AD FS 來註冊 LDAP 存放區：

   ```
   Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

   # Connection info
   -LdapServerConnection $vendorDirectory

   # How to locate user objects in directory
   -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic

   # Claims for authenticated users
   -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName)

   # General claims provider properties
   -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true

   # Optional - supply user name suffix if you want to use Ws-Trust
   -OrganizationalAccountSuffix "vendors.contoso.com"
   ```

   在上述範例中，您會建立名為「廠商」的本機宣告提供者信任。 您要指定連接資訊，以供 AD FS 連接到此本機宣告提供者信任所代表的 LDAP 目錄，方法是指派 `$vendorDirectory` 給 `-LdapServerConnection` 參數。 請注意，在第一步中，您已指派 `$vendorDirectory` 連接字串，以在連線到您特定的 LDAP 目錄時使用。 最後，您會指定要 `$GivenName` `$Surname` `$CommonName` 將對應至 AD FS 宣告) 的、和 LDAP 屬性 (用於條件式存取控制，包括多重要素驗證原則和發佈授權規則，以及透過 AD FS 發行的安全性權杖中的宣告來發行。 若要使用作用中的通訊協定（例如 Ws-trust 與 AD FS），您必須指定 OrganizationalAccountSuffix 參數，這可讓 AD FS 在服務使用中的授權要求時，能夠區分本機宣告提供者信任。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)
