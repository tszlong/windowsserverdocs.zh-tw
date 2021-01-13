---
ms.assetid: acc9101b-841c-4540-8b3c-62a53869ef7a
title: AD FS 常見問題集
description: AD FS 常見問題集
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/02/2020
ms.topic: article
ms.openlocfilehash: 94b5a3e188567875332f05f378ab3ebf8a84336f
ms.sourcegitcommit: 29b8942ea46196c12a67f6b6ad7f8dd46bf94fb2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/11/2021
ms.locfileid: "98065644"
---
# <a name="ad-fs-frequently-asked-questions-faq"></a>AD FS 常見問題集 (FAQ)

以下文件是有關 Active Directory 同盟服務的常見問題集首頁。  該文件已根據問題類型分成數個群組。

## <a name="deployment"></a>部署

### <a name="how-can-i-upgrademigrate-from-previous-versions-of-ad-fs"></a>如何從舊版 AD FS 進行升級/遷移

您可以使用下列其中一項來升級 AD FS：

- Windows Server 2012 R2 AD FS 至 Windows Server 2016 AD FS 或更新版本。 請注意，如果您從 Windows Server 2016 AD FS 升級至 Windows Server 2019 AD FS，則方法相同。
    - [升級至使用 WID 資料庫的 Windows Server 2016 AD FS](../deployment/upgrading-to-ad-fs-in-windows-server.md)
    - [升級至使用 SQL 資料庫的 Windows Server 2016 AD FS](../deployment/upgrading-to-ad-fs-in-windows-server-sql.md)
- Windows Server 2012 AD FS 升級至 Windows Server 2012 R2 AD FS
    - [遷移到 Windows Server 2012 R2 上的 AD FS](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486815(v=ws.11))
- AD FS 2.0 至 Windows Server 2012 AD FS
    - [遷移到 Windows Server 2012 上的 AD FS](../deployment/migrate-ad-fs-role-services-to-windows-server-2012.md)
- AD FS 1.x 至 AD FS 2.0
    - [從 AD FS 1.x 升級至 AD FS 2.0](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff678035(v=ws.10))

如果您需要從 AD FS 2.0 或 2.1 (Windows Server 2008 R2 或 Windows Server 2012) 升級，則必須使用內建指令碼 (位於 C:\Windows\ADFS)。

### <a name="why-does-ad-fs-installation-require-a-reboot-of-the-server"></a>AD FS 安裝為何需要重新啟動伺服器？

Windows Server 2016 已新增 HTTP/2 支援，但 HTTP/2 無法用於用戶端憑證驗證。  因為許多 AD FS 案例都會使用用戶端憑證驗證，且大量用戶端不支援使用 HTTP/1.1 來重試要求，所以 AD FS 伺服器陣列設定會將本機伺服器的 HTTP 設定重新設定為 HTTP/1.1。  這需要重新啟動伺服器。

### <a name="is-using-windows-2016-wap-servers-to-publish-the-ad-fs-farm-to-the-internet-without-upgrading-the-back-end-ad-fs-farm-supported"></a>使用 Windows 2016 WAP 伺服器將 AD FS 伺服器陣列發佈到網際網路，但不升級所支援的後端 AD FS 伺服器陣列嗎？
是，支援此設定，但是此設定不會支援新的 AD FS 2016 功能。  此設定在從 AD FS 2012 R2 至 AD FS 2016 的移轉階段應該是暫時的，不得長時間部署。

### <a name="is-it-possible-to-deploy-ad-fs-for-office-365-without-publishing-a-proxy-to-office-365"></a>是否可能部署適用於 Office 365 的 AD FS，而不需將 Proxy 發佈到 Office 365？
是，支援這種情況。 不過，副作用如下：

1. 您需要手動管理更新權杖簽署憑證，因為 Azure AD 將無法存取同盟中繼資料。 如需有關手動更新權杖簽署憑證的詳細資訊，請參閱[更新 Office 365 和 Azure Active Directory 的同盟憑證](/azure/active-directory/connect/active-directory-aadconnect-o365-certs)
2. 您將無法利用舊版驗證流程 (例如 ExO Proxy 驗證流程)

### <a name="what-are-load-balancing-requirements-for-ad-fs-and-wap-servers"></a>AD FS 與 WAP 伺服器的負載平衡需求為何？

AD FS 是無狀態系統。 因此，登入的負載平衡相當簡單。 以下是負載平衡系統的重要建議。


- 負載平衡器不得設定 IP 親和性。 在某些 Exchange Online 案例中，這可能會對部分的伺服器造成過度負載。
- 負載平衡器不得終止 HTTPS 連線並重新起始對 ADFS 伺服器的新連線。
- 負載平衡器應確保正在連線的 IP 位址在傳送至 ADFS 時，應該轉譯為 HTTP 封包中的來源 IP。 如果負載平衡器無法傳送 HTTP 封包中的來源 IP，負載平衡器就必須將 IP 位址新增 (或在現有的情況下附加) 至 x-forwarded-for 標頭。 若要正確處理特定 IP 相關功能 (禁用的 IP、外部網路智慧鎖定...)，這是必要做法，若設定不正確，則可能導致安全性降低。
- 負載平衡器應支援 SNI。 如果情況並非如此，請確定已將 AD FS 設定為建立 HTTPS 繫結，以處理不支援 SNI 的用戶端。
- 負載平衡器應該使用 AD FS HTTP 健康情況探查端點來偵測 AD FS 或 WAP 伺服器是否已順利運作，如果未傳回 200 [確定]，則將其排除。

### <a name="what-multi-forest-configurations-are-supported-by-ad-fs"></a>AD FS 支援哪些多樹系設定？

AD FS 支援多樹系設定，且依賴基礎 AD DS 信任網路來驗證橫跨多個受信任領域的使用者。 我們強烈建議使用雙向樹系信任，因為這是比較簡單的設定，可確保信任子系統正常運作，而不會發生問題。 此外：

- 在單向樹系信任的情況下 (例如包含合作夥伴身分識別的 DMZ 樹系)，我們建議在 corp 樹系中部署 ADFS，並將 DMZ 樹系視為另一個透過 LDAP 連線的本機宣告提供者信任。 在此情況下，Windows 整合式驗證將無法用於 DMZ 樹系使用者，而他們必須執行密碼驗證，因為這是 LDAP 唯一支援的機制。 如果您無法採用此選項，則需要在 DMZ 樹系中設定另一個 ADFS，並在 corp 樹系的 ADFS 中將其新增為「宣告提供者信任」。 使用者必須進行主領域探索，但 Windows 整合式驗證和密碼驗證都會正常執行。 請在 DMZ 樹系中對 ADFS 發行規則進行適當的變更，因為 corp 樹系中的 ADFS 將無法從 DMZ 樹系取得有關使用者的額外使用者資訊。
- 雖然支援網域層級信任並可運作，但我們強烈建議您移至樹系層級信任模型。 此外，您還需要確保名稱的 UPN 路由和 NETBIOS 名稱解析必須能夠正確地運作。

>[!NOTE]
>如果選用驗證搭配雙向信任設定使用，請確定呼叫端使用者已獲得目標服務帳戶的「允許驗證」權限。

### <a name="does-ad-fs-extranet-smart-lockout-support-ipv6"></a>AD FS 外部網路智慧鎖定是否支援 IPv6？
是，您可以考慮為熟悉/未知的位置使用 IPv6 位址。


## <a name="design"></a>設計

### <a name="what-third-party-multi-factor-authentication-providers-are-available-for-ad-fs"></a>AD FS 有哪些第三方多重要素驗證提供者可用？
AD FS 會提供一個可延伸的機制，以供第三方 MFA 提供者進行整合。 並沒有固定的認證方案。 在發行之前，假定廠商已執行必要的驗證。

已通知 Microsoft 的廠商清單會發佈於 [AD FS 的 MFA 提供者](../operations/Configure-Additional-Authentication-Methods-for-AD-FS.md)。  永遠都可能有我們不知道的提供者可用，我們將會在得知時更新清單。

### <a name="are-third-party-proxies-supported-with-ad-fs"></a>AD FS 是否支援第三方 Proxy？
是，第三方 Proxy 可以放在 AD FS 前面，但所有第三方 Proxy 都必須支援 [ 通訊協定](/openspecs/windows_protocols/ms-adfspip/76deccb1-1429-4c80-8349-d38e61da5cbb)，才能用來取代 Web 應用程式 Proxy。

以下是我們所知道的第三方提供者清單。  永遠都可能有我們不知道的提供者可用，我們將會在得知時更新清單。

- [F5 存取原則管理員](https://support.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-third-party-integration-13-1-0/12.html#guid-1ee8fbb3-1b33-4982-8bb3-05ae6868d9ee)
- [Citrix 應用程式傳遞控制器 (ADC)](https://docs.citrix.com/en-us/citrix-adc/current-release/aaa-tm/adfs-proxy-wsfed.html)


### <a name="where-is-the-capacity-planning-sizing-spreadsheet-for-ad-fs-2016"></a>AD FS 2016 的容量規劃大小調整試算表在哪裡？
您可以在[這裡](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)下載 AD FS 2016 版的試算表。
這份試算表也可用於 Windows Server 2012 R2 中的 AD FS。

### <a name="how-can-i-ensure-my-ad-fs-and-wap-servers-support-apples-atp-requirements"></a>如何確保我的 AD FS 和 WAP 伺服器能支援 Apple 的 ATP 需求？

Apple 發行了一組稱為 App Transport Security (ATS) 的需求，其可能影響來自向 AD FS 驗證之 iOS 應用程式的呼叫。  只要確定 AD FS 和 WAP 伺服器都支援[使用 ATS 進行連線的需求](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)，即可確保這兩部伺服器相符。
尤其是，您應確認 AD FS 和 WAP 伺服器都支援 TLS 1.2，而且 TLS 連線的交涉加密套件將會支援完整轉寄密碼。

您可以使用[管理 AD FS 中的 SSL 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)，啟用及停用 SSL 2.0 和 3.0 以及 TLS 1.0、1.1 和1.2 版。

若要確保 AD FS 和 WAP 伺服器只會交涉可支援 ATP 的 TLS 加密套件，您可停用不在 [ATP 相容加密套件清單](https://developer.apple.com/library/prerelease/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW57)中的所有加密套件。  若要這麼做，請使用 [Windows TLS PowerShell Cmdlet](/powershell/module/tls/)。

## <a name="developer"></a>Developer

### <a name="when-generating-an-id_token-with-adfs-for-a-user-authenticated-against-ad-how-is-the-sub-claim-generated-in-the-id_token"></a>針對向 AD 驗證的使用者產生 ADFS 的 id_token 時，如何在 id_token 中產生「子」宣告？
「子」宣告的值是用戶端識別碼 + 錨點宣告值的雜湊。

### <a name="what-is-the-lifetime-of-the-refresh-tokenaccess-token-when-the-user-logs-in-via-a-remote-claims-provider-trust-over-ws-fedsaml-p"></a>當使用者透過 WS-Fed/SAML-P 的遠端宣告提供者信任登入時，重新整理權杖/存取權杖的存留期為何？
重新整理權杖的存留期會是 ADFS 從遠端宣告提供者信任所取得權杖的存留期。 存取權杖的存留期會是對其發行存取權杖之信賴憑證者的權杖存留期。

### <a name="i-need-to-return-profile-and-email-scopes-as-well-in-addition-to-the-openid-scope-can-i-obtain-additional-information-using-scopes-how-to-do-it-in-ad-fs"></a>除了 OpenId 範圍以外，我還需要傳回設定檔和電子郵件範圍。 我可以使用範圍取得其他資訊嗎？ 如何在 AD FS 中執行此動作？
您可以使用自訂的 id_token，在 id_token 本身新增相關資訊。 如需詳細資訊，請參閱[自訂要在 id_token 中發出的宣告](../development/Custom-Id-Tokens-in-AD-FS.md)一文。

### <a name="how-to-issue-json-blobs-inside-jwt-tokens"></a>如何在 JWT 權杖內發行 json blob？
AD FS 2016 中已新增特殊的 ValueType ("<http://www.w3.org/2001/XMLSchema#json>") 及其逸出字元 (\x22)。 請參閱下面的發行規則範例，以及來自存取權杖的最終輸出。

發行規則範例：

```
=> issue(Type = "array_in_json", ValueType = "http://www.w3.org/2001/XMLSchema#json", Value = "{\x22Items\x22:[{\x22Name\x22:\x22Apple\x22,\x22Price\x22:12.3},{\x22Name\x22:\x22Grape\x22,\x22Price\x22:3.21}],\x22Date\x22:\x2221/11/2010\x22}");
```

在存取權杖中發出的宣告：

```
"array_in_json":{"Items":[{"Name":"Apple","Price":12.3},{"Name":"Grape","Price":3.21}],"Date":"21/11/2010"}
```

### <a name="can-i-pass-resource-value-as-part-of-the-scope-value-like-how-requests-are-done-against-azure-ad"></a>我可以將資源值當作範圍值的一部分傳遞，如同對 Azure AD 進行要求一樣？
使用 Server 2019 上的 AD FS，您現在可以傳遞範圍參數中內嵌的資源值。 範圍參數現在可以組織成以空格分隔的清單，其中的每個項目都會建構為資源/範圍。 例如， **<建立有效的範例要求>**

### <a name="does-ad-fs-support-pkce-extension"></a>AD FS 是否支援 PKCE 擴充功能？
Server 2019 中的 AD FS 支援 OAuth 授權碼授與流程的 Proof Key for Code Exchange (PKCE)

### <a name="what-permitted-scopes-are-supported-by-ad-fs"></a>AD FS 支援哪些允許的範圍？
- aza - 如果使用[適用於代理人用戶端的 OAuth 2.0 通訊協定擴充功能](/openspecs/windows_protocols/ms-oapxbc/2f7d8875-0383-4058-956d-2fb216b44706)，而且範圍參數包含 "aza" 範圍，則伺服器會發行新的主要重新整理權杖，並在回應的 refresh_token 欄位中加以設定，以及將 refresh_token_expires_in 欄位設定為新主要重新整理權杖的存留期 (如果強制執行主要重新整理權杖)。
- openid - 可讓應用程式要求使用 OpenID Connect 授權通訊協定。
- logon_cert - logon_cert 範圍可讓應用程式要求登入憑證，以便用來以互動方式登入已經過驗證的使用者。 AD FS 伺服器會省略回應中的 access_token 參數，並改為提供 base64 編碼的 CMS 憑證鏈或 CMC 完整 PKI 回應。 [這裡](/openspecs/windows_protocols/ms-oapx/32ce8878-7d33-4c02-818b-6c9164cc731e)可取得更多詳細資訊。
- user_impersonation - 必須有 user_impersonation 範圍，才能成功向 AD FS 要求代理者存取權杖。 如需此範圍使用方式的詳細資訊，請參閱[透過使用 OAuth 搭配 AD FS 2016 的代理者 (OBO) 建置多層式應用程式](../../ad-fs/development/ad-fs-on-behalf-of-authentication-in-windows-server.md)。
- vpn_cert - vpn_cert 範圍可讓應用程式要求 VPN 憑證，其可用來建立使用 EAP-TLS 驗證的 VPN 連線。 不再支援此範圍。
- 電子郵件 - 可讓應用程式要求已登入使用者的電子郵件宣告。 不再支援此範圍。
- 設定檔 - 可讓應用程式要求已登入使用者的設定檔相關宣告。 不再支援此範圍。


## <a name="operations"></a>操作

### <a name="how-do-i-replace-the-ssl-certificate-for-ad-fs"></a>如何取代 AD FS 的 SSL 憑證？
AD FS SSL 憑證與 AD FS 管理嵌入式管理單元中找到的 AD FS 服務通訊憑證不同。  若要變更 AD FS SSL 憑證，您必須使用 PowerShell。 遵循以下文章中的指導方針：

[管理 AD FS 和 WAP 2016 中的 SSL 憑證](../operations/manage-ssl-certificates-ad-fs-wap.md)

### <a name="how-can-i-enable-or-disable-tlsssl-settings-for-ad-fs"></a>如何啟用或停用 AD FS 的 TLS/SSL 設定
若要停用或啟用 SSL 通訊協定和加密套件，請使用下列各項：

[管理 AD FS 中的 SSL 通訊協定](../operations/Manage-SSL-Protocols-in-AD-FS.md)

### <a name="does-the-proxy-ssl-certificate-have-to-be-the-same-as-the-ad-fs-ssl-certificate"></a>Proxy SSL 憑證必須與 AD FS SSL 憑證相同嗎？
請使用下列關於 Proxy SSL 憑證和 AD FS SSL 憑證的指導方針：


- 如果 Proxy 用來代理使用 Windows 整合式驗證的 AD FS 要求，則 Proxy SSL 憑證必須與同盟伺服器 SSL 憑證相同 (使用相同金鑰)
- 如果已啟用 AD FS 屬性 "ExtendedProtectionTokenCheck" (AD FS 中的預設設定)，則 Proxy SSL 憑證必須與同盟伺服器 SSL 憑證相同 (使用相同金鑰)
- 否則，Proxy SSL 憑證可有來自 AD FS SSL 憑證的不同金鑰，但必須符合相同的[需求](./ad-fs-requirements.md)。

### <a name="why-do-i-only-see-a-password-login-on-ad-fs-and-not-my-other-authentication-methods-that-i-have-configured"></a>為何我在 AD FS 上只看到密碼登入，而不是我已設定的其他驗證方法？
當應用程式明確要求的特定驗證 URI 對應至已設定並啟用的驗證方法時，AD FS 只會在登入畫面中顯示單一驗證方法。 這會在 WS-同盟要求的 'wauth' 參數和 SAML 通訊協定要求中的 'RequestedAuthnCtxRef' 參數中傳達。 因此，只會顯示所要求的驗證方法 (例如密碼登入)。

當 AD FS 搭配 Azure AD 使用時，應用程式通常會將 prompt=login 參數傳送至 Azure AD。 Azure AD 預設會將此轉譯為要求以全新的密碼登入 AD FS。 這是在您的網路內部 AD FS 上看見密碼登入，而不是使用憑證登入選項的最常見原因。 變更 Azure AD 中的同盟網域設定，即可輕鬆地補救這點。

如需有關如何進行此設定的詳細資訊，請參閱 [Active Directory 同盟服務 prompt=login 參數支援](../operations/AD-FS-Prompt-Login.md)。

### <a name="how-can-i-change-the-ad-fs-service-account"></a>如何才能變更 AD FS 服務帳戶？
若要變更 AD FS 服務帳戶，請遵循使用 AD FS 工具箱[服務帳戶 Powershell 模組](https://github.com/Microsoft/adfsToolbox/tree/master/serviceAccountModule)的相關指示。

### <a name="how-can-i-configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>如何將瀏覽器設定為使用 Windows 整合式驗證 (WIA) 搭配 AD FS？

如需如何設定瀏覽器的詳細資訊，請參閱[將瀏覽器設定為使用 Windows 整合式驗證 (WIA) 搭配 AD FS](../operations/Configure-AD-FS-Browser-WIA.md)。

### <a name="can-i-turn-off-browserssoenabled"></a>我可以關閉 BrowserSsoEnabled 嗎？
如果您沒有以 ADFS 上的裝置為基礎的存取控制原則，或使用 ADFS 的 Windows Hello 企業版憑證註冊，您可以關閉 BrowserSsoEnabled。 BrowserSsoEnabled 可讓 ADFS 向包含裝置資訊的用戶端收集 PRT (主要重新整理權杖)。 若沒有該裝置驗證，ADFS 將無法在 Windows 10 裝置上運作。

### <a name="how-long-are-ad-fs-tokens-valid"></a>AD FS 權杖的有效期限是多久？

此問題通常是指「使用者多久才能取得單一登入 (SSO)，而不必輸入新的認證，以及我如何才能以系統管理員的身分進行控制？」  [AD FS 單一登入設定](../operations/ad-fs-single-sign-on-settings.md)一文會說明此行為，以及控制該行為的組態設定。

以下列出各種 Cookie 和權杖的預設存留期 (以及可控管存留期的參數)：

**已註冊的裝置**

- PRT 和 SSO Cookie：最常 90 天 (受 PSSOLifeTimeMins 控管)。 (提供的裝置至少每隔 14 天使用一次，其由 DeviceUsageWindow 控制)

- 重新整理權杖：根據上述描述計算以提供一致的行為

- access_token：預設為 1 小時 (根據信賴憑證者)

- id_token：與存取權杖相同

**已取消註冊的裝置**

- SSO Cookie：預設為8小時 (受 SSOLifetimeMins 控管)。  若已啟用 [讓我保持登入 (KMSI)]，預設值為 24 小時，並可透過 KMSILifetimeMins 進行設定。


- 重新整理權杖：預設為 8 小時。 24小時 (已啟用 KMSI)

- access_token：預設為 1 小時 (根據信賴憑證者)

- id_token：與存取權杖相同

### <a name="does-ad-fs-support-http-strict-transport-security-hsts"></a>AD FS 是否支援 HTTP Strict Transport Security (HSTS)？

HTTP Strict Transport Security (HSTS) 是一種 Web 安全性原則機制，可協助同時具有 HTTP 和 HTTPS 端點的服務減輕通訊協定降級攻擊和 Cookie 劫持。 其可讓網頁伺服器宣告：網頁瀏覽器 (或其他相符的使用者代理程式) 只能使用 HTTPS (而不能透過 HTTP 通訊協定) 與其互動。

Web 驗證流量的所有 AD FS 端點都會以獨佔方式透過 HTTPS 開啟。  因此，AD FS 會有效地減輕 HTTP Strict Transport Security 原則機制所提供的威脅 (依照設計，因為 HTTP 中沒有接聽程式，所以不會降級至 HTTP)。 此外，AD FS 會使用安全旗標來標記所有 Cookie，以防止 Cookie 傳送至另一部具有 HTTP 通訊協定端點的伺服器。

因此不需要在 AD FS 伺服器上實作 HSTS，因為其永遠不會降級。  基於合規性目的，AD FS 伺服器會符合這些需求，因為其絕對不能使用 HTTP，而且所有 Cookie 都會標示為安全的。

此外，AD FS 2016 (具有最新修補程式) 和 AD FS 2019 都支援發出 HSTS 標頭。 若要設定此項，請參閱[使用 AD FS 自訂 HTTP 安全性回應標頭](../operations/customize-http-security-headers-ad-fs.md)

### <a name="x-ms-forwarded-client-ip-does-not-contain-the-ip-of-the-client-but-contains-ip-of-the-firewall-in-front-of-the-proxy-where-can-i-get-the-right-ip-of-the-client"></a>X-ms-forwarded-client-ip 不包含用戶端的 IP，而包含 Proxy 前面的防火牆 IP。 哪裡可以取得正確的用戶端 IP？
不建議在 WAP 之前進行 SSL 終止。 如果在 WAP 前面完成 SSL 終止，則 X-ms-forwarded-client-ip 將包含 WAP 前面的網路裝置 IP。 以下是 AD FS 所支援的各種 IP 相關宣告的簡短描述：
 - x-ms-client-ip：連線到 STS 的裝置網路 IP。  在外部網路要求的情況下，其一律包含 WAP 的 IP。
 - x-ms-forwarded-client-ip：多重值宣告，其中會包含由 Exchange Online 轉送至 ADFS 的任何值，加上連線到 WAP 的裝置 IP 位址。
 - Userip：對於外部網路要求，此宣告會包含 x-ms-forwarded-client-ip 的值。  對於內部網路要求，此宣告會包含與 x-ms-client-ip 相同的值。

 此外，在 AD FS 2016 (具有最新修補程式) 和更高版本中，也支援擷取 x-forwarded-for 標頭。 未在第 3 層轉寄的任何負載平衡器或網路裝置 (已保留 IP)，都應該將連入的用戶端 IP 新增至業界標準 x-forwarded-for 標頭。

### <a name="i-am-trying-to-get-additional-claims-on-the-user-info-endpoint-but-its-only-returning-subject-how-can-i-get-additional-claims"></a>我正嘗試取得使用者資訊端點上的其他宣告，但其只會傳回主體。 如何取得其他宣告？
AD FS UserInfo 端點一律會傳回 OpenID 標準中所指定的主體宣告。 AD FS 不會提供透過 UserInfo 端點所要求的其他宣告。 如果您需要識別碼權杖中的其他宣告，請參閱 [AD FS 中的自訂識別碼權杖](../development/custom-id-tokens-in-ad-fs.md)。

### <a name="why-do-i-see-a-lot-of-1021-errors-on-my-ad-fs-servers"></a>為何我在 AD FS 伺服器上看到許多 1021 錯誤？
通常會針對 AD FS 上資源 00000003-0000-0000-c000-000000000000 的無效資源存取記錄此事件。 此錯誤的造成原因是用戶端在嘗試取得 Azure AD Graph 服務的存取權杖時發生錯誤行為。 由於資源不存在於 AD FS 上，因此導致 AD FS 伺服器出現事件識別碼 1021。 您可以放心地忽略 AD FS 上任何針對資源 00000003-0000-0000-c000-000000000000 的警告或錯誤。

### <a name="why-am-i-seeing-a-warning-for-failure-to-add-the-ad-fs-service-account-to-the-enterprise-key-admins-group"></a>為何我會看到無法將 AD FS 服務帳戶新增至 Enterprise Key Admins 群組的警告？
只有當網域中存在具有 FSMO PDC 角色的 Windows 2016 網域控制站時，才會建立此群組。 若要解決此錯誤，您可以手動建立群組，並遵循下列步驟，在將服務帳戶新增為群組成員之後，提供必要的權限。
1.    開啟 [Active Directory 使用者和電腦] 。
2.    在導覽窗格中您的網域名稱上 **按一下滑鼠右鍵**，然後按一下 [屬性]。
3.    按一下 [安全性] (如果沒看見 [安全性] 索引標籤，請從 [檢視] 功能表開啟 [進階功能])。
4.    按一下 [進階]。 按一下 [新增]。 按一下 [選取主體]。
5.    [選取使用者、電腦、服務帳戶或群組] 對話方塊隨即出現。  在 [輸入物件名稱來選取] 文字方塊中，鍵入 Key Admin Group。  按一下 [確定]。
6.    在 [適用對象] 清單方塊中，選取 [子系使用者物件]。
7.    使用捲軸捲動到頁面底部，然後按一下 [全部清除]。
8.    在 [屬性] 區段中，選取 [讀取 msDS-KeyCredentialLink] 和 [寫入 msDS-KeyCrendentialLink]。

### <a name="why-does-modern-authentication-from-android-devices-fail-if-the-server-does-not-send-all-the-intermediate-certificates-in-the-chain-with-the-ssl-cert"></a>當伺服器未使用 SSL 憑證傳送憑證鏈中的所有中繼憑證時，Android 裝置的新式驗證為何會失敗？

同盟使用者可能會遇到使用 Android ADAL 程式庫的應用程式向 Azure AD 驗證失敗。 此應用程式會在嘗試顯示登入頁面時，收到 **AuthenticationException**。 在 Chrome 中，AD FS 登入頁面可能會被宣稱「不安全」。

Android (橫跨所有版本和所有裝置) 不支援從憑證的 **authorityInformationAccess** 欄位下載其他憑證。 這也適用於 Chrome 瀏覽器。 如果未從 AD FS 傳遞整個憑證鏈，則任何缺少中繼憑證的伺服器驗證憑證都會導致此錯誤。

此問題的適當解決方式是設定 AD FS 和 WAP 伺服器，以傳送必要的中繼憑證與 SSL 憑證。

從一部電腦匯出 SSL 憑證時，若要匯入到 AD FS 和 WAP 伺服器的電腦個人存放區，請務必匯出私密金鑰，然後選取 [個人資訊交換 - PKCS #12]。

請務必勾選 [盡可能包含憑證路徑中的所有憑證] 核取方塊，以及 [匯出所有擴充屬性]。

在 Windows 伺服器上執行 certlm.msc，並將 *.PFX 匯入到電腦的個人憑證存放區中。 這會導致伺服器將整個憑證鏈傳遞至 ADAL 程式庫。

>[!NOTE]
> 網路負載平衡器的憑證存放區也應該更新為包含整個憑證連結 (如果有的話)

### <a name="does-ad-fs-support-head-requests"></a>AD FS 是否支援 HEAD 要求？
AD FS 不支援 HEAD 要求。  應用程式不應該對 AD FS 端點使用 HEAD 要求。  這可能會導致未預期和/或延遲的 HTTP 錯誤回應。  此外，您可能會在 AD FS 事件記錄中看到未預期的錯誤事件。

### <a name="why-am-i-not-seeing-a-refresh-token-when-i-am-logging-in-with-a-remote-idp"></a>當我使用遠端 IdP 登入時，為何看不到重新整理權杖？
如果 IdP 所發行權杖的有效期間少於 1 小時，則不會發行重新整理權杖。 若要確保已發行重新整理權杖，請將 IdP 所發行權杖的有效期限增加至 1 小時以上。

### <a name="do-we-have-any-way-to-change-rp-token-encryption-algorithm"></a>我們是否有任何方法可變更 RP 權杖加密演算法？
根據預設，RP 權杖加密會設定為 AES256，而且無法變更為其他任何值。

### <a name="on-a-mixed-mode-farm-i-get-error-when-trying-to-set-the-new-ssl-certificate-using-set-adfssslcertificate--thumbprint-how-can-i-update-the-ssl-certificate-in-a-mixed-mode-ad-fs-farm"></a>在混合模式的伺服器陣列上，我在嘗試使用 Set-AdfsSslCertificate -Thumbprint 設定新的 SSL 憑證時收到錯誤。 如何在混合模式 AD FS 伺服器陣列中更新 SSL 憑證？
混合模式 AD FS 伺服器陣列應該是過渡狀態。 建議您規劃在升級程序前變換 SSL 憑證，或在更新 SSL 憑證前完成此程序並提升伺服器陣列行為層級。 若未完成這項作業，下列指示會提供更新 SSL 憑證的能力。

在 WAP 伺服器上，您仍然可以使用 Set-WebApplicationProxySslCertificate。 在 ADFS 伺服器上，您必須使用 netsh。 依照下列步驟操作：

1. 選取 ADFS 2016 伺服器的子集以進行維護 (例如從負載平衡器中移除)
2. 在 #1 中選取的伺服器上，透過 MMC 匯入新憑證
3. 刪除現有的憑證

    a. netsh http delete sslcert hostnameport=fs.contoso.com:443
    
    b. netsh http delete sslcert hostnameport=localhost:443
    
    c. netsh http delete sslcert hostnameport=fs.contoso.com:49443

4.  新增憑證

    a. netsh http add sslcert hostnameport=fs.contoso.com:443 certhash=THUMBPRINT appid="{5d89a20c-beab-4389-9447-324788eb944a}" certstorename=My verifyclientcertrevocation=Enable sslctlstorename=AdfsTrustedDevices

    b. netsh http add sslcert hostnameport=localhost:443 certhash=THUMBPRINT appid="{5d89a20c-beab-4389-9447-324788eb944a}" certstorename=My verifyclientcertrevocation=Enable

    c. netsh http add sslcert hostnameport=fs.contoso.com:49443 certhash=THUMBPRINT appid="{5d89a20c-beab-4389-9447-324788eb944a}" certstorename=My verifyclientcertrevocation=Enable clientcertnegotiation=Enable

5. 在選取的伺服器上重新啟動 ADFS 服務
6. 移除 WAP 伺服器的子集以進行維護
7. 在選取的 WAP 伺服器上，透過 MMC 匯入新憑證
8. 使用 Cmdlet 在 WAP 上設定新憑證

    a. Set-WebApplicationProxySslCertificate -Thumbprint " CERTTHUMBPRINT"

9. 在選取的 WAP 伺服器上重新啟動服務
10. 將選取的 WAP 和 AD FS 伺服器放回生產環境中。

以類似的方式，在其餘的 AD FS 和 WAP 伺服器上執行更新。

### <a name="is-adfs-supported-when-web-application-proxy-wap-servers-are-behind-azure-web-application-firewallwaf"></a>當 Web 應用程式 Proxy (WAP) 伺服器位於 Azure Web 應用程式防火牆 (WAF) 後方時，是否支援 ADFS？
ADFS 和 Web 應用程式伺服器支援任何不會在端點上執行 SSL 終止的防火牆。 此外，ADFS/WAP 伺服器有內建機制可防止常見的 Web 攻擊 (例如跨網站指令碼、ADFS Proxy)，並可滿足 [MS-ADFSPIP 通訊協定](/openspecs/windows_protocols/ms-adfspip/76deccb1-1429-4c80-8349-d38e61da5cbb)定義的所有需求。

### <a name="i-am-seeing-an-event-441-a-token-with-a-bad-token-binding-key-was-found-what-should-i-do-to-resolve-this"></a>我看到「事件441：找到具有錯誤權杖繫結金鑰的權杖。」 我該怎麼做才能解決此問題？
在 AD FS 2016 中，權杖繫結會自動啟用，並導致造成此錯誤的 Proxy 和同盟案例發生多個已知問題。 若要解決此問題，請執行下列 Powershell 命令，並移除權杖繫結支援。

`Set-AdfsProperties -IgnoreTokenBinding $true`

### <a name="i-have-upgraded-my-farm-from-ad-fs-in-windows-server-2016-to-ad-fs-in-windows-server-2019-the-farm-behavior-level-for-the-ad-fs-farm-has-been-successfully-raised-to-2019-but-the-web-application-proxy-configuration-is-still-displayed-as-windows-server-2016"></a>我已將伺服器陣列從 Windows Server 2016 中的 AD FS 升級到 Windows Server 2019 中的 AD FS。 AD FS 伺服器陣列的伺服器陣列行為層級已成功提升至2019，但 Web 應用程式 Proxy 設定仍然顯示為 Windows Server 2016？
升級至 Windows Server 2019 之後，Web 應用程式 Proxy 的設定版本會繼續顯示為 Windows Server 2016。 Web 應用程式 Proxy 沒有適用於 Windows Server 2019 的新版本特有功能，如果已成功在 AD FS 上提升伺服器陣列行為層級，則 Web 應用程式 Proxy 會繼續依照設計顯示為 Windows Server 2016。

### <a name="can-i-estimate-the-size-of-the-adfsartifactstore-before-enabling-esl"></a>我可以在啟用 ESL 之前，先估計 ADFSArtifactStore 的大小嗎？
若啟用 ESL，AD FS 會追蹤 ADFSArtifactStore 資料庫中使用者的帳戶活動和已知位置。 此資料庫的大小會隨著所追蹤的使用者數目和已知位置數目而相對調整。 規劃要啟用 ESL 時，您可以估計 ADFSArtifactStore 資料庫的大小，以每 10 萬個使用者最多 1GB 的速率成長。 如果 AD FS 伺服器陣列使用 Windows 內部資料庫 (WID)，則資料庫檔案的預設位置為 C:\Windows\WID\Data。 若要避免填滿此磁碟機，請在啟用 ESL 之前，先確定您至少有 5GB 的可用儲存體。 除了磁碟儲存體以外，請在啟用 ESL 之後，針對總處理序記憶體進行規劃，50 萬或更少使用者人口數最多可成長額外 1GB 的 RAM。

### <a name="i-am-seeing-event-570-active-directory-trust-enumeration-was-unable-to-enumerate-one-of-more-domains-due-to-the-following-error-enumeration-will-continue-but-the-active-directory-identifier-list-may-not-be-correct-validate-that-all-expected-active-directory-identifiers-are-present-by-running-get-adfsdirectoryproperties-on-ad-fs-2019-what-is-the-mitigation-for-this-event"></a>我在 AD FS 2019 上看到「事件 570」(Active Directory 信任列舉因下列錯誤而無法列舉一或多個網域。 列舉將會繼續，但 Active Directory 識別碼清單可能不正確。 請執行 Get-ADFSDirectoryProperties 來驗證所有預期的 Active Directory 識別碼都已存在)。 要如何緩解此事件？
當 AD FS 嘗試列舉受信任樹系鏈結中的所有樹系並連結所有樹系，但樹系卻不受信任時，就會發生此事件。 例如，如果 AD FS 樹系 A 和樹系 B 受信任，而樹系 B 和樹系 C 受信任，則 AD FS 會將這三個樹系全都列舉出來，並嘗試尋找樹系 A 和 C 之間的信任。如果失敗樹系中的使用者應由 AD FS 進行驗證，則請設定 AD FS 樹系與失敗樹系之間的信任。 如果失敗樹系中的使用者不應由 AD FS 驗證，則請忽略此錯誤。

### <a name="i-am-seeing-an-event-id-364-microsoftidentityserverauthenticationfailedexception-msis5015-authentication-of-the-presented-token-failed-token-binding-claim-in-token-must-match-the-binding-provided-by-the-channel-what-should-i-do-to-resolve-this"></a>我看到「事件識別碼 364：Microsoft.IdentityServer.AuthenticationFailedException：MSIS5015：所出示的權杖驗證失敗。 權杖中的權杖繫結宣告必須符合通道所提供的繫結。」 我該怎麼做才能解決此問題？
在 AD FS 2016 中，權杖繫結會自動啟用，並導致造成此錯誤的 Proxy 和同盟案例發生多個已知問題。 若要解決此問題，請執行下列 Powershell 命令，並移除權杖繫結支援。

`Set-AdfsProperties -IgnoreTokenBinding $true`
