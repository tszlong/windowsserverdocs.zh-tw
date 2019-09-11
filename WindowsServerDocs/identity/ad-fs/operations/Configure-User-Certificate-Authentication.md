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
ms.openlocfilehash: 1616a1fe2e28534cc30c8955b0309c233555fa14
ms.sourcegitcommit: ee8e0b217be6f6b2532ee7265fb4be00c106e124
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70878149"
---
# <a name="configuring-ad-fs-for-user-certificate-authentication"></a>設定使用者憑證驗證的 AD FS

使用者憑證驗證主要用於2個使用案例
* 使用者使用智慧卡登入其 AD FS 系統
* 使用者使用布建到行動裝置的憑證


## <a name="prerequisites"></a>必要條件
1) 使用[本文所](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)述的其中一種模式，判斷您想要啟用之 AD FS 使用者憑證驗證的模式
2) 請確定您的使用者憑證信任鏈已安裝 & 由所有 AD FS 和 WAP 伺服器信任，包括任何中繼憑證授權單位單位。 這通常是透過 AD FS/WAP 伺服器上的 GPO 來完成的
3)  請確定您的使用者憑證信任鏈的根憑證位於 NTAuth 存放區中的 Active Directory
4) 如果在替代憑證驗證模式中使用 AD FS，請確定您的 AD FS 和 WAP 伺服器都具有 SSL 憑證，其中包含前面加上 "certauth" （例如 "certauth.fs.contoso.com"）的 AD FS 主機名稱，且允許此主機名稱的流量透過防火牆
5) 如果使用外部網路的憑證驗證，請確定您在憑證中指定的清單中，至少有一個 AIA 和至少一個 CDP 或 OCSP 位置可從網際網路存取。
6) 此外，對於 Exchange ActiveSync 用戶端的 Azure AD 憑證驗證，用戶端憑證必須在 [主體別名] 欄位的 [主體名稱] 或 [RFC822 名稱] 值中，讓使用者可路由傳送的電子郵件地址在 Exchange online 中。 （Azure Active Directory 會將 RFC822 值對應到目錄中的 Proxy 位址屬性）。


## <a name="configure-ad-fs-for-user-certificate-authentication"></a>設定 AD FS 用於使用者憑證驗證  

使用 AD FS 管理主控台或 PowerShell Cmdlet `Set-AdfsGlobalAuthenticationPolicy`，在 AD FS 中啟用使用者憑證驗證做為內部網路或外部網路驗證方法。

如果您要設定 Azure AD 憑證驗證的 AD FS，請確定您已進行[Azure AD 設定](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)和憑證簽發者和序號[所需的 AD FS 宣告規則](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-ios#requirements)

此外，還有一些選擇性層面。
- 如果您想要根據憑證欄位和擴充功能來使用宣告（ https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku) 除了 EKU 以外），請在 Active Directory 宣告提供者信任上設定額外的宣告傳遞規則。  如需可用憑證宣告的完整清單，請參閱下文。  
- 如果您需要根據憑證類型來限制存取，您可以在應用程式的 AD FS 發行授權規則中，使用憑證上的其他屬性。 常見的案例是「只允許 MDM 提供者所布建的憑證」或「只允許智慧卡憑證」
- 使用[本文中「](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)管理用戶端驗證的受信任簽發者」下的指引，設定允許的用戶端憑證頒發證書授權單位。
- 在執行憑證驗證時，您可能會想要考慮修改登入頁面，以符合使用者的需求。 常見的情況是（a）將「使用您的 X509 憑證登入」變更為更容易使用的使用者

## <a name="configure-seamless-certificate-authentication-for-chrome-browser-on-windows-desktops"></a>在 Windows 桌上型電腦上設定 Chrome 瀏覽器的無縫憑證驗證
當電腦上有多個使用者憑證（例如 Wi-fi 憑證）符合用戶端驗證的目的時，Windows 桌面上的 Chrome 瀏覽器會提示使用者選取正確的憑證。 這對終端使用者可能會造成混淆。 若要將這種體驗優化，您可以設定 Chrome 的原則，以自動選取正確的憑證，以獲得更好的使用者體驗。 這項原則可以透過進行登錄變更或透過 GPO 自動設定（以設定登錄機碼）的方式來設定。 這需要您的使用者用戶端憑證來驗證 AD FS，使其與其他使用案例有不同的簽發者。 

如需有關為 Chrome 設定此配置的詳細資訊，請參閱此[連結](http://www.chromium.org/administrators/policy-list-3#AutoSelectCertificateForUrls)。  


## <a name="troubleshoot-certificate-authentication"></a>針對憑證驗證進行疑難排解
本檔的重點在於，當 AD FS 設定為使用者的憑證驗證時，遇到常見問題的問題。 

### <a name="check-if-certificate-trusted-issuers-is-configured-properly-in-all-the-ad-fswap-servers"></a>檢查是否已在所有 AD FS/WAP 伺服器中正確設定憑證信任簽發者
*常見的徵兆：HTTP 204 「沒有來自 HTTPs\://certuath.adfs.contoso.com 的內容」*

AD FS 使用基礎 windows 作業系統來證明擁有使用者憑證，並藉由執行憑證信任鏈驗證，確保它符合受信任的簽發者。 若要比對受信任的簽發者，您必須確定所有的根和中繼授權單位都已設定為 [本機電腦憑證授權單位單位] 存放區中的 [信任的發行者]。 若要自動驗證此情況，請使用[AD FS 診斷分析器工具](https://adfshelp.microsoft.com/DiagnosticsAnalyzer/Analyze)。 此工具會查詢所有伺服器，並確保正確布建正確的憑證。 
1)  依據上述連結中提供的指示，下載並執行工具
2)  上傳結果，並查看是否有任何失敗

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>檢查 AD FS 驗證原則中是否已啟用憑證驗證
AD FS 預設不會啟用憑證驗證。 如需如何啟用憑證驗證的資訊，請參閱本檔的開頭。 

### <a name="check-if-certificate-authentication-is-enabled-in-the-ad-fs-authentication-policy"></a>檢查 AD FS 驗證原則中是否已啟用憑證驗證
AD FS 預設會在通訊埠49443上，使用與 AD FS 相同的主機名稱（例如`adfs.contoso.com`）來驗證使用者憑證。 您也可以使用替代 SSL 系結，將 AD FS 設定為使用埠443（預設的 HTTPS 埠）。 不過，此設定中使用的 URL 為`certauth.<adfs-farm-name>` （ `certauth.contoso.com`例如）。 如需詳細資訊，請參閱[此連結](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)。 網路連線最常見的情況是防火牆設定不正確，而且會封鎖或干擾使用者憑證驗證流量。 通常當此問題發生時，您會看到空白畫面或500伺服器錯誤。 
1)  請注意您在中設定的主機名稱和埠 AD FS
2)  確定 AD FS 或 Web 應用程式 Proxy （WAP）前方的任何防火牆都已設定為允許您`hostname:port`的 AD FS 伺服器陣列組合。 您必須參考您的網路工程師來執行此步驟。 

### <a name="check-certificate-revocation-list-connectivity"></a>檢查憑證撤銷清單連線能力
憑證撤銷清單（CRL）是編碼成使用者憑證以執行執行時間撤銷檢查的端點。 例如，如果包含憑證的裝置遭竊，系統管理員可以將憑證新增至 [已撤銷的憑證] 清單。 先前接受此憑證的任何端點現在都會使驗證失敗。

每個 AD FS 和 WAP 伺服器都必須連線到 CRL 端點，以驗證出示給它的憑證是否仍然有效，而且尚未被撤銷。 CRL 驗證可透過 HTTPS、HTTP、LDAP 或 OCSP （線上憑證狀態通訊協定）進行。 如果 AD FS/WAP 伺服器無法連接到端點，驗證將會失敗。 請遵循下列步驟來進行疑難排解。 
1) 請洽詢您的 PKI 工程師，以判斷用來從 PKI 系統撤銷使用者憑證的 CRL 端點。 
2)  在每個 AD FS/WAP 伺服器上，確保 CRL 端點可透過所使用的通訊協定（通常是 HTTPS 或 HTTP）來連線
3)  若要進行 advanced 驗證，請在每個 AD FS/WAP 伺服器上[啟用 CAPI2 事件記錄](https://blogs.msdn.microsoft.com/benjaminperkins/2013/09/30/enable-capi2-event-logging-to-troubleshoot-pki-and-ssl-certificate-issues/)
4) 檢查 CAPI2 操作記錄中的事件識別碼41（驗證撤銷）
5) 檢查`‘\<Result value="80092013"\>The revocation function was unable to check revocation because the revocation server was offline.\</Result\>'`

***提示***：您可以將單一 AD FS 或 WAP 伺服器設為目標，藉由設定 DNS 解析（Windows 上的主機檔案）指向特定伺服器，更容易進行疑難排解。 這可讓您啟用以伺服器為目標的追蹤。 

### <a name="check-if-this-is-a-server-name-indication-sni-issue"></a>檢查這是否為伺服器名稱指示（SNI）問題
AD FS 需要用戶端裝置（或瀏覽器）和負載平衡器支援 SNI。 某些用戶端裝置（通常是較舊版本的 Android）可能不支援 SNI。 此外，負載平衡器可能不支援 SNI 或尚未針對 SNI 進行設定。 在這些情況中，您可能會看到使用者認證失敗。 
1)  與您的網路工程師合作，以確保 AD FS/WAP 的 Load Balancer 支援 SNI
2)  在無法支援 SNI 的事件中 AD FS 會遵循下列步驟來解決工作
    *   在主要 AD FS 伺服器上開啟提升許可權的命令提示字元視窗
    *   輸入```Netsh http show sslcert```
    *   複製 federation service 的「應用程式 GUID」和「憑證雜湊」
    *   輸入`netsh http add sslcert ipport=0.0.0.0:{your_certauth_port} certhash={your_certhash} appid={your_applicaitonGUID}`

### <a name="check-if-the-client-device-has-been-provisioned-with-the-certificate-correctly"></a>檢查是否已正確布建憑證給用戶端裝置
您可能會注意到有些裝置正常運作，但其他裝置則否。 在此情況下，通常是因為未在用戶端裝置上正確布建使用者憑證的結果。 請遵循下列步驟。 
1)  如果問題是 Android 裝置特有的，最常見的問題是憑證鏈在 Android 裝置上並未完全受信任。  請參閱您的 MDM 廠商，以確保憑證已正確布建，而且整個鏈在 Android 裝置上完全受信任。 
2)  如果問題專屬於 Windows 裝置，請檢查已登入使用者（非系統/電腦）的 Windows 憑證存放區，以檢查憑證是否已正確布建。
3)  將用戶端使用者憑證匯出為 .cer 檔案，並執行命令 ' certutil-f-urlfetch verify-verify certificatefilename .cer '


### <a name="check-if-the-tls-version-is-compatible-between-ad-fswap-servers-and-the-client-device"></a>檢查 AD FS/WAP 伺服器和用戶端裝置之間的 TLS 版本是否相容
在罕見的情況下，用戶端裝置（通常是行動裝置）只會更新為支援較高版本的 TLS （例如1.3），或者您可能會有反向問題，其中 AD FS/WAP 伺服器已更新為只使用較高的 TLS 版本，而且用戶端裝置不支援它。 您可以使用線上 SSL 工具來檢查您的 AD FS/WAP 伺服器，並查看其是否與裝置相容。 如需如何控制 TLS 版本的詳細資訊，請參閱[此連結](manage-ssl-protocols-in-ad-fs.md)。

### <a name="check-if-azure-ad-promptloginbehavior-is-configured-correctly-on-your-federated-domain-settings"></a>檢查您的同盟網域設定是否已正確設定 Azure AD PromptLoginBehavior
許多 Office 365 應用程式傳送 prompt = 登入 Azure AD。 Azure AD 預設會將它轉換成 AD FS 的新密碼登入。 如此一來，即使您已在 AD FS 中設定憑證驗證，您的終端使用者還是只會看到密碼登入。 
1)  使用 ' Set-msoldomainfederationsettings ' 命令來取得同盟網域設定
2)  請確定 PromptLoginBehavior 參數已設定為其中一個 [已停用] 或 [NativeSupport]

如需詳細資訊，請參閱[此連結](ad-fs-prompt-login.md)。 

### <a name="additional-troubleshooting"></a>其他疑難排解
這些很罕見發生
1)  如果您的 CRL 清單很長，可能會在嘗試下載時遇到一段時間。 在這種情況下，您必須依據，更新 ' MaxFieldLength ' 和 ' MaxRequestByte 'https://support.microsoft.com/en-us/help/820129/http-sys-registry-settings-for-windows




## <a name="reference-complete-list-of-user-certificate-claim-types-and-example-values"></a>參考：使用者憑證宣告類型和範例值的完整清單

|                                         宣告類型                                         |                              範例值                               |
|--------------------------------------------------------------------------------------------|--------------------------------------------------------------------------|
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/x509version         |                                    3                                     |
|     https://schemas.microsoft.com/2012/12/certificatecontext/field/signaturealgorithm      |                                sha256RSA                                 |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/issuer            |                 CN = entca，DC = domain，DC = contoso，DC = com                  |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/issuername          |                 CN = entca，DC = domain，DC = contoso，DC = com                  |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notbefore          |                           12/05/2016 20:50:18                            |
|          https://schemas.microsoft.com/2012/12/certificatecontext/field/notafter           |                           12/05/2017 20:50:18                            |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/subject           |   E =user@contoso.com，cn = user，cn = Users，dc = domain，dc = contoso，dc = com   |
|         https://schemas.microsoft.com/2012/12/certificatecontext/field/subjectname         |   E =user@contoso.com，cn = user，cn = Users，dc = domain，dc = contoso，dc = com   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/field/rawdata           |                {Base64 編碼的數位憑證資料}                 |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             DigitalSignature                             |
|        https://schemas.microsoft.com/2012/12/certificatecontext/extension/keyusage         |                             KeyEncipherment                              |
|  https://schemas.microsoft.com/2012/12/certificatecontext/extension/subjectkeyidentifier   |                 9D11941EC06FACCCCB1B116B56AA97F3987D620A                 |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/authoritykeyidentifier  |    KeyID = d6 13 e3 6b bc e5 d8 15 52 0a fd 36 6a d5 0b 51 f3 0b 25 7f     |
| https://schemas.microsoft.com/2012/12/certificatecontext/extension/certificatetemplatename |                                   使用者                                   |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/san           | 其他名稱：主體名稱 =user@contoso.com，RFC822 名稱 =user@contoso.com |
|           https://schemas.microsoft.com/2012/12/certificatecontext/extension/eku           |                          1.3.6.1.4.1.311.10.3.4                          |

## <a name="related-links"></a>相關連結
* [設定 AD FS 憑證驗證的替代主機名稱系結](ad-fs-support-for-alternate-hostname-binding-for-certificate-authentication.md)
* [在 Azure AD 中設定憑證授權單位單位](https://docs.microsoft.com/azure/active-directory/active-directory-certificate-based-authentication-get-started#step-2-configure-the-certificate-authorities)
