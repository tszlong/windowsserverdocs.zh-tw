---
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: "設定使用者憑證驗證 AD FS 支援"
description: 
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.service: active-directory
ms.technology: identity-adfs
ms.openlocfilehash: 9941d4dd997e857874aceddc920ec7f9d8944a81
ms.sourcegitcommit: d351cdbb0bf2533d6db35626ebbc4924b3834309
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2018
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>設定 AD FS 進行使用者憑證驗證

>適用於：Windows Server 2016、Windows Server 2012 R2

AD FS 可設定的 x509 使用其中一種模式使用者憑證驗證中所述的[本文](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 此功能可以使用[使用 Azure Active Directory](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/)或自己，以讓戶端和裝置提供給使用者的憑證存取 AD FS 資源內部網路或外部網路。

## <a name="prerequisites"></a>必要條件
- 確定您的使用者憑證的信任的所有 AD FS 和 WAP 伺服器
- 確保您使用者的認證信任的根憑證 NTAuth 市集中 Active Directory 中
- 如果使用 AD FS 替代憑證驗證模式，請確定您 AD FS 和 WAP 伺服器有 SSL 憑證包含 AD FS 主機加上「certauth「，例如「certauth.fs.contoso.com」，並透過防火牆允許此主機流量的
- 如果使用的外部憑證驗證，確保至少一個 AIA 和至少一個 CDP 或 OCSP 位置，指定您的憑證在清單中從網際網路存取。
- 如果您要設定 AD FS 進行 Azure AD 憑證驗證，請確定您已設定[設定 Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)和[AD FS 取得所需的規則](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)憑證發行者和的序號
- 也 Azure AD 憑證驗證 Exchange ActiveSync 用戶端，client 憑證必須使用者路由傳送電子郵件地址 Exchange online 主體名稱或主體替代名稱欄位 RFC822 名稱值。 （azure Active Directory 地圖服務 RFC822 值 directory 中的位址 Proxy 屬性。）

## <a name="configure-ad-fs-for-user-certificate-authentication"></a>設定 AD FS 進行使用者憑證驗證  
- 讓使用者憑證驗證為內部或中 AD FS 使用 AD FS 管理主控台或 PowerShell cmdlet Set-AdfsGlobalAuthenticationPolicy 外部網路的驗證方法
- 確定每個 AD FS 和 WAP 伺服器上安裝整個信任，包括任何中繼的憑證鏈結。 應該安裝中繼憑證在本機電腦中繼憑證授權單位網上商店的所有 AD FS 與 WAP 伺服器上。
- 如果您想要使用宣告根據憑證欄位和擴充功能，除了 EKU（宣告類型 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku），請在 Active Directory 宣告提供者信任設定通過規則的其他理賠要求。  查看下列的主張使用憑證的完整清單。  
- [選擇性]設定中的 [允許發行憑證憑證授權單位 client 使用指導方針在「信任的發行者 client 驗證管理」[本文](https://technet.microsoft.com/en-us/library/dn786429(v=ws.11).aspx)。

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>Windows 桌面 Chrome 瀏覽器設定順暢憑證驗證
多個使用者憑證（例如「Wi ‑ Fi 憑證）滿足 client 驗證之目的，在電腦上出現時，Windows 桌面 Chrome 瀏覽器將會提示使用者選取正確的憑證。 這可能會對使用者造成混淆。 若要最佳化此經驗，您可以設定為自動選取向憑證獲得更好的使用者體驗 Chrome 的原則。 這項原則可以手動設定變更登錄，或透過（若要設定登錄）GPO 自動設定。 這需要驗證，AD FS 有不同的發行者有時候使用從針對您的使用者 client 的憑證。 

如需有關 Chrome 此設定，請參考此[連結](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshooting"></a>疑難排解
- 如果憑證驗證要求失敗，並「不內容 https://certauth.fs.contoso.com 的「回應 HTTP 204，確認根和中繼 CA 憑證已安裝，分別，CA 和中繼 CA 受信任的根憑證存放區所有聯盟伺服器上。
- 如果憑證驗證要求會因不明原因而失敗，client 憑證匯出至.cer 檔案，並執行的命令 

`certutil -f -urlfetch -verify certificatefilename.cer`

請確定任何 CRL 和 delta CRL 位置解析。  請注意，找不到 delta CRL 位置為基礎的基底 CRL 到。

## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>參考：使用者憑證的完整清單取得類型與範例值

|宣告類型|範例值。
|-----|-----
|https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version | 3
|https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm | sha256RSA
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer | DATA-CN = entca，DC = 網域俠 = contoso 俠 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername | DATA-CN = entca，DC = 網域俠 = contoso 俠 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore | 12/05/2016 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter | 12/05/2017 20:50:18
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subject | E =user@contoso.com，DATA-CN = 使用者 DATA-CN = DC 的使用者，= 網域俠 = contoso 俠 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname | E =user@contoso.com，DATA-CN = 使用者 DATA-CN = DC 的使用者，= 網域俠 = contoso 俠 = com
|https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata | {Base64 編碼數位憑證資料}
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | DigitalSignature
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage | KeyEncipherment
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier | 9D11941EC06FACCCCB1B116B56AA97F3987D620A
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier | 金鑰識別碼為 = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename | 使用者
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/san | 其他名稱：主體名稱 =user@contoso.com、RFC822 名稱 =user@contoso.com
|https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku | 1.3.6.1.4.1.311.10.3.4


