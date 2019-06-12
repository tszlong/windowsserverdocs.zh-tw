---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: 設定 AD FS 驗證 LDAP 目錄中儲存的使用者
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2053f0a93f33cdfdd85eec8cdbb6eca4ebad1ff0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444924"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>設定 AD FS 驗證 LDAP 目錄中儲存的使用者

下列主題說明讓您的 AD FS 基礎結構，來驗證其身分識別會儲存在輕量型目錄存取通訊協定 (LDAP) v3 相容目錄中的使用者所需的設定。

在許多組織來說，身分識別管理解決方案包含 Active Directory、 AD LDS 中或第三方 LDAP 目錄的組合。 透過 AD FS 支援的驗證 LDAP v3 相容目錄中儲存的使用者，就能輕易從整個企業級 AD FS 功能集，不論您的使用者身分識別的儲存位置。 AD FS 支援任何 LDAP v3 相容的目錄。

> [!NOTE]
> 某些 AD FS 功能包括單一登入 (SSO)，裝置驗證，有彈性的條件式存取原則、 支援和無縫式，從-雲遊透過整合的 Web 應用程式 proxy，然後依序的 Azure AD 的同盟可讓您和您的使用者利用雲端，包括 Office 365 和其他 SaaS 應用程式。  如需詳細資訊，請參閱 < [Active Directory Federation Services 概觀](../../ad-fs/AD-FS-2016-Overview.md)。

為了讓 AD FS 驗證 LDAP 目錄的使用者，您必須此 LDAP 目錄建立連線到您的 AD FS 伺服器陣列**本機宣告提供者信任**。  本機宣告提供者信任是一個信任物件，表示您的 AD FS 伺服器陣列中的 LDAP 目錄。 本機宣告提供者信任物件是由各種識別碼、 名稱和規則來識別此 LDAP 目錄，可讓本機 federation service 所組成。

您可以支援多個 LDAP 目錄，各有自己的組態中加入多個相同的 AD FS 伺服器陣列**本機宣告提供者信任**。 此外，AD DS 樹系不信任的 AD FS 位於樹系也可以建模為本機宣告提供者信任。 您可以使用 Windows PowerShell 來建立本機宣告提供者信任。

LDAP 目錄 （本機宣告提供者信任） 可以同時具有 AD 目錄 （宣告提供者信任） 存在相同的 AD FS 伺服器，在相同的 AD FS 伺服器陣列上，因此，AD FS 的單一執行個體能夠驗證和授權使用者的存取儲存在兩個 AD 和非 AD 目錄。

只有表單型驗證來驗證使用者，從 LDAP 目錄支援。 不支援憑證型 」 和 「 整合式 Windows 驗證驗證使用者在 LDAP 目錄中。

所有的被動授權通訊協定由 AD FS 中，包括 SAML、 WS-同盟、 支援和儲存在 LDAP 目錄中的身分識別也支援 OAuth。

儲存在 LDAP 目錄中的身分識別也支援 Ws-trust 作用中的授權通訊協定。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>設定 AD FS 驗證 LDAP 目錄中儲存的使用者
若要設定您的 AD FS 伺服器陣列，從 LDAP 目錄驗證使用者，您可以完成下列步驟：

1. 首先，設定 使用 LDAP 目錄連線**新增 AdfsLdapServerConnection** cmdlet:

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > 建議您建立新的連線物件，為您想要連接到每個 LDAP 伺服器。 AD FS 可以連接到多個複本 LDAP 伺服器，並自動容錯移轉，如果特定的 LDAP 伺服器已關閉。 這類情況下，您可以為每個這些複本 LDAP 伺服器建立一個 AdfsLdapServerConnection，然後再新增 使用的連接物件的陣列-**LdapServerConnection**參數**新增 AdfsLocalClaimsProviderTrust** cmdlet。

   **注意：** 因為您試圖使用 Get-credential，並輸入 DN] 和 [密碼用來繫結至 LDAP 執行個體可能會造成失敗的特定輸入的格式，例如，網域 \ 使用者名稱的使用者介面需求或user@domain.tld。 您可以改為使用 Convertto-securestring cmdlet 如下 (下列範例假設 uid = admin，ou = 系統做為認證的 DN，用來繫結至 LDAP 執行個體):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   然後輸入密碼，進行 uid = admin，並完成其餘步驟。

2. 接下來，您可以在其中執行將 LDAP 屬性對應到現有的 AD FS 宣告使用的選擇性步驟**新增 AdfsLdapAttributeToClaimMapping** cmdlet。 在下列範例中，您將對應 givenName、 姓氏和 CommonName LDAP 屬性加入至 AD FS 宣告：

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   此對應是為了提供屬性的 LDAP 存放區做為 AD FS 中以便在 AD FS 中建立條件式存取控制規則的宣告。 它也可讓 AD FS 提供 LDAP 屬性對應到宣告簡單的方式來使用 LDAP 存放區中的自訂結構描述。

3. 最後，您必須註冊的 LDAP 存放區與 AD FS 為本機宣告提供者信任使用**新增 AdfsLocalClaimsProviderTrust** cmdlet:

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

   在上述範例中，您會建立稱為 「 供應商 」 本機宣告提供者信任。 您指定這個本機宣告提供者信任代表藉由指派的連接資訊交由 LDAP 目錄連線的 AD fs`$vendorDirectory`至`-LdapServerConnection`參數。 請注意，在其中一個步驟中，您已指派`$vendorDirectory`連接至特定的 LDAP 目錄時要使用的連接字串。 最後，您會指定`$GivenName`， `$Surname`，和`$CommonName`LDAP 屬性 （這您對應至 AD FS 宣告） 是用於條件式存取控制，包括多重要素驗證原則及發行授權規則，以及與透過 AD FS 簽發安全性權杖中宣告的發行。 若要使用作用中的通訊協定，例如 Ws-trust 與 AD FS，您必須指定 OrganizationalAccountSuffix 參數，可讓 AD FS 來區分服務作用中的授權要求時，本機宣告提供者信任。

## <a name="see-also"></a>另請參閱
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


