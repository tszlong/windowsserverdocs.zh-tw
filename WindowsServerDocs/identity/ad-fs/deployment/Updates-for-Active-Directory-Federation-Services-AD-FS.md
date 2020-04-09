---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: Active Directory 同盟服務的必要更新（AD FS）
author: billmath
ms.author: billmath
manager: femila
ms.date: 3/29/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f72a29d97da89b0b6de93b6b573bce4fdf5e35c2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855861"
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Active Directory 同盟服務（AD FS）和 Web 應用程式 Proxy （WAP）的必要更新

從2016年10月起，Windows Server 所有元件的所有更新都只會透過 Windows Update （WU）發行。  沒有更多的修補程式或個別下載。
這適用于 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 SP1。

此頁面會列出 AD FS 和 WAP 的特定匯總套件，以及建議的 AD FS 和 WAP 的修補程式更新歷程清單。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Windows Server 2016 中 AD FS 和 WAP 的更新
Windows Server 2016 的更新會每月透過 Windows Update 傳遞，而且是累計的。 下列為所有 AD FS 和 WAP 2016 伺服器建議的更新套件，並包含所有先前必要的更新，以及最新的修正程式。

|文庫# |描述|發行日期
|----- | ----- |-----
|[4534271](https://support.microsoft.com/help/4534271/windows-10-update-kb4534271) | 因 Google Chrome 版本80預設支援新的[SameSite](https://www.chromestatus.com/feature/5088147346030592) cookie 原則，而無法解決潛在 AD FS chrome 失敗。 如需詳細資訊，請參閱[這裡](https://docs.microsoft.com/office365/troubleshoot/miscellaneous/chrome-behavior-affects-applications)。 |2020年1月|
|[CVE-2019-1126](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1126) | 此安全性更新解決了 Active Directory 同盟服務（AD FS）中的弱點，這可能會讓攻擊者略過外部網路鎖定原則。 |2019 年 7 月|
|[4489889（OS 組建14393.2879）](https://support.microsoft.com/help/4489889/windows-10-update-kb4489889) | 解決 Active Directory 同盟服務（AD FS）中導致重複的信賴憑證者信任出現在 AD FS 管理主控台的問題。 當您使用 AD FS 管理主控台建立或查看信賴憑證者信任時，就會發生這種情況。</br></br> 解決在 AD FS 2016 上啟用外部網路智慧鎖定（ESL）時，所發生的高 Active Directory 同盟服務（ADFS） Web 應用程式 Proxy （WAP）延遲問題（超過10毫秒）。 此安全性更新會解決[CVE-2018-16794](https://nvd.nist.gov/vuln/detail/CVE-2018-16794)中所述的弱點。 |2019 年 3 月|
|[4487006（OS 組建14393.2828）](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) | 解決在使用 PowerShell 或 Active Directory 同盟服務（AD FS）管理主控台時，導致信賴憑證者信任的更新失敗的問題。 如果您將信賴憑證者信任設定為使用可發佈多個 PassiveRequestorEndpoint 的線上中繼資料 URL，就會發生此問題。 錯誤為「MSIS7615：信賴憑證者信任中指定的信任端點對於該信賴憑證者信任必須是唯一的。」  </br></br>解決因 Azure 密碼保護原則而顯示特定錯誤訊息的問題，以因應外部複雜度的密碼變更。 |2019 年 2 月|
|[4462928（OS 組建14393.2580）](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)|解決 Active Directory 同盟服務（ADFS）外部網路智慧鎖定（ESL）與替代登入識別碼之間的互通性問題。 啟用替代登入識別碼時，會呼叫 AD FS Powershell Cmdlet AdfsAccountActivity 和 Reset-AdfsAccountLockout，傳回「找不到帳戶」錯誤。 呼叫 AdfsAccountActivity 時，會加入新的專案，而不是編輯現有的專案。|2018 年 10 月|
|[4343884（OS 組建14393.2457）](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)|解決在使用自訂文化特性定義的行動裝置上，多重要素驗證無法正常運作的 Active Directory 同盟服務（AD FS）問題。 </br></br>解決 Windows Hello 企業版中導致新使用者註冊明顯延遲（15秒）的問題。 當使用硬體安全性模組來儲存 ADFS 登錄授權單位（RA）憑證時，就會發生此問題。|2018 年 8 月|
|[4338822（OS 組建14393.2395）](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)|解決 AD FS 中的問題，在從主控台建立或查看信賴憑證者信任時，在 AD FS 管理主控台中顯示重複的信賴憑證者信任。</br></br>解決 ADFS 中導致 Windows Hello 企業版失敗的問題。 當有兩個宣告提供者時，就會發生此問題。 PIN 註冊將會失敗，並出現「400內部伺服器錯誤：無法取得裝置識別碼」。</br></br> 解決與永不結束的非使用中連線相關的 WAP 問題。 這會導致系統資源流失（例如，記憶體流失）以及不再回應的 WAP 服務。 解決防止使用者選取不同登入選項的 AD FS 問題。 當使用者選擇使用以憑證為基礎的驗證來登入，但尚未設定時，就會發生這種情況。 如果使用者選取 [憑證型驗證]，然後嘗試選取另一個登入選項，也會發生這種情況。 如果發生這種情況，系統會將使用者重新導向至以憑證為基礎的驗證頁面，直到他們關閉瀏覽器為止。|2018 年 7 月|
|[4103720（OS 組建14393.2273）](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)|解決 ADFS 的問題，當啟用 PreventTokenReplays 時，會導致 IdP 起始的登入 SAML 信賴憑證者失敗。 </br></br>解決當 OAUTH 從裝置或瀏覽器應用程式進行驗證時所發生的 ADFS 問題。 使用者密碼變更會產生失敗，並要求使用者結束應用程式或瀏覽器以登入。 </br></br>解決在 UTC + 1 和更新版本（歐洲和亞洲）中啟用外部網路智慧鎖定無法使用的問題。 此外，它會導致一般外部網路鎖定因下列錯誤而失敗： AdfsAccountActivity：大於或小於 MinValue 的 DateTime 值，轉換成 UTC 時，無法序列化為 JSON。</br></br>解決 ADFS Windows Hello 企業版的問題，其中新的使用者無法布建其 PIN。 未設定 MFA 提供者時，就會發生這種情況。|2018 年 5 月|
|[4093120（OS 組建14393.2214）](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)| 解決未處理的重新整理權杖驗證問題。 它會產生下列錯誤：「IdentityServer。 OAuthInvalidRefreshTokenException： MSIS9312：收到不正確 OAuth 重新整理權杖。 在權杖中的允許時間之前收到重新整理權杖。」 |2018 年 4 月|
|[4077525（OS 組建14393.2097）](https://support.microsoft.com/help/4077525/windows-10-update-kb4077525)|解決當 ADFS 伺服器陣列至少有兩部使用 Windows Internal Database （WID）的伺服器時，發生 HTTP 500 錯誤的問題。 在此案例中，Web 應用程式 Proxy （WAP）伺服器上的 HTTP 基本預先驗證無法驗證某些使用者。 發生錯誤時，您可能也會在 WAP 事件記錄檔中看到 Microsoft Windows Web 應用程式 Proxy 警告事件識別碼13039。 描述會讀取「Web 應用程式 Proxy 無法驗證使用者。 預先驗證是「適用于豐富型用戶端的 ADFS」。 指定的使用者未獲授權，無法存取指定的信賴憑證者。 需要修改目標信賴憑證者或 WAP 信賴憑證者的授權規則。」</br></br>解決 AD FS 在驗證期間無法再忽略 prompt = login 的問題。 已新增停用的選項，以支援未使用密碼驗證的案例。 如需詳細資訊，請參閱 AD FS 在 Windows Server 2016 RTM 驗證期間忽略 "prompt = login" 參數。</br></br>解決 AD FS 在授權客戶（和信賴憑證者）選取 [憑證作為驗證選項] 時，將無法連線的問題。 如果已啟用 Windows 整合式驗證（WIA），而且要求可以執行 WIA，則在使用 prompt = login 時，就會發生此失敗。</br></br>解決當識別提供者（IDP）與 OAuth 群組中的信賴憑證者（RP）相關聯時，AD FS 不正確地顯示 [主領域探索（HRD）] 頁面的問題。 除非有多個 Idp 與 OAuth 群組中的 RP 相關聯，否則使用者不會顯示在 [HRD] 頁面上。 相反地，使用者會直接移至相關聯的 IDP 進行驗證。|2018 年 2 月|
|[4041688（OS 組建14393.1794）](https://support.microsoft.com/kb/4041688)|此修正程式會解決因為快取行為不正確而導致 AD 授權單位要求錯誤的身分識別提供者的問題。 這可能會影響驗證功能，例如多重要素驗證。 </br></br>已新增 AAD Connect Health 在混合 SQL2014SP1-WS2012R2 和 WS2016 ADFS 伺服器陣列上以正確的精確度（使用詳細資訊審核）來報告 ADFS 伺服器健全狀況的功能。</br></br>已修正將 2012 R2 ADFS 伺服器陣列升級至 ADFS 2016 的問題，當有許多信賴憑證者信任時，引發伺服器陣列行為層級的 powershell Cmdlet 會失敗，且會有一個超時時間。</br></br>解決了 AD FS 會在將要求與其他安全性權杖伺服器（STS）同盟時修改 wct 參數值，而導致驗證失敗的問題。|2017 年 10 月|
|[4038801（OS 組建14393.1737）](https://support.microsoft.com/kb/4038801)|已針對使用同盟 LDPs 的 OIDC 登出新增支援。 這會允許「Kiosk 案例」，其中多個使用者可能會以連續方式登入單一裝置，其中會與 LDP 進行同盟。</br></br>已修正 CEP/CES 型憑證無法搭配 gMSA 帳戶使用的 WinHello 問題。</br></br>修正 Windows Server 2016 ADFS 伺服器上的 Windows 內部資料庫（WID）因外鍵條件約束而無法同步某些設定的問題，例如 ApplicationGroupId 的資料行 IdentityServerPolicy. 範圍和 IdentityServerPolicy 資料表）。 這類同步處理失敗可能會導致主要到次要 ADFS 伺服器之間有不同的宣告、宣告提供者和應用程式體驗。 此外，如果將 WID 主要角色移到次要節點，應用程式群組將不再可在 ADFS 管理 UX 中進行管理。</br></br>此更新修正了使用自訂文化特性定義的行動裝置無法正常運作的多重要素驗證問題|2017 年 9 月|
|[4034661（OS 組建14393.1613）](https://support.microsoft.com/kb/4034661)|修正在 ADFS 4.0 \ Windows 2016 Server 的安全性事件記錄檔中，nog 由411事件記錄呼叫者 IP 位址的問題，即使啟用「成功的審查」和「失敗的審核」之後也是如此。</br></br>當 ADFX 伺服器設定為使用 HTTP Proxy 時，此修正可解決 Azure 多重要素驗證（MFA）的問題。</br></br>「解決了向 ADFS Proxy 伺服器出示過期或撤銷的憑證，並不會對使用者傳回錯誤」的問題。|2017 年 8 月|
|[4034658（OS 組建14393.1593）](https://support.microsoft.com/kb/4034658)|修正 2016 AD FS 伺服器，以便在內部部署部署上支援適用于 Windows Hello 企業版的 MFA 憑證註冊|2017 年 8 月|
|[4025334（OS 組建14393.1532）](https://support.microsoft.com/kb/4025334)|解決了當 PkeyAuth 要求包含不正確的資料時，PkeyAuth token 處理常式可能使驗證失敗的問題。 驗證仍應繼續進行，而不需要執行裝置驗證|2017 年 7 月|
|[4022723（OS 組建14393.1378）](https://support.microsoft.com/kb/4022723)|[Web 應用程式 Proxy]2012R2/2016 混合部署中的 WAP 2016 不會拾取 DisableHttpOnlyCookieProtection 設定屬性的值 </br></br>[Web 應用程式 Proxy]無法從 EAS 預先驗證案例中的 AD FS 取得使用者存取權杖。</br></br>AD FS 2016： WSFED 登出會導致例外狀況|2017 年 6 月
|[3213986](https://support.microsoft.com/kb/3213986)|適用于 x64 型系統的 Windows Server 2016 累積更新（KB3213986）| 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 和 WAP 的更新
以下是 Windows Server 2012 R2 中 Active Directory 同盟服務（AD FS）已發行的「修補程式」和「更新彙總套件」清單。

|文庫# |描述|發行日期
|----- | ----- |-----
|[4534309](https://support.microsoft.com/help/4534309/windows-8-1-kb4534309)| 因 Google Chrome 版本80預設支援新的[SameSite](https://www.chromestatus.com/feature/5088147346030592) cookie 原則，而無法解決潛在 AD FS chrome 失敗。 如需詳細資訊，請參閱[這裡](https://docs.microsoft.com/office365/troubleshoot/miscellaneous/chrome-behavior-affects-applications)。 |2020年1月
|[4507448](https://support.microsoft.com/help/4507448/windows-8-1-update-kb4507448)| 此安全性更新解決了 Active Directory 同盟服務（AD FS）中的弱點，這可能會讓攻擊者略過外部網路鎖定原則。 |2019 年 7 月
|[4041685](https://support.microsoft.com/kb/4041685)|解決了 AD FS 問題，其中要求標頭中的 MSISConext cookie 最後可能會溢位標頭大小限制，並導致無法以 HTTP 狀態碼400「錯誤的要求標頭太長」進行驗證。</br></br>修正了在驗證期間，ADFS 無法再忽略「提示 = 登入」的問題。 已將 [已停用] 選項新增至使用非密碼驗證的還原案例。|2017年10月更新彙總套件預覽
|[4019217](https://support.microsoft.com/kb/4019217)|使用伺服器 2012 R2 AD FS 伺服器時，使用權杖代理程式的工作資料夾用戶端無法正常執行|5月2017預覽更新彙總套件
|[4015550](https://support.microsoft.com/kb/4015550)|修正了 AD FS 未驗證外部使用者，以及 AD FS WAP 隨機無法轉寄要求的問題|2017年4月更新彙總套件
|[4015547](https://support.microsoft.com/kb/4015547)|修正了 AD FS 未驗證外部使用者，以及 AD FS WAP 隨機無法轉寄要求的問題|2017年4月安全性更新
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 此安全性更新可解決 Active Directory 同盟服務（ADFS）中的弱點。 如果攻擊者將特製的要求傳送至 AD FS 伺服器，此弱點可能會導致資訊洩漏，讓攻擊者能夠讀取目標系統的機密資訊。|2017年3月更新彙總套件
|[3179574](https://support.microsoft.com/kb/3179574)|已修正 AD FS 外部網路密碼更新的問題。 |2016年8月更新彙總套件
|[3172614](https://support.microsoft.com/kb/3172614)|引進了提示 = 登入[支援](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7)，已修正 AD FS 管理主控台和 AlwaysRequireAuthentication 設定的問題。 |2016年7月更新彙總套件
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory 同盟服務（AD FS）3.0 無法連線到已設定為使用連接字串中安全通訊端層（SSL）埠636或3269的輕量型目錄存取協定（LDAP）屬性存放區。 |2016年6月更新彙總套件
|[3148533](https://support.microsoft.com/kb/3148533)|透過 Windows Server 2012 R2 中的 ADFS Proxy，MFA fallback 驗證失敗 |2016 年 5 月
|[3134787](https://support.microsoft.com/kb/3134787)|在 Windows Server 2012 R2 中，AD FS 記錄不包含帳戶鎖定案例的用戶端 IP 位址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020： Active Directory 同盟服務的安全性更新，以解決阻絕服務：2016年2月9日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|當 Windows Server 2012 R2 架構 AD FS 伺服器中啟用了裝置驗證時，無法存取應用程式|2015 年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|當使用者在 Windows Server 2012 R2 中使用 MFA 時，重複載入頁面並驗證失敗 AD FS|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|當 MFA adapter 在 Windows Server 2012 R2 中擲回例外狀況時，AD FS 不會呼叫 OnError|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|在 Windows Server 2012 R2 中新增或移除宣告提供者之後，次要 AD FS 伺服器上的信任關係會遺失|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|在非宣告感知信賴憑證者信任中，主領域探索無法正常運作|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新在 Windows Server 2012 R2 的 AD FS token 中新增複合識別碼宣告的支援|2015 年 5 月
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040： Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|當您透過 Windows Server 2012 R2 中的 WAP 開啟共用信箱時，出現「HTTP 400-不正確的要求」錯誤|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|Windows Server 2012 R2 中 Web 應用程式 Proxy 驗證權杖的 AD FS token 重新執行保護|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|更新密碼功能的修復，讓使用者不需要在 Windows Server 2012 R2 中使用已註冊的裝置|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS 無法處理 Windows Server 2012 R2 中的 SAML 回應|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|當您嘗試透過 Windows Server 2012 R2 中的 Web 應用程式 Proxy 儲存 Office 檔案時，操作會失敗|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|當您使用不正確的使用者名稱登入 Windows Server 2012 R2 時，系統不會再次提示您輸入 username|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|當您在 Windows Server 2012 R2 中執行 web 應用程式時，系統會提示您進行驗證 AD FS|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|Windows Server 2012 R2 中的裝置註冊服務初始部署後的超時失敗|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|當您從內部網路存取 Windows Server 2012 R2 AD FS 伺服器時，系統會提示您輸入使用者名稱和密碼兩次|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新彙總套件|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新彙總套件|2014 年 11 月
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新彙總套件|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新彙總套件|2014 年 7 月
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新彙總套件|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新彙總套件|2014 年 5 月
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新彙總套件|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Windows Server 2012 （AD FS 2.1）和 AD FS 2.0 中的 AD FS 更新
以下是已針對 AD FS 2.0 和2.1 發行的「修補程式」和「更新彙總套件」清單。

|文庫# |描述|發行日期|適用於：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|在 Windows Server 2012 中透過 proxy 驗證失敗（這是「修補程式3094446」的一般版本）|2016年11月品質匯總套件|AD FS 2。1
|[3197869](https://support.microsoft.com/kb/3197869)|在 Windows Server 2008 R2 SP1 中透過 proxy 驗證失敗（這是「修補程式3094446」的一般版本）|2016年11月品質匯總套件|AD FS 2。0
|[3094446](https://support.microsoft.com/kb/3094446)|在 Windows Server 2012 或 Windows Server 2008 R2 SP1 中透過 proxy 驗證失敗|2015 年 9 月|AD FS 2.0 和2。1
|[3070078](https://support.microsoft.com/kb/3070078)|當您對 Windows Server 2012 中的加密憑證進行驗證時，AD FS 2.1 會擲回例外狀況|2015 年 7 月|AD FS 2。1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062： Active Directory federation services 中的弱點可能會允許權限提高|2015 年 6 月|AD FS 2.0/2。1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077： Active Directory 同盟服務中的弱點可能會允許資訊洩漏：2015年4月14日|2014 年 11 月|AD FS 2.0/2。1
|[2987843](https://support.microsoft.com/kb/2987843)|當許多使用者登入 Windows Server 2012 中的 web 應用程式時，AD FS 同盟伺服器的記憶體使用量會持續增加|2014 年 7 月|AD FS 2。1
|[2957619](https://support.microsoft.com/kb/2957619)|對委派權杖的 AD FS 提出要求時，AD FS 中的信賴憑證者信任已停止|2014 年 5 月|AD FS 2。1
|[2926658](https://support.microsoft.com/kb/2926658)|如果您沒有 SQL 許可權，ADFS SQL 伺服器陣列部署將會失敗|2014 年 10 月|AD FS 2。1
|[2896713](https://support.microsoft.com/kb/2896713)或[2989956](https://support.microsoft.com/kb/2989956)|在 AD FS 伺服器上安裝安全性更新2843638之後，有可用來修正數個問題的更新|2013年11月</br></br>2014 年 9 月|AD FS 2.0/2。1
|[2877424](https://support.microsoft.com/kb/2877424)|更新可讓您在 AD FS 2.1 伺服器陣列中，將一個憑證用於多個信賴憑證者信任|2013 年 10 月|AD FS 2。1
|[2873168](https://support.microsoft.com/kb/2873168)|修正：當您使用協力廠商 CSP 和 HSM，然後在 Windows Server 2008 R2 Service Pack 1 上的 AD FS 2.0 的更新彙總套件3中設定宣告提供者信任時，就會發生錯誤。|2013年9月|AD FS 2。0
|[2861090](https://support.microsoft.com/kb/2861090)|加密憑證的主體名稱中的逗號會導致 Windows Server 2008 R2 SP1 發生例外狀況|2013 年 8 月|AD FS 2。0
|[2843639](https://support.microsoft.com/kb/2843639)|安全級Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2013年11月|AD FS 2。1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066： Active Directory 同盟服務2.0 的安全性更新描述：2013年8月13日|2013 年 8 月|AD FS 2。0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata 在 Windows Server 2012 中未包含 WS-TRUST 和 WS-同盟端點的 MEX 端點資訊|2013 年 5 月|AD FS 2。1
|[2790338](https://support.microsoft.com/kb/2790338)|Active Directory 同盟服務（AD FS）2.0 的更新彙總套件3描述|2013 年 3 月|AD FS 2。0
