---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: Active Directory Federation Services (AD FS) 所需的更新
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 3/29/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2336847825cfb3f232674a1e39d3bab7953a32c0
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67792305"
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>必要的更新，適用於 Active Directory Federation Services (AD FS) 和 Web 應用程式 Proxy (WAP)


自 2016 年 10 月起的 Windows Server 的所有元件的所有更新會都發行只透過 Windows Update (WU)。  沒有任何其他 hotfix 或個別的下載項目。
這適用於 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 SP1。

此頁面列出 AD FS 和 WAP，以及建議用於 AD FS 和 WAP 的 hotfix 更新歷程記錄清單特別感興趣的彙總套件。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>適用於 AD FS 和 WAP 中 Windows Server 2016 更新
適用於 Windows Server 2016 的更新會透過 Windows Update 的每個月傳遞，並會累計。 下面所列的更新套件建議用於所有的 AD FS 和 WAP 2016 伺服器，並包含所有先前所需的更新，以及最新的修正。

|KB # |描述|發行日期
|----- | ----- |-----
|[CVE-2019-1126](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1126) | 這項安全性更新可解決的弱點可能會在 Active Directory 同盟服務 (AD FS) 可能會允許攻擊者略過外部網路鎖定原則。 |2019 年 7 月|
|[4489889 （OS 組建 14393.2879）](https://support.microsoft.com/help/4489889/windows-10-update-kb4489889) | 解決在 Active Directory Federation Services (AD FS) 會造成信賴憑證者信任的 AD FS 管理主控台中出現重複的問題。 這是當您建立或檢視信賴憑證者信任使用 AD FS 管理主控台。 |2019 年 3 月|
|[4487006 （OS 組建 14393.2828）](https://support.microsoft.com/help/4487006/windows-10-update-kb4487006) | 解決的問題造成的信賴憑證者信任時使用 PowerShell 或 [Active Directory Federation Services (AD FS) 的管理] 主控台中選取失敗的更新。 如果您設定要使用線上中繼資料 URL 發佈一個以上的 PassiveRequestorEndpoint 信賴憑證者信任，就會發生此問題。 錯誤是，「 MSIS7615:中的信賴憑證者信任指定的受信任的端點必須是唯一的該信賴憑證者信任。 」  </br></br>解決由於 Azure 密碼保護原則會顯示特定的錯誤訊息外部的複雜密碼變更的問題。 |2019 年 2 月|
|[4462928 （OS 組建 14393.2580）](https://support.microsoft.com/help/4462928/windows-10-update-kb4462928)|解決 Active Directory Federation Services (ADFS) 外部網路智慧鎖定 (ESL) 與替代的登入識別碼之間的互通性問題 啟用替代登入識別碼時，會呼叫至 AD FS Powershell cmdlet，取得 AdfsAccountActivity 和重設-AdfsAccountLockout，傳回 「 找不到帳戶 」 錯誤。 組 AdfsAccountActivity 呼叫時，而不是編輯現有的 fgpp 加入新項目。|2018 年 10 月|
|[4343884 （OS 組建 14393.2457）](https://support.microsoft.com/en-us/help/4343884/windows-10-update-kb4343884)|解決其中 Multi-factor Authentication 無法正常運作的行動裝置使用的自訂文化特性定義的 Active Directory Federation Services (AD FS) 問題。 </br></br>解決的問題，在 Windows hello 企業版的新使用者註冊會造成明顯的延遲 （15 秒）。 硬體安全性模組用來儲存 ADFS 登錄授權單位 (RA) 憑證時，就會發生此問題。|2018 年 8 月|
|[4338822 （OS 組建 14393.2395）](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822)|解決顯示重複的信賴憑證者信任，在建立或檢視從主控台的 信賴憑證者信任時，才會進行 AD FS 管理主控台中的 AD FS 中的問題。</br></br>解決造成 Windows hello 企業版失敗的 ADFS 中的問題。 有兩個宣告提供者時，就會發生此問題。 Pin 碼註冊將會失敗，「 400 的內部伺服器錯誤：無法取得裝置識別碼。 」</br></br> 解決與非作用中連線指永不結束 WAP 問題。 這會導致系統資源流失 （例如記憶體流失），並不再有回應的 WAP 服務。 解決選取不同的登入選項可防止使用者的 AD FS 問題。 會發生這種情況是當使用者選擇使用憑證型驗證，登入，但尚未設定。 如果使用者選取憑證型驗證，然後再嘗試選取另一個登入選項，這也會發生。 如果發生這種情況，則使用者將重新導向，以憑證型驗證頁面之前就關閉瀏覽器。|2018 年 7 月|
|[4103720 （OS 組建 14393.2273）](https://support.microsoft.com/help/4103720/windows-10-update-kb4103720)|解決與 SAML 信賴憑證者合作對象時啟用 PreventTokenReplays 失敗導致 IdP 起始的登入的 ADFS 的問題。 </br></br>解決從裝置或瀏覽器應用程式的 OAUTH 驗證時，就會發生的 ADFS 問題。 使用者密碼變更會產生失敗，並要求使用者結束應用程式或瀏覽器來登入。 </br></br>解決的問題，讓外部網路的智慧鎖定，以 UTC + 1 和更高版本 （歐洲和亞洲） 無法運作。 此外，它會導致一般的外部網路鎖定失敗，發生下列錯誤：Get-AdfsAccountActivity:大於 DateTime.MaxValue 或小於 DateTime.MinValue 時轉換為 UTC 的日期時間值不能序列化為 JSON。</br></br>解決 ADFS Windows Hello 以新的使用者不能佈建他們的 pin 碼的商務問題。 會發生這種情況是當沒有 MFA 提供者設定。|2018 年 5 月|
|[4093120 （OS 組建 14393.2214）](https://support.microsoft.com/help/4093120/windows-10-update-kb4093120)| 解決的未處理的重新整理權杖的驗證問題。 它會產生下列錯誤：“Microsoft.IdentityServer.Web.Protocols.OAuth.Exceptions.OAuthInvalidRefreshTokenException:MSIS9312:收到無效的 OAuth 重新整理權杖。 重新整理權杖已收到早於 在權杖中允許的時間。 」 |2018 年 4 月|
|[4077525 （OS 組建 14393.2097）](https://support.microsoft.com/help/4077525/windows-10-update-kb4077525)|解決 HTTP 500 錯誤的 ADFS 伺服器陣列中有至少兩部伺服器使用 Windows 內部資料庫 (WID) 發生的問題。 在此案例中，HTTP 的基本預先驗證 Web 應用程式 Proxy (WAP) 伺服器無法驗證一些使用者。 發生錯誤時，您也可能會看到警告 WAP 事件記錄檔中的事件識別碼 13039 Microsoft Windows Web 應用程式 Proxy。 描述讀取，「 Web 應用程式 Proxy 無法驗證使用者。 預先驗證是 「 ADFS 的豐富型用戶端 」。 指定的使用者未獲授權存取給定信賴憑證者的合作對象。 授權規則的目標信賴憑證者合作對象或 WAP 信賴憑證者合作對象被需要加以修改。 」</br></br>解決的問題所在 AD FS 可以不再忽略提示在驗證期間 = 登入。 已停用選項已新增至支援哪些密碼中不使用驗證的案例。 如需詳細資訊，請參閱 < AD FS 驗證 Windows Server 2016 rtm 期間忽略"prompt = login"的參數。</br></br>AD FS 中解決問題，其中授權的客戶 （以及信賴憑證者的合作對象） 使用者選取憑證驗證選項將無法連線。 使用提示字元時，就會發生失敗 = 登入，如果已啟用 Windows 整合式驗證 (WIA)，且要求可以執行 WIA。</br></br>解決的問題，AD FS 不正確地顯示首頁領域探索 (HRD) 頁面與信賴憑證者 (RP) 中的 OAuth 群組相關聯的身分識別提供者 (IDP) 時。 多個 Idp 是 OAuth 群組中的 RP 與相關聯，除非使用者將不會顯示 [HRD] 頁面。 相反地，使用者會直接移至相關聯的 IDP 進行驗證。|2018 年 2 月|
|[4041688 （OS 組建 14393.1794）](https://support.microsoft.com/kb/4041688)|此修正解決間歇性 misdirects 錯誤的身分識別提供者的 AD 授權單位要求，因為不正確的快取行為的問題。 這可以影響驗證功能，例如多重要素驗證。 </br></br>新增的功能 AAD Connect Health for ADFS 伺服器健全狀況報表與在混合的 WS2012R2 和 WS2016 ADFS 伺服器陣列上正確精確度 （使用設定稽核的詳細資訊）。</br></br>有許多信賴憑證者信任時，powershell cmdlet，以提升伺服器陣列的行為，2012年升級期間 R2 ADFS 伺服器陣列至 ADFS 2016 的情況下修正的問題，失敗逾時。</br></br>解決的問題，AD FS 會導致驗證失敗時同盟要求至其他安全性權杖伺服器 (STS) 修改的 wct 參數值。|2017 年 10 月|
|[4038801 （OS 組建 14393.1737）](https://support.microsoft.com/kb/4038801)|新增同盟的 LDPs 的 OIDC 登出的支援。 這可讓 「 Kiosk 案例 」 其中多個使用者可能會以序列方式登入單一裝置沒有搭配 LDP 的同盟。</br></br>已修正的問題，CEP/CES 型憑證不適用於 gMSA 帳戶 WinHello。</br></br>將 Windows 內部資料庫 (WID) 在 microsoft Windows Server 2016 ADFS 伺服器上無法同步處理某些設定，例如從 IdentityServerPolicy.Scopes 和 IdentityServerPolicy.Clients 資料表 ApplicationGroupId 資料行，修正問題） 因為外部索引鍵條件約束。 這類的同步處理失敗可導致不同的宣告，宣告提供者和應用程式主要到次要的 ADFS 伺服器之間的體驗。 此外，如果 WID 主要角色移至次要節點，應用程式群組將不再可管理在 ADFS 管理 ux。</br></br>此更新修正問題，多重要素驗證無法正常運作的行動裝置使用的自訂文化特性定義|2017 年 9 月|
|[4034661 （OS 組建 14393.1613）](https://support.microsoft.com/kb/4034661)|呼叫端 IP 位址是 nog 411 ADFS 4.0 的安全性事件記錄檔中的事件所記錄的位置，修正問題 \ 即使在啟用 [成功稽核] 和 [失敗稽核] 之後的 microsoft Windows Server 2016 RS1 ADFS 伺服器。</br></br>ADFX 伺服器設定為使用 HTTP Proxy 時，此修正會解決的問題與 Azure 多重要素驗證 (MFA)。</br></br>「 解決其中呈現過期或撤銷的憑證，ADFS Proxy 伺服器不會傳回錯誤給使用者的問題。 」|2017 年 8 月|
|[4034658 （OS 組建 14393.1593）](https://support.microsoft.com/kb/4034658)|修正 2016 AD FS 伺服器以支援 Windows Hello For Business 的內部部署部署 MFA 憑證註冊|2017 年 8 月|
|[4025334 （OS 組建 14393.1532）](https://support.microsoft.com/kb/4025334)|解決的問題其中 PkeyAuth 權杖處理常式無法驗證如果 pkeyauth 要求包含不正確的資料。 驗證應該還是會繼續而不需執行裝置驗證|2017 年 7 月|
|[4022723 （OS 組建 14393.1378）](https://support.microsoft.com/kb/4022723)|[Web 應用程式 Proxy]DisableHttpOnlyCookieProtection 組態屬性的值是未收取 WAP 2016 2012R2/2016年混合式部署中 </br></br>[Web 應用程式 Proxy]無法取得使用者存取權杖來自 EAS 預先驗證案例中的 AD FS。</br></br>AD FS 2016:登出 WSFED 會導致例外狀況|2017 年 6 月
|[3213986](https://support.microsoft.com/kb/3213986)|適用於 x64 型系統 (KB3213986) 適用於 Windows Server 2016 的累積更新| 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>適用於 AD FS 和 WAP 中 Windows Server 2012 R2 更新
以下是 hotfix 和更新彙總套件已在 Windows Server 2012 R2 中發行的 Active Directory Federation Services (AD FS) 的清單。

|KB # |描述|發行日期
|----- | ----- |-----
|[4507448](https://support.microsoft.com/help/4507448/windows-8-1-update-kb4507448)| 這項安全性更新可解決的弱點可能會在 Active Directory 同盟服務 (AD FS) 可能會允許攻擊者略過外部網路鎖定原則。 |2019 年 7 月
|[4041685](https://support.microsoft.com/kb/4041685)|處理 AD FS 問題 MSISConext cookie 在要求標頭可以最終溢位的標頭大小限制而導致驗證失敗，HTTP 狀態碼 400 「 不正確的要求-標頭太長時間 」。</br></br>修正其中 ADFS 可以不再忽略"prompt = login 「 在驗證期間的問題。 [停用] 選項已新增至還原非密碼驗證使用的案例。|2017 年 10 月更新彙總套件預覽
|[4019217](https://support.microsoft.com/kb/4019217)|工作的資料夾使用 Server 2012 R2 AD FS 伺服器時，用戶端使用權杖訊息代理程式無法運作|2017 年預覽更新彙總套件
|[4015550](https://support.microsoft.com/kb/4015550)|已修正的問題與 AD FS 無法驗證外部使用者和 AD FS WAP 隨機失敗的要求轉送給|2017 年 4 月更新彙總套件
|[4015547](https://support.microsoft.com/kb/4015547)|已修正的問題與 AD FS 無法驗證外部使用者和 AD FS WAP 隨機失敗的要求轉送給|2017 年 4 月安全性更新
|[4012216](https://support.microsoft.com/kb/4009970)|MS17 019 這項安全性更新可解決的弱點可能會在 Active Directory Federation Services (ADFS)。 如果攻擊者特別設計將要求傳送至 AD FS 伺服器，讓攻擊者讀取目標系統的機密資訊的弱點可能會允許資訊洩漏。|2017 年 3 月更新彙總套件
|[3179574](https://support.microsoft.com/kb/3179574)|AD FS 外部網路的密碼更新已修正的問題。 |2016 年 8 月更新彙總套件
|[3172614](https://support.microsoft.com/kb/3172614)|導入的提示字元 = 登入[支援](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7)，AlwaysRequireAuthentication 設定 AD FS 管理主控台與已修正問題。 |2016 年 7 月更新彙總套件
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory Federation Services (AD FS) 3.0 無法連接到設定為使用安全通訊端層 (SSL) 連接埠 636 或 3269 連接字串中的輕量型目錄存取通訊協定 (LDAP) 屬性存放區。 |2016 年 6 月更新彙總套件
|[3148533](https://support.microsoft.com/kb/3148533)|透過 Windows Server 2012 R2 中的 ADFS Proxy 的 MFA 遞補驗證失敗 |2016 年 5 月
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS 記錄檔不包含 Windows Server 2012 R2 中的帳戶鎖定情況下的用戶端 IP 位址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16 020-MENS:安全性更新，Active Directory 同盟服務阻斷服務的位址：2016 年 2 月 9日日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|Windows Server 2012 r2 AD FS 伺服器中啟用裝置驗證時，無法存取應用程式|2015 年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|頁面載入重複，且驗證失敗，當使用者在 Windows Server 2012 R2 AD FS 使用 MFA|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|當 MFA 配接器擲回例外狀況，Windows Server 2012 R2 中 AD FS 不會呼叫 OnError|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|您新增或移除 Windows Server 2012 R2 中的宣告提供者之後，將會遺失次要的 AD FS 伺服器上的信任關係|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|主領域探索不正常的非宣告感知信賴憑證者信任|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新 Windows Server 2012 R2 中的 AD FS 權杖中新增的複合識別碼宣告的支援|5 月 2015
|[3045711](https://support.microsoft.com/kb/3045711)|與 MS15-040:在 Active Directory Federation Services 中的弱點可能會允許資訊洩漏|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|「 HTTP 400-不正確的要求 」 錯誤，當您開啟 Windows Server 2012 R2 中的 WAP 透過共用的信箱時|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|AD FS 權杖重新執行保護的 Windows Server 2012 R2 中的 Web 應用程式 Proxy 驗證權杖|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|Hotfix 更新密碼的功能，讓使用者不需要使用 Windows Server 2012 R2 中的已註冊的裝置|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS 無法處理 Windows Server 2012 R2 中的 SAML 回應|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|當您嘗試將透過 Web Application Proxy 將 Office 檔案儲存在 Windows Server 2012 R2 時，作業就會失敗|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|您不會提示使用者名稱一次當您使用不正確的使用者名稱登入 Windows Server 2012 R2|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|當您執行 Windows Server 2012 R2 AD FS 中的 web 應用程式時，會提示進行驗證|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|Windows Server 2012 R2 中的裝置註冊服務的初始部署之後逾時錯誤|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|系統會提示您輸入使用者名稱和密碼兩次當您從內部網路存取 Windows Server 2012 R2 AD FS 伺服器|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新彙總|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新彙總|2014 年 11 月
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新彙總|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新彙總|2014 年 7 月
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新彙總|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新彙總|2014 年
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新彙總|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>更新適用於 AD FS，在 Windows Server 2012 (AD FS 2.1) 和 AD FS 2.0
以下是已發行適用於 AD FS 2.0 和 2.1 的 hotfix 和更新彙總清單。

|KB # |描述|發行日期|適用於：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|透過 proxy 驗證失敗 （這是一般的 hotfix 3094446 版本） 的 Windows Server 2012 中|2016 年 11 月品質彙總套件|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|透過 proxy 驗證失敗 （這是一般的 hotfix 3094446 版本） 的 Windows Server 2008 R2 sp1|2016 年 11 月品質彙總套件|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|透過 proxy 驗證失敗，在 Windows Server 2012 或 Windows Server 2008 R2 SP1|2015 年 9 月|AD FS 2.0 和 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|AD FS 2.1 在驗證對 Windows Server 2012 中的加密憑證時，會擲回例外狀況|2015 年 7 月|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|與 MS15-062:Active Directory federation services 中的弱點可能會允許提高權限|2015 年 6 月|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077:Active Directory Federation Services 的弱點可能會允許資訊洩漏：2015 年 4 月 14日日|2014 年 11 月|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|AD FS 同盟伺服器的記憶體使用量會持續增加，許多使用者登入 Windows Server 2012 中的 web 應用程式時|2014 年 7 月|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|委派的語彙基元的 AD FS 來提出要求時，已停止的信賴憑證者信任的 AD fs|2014 年|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|如果您沒有 SQL 權限，就會失敗 ADFS SQL 伺服器陣列部署|2014 年 10 月|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713)或[2989956](https://support.microsoft.com/kb/2989956)|更新已提供修正幾個問題之後您的 AD FS 伺服器上安裝安全性更新 2843638|2013 年 11 月</br></br>2014 年 9 月|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|更新可讓您使用一個憑證，進行多個信賴憑證者信任的 AD FS 中 2.1 的伺服器陣列|2013 年 10 月|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|修正：當您使用第三方 CSP 和 HSM，然後再設定適用於 Windows Server 2008 R2 Service Pack 1 上的 AD FS 2.0 的 更新彙總套件 3 中的宣告提供者信任時，就會發生錯誤|2013 年 9 月|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|加密憑證的主體名稱的逗號會造成 Windows Server 2008 R2 SP1 中的例外狀況|2013 年 8 月|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Security]在 Active Directory Federation Services 中的弱點可能會允許資訊洩漏|2013 年 11 月|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13 066:Active Directory Federation Services 2.0 的安全性更新的描述：2013 年 8 月 13日日|2013 年 8 月|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata.xml 檔案不包含 Windows Server 2012 中 1.1、WS-Trust 和 WS-同盟端點的 MEX 端點資訊|2013 年|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|更新彙總套件 3 描述 Active Directory Federation Services (AD FS) 2.0|2013 年 3 月|AD FS 2.0
