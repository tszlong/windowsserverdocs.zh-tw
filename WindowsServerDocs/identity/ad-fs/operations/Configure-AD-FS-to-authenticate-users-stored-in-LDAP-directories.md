---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: "AD FS 進行驗證使用者 LDAP 目錄中儲存的設定"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>AD FS 進行驗證使用者 LDAP 目錄中儲存的設定

>適用於：Windows Server 2016

下列主題描述，可讓您 AD FS 基礎結構，以驗證身分其儲存在輕量型 Directory 存取通訊協定 (LDAP) v3 相容目錄使用者所需的設定。

許多組織中的身分管理方案所組成的 Active Directory、AD LDS 或第三方 LDAP 目錄組合。 搭配 AD FS 驗證使用者 LDAP v3 相容目錄中儲存的支援，可受惠整個企業級的 AD FS 功能無論儲存您的使用者身分設定。 AD FS 支援任何 LDAP v3 相容 directory。

> [!NOTE]
> 部分 AD FS 功能包括單一登入 (SSO)，裝置驗證，彈性的條件存取原則、工作-從的任何位置點一下整合的應用程式網路 Proxy，並使用 Azure AD 接著順暢聯盟透過可讓您與您的使用者使用雲端，包括 Office 365 和其他 SaaS 應用程式的支援。  如需詳細資訊，請查看[Active Directory 同盟服務概觀](../../ad-fs/AD-FS-2016-Overview.md)。

為了讓 AD FS 進行驗證使用者 LDAP directory，您必須連接這個 LDAP directory 到您 AD FS 發電廠來建立**本機宣告提供者信任**。  本機宣告提供者信任是表示 AD FS 陣列中的 LDAP directory 信任物件。 本機宣告提供者信任物件包含許多不同的識別碼、名稱，以及找出本機同盟服務此 LDAP directory 規則。

您可以在多個 LDAP 目錄，每一個都有它自己的設定中新增多個相同 AD FS 發電廠支援**本機宣告提供者信任**。 此外，不受信任的樹系 AD FS 存放於 AD DS 森林也可以為本機宣告提供者信任以。 您可以使用 Windows PowerShell 來建立本機宣告提供者信任。

LDAP 目錄（本機宣告提供者信任）一起相同 AD FS 伺服器，在同一個 AD FS 農場上存在的廣告目錄（宣告提供者信任），因此，AD FS 一個執行個體的驗證並授權的存取，會儲存在兩 AD 和非 AD 目錄。

只表單架構的驗證，驗證使用者從 LDAP 目錄支援。 適用於驗證使用者 LDAP 目錄不支援憑證式和整合 Windows 驗證。

所有被動式授權通訊協定 AD FS，包括 SAML，WS 聯盟來支援的和的身分儲存 LDAP 目錄中的 OAuth 也支援。

也支援的身分儲存在 LDAP 目錄 Ws-trust 授權使用通訊協定。

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>AD FS 進行驗證使用者 LDAP directory 中儲存的設定
若要設定您的 AD FS 陣列驗證使用者從 LDAP directory，您可以完成下列步驟：

1.  首先，設定連接到您 LDAP directory 使用**新-AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > 建議您建立新的每個 LDAP 伺服器您想要連接到連接物件。 AD FS 可連接到多個複本 LDAP 伺服器，並自動容錯以防特定 LDAP 伺服器已關閉。 針對此類案例中，您可以為每個這些複本 LDAP 伺服器建立一個 AdfsLdapServerConnection 並再新增連接物件使用陣列-**LdapServerConnection**的參數**新增-AdfsLocalClaimsProviderTrust** cmdlet。

    **注意：**您嘗試使用 Get-Credential 並輸入一個 DN 和密碼會用來執行個體 LDAP 繫結可能造成失敗因為的使用者介面需求適用於特定輸入格式，例如網域 \ 使用者名稱或user@domain.tld。 您可以改為使用 ConvertTo-SecureString cmdlet 如下 (下方假設 uid = 組織單位，管理員為要用來繫結至 LDAP 執行個體的認證 DN = 系統):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    然後輸入的密碼 uid = 系統管理員，並完成其餘步驟。

2.  接下來，您可以執行步驟選用 LDAP 屬性對應至現有的 AD FS 主張使用**新-AdfsLdapAttributeToClaimMapping** cmdlet。 以下範例您地圖 givenName，姓氏和 CommonName LDAP 屬性 AD FS 宣告：

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    為了進行屬性從 LDAP 存放區提供建立 AD FS 條件存取控制規則，以便在 AD FS 宣告完成此對應。 它也會讓 AD FS 提供對應宣告 LDAP 屬性簡單的方式來使用自訂 LDAP 存放區結構描述。

3.  最後，您必須登記 LDAP 網上商店使用 AD FS 在本機宣告提供者信任使用**新增-AdfsLocalClaimsProviderTrust** cmdlet:

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

    在上面範例中，您會建立本機宣告提供者信任稱為「廠商」。 這個區域宣告提供者信任代表指派 AD FS 連接到 LDAP directory 連接資訊指定`$vendorDirectory`到`-LdapServerConnection`的參數。 請注意，在一個步驟中，您已指派給`$vendorDirectory`以供連接到您的特定 LDAP directory 時連接字串。 最後，您所指定的`$GivenName`，`$Surname`，以及`$CommonName`（在您對應至 AD FS 宣告）LDAP 屬性的條件存取控制，包括多因素驗證原則和發行授權規則，以及透過 AD FS 發行的安全性權杖中宣告發行的使用。 您必須使用 AD FS Ws-trust 像的作用中通訊協定，以來指定 OrganizationalAccountSuffix 參數，可讓明確之間本機宣告提供者信任維護授權使用要求時 AD FS。

## <a name="see-also"></a>也了
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)


