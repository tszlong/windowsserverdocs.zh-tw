---
title: AD FS 疑難排解-Azure AD
description: 本檔說明如何針對 AD FS 和 Azure AD 的各個層面進行疑難排解
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 5e90ad9fbd2ae9dbb08d2137ead0705556184858
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966900"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>AD FS 疑難排解-Azure AD
隨著雲端的成長，許多公司都已繼續為其各種應用程式和服務使用 Azure AD。  與 Azure AD 的聯盟已成為許多組織的標準實務。  本檔將涵蓋此同盟所引發之問題的一些疑難排解部分。  一般疑難排解檔中的數個主題仍屬於與 Azure 的同盟，因此本檔僅著重于 Azure AD 和 AD FS 互動方面的細節。

## <a name="redirection-to-ad-fs"></a>重新導向至 AD FS
當您登入 Office 365 之類的應用程式時，就會進行重新導向，而您的組織 AD FS 伺服器進行登入。

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>要檢查的第一個事項
如果未發生重新導向，您會想要檢查幾件事

   1. 請確認您的 Azure AD 租使用者已啟用同盟，方法是登入 Azure 入口網站並核取 [Azure AD Connect]。

![](media/ad-fs-tshoot-azure/azure2.png)

1. 請確定您的自訂網域已通過驗證，方法是按一下 Azure 入口網站中 [同盟] 旁的網域。
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. 最後，您會想要檢查[DNS](ad-fs-tshoot-dns.md) ，並確定您的 AD FS 伺服器或 WAP 伺服器是從網際網路解析。  確認這會解析，而且您可以流覽至該檔案。
3. 您也可以使用 PowerShell cmdlt `Get-AzureADDomain` 來取得此資訊。

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>您收到未知的驗證方法錯誤
您可能會遇到「不明的驗證方法」錯誤，指出當您從 Azure 重新導向時，AD FS 或 STS 層級不支援 AuthnCoNtext。 

當 Azure AD 使用強制執行驗證方法的參數重新導向至 AD FS 或 STS 時，這是最常見的情況。 

若要強制執行驗證方法，請使用下列其中一種方法：
- 針對 WS-同盟，請使用 WAUTH 查詢字串來強制執行慣用的驗證方法。

- 針對 SAML 2.0，請使用下列各項：
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  當使用不正確的值傳送強制的驗證方法，或 AD FS 或 STS 不支援該驗證方法時，您會在驗證之前收到錯誤訊息。

|需要驗證的方法|wauth URI|
|-----|-----|
|使用者名稱和密碼驗證|urn:oasis:names:tc:SAML:1.0:am:password|
|SSL 用戶端驗證|urn： ietf： rfc：2246|
|Windows 整合式驗證|urn：同盟：驗證： windows|

支援的 SAML 驗證內容類別

|驗證方法|驗證內容類別 URI|
|-----|-----| 
|使用者名稱和密碼|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|受密碼保護的傳輸|urn： oasis： names： tc： SAML：2.0： ac：類別： PasswordProtectedTransport|
|傳輸層安全性（TLS）用戶端|urn： oasis： names： tc： SAML：2.0： ac：類別： TLSClient
|X.509 憑證|urn： oasis： names： tc： SAML：2.0： ac：類別： X509
|整合式 Windows 驗證|urn：同盟：驗證： windows|
|Kerberos|urn： oasis： names： tc： SAML：2.0： ac：類別： Kerberos|

若要確保 AD FS 層級支援驗證方法，請檢查下列各項。

#### <a name="ad-fs-20"></a>AD FS 2.0 

在 [ **/adfs/ls/web.config**] 底下，確認 [驗證類型] 的專案存在。

```
<microsoft.identityServer.web>
<localAuthenticationTypes>
<add name="Forms" page="FormsSignIn.aspx" />
<add name="Integrated" page="auth/integrated/" />
<add name="TlsClient" page="auth/sslclient/" />
<add name="Basic" page="auth/basic/" />
</localAuthenticationTypes>
```

#### <a name="ad-fs-2012-r2"></a>AD FS 2012 R2

在 [ **AD FS 管理**] 底下，按一下 [AD FS] 嵌入式管理單元中的 [**驗證原則**]。

在 [**主要驗證**] 區段中，按一下 [全域設定] 旁的 [編輯]。 您也可以在 [驗證原則] 上按一下滑鼠右鍵，然後選取 [編輯全域主要驗證]。 或者，在 [動作] 窗格中，選取 [編輯全域主要驗證]。

在 [編輯全域驗證原則] 視窗的 [主要] 索引標籤上，您可以將設定設為全域驗證原則的一部分。 例如，在主要驗證中，您可以選取 [外部網路和內部網路] 底下可用的驗證方法。

* * 請確定已選取 [必要的驗證方法] 核取方塊。 

#### <a name="ad-fs-2016"></a>AD FS 2016

在 [ **AD FS 管理**] 底下，按一下 [AD FS] 嵌入式管理單元中的 [**服務**和**驗證方法**]。

在 [**主要驗證**] 區段中，按一下 [編輯]。

在 [**編輯驗證方法**] 視窗的 [主要] 索引標籤上，您可以將設定設為驗證原則的一部分。

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>AD FS 所簽發的權杖

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD 在權杖發行之後擲回錯誤
AD FS 發出權杖之後，Azure AD 可能會擲回錯誤。 在此情況下，請檢查下列問題：
- AD FS 在權杖中發出的宣告，應符合 Azure AD 中使用者的個別屬性。
- Azure AD 的權杖應該包含下列必要的宣告：
    - WSFED 
        - UPN：此宣告的值應該符合 Azure AD 中使用者的 UPN。
        - ImmutableID：此宣告的值應該符合使用者在 Azure AD 中的 sourceAnchor 或 ImmutableID。

若要取得 Azure AD 中的使用者屬性值，請執行下列命令列：`Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0：
       - IDPEmail：此宣告的值應該符合 Azure AD 中使用者的使用者主體名稱。
       - NAMEID：此宣告的值應該符合 Azure AD 中使用者的 sourceAnchor 或 ImmutableID。

如需詳細資訊，請參閱[使用 SAML 2.0 身分識別提供者來執行單一登入](/previous-versions/azure/azure-services/dn641269(v=azure.100))。

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>AD FS 和 Azure AD 之間的權杖簽署憑證不相符。

AD FS 使用權杖簽署憑證來簽署傳送給使用者或應用程式的權杖。 AD FS 和 Azure AD 之間的信任是以此權杖簽署憑證為基礎的同盟信任。

不過，如果 AD FS 端上的權杖簽署憑證因為自動憑證變換或部分介入而變更，則必須在同盟網域的 Azure AD 端更新新憑證的詳細資料。 當 AD FS 上的主要權杖簽署憑證與 Azure ADs 不同時，AD FS 所簽發的權杖不會受到 Azure AD 的信任。 因此，不允許同盟使用者登入。

若要修正此問題，您可以使用[更新 Office 365 和 Azure Active Directory 的同盟憑證](/azure/active-directory/connect/active-directory-aadconnect-o365-certs)中的步驟概述。

## <a name="other-common-things-to-check"></a>其他常見的檢查事項
以下是您在 AD FS 和 Azure AD 互動方面遇到問題時，要檢查的快速清單。
- Windows 認證管理員中的過時或快取認證
- 在適用于 Office 365 的信賴憑證者信任上設定的安全雜湊演算法已設為 SHA1

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)
