---
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: 設定替代登入識別碼
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7e7a881a2e6bae499ed7d4713bd70a804c3412e6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816961"
---
# <a name="configuring-alternate-login-id"></a>設定替代登入識別碼


## <a name="what-is-alternate-login-id"></a>什麼是替代登入識別碼？
在大部分的情況下，使用者會使用其 UPN （使用者主體名稱）來登入其帳戶。 不過，在某些環境中，由於公司原則或內部部署的企業營運應用程式相依性，使用者可能會使用某種形式的登入。 

>[!NOTE]
>Microsoft 建議的最佳作法是將 UPN 與主要 SMTP 位址比對。 本文說明無法補救 UPN 以符合的少數客戶。

例如，他們可以使用其電子郵件識別碼來進行登入，而且其 UPN 可能不同。 這在其 UPN 無法路由傳送的情況下特別常見。 請考慮使用 UPN jdoe@contoso.local 和電子郵件地址 jdoe@contoso.com的使用者 Jane Doe。 Jane 可能甚至不知道 UPN，因為她一直使用她的電子郵件識別碼進行登入。 使用任何其他登入方法，而不是 UPN 構成替代識別碼。 如需如何建立 UPN 的詳細資訊，請參閱[Azure AD UserPrincipalName 填入](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-userprincipalname)。

Active Directory 同盟服務（AD FS）可讓使用 AD FS 的同盟應用程式使用替代識別碼進行登入。 這可讓系統管理員指定預設 UPN 的替代方式，以用於登入。 AD FS 已經支援使用 Active Directory Domain Services （AD DS）接受的任何形式的使用者識別碼。 針對替代識別碼進行設定時，AD FS 可讓使用者使用設定的替代識別碼值（例如電子郵件識別碼）進行登入。使用替代識別碼可讓您採用 SaaS 提供者（例如 Office 365），而不需要修改您的內部部署 Upn。 它也可讓您使用取用者布建的身分識別來支援企業營運服務應用程式。

## <a name="alternate-id-in-azure-ad"></a>Azure AD 中的替代識別碼
在下列案例中，組織可能必須使用替代識別碼：
1. 內部部署功能變數名稱無法路由傳送，例如 Contoso。因此，預設的使用者主體名稱是不可路由傳送的（jdoe@contoso.local）。 因為本機應用程式相依性或公司原則，所以無法變更現有的 UPN。 Azure AD 和 Office 365 需要與 Azure AD 目錄相關聯的所有網域尾碼，才能完全可進行網際網路路由傳送。 
2. 內部部署 UPN 與使用者的電子郵件地址和登入 Office 365 不相同，因為組織的限制，使用者使用電子郵件地址和 UPN 時無法使用。
   在上述案例中，具有 AD FS 的替代識別碼可讓使用者登入 Azure AD，而不需要修改您的內部部署 Upn。 

## <a name="end-user-experience-with-alternate-login-id"></a>具有替代登入識別碼的終端使用者體驗
使用者體驗會根據搭配替代登入識別碼使用的驗證方法而有所不同。 目前有三種不同的方式可以達到使用替代登入識別碼。  這些系統為：

- **一般驗證（舊版）** -使用基本驗證通訊協定。
- **新式驗證**-將以 ACTIVE DIRECTORY 驗證程式庫（ADAL）為基礎的登入帶入應用程式。 這可讓登入功能（例如多重要素驗證（MFA）、以 SAML 為基礎的協力廠商身分識別提供者）搭配 Office 用戶端應用程式、智慧卡和憑證型驗證。
- **混合式新式驗證**-提供新式驗證的所有優點，並讓使用者能夠使用從雲端取得的授權權杖來存取內部部署應用程式。

>[!NOTE]
> 為了獲得最佳的可能體驗，Microsoft 強烈建議混合式新式驗證。



## <a name="configure-alternate-logon-id"></a>設定替代登入識別碼
使用 Azure AD Connect 建議使用 Azure AD Connect，為您的環境設定替代登入識別碼。

- 如需 Azure AD Connect 的新設定，請參閱連接到 Azure AD，以取得如何設定替代識別碼和 AD FS 伺服器陣列的詳細指示。
- 如需現有的 Azure AD Connect 安裝，請參閱變更使用者登入方法，以取得將登入方法變更為 AD FS 的指示

當 Azure AD Connect 提供 AD FS 環境的詳細資料時，它會自動檢查您的 AD FS 是否有正確的 KB，並為替代識別碼設定 AD FS，包括 Azure AD 同盟信任的所有必要的正確宣告規則。 在 wizard 以外的地方，不需要額外的步驟來設定替代識別碼。

>[!NOTE]
> Microsoft 建議使用 Azure AD Connect 來設定替代登入識別碼。

### <a name="manually-configure-alternate-id"></a>手動設定替代識別碼
若要設定替代登入識別碼，您必須執行下列工作：設定您的 AD FS 宣告提供者信任以啟用替代登入識別碼

1.  如果您有伺服器2012R2，請確定您已在所有 AD FS 伺服器上安裝 KB2919355。 您可以透過 Windows Update 服務取得，或直接下載。 

2.  在伺服器陣列中的任何同盟伺服器上執行下列 PowerShell Cmdlet 來更新 AD FS 設定（如果您有 WID 伺服器陣列，您必須在伺服器陣列中的主要 AD FS 伺服器上執行此命令）：

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID**是您想要用於登入之屬性的 LDAP 名稱。

**LookupForests**是您的使用者所屬的樹系 DNS 清單。

若要啟用替代登入識別碼功能，您必須使用非 null 的有效值來設定-AlternateLoginID 和-LookupForests 參數。

在下列範例中，您會啟用替代登入識別碼功能，讓具有 contoso.com 和 fabrikam.com 樹系中帳戶的使用者可以使用其 "mail" 屬性登入已啟用 AD FS 的應用程式。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. 若要停用這項功能，請將兩個參數的值都設定為 null。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>使用替代識別碼的混合式新式驗證

>[!IMPORTANT]
>下列僅針對 AD FS 而非協力廠商身分識別提供者測試過。

### <a name="exchange-and-skype-for-business"></a>Exchange 與商務用 Skype
如果您使用替代登入識別碼與 Exchange 和商務用 Skype，則使用者體驗會根據您是否使用 HMA 而有所不同。

>[!NOTE]
>為了獲得最佳的使用者體驗，Microsoft 建議使用混合式新式驗證。

或詳細資訊，請參閱[混合式新式驗證總覽](https://support.office.com/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Exchange 和商務用 Skype 的必要條件
以下是使用替代識別碼來達到 SSO 的必要條件。

- Exchange Online 應已開啟新式驗證。
- 線上商務用 Skype （SFB）應已開啟新式驗證。
- Exchange 內部部署應已開啟新式驗證。  Exchange 2013 CU19 或 Exchange 2016 CU18 必須在所有 Exchange 伺服器上更新。 環境中沒有 Exchange 2010。
- 商務用 Skype 內部部署應已開啟新式驗證。
- 您必須使用已啟用新式驗證的 Exchange 和 Skype 用戶端。 所有伺服器都必須執行 SFB Server 2015 CU5。
- 支援新式驗證的商務用 Skype 用戶端
   - iOS、Android、Windows Phone
   - SFB 2016 （MA 預設為開啟，但請確定它並未停用）。
   - SFB 2013 （MA 預設為關閉，因此請確定已開啟 MA）。
   - SFB Mac 桌面
- 具備新式驗證功能且支援 AltID regkeys 的 Exchange 用戶端
    - 僅限 Office Pro Plus 2016





#### <a name="supported-office-version"></a>支援的 Office 版本

如果未完成這些額外的設定，使用替代識別碼來設定具有替代識別碼之 SSO 的目錄，可能會導致額外的驗證提示。 請參閱文章以瞭解使用替代識別碼的使用者體驗可能會受到的影響。

透過下列額外的設定，使用者體驗會大幅改善，而且您可以針對組織中的替代識別碼使用者，達到接近零的提示以進行驗證。

##### <a name="step-1-update-to-required-office-version"></a>步驟 1. 更新為所需的 Office 版本
Office 1712 版（組建無8827.2148）和更新版本已更新驗證邏輯，以處理替代識別碼案例。 若要利用新邏輯，用戶端電腦必須更新為 Office 1712 版（不含8827.2148）和以上版本。

##### <a name="step-2-update-to-required-windows-version"></a>步驟 2. 更新為所需的 Windows 版本
Windows 1709 和更新版本已更新驗證邏輯，以處理替代識別碼案例。 若要利用新邏輯，用戶端電腦必須更新為 Windows 1709 版和更高版本。

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>步驟 3： 使用群組原則為受影響的使用者設定登錄
Office 應用程式會依賴目錄系統管理員所推送的資訊來識別替代識別碼環境。 必須設定下列登錄機碼，以協助 office 應用程式使用替代識別碼來驗證使用者，而不會顯示任何額外的提示

|要新增的 Regkey|Regkey 資料名稱、類型和值|Windows 7/8|Windows 10|描述|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER \Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|必要項|必要項|此 regkey 的值是組織租使用者中已驗證的自訂功能變數名稱。 例如，如果 Contoso.com 是租使用者 Contoso.onmicrosoft.com 中其中一個已驗證的自訂功能變數名稱，則 Contoso corp 可以在此 regkey 中提供 Contoso.com 的值。|
HKEY_CURRENT_USER \Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Outlook 2016 ProPlus 的必要|Outlook 2016 ProPlus 的必要|此 regkey 的值可以是 1/0，向 Outlook 應用程式表示它是否應該參與改良的替代識別碼驗證邏輯。|
HKEY_CURRENT_USER \Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|必要項|必要項|此 regkey 可以用來將 STS 設定為網際網路設定中的信任區域。 標準 ADFS 部署建議將 ADFS 命名空間新增至 Internet Explorer 的近端內部網路區域|

## <a name="new-authentication-flow-after-additional-configuration"></a>其他設定之後的新驗證流程

![驗證流程](media/Configure-Alternate-Login-ID/alt1a.png)

1. 答：使用替代識別碼在 Azure AD 中布建使用者
   </br>b：目錄系統管理員將所需的 regkey 設定推送至受影響的用戶端電腦
2. 使用者在本機電腦上進行驗證，並開啟 office 應用程式
3. Office 應用程式會使用本機會話認證
4. Office 應用程式會使用系統管理員和本機認證所推送的網域提示，向 Azure AD 進行驗證
5. Azure AD 藉由導向正確的同盟領域併發出權杖，成功驗證使用者

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>其他設定之後的應用程式和使用者體驗

### <a name="non-exchange-and-skype-for-business-clients"></a>非 Exchange 和商務用 Skype 用戶端

|用戶端|支援聲明|備註|
| ----- | -----|-----|
|Microsoft Teams|支援|<li>Microsoft 小組支援 AD FS （SAML-P、WS-ADDRESSING、WS-TRUST 和 OAuth）和新式驗證。</li><li> 核心 Microsoft 團隊（例如頻道、聊天室和檔案功能）可與替代登入識別碼搭配運作。</li><li>第1和協力廠商應用程式必須由客戶分開調查。 這是因為每個應用程式都有自己的可支援性驗證通訊協定。</li>|     
|商務用 OneDrive|支援-建議使用的用戶端登錄機碼 |設定替代識別碼之後，您會在 [驗證] 欄位中看到內部部署 UPN 已預先填入。 這需要變更為所使用的替代身分識別。 建議使用本文所述的用戶端登錄機碼： Office 2013 和 Lync 2013 會定期提示您提供 SharePoint Online、OneDrive 和 Lync Online 的認證。|
|商務用 OneDrive 行動用戶端|支援|| 
|Office 365 Pro Plus 啟用頁面|支援-建議使用的用戶端登錄機碼|設定替代識別碼之後，您會在 [驗證] 欄位中看到內部部署 UPN 已預先填入。 這需要變更為所使用的替代身分識別。 我們建議使用本文所述的用戶端登錄機碼： Office 2013 和 Lync 2013 會定期提示您提供 SharePoint Online、OneDrive 和 Lync Online 的認證。|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange 和商務用 Skype 用戶端

|用戶端|支援聲明-使用 HMA|支援聲明-不含 HMA|
| ----- |----- | ----- |
|Outlook|支援，無額外提示|支援</br></br>Exchange Online 的**新式驗證**：支援</br></br>使用 Exchange Online 的**一般驗證**：支援下列注意事項：</br><li>您必須在已加入網域的電腦上，並聯機到公司網路 </li><li>您只能在不允許對信箱使用者進行外部存取的環境中使用替代識別碼。 這表示使用者只能在連線並加入公司網路、VPN 或透過直接存取電腦連線時，以支援的方式向信箱進行驗證，但在設定 Outlook 設定檔時，您會收到幾個額外的提示。| 
|混合式公用資料夾|支援，不會有額外的提示。|Exchange Online 的**新式驗證**：支援</br></br>Exchange Online 的**一般驗證**：不支援</br></br><li>如果使用替代識別碼，就無法擴充混合式公用資料夾，因此現在不應該搭配一般驗證方法來使用。|
|跨單位委派|請參閱[在混合式部署中設定 Exchange 支援委派的信箱許可權](https://technet.microsoft.com/library/mt784505.aspx)|請參閱[在混合式部署中設定 Exchange 支援委派的信箱許可權](https://technet.microsoft.com/library/mt784505.aspx)|
|封存信箱存取（信箱內部部署-在雲端中封存）|支援，無額外提示|支援-使用者在存取封存時，會收到額外的認證提示，他們必須在出現提示時提供其替代識別碼。| 
|Outlook Web Access|支援|支援|
|適用于 Android、IOS 和 Windows Phone 的 Outlook Mobile Apps|支援|支援|
|商務用 Skype/Lync|支援，不含額外提示|支援（除了所述），但可能會造成使用者混淆。</br></br>在行動用戶端上，只有在 SIP 位址 = 電子郵件地址 = 替代識別碼時，才支援替代識別碼。</br></br> 使用者可能需要登入商務用 Skype 桌面用戶端兩次，首先使用內部部署 UPN，然後使用替代識別碼。 （請注意，「登入位址」實際上是 SIP 位址，可能與「使用者名稱」不同，但通常是）。 當第一次提示輸入使用者名稱時，使用者應該輸入 UPN，即使它不正確地預先填入替代識別碼或 SIP 位址也一樣。 使用者按一下 [使用 UPN 登入] 之後，會重新出現使用者名稱提示，這次會預先填入 UPN。 這次，使用者必須以替代識別碼取代此項，然後按一下 [登入] 以完成登入程式。 在行動用戶端上，使用者應該使用 SAM 樣式格式（網域 \ 使用者名稱），而不是 UPN 格式，在 [advanced] 頁面中輸入內部部署使用者識別碼。</br></br>成功登入之後，如果商務用 Skype 或 Lync 顯示「Exchange 需要您的認證」，您必須提供適用于信箱所在位置的認證。 如果信箱在雲端中，您必須提供替代識別碼。 如果信箱是內部部署，您必須提供內部部署 UPN。| 

## <a name="additional-details--considerations"></a>& 考慮的其他詳細資料

-   替代登入識別碼功能適用于已部署 AD FS 的同盟環境。  在下列案例中不支援此功能：
    -   無法由 Azure AD 驗證的不可路由網域（例如 Contoso. local）。
    -   未部署 AD FS 的受管理環境。


-   啟用時，[替代登入識別碼] 功能僅適用于 AD FS （SAML-P、WS-ADDRESSING、WS-TRUST 和 OAuth）支援的所有使用者名稱/密碼驗證通訊協定。


-   執行 Windows 整合式驗證（WIA）時（例如，當使用者嘗試從內部網路存取已加入網域之電腦上的公司應用程式，且 AD FS 系統管理員已將驗證原則設定為使用適用于內部網路的 WIA）、UPN isused 進行驗證。 如果您已針對替代登入識別碼功能的信賴憑證者設定任何宣告規則，您應該確定這些規則在 WIA 案例中仍然有效。

-   啟用時，替代登入識別碼功能需要至少一個通用類別目錄伺服器，才能從 AD FS 伺服器連線到 AD FS 支援的每個使用者帳戶樹系。 無法連線到使用者帳戶樹系中的通用類別目錄伺服器，會導致 AD FS 回到使用 UPN。 根據預設，所有網域控制站都是通用類別目錄伺服器。

-   啟用時，如果 AD FS 伺服器在所有設定的使用者帳戶樹系中找到一個以上具有相同替代登入識別碼值的使用者物件，它就會使登入失敗。

-   當 [替代登入識別碼] 功能已啟用時，AD FS 會先嘗試使用替代登入識別碼來驗證使用者，如果找不到可由替代登入識別碼識別的帳戶，則會切換回使用 UPN。 如果您想要繼續支援 UPN 登入，請確定替代登入識別碼和 UPN 之間沒有衝突。 例如，設定一個 mail 屬性與另一個的 UPN 會封鎖其他使用者使用他的 UPN 登入。

-   如果系統管理員所設定的其中一個樹系已關閉，AD FS 會繼續在已設定的其他樹系中查詢具有替代登入識別碼的使用者帳戶。 如果 AD FS server 在其搜尋的樹系中找到唯一的使用者物件，則使用者會成功登入。

-   您可能需要自訂 AD FS 登入頁面，以提供使用者有關替代登入識別碼的一些提示。 若要這麼做，您可以新增自訂的登入頁面描述（如需詳細資訊，請參閱[自訂 AD FS 登入頁面](https://technet.microsoft.com/library/dn280950.aspx)，或自訂 [以組織帳戶登入] 的 [使用者名稱] 欄位（如需詳細資訊，請參閱[AD FS 登入頁面的 Advanced 自訂](https://technet.microsoft.com/library/dn636121.aspx)。

-   包含替代登入識別碼值的新宣告類型為**HTTP: schemas.microsoft.com。 microsoft .com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和效能計數器
已新增下列效能計數器，以在啟用替代登入識別碼時測量 AD FS 伺服器的效能：

-   替代登入識別碼驗證：使用替代登入識別碼執行的驗證次數

-   替代登入識別碼驗證/秒：使用每秒的替代登入識別碼執行的驗證次數

-   替代登入識別碼的平均搜尋延遲：系統管理員為替代登入識別碼設定的樹系平均搜尋延遲

以下是各種錯誤案例，並對應到使用者登入體驗的影響，以及 AD FS 所記錄的事件：



|                       **錯誤案例**                        | **對登入體驗的影響** |                                                              **發生**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| 無法取得使用者物件的 SAMAccountName 值 |          登入失敗           |                  事件識別碼364，例外狀況訊息 MSIS8012：找不到使用者的 samAccountName： '{0}'。                   |
|        無法存取 CanonicalName 屬性         |          登入失敗           |               事件識別碼364，例外狀況訊息 MSIS8013： CanonicalName：使用者的 '{0}'： '{1}' 格式不正確。                |
|        在一個樹系中找到多個使用者物件        |          登入失敗           | 事件識別碼364，例外狀況訊息 MSIS8015：在樹系 '{1}' 中找到多個身分識別為 '{0}' 的使用者帳戶，具有身分識別： {2} |
|   在多個樹系中找到多個使用者物件    |          登入失敗           |           事件識別碼364，例外狀況訊息 MSIS8014：在樹系中找到多個身分識別為 '{0}' 的使用者帳戶： {1}            |

## <a name="see-also"></a>另請參閱
[AD FS 操作](../../ad-fs/AD-FS-2016-Operations.md)


