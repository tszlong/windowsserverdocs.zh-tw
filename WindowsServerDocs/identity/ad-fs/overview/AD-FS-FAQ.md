---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: "AD FS 2016 常見問題集"
description: "Ad FS 2016 常見問題集"
author: jenfieldmsft
ms.author: billmath
manager: femila
ms.date: 03/06/2018
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 313447d2c92c15505434ec5c39898ca84aef46db
ms.sourcegitcommit: 556361fe7c73c75d6cdc46a67dc88679fbe89c61
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2018
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS 常見問題集（常見問題集）

>適用於：Windows Server 2016

下列文件已 home 有關 Active Directory 同盟服務常見問題的解答。  在文件已分為群組根據問題的類型。

## <a name="deployment"></a>部署 

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何可以我升級日移轉從舊版 AD FS
您可以升級 AD FS 使用下列其中一個動作：


- Windows Server 2012 R2 AD FS 以 Windows Server 2016 AD FS
    - [升級到 Windows Server 2016 使用 WID 資料庫中 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [升級到 Windows Server 2016 使用 SQL 資料庫中 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS 以 Windows Server 2012 R2 AD FS
    - [移轉到 Windows Server 2012 R2 上 AD FS](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 以 Windows Server 2012 AD FS
    - [若要在 Windows Server 2012 上 AD FS 移轉](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x AD FS 2.0 到 
    - [AD FS 從升級至 AD FS 2.0 1.x](https://technet.microsoft.com/library/ff678035.aspx)

如果您需要從 AD FS 2.0 或 2.1（Windows Server 2008 R2 或 Windows Server 2012）升級，您必須使用附隨指令碼（位於 C:\Windows\ADFS）。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>AD FS 安裝為何需要伺服器的重新開機？

HTTP 2 月的支援在 Windows Server 2016 中新增但 HTTP 2 月不能用於 client 憑證驗證。  因為許多 AD FS 案例使用 client 憑證驗證，以及大量戶端執行支援重試要求使用 HTTP 日 1.1，AD FS 發電廠設定重新設定 HTTP 日 1.1 本機伺服器 HTTP 設定。  這需要重新開機的伺服器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>AD FS 發電廠發行網際網路，而不需要升級後端 AD FS 發電廠支援使用 Windows 2016 WAP 伺服器嗎？
是的支援此設定，但想支援此設定不 AD FS 2016 的新功能。  此設定是 ad FS 2016 AD FS 2012 R2 的移轉階段期間的 [暫存並不會部署長一段時間。

## <a name="design"></a>設計

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>哪些第三方多因素驗證提供者可供使用 AD FS？ 
以下是我們已經知道協力廠商提供者的清單。  隨時可能提供者可，我們不知道我們將會在為我們了解這些更新的清單。

- [Gemalto 身分和安全性服務](http://www.gemalto.com/identity)
- [inWebo 驗證企業服務](http://www.inwebo.com/)
- [登入人 MFA API 連接器](https://www.loginpeople.com)
- [Microsoft Active Directory 同盟服務的 RSA SecurID 驗證代理程式](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [AD fs SafeNet 驗證服務 (SAS) 代理程式](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Swisscom 行動裝置版 ID 驗證服務](http://swisscom.ch/mid)
- [Symantec 驗證和 ID Protection Service (VIP)](http://www.symantec.com/vip-authentication-service) 

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>第三方 proxy AD FS 進行支援？
是的第三方 proxy 可以放前面應用程式網路 Proxy，但任何第三方 proxy 必須支援[MS-ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)來取代 Proxy Web 應用程式。

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>適用於支援 MS-ADFSPIP AD FS 哪些第三方 proxy？

以下是我們已經知道協力廠商提供者的清單。  隨時可能提供者可，我們不知道我們將會在為我們了解這些更新的清單。

- [F5 存取原則管理員](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>何處容量規劃縮放試算表 AD FS 2016？
AD FS 2016 版本試算表可以下載[在此](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
這也可以使用在 Windows Server 2012 R2 AD fs。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何確定我 AD FS 和 WAP 伺服器支援的 Apple ATP 需求？

蘋果發行一組稱為 App 傳輸安全性 (ATS)，可能會影響來電向 AD FS 進行驗證的 iOS 應用程式需求。  AD FS 可確保與，並確認它們支援，符合 WAP 伺服器][適用於使用 ATS 連接需求](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  
尤其是您應該驗證，AD FS 和 WAP 伺服器支援 TLS 1.2 以及的 TLS 連接交涉的密碼套件，將會支援完整轉寄密碼。

您可以讓和停用 1.0，1.1、1.2 SSL 2.0 及 3.0 TLS 版本使用[管理 SSL 通訊協定 AD FS 在](../operations/Manage-SSL-Protocols-in-AD-FS.md)。

確保您 AD FS 和 WAP 伺服器交涉只支援 ATP TLS 密碼套件，您可以停用所有密碼套件不是在[清單 ATP 相容密碼套件的](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  若要這樣做，請使用[Windows TLS PowerShell cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。 


## <a name="operations"></a>作業

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何取代 AD FS SSL 憑證？ 
AD FS SSL 憑證不找到 AD FS 管理嵌入式管理單元 AD FS 服務通訊憑證相同。  若要變更 AD FS SSL 憑證，您將需要使用 PowerShell。 請依照下列指導方針下方文章中：

[AD FS 和 WAP 2016 管理 SSL 憑證](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何讓或停用 AD FS TLS 日 SSL 設定
若要停用，或讓 SSL 通訊協定並密碼套件，使用下列方法：

[管理通訊協定 SSL AD FS](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Proxy SSL 憑證有 AD FS SSL 憑證相同嗎？  
使用下列指導方針與 proxy SSL 憑證，AD FS SSL 憑證：


- 如果您使用的 proxy 使用 Windows 整合驗證 proxy SSL 憑證 proxy AD FS 要求必須是相同（使用相同的按鍵）為聯盟伺服器 SSL 憑證
- 如果」ExtendedProtectionTokenCheck」是 AD FS 屬性支援（預設值 AD FS 中），proxy SSL 憑證必須是相同（使用相同的按鍵）聯盟伺服器 SSL 憑證
- 否則，proxy SSL 憑證可以有不同的金鑰 AD FS SSL 憑證，但必須符合相同[需求](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>如何設定提示 AD fs = 登入問題？
如何設定提示資訊 = 登入，查看[Active Directory 同盟服務提示 = 登入參數支援](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何設定 Windows 整合驗證 (WIA) 使用 AD FS 使用的瀏覽器？

如何設定瀏覽器資訊的查看[設定瀏覽器，AD FS 使用 Windows 整合驗證 (WIA) 以](../operations/Configure-AD-FS-Browser-WIA.md)。


### <a name="how-long-are-ad-fs-tokens-valid"></a>多久的 AD FS 權杖有效？

這個問題通常表示 '多久使用者才能單一登入 (SSO) 而不需要輸入新的憑證，並如何以系統管理員身分控制的' 嗎？  這個問題，以及控制該設定的文件中所述[在此](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)。

以下列出的各種 cookie 和權杖預設存留時間（以及參數管理存留時間）：

**且已的裝置**

- PRT 和 SSO cookie：最大的 90 天 PSSOLifeTimeMins 所規範。 （所提供的裝置使用至少每個 14 天，這由 DeviceUsageWindow）

- 重新整理預付碼：計算會根據上述提供一致的行為

- access_token：根據信賴的預設 1 小時

- id_token：相同存取權杖

**未且已的裝置**

- SSO cookie: SSOLifetimeMins 8 小時的預設所規範。  預設時可以的保留我登入 (KMSI)，為 24 小時，可透過 KMSILifetimeMins 設定。


- 重新整理預付碼：8 小時的預設值。 與支援 KMSI 24 小時

- access_token：根據信賴的預設 1 小時

- id_token：相同存取權杖

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 不支援 HTTP 嚴格傳輸安全性 (HSTS)？  

HTTP 嚴格傳輸安全性 (HSTS) 是有助於減少通訊協定降級攻擊和服務的 HTTPS 和 HTTP 結束 cookie 劫持網站的安全性原則機制。 它可以讓宣告的網頁瀏覽器（或其他 complying 使用者代理人）應該只互動使用 HTTPS 並不會透過 HTTP 通訊協定的網頁伺服器。

Web 驗證流量的所有 AD FS 端點被都一個專屬透過 HTTPS。  如此一來，AD FS 有效降低 HTTP 嚴格傳輸的安全性原則機制提供威脅（所設計還有至 HTTP 不降級因為 HTTP 中有不接聽）。 此外，AD FS 可防止 cookie 傳送至 HTTP 通訊協定的端點使用另一部伺服器標示的安全旗標與的所有 cookie。

因此，AD FS 伺服器上實作 HSTS 不需要它永遠不會降級。  針對相容性目的，AD FS 伺服器符合下列需求，因為它們可以不使用 HTTP 且安全標示的所有 cookie。

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>不包含 client 的 IP X ms-轉送-client-ip，但包含 proxy 前面防火牆的 IP。 何處取得正確的 client IP？
不建議方法之前 WAP SSL 終止。 萬一 SSL 終止完成 WAP 前面，X ms-轉送-client-ip 會包含前面 WAP 網路裝置的 IP。 以下是各種 IP 的簡短描述相關宣告 AD FS 支援：
 - X ms-client ip：網路連接到 STS 裝置的 IP。  在外部要求這一律會包含 WAP 的 IP。
 - X ms-轉送-client-ip：這將會包含 Online 換貨，以及連接至 WAP 裝置的 IP 位址，轉送給 ADFS 的任何值多重值理賠要求。
 - Userip：外部要求此宣告將會包含 x ms-轉送-client-ip 的值。  內部要求，此宣告將會包含 x ms-client ip 相同的值。

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我正在嘗試取得其他宣告端點上的使用者的資訊，但它只退貨主題。 如何取得其他宣告？
ADFS 使用者資訊端點一定會傳回 OpenID 標準中指定以主旨理賠要求。 AD FS 不會透過使用者資訊端點要求的其他宣告。 如果您需要 ID 權杖中的其他宣告，請參考[自訂 ID 權杖 AD FS 在](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-alot-of-1021-errors-on-my-ad-fs-servers"></a>為何我 AD FS 伺服器中看到許多 1021 年錯誤？
這個事件通常是登入不正確存取資源 AD FS 上的資源 00000003-0000-0000-c000-000000000000。 這個錯誤會造成嘗試存取權杖取得 Azure AD 圖形服務 client 錯誤行為。 在 AD FS 不資源，因為這會導致事件 ID 1021 AD FS 伺服器上。 它是可以放心略過任何警告或錯誤訊息的資源 00000003-0000-0000-c000-000000000000 上 AD FS。 

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>為何我會看到一則警告失敗 AD FS 服務 account 新增至企業鍵系統管理員」群組？
此群組只建立網域中的 Windows 2016 網域控制站的 FSMO PDC 角色有時。 若要解析錯誤，您可以手動建立群組，並依照以下提供服務 account 新增為群組成員後的權限。
1.  開放**Active Directory 使用者和電腦**。 
2.  **以滑鼠右鍵按一下**您的瀏覽窗格中的網域名稱和**按**屬性。
3.  **按一下**安全性（如果您遺失了 [安全性] 索引標籤，將進階功能的 [檢視] 功能表）。
4.  **按一下**進階。 **按一下**新增。 **按一下**請選取主體。
5.  [選取使用者、電腦、或群組] 對話方塊中出現。  在 [輸入物件名稱來選取] 文字方塊中，輸入鍵管理群組。  按一下 \ [確定 \]。
6.  在適用於清單中，選取 [**系使用者物件**。
7.  使用捲，捲動到頁面底部，**按一下 [** [全部清除]。
8.  在**屬性**區段中，選取**朗讀 msDS-KeyCredentialLink**並**撰寫 msDS-KeyCrendentialLink**。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>會從 Android 裝置的現代化驗證失敗的原因是否伺服器不會傳送中使用 SSL 憑證鏈結所有中繼憑證？

聯盟的使用者可能會遇到驗證 Azure ad 的應用程式使用 Android ADAL 媒體櫃失敗。 App 將會取得**AuthenticationException**當嘗試顯示登入頁面。 AD FS 中登入頁面可能會被如何稱呼為不安全。

Android 的所有版本和所有的裝置不支援下載額外的憑證的**authorityInformationAccess**欄位的憑證。 這是 Chrome 瀏覽器，為 true 也。 如果從 AD FS 不會傳送整個憑證鏈結遺失中繼憑證的任何伺服器驗證憑證會導致這個錯誤。 

這個問題的適當方案是設定 AD FS 和 WAP 伺服器來傳送必要中間的憑證，以及 SSL 憑證。 

當匯出 SSL 憑證，從一部電腦，若要匯入到電腦的個人的市集，AD FS 和 WAP 伺服器，請務必匯出私人鍵，然後選取**個人資訊交換 PKCS #12**。 

很重要的核取方塊來**如果可能包含所有的憑證憑證路徑中**核取，以及**匯出所有擴充的屬性**。 

Windows 伺服器上執行 certlm.msc 和匯入 *。插入電腦的個人憑證存放區 PFX。 這會造成伺服器 ADAL 文件庫傳遞整個憑證鏈結。 

>[!NOTE]
> 憑證存放區的網路負載平衡器也應包含完整的憑證鏈結如果有的話更新

### <a name="does-ad-fs-support-head-requests"></a>AD FS 不支援車頭要求？
AD FS 不支援車頭要求。  應用程式不應該使用 AD FS 端點針對車頭要求。  這可能會造成 HTTP 錯誤回應是發生未預期和/或延遲。  此外，您可能會看到未預期的錯誤事件 AD FS 事件登入。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>為何無法看到重新整理預付碼時，我已登入遠端 IdP？
如果 IdP 所發行的權杖 validty 小於 1 小時的不被發行重新整理預付碼。 為確保發出重新整理預付碼，請增加權杖 IdP 發給超過 1 小時的有效性。
