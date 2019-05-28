---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: AD FS 2016 常見問題集
description: 適用於 AD FS 2016 的常見問題集
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 04/17/2019
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdd31a8b7c2c6ef87d1d22d901b5c6ca69b5c70d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188723"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS 常見問題集 (Faq)


下列文件是常見問題與 Active Directory Federation Services 首頁。  文件已分割成的問題類型為基礎的群組。

## <a name="deployment"></a>部署

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何可以升級/從移轉舊版 AD FS
您可以升級 AD FS，使用下列其中一項：


- Windows Server 2012 R2 到 Windows Server 2016 AD FS 的 AD FS
    - [升級至使用 WID 資料庫的 Windows Server 2016 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md)
    - [升級至使用 SQL 資料庫的 Windows Server 2016 AD FS](../deployment/Upgrading-to-AD-FS-in-Windows-Server-2016-SQL.md)
- Windows Server 2012 AD FS Windows Server 2012 R2 AD fs
    - [移轉至 Windows Server 2012 R2 上的 AD FS](https://technet.microsoft.com/library/dn486815.aspx)
- Windows Server 2012 AD fs 的 AD FS 2.0
    - [移轉至 Windows Server 2012 上的 AD FS](https://technet.microsoft.com/library/jj647765.aspx)
- AD FS 1.x AD fs 2.0
    - [從 AD FS 升級 AD fs 2.0 的 1.x](https://technet.microsoft.com/library/ff678035.aspx)

如果您需要從 AD FS 2.0 或 2.1 （Windows Server 2008 R2 或 Windows Server 2012） 升級，您必須使用內建指令碼 （位於 C:\Windows\ADFS）。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>安裝 AD FS 為什麼需要重新啟動伺服器？

HTTP/2 支援已新增在 Windows Server 2016 中，但 HTTP/2 無法用於用戶端憑證驗證。  因為許多的 AD FS 案例都會使用用戶端憑證驗證，以及大量的用戶端執行不支援 重試要求使用 HTTP/1.1，AD FS 伺服器陣列組態重新設定為 HTTP/1.1 的本機伺服器的 HTTP 設定。  這需要重新啟動伺服器。  

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>Windows 2016 WAP 伺服器用來發佈至網際網路的 AD FS 伺服器陣列，而不需升級後端的 AD FS 伺服器陣列支援嗎？
是，不支援此設定，不過任何新的 AD FS 2016 功能會支援此組態中。  此設定要在移轉階段中從 AD FS 2012 R2 ad FS 2016 是暫時性，且不應部署長一段時間。

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>是否可以將適用於 Office 365 部署 AD FS，而不將 proxy 發佈至 Office 365？
是，支援此功能。 不過，隨著副作用

1. 您必須手動管理更新權杖簽署憑證，因為 Azure AD 不能存取同盟中繼資料。 如需手動更新權杖簽署憑證的詳細資訊，請參閱[更新 Office 365 和 Azure Active Directory 的同盟憑證](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. 您無法利用舊版驗證流程 （例如 ExO proxy 驗證流程）

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>AD FS 和 WAP 伺服器的負載平衡需求有哪些？

AD FS 是無狀態的系統。 因此，負載平衡是相當簡單的登入。 以下是負載平衡系統的重要建議。


- 不應使用 IP 同質性設定負載平衡器。 這可能過度負載放在特定 Exchange Online 的情況下您伺服器的子集。
- 負載平衡器必須不終止 HTTPS 連線，並重新初始化在 ADFS 伺服器的新連接。
- 負載平衡器應確保 HTTP 封包中的來源 ip 應該轉譯，連接的 IP 位址，傳送至 ADFS 時。 負載平衡器無法傳送 HTTP 封包中的來源 IP，負載平衡器必須新增 （或附加時現有的） IP 位址，以 x-轉送標頭。 針對特定 IP 能夠正確處理相關功能 （禁用 IP、 外部網路智慧鎖定，...），而且可能會導致降低的安全性，如果設定不當，這是必要的。
- 負載平衡器應支援 SNI。 在事件沒有，請確定 AD FS 已建立來處理非 SNI 支援用戶端的 HTTPS 繫結。
- 負載平衡器應該使用 AD FS HTTP 健康情況探查端點來偵測如果 AD FS 或 WAP 伺服器已啟動並執行，並將它們排除，如果 [確定] 就不會傳回 200。

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>支援多樹系設定 AD FS？

AD FS 支援多個多樹系的組態，並依賴基礎的 AD DS 信任網路，來驗證使用者跨多個受信任的領域。 我們強烈建議 2 向樹系信任，因為這是更簡單的安裝程式，以確保信任子系統可正確運作而不發生問題。 此外，

- 如果發生其中一種方式的樹系信任，例如 DMZ 樹系包含合作夥伴的身分識別，建議部署 corp 樹系中的 ADFS 和 DMZ 樹系視為透過 LDAP 連線的另一個本機宣告提供者信任。 在此情況下整合式 Windows 驗證並不適用於 DMZ 樹系使用者，他們必須執行密碼驗證，因為這是 ldap 唯一支援的機制。 萬一您不能採用此選項，您必須設定其他的 ADFS，在 DMZ 樹系中，然後將它加入做為宣告提供者信任的 ADFS 中 corp 樹系中。 使用者必須執行首頁領域探索，但 Windows 整合式驗證和密碼驗證會運作。 請為 corp 樹系中的 ADFS 將無法從 DMZ 樹系取得使用者的額外使用者資訊，請在 ADFS 中 DMZ 樹系中的發行規則中做出適當變更。
- 雖然網域層級信任支援，並且可以運作，但我們強烈建議您移至樹系層級的信任模式。 此外，您必須確保 UPN 路由和 NETBIOS 名稱解析的名稱需要精確地運作。



## <a name="design"></a>設計

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>適用於 AD FS 可使用哪些協力廠商的多重要素驗證提供者？
以下是我們已了解的協力廠商提供者的清單。  可能一律有可用，我們不知道，我們會更新清單，因為我們了解這些提供者。

- [Gemalto 身分識別與安全性服務](http://www.gemalto.com/identity)
- [inWebo 企業驗證服務](http://www.inwebo.com/)
- [登入 People MFA API 連接器](https://www.loginpeople.com)
- [RSA SecurID 驗證代理程式，Microsoft Active Directory Federation services](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)
- [SafeNet 驗證服務 (SAS) 代理程式，適用於 AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)
- [Swisscom 公司行動裝置識別碼驗證服務](http://swisscom.ch/mid)
- [Symantec 驗證和識別碼保護服務 (VIP)](http://www.symantec.com/vip-authentication-service)

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>協力廠商 proxy 與 AD FS 支援？
沒錯，協力廠商 proxy 可以位於 Web 應用程式 Proxy，但是必須支援的任何協力廠商 proxy [MS ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)用來取代 Web 應用程式 Proxy。

### <a name="what-third-party-proxies-are-available-for-ad-fs-that-support-ms-adfspip"></a>適用於 AD FS 支援 MS ADFSPIP 哪些協力廠商 proxy？

以下是我們已了解的協力廠商提供者的清單。  可能一律有可用，我們不知道，我們會更新清單，因為我們了解這些提供者。

- [F5 Access Policy Manager](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)

### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>其中是容量規劃調整大小試算表適用於 AD FS 2016？
試算表中的 AD FS 2016 新版可下載[此處](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)。
這也可以使用適用於 Windows Server 2012 R2 中的 AD FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何確保我的 AD FS 和 WAP 伺服器支援 Apple 的 ATP 需求？

Apple 已發行一組稱為 App Transport Security (ATS) 可能會影響從 AD fs 進行驗證的 iOS 應用程式呼叫的需求。  您可以確保您的 AD FS 和 WAP 伺服器符合藉由確定它們支援[連線使用 ATS 需求](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  
特別是，您應該確認您的 AD FS 和 WAP 伺服器支援 TLS 1.2 和 TLS 連線的交涉的加密套件，將會支援完整轉寄密碼。

您可以啟用和停用 SSL 2.0、 3.0 和 TLS 版本 1.0、 1.1 及 1.2 使用[管理 AD FS 中的 SSL 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)。

若要確保您的 AD FS 和 WAP 伺服器交涉只支援 ATP 的 TLS 加密套件，您可以停用所有的加密套件不在[ATP 符合規範的加密套件的清單](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)。  若要這樣做，請使用[Windows TLS PowerShell cmdlet](https://technet.microsoft.com/itpro/powershell/windows/tls/index)。

## <a name="developer"></a>開發人員

### <a name="when-generating-an-idtoken-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-idtoken"></a>產生針對 AD 驗證的使用者使用 ADFS id_token 時, 如何"sub"的宣告產生的 id_token 中？
"Sub"宣告的值是用戶端識別碼的雜湊 + 錨點宣告值。

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>什麼是重新整理語彙基元/存取權杖的存留期，當使用者透過登入遠端的宣告提供者信任透過 WS-27001/27002、fed/SAML-P？
重新整理權杖的存留期會從遠端的宣告提供者信任的 ADFS 取得權杖的存留期。 存取權杖的存留期會為其發出存取權杖的信賴憑證者合作對象的權杖存留期。

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>我要傳回設定檔和電子郵件的範圍，以及除了 OpenId 範圍。 我可以取得使用領域的其他資訊？ 如何在 AD FS 中的作業？
您可以使用自訂的 id_token 以新增在本身的 id_token 中的相關資訊。 如需詳細資訊，請參閱文章[自訂要在 id_token 中發出的宣告](../development/Custom-Id-Tokens-in-AD-FS.md)。

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>如何將發出的 JWT 權杖內的 json blob？
特殊的 ValueType (「 http://www.w3.org/2001/XMLSchema#json") 和逸出 character(\x22)，這已加入 AD FS 2016。 請發行規則以及存取權杖的最終輸出的範例如下。

範例發行規則：

    => issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");

存取權杖中發出的宣告：

    "array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>可以傳遞的資源值的範圍值，例如如何針對 Azure AD 完成要求的一部分？
使用 AD FS 伺服器 2019年上，您現在可以傳遞內嵌在 scope 參數中的資源值。 範圍參數現在可以按照空格分隔的清單，其中每個項目是資源/範圍的結構。 例如：  
**< 建立有效的範例要求 >**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS 支援 PKCE 延伸模組？
AD FS 伺服器 2019年中的支援證明金鑰的程式碼 Exchange (PKCE) OAuth 授權碼授與流程

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>AD FS 支援哪些允許的範圍？
- aza-如果使用[Broker 的用戶端的 OAuth 2.0 通訊協定延伸](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706)和 scope 參數會包含"aza"的範圍，如果伺服器發出新的主要重新整理權杖，並將它設定 refresh_token 的欄位中的回應，以及設定如果其中一個會強制執行的新主要重新整理權杖的存留期 refresh_token_expires_in 欄位。
- openid-可讓應用程式要求使用 OpenID Connect 授權通訊協定。
- logon_cert-logon_cert 範圍可讓應用程式要求登入憑證，可用來以互動方式登入已驗證的使用者。 在 AD FS 伺服器會省略來自回應的 access_token 參數，並改為提供 base64 編碼的 CMS 憑證鏈結或 CMC 完整 PKI 的回應。 提供的更多詳細資料[此處](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)。 
- user_impersonation-user_impersonation 範圍，才能成功從 AD FS 要求代表的存取權杖。 如需如何使用此範圍的詳細資訊，請參閱[建置多層式應用程式使用代理者 (OBO) 使用 OAuth 與 AD FS 2016](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md)。
- vpn_cert-vpn_cert 範圍可讓要求 VPN 憑證，可用來建立 VPN 連線使用 EAP-TLS 驗證的應用程式。 這不再支援。
- 電子郵件-可讓應用程式要求登入使用者的電子郵件宣告。 這不再支援。 
- 設定檔-可讓應用程式要求設定檔相關的登入使用者的宣告。 這不再支援。 


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何取代 AD FS 的 SSL 憑證？
AD FS SSL 憑證不是在 AD FS 管理嵌入式管理單元中找到的 AD FS 服務通訊憑證相同。  若要變更 AD FS SSL 憑證，您必須使用 PowerShell。 請遵循下列文章中的指導方針：

[管理 AD FS 和 WAP 2016 中的 SSL 憑證](../operations/Manage-SSL-Certificates-AD-FS-WAP-2016.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何啟用或停用 TLS/SSL 設定，適用於 AD FS
若要停用或啟用 SSL 通訊協定和加密套件，使用下列方法：

[管理 AD FS 中的 SSL 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Proxy 的 SSL 憑證是否必須是相同 AD FS SSL 憑證？  
使用 proxy 的 SSL 憑證和 AD FS SSL 憑證的下列指導方針：


- 如果使用 proxy 來使用 Windows 整合式驗證的 proxy 的 SSL 憑證的 AD FS proxy 要求必須相同 （使用相同的索引鍵） 做為同盟伺服器 SSL 憑證
- 如果 「 ExtendedProtectionTokenCheck 」 是 AD FS 屬性啟用 （AD FS 中設定的預設值），proxy 的 SSL 憑證必須是相同 （使用相同的索引鍵） 做為同盟伺服器 SSL 憑證
- Proxy 的 SSL 憑證，否則可以有不同的金鑰從 AD FS SSL 憑證，但必須符合相同[需求](../overview/AD-FS-2016-Requirements.md)

### <a name="how-can-i-configure-promptlogin-behavior-for-ad-fs"></a>如何設定提示適用於 AD FS = 登入行為？
如需如何設定提示字元 = 登入，請參閱[Active Directory Federation Services 提示 = 登入參數支援](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-change-the-ad-fs-service-account"></a>如何變更 AD FS 服務帳戶？
若要變更 AD FS 服務帳戶，請依照下列指示使用 AD FS 工具箱[服務帳戶的 Powershell 模組](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)。 

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何設定與 AD FS 搭配使用 Windows 整合式驗證 (WIA) 的瀏覽器？

如需如何設定瀏覽器的詳細資訊，請參閱[設定與 AD FS 搭配使用 Windows 整合式驗證 (WIA) 的瀏覽器](../operations/Configure-AD-FS-Browser-WIA.md)。

### <a name="can-i-trun-off-browserssoenabled"></a>我可以關閉 BrowserSsoEnabled trun 嗎？
如果您沒有使用 ADFS; 的商業憑證註冊的裝置上 ADFS 或 Windows Hello 為基礎的存取控制原則您可以關閉 BrowserSsoEnabled。 BrowserSsoEnabled 可讓您從用戶端，其中包含裝置資訊收集 PRT （主要重新整理權杖） 的 ADFS。 沒有該裝置的 ADFS 驗證將無法在 Windows 10 裝置上。

### <a name="how-long-are-ad-fs-tokens-valid"></a>時間長度是有效的 AD FS 權杖？

此問題通常表示 '多久執行使用者取得單一登入 (SSO) 而不需要輸入新的認證，以及如何身為系統管理員控制的？ 」  此行為，以及控制，組態設定本文所述[AD FS 的單一登入設定](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/ad-fs-2016-single-sign-on-settings)。

以下列出各種 cookie 和權杖預設存留時間 （以及管理存留期的參數）：

**已註冊的裝置**

- PRT 和 SSO cookie:最大的 90 天由 PSSOLifeTimeMins 所控制。 （提供的裝置使用至少每 14 天，這會受到 DeviceUsageWindow）

- 重新整理權杖： 計算是根據上述提供一致的行為

- access_token:根據預設，根據信賴憑證者的合作對象的 1 小時

- id_token： 相同的存取權杖

**取消註冊的裝置**

- SSO cookie:根據預設，受到 SSOLifetimeMins 8 小時。  啟用我保持登入 (KMSI) 時，預設值是 24 小時內且可透過 KMSILifetimeMins 設定。


- 重新整理權杖：預設為 8 小時。 24 小時內已啟用 KMSI

- access_token:根據預設，根據信賴憑證者的合作對象的 1 小時

- id_token： 相同的存取權杖

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 支援 HTTP Strict Transport Security (HSTS)？  

HTTP Strict Transport Security (HSTS) 是 web 安全性原則機制，可協助降低通訊協定降級攻擊和 cookie 攔截有 HTTP 和 HTTPS 端點的服務。 它可讓 web 伺服器，來宣告，網頁瀏覽器 （或其他 complying 使用者代理程式） 應該僅互動與它使用 HTTPS，並永遠不會透過 HTTP 通訊協定。

適用於 web 驗證流量的所有 AD FS 端點會透過 HTTPS 以獨佔方式都開啟。  如此一來，AD FS 有效地降低 HTTP Strict Transport Security 原則機制提供的威脅 （依設計沒有任何降級至 HTTP 由於 HTTP 有沒有接聽程式）。 此外，AD FS 可防止 cookie 傳送至另一部伺服器，透過 HTTP 通訊協定端點來標記使用安全的旗標的所有 cookie。

因此，AD FS 伺服器上實作 HSTS 不需要因為它永遠不會降級。  基於合規性時，AD FS 伺服器都符合這些需求，因為它們可以永遠不會使用 HTTP，而且所有的 cookie 會標示為安全。

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-轉送-用戶端-ip 不包含用戶端的 IP，但包含 proxy 前面的防火牆的 IP。 哪裡可以取得正確的用戶端的 IP？
不建議執行之前 WAP 的 SSL 終止。 如果 SSL 終止完成 WAP 前面，X-ms-轉送-用戶端-ip 會包含 WAP 前面之網路裝置的 IP。 以下是各種不同的 IP 的簡短描述相關的 AD FS 所支援的宣告：
 - x-ms-用戶端-ip:網路裝置連接至 STS 的 IP。  在外部網路的要求的情況下這一定會包含 WAP 的 IP。
 - x-ms-轉送-用戶端-ip:多重值的宣告會包含任何資訊轉送至 ADFS Exchange Online 加上裝置連接到 WAP 的 IP 位址的值。
 - Userip:外部網路的要求此宣告會包含 x-ms-轉送-用戶端-ip 的值。  對於內部網路的要求，此宣告會包含相同的值做為 x ms-用戶端 ip。

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我嘗試取得使用者資訊端點，但其唯一傳回的其他宣告的主旨。 如何取得額外宣告？
ADFS userinfo 端點一律會傳回 OpenID 標準中所指定的主體宣告。 AD FS 不提供透過 UserInfo 端點要求的其他宣告。 如果您需要在 ID 權杖中的其他宣告，請參閱[AD FS 中自訂識別碼權杖](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>為什麼我的 AD FS 伺服器上看到許多 1021年錯誤？
無效的資源上存取 AD FS 資源 00000003-0000-0000-c000-000000000000 通常會記錄此事件。 此錯誤被因為它嘗試取得存取權杖的 Azure AD Graph 服務的用戶端的錯誤行為。 因為資源不存在於 AD FS 上，這會導致 AD FS 伺服器上的事件識別碼 1021年。 它可以安全地忽略任何警告或錯誤的 AD FS 上的資源 00000003-0000-0000-c000-000000000000。

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>為何我會看見警告，以將 AD FS 服務帳戶新增至 Enterprise Key Admins 群組的失敗？
只有在具有 PDC FSMO 角色的 Windows 2016 網域控制站存在網域中時，會建立此群組。 若要解決此錯誤，您可以手動建立群組，依照以下提供必要的權限之後將服務帳戶新增為群組的成員。
1.  開啟 [Active Directory 使用者和電腦]  。
2.  **以滑鼠右鍵按一下**您的網域名稱，從 [導覽] 窗格和**按一下**屬性。
3.  **按一下**安全性 （如果 安全性 索引標籤，開啟進階功能檢視 功能表中）。
4.  **按一下**進階。 **按一下**新增。 **按一下**選取一個主體。
5.  [選取使用者、 電腦、 服務帳戶或群組] 對話方塊隨即出現。  在 [輸入物件名稱來選取] 文字方塊中，輸入金鑰系統管理員群組。  按一下 [確定]。
6.  在 套用至清單方塊中，選取**子系 User 使用者**。
7.  使用捲軸，捲動到頁面底部並**按一下**全部清除。
8.  在 **屬性**區段中，選取**讀取 Msds-keycredentiallink**並**撰寫 Msds-keycredentiallink**。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>為什麼從 Android 裝置的新式驗證失敗如果伺服器不會傳送的 SSL 憑證鏈結中的所有中繼憑證？

同盟的使用者可能會遇到使用 Android ADAL 程式庫失敗的應用程式的 Azure ad 的驗證。 應用程式會收到**AuthenticationException**當它嘗試顯示登入頁面。 在 chrome 中的 AD FS 登入頁面可能會叫出為不安全。

Android-所有版本和所有裝置-不支援下載額外的憑證，從**authorityInformationAccess**憑證欄位。 這是 Chrome 瀏覽器的則為 true。 如果整個憑證鏈結不會從 AD FS，任何遺失中繼憑證的伺服器驗證憑證會導致此錯誤。

此問題的適當解決方案是設定 AD FS 和 WAP 伺服器來傳送必要的中繼憑證，以及 SSL 憑證。

當匯出 SSL 憑證，從一部電腦，若要匯入到電腦的個人存放區，AD FS 和 WAP 伺服器，請務必匯出私密金鑰並選取**個人資訊交換-PKCS #12**。

很重要的核取方塊**盡可能包含憑證路徑中的所有憑證**勾選，以及**匯出所有延伸的內容**。  

在 Windows 伺服器上執行 certlm.msc 並匯入 *。電腦的個人憑證存放區的 PFX。 這會導致將整個憑證鏈結至 ADAL 程式庫伺服器。

>[!NOTE]
> 憑證存放區的網路負載平衡器也應該更新以包含整個憑證鏈結，如果有的話

### <a name="does-ad-fs-support-head-requests"></a>AD FS 支援 HEAD 要求？
AD FS 不支援 HEAD 要求。  應用程式應該不會使用針對 AD FS 端點的 HEAD 要求。  這可能會導致非預期和/或延遲的 HTTP 錯誤回應。  此外，您可能會看到 AD FS 事件記錄檔中的未預期的錯誤事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>為什麼我看不到重新整理權杖時我登入遠端 IdP？
IdP 所簽發的權杖是否少於 1 小時的 validty，不會發出重新整理權杖。 若要確保簽發重新整理權杖時，增加到超過 1 小時 IdP 所簽發權杖的有效性。

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>我們有任何方式來變更 RP 的權杖加密演算法嗎？
根據預設，RP 的權杖加密設 AES256 和無法變更為其他任何值。

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>混合式伺服器陣列上出現錯誤時嘗試設定新的 SSL 憑證使用 Set-adfssslcertificate-憑證指紋。 如何更新在混合模式中 AD FS 伺服器陣列的 SSL 憑證？
您仍然可以使用組 WebApplicationProxySslCertificate WAP 伺服器上。 在 ADFS 伺服器，您需要使用 netsh。 請遵循以下指定的步驟：

1. 選取小組維護的 ADFS 2016 的伺服器 （例如移除負載平衡器）
2. #1 中所選取的伺服器，在匯入新的憑證，透過 MMC
3. 刪除現有的憑證

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443 b。 netsh http delete sslcert hostnameport = localhost:443 c。 netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  加入新的憑證

    a. netsh http 新增 sslcert hostnameport=fs.contoso.com:443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=CERTTHUMBPRINT appid={5d89a20c-beab-4389-9447-324788eb944a} certstorename=MY sslctlstorename=AdfsTrustedDevices

    c.  netsh http 新增 sslcert hostnameport=fs.contoso.com:49443 certhash = CERTTHUMBPRINT appid = {5d89a20c-beab-4389-9447-324788eb944a} certstorename = MY sslctlstorename = AdfsTrustedDevices

5. 重新啟動選取的伺服器上的 ADFS 服務
6. 移除維護的 WAP 伺服器的子集
7. 選取的 WAP 伺服器，在匯入新的憑證，透過 MMC
8. 使用 cmdlet 的 WAP 上設定新的憑證

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. 重新啟動選取的 WAP 伺服器上的服務
10. 將選取的 WAP 和 AD FS 伺服器放回生產環境中。

其餘的 AD FS 和 WAP 伺服器，以類似的方式執行更新。

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>ADFS 時，支援 Web 應用程式 Proxy (WAP) 伺服器位於 Azure Web 應用程式 Firewall(WAF) 嗎？
ADFS 和 Web 應用程式伺服器支援不會在端點執行 SSL 終止的任何防火牆。 此外，ADFS/WAP 伺服器有內建的機制，以防止跨站台指令碼、 ADFS proxy 的常見 web 攻擊並滿足所定義的所有需求[MS ADFSPIP 通訊協定](https://msdn.microsoft.com/library/dn392811.aspx)。
