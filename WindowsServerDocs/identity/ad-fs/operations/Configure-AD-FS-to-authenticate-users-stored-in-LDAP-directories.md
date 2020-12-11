---
description: 深入瞭解：設定 AD FS 以驗證 Windows Server 2016 或更新版本中 LDAP 目錄中儲存的使用者
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: 設定 AD FS 驗證 LDAP 目錄中儲存的使用者
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: aa7bb41b22edafa6063e6015087f8188d841b52e
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046696"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories-in-windows-server-2016-or-later"></a>設定 AD FS 以驗證儲存在 Windows Server 2016 或更新版本之 LDAP 目錄中的使用者

下列主題描述讓您的 AD FS 基礎結構進行驗證的使用者，其身分識別會儲存在輕量型目錄存取協定 (LDAP) v3 相容目錄中。

在許多組織中，身分識別管理解決方案都是由 Active Directory、AD LDS 或協力廠商 LDAP 目錄的組合所組成。 藉由新增 AD FS 支援來驗證儲存在 LDAP v3 相容目錄中的使用者，不論您的使用者身分識別儲存在何處，您都可以受益于整個企業級的 AD FS 功能集。 AD FS 支援任何符合 LDAP v3 標準的目錄。

> [!NOTE]
> 部分 AD FS 功能包括單一登入 (SSO) 、裝置驗證、彈性的條件式存取原則、透過與 Web 應用程式 Proxy 整合的任何地方進行工作的支援，以及與 Azure AD 的緊密同盟，進而讓您和您的使用者利用雲端，包括 Office 365 和其他 SaaS 應用程式。  如需詳細資訊，請參閱 [Active Directory 同盟服務總覽](../ad-fs-overview.md)。

為了讓 AD FS 從 LDAP 目錄驗證使用者，您必須建立 **本機宣告提供者信任**，將此 LDAP 目錄連線至您的 AD FS 伺服器陣列。  本機宣告提供者信任是一種信任物件，代表您 AD FS 伺服器陣列中的 LDAP 目錄。 本機宣告提供者信任物件是由各種識別碼、名稱和規則所組成，此 LDAP 目錄可識別本機 federation service。

您可以透過新增多個 **本機宣告提供者信任**，在相同的 AD FS 陣列中支援多個 LDAP 目錄（每個都有自己的設定）。 此外，AD FS 居住的樹系不信任的 AD DS 樹系也可以模型化為本機宣告提供者信任。 您可以使用 Windows PowerShell 來建立本機宣告提供者信任。

 (本機宣告提供者信任的 LDAP 目錄) 可以與 AD 目錄並存 (宣告提供者信任在相同 AD FS 伺服器上) 相同的 AD FS 伺服器陣列中，因此，單一 AD FS 實例可以驗證和授權儲存在 AD 和非 AD 目錄中的使用者存取權。

只支援以表單為基礎的驗證，以驗證來自 LDAP 目錄的使用者。 以憑證為基礎的整合式 Windows 驗證不支援在 LDAP 目錄中驗證使用者。

針對儲存在 LDAP 目錄中的身分識別，也支援 AD FS 所支援的所有被動授權通訊協定，包括 SAML、WS-同盟和 OAuth。

針對儲存在 LDAP 目錄中的身分識別，也支援使用 WS-Trust active authorization protocol。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>設定 AD FS 以驗證儲存在 LDAP 目錄中的使用者
若要設定您的 AD FS 伺服器陣列以從 LDAP 目錄驗證使用者，您可以完成下列步驟：

1. 首先，使用 **AdfsLdapServerConnection** Cmdlet 設定與 LDAP 目錄的連線：

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > 建議您為每個想要連接的 LDAP 伺服器建立新的連線物件。 AD FS 可以連接到多個複本 LDAP 伺服器，並在特定 LDAP 伺服器關閉時自動容錯移轉。 在這種情況下，您可以為每個複本 LDAP 伺服器建立一個 AdfsLdapServerConnection，然後使用 **AdfsLocalClaimsProviderTrust** Cmdlet 的-**LdapServerConnection** 參數新增連線物件的陣列。

   **注意：** 如果您嘗試使用 Get-Credential 並輸入 DN 和密碼來系結至 LDAP 實例，可能會因為特定輸入格式的使用者介面需求（例如網域 \ 使用者或）而導致失敗 user@domain.tld 。 您可以改為使用 ConvertTo-SecureString Cmdlet，如下所示 (下列範例假設 uid = admin，ou = system 做為要用來系結至 LDAP 實例) 的認證 DN：

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   然後輸入 uid = admin 的密碼，並完成其餘的步驟。

2. 接下來，您可以使用 **AdfsLdapAttributeToClaimMapping 指令程式** ，執行將 LDAP 屬性對應至現有 AD FS 宣告的選擇性步驟。 在下列範例中，您會將 givenName、姓氏和 CommonName LDAP 屬性對應至 AD FS 宣告：

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   完成此對應的目的是為了讓 LDAP 存放區中的屬性可作為 AD FS 中的宣告，以便在 AD FS 中建立條件式存取控制規則。 它也可讓 AD FS 藉由提供簡單的方法將 LDAP 屬性對應至宣告，來使用 LDAP 存放區中的自訂架構。

3. 最後，您必須使用 **AdfsLocalClaimsProviderTrust** 指令程式，向 AD FS 註冊 LDAP 存放區，作為本機宣告提供者信任：

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

   在上述範例中，您會建立名為「廠商」的本機宣告提供者信任。 您要指定連接資訊，讓 AD FS 連接到這個本機宣告提供者信任所代表的 LDAP 目錄，方法是指派 `$vendorDirectory` 給 `-LdapServerConnection` 參數。 請注意，在第一步，您已指派 `$vendorDirectory` 連接到特定 LDAP 目錄時所要使用的連接字串。 最後，您會指定 `$GivenName` `$Surname` `$CommonName` (您對應至 AD FS 宣告) 的、和 LDAP 屬性用於條件式存取控制，包括多重要素驗證原則和發佈授權規則，以及透過 AD FS 發行的安全性權杖中的宣告進行發佈。 若要使用作用中的通訊協定，例如 Ws-Trust 與 AD FS，您必須指定 OrganizationalAccountSuffix 參數，讓 AD FS 在服務使用中的授權要求時，能夠區分本機宣告提供者信任。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../ad-fs-operations.md)
