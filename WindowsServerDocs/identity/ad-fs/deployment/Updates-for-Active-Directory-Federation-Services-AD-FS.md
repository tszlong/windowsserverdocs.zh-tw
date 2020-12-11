---
description: '深入瞭解： Active Directory 同盟服務 (AD FS) 及 Web 應用程式 Proxy (WAP 的必要更新) '
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: 'Active Directory 同盟服務 (AD FS 的必要更新) '
author: billmath
ms.author: billmath
manager: femila
ms.date: 3/29/2019
ms.topic: article
ms.openlocfilehash: b970a5e098d2d950380356a4254599859b3e39d7
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049056"
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy 的必要更新 (WAP) 

從2016年10月起，所有 Windows Server 元件的更新都只會透過 Windows Update (WU) 來發行。  沒有其他修補程式或個別下載。
這適用于 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012 和 Windows Server 2008 R2 SP1。

此頁面會列出 AD FS 和 WAP 特定興趣的匯總套件，以及針對 AD FS 和 WAP 建議的修復程式更新歷程記錄清單。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Windows Server 2016 中 AD FS 和 WAP 的更新
Windows Server 2016 的更新會每月透過 Windows Update 傳遞，並且是累計的。 針對所有 AD FS 和 WAP 2016 伺服器，建議使用下列更新套件，並包含先前所需的所有更新以及最新的修正程式。

|K b# |描述|發行日期
|----- | ----- |-----
|[4534271](https://support.microsoft.com/help/4534271/windows-10-update-kb4534271) | 因預設針對 Google Chrome 80 版支援新的 [SameSite](https://www.chromestatus.com/feature/5088147346030592) cookie 原則，解決了潛在的 AD FS chrome 失敗。 如需詳細資訊，請參閱 [這裡](/office365/troubleshoot/miscellaneous/chrome-behavior-affects-applications)。 |2020 年 1 月|
|[CVE-2019-1126](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1126) | 這項安全性更新解決了 Active Directory 同盟服務 (AD FS) 的弱點，可能會讓攻擊者略過外部網路鎖定原則。 |2019 年 7 月|
|[4489889 (OS 組建 14393.2879) ](https://support.microsoft.com/help/4489889/windows-10-update-kb4489889) | 解決 Active Directory 同盟服務 (AD FS) 造成重複的信賴憑證者信任出現在 AD FS 管理主控台中的問題。 當您使用 AD FS 管理主控台建立或查看信賴憑證者信任時，就會發生這種情況。</br></br> 處理高 Active Directory 同盟服務 (ADFS) Web 應用程式 Proxy (WAP) 超過10的延遲問題 (超過 10) 延遲問題 () 2016 上啟用了外部網路智慧鎖定 AD FS ESL。 此安全性更新會解決 [CVE-2018-16794](https://nvd.nist.gov/vuln/detail/CVE-2018-16794)中所述的弱點。 |2019 年 3 月|
|[4487006 (OS 組建 14393.2828) ](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) | 解決在使用 PowerShell 或 Active Directory 同盟服務 (AD FS) 管理主控台時，造成信賴憑證者信任更新失敗的問題。 如果您設定信賴憑證者信任以使用發佈多個 PassiveRequestorEndpoint 的線上中繼資料 URL，就會發生此問題。 錯誤為「MSIS7615：信賴憑證者信任中指定的受信任端點必須是該信賴憑證者信任的唯一」。  </br></br>解決因 Azure 密碼保護原則而顯示外部複雜性密碼變更的特定錯誤訊息的問題。 |2019 年 2 月|
|[4462928 (OS 組建 14393.2580) ](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)|解決 Active Directory 同盟服務 (ADFS) 外部網路智慧鎖定 (ESL) 和替代登入識別碼之間的互通性問題。 啟用替代登入識別碼時，AD FS Powershell Cmdlet、Get-AdfsAccountActivity 和重設 AdfsAccountLockout 的呼叫會傳回「找不到帳戶」錯誤。 呼叫 Set-AdfsAccountActivity 時，會加入新的專案，而不是編輯現有的專案。|2018 年 10 月|
|[4343884 (OS 組建 14393.2457) ](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)|解決 Active Directory 同盟服務 (AD FS) 問題，Multi-Factor Authentication 無法在使用自訂文化特性定義的行動裝置上正確運作。 </br></br>解決在新的使用者註冊中，在 15) 秒內造成明顯延遲 (Windows Hello 企業版的問題。 當使用硬體安全模組來儲存 ADFS 登錄授權單位 (RA) 憑證時，就會發生此問題。|2018 年 8 月|
|[4338822 (OS 組建 14393.2395) ](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)|解決 AD FS 中的問題，該問題會在從主控台建立或查看信賴憑證者信任時，在 AD FS 管理主控台中顯示重複的信賴憑證者信任。</br></br>解決 ADFS 中導致 Windows Hello 企業版失敗的問題。 當有兩個宣告提供者時，就會發生此問題。 PIN 註冊將會失敗，並出現「400內部伺服器錯誤：無法取得裝置識別碼」。</br></br> 解決與非作用中連線不會結束相關的 WAP 問題。 這會導致系統資源流失 (例如，記憶體流失) ，以及不再回應的 WAP 服務。 解決導致使用者無法選取不同登入選項的 AD FS 問題。 當使用者選擇使用以憑證為基礎的驗證進行登入，但尚未設定時，就會發生這種情況。 如果使用者選取 [憑證型驗證]，然後嘗試選取其他登入選項，也會發生這種情況。 如果發生這種情況，系統會將使用者重新導向至以憑證為基礎的驗證頁面，直到他們關閉瀏覽器為止。|2018 年 7 月|
|[4103720 (OS 組建 14393.2273) ](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)|解決在啟用 PreventTokenReplays 時，導致 IdP 起始的 SAML 信賴憑證者登入失敗的 ADFS 問題。 </br></br>解決當 OAUTH 從裝置或瀏覽器應用程式進行驗證時所發生的 ADFS 問題。 使用者密碼變更會產生失敗，而且需要使用者結束應用程式或瀏覽器才能登入。 </br></br>解決在 UTC + 1 和更高 (歐洲和亞洲) 啟用外部網路智慧鎖定的問題無法運作。 此外，它會導致一般外部網路鎖定失敗，並出現下列錯誤： AdfsAccountActivity：大於 DateTime 的 DateTime 值，或小於 datetime. MinValue 轉換成 UTC 時，無法序列化為 JSON。</br></br>解決 ADFS Windows Hello 的商務問題，而新的使用者無法布建其 PIN。 未設定任何 MFA 提供者時，就會發生這種情況。|2018 年 5 月|
|[4093120 (OS 組建 14393.2214) ](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)| 解決未處理的重新整理權杖驗證問題。 它會產生下列錯誤： IdentityServer： OAuthInvalidRefreshTokenException： MSIS9312：收到不正確 OAuth 重新整理權杖。 在權杖中收到的重新整理權杖早于允許的時間。」 |2018 年 4 月|
|[4077525 (OS 組建 14393.2097) ](https://support.microsoft.com/help/4077525/windows-10-update-kb4077525)|解決當 ADFS 伺服器陣列至少有兩部使用 Windows 內部資料庫 (WID) 的伺服器時，會發生 HTTP 500 錯誤的問題。 在此案例中，Web 應用程式 Proxy 上的 HTTP 基本預先驗證 (WAP) server 無法驗證某些使用者。 發生錯誤時，您可能也會在 WAP 事件記錄檔中看到 Microsoft Windows Web 應用程式 Proxy 警告事件識別碼13039。 描述會讀取「Web 應用程式 Proxy 無法驗證使用者。 預先驗證是「適用于豐富用戶端的 ADFS」。 指定的使用者未獲授權存取指定的信賴憑證者。 需要修改目標信賴憑證者或 WAP 信賴憑證者的授權規則。」</br></br>解決在驗證期間 AD FS 無法再忽略 prompt = login 的問題。 已將停用的選項新增至未使用密碼驗證的支援案例。 如需詳細資訊，請參閱 AD FS 在 Windows Server 2016 RTM 驗證期間忽略 "prompt = login" 參數。</br></br>解決 AD FS 中的問題，也就是授權的客戶 (和信賴憑證者) 選取憑證作為驗證選項的信賴憑證者將無法連接。 如果啟用 Windows 整合式驗證 (WIA) ，而且要求可以執行 WIA，則在使用 prompt = login 時，會發生失敗。</br></br>解決問題：當身分識別提供者 (IDP) 與 OAuth 群組中的信賴憑證者 (RP) 相關聯時，AD FS 錯誤地顯示主領域探索 (HRD) 頁面。 除非有多個 Idp 與 OAuth 群組中的 RP 相關聯，否則使用者不會顯示在 [HRD] 頁面上。 相反地，使用者會直接前往相關聯的 IDP 進行驗證。|2018 年 2 月|
|[4041688 (OS 組建 14393.1794) ](https://support.microsoft.com/kb/4041688)|這項修正解決了因為不正確的快取行為，而導致 AD 授權要求 misdirects 至錯誤身分識別提供者的問題。 這可能會影響驗證功能，例如多重要素驗證。 </br></br>使用混合 WS2012R2 和 WS2016 ADFS 伺服器陣列上的詳細資訊審核) ，新增 AAD Connect Health 回報 ADFS 伺服器健全狀況的功能，並提供正確的精確度 (。</br></br>修正將 2012 R2 ADFS 伺服器陣列升級至 ADFS 2016 期間的問題，在有許多信賴憑證者信任時，引發伺服器陣列行為層級的 powershell Cmdlet 會失敗，並會有超時時間。</br></br>解決了 AD FS 藉由修改 wct 參數值並將要求同盟至其他安全性權杖伺服器 (STS) 的問題，而導致驗證失敗。|2017年 10 月|
|[4038801 (OS 組建 14393.1737) ](https://support.microsoft.com/kb/4038801)|針對使用同盟 LDPs 的 OIDC 登出新增了支援。 這會允許「Kiosk 案例」，其中多個使用者可能會以串列方式登入與 LDP 同盟的單一裝置。</br></br>修正 CEP/CES 型憑證無法搭配 gMSA 帳戶使用的 WinHello 問題。</br></br>修正 Windows Server 2016 ADFS 伺服器上的 Windows 內部資料庫 (WID) 無法同步某些設定的問題，例如來自 IdentityServerPolicy 的 ApplicationGroupId 資料行和 IdentityServerPolicy。用戶端資料表) ，因為外鍵條件約束。 這類同步失敗可能會導致主要到次要 ADFS 伺服器之間有不同的宣告、宣告提供者和應用程式體驗。 此外，如果將 WID 主要角色移至次要節點，則在 ADFS 管理 UX 中將無法再管理應用程式群組。</br></br>此更新修正了多重要素驗證無法在使用自訂文化特性定義的行動裝置上正確運作的問題|2017 年 9 月|
|[4034661 (OS 組建 14393.1613) ](https://support.microsoft.com/kb/4034661)|修正在 ADFS 4.0 \ Windows Server 2016 RS1 ADFS 伺服器的安全性事件記錄檔中，nog 由411事件記錄呼叫端 IP 位址的問題，即使在啟用「成功審核」和「失敗的審核」之後也是如此。</br></br>此修正解決了當 ADFX 伺服器設定為使用 HTTP Proxy 時，Azure 多重要素驗證 (MFA) 的問題。</br></br>「解決了將過期或撤銷的憑證呈現給 ADFS Proxy 伺服器的問題，並不會對使用者傳回錯誤」。|2017 年 8 月|
|[4034658 (OS 組建 14393.1593) ](https://support.microsoft.com/kb/4034658)|修正 2016 AD FS server 以支援在內部部署部署上進行商務用 Windows Hello 的 MFA 憑證註冊|2017 年 8 月|
|[4025334 (OS 組建 14393.1532) ](https://support.microsoft.com/kb/4025334)|解決問題：如果 PkeyAuth 要求包含不正確的資料，PkeyAuth token 處理常式可能會導致驗證失敗。 驗證仍應繼續，而不執行裝置驗證|2017 年 7 月|
|[4022723 (OS 組建 14393.1378) ](https://support.microsoft.com/kb/4022723)|[Web 應用程式 Proxy]2012R2/2016 混合部署中的 WAP 2016 不會挑選 DisableHttpOnlyCookieProtection 設定屬性的值 </br></br>[Web 應用程式 Proxy]在 EAS 預先驗證的情況下，無法從 AD FS 取得使用者存取權杖。</br></br>AD FS 2016： WSFED 登出會導致例外狀況|2017 年 6 月
|[3213986](https://support.microsoft.com/kb/3213986)|適用于 x64 型系統的 Windows Server 2016 累積更新 (KB3213986) | 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 和 WAP 的更新
以下是 Windows Server 2012 R2 中針對 Active Directory 同盟服務 (AD FS) 發行的修補程式和更新彙總套件清單。

|K b# |描述|發行日期
|----- | ----- |-----
|[4534309](https://support.microsoft.com/help/4534309/windows-8-1-kb4534309)| 因預設針對 Google Chrome 80 版支援新的 [SameSite](https://www.chromestatus.com/feature/5088147346030592) cookie 原則，解決了潛在的 AD FS chrome 失敗。 如需詳細資訊，請參閱 [這裡](/office365/troubleshoot/miscellaneous/chrome-behavior-affects-applications)。 |2020 年 1 月
|[4507448](https://support.microsoft.com/help/4507448/windows-8-1-update-kb4507448)| 這項安全性更新解決了 Active Directory 同盟服務 (AD FS) 的弱點，可能會讓攻擊者略過外部網路鎖定原則。 |2019 年 7 月
|[4041685](https://support.microsoft.com/kb/4041685)|解決了 AD FS 的問題，要求標頭中的 cookie 最終可能會溢位標頭大小限制，並導致無法驗證 HTTP 狀態碼400「錯誤的要求標頭太久」。</br></br>修正了在驗證期間 ADFS 無法再忽略 "prompt = login" 的問題。 在使用非密碼驗證的還原案例中，已新增「停用」選項。|2017年10月預覽的更新彙總套件
|[4019217](https://support.microsoft.com/kb/4019217)|使用 Server 2012 R2 AD FS Server 時，使用權杖訊息代理程式的工作資料夾用戶端無法運作|5月2017預覽更新彙總套件
|[4015550](https://support.microsoft.com/kb/4015550)|已修正 AD FS 無法驗證外部使用者和 AD FS WAP 隨機無法轉寄要求的問題|2017年4月更新彙總套件
|[4015547](https://support.microsoft.com/kb/4015547)|已修正 AD FS 無法驗證外部使用者和 AD FS WAP 隨機無法轉寄要求的問題|2017年4月安全性更新
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 此安全性更新會解決 Active Directory 同盟服務 (ADFS) 中的弱點。 如果攻擊者將特製要求傳送到 AD FS 伺服器，此弱點可能會導致資訊洩漏，讓攻擊者能夠讀取目標系統的機密資訊。|2017年3月更新彙總套件
|[3179574](https://support.microsoft.com/kb/3179574)|已修正 AD FS 外部網路密碼更新的問題。 |2016年8月更新彙總套件
|[3172614](https://support.microsoft.com/kb/3172614)|引進了 prompt = 登入 [支援](../overview/ad-fs-faq.md)、已修正 AD FS 管理主控台和 AlwaysRequireAuthentication 設定的問題。 |2016年7月更新彙總套件
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory 同盟服務 (AD FS) 3.0 無法連線到輕量型目錄存取協定 (設定為在連接字串中使用) 安全通訊端層 SSL (埠636或3269的 LDAP) 屬性存放區。 |2016年6月更新彙總套件
|[3148533](https://support.microsoft.com/kb/3148533)|透過 Windows Server 2012 R2 中的 ADFS Proxy 進行 MFA 回溯驗證失敗 |2016 年 5 月
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS 記錄不包含 Windows Server 2012 R2 中帳戶鎖定案例的用戶端 IP 位址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020： Active Directory 同盟服務的安全性更新，以解決拒絕服務：2016年2月9日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|當 Windows Server 2012 R2 AD FS Server 中啟用裝置驗證時，無法存取應用程式|2015 年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|當使用者在 Windows Server 2012 R2 中使用 MFA 時，重複載入頁面並驗證失敗 AD FS|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|當 MFA adapter 在 Windows Server 2012 R2 中擲回例外狀況時，AD FS 不會呼叫 OnError|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|當您在 Windows Server 2012 R2 中新增或移除宣告提供者之後，次要 AD FS 伺服器上的信任關係會遺失|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|找不到非宣告感知信賴憑證者信任的主領域探索無法正常運作|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新在 Windows Server 2012 R2 的 AD FS 權杖中新增了複合識別碼宣告的支援|2015 年 5 月
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040： Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|當您透過 Windows Server 2012 R2 中的 WAP 開啟共用信箱時，出現「HTTP 400-不正確的要求」錯誤|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|AD FS Windows Server 2012 R2 中的 Web 應用程式 Proxy 驗證權杖的權杖重新執行保護|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|更新密碼功能的修正程式，讓使用者不需要在 Windows Server 2012 R2 中使用已註冊的裝置|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS 無法在 Windows Server 2012 R2 中處理 SAML 回應|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|當您嘗試透過 Windows Server 2012 R2 中的 Web 應用程式 Proxy 儲存 Office 檔案時，操作會失敗|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|當您使用不正確的使用者名稱登入 Windows Server 2012 R2 時，不會再次提示您輸入使用者名稱|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|當您在 Windows Server 2012 R2 中執行 web 應用程式時，系統會提示您進行驗證 AD FS|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|在 Windows Server 2012 R2 中初始部署裝置註冊服務之後的超時失敗|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|當您從內部網路存取 Windows Server 2012 R2 AD FS Server 時，系統會提示您輸入使用者名稱和密碼兩次|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新彙總套件|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新彙總套件|November 2014
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新彙總套件|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新彙總套件|2014 年 7 月
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新彙總套件|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新彙總套件|2014 年 5 月
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新彙總套件|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Windows Server 2012 中的 AD FS 更新 (AD FS 2.1) 和 AD FS 2。0
以下是已針對 AD FS 2.0 和2.1 發行的修補程式和更新彙總套件清單。

|K b# |描述|發行日期|適用於：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|在 Windows Server 2012 中透過 proxy 進行驗證失敗 (這是修正程式3094446的一般版本) |2016年11月品質匯總套件|AD FS 2。1
|[3197869](https://support.microsoft.com/kb/3197869)|在 Windows Server 2008 R2 SP1 中透過 proxy 進行驗證失敗 (這是修正程式3094446的一般版本) |2016年11月品質匯總套件|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|在 Windows Server 2012 或 Windows Server 2008 R2 SP1 中透過 proxy 進行驗證失敗|2015 年 9 月|AD FS 2.0 和2。1
|[3070078](https://support.microsoft.com/kb/3070078)|當您在 Windows Server 2012 中針對加密憑證進行驗證時，AD FS 2.1 擲回例外狀況|2015 年 7 月|AD FS 2。1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062： Active Directory federation services 中的弱點可能會允許權限提高|2015 年 6 月|AD FS 2.0/2。1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077： Active Directory 同盟服務中的弱點可能會允許資訊洩漏：2015年4月14日|November 2014|AD FS 2.0/2。1
|[2987843](https://support.microsoft.com/kb/2987843)|當許多使用者登入 Windows Server 2012 中的 web 應用程式時，AD FS 同盟伺服器的記憶體使用量會持續增加|2014 年 7 月|AD FS 2。1
|[2957619](https://support.microsoft.com/kb/2957619)|對委派的權杖進行 AD FS 的要求時，AD FS 中的信賴憑證者信任已停止|2014 年 5 月|AD FS 2。1
|[2926658](https://support.microsoft.com/kb/2926658)|如果您沒有 SQL 許可權，ADFS SQL 伺服器陣列部署將會失敗|2014 年 10 月|AD FS 2。1
|[2896713](https://support.microsoft.com/kb/2896713) 或 [2989956](https://support.microsoft.com/kb/2989956)|在 AD FS 伺服器上安裝安全性更新2843638之後，可使用更新來修正數個問題|2013年11月</br></br>2014 年 9 月|AD FS 2.0/2。1
|[2877424](https://support.microsoft.com/kb/2877424)|更新可讓您在 AD FS 2.1 伺服器陣列中的多個信賴憑證者信任中使用一個憑證|2013 年 10 月|AD FS 2。1
|[2873168](https://support.microsoft.com/kb/2873168)|修正：當您使用協力廠商 CSP 和 HSM，然後在 Windows Server 2008 R2 Service Pack 1 上的 AD FS 2.0 的更新彙總套件3中設定宣告提供者信任時，就會發生錯誤。|2013年9月|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|加密憑證的主體名稱中的逗號會導致 Windows Server 2008 R2 SP1 中發生例外狀況|2013 年 8 月|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|有價證券Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2013年11月|AD FS 2。1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066： Active Directory 同盟服務2.0 的安全性更新描述：2013年8月13日|2013 年 8 月|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata.xml 檔案未包含 Windows Server 2012 中 WS-Trust 和 WS-Federation 端點的 MEX 端點資訊|2013 年 5 月|AD FS 2。1
|[2790338](https://support.microsoft.com/kb/2790338)|描述 Active Directory 同盟服務 (AD FS) 2.0 的更新彙總套件3|2013 年 3 月|AD FS 2.0
