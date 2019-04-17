---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: "設定其他登入 ID"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 12/04/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb4c98ff1090a9d3c35654614a43e12db99d691d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="configuring-alternate-login-id"></a>設定其他登入 ID

>適用於：Windows Server 2016、Windows Server 2012 R2

使用者 Active Directory 同盟 Services (AD FS) 功能的應用程式使用任何形式的使用者識別碼接受 Active Directory Domain Services (AD DS)，可以登入。 其中包括使用者主體名稱 (Upn) (johndoe@contoso.com) 或網域完整坡-account 名稱（contoso\johndoe 或 contoso.com\johndoe）。

在某些環境，公司的原則或先業務的應用程式相依性，因為終端使用者只可能會注意到他們的電子郵件地址並不他們 UPN 或坡-帳號名稱。 有時候，UPN 也是非路由 (jdoe@contoso.local) 和僅適用於驗證到企業網路上的應用程式。

自從非路由網域 (ex。 Contoso.local) 擁有無法驗證，Office 365 需要所有使用者登入是完整 internet 路由 Id。 如果在場所 UPN 使用非路由網域 (ex。 Contoso.local)，或無法變更現有 UPN 因為本機應用程式相依性，我們建議使用其他登入收到設定 ID 登入其他可讓您的體驗中設定登入的使用者可以登入屬性以外他們 UPN，例如電子郵件的位置。

這項功能的優勢時，它可以讓您收養 SaaS 提供者，例如不需要將修改您先 Upn Office 365。 它也可讓您的業務服務消費者提供身分應用程式的支援。

> [!IMPORTANT]
> 使用商務用在混合的環境 Exchange 和/或 Skype ID 替代是支援，但不是建議這樣做。 使用的憑證 (例如 UPN) 上場所和 online 提供最佳在混合的環境中的使用者體驗。  Microsoft 建議針對變更其 Upn 盡可能避免替代 id 需要 使用商務用 Lync 或 Skype ID 替代需要 Lync Server 2013 或更新版本。 針對使用替代 ID 應考慮[現代化驗證](https://support.office.com/article/using-office-365-modern-authentication-with-office-clients-776c0036-66fd-41cb-8928-5495c0f9168a)的換貨中 Office 365 更佳的使用體驗。 此外，針對使用商務用 Skype 與行動裝置版戶端必須確保 SIP 地址使用者的電子郵件地址（及其他 ID）。 

請參考下表中的使用者體驗替代一般驗證，驗證現代化與憑證驗證（需要讓現代化驗證）中使用各種不同的 Office 365 戶端的來電顯示。

|**Client 類型**|**其他資訊**|**支援聲明-一般和現代化驗證**|**描述**|
|--------------------|------------------------------|------------------------------|------------------------------|
|Outlook|一般驗證：您必須在 [加入網域的電腦，並連接到企業網路<br /><br />現代化驗證：支援|您只能使用替代 ID 不允許外部存取信箱使用者的環境中。 這表示的使用者可以僅限進行驗證有信箱支援的方式連接和加入到企業網路，在 VPN，或透過直接存取連接時。 如果您選擇設定現代化驗證（也就是 ADAL），您可以使用 Outlook 的非網域中加入日連接的電腦，但設定您的設定檔 Outlook 時，您將會取得數個額外的提示。<br /><br />查看第一的使用者體驗示範表格下映像。|[現代化驗證，Office 2013 中](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|混合公用資料夾|不支援一般驗證：<br /><br />現代化驗證：支援|混合公用資料夾無法展開如果替代 ID 可用，因此不應該使用今天使用一般的驗證方法。 如果您想要無法使用公用混合資料夾您必須設定現代化驗證（也就是 ADAL）。<br /><br />查看第一的使用者體驗示範表格下映像。|[現代化驗證，Office 2013 中](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|跨場所委派|不支援|目前跨的場所在混合設定中，不支援的權限，但它們也會無法再使用 AltID。||
|保存信箱存取（信箱先-保存在雲端中）|支援|使用者存取封存時，將會取得認證額外的提示，他們就會有提供那里其他出現提示時的來電顯示。<br /><br />查看第一的使用者體驗示範表格下映像。||
|Office 365 Pro 加上啟用] 頁面|支援的側邊登錄 client 建議|與其他 ID 設定，您會看到先 UPN 在驗證欄位會預先填入。 這需要變更，以替代所使用的身分。 我們建議使用 client 側邊 reg 鍵連結欄中所述。<br /><br />查看第二個映像的使用者體驗示範表格下方。|[Office 2013 和 Lync 2013 定期提示您輸入認證 SharePoint Online、OneDrive 及 Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|Microsoft 小組|支援|Microsoft 團隊支援 AD FS (SAML-P、WS-Fed，Ws-trust，與 OAuth) 和現代化驗證。<br/><br/> 核心 Microsoft 團隊，例如頻道、聊天及檔案功能運作的 id Altnernate 登入。 </br></br>1 並 3 方應用程式可以分開客戶調查。 這是因為他們自己的性驗證通訊協定每個應用程式。| 
|商務用 Skype 日 Lync|支援（除非如上所述）但可能會對使用者造成混淆。  在行動裝置版戶端，替代 Id 平台 SIP 位址 = 電子郵件地址 = 替代編號。|使用者可能需要登入 Skype 兩次，適用於企業桌面 client，第一次使用先 UPN，然後使用 [替代編號。 （請注意，[登入位址」，可能不是「的使用者名稱」一樣，但通常是實際 SIP 地址）。  出現的使用者名稱的第一次提示時，使用者應該輸入 UPN，即使這不正確預先填入的替代 ID 或 SIP 地址。 登入與 UPN、使用者名稱命令提示字元中將會再出現這填入 UPN 一次按下後使用者。 這次使用者必須替代 ID 取代此並按一下 [登入以完成登入處理程序。 在行動裝置版戶端，使用者應該先使用者 ID 在 [進階] 頁面，使用輸入坡樣式格式使用者名稱，不 UPN 格式。<br /><br />成功登入之後 Lync 的商務用 Skype 標示為「換貨需要認證」時，如果您需要提供適用於信箱所在的認證。 如果您需要提供其他 ID 雲端信箱 如果信箱場所在您需要提供先 UPN。|[現代化驗證，Office 2013 中](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/)|
|Outlook Web Access|支援|||
|適用於 Android、IOS 和 Windows Phone outlook 行動裝置版的應用程式|支援|||
|商務用 OneDrive|支援的側邊登錄 client 建議|與其他 ID 設定，您會看到先 UPN 在驗證欄位會預先填入。 這需要變更，以替代所使用的身分。 我們建議使用 client 側邊 reg 鍵連結欄中所述。<br /><br />查看第二個映像的使用者體驗示範表格下方。|[Office 2013 和 Lync 2013 定期提示您輸入認證 SharePoint Online、OneDrive 及 Lync Online](https://support.microsoft.com/en-us/kb/2913639)|
|適用於行動裝置版 Client 的商務用 OneDrive|支援|||

![其他登入](media/Configure-Alternate-Login-ID/ADFS_Alt_ID1.png)

![其他登入](media/Configure-Alternate-Login-ID/ADFS_Alt_ID2.png)

![其他登入](media/Configure-Alternate-Login-ID/ADFS_Alt_ID3.png)

下列螢幕擷取畫面，，以下是使用商務用 Skype 其他範例。  在範例會使用下列資訊


- SIP:userA@contoso.com 
- UPN:userA@contoso.local
- 電子郵件：userA@contoso.com
- AltId:userA@contoso.com 

登入] 欄位中輸入 SIP 地址。

![Skype](media/Configure-Alternate-Login-ID/SkypeA.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeB.png)

![Skype](media/Configure-Alternate-Login-ID/SkypeC.png)

## <a name="to-configure-alternate-login-id"></a>若要設定 ID 其他登入
為了設定 ID 其他登入，您必須執行下列工作：

設定，ID 登入其他可讓您 AD FS 宣告提供者信任

1.  安裝[KB2919355](https://go.microsoft.com/fwlink/?LinkID=396590)。  您可以透過 Windows Update 服務取得或直接下載。

2.  更新的任何伺服器聯盟陣列中執行下列 PowerShell cmdlet AD FS 設定（如果您有 WID 發電廠，您必須執行這個命令陣列中主要 AD FS 伺服器上）：

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>

    ```

    **AlternateLoginID**您想要使用的登入屬性 LDAP 名稱。

    **LookupForests**是之子-森林屬於您的使用者 DNS 清單。

    若要以便其他登入 ID 功能，您必須設定-AlternateLoginID 和-LookupForests 參數非空值、有效的值。

    下列範例中，您的讓其他登入 ID 功能，例如您帳號 contoso.com 和 fabrikam.com 森林中的使用者可以登入 AD FS 功能的應用程式與他們「郵件」屬性。

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
    ```

3.  停用此功能，請將值設定為兩個參數為空值。

    ```
    Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
    ```

4.  要 ID 其他登入 Azure AD，請使用 Azure AD 連接需要額外的設定步驟。   您可以直接從精靈設定替代 ID。  查看唯一找出您的使用者] 區段底下[連接至 Azure AD](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/#connect-to-azure-ad)。

## <a name="additional-details--considerations"></a>其他詳細資料和注意事項

-   其他登入 ID 功能只有聯盟環境中部署 AD FS 進行。  不支援下列案例中：
    -   非路由網域 (例如 Contoso.local) 無法 Azure AD 來確認。
    -   管理部署 AD FS 不需要的環境。


-   當支援，其他登入 ID 功能目前僅適用於驗證使用者名稱/密碼上所有的使用者名稱/密碼驗證支援的通訊協定 AD FS (SAML-P、WS-Fed，Ws-trust，與 OAuth)。


-   Windows 整合驗證 (WIA) 執行時（例如，當使用者試著從內部網路存取加入網域的電腦上的公司應用程式和 AD FS 管理員已經設定使用內部 WIA 驗證原則），將會使用驗證 UPN。 如果您已經設定的任何理賠要求規則的其他登入 ID 功能信賴派對，您應該確定那些規則的 WIA 案例中仍然有效。

-   當支援，其他登入 ID 功能需要至少一個通用伺服器可從每個使用者 account 樹系 AD FS 支援 AD FS 伺服器。 瑞曲之戰使用者 account 森林中的通用伺服器會導致回到使用 UPN AD FS。 預設的網域控制站的通用伺服器。

-   時，如果 AD FS 伺服器以相同的其他登入 ID 值指定跨所有設定的使用者 account 樹系找到更多個使用者物件功能，它將無法登入。

-   AD FS 時其他登入 ID 功能，將會嘗試第一次驗證使用者與其他登入 ID 和然後改為使用 UPN，如果找不到由其他登入收到 account 請確定您有任何其他登入 ID 和 UPN 衝突如果您想要仍然支援 UPN 登入。 例如，與其他人的 UPN 設定的電子郵件屬性會封鎖登入他 UPN 從另一位使用者。

-   由系統管理員設定的樹系很往下，如果會繼續查看其他登入其他設定的樹系的來電顯示與帳號 AD FS。 如果 AD FS 伺服器找到跨樹系它已搜尋獨特的使用者物件，使用者會成功登入。

-   您可能會此外想要自訂 AD FS 登入頁面，讓使用者一些有關其他登入收到提示 你就可以加入描述自訂的登入頁面 (如需詳細資訊，請查看[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)或自訂使用者名稱] 欄位上述的「登入組織 account「字串 (如需詳細資訊，[進階自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含 ID 登入其他值新宣告類型是**http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和效能計數器
已新增下列效能計數器測量 AD FS 伺服器的效能，會支援其他登入 ID 時：

-   其他登入 Id 驗證：數字的驗證使用其他登入 ID 來執行

-   其他登入 Id 驗證秒：的驗證執行使用其他登入 ID 秒的數字

-   搜尋延遲平均其他登入 id：搜尋延遲平均跨樹系的系統管理員也已設定的其他登入 ID

以下是各種錯誤案例，以及對使用者的登入體驗事件登入，AD FS 使用的對應影響：



**錯誤案例**|**登入的使用經驗影響**|**事件**|
---------|---------|---------
無法取得 SAMAccountName 使用者物件的值。|登入失敗|事件 ID 364 例外訊息 MSIS8012：找不到 samAccountName 的使用者: ' {0} '。|
CanonicalName 屬性不能存取|登入失敗|事件 ID 364 例外訊息 MSIS8013: CanonicalName: '{0}' 的使用者：{1} ' 是格式不正確。|
在一個森林中找到多個使用者物件|登入失敗|事件 ID 364 例外訊息 MSIS8015：森林 '{1} 的身分中找到的身分' {0} ' 的多個帳號：{2}|
跨樹系多個位於多個使用者物件|登入失敗|事件 ID 364 例外訊息 MSIS8014：森林中找到的身分 '{0}' 的多個帳號：{1}|

## <a name="see-also"></a>也了
[AD FS 作業](../../ad-fs/AD-FS-2016-Operations.md)


