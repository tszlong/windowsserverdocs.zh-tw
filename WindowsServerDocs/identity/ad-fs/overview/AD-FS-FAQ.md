---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: AD FS 常見問題
description: AD FS 的常見問題
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 0a2bbeeb459fd364db728579dc20015a2474fd25
ms.sourcegitcommit: e5df3fd267352528eaab5546f817d64d648b297f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2019
ms.locfileid: "74163098"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS 常見問題（FAQ）


下列檔是有關 Active Directory 同盟服務的常見問題首頁。  檔已根據問題類型分割成群組。

## <a name="deployment"></a>部署

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何從舊版的 AD FS 進行升級/遷移
您可以使用下列其中一項升級 AD FS：


- Windows Server 2012 R2 AD FS 到 Windows Server 2016 AD FS 或更高版本。 請注意，如果您從 Windows Server 2016 AD FS 升級至 Windows Server 2019 AD FS，方法就會相同。 
    - [升級至使用 WID 資料庫的 Windows Server 2016 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [升級至使用 SQL 資料庫的 Windows Server 2016 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS 到 Windows Server 2012 R2 AD FS
    - [遷移至 Windows Server 2012 R2 上的 AD FS](https://technet.microsoft.com/library/dn486815.aspx)
- AD FS 2.0 到 Windows Server 2012 AD FS
    - [遷移至 Windows Server 2012 上的 AD FS](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x 至 AD FS 2。0
    - [從 AD FS 1.x 升級至 AD FS 2。0](https://technet.microsoft.com/library/ff678035.aspx)

如果您需要從 AD FS 2.0 或2.1 （Windows Server 2008 R2 或 Windows Server 2012）升級，則必須使用內建腳本（位於 C:\Windows\ADFS）。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>為什麼 AD FS 安裝需要重新開機伺服器？

Windows Server 2016 已新增 HTTP/2 支援，但 HTTP/2 無法用於用戶端憑證驗證。  因為許多 AD FS 案例都會使用用戶端憑證驗證，而大量用戶端不支援使用 HTTP/1.1 來重試要求，AD FS 伺服器陣列設定會將本機伺服器的 HTTP 設定重新設定為 HTTP/1.1。  這需要重新開機伺服器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>使用 Windows 2016 WAP 伺服器將 AD FS 伺服器陣列發佈到網際網路，但不升級支援的後端 AD FS 伺服器陣列嗎？
是，支援此設定，但此設定不支援新的 AD FS 2016 功能。  這項設定在從 AD FS 2012 R2 到 AD FS 2016 的遷移階段，應該是暫時性的，而且不應該長時間部署。

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>是否可以部署 Office 365 AD FS，而不需要將 proxy 發佈到 Office 365？
是，支援這種情況。 不過，做為副作用

1. 您將需要手動管理更新權杖簽署憑證，因為 Azure AD 將無法存取同盟中繼資料。 如需有關手動更新權杖簽署憑證的詳細資訊，請參閱[更新 Office 365 和 Azure Active Directory 的同盟憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. 您將無法利用舊版驗證流程（例如 ExO proxy 驗證流程）

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>AD FS 和 WAP 伺服器的負載平衡需求為何？

AD FS 是無狀態系統。 因此，登入的負載平衡相當簡單。 以下是負載平衡系統的重要建議。


- 負載平衡器不應設定 IP 親和性。 這可能會在某些 Exchange Online 案例中，將過度的負載放在您的伺服器子集上。
- 負載平衡器不得終止 HTTPS 連線，並重新起始與 ADFS 伺服器的新連線。
- 負載平衡器應確保在傳送至 ADFS 時，連接的 IP 位址應該轉譯為 HTTP 封包中的來源 IP。 當負載平衡器無法傳送 HTTP 封包中的來源 IP 時，負載平衡器必須將 IP 位址新增（或在現有的情況下），以作為標頭的 x 轉送。 若要正確處理特定的 IP 相關功能（禁用的 IP、外部網路智慧鎖定,...），這是必要的，而且如果設定不正確，可能會導致安全性降低。
- 負載平衡器應支援 SNI。 在此情況下，請確定已將 AD FS 設定為建立 HTTPS 系結，以處理不支援 SNI 的用戶端。
- 負載平衡器應該使用 AD FS HTTP 健康情況探查端點來偵測 AD FS 或 WAP 伺服器是否已啟動且正在執行，如果未傳回 200 [確定]，則將其排除。

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>AD FS 支援哪些多樹系設定？

AD FS 支援多個多樹系設定，並依賴基礎 AD DS 信任網路來驗證跨多個受信任領域的使用者。 我們強烈建議使用雙向樹系信任，因為這是較簡單的設定，以確保信任子系統正常運作，而不會發生問題。 附加

- 在單向樹系信任（例如包含合作夥伴身分識別的 DMZ 樹系）的事件中，我們建議在 corp 樹系中部署 ADFS，並將 DMZ 樹系視為另一個本機宣告提供者信任透過 LDAP 連線。 在此情況下，Windows 整合式驗證將無法用於 DMZ 樹系使用者，而且需要執行密碼驗證，因為這是唯一支援的 LDAP 機制。 如果您無法採用此選項，您將需要在 DMZ 樹系中設定另一個 ADFS，並在 corp 樹系的 ADFS 中將它新增為宣告提供者信任。 使用者必須進行主領域探索，但 Windows 整合式驗證和密碼驗證都能正常執行。 請在 DMZ 樹系中的 ADFS 發佈規則中進行適當的變更，因為 corp 樹系中的 ADFS 將無法從 DMZ 樹系取得使用者的額外使用者資訊。
- 雖然支援網域層級信任且可以使用，但我們強烈建議您移至樹系層級信任模型。 此外，您還需要確保名稱的 UPN 路由和 NETBIOS 名稱解析必須能夠正確地正常執行。

>[!NOTE]  
>如果選用 authentication 與雙向信任設定搭配使用，請確定呼叫者使用者被授與目標服務帳戶的「允許驗證」許可權。 


## <a name="design"></a>設計

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>AD FS 有哪些協力廠商的多重要素驗證提供者？
AD FS 提供可延伸的機制，供協力廠商 MFA 提供者進行整合。 沒有設定認證方案。 在發行之前，會假設廠商已執行必要的驗證。 

已通知 Microsoft 的廠商清單會在[AD FS 的 MFA 提供者](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)發佈。  有些提供者可能永遠不知道，我們將會在瞭解它們時更新清單。

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>AD FS 是否支援協力廠商 proxy？
是，協力廠商 proxy 可以放在 Web 應用程式 Proxy 前面，但任何協力廠商 Proxy 都必須支援使用[MS ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)來取代 Web 應用程式 proxy。

以下是我們知道的協力廠商提供者清單。  有些提供者可能永遠不知道，我們將會在瞭解它們時更新清單。

- [F5 存取原則管理員](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>AD FS 2016 的容量規劃調整大小試算表在哪裡？
您可以在[這裡](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)下載試算表的 AD FS 2016 版本。
這也可以用於 Windows Server 2012 R2 中的 AD FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何確保我的 AD FS 和 WAP 伺服器能夠支援 Apple 的 ATP 需求？

Apple 已發行一組稱為應用程式傳輸安全性（ATS）的需求，可能會影響從驗證至 AD FS 之 iOS 應用程式的呼叫。  您可以確定您的 AD FS 和 WAP 伺服器[是否符合使用 ATS 連接的需求，以](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)確保其相容。  
特別是，您應該確認您的 AD FS 和 WAP 伺服器支援 TLS 1.2，而且 TLS 連線的「協商的加密套件」將支援「完整轉寄密碼」。

您可以使用[AD FS 中的 [管理 Ssl 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)]，啟用和停用 SSL 2.0 和3.0 以及 TLS 版本1.0、1.1 和1.2。

若要確保您的 AD FS 和 WAP 伺服器只會協調支援 ATP 的 TLS 加密套件，您可以停用不在[atp 相容的加密套件清單](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)中的所有加密套件。  若要這麼做，請使用[WINDOWS TLS PowerShell Cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。

## <a name="developer"></a>開發人員

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>針對以 AD 進行驗證的使用者產生具有 ADFS 的 id_token 時，如何在 id_token 中產生「子」宣告？
"Sub" 宣告的值是用戶端識別碼 + 錨點宣告值的雜湊。

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>當使用者透過 WS-ADDRESSING/SAML-P 透過遠端宣告提供者信任登入時，重新整理權杖/存取權杖的存留期為何？
重新整理權杖的存留期會是 ADFS 從遠端宣告提供者信任所得到權杖的存留期。 存取權杖的存留期將是發出存取權杖之信賴憑證者的權杖存留期。

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>除了 OpenId 範圍之外，我還需要傳回設定檔和電子郵件範圍。 我可以使用範圍取得其他資訊嗎？ 如何在 AD FS 中執行？
您可以使用自訂的 id_token，在 id_token 本身新增相關資訊。 如需詳細資訊，請參閱[自訂要在 id_token 中發出的宣告](../development/Custom-Id-Tokens-in-AD-FS.md)文章。

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>如何在 JWT 權杖內發行 json blob？
AD FS 2016 中已加入這個的特殊 ValueType （"<http://www.w3.org/2001/XMLSchema#json>"）和 escape 字元（\x22）。 請參閱下列適用于發佈規則的範例，以及來自存取權杖的最終輸出。

範例發行規則：

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

在存取權杖中發出的宣告：

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>我可以將資源值當做範圍值的一部分來傳遞，像是如何對 Azure AD 進行要求？
使用伺服器2019上的 AD FS，您現在可以傳遞內嵌在範圍參數中的資源值。 範圍參數現在可以組織成以空格分隔的清單，其中每個專案的結構都是資源/範圍。 例如  
**< 建立有效的範例要求 >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS 是否支援 PKCE 延伸模組？
伺服器2019中的 AD FS 支援 OAuth 授權碼授與流程的程式碼交換的證明金鑰（PKCE）

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>AD FS 支援哪些允許的範圍？
- aza-如果使用訊息[代理程式用戶端的 OAuth 2.0 通訊協定延伸](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706)模組，且範圍參數包含「aza」範圍，則伺服器會發出新的主要重新整理權杖，並將它設定在回應的 [refresh_token] 欄位中，以及將 [refresh_token_expires_in] 欄位設定為新主要重新整理權杖的存留期（如果有強制執行）。
- openid-允許應用程式要求使用 OpenID Connect 授權通訊協定。
- logon_cert-logon_cert 範圍可讓應用程式要求登入憑證，以便用來以互動方式登入已驗證的使用者。 AD FS 伺服器會省略回應中的 access_token 參數，並改為提供 base64 編碼的 CMS 憑證鏈或 CMC 完整的 PKI 回應。 [這裡](https://docs.microsoft.com/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)提供更多詳細資料。 
- user_impersonation-必須有 user_impersonation 範圍，才能成功向 AD FS 要求代理者存取權杖。 如需如何使用此範圍的詳細資訊，請參閱[使用 OAuth 搭配 AD FS 2016，使用代理者（OBO）建立多層式應用程式](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md)。
- vpn_cert-vpn_cert 範圍可讓應用程式要求 VPN 憑證，其可用來建立使用 EAP-TLS 驗證的 VPN 連線。 這已不再受到支援。
- 電子郵件-允許應用程式要求已登入使用者的電子郵件宣告。 這已不再受到支援。 
- 設定檔-允許應用程式要求登入使用者的設定檔相關宣告。 這已不再受到支援。 


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何? 取代 AD FS 的 SSL 憑證？
AD FS SSL 憑證與 AD FS 管理嵌入式管理單元中找到的 AD FS 服務通訊憑證不同。  若要變更 AD FS SSL 憑證，您必須使用 PowerShell。 遵循下列文章中的指導方針：

[管理 AD FS 和 WAP 2016 中的 SSL 憑證](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何啟用或停用 AD FS 的 TLS/SSL 設定
若要停用或啟用 SSL 通訊協定和加密套件，請使用下列各項：

[管理 AD FS 中的 SSL 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Proxy SSL 憑證必須與 AD FS SSL 憑證相同嗎？  
請遵循下列關於 proxy SSL 憑證和 AD FS SSL 憑證的指導方針：


- 如果 proxy 是用來 proxy AD FS 使用 Windows 整合式驗證的要求，則 proxy SSL 憑證必須相同（使用相同的金鑰）做為同盟伺服器 SSL 憑證
- 如果已啟用 AD FS 屬性 "ExtendedProtectionTokenCheck" （AD FS 中的預設設定），proxy SSL 憑證必須相同（使用相同的金鑰）做為同盟伺服器 SSL 憑證
- 否則，proxy SSL 憑證可以有來自 AD FS SSL 憑證的不同金鑰，但必須符合相同的[需求](../overview/AD-FS-2016-Requirements.md)

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>為什麼我在 AD FS 上只看到密碼登入，而不是我已設定的其他驗證方法？ 
當應用程式明確要求對應至已設定並啟用之驗證方法的特定驗證 URI 時，AD FS 只會在登入畫面中顯示單一驗證方法。 這會在 WS-同盟要求的 ' wauth ' 參數和 SAML 通訊協定要求中的 ' RequestedAuthnCtxRef ' 參數中傳達。 因此，只會顯示所要求的驗證方法（例如密碼登入）。

當 AD FS 搭配 Azure AD 使用時，應用程式通常會將 prompt = login 參數傳送給 Azure AD。 Azure AD 預設會將此轉譯為要求以新密碼為基礎的登入，以 AD FS。 這是在網路內部 AD FS 查看密碼登入的最常見原因，或看不到使用您的憑證登入的選項。 您可以變更 Azure AD 中的同盟網域設定，輕鬆地補救這項功能。 

如需有關如何進行此設定的詳細資訊，請參閱[Active Directory 同盟服務 prompt = 登入參數支援](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-change-the-ad-fs-service-account"></a>如何變更 AD FS 服務帳戶？
若要變更 AD FS 服務帳戶，請遵循使用 AD FS 工具箱[服務帳戶 Powershell 模組](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)中的指示。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何將瀏覽器設定為使用 Windows 整合式驗證（WIA）搭配 AD FS？

如需有關如何設定瀏覽器的詳細資訊，請參閱[將瀏覽器設定為使用 Windows 整合式驗證（WIA）搭配 AD FS](../operations/Configure-AD-FS-Browser-WIA.md)。

### <a name="can-i-turn-off-browserssoenabled"></a>我可以關閉 BrowserSsoEnabled 嗎？
如果您沒有以 ADFS 上的裝置為基礎的存取控制原則，或使用 ADFS 的 Windows Hello 企業版憑證註冊，則為，您可以關閉 BrowserSsoEnabled。 BrowserSsoEnabled 可讓 ADFS 從包含裝置資訊的用戶端收集 PRT （主要重新整理權杖）。 沒有 ADFS 的裝置驗證將無法在 Windows 10 裝置上使用。

### <a name="how-long-are-ad-fs-tokens-valid"></a>AD FS 的權杖有效期限是多久？

此問題通常是指「使用者多久才能取得單一登入（SSO），而不需要輸入新的認證，以及我如何以系統管理員的身分控制？」  這項行為以及控制它的設定，將在[AD FS 單一登入設定一](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)文中說明。

以下列出各種 cookie 和權杖的預設存留期（以及管理存留期的參數）：

**已註冊的裝置**

- PRT 和 SSO cookie：90天的最大值，由 PSSOLifeTimeMins 控管。 （提供的裝置至少每隔14天會使用，其由 DeviceUsageWindow 所控制）

- 重新整理權杖：根據上面的計算來提供一致的行為

- access_token：預設為1小時，根據信賴憑證者

- id_token：與存取權杖相同

**未註冊的裝置**

- SSO cookie：預設為8小時，受 SSOLifetimeMins 控管。  啟用 [讓我保持登入（KMSI）] 時，預設值為24小時，且可透過 KMSILifetimeMins 進行設定。


- 重新整理權杖：預設為8小時。 已啟用 KMSI 的24小時

- access_token：預設為1小時，根據信賴憑證者

- id_token：與存取權杖相同

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 是否支援 HTTP 嚴格傳輸安全性（HSTS）？  

HTTP Strict Transport Security （HSTS）是一種 web 安全性原則機制，可協助減少同時具有 HTTP 和 HTTPS 端點之服務的通訊協定降級攻擊和 cookie 劫持。 它可讓網頁伺服器宣告網頁瀏覽器（或其他符合使用者代理程式）只能使用 HTTPS 與其互動，而不是透過 HTTP 通訊協定。

Web 驗證流量的所有 AD FS 端點都會以獨佔方式透過 HTTPS 開啟。  因此，AD FS 有效地降低 HTTP Strict 傳輸安全性原則機制所提供的威脅（根據設計，因為 HTTP 中沒有接聽程式），所以不會降級至 HTTP。 此外，AD FS 藉由使用安全旗標標記所有 cookie，以防止 cookie 以 HTTP 通訊協定端點傳送至另一部伺服器。

因此，不需要在 AD FS 伺服器上執行 HSTS，因為它永遠不會降級。  基於合規性目的，AD FS 伺服器會符合這些需求，因為它們永遠不會使用 HTTP，而且所有 cookie 都會標示為安全。

此外，AD FS 2016 （包含最新的修補程式），以及 AD FS 2019 支援發出 HSTS 標頭。 若要進行此設定，請參閱[使用 AD FS 自訂 HTTP 安全性回應標頭](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms 轉送-用戶端 ip 不包含用戶端的 IP，而是包含 proxy 前面的防火牆 IP。 哪裡可以取得正確的用戶端 IP？
不建議在 WAP 之前進行 SSL 終止。 如果在 WAP 前面完成 SSL 終止，則 X 毫秒轉送的用戶端 ip 將包含 WAP 前面的網路裝置 IP。 以下是 AD FS 支援的各種 IP 相關宣告的簡短說明：
 - x-ms-用戶端-ip：連線到 STS 之裝置的網路 IP。  在外部網路要求的情況下，這一律包含 WAP 的 IP。
 - x-ms 轉送-用戶端 ip：多重值宣告，其中會包含由 Exchange Online 轉送至 ADFS 的任何值，加上連接到 WAP 的裝置 IP 位址。
 - Userip：針對外部網路要求，此宣告會包含「x-毫秒-轉送-用戶端 ip」的值。  針對內部網路要求，此宣告會包含與 [x-ms-用戶端 ip] 相同的值。

 此外，在 AD FS 2016 （具有最新的修補程式）和更高版本中，也支援捕捉 x 轉送的標頭。 在第3層（保留 IP）未轉寄的任何負載平衡器或網路裝置，都應該將連入的用戶端 IP 新增至業界標準 x 轉送的標頭。 

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我嘗試取得使用者資訊端點上的其他宣告，但它只會傳回主體。 如何取得額外的宣告？
AD FS 的使用者資訊端點一律會傳回 OpenID 標準中所指定的主體宣告。 AD FS 不會提供透過使用者資訊端點所要求的其他宣告。 如果您需要識別碼權杖中的其他宣告，請參閱[AD FS 中的自訂識別碼權杖](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>為什麼我在 AD FS 伺服器上看到許多1021錯誤？
在資源 00000003-0000-0000-c000-000000000000 的 AD FS 上，通常會記錄此事件，以取得不正確資源存取權。 此錯誤是因為用戶端嘗試取得 Azure AD Graph 服務的存取權杖時發生錯誤的行為所造成。 由於資源不存在 AD FS 上，因此會導致 AD FS 伺服器上的事件識別碼為1021。 在 AD FS 上，您可以放心忽略資源 00000003-0000-0000-c000-000000000000 的任何警告或錯誤。

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>為什麼我會看到無法將 AD FS 服務帳戶新增至 Enterprise Key Admins 群組的警告？
只有當網域中存在具有 FSMO PDC 角色的 Windows 2016 網域控制站時，才會建立此群組。 若要解決此錯誤，您可以手動建立群組，並遵循下列步驟，在將服務帳戶新增為群組成員之後，授與必要的許可權。
1.  開啟 **\[Active Directory 使用者和電腦\]** 。
2.  在流覽窗格中，以**滑鼠右鍵按一下**您的功能變數名稱，然後**按一下**[屬性]。
3.  **按一下**安全性（如果遺失 [安全性] 索引標籤，請從 [View] 功能表開啟 [Advanced Features]）。
4.  **按一下**提前. **按一下**載入. **按一下**選取 [主體]。
5.  [選取使用者、電腦、服務帳戶或群組] 對話方塊隨即出現。  在 [輸入要選取的物件名稱] 文字方塊中，輸入 Key Admin Group。  按一下 [確定]。
6.  在 [套用至] 清單方塊中，選取 [子系**使用者物件**]。
7.  使用捲軸，並**按一下**頁面底部的 [全部清除]。
8.  在 [**屬性**] 區段中，選取 [**讀取 Msds-keycredentiallink**並**寫入 msds-keycredentiallink**]。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>為什麼當伺服器未使用 SSL 憑證傳送鏈中的所有中繼憑證時，Android 裝置的新式驗證會失敗？

同盟使用者可能會遇到使用 Android ADAL 程式庫失敗之應用程式的驗證 Azure AD。 應用程式會在嘗試顯示登入頁面時取得**AuthenticationException** 。 在 chrome 中，AD FS 登入頁面可能被稱為「不安全」。

Android-跨所有版本和所有裝置-不支援從憑證的 [ **authorityInformationAccess** ] 欄位下載其他憑證。 這也適用于 Chrome 瀏覽器。 如果未從 AD FS 傳遞整個憑證鏈，任何缺少中繼憑證的伺服器驗證憑證都會導致此錯誤。

此問題的適當解決方案是設定 AD FS 和 WAP 伺服器，以傳送必要的中繼憑證以及 SSL 憑證。

從一部電腦匯出 SSL 憑證時，若要匯入 AD FS 和 WAP 伺服器的電腦個人存放區，請務必匯出私密金鑰，然後選取 [**個人資訊交換-PKCS #12**]。

請務必選取 [**如果可能的話，包含憑證路徑中的所有憑證**] 核取方塊，以及 [**匯出所有延伸屬性**]。  

在 Windows 伺服器上執行 certlm.msc，並匯入 *。PFX 到電腦的個人憑證存儲。 這會導致伺服器將整個憑證鏈傳遞至 ADAL 程式庫。

>[!NOTE]
> 網路負載平衡器的憑證存放區也應該更新為包含整個憑證連結（如果有的話）

### <a name="does-ad-fs-support-head-requests"></a>AD FS 是否支援 HEAD 要求？
AD FS 不支援 HEAD 要求。  應用程式不應該對 AD FS 端點使用 HEAD 要求。  這可能會導致非預期和/或延遲的 HTTP 錯誤回應。  此外，您可能會在 AD FS 事件記錄檔中看到未預期的錯誤事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>當我使用遠端 IdP 登入時，為什麼看不到重新整理權杖？
如果 IdP 所發行的權杖 validty 少於1小時，則不會發出重新整理權杖。 為確保發行重新整理權杖，請將 IdP 發出的權杖有效期限增加至1小時以上。

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>我們是否有任何方式可以變更 RP 權杖加密演算法？
根據預設，RP 權杖加密會設定為 AES256，而且無法變更為其他任何值。

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>在混合模式的伺服器陣列上，我在嘗試使用 Set-adfssslcertificate-指紋設定新的 SSL 憑證時收到錯誤。 如何將混合模式中的 SSL 憑證更新 AD FS 伺服器陣列？
混合模式 AD FS 伺服器陣列應該是 transitionary 狀態。 建議您在進行升級程式之前，先變換 SSL 憑證，或在更新 SSL 憑證之前先完成進程並增加伺服器陣列行為層級。 如果未完成這項作業，下列指示會提供更新 SSL 憑證的功能。 

在 WAP 伺服器上，您仍然可以使用 WebApplicationProxySslCertificate。 在 ADFS 伺服器上，您必須使用 netsh。 依照下列步驟執行：

1. 選取用於維護的 ADFS 2016 伺服器子集（例如從負載平衡器移除）
2. 在 #1 中選取的伺服器上，透過 MMC 匯入新的憑證
3. 刪除現有的憑證

    a. netsh HTTP delete sslcert hostnameport = fs。 contoso .com： 443 b。 netsh HTTP delete sslcert hostnameport = localhost： 443 c。 netsh HTTP delete sslcert hostnameport = fs. contoso .com：49443

4.  加入新的憑證

    a. netsh HTTP add sslcert hostnameport = fs。 contoso .com： 443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b。 netsh HTTP add sslcert hostnameport = localhost： 443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    c. netsh HTTP add sslcert hostnameport = fs。 contoso .com： 49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. 在選取的伺服器上重新開機 ADFS 服務
6. 移除 WAP 伺服器的子集以進行維護
7. 在選取的 WAP 伺服器上，透過 MMC 匯入新的憑證
8. 使用 Cmdlet 設定 WAP 上的新憑證

    a. WebApplicationProxySslCertificate-指紋 "CERTTHUMBPRINT"

9. 在選取的 WAP 伺服器上重新開機服務
10. 將選取的 WAP 和 AD FS 伺服器放回生產環境中。

以類似的方式，在其餘的 AD FS 和 WAP 伺服器上執行更新。

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>當 Web 應用程式 Proxy （WAP）伺服器位於 Azure Web 應用程式防火牆（WAF）後方時，是否支援 ADFS？
ADFS 和 Web 應用程式伺服器支援任何不會在端點上執行 SSL 終止的防火牆。 此外，ADFS/WAP 伺服器具有內建機制，可防止常見的 web 攻擊，例如跨網站腳本、ADFS proxy，以及滿足[MS ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)所定義的所有需求。

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>我看到「事件441：找到具有錯誤標記系結機碼的權杖」。 我該怎麼做才能解決此問題？
在 AD FS 2016 中，會自動啟用權杖系結，並會導致發生此錯誤的 proxy 和同盟案例有多個已知問題。 若要解決此問題，請執行下列 Powershell 命令，並移除權杖系結支援。

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>我已將伺服器陣列從 Windows Server 2016 中的 AD FS 升級到 Windows Server 2019 中的 AD FS。 AD FS 伺服器陣列的伺服器陣列行為層級已成功提高至2019，但 Web 應用程式 Proxy 設定仍然顯示為 Windows Server 2016？
升級至 Windows Server 2019 之後，Web 應用程式 Proxy 的設定版本將會繼續顯示為 Windows Server 2016。 Web 應用程式 Proxy 沒有適用于 Windows Server 2019 的新版本特定功能，如果伺服器陣列行為層級已成功在 AD FS 上引發，Web 應用程式 Proxy 就會繼續依設計的形式顯示為 Windows Server 2016。 
