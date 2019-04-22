---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: 設定替代登入識別碼
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 615faf4153949aa4ad989f017068d1809fca26b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820869"
---
# <a name="configuring-alternate-login-id"></a>設定替代登入識別碼

>適用於：Windows Server 2019、 Windows Server 2016、 Windows Server 2012 R2

## <a name="what-is-alternate-login-id"></a>替代的登入識別碼為何？
在大部分情況下，使用者會使用其 UPN （使用者主體名稱） 來登入他們的帳戶。 不過，在某些環境由於公司的原則或內部部署的特定業務應用程式相依性中，使用者可能使用其他形式的登入。 

>[!NOTE]
>Microsoft 的建議最佳作法是以主要 SMTP 位址，以便符合 UPN。 本文將說明無法修復 UPN 的客戶小部分的比對。

比方說，他們可以在登入時使用他們的電子郵件識別碼和，可能會不同於其 UPN。 這是特別常見一次在其 UPN 的非可路由傳送案例中。 請考慮使用者的 upn Jane Doejdoe@contoso.local電子郵件地址和jdoe@contoso.com。 Jane 可能不會甚至察覺的 UPN，她一直都是使用她的電子郵件識別碼登入。 使用任何其他登入方法，而非 UPN，即表示 「 替代識別碼。 更多有關 UPN 的方式建立，請參閱 < [Azure AD UserPrincipalName 填入](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname)。

Active Directory Federation Services (AD FS) 可讓同盟應用程式使用 AD FS 登入使用替代識別碼。 這可讓系統管理員指定預設的 UPN 來登入的替代方案。 AD FS 已經可以支援使用任何形式的可接受的 Active Directory 網域服務 (AD DS) 的使用者識別碼。 針對替代 ID 設定時，AD FS 可讓使用者能夠登入使用設定的替代識別碼值，例如電子郵件識別碼。使用替代識別碼，可讓您採用 SaaS 提供者，例如 Office 365，而不需要修改您的內部部署 Upn。 它也可讓您支援的業務服務取用者佈建身分識別應用程式。

## <a name="alternate-id-in-azure-ad"></a>在 Azure AD 中的替代識別碼
組織可能要在下列案例中使用替代識別碼：
1.  非可路由，例如內部部署網域名稱。 Contoso.local，因此預設的使用者主體名稱為非可路由傳送 (jdoe@contoso.local)。 無法變更現有的 UPN 由於本機應用程式相依性或公司原則。 Azure AD 和 Office 365 需要完全是網際網路可路由傳送的 Azure AD 目錄相關聯的所有網域尾碼。 
2.  在內部部署 UPN 不相同使用者的電子郵件地址來登入 Office 365 使用者使用電子郵件地址和無法使用 UPN，因為組織的條件約束。
在上述案例中，使用 AD FS 的替代識別碼可讓使用者登入 Azure AD 而不需要修改您的內部部署 Upn。 

## <a name="end-user-experience-with-alternate-login-id"></a>使用替代登入識別碼的使用者體驗
使用者體驗是根據使用替代登入識別碼的驗證方法而有所不同。目前那里三種不同的方式，在其中使用替代登入識別碼可達成。  其中包括：

- **規則的驗證 （舊版）**-使用基本驗證通訊協定。
- **新式驗證**-Active Directory 驗證程式庫 ADAL 型登入帶入應用程式。 這可讓登入功能，例如 Multi-factor Authentication (MFA)、 SAML 型協力廠商身分識別提供者與 Office 用戶端應用程式，智慧卡和憑證型驗證。
- **混合式新式驗證**-提供所有新式驗證的優點，並提供使用者存取使用從雲端取得的授權權杖的內部部署應用程式的能力。

>[!NOTE]
> 為獲得最佳的可能體驗，Microsoft 強烈建議混合式新式驗證。



## <a name="configure-alternate-logon-id"></a>設定替代登入識別碼
使用 Azure AD Connect 我們建議使用 Azure AD 連線到設定為您的環境的替代登入識別碼。

- 新的 Azure AD Connect 設定，請參閱有關如何設定替代識別碼 」 和 「 AD FS 伺服器陣列的連線到 Azure AD 的詳細指示。
- 對於現有的 Azure AD Connect 安裝，請參閱變更使用者登入方法如需有關將登入方法變更為 AD FS

當 Azure AD Connect 會提供有關 AD FS 環境詳細資料時，自動檢查存在的 AD FS 上正確的 KB 並設定 AD FS 包括 Azure AD 的同盟信任的所有必要權限的宣告規則的替代識別碼。 沒有任何額外的步驟需要外部精靈設定替代識別碼。

>[!NOTE]
> Microsoft 建議使用 Azure AD Connect 設定替代登入識別碼。

### <a name="manually-configure-alternate-id"></a>手動設定替代識別碼
若要設定替代登入識別碼，您必須執行下列工作：設定您 AD FS 宣告提供者信任 來啟用替代登入識別碼

1.  如果您有 Server 2012 r2，請確定您擁有所有 AD FS 伺服器上安裝 KB2919355。 您可以透過 Windows Update 服務取得它，或直接下載它。 

2.  在任何伺服器陣列中的同盟伺服器上執行下列 PowerShell cmdlet 來更新 AD FS 設定 （如果您有 WID 伺服器陣列時，您必須執行此命令在伺服器陣列中主要的 AD FS 伺服器上）：

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID**是您想要用於登入之屬性的 LDAP 名稱。

**LookupForests**是樹系 DNS，您的使用者所屬的清單。

若要啟用替代登入識別碼的功能，您必須設定-AlternateLoginID 和-LookupForests 參數非 null 且有效的值。

在下列範例中，您要啟用替代登入識別碼的功能，使您的使用者使用 contoso.com 和 fabrikam.com 的樹系中的帳戶可以登入 AD FS 的應用程式具有其"mail"屬性。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3.  若要停用這項功能，請設定這兩個參數為 null 的值。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>混合式新式驗證的替代識別碼

>[!IMPORTANT]
>以下只經過對 AD FS 和不第 3 方識別提供者。

### <a name="exchange-and-skype-for-business"></a>Exchange 和商務用 Skype
如果您使用替代登入識別碼與 Exchange 和 Skype for Business，使用者體驗會根據您使用 HMA 而有所不同。

>[!NOTE]
>以獲得最佳使用者體驗，Microsoft 會建議使用混合式新式驗證。

或詳細資訊，請參閱[混合式新式驗證的概觀](https://support.office.com/en-us/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Exchange 和商務用 Skype 的必要條件
以下是必要的元件，用於達成 SSO，並提供替代的識別碼。

- Exchange Online 應該有開啟的新式驗證。
- Skype for Business (SFB) 線上應該有開啟的新式驗證。
- Exchange 內部部署應該已新式驗證開啟。  Exchange 2013 CU19 或 Exchange 2016 CU18，且最多需要所有的 Exchange 伺服器上。 在環境中沒有 Exchange 2010。
- 商務用 Skype 內部部署應該已新式驗證開啟。
- 您必須使用已啟用新式驗證的 Exchange 和 Skype 用戶端。 所有伺服器都必須都執行 SFB Server 2015 CU5。
- 支援新式驗證的商務用戶端用 Skype
   - iOS、 Android、 Windows Phone
   - SFB 2016 （MA 是 ON，根據預設，但請確定它不已停用）。
   - SFB 2013 (MA 預設為關閉，因此請確定已開啟 MA。)
   - SFB Mac 桌面
- Exchange 用戶端，並支援新式驗證，並且支援 AltID 登錄機碼
    - Office Pro Plus 只 2016年





#### <a name="supported-office-version"></a>支援的 Office 版本

如果這些其他的設定未完成的時間，sso 設定您的目錄，使用替代識別碼使用替代識別碼會造成額外的提示進行驗證。 請參閱使用替代識別碼的使用者體驗可能會影響的文章。

下列額外的設定，使用者體驗已大幅改善，與您可以在組織中達到接近零的替代識別碼的使用者驗證的提示。

##### <a name="step-1-update-to-required-office-version"></a>步驟 1. 更新所需的 office 版本
Office 1712 版 （組建沒有 8827.2148） 和更新版本已更新處理的替代識別碼 」 案例的驗證邏輯。 若要使用新的邏輯，用戶端電腦需要更新 office 1712 版 （組建沒有 8827.2148） 和上方。

##### <a name="step-2-configure-registry-for-impacted-users-using-group-policy"></a>步驟 2. 設定為使用群組原則的受影響使用者的登錄
Office 應用程式仰賴推入的目錄系統管理員，來識別 「 替代識別碼 」 環境的資訊。 下列登錄機碼需要為了驗證而不會顯示其他任何提示的替代識別碼的使用者的 office 應用程式設定

|加入 Regkey|Regkey 資料名稱、 類型和值|Windows 7/8|Windows 10|描述|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|必要項|必要項|此 regkey 的值是在組織的租用戶中驗證的自訂網域名稱。 比方說，Contoso corp 可以提供此 regkey 在 Contoso.com 的值，如果 Contoso.com 是其中一個租用戶 Contoso.onmicrosoft.com 中的已驗證的自訂網域名稱。|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|所需的 Outlook 2016 專業增強版|所需的 Outlook 2016 專業增強版|此 regkey 的值可以是 1 / 0 表示 Outlook 應用程式是否應該與互動的改良的替代識別碼驗證邏輯。|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableADALatopWAMOverride</br>REG_DWORD</br>1|不適用|必要。|這可確保 Office 不會使用 WAM 因為 WAM 不支援 alt 識別碼。|
HKEY_CURRENT_USER\SOFTWARE\Microsoft\Office\16.0\Common\Identity|DisableAADWAM</br>REG_DWORD</br>1|不適用|必要。|這可確保 Office 不會使用 WAM 因為 WAM 不支援 alt 識別碼。|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|必要項|必要項|此 regkey 可用來設定為受信任的區域中的網際網路設定的 STS。 標準的 ADFS 部署建議的 ADFS 命名空間加入 Internet explorer 的近端內部網路區域|

## <a name="new-authentication-flow-after-additional-configuration"></a>額外的設定之後新的驗證流程

![驗證流程](media/Configure-Alternate-Login-ID/alt1a.png)

1. 答：使用替代識別碼的 Azure AD 中佈建使用者
   </br>B:目錄系統管理員將需要的 regkey 設定推入至受影響的用戶端電腦
2. 使用者在本機電腦上經過驗證，並開啟 office 應用程式
3. Office 應用程式所花的本機工作階段的認證
4. Office 應用程式向 Azure AD 中使用推入的系統管理員和本機認證的網域提示
5. 若要更正同盟領域，並發出的權杖將導向 azure AD 成功驗證的使用者

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>應用程式和使用者體驗的其他組態之後

### <a name="non-exchange-and-skype-for-business-clients"></a>非 Exchange 和 Skype for Business 用戶端
|Client|支援陳述式|備註|
| ----- | -----|-----|
|Microsoft Teams|支援|<li>Microsoft Teams 支援 AD FS (SAML-P、 Ws-fed、 Ws-trust 和 OAuth) 和新式驗證。</li><li> 例如通道、 聊天室和檔案功能的核心 Microsoft Teams 運作並提供替代的登入識別碼。</li><li>第 1 個和第 3 方應用程式必須由客戶個別調查。 這是因為每個應用程式有自己支援的驗證通訊協定。</li>|     
|商務用 OneDrive|支援-建議的用戶端登錄機碼 |設定替代識別碼與您會看到在 [驗證] 欄位會預先填入的內部部署 UPN。 這需要變更為正在使用其他身分識別。 我們建議使用這篇文章中記下的用戶端端登錄機碼：Office 2013 和 Lync 2013 定期提示 SharePoint Online、 OneDrive 和 Lync Online 認證。|
|OneDrive for Business 的行動用戶端|支援|| 
|Office 365 Pro Plus 啟用頁面|支援-建議的用戶端登錄機碼|設定替代識別碼與您會看到在 [驗證] 欄位會預先填入的內部部署 UPN。 這需要變更為正在使用其他身分識別。 我們建議使用這篇文章中記下的用戶端登錄機碼：Office 2013 和 Lync 2013 定期提示 SharePoint Online、 OneDrive 和 Lync Online 認證。|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange 和 Skype for Business 用戶端

|Client|支援陳述式-HMA|支援-沒有 HMA 陳述式|
| ----- |----- | ----- |
|Outlook|支援，不額外的提示|支援</br></br>具有**新式驗證**針對 Exchange Online:支援</br></br>具有**一般驗證**適用於 Exchange Online:支援下列注意事項：</br><li>您必須在加入網域的電腦上，並連線到公司網路 </li><li>您只能對信箱使用者，不允許外部存取的環境中使用替代識別碼。 這表示，使用者可以只向其信箱的支援方式當他們連線且已加入到公司網路的 vpn，或透過直接存取機器連線，但設定您的 Outlook 設定檔時，會取得幾個額外的提示時。| 
|混合式公用資料夾|支援，不額外的提示。|具有**新式驗證**針對 Exchange Online:支援</br></br>具有**一般驗證**適用於 Exchange Online:不支援</br></br><li>混合式公用資料夾不能以展開，如果使用替代識別碼，而且因此不應立即使用一般的驗證方法。|
|跨內部部署委派|請參閱[設定 Exchange 混合式部署中支援委派的信箱權限](https://technet.microsoft.com/library/mt784505.aspx)|請參閱[設定 Exchange 混合式部署中支援委派的信箱權限](https://technet.microsoft.com/library/mt784505.aspx)|
|封存信箱存取 （信箱在內部層在雲端中的封存）|支援，不額外的提示|支援-使用者會取得額外提示輸入認證，存取封存時，他們必須提供其替代識別碼，當系統提示您。| 
|Outlook Web Access|支援|支援|
|Outlook for Android、 IOS 和 Windows Phone 行動裝置應用程式|支援|支援|
|商務用 Skype / Lync|支援，可使用任何額外的提示|支援 （除非另有註明） 但沒有使用者造成混淆的可能性。</br></br>在行動裝置的用戶端，替代識別碼時，才支援的 SIP 位址 = 電子郵件地址 = 替代識別碼 」。</br></br> 使用者可能需要登入兩次 Skype 商務桌面用戶端，第一次使用內部部署 UPN，然後使用 「 替代識別碼 」。 （請注意，「 登入位址 」 實際上 SIP 位址可能不是 「 使用者名稱 」 相同，不過通常是）。 第一次出現提示時提供使用者名稱，使用者應該輸入 UPN，即使它已不正確地預先填入的替代識別碼或 SIP 位址。 在使用者按下使用 UPN、 使用者名稱提示隨即再度出現，這次使用的 UPN 預先填入登入。 若要完成登入程序，的此時使用者必須以 「 替代識別碼取代此項，然後按一下 登入。 在行動裝置的用戶端，使用者應該在內部部署使用者識別碼在中輸入 [進階] 頁面中，使用 SAM 樣式格式 (domain\username)，不是 UPN 格式。</br></br>成功登入之後，商務用 onedrive 或 Lync 的 Skype 說 「 Exchange 需要您的認證，「 如果您需要提供有效的信箱所在的認證。 若信箱已在雲端中您需要提供 「 替代識別碼 」。 如果信箱在內部部署您需要提供內部部署 UPN。| 
 
## <a name="additional-details--considerations"></a>其他詳細資訊和考量

-   替代登入識別碼功能僅適用於同盟的環境與 AD FS 部署。  不支援在下列情況：
    -   非可路由傳送的網域 (例如 Contoso.local) 無法由 Azure AD 驗證。
    -   管理不需要 AD FS 部署的環境。


-   啟用時，替代登入識別碼功能跨所有的使用者名稱/密碼驗證通訊協定支援的 AD FS 僅適用於使用者名稱/密碼驗證 (SAML-P、 Ws-fed、 Ws-trust 和 OAuth)。


-   Windows 整合式驗證 (WIA) 執行時 （例如，當使用者嘗試存取已加入網域的電腦上的公司應用程式從內部網路和 AD FS 系統管理員已設定內部網路使用 WIA 的驗證原則），UPN isused用於驗證。 如果您已設定替代登入識別碼功能信賴憑證者的合作對象的任何宣告規則，您應該確定這些規則會在 WIA 的情況下仍然有效。

-   啟用時，替代登入識別碼 」 功能會需要至少一個可從 AD FS 支援每個使用者帳戶樹系的 AD FS 伺服器的通用類別目錄伺服器。 觸達的使用者帳戶樹系中的通用類別目錄伺服器的失敗會導致 AD FS 切換回使用 UPN。 根據預設所有網域控制站都是通用類別目錄伺服器。

-   啟用時，如果 AD FS 伺服器會尋找一個以上的使用者物件具有相同的替代登入識別碼值所指定的所有設定的使用者帳戶的樹系，就會失敗的登入。

-   AD FS 啟用替代登入識別碼的功能時，會嘗試先驗證使用者使用替代登入識別碼，並會改為使用 UPN，如果找不到可以替代登入識別碼所識別的帳戶 您應該確定有會替代登入識別碼和 UPN 之間沒有衝突，如果您想要仍然支援 UPN 登入。 例如，設定其中一個的 mail 屬性與其他的 UPN 會封鎖其他使用者登入他的 UPN。

-   如果其中一個樹系已由系統管理員已關閉，AD FS 會繼續尋找與設定其他樹系中的替代登入識別碼的使用者帳戶。 AD FS 伺服器尋找已搜尋過的樹系之間唯一的使用者物件，如果使用者成功登入。

-   此外您可以自訂 AD FS 登入頁面，讓使用者的一些秘訣替代登入識別碼。 您可以藉由新增自訂的登入頁面描述執行它 (如需詳細資訊，請參閱 < [Customizing the AD FS sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx)或 （適用於多個自訂上述使用者名稱 欄位的 「 使用組織帳戶登入 」 字串詳細資訊，請參閱[Advanced Customization of AD FS sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含的替代登入識別碼值的新宣告型別是**http:schemas.microsoft.com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和效能計數器
已新增下列效能計數器來測量 AD FS 伺服器的效能，當已啟用替代登入識別碼：

-   使用替代登入識別碼所執行之驗證的替代登入識別碼的驗證： 數目

-   使用替代登入識別碼，每秒所執行之驗證的替代登入識別碼的驗證/Sec： 數目

-   替代的登入識別碼的平均搜尋延遲： 系統管理員已設定替代登入識別碼在樹系的平均搜尋延遲

以下是各種錯誤案例和對應的影響，在使用者的登入體驗與 AD FS 所記錄的事件：



**錯誤案例**|**在 登入體驗的影響**|**事件**|
---------|---------|---------
無法取得 SAMAccountName 使用者物件的值|登入失敗|事件識別碼 364 MSIS8012 的例外狀況訊息：找不到使用者的 samAccountName: '{0}'。|
CanonicalName 屬性不能存取|登入失敗|事件識別碼 364 MSIS8013 的例外狀況訊息：CanonicalName: '{0}' 的使用者:'{1}' 是不正確的格式。|
在一個樹系中找到多個使用者物件|登入失敗|事件識別碼 364 MSIS8015 的例外狀況訊息：找到多個使用者帳戶與身分識別 '{0}'中的樹系'{1}' 與身分識別： {2}|
跨多個樹系中找到多個使用者物件|登入失敗|事件識別碼 364 MSIS8014 的例外狀況訊息：找到多個使用者帳戶與身分識別 '{0}' 樹系中： {1}|

## <a name="see-also"></a>另請參閱
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


