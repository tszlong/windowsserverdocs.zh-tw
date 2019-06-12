---
title: AD FS 疑難排解-Azure AD
description: 本文件說明如何疑難排解 AD FS 與 Azure AD 的各種層面
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 03/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 228ef34ab25276c1cf98f9b2b64e997390023c87
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444017"
---
# <a name="ad-fs-troubleshooting---azure-ad"></a>AD FS 疑難排解-Azure AD
使用雲端的成長，很多公司都有已移至其各種應用程式和服務使用 Azure AD。  與 Azure AD 同盟，已成為許多組織的標準做法。  這份文件將涵蓋一些疑難排解問題所引發的這個同盟的層面。  有幾個一般的疑難排解文件中的主題還是屬於與 Azure 同盟，因此本文將著重於只是與 Azure AD 的詳細資訊以及 AD FS 之間的互動。

## <a name="redirection-to-ad-fs"></a>重新導向至 AD FS
重新導向發生於您登入 Office 365 之類的應用程式並將您 「 重新導向 」 您的組織登入的 AD FS 伺服器。

![](media/ad-fs-tshoot-azure/azure1.png)


### <a name="first-things-to-check"></a>若要檢查的第一個項目
如果重新導向不發生幾件事您想要檢查

   1. 請確定您的 Azure AD 租用戶，會啟用同盟登入 Azure 入口網站，並檢查在 Azure AD Connect。

![](media/ad-fs-tshoot-azure/azure2.png)

1. 請務必按一下旁邊同盟網域，在 Azure 入口網站中驗證您的自訂網域。
   ![](media/ad-fs-tshoot-azure/azure3.png)

2. 最後，您想要檢查[DNS](ad-fs-tshoot-dns.md) ，並確定您的 AD FS 伺服器或 WAP 伺服器會從網際網路解析。  確認這個方法可以解決，且您就能夠瀏覽至它。
3. 您也可以使用 PowerShell cmdlt`Get-AzureADDomain`也取得這項資訊。

![](media/ad-fs-tshoot-azure/azure6.png)

### <a name="you-are-receiving-an-unknown-auth-method-error"></a>您收到未知的驗證方法的錯誤
您可能會遇到 「 未知的驗證方法 」 錯誤，指出，AuthnContext 時，不支援在 AD FS 或 STS 的層級您從 Azure 重新導向。 

當 Azure AD 使用重新導向至 AD FS 或 STS 會強制執行驗證方法的參數時，這是最常見的。 

若要強制執行驗證方法，請使用下列方法之一：
- 適用於 WS-同盟，使用的 WAUTH 查詢字串，強制慣用的驗證方法。

- 針對 SAML2.0，使用下列方法：
  ```
  <saml:AuthnContext>
  <saml:AuthnContextClassRef>
  urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport
  </saml:AuthnContextClassRef>
  </saml:AuthnContext>
  ```
  當強制執行的驗證方法會傳送不正確的值，或對 AD FS 或 STS 不支援該驗證方法，您會收到錯誤訊息之前會驗證您。

|想要的驗證方法|wauth URI|
|-----|-----|
|使用者名稱和密碼驗證|urn:oasis:names:tc:SAML:1.0:am:password|
|SSL 用戶端驗證|urn:ietf:rfc:2246|
|Windows 整合式驗證|urn:federation:authentication:windows|

支援的 SAML 驗證內容類別

|驗證方法|驗證內容類別的 URI|
|-----|-----| 
|使用者名稱和密碼|urn:oasis:names:tc:SAML:2.0:ac:classes:Password|
|受密碼保護傳輸|urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport|
|傳輸層安全性 (TLS) 用戶端|urn:oasis:names:tc:SAML:2.0:ac:classes:TLSClient
|X.509 憑證|urn:oasis:names:tc:SAML:2.0:ac:classes:X509
|整合式的 Windows 驗證|urn:federation:authentication:windows|
|Kerberos|urn:oasis:names:tc:SAML:2.0:ac:classes:Kerberos|

若要確定在 AD FS 層級支援的驗證方法，請檢查下列項目。

#### <a name="ad-fs-20"></a>AD FS 2.0 

底下 **/adfs/ls/web.config**，請確定驗證類型的項目存在。

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

底下**AD FS 管理**，按一下**驗證原則**在 AD FS 嵌入式管理單元。

在 **主要驗證**區段中，按一下 全域設定旁邊的編輯。 您可以也在驗證原則上按一下滑鼠右鍵，然後選取 編輯全域主要驗證。 或者，您也可以在 動作 窗格中，選取 編輯全域主要驗證。

在 [編輯全域驗證原則] 視窗中，[主要] 索引標籤中，您可以設定為全域驗證原則的一部分。 比方說，對主要驗證，您可以選取可用的驗證方法，在外部網路和內部網路。

* * 請確定已選取 [必要的驗證方法] 核取方塊。 

#### <a name="ad-fs-2016"></a>AD FS 2016

底下**AD FS 管理**，按一下**服務**並**驗證方法**在 AD FS 嵌入式管理單元。

在 **主要驗證**區段中，按一下 編輯。

在 **編輯驗證方法** 視窗中，主要 索引標籤中，您可以設定為 驗證原則的一部分。

![](media/ad-fs-tshoot-azure/azure4.png)

## <a name="tokens-issued-by-ad-fs"></a>AD FS 所發出的權杖

### <a name="azure-ad-throws-error-after-token-issuance"></a>Azure AD 權杖發行之後擲回錯誤
AD FS 簽發的權杖之後，Azure AD 可能會擲回錯誤。 在此情況下，請檢查下列問題：
- 在權杖中的 AD FS 所發出的宣告應該符合 Azure AD 中的兩個屬性的使用者。
- Azure AD 的權杖應該包含下列必要的宣告：
    - WSFED: 
        - UPN:此宣告的值應該符合使用者的 UPN，Azure AD 中。
        - ImmutableID:此宣告的值應該符合使用者的 ImmutableID 的 sourceAnchor，Azure AD 中。

若要取得 Azure AD 中的使用者屬性值，請執行下列命令列： `Get-AzureADUser –UserPrincipalName <UPN>`

![](media/ad-fs-tshoot-azure/azure5.png)

   - SAML 2.0:
       - IDPEmail:此宣告的值應該符合 Azure AD 中的使用者的使用者主體名稱。
       - NAMEID:此宣告的值應該符合使用者的 ImmutableID 的 sourceAnchor，Azure AD 中。

如需詳細資訊，請參閱 <<c0> [ 使用 SAML 2.0 身分識別提供者實作單一登入](https://technet.microsoft.com/library/dn641269.aspx)。

### <a name="token-signing-certificate-mismatch-between-ad-fs-and-azure-ad"></a>權杖簽署 AD FS 與 Azure AD 之間的憑證不相符。

AD FS 使用的權杖簽署憑證來簽署權杖傳送至使用者或應用程式。 AD FS 與 Azure AD 之間的信任是具有此權杖簽署憑證為基礎的同盟的信任。

不過，如果 AD FS 端上的 權杖簽署憑證已變更，因為自動憑證變換或某種程度的介入，新的憑證詳細資料必須更新 Azure AD 端的同盟網域。 不同於 Azure Ad 的 AD FS 上的主要權杖簽署憑證時，AD FS 所發出的權杖不會信任由 Azure AD。 因此，同盟的使用者不允許登入。

若要修正您可以使用此步驟會概述在[更新 Office 365 和 Azure Active Directory 的同盟憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)。

## <a name="other-common-things-to-check"></a>若要檢查其他常見的項目
以下是幾個簡要的檢查是否您有 AD FS 與 Azure AD 互動問題的項目。
- 過時或快取的認證在 「 Windows 認證管理員
- 適用於 Office 365 信賴憑證者信任設定的安全雜湊演算法是設為 SHA1

## <a name="next-steps"></a>後續步驟

- [AD FS 疑難排解](ad-fs-tshoot-overview.md)