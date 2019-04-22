---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: 設定使用者憑證驗證的 AD FS 支援
description: ''
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5c2d84c263517a4c81622ca02538796ccd9da71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817499"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>設定使用者憑證驗證的 AD FS

>適用於：Windows Server 2016, Windows Server 2012 R2

AD FS 可設定為 x509 使用者憑證驗證，使用其中一個模式中所述[這篇文章](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 這項功能可以使用[與 Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)或本身來啟用用戶端和裝置佈建與使用者憑證，以存取 AD FS 內部網路或外部網路的資源。

## <a name="prerequisites"></a>先決條件
- 請確定您的使用者憑證所信任的所有 AD FS 和 WAP 伺服器
- 請確定您的使用者憑證的信任鏈結的根憑證位於 NTAuth 存放區，在 Active Directory 中
- 如果在其他憑證驗證模式中使用 AD FS，確保您的 AD FS 和 WAP 伺服器擁有包含加上"certauth"，例如"certauth.fs.contoso.com，「 將 AD FS 主機名稱的 SSL 憑證，並允許該流量到此主機名稱透過防火牆
- 如果使用從外部網路的憑證驗證，請確定至少一個 AIA 和至少一個 CDP 或 OCSP 的位置，從您的憑證中指定的清單是從網際網路存取。
- 如果您要設定 Azure AD 憑證驗證的 AD FS，請確定您已設定[Azure AD 設定](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)並[AD FS 宣告規則所需](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)憑證簽發者和序號
- 也針對 Azure AD 憑證驗證，Exchange ActiveSync 用戶端，用戶端憑證必須使用者路由傳送的電子郵件地址在 Exchange online 中為主體名稱或主體別名 欄位的 RFC822 名稱 值。 （azure Active Directory 會對應 RFC822 值到目錄中的 Proxy 位址 屬性）。

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>設定 AD FS 用於使用者憑證驗證  
- 啟用內部網路或外部網路驗證方法，在 AD FS 中，使用 AD FS 管理主控台或 PowerShell cmdlet 設定 AdfsGlobalAuthenticationPolicy 使用者憑證驗證
- 請確定整個鏈結的信任，包括任何中繼憑證，請確認已安裝在每部 AD FS 和 WAP 伺服器上。 中繼憑證應該安裝在所有的 AD FS 和 WAP 伺服器上的本機電腦中繼憑證授權單位存放區。
- 如果您想要使用憑證的欄位和除了 EKU 延伸模組為基礎的宣告 (宣告類型 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku)，Active Directory 宣告提供者信任上設定其他宣告規則通過。  如需可用的憑證宣告的完整清單，請參閱下方內容。  
- [選用]在中設定允許發行的憑證授權單位，使用 [管理的用戶端驗證的受信任簽發者] 下的指導方針的用戶端憑證[這篇文章](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)。

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>針對 Windows 桌上型電腦上的 Chrome 瀏覽器設定無縫式的憑證驗證
存在於滿足用戶端驗證目的電腦上 （例如 Wi-fi 憑證） 的多個使用者憑證時，在 Windows 桌面上的 Chrome 瀏覽器會提示使用者選取正確的憑證。 這可能會造成使用者混淆。 若要最佳化這項體驗，您可以設定自動選取正確的憑證，以便改善使用者經驗的 Chrome 的原則。 此原則可以手動設定進行登錄變更，或透過 GPO （若要設定的登錄機碼） 自動設定。 這需要您使用者的用戶端憑證，對具有不同的簽發者與其他使用案例的 AD FS 進行驗證。 

如需有關設定此 chrome 的詳細資訊，請參閱此[連結](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshooting"></a>疑難排解
- 如果憑證的驗證要求失敗與 HTTP 204 「 無內容檔案從 https://certauth.fs.contoso.com」 回應，確認根和中繼 CA 憑證已安裝，分別以受信任的根 CA 和憑證存放區上所有的中繼 CA同盟伺服器。
- 如果因不明原因而失敗的驗證憑證要求，將用戶端憑證匯出到.cer 檔案，然後執行命令 

`certutil -f -urlfetch -verify certificatefilename.cer`

請確定任何 CRL 和 delta CRL 位置解析。  請注意，delta CRL 的位置會根據基底 CRL 的內容。

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>參考：使用者憑證的完整清單宣告類型和範例值

|宣告類型|範例值
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | CN=entca, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | CN=entca, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E=user@contoso.com, CN=user, CN=Users, DC=domain, DC=contoso, DC=com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Base64 編碼的數位認證資料}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | KeyID = 13 版 e3 6b d6 bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | 使用者
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | 其他主體名稱： 名稱 =user@contoso.com，RFC822 名稱 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


