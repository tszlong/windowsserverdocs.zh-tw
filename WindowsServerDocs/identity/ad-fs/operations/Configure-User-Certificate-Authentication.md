---
description: 深入瞭解：設定使用者憑證驗證 AD FS
ms.assetid: 1ea2e1be-874f-4df3-bc9a-eb215002da91
title: 設定使用者憑證驗證 AD FS 支援
author: jenfieldmsft
ms.author: billmath
manager: samueld
ms.date: 01/18/2018
ms.topic: article
ms.openlocfilehash: 02d1d3e6e0b1d8aac739f59c2a6ef60ce8a8088a
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042266"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>設定使用者憑證驗證 AD FS

使用者憑證驗證主要用於2個使用案例
* 使用者使用智慧卡來登入其 AD FS 系統
* 使用者使用布建至行動裝置的憑證


## <a name="prerequisites"></a>必要條件
1) 使用[本文所](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)述的其中一種模式，判斷您想要啟用的 AD FS 使用者憑證驗證模式
2) 確定您的使用者憑證信任鏈已安裝 & 由所有 AD FS 和 WAP 伺服器（包括任何中繼憑證授權單位單位）所信任。 這通常是透過 AD FS/WAP 伺服器上的 GPO 來完成
3)  確定您的使用者憑證的信任鏈根憑證位於 Active Directory 的 NTAuth 存放區中。
4) 如果在替代憑證驗證模式中使用 AD FS，請確定您的 AD FS 和 WAP 伺服器的 SSL 憑證包含前面加上 "certauth" 的 AD FS 主機名稱（例如 "certauth.fs.contoso.com"），且允許此主機名稱的流量通過防火牆
5) 如果使用來自外部網路的憑證驗證，請確定您的憑證中指定的清單至少有一個 AIA 和至少一個 CDP 或 OCSP 位置可從網際網路存取。
6) 此外，針對 Exchange ActiveSync 用戶端的 Azure AD 憑證驗證，用戶端憑證必須在 [主體別名] 欄位的 [主體名稱] 或 [RFC822 名稱] 值中，讓使用者在 Exchange online 中有可路由傳送的電子郵件地址。  (Azure Active Directory 會將 RFC822 值對應至目錄中的 Proxy 位址屬性。 ) 
7) AD FS 不支援使用以智慧卡/憑證為基礎的驗證的使用者名稱提示。


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>設定 AD FS 用於使用者憑證驗證

使用 AD FS 管理主控台或 PowerShell Cmdlet，在 AD FS 中啟用使用者憑證驗證做為內部網路或外部網路驗證方法 `Set-AdfsGlobalAuthenticationPolicy` 。

如果您要設定 Azure AD 憑證驗證 AD FS，請確定您已設定[Azure AD 設定](/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)和憑證簽發者和序號[所需的 AD FS 宣告規則](/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)

此外，還有一些選擇性的層面。
- 如果您想要根據憑證欄位和延伸模組來使用宣告 (宣告類型 https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku) ，請在 Active Directory 宣告提供者信任上，設定其他宣告傳遞規則。  如需可用憑證宣告的完整清單，請參閱下文。
- 如果您需要根據憑證的類型來限制存取權，您可以在應用程式的 AD FS 發行授權規則中，使用憑證上的其他屬性。 常見的案例是「只允許由 MDM 提供者布建的憑證」或「僅允許智慧卡憑證」
>[!IMPORTANT]
> 使用裝置程式碼流程進行驗證以及使用 Azure AD 以外的 IDP 來執行裝置驗證的客戶 (例如 AD FS) 將無法強制執行裝置型存取 (例如，只允許使用協力廠商 MDM 服務) Azure AD 資源的受控裝置。 若要在 Azure AD 中保護公司資源的存取權，並防止任何資料洩漏，客戶應設定 Azure AD 裝置型條件式存取 (例如「要求裝置必須標記為投訴」授與 Azure AD 條件式存取) 中的控制權。
- 在 [本文中，](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn786429(v=ws.11))使用「用戶端驗證的受信任簽發者管理」底下的指導方針，為用戶端憑證設定允許的頒發證書授權單位。
- 您可能想要在執行憑證驗證時，考慮修改登入頁面，以符合使用者的需求。 常見的案例是 () 變更「使用您的 X509 憑證登入」，讓使用者更容易瞭解

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>在 Windows 桌上型電腦上設定 Chrome 瀏覽器的無縫憑證驗證
當符合用戶端驗證目的的電腦上有多個使用者憑證 (例如 Wi-Fi 憑證) 時，Windows 桌面上的 Chrome 瀏覽器會提示使用者選取正確的憑證。 這可能會對終端使用者造成混淆。 若要優化此體驗，您可以設定 Chrome 的原則以自動選取正確的憑證，以提供更好的使用者體驗。 您可以手動設定此原則，方法是進行登錄變更或透過 GPO 自動設定 (設定) 的登錄機碼。 這需要您的使用者用戶端憑證來進行驗證，以使 AD FS 與其他使用案例具有不同的簽發者。

如需針對 Chrome 設定此設定的詳細資訊，請參閱此 [連結](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。


## <a name="troubleshoot-certificate-authentication"></a>針對憑證驗證進行疑難排解
本檔著重于當 AD FS 設定給使用者的憑證驗證時，遇到疑難排解常見問題的問題。

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>檢查是否已在所有 AD FS/WAP 伺服器中正確設定憑證信任簽發者
*常見的徵兆： HTTP 204 「沒有 HTTPs \: //certauth.adfs.contoso.com 的內容」*

AD FS 使用基礎 windows 作業系統來證明擁有使用者憑證，並藉由執行憑證信任鏈驗證，確保它符合受信任的簽發者。 為了符合受信任的簽發者，您必須確保所有根和中繼授權單位都設定為 [本機電腦憑證授權單位單位] 存放區中的受信任簽發者。
若要自動進行驗證，請使用 [AD FS 診斷分析器工具](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze)。 此工具會查詢所有伺服器，並確保正確布建正確的憑證。
1)  依據上述連結中提供的指示，下載並執行此工具
2)  上傳結果並查看是否有任何失敗

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>檢查 AD FS 驗證原則中是否已啟用憑證驗證
AD FS 預設不會啟用憑證驗證。 請參閱本檔的開頭，以瞭解如何啟用憑證驗證。

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>檢查 AD FS 驗證原則中是否已啟用憑證驗證
AD FS 預設會在通訊埠49443上使用相同的主機名稱作為 AD FS (例如) ）進行使用者憑證驗證 `adfs.contoso.com` 。 您也可以使用替代 SSL 系結，將 AD FS 設定為使用埠 443 (預設 HTTPS 埠) 。 不過，此設定中使用的 URL `certauth.<adfs-farm-name>` (例如 `certauth.contoso.com`) 。 如需詳細資訊，請參閱 [此連結](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md) 。
最常見的網路連線情況是防火牆的設定不正確，且封鎖或干擾使用者憑證驗證流量。 通常，您會在發生此問題時看到空白畫面或500伺服器錯誤。
1)  請注意您在 AD FS 中設定的主機名稱和埠
2)  確認 AD FS 或 Web Application Proxy 之前的任何防火牆 (WAP) 已設定為允許 `hostname:port` 您的 AD FS 伺服器陣列組合。 您必須參閱您的網路工程師來執行此步驟。

### <a name="check-certificate-revocation-list-connectivity"></a>檢查憑證撤銷清單連線能力
憑證撤銷清單 (CRL) 是編碼為使用者憑證以執行執行時間撤銷檢查的端點。 例如，如果包含憑證的裝置遭竊，系統管理員可以將憑證新增到 [已撤銷的憑證] 清單中。 先前接受此憑證的任何端點現在都會使驗證失敗。

每個 AD FS 和 WAP 伺服器都必須連線到 CRL 端點，以驗證出示給它的憑證是否仍然有效且尚未撤銷。 CRL 驗證可以透過 HTTPS、HTTP、LDAP 或 OCSP (線上憑證狀態通訊協定) 來進行。 如果 AD FS/WAP 伺服器無法連線到端點，驗證將會失敗。 請依照下列步驟進行疑難排解。
1) 洽詢您的 PKI 工程師，以判斷用來撤銷 PKI 系統中使用者憑證的 CRL 端點。
2)  在每個 AD FS/WAP 伺服器上，請確定可透過 (使用的通訊協定（通常是 HTTPS 或 HTTP）來連線 CRL 端點) 
3)  若要進行 advanced 驗證，請在每個 AD FS/WAP 伺服器上[啟用 CAPI2 事件記錄](/archive/blogs/benjaminperkins/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues)
4) 檢查事件識別碼 41 (確認 CAPI2 操作記錄中的撤銷) 
5) 檢查是否有 `‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***秘訣** _：您可以設定單一 AD FS 或 WAP 伺服器的目標，以便更輕鬆地進行疑難排解，方法是在 Windows) 上設定 DNS 解析 (HOSTS 檔案來指向特定的伺服器。 這可讓您啟用以伺服器為目標的追蹤。

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>檢查這是否為伺服器名稱指示的 (SNI) 問題
AD FS 需要用戶端裝置 (或瀏覽器) ，以及支援 SNI 的負載平衡器。 某些用戶端裝置 (通常較舊版本的 Android) 可能不支援 SNI。 此外，負載平衡器可能不支援 SNI 或尚未針對 SNI 進行設定。 在這些情況中，您可能會看到使用者認證失敗。
1)  與您的網路工程師合作，以確保 AD FS/WAP 的 Load Balancer 支援 SNI
2)  如果無法支援 SNI AD FS 可透過下列步驟來解決問題：在主要 AD FS 伺服器上開啟提高許可權的命令提示字元視窗
    *   輸入 ```Netsh http show sslcert```
    *   複製 federation service 的「應用程式 GUID」和「憑證雜湊」
    *   輸入 `netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>檢查是否已正確布建憑證的用戶端裝置
您可能會注意到某些裝置運作正常，但其他裝置則否。 在此情況下，通常是因為用戶端裝置上未正確布建使用者憑證的結果。 請依照下面的步驟執行。
1)  如果問題是針對 Android 裝置所特有，最常見的問題是在 Android 裝置上的憑證鏈並非完全受信任。  請參閱您的 MDM 廠商，以確定憑證已正確布建，且整個鏈在 Android 裝置上完全受信任。
2)  如果問題是 Windows 裝置特有的，請檢查登入使用者的 Windows 憑證存放區是否已正確布建， (不是系統/電腦) 。
3)  將用戶端使用者憑證匯出為 .cer 檔案，然後執行 ' certutil-f-urlfetch verify-verify certificatefilename .cer ' 命令


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>檢查 AD FS/WAP 伺服器與用戶端裝置之間的 TLS 版本是否相容
在罕見的情況下，用戶端裝置 (通常會更新行動裝置) ，只支援較高版本的 TLS (例如 1.3) 或者，您可能會遇到 AD FS/WAP 伺服器更新為只使用較高 TLS 版本，且用戶端裝置不支援的反向問題。
您可以使用線上 SSL 工具來檢查您的 AD FS/WAP 伺服器，並查看它是否與裝置相容。
如需如何控制 TLS 版本的詳細資訊，請參閱 [此連結](manage-ssl-protocols-in-ad-fs.md)。

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>檢查是否已在您的同盟網域設定上正確設定 Azure AD PromptLoginBehavior
許多 Office 365 應用程式傳送提示 = 登入 Azure AD。 Azure AD 預設會將它轉換成新的密碼登入，以 AD FS。 因此，即使您已在 AD FS 中設定憑證驗證，您的使用者還是只會看到密碼登入。
1)  使用 ' Get-msoldomainfederationsettings ' 命令來取得同盟網域設定
2)  確定 PromptLoginBehavior 參數設定為 ' Disabled ' 或 ' NativeSupport ' 其中之一

如需詳細資訊，請參閱 [此連結](ad-fs-prompt-login.md)。

### <a name="additional-troubleshooting"></a>其他疑難排解
這些很少出現
1)  如果您的 CRL 清單很長，則可能會在嘗試下載時遇到超時時間。 在此情況下，您需要更新 ' MaxFieldLength ' 和 ' MaxRequestByte ' （依據 https://support.microsoft.com/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>參考：使用者憑證宣告類型的完整清單和範例值

|                                         宣告類型                                         |                              範例值                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = entca，DC = domain，DC = contoso，DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = entca，DC = domain，DC = contoso，DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E = user@contoso.com ，cn = user，cn = Users，dc = domain，DC = contoso，dc = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E = user@contoso.com ，cn = user，cn = Users，dc = domain，DC = contoso，dc = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Base64 編碼的數位憑證資料}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   User                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | 其他名稱： Principal Name = user@contoso.com ，RFC822 Name =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>相關連結
* [針對 AD FS 憑證驗證設定替代主機名稱系結](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [在 Azure AD 中設定憑證授權單位單位](/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
