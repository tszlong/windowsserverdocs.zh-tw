---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: "Active Directory 同盟服務 (AD FS) 所需的更新"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Active Directory 同盟服務 (AD FS) 和 Web 應用程式 Proxy (WAP) 所需的更新

>適用於： Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2 SP1

截至 2016 年 10 月 Windows Server 的所有元件的所有更新的都發行只透過 Windows Update （wu 提供修正檔）。  有任何其他 hotfix 或個人下載項目。
這適用於 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 和 Windows Server 2008 R2 SP1。

此頁面列出最重要的彙總套件 AD FS 和 WAP，以及歷史 hotfix AD FS 和 WAP 建議的更新的清單。

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>AD FS 和 WAP 在 Windows Server 2016 的更新
Windows Server 2016 的更新每個月透過 Windows Update 傳遞和是累計的。 下列更新套件建議的所有 AD FS 和 WAP 2016 伺服器，並包含先前所需的所有更新，以及最新的修正。

|KB # |描述|發行日期
|----- | ----- |-----
|[4041688（作業系統組建 14393.1794）](https://support.microsoft.com/kb/4041688)|此修正程式位址間歇性 misdirects AD 授權請求身分錯誤提供者，因為不正確的快取行為的問題。 這可以影響像使用多監視器因素驗證驗證功能。 </br></br>新增的功能的 AAD 連接健康報告 ADFS 伺服器健康混合 WS2012R2 和 WS2016 ADFS 農場上正確精確度（使用 [詳細資訊稽核）使用。</br></br>有許多信賴廠商信任時，以提升發電廠行為 powershell cmdlet 位置期間升級 2012 R2 ADFS 伺服陣列 ADFS 2016 修正問題，失敗逾時。</br></br>處理 AD FS 位置修改 wct 參數值聯盟要求到其他的安全性權杖伺服器 (STS) 時導致驗證失敗的問題。|2017 年 10 月|
|[4038801（作業系統組建 14393.1737）](https://support.microsoft.com/kb/4038801)|新增 OIDC 登出使用聯盟的 LDPs 的支援。 這可讓「Kiosk 案例「位置多位使用者可能會依序登入單一裝置與 LDP 聯盟的地方。</br></br>已修正位置 CEP 日 CES 根據憑證問題不適用於 gMSA 帳號 WinHello。</br></br>若要同步部分設定，例如 IdentityServerPolicy.Scopes 和 IdentityServerPolicy.Clients 表格 ApplicationGroupId 欄失敗 Windows 內部資料庫 (WID) 在 Windows Server 2016 ADFS 伺服器上的位置修正的問題）因為外部按鍵限制。 這類同步失敗可以造成不同宣告、取得提供者和應用程式到第二個主要 ADFS 伺服器之間的體驗。 此外，如果 WID 的主要職務移到第二個節點中，應用程式群組將不會 ADFS 管理細微的</br></br>這個更新可以修正問題位置多因素驗證無法正確運作的行動裝置版的裝置使用自訂文化的群島定義|2017 年 9 月|
|[4034661（作業系統組建 14393.1613）](https://support.microsoft.com/kb/4034661)|修正問題的本機號碼 IP 位址所在 nog 411 事件 ADFS 4.0 的安全性事件登入來登入 \ 即使讓「成功稽核」及「失敗稽核「Windows Server 2016 RS1 ADFS 伺服器。</br></br>此修正程式在時 ADFX 伺服器設定為 HTTP proxy 伺服器位址使用 Azure 多因數驗證 (MFA) 的問題。</br></br>「處理位置簡報過期或撤銷憑證 ADFS Proxy 伺服器不會傳回錯誤給使用者的問題。」|2017 年 8 月|
|[4034658（作業系統組建 14393.1593）](https://support.microsoft.com/kb/4034658)|修正 2016 ad FS 伺服器為了支援 Windows Hello 適用於商務的 MFA 憑證註冊上 prem 部署|2017 年 8 月| 
|[4025334（作業系統組建 14393.1532）](https://support.microsoft.com/kb/4025334)|處理位置 PkeyAuth 權杖處理常式如果可能會失敗驗證 pkeyauth 要求包含不正確資料的問題。 執行裝置驗證不應該繼續驗證|2017 年 7 月|
|[4022723（作業系統組建 14393.1378）](https://support.microsoft.com/kb/4022723)|[Proxy web 應用程式]DisableHttpOnlyCookieProtection 設定屬性的值不是收取的 WAP 2016 在 2016 年 2012R2 日混合部署 </br></br>[Proxy web 應用程式]無法從 AD FS EAS 的預先授權案例中取得使用者存取預付碼。</br></br>AD FS 2016: WSFED sign-out 導致例外|2017 年 6 月 
|[3213986](https://support.microsoft.com/kb/3213986)|適用於 x64 系統 (KB3213986) 的 Windows Server 2016 的累積更新| 2017 年 1 月

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>AD FS 和 WAP 在 Windows Server 2012 R2 的更新
以下是清單 hotfix 和更新彙總套件的已在 Windows Server 2012 R2 推出的 Active Directory 同盟 Services (AD FS)。

|KB # |描述|發行日期
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|處理 AD FS 問題要求標頭的 MSISConext cookie 可以最後溢位標頭大小上限，HTTP 狀態碼 400 造成驗證失敗「不良要求-標頭太長的時間」。</br></br>已修正位置 ADFS 可以再忽略」提示 = 登入「在驗證期間發生問題。 還原的案例可非密碼驗證加入」已停用] 的選項。|2017 年 10 月更新彙總套件預覽|
|[4019217](https://support.microsoft.com/kb/4019217)|工作的資料夾使用 Server 2012 R2 AD FS 伺服器用權杖代理人無法運作|2017 年 Preview 更新彙總套件| 
|[4015550](https://support.microsoft.com/kb/4015550)|AD FS 不驗證外部使用者和 AD FS WAP 向前要求隨機無法修正問題|2017 年 4 月更新彙總套件| 
|[4015547](https://support.microsoft.com/kb/4015547)|AD FS 不驗證外部使用者和 AD FS WAP 向前要求隨機無法修正問題|2017 年 4 月安全性更新| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 此安全性更新解析 Active Directory 同盟服務 (ADFS) 中的弱點。 可能會允許資訊洩漏如果攻擊蓄意將要求傳送給 AD FS 伺服器，讓攻擊者讀取機密目標系統的相關資訊。|2017 年 3 月更新彙總套件| 
|[3179574](https://support.microsoft.com/kb/3179574)|AD FS 外部網路密碼更新以修正問題。 |2016 年 8 月更新彙總套件
|[3172614](https://support.microsoft.com/kb/3172614)|引入命令提示字元中 = 登入[支援](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7)，AD FS 管理主控台與 AlwaysRequireAuthentication 設定修正問題。 |2016 年 7 月更新彙總套件
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory 同盟 Services (AD FS) 3.0 無法連接到使用安全通訊端層 (SSL) 連接埠 636 或 3269 連接字串中的設定輕量型 Directory 存取通訊協定 (LDAP) 屬性存放區。 |2016 年 6 月更新彙總套件
|[3148533](https://support.microsoft.com/kb/3148533)|MFA 回溯驗證失敗透過 ADFS Proxy 在 Windows Server 2012 R2 |2016 年
|[3134787](https://support.microsoft.com/kb/3134787)|AD FS 登不包含在 Windows Server 2012 R2 account 鎖定案例 client IP 位址 |2016 年 2 月
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020： 安全性更新 Active Directory 同盟服務的地址阻斷服務： 2016 年 2 月 9 日|2016 年 2 月
|[3105881](https://support.microsoft.com/kb/3105881)|在 Windows Server 2012 R2 型 AD FS 伺服器支援的裝置驗證時，無法存取應用程式|2015 年 10 月
|[3092003](https://support.microsoft.com/kb/3092003)|重複載入頁面，並驗證失敗時的使用者在 Windows Server 2012 R2 AD FS 使用 MFA|2015 年 8 月
|[3080778](https://support.microsoft.com/kb/3080778)|AD FS 不會呼叫 on Error 時 MFA 介面卡擲在 Windows Server 2012 R2 回例外|2015 年 7 月
|[3075610](https://support.microsoft.com/kb/3075610)|信任關係的遺失次要 AD FS 伺服器上之後您新增或移除宣告提供者，在 Windows Server 2012 R2|2015 年 7 月
|[3070080](https://support.microsoft.com/kb/3070080)|家用領域探索的非-宣告注意可以方信任無法運作|2015 年 6 月
|[3052122](https://support.microsoft.com/kb/3052122)|更新的 Windows Server 2012 R2 AD FS 權杖中新增支援複合 ID 宣告|2015 年 5 月
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2015 年 4 月
|[3042127](https://support.microsoft.com/kb/3042127)|「 HTTP 400-錯誤的要求，「 您在 Windows Server 2012 R2 開放 WAP 透過共用的信箱時發生錯誤|2015 年 3 月
|[3042121](https://support.microsoft.com/kb/3042121)|在 Windows Server 2012 R2 Web 應用程式 Proxy 驗證權杖 AD FS 權杖重播保護|2015 年 3 月
|[3035025](https://support.microsoft.com/kb/3035025)|Hotfix 更新密碼的功能，讓使用者不必使用在 Windows Server 2012 R2 的且已的裝置|2015 年 1 月
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS 無法處理 SAML 回應在 Windows Server 2012 R2|2015 年 1 月
|[3025080](https://support.microsoft.com/kb/3025080)|當您嘗試透過 Web 應用程式 Proxy Office 檔案儲存在 Windows Server 2012 R2 失敗|2015 年 1 月
|[3025078](https://support.microsoft.com/kb/3025078)|您不會提示輸入使用者名稱再次當您登入 Windows Server 2012 R2 使用正確的使用者名稱|2015 年 1 月
|[3020813](https://support.microsoft.com/kb/3020813)|系統提示您輸入驗證當您執行 Windows Server 2012 R2 AD FS web 應用程式|2015 年 1 月
|[3020773](https://support.microsoft.com/kb/3020773)|在 Windows Server 2012 R2 裝置登記服務的初始部署後逾時錯誤|2015 年 1 月
|[3018886](https://support.microsoft.com/kb/3018886)|系統提示您輸入使用者名稱和密碼兩次當您從內部網路存取 Windows Server 2012 R2 AD FS 伺服器|2015 年 1 月
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 更新彙總|2014 年 12 月
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 更新彙總|2014 年 11 月
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 更新彙總|2014 年 8 月
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 更新彙總|2014 年 7 月
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 更新彙總|2014 年 6 月
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 更新彙總|2014 年 5 月
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 更新彙總|2014 年 4 月

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>更新 AD FS 在 Windows Server 2012 (AD FS 2.1) 以及 AD FS 2.0
以下是清單 hotfix 和更新彙總套件所推出的 2.0 和 2.1 AD FS。

|KB # |描述|發行日期|適用於：
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|透過 proxy 驗證失敗 （這是發行一般 hotfix 3094446） 的 Windows Server 2012 中|2016 年 11 月品質彙總套件|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|透過 proxy 驗證無法在 Windows Server 2008 R2 SP1 （這是 hotfix 3094446 一般版本）|2016 年 11 月品質彙總套件|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|Windows Server 2012 或 Windows Server 2008 R2 SP1 的透過 proxy 驗證失敗|2015 年 9 月|AD FS 2.0 和 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|當您在 Windows Server 2012 中的加密憑證驗證，AD FS 2.1 擲例外|2015 年 7 月|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062: Active Directory 同盟服務中的弱點可能會允許提高權限|2015 年 6 月|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Active Directory 同盟服務中的弱點可能會允許資訊洩漏： 2015 年 4 月 14 日|2014 年 11 月|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|許多使用者登入 Windows Server 2012 中 web 應用程式會保留增加 AD FS 聯盟伺服器記憶體使用量|2014 年 7 月|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|AD FS 信賴廠商信任停止委派權杖 AD FS 當要求|2014 年 5 月|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|ADFS SQL 發電廠部署失敗時，如果您不需要 SQL 權限|2014 年 10 月|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713)或[2989956](https://support.microsoft.com/kb/2989956)|更新可提供給 AD FS 伺服器上安裝的安全性更新 2843638 之後修正一些問題|2013 年 11 月</br></br>2014 年 9 月|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|更新可讓您使用多個 Relying 派對中 AD FS 2.1 發電廠信任憑證|2013 年 10 月|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|修正： 就會發生錯誤當您使用協力廠商 CSP 和 HSM，然後設定 Windows Server 2008 R2 Service Pack 1 AD FS 2.0 宣告提供者信任更新彙總套件 3|2013 年 9 月|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|在主體名稱的加密憑證逗號例外導致 Windows Server 2008 R2 SP1|2013 年 8 月|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[安全性]Active Directory 同盟服務中的弱點可能會允許資訊洩漏|2013 年 11 月|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|Active Directory 同盟服務 2.0 的安全性更新 MS13-066： 描述： 2013 年 8 月 13 日|2013 年 8 月|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Federationmetadata.xml 檔案不包含 Windows Server 2012 中 Ws-trust 和 WS 同盟端點 MEX 端點資訊|2013 年|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|Active Directory 同盟服務 (AD FS) 更新彙總套件 3 描述 2.0|2013 年 3 月|AD FS 2.0




