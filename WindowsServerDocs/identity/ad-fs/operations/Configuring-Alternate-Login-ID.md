---
description: 深入瞭解：設定替代登入識別碼
ms.assetid: f0cbdd78-f5ae-47ff-b5d3-96faf4940f4a
title: 設定替代登入識別碼
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.openlocfilehash: 60e93280e00fb980871c1289047ee2ff54d4d449
ms.sourcegitcommit: eb995fa887ffe1408b9f67caf743c66107173666
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/21/2021
ms.locfileid: "98666587"
---
# <a name="configuring-alternate-login-id"></a>設定替代登入識別碼


## <a name="what-is-alternate-login-id"></a>什麼是替代登入識別碼？

在大部分的情況下，使用者會使用其 UPN (使用者主體名稱) 來登入其帳戶。 不過，在某些環境中，因為公司原則或內部部署的企業營運應用程式相依性，所以使用者可能會使用其他形式的登入。

> [!NOTE]
> Microsoft 建議的最佳作法是將 UPN 與主要 SMTP 位址比對。 本文說明無法補救 UPN 以符合的小型客戶百分比。

例如，他們可以使用其電子郵件識別碼進行登入，且其 UPN 可能不同。 這在其 UPN 無法路由傳送的情況下特別常見。 請考慮使用 UPN jdoe@contoso.local 和電子郵件地址的使用者 Jane Doe jdoe@contoso.com 。 Jane 可能甚至不知道 UPN，因為她一直都是使用她的電子郵件識別碼進行登入。 使用任何其他登入方法，而不是 UPN 構成替代識別碼。 如需有關如何建立 UPN 的詳細資訊，請參閱 [Azure AD UserPrincipalName 人口](/azure/active-directory/connect/active-directory-aadconnect-userprincipalname)。

Active Directory 同盟服務 (AD FS) 使用 AD FS 來啟用同盟應用程式，以使用替代識別碼進行登入。 這可讓系統管理員指定預設 UPN 的替代方式，以用於登入。 AD FS 已支援使用 Active Directory Domain Services (AD DS) 所接受的任何使用者識別碼形式。 如果設定替代識別碼，AD FS 可讓使用者使用設定的替代識別碼值（例如電子郵件識別碼）進行登入。使用替代識別碼可讓您採用 SaaS 提供者（例如 Office 365），而不需要修改您的內部部署 Upn。 它也可讓您透過取用者布建的身分識別，支援企業營運服務應用程式。

## <a name="alternate-id-in-azure-ad"></a>Azure AD 中的替代識別碼

組織可能必須在下列案例中使用替代識別碼：
1. 內部部署功能變數名稱無法路由傳送，例如 然後，預設的使用者主體名稱無法路由傳送 (jdoe@contoso.local) 。 因為本機應用程式相依性或公司原則，所以無法變更現有的 UPN。 Azure AD 和 Office 365 都需要與 Azure AD 目錄相關聯的所有網域尾碼，才能完全以網際網路路由傳送。
2. 內部部署 UPN 與使用者的電子郵件地址不同，若要登入 Office 365，使用者將無法使用電子郵件地址和 UPN，因為組織的限制。
   在上述案例中，具有 AD FS 的替代識別碼可讓使用者登入 Azure AD，而不需要修改您的內部部署 Upn。

## <a name="configure-alternate-logon-id"></a>設定替代登入識別碼

使用 Azure AD Connect 建議您使用 Azure AD Connect 來為您的環境設定替代的登入識別碼。

- 如需 Azure AD Connect 的新設定，請參閱連接到 Azure AD，以取得如何設定替代識別碼和 AD FS 伺服器陣列的詳細指示。
- 針對現有的 Azure AD Connect 安裝，請參閱變更使用者登入方法，以取得將登入方法變更為 AD FS 的指示。

當 Azure AD Connect 提供 AD FS 環境的詳細資料時，它會自動檢查您的 AD FS 是否存在正確的 KB，並為替代識別碼設定 AD FS，包括 Azure AD 同盟信任的所有必要的正確宣告規則。 不需要額外的步驟來設定替代識別碼。

> [!NOTE]
> Microsoft 建議使用 Azure AD Connect 來設定替代登入識別碼。

### <a name="manually-configure-alternate-id"></a>手動設定替代識別碼

為了設定替代登入識別碼，您必須執行下列工作：設定您的 AD FS 宣告提供者信任以啟用替代登入識別碼

1.  如果您有伺服器2012R2，請確定您已在所有 AD FS 伺服器上安裝 KB2919355。 您可以透過 Windows Update 服務取得，或直接下載。

2.  在伺服器陣列中的任何同盟伺服器上執行下列 PowerShell Cmdlet，以更新 AD FS 設定 (如果您有 WID 伺服器陣列，則必須在伺服器陣列中的主要 AD FS 伺服器上執行此命令) ：

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID <attribute> -LookupForests <forest domain>
```

**AlternateLoginID** 是您要用於登入之屬性的 LDAP 名稱。

**LookupForests** 是您的使用者所屬的樹系 DNS 清單。

若要啟用替代登入識別碼功能，您必須以非 null 的有效值設定-AlternateLoginID 和-LookupForests 參數。

在下列範例中，您會啟用替代登入識別碼功能，讓具有 contoso.com 和 fabrikam.com 樹系帳戶的使用者可以使用其 "mail" 屬性登入 AD FS 啟用的應用程式。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID mail -LookupForests contoso.com,fabrikam.com
```

3. 若要停用這項功能，請將兩個參數的值設定為 null。

``` powershell
Set-AdfsClaimsProviderTrust -TargetIdentifier "AD AUTHORITY" -AlternateLoginID $NULL -LookupForests $NULL
```

## <a name="hybrid-modern-authentication-with-alternate-id"></a>使用替代識別碼的混合式新式驗證

> [!IMPORTANT]
> 下列僅針對 AD FS 而非協力廠商身分識別提供者進行測試。

### <a name="exchange-and-skype-for-business"></a>Exchange 和商務用 Skype

如果您搭配 Exchange 和商務用 Skype 使用替代登入識別碼，使用者體驗會根據您是否使用 HMA 而有所不同。

> [!NOTE]
> 為了獲得最佳的終端使用者體驗，Microsoft 建議使用混合式新式驗證。

如需詳細資訊，請參閱 [混合式新式驗證總覽](https://support.office.com/article/Hybrid-Modern-Authentication-overview-and-prerequisites-for-using-it-with-on-premises-Skype-for-Business-and-Exchange-servers-ef753b32-7251-4c9e-b442-1a5aec14e58d)

### <a name="pre-requisites-for-exchange-and-skype-for-business"></a>Exchange 和商務用 Skype 的先決條件

以下是使用替代識別碼來達成 SSO 的先決條件。

- Exchange Online 應已開啟新式驗證。
- 商務用 Skype (SFB) Online 應已開啟新式驗證。
- Exchange 內部部署應已開啟新式驗證。  Exchange 2013 CU19 或 Exchange 2016 CU18 和更新的所有 Exchange 伺服器都需要。 環境中沒有任何 Exchange 2010。
- 商務用 Skype 內部部署應已開啟新式驗證。
- 您必須使用已啟用新式驗證的 Exchange 和 Skype 用戶端。 所有伺服器都必須執行 SFB Server 2015 CU5。
- 支援新式驗證的商務用 Skype 用戶端
   - iOS、Android、Windows Phone
   - SFB 2016 (MA 預設為開啟，但請確定未停用。 ) 
   - SFB 2013 (MA 預設為關閉，因此請確定已開啟 MA。 ) 
   - SFB Mac desktop
- 支援新式驗證並支援 AltID 的 Exchange 用戶端 regkeys
    - 僅限 Office Pro Plus 2016

#### <a name="supported-office-version"></a>支援的 Office 版本

如果未完成這些額外的設定，使用替代識別碼以替代識別碼設定您的 SSO 的目錄，可能會導致額外的驗證提示。 請參閱本文，以瞭解使用替代識別碼的使用者體驗可能會有何影響。

使用下列額外的設定，可大幅改善使用者體驗，而且您可以在組織中取得替代識別碼使用者驗證的近乎零提示。

##### <a name="step-1-update-to-required-office-version"></a>步驟 1： 更新為所需的 Office 版本

Office 版本 1712 (build no 8827.2148) 以上版本已更新驗證邏輯以處理替代識別碼案例。 為了充分利用新的邏輯，用戶端電腦必須更新為 Office 版本 1712 (組建無 8827.2148) 以上。

##### <a name="step-2-update-to-required-windows-version"></a>步驟 2： 更新為所需的 Windows 版本

Windows 1709 版和更新版本已更新驗證邏輯，以處理替代識別碼案例。 為了充分利用新的邏輯，用戶端電腦必須更新為 Windows 1709 版和更新版本。

##### <a name="step-3-configure-registry-for-impacted-users-using-group-policy"></a>步驟 3： 使用群組原則設定受影響之使用者的登錄

Office 應用程式依賴目錄管理員所推送的資訊來識別替代識別碼環境。 您必須設定下列登錄機碼，以協助 office 應用程式以替代識別碼驗證使用者，而不顯示任何額外的提示

|要新增的 Regkey|Regkey 資料名稱、類型和值|Windows 7/8|Windows 10|Description|
|-----|-----|-----|-----|-----|
|HKEY_CURRENT_USER\Software\Microsoft\AuthN|DomainHint</br>REG_SZ</br>contoso.com|必要|必要|此 regkey 的值是組織租使用者中已驗證的自訂功能變數名稱。 例如，如果 Contoso.com 是租使用者 Contoso.onmicrosoft.com 中其中一個已驗證的自訂功能變數名稱，則 Contoso corp 可以在此登錄中提供 Contoso.com 值。|
HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity|EnableAlternateIdSupport</br>REG_DWORD</br>1|Outlook 2016 ProPlus 的必要參數|Outlook 2016 ProPlus 的必要參數|此 regkey 的值可能是 1/0，表示 Outlook 應用程式是否應該參與改良的替代識別碼驗證邏輯。|
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap\Domains\contoso.com\sts|&#42;</br>REG_DWORD</br>1|必要|必要|此登錄可以用來將 STS 設定為網際網路設定中的信任區域。 標準 ADFS 部署建議將 ADFS 命名空間新增至近端內部網路區域，以供 Internet Explorer|

## <a name="new-authentication-flow-after-additional-configuration"></a>其他設定之後的新驗證流程

![驗證流程](media/Configure-Alternate-Login-ID/alt1a.png)

1. 答：使用替代識別碼在 Azure AD 中布建使用者
   </br>b：目錄系統管理員將必要的 regkey 設定推送到受影響的用戶端電腦
2. 使用者在本機電腦上進行驗證並開啟 office 應用程式
3. Office 應用程式會使用本機會話認證
4. Office 應用程式使用系統管理員和本機認證所推送的網域提示，向 Azure AD 進行驗證
5. Azure AD 藉由導向至正確的同盟領域併發出權杖來成功驗證使用者

## <a name="applications-and-user-experience-after-the-additional-configuration"></a>其他設定之後的應用程式和使用者體驗

### <a name="non-exchange-and-skype-for-business-clients"></a>非 Exchange 和商務用 Skype 用戶端

|用戶端|支援聲明|備註|
| ----- | -----|-----|
|Microsoft Teams|支援|<li>Microsoft 小組支援 AD FS (的 SAML-P、WS-ADDRESSING、WS-TRUST 和 OAuth) 以及新式驗證。</li><li> 通道、聊天室和檔案等核心 Microsoft 團隊都可以使用替代登入識別碼。</li><li>第1和協力廠商應用程式必須由客戶個別調查。 這是因為每個應用程式都有自己的可支援性驗證通訊協定。</li>|
|商務用 OneDrive|支援-建議使用用戶端登錄機碼 |設定替代識別碼之後，您會看到內部部署 UPN 已預先填入驗證欄位中。 這需要變更為所使用的替代身分識別。 我們建議使用本文中所述的用戶端登錄機碼： Office 2013 和 Lync 2013 會定期提示您輸入 SharePoint Online、OneDrive 和 Lync Online 的認證。|
|商務用 OneDrive 行動用戶端|支援||
|Office 365 Pro Plus 啟用頁面|支援-建議使用用戶端登錄機碼|設定替代識別碼之後，您會看到內部部署 UPN 已預先填入驗證欄位中。 這需要變更為所使用的替代身分識別。 我們建議使用本文中所述的用戶端登錄機碼： Office 2013 和 Lync 2013 會定期提示您輸入 SharePoint Online、OneDrive 和 Lync Online 的認證。|

### <a name="exchange-and-skype-for-business-clients"></a>Exchange 和商務用 Skype 用戶端

|用戶端|支援語句-使用 HMA|支援語句-不含 HMA|
| ----- |----- | ----- |
|Outlook|支援，沒有額外的提示|支援</br></br>使用 Exchange Online 的 **新式驗證** ：支援</br></br>使用 **一般** 的 Exchange Online 驗證：支援下列注意事項：</br><li>您必須位於已加入網域的電腦上，且已連線到公司網路 </li><li>您只能在不允許外部存取信箱使用者的環境中使用替代識別碼。 這表示當使用者連線並加入公司網路、VPN 或透過直接存取電腦連線時，使用者只能以支援的方式驗證其信箱，但在設定 Outlook 設定檔時，您會收到一些額外的提示。|
|混合式公用資料夾|支援，沒有額外的提示。|使用 Exchange Online 的 **新式驗證** ：支援</br></br>使用 **一般** 的 Exchange Online 驗證：不支援</br></br><li>如果使用替代識別碼，則混合式公用資料夾無法擴充，因此目前不應使用一般驗證方法。|
|跨單位委派|請參閱 [設定 Exchange 以支援混合式部署中的委派信箱許可權](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|請參閱 [設定 Exchange 以支援混合式部署中的委派信箱許可權](/exchange/hybrid-deployment/set-up-delegated-mailbox-permissions)|
|封存信箱存取 (信箱內部部署-在雲端中封存) |支援，沒有額外的提示|支援的使用者在存取封存時，會收到額外的認證提示，他們必須在出現提示時提供其替代識別碼。|
|Outlook Web Access|支援|支援|
|適用于 Android、IOS 和 Windows Phone 的 Outlook Mobile Apps|支援|支援|
|商務用 Skype/Lync|支援，沒有額外的提示|除了) 所述的支援 (，但有可能會造成使用者混淆。</br></br>在行動用戶端上，只有當 SIP 位址 = 電子郵件地址 = 替代識別碼時，才支援替代識別碼。</br></br> 使用者可能需要登入兩次，才能登入商務用 Skype 桌面用戶端，首先使用內部部署 UPN，然後使用替代識別碼。  (請注意，「登入位址」實際上是可能與「使用者名稱」不同的 SIP 位址，不過通常是) 。 當第一次提示輸入使用者名稱時，使用者應該輸入 UPN，即使它不正確地預先填入替代識別碼或 SIP 位址。 當使用者按一下 [使用 UPN 登入] 之後，使用者名稱提示隨即出現，這次預先填入 UPN。 這次使用者必須將此取代為替代識別碼，然後按一下 [登入] 以完成登入程式。 在行動用戶端上，使用者應該在 [advanced] 頁面中輸入內部部署使用者識別碼，並使用 SAM 樣式格式 (網域 \ 使用者識別碼) ，而不是 UPN 格式。</br></br>成功登入之後，如果商務用 Skype 或 Lync 顯示「Exchange 需要您的認證」，您就必須提供適用于信箱所在位置的認證。 如果信箱位於雲端，您需要提供替代識別碼。 如果是內部部署的信箱，您需要提供內部部署 UPN。|

## <a name="additional-details--considerations"></a>其他詳細資料 & 考慮

- 替代登入識別碼功能適用于已部署 AD FS 的同盟環境。  在下列案例中不支援此功能：
    - 無法路由傳送的網域 (例如，無法由 Azure AD 驗證的 Contoso. 本機) 。
    - 未部署 AD FS 的受控環境。

- 啟用時，[替代登入識別碼] 功能僅適用于 AD FS (SAML-P、WS-ADDRESSING、WS-TRUST 和 OAuth) 所支援的所有使用者名稱/密碼驗證通訊協定上的使用者名稱/密碼驗證。

- 當 Windows 整合式驗證 (WIA) 執行時 (例如，當使用者嘗試從內部網路存取已加入網域之電腦上的公司應用程式，且 AD FS 系統管理員已將驗證原則設定為使用內部網路) 的 WIA 時，會使用 UPN 進行驗證。 如果您已針對信賴憑證者設定替代登入識別碼功能的任何宣告規則，您應該確定這些規則在 WIA 案例中仍然有效。

- 啟用時，[替代登入識別碼] 功能至少需要一個通用類別目錄伺服器，才能從 AD FS 支援的每個使用者帳戶樹系的 AD FS 伺服器連線。 若無法連線到使用者帳戶樹系中的通用類別目錄伺服器，會導致 AD FS 回到使用 UPN。 依預設，所有網域控制站都是通用類別目錄伺服器。

- 啟用時，如果 AD FS 伺服器在所有設定的使用者帳戶樹系中找到多個具有相同替代登入識別碼值的使用者物件，則會使登入失敗。

- 啟用替代登入識別碼功能時，AD FS 會嘗試先使用替代登入識別碼來驗證使用者，然後在找不到可由替代登入識別碼識別的帳戶時，切換回使用 UPN。 如果您想要繼續支援 UPN 登入，請確定替代登入識別碼與 UPN 之間沒有任何衝突。 例如，使用另一個 UPN 來設定一個的 mail 屬性，會封鎖其他使用者以他的 UPN 登入。

- 如果系統管理員所設定的其中一個樹系已關閉，AD FS 在設定的其他樹系中繼續查詢具有替代登入識別碼的使用者帳戶。 如果 AD FS server 在已搜尋的樹系中找到唯一的使用者物件，則使用者會成功登入。

- 您可能還需要自訂 AD FS 登入頁面，以提供使用者有關替代登入識別碼的一些提示。 您可以藉由新增自訂登入頁面描述來進行 (如需詳細資訊，請參閱在使用者名稱欄位中 [自訂 AD FS 登入頁面](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280950(v=ws.11)) 或自訂 [使用組織帳戶登入] 字串 (如需詳細資訊，請參閱 [AD FS 登入頁面的自訂自訂](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn636121(v=ws.11))。

- 包含替代登入識別碼值的新宣告類型為 **HTTP: schemas.microsoft.com。 microsoft .com/ws/2013/11/alternateloginid**

## <a name="events-and-performance-counters"></a>事件和效能計數器

已新增下列效能計數器，以在啟用替代登入識別碼時測量 AD FS 伺服器的效能：

- 替代登入識別碼驗證：使用替代登入識別碼執行的驗證數目

- 替代登入識別碼驗證/秒：使用替代登入識別碼每秒執行的驗證數目

- 替代登入識別碼的平均搜尋延遲：系統管理員為替代登入識別碼設定之樹系中的平均搜尋延遲

以下是各種不同的錯誤案例，並會對使用者的登入體驗產生與 AD FS 記錄的事件相關的影響：



|                       **錯誤案例**                        | **對登入體驗的影響** |                                                              **事件**                                                              |
|--------------------------------------------------------------|----------------------------------|-------------------------------------------------------------------------------------------------------------------------------------|
| 無法取得使用者物件的 SAMAccountName 值 |          登入失敗           |                  事件識別碼364，例外狀況訊息 MSIS8012：找不到使用者的 samAccountName： ' {0} '。                   |
|        無法存取 CanonicalName 屬性         |          登入失敗           |               事件識別碼364，例外狀況訊息 MSIS8013： CanonicalName： ' ' {0} 的使用者： ' {1} ' 格式不正確。                |
|        在一個樹系中找到多個使用者物件        |          登入失敗           | 事件識別碼364，例外狀況訊息 MSIS8015：在樹系 ' ' 中找到多個識別符為 ' ' 的使用者帳戶 {0} {1} ，具有身分識別： {2} |
|   跨多個樹系找到多個使用者物件    |          登入失敗           |           事件識別碼364，例外狀況訊息 MSIS8014：在樹系中找到多個身分識別為 ' ' 的使用者帳戶 {0} ： {1}            |

## <a name="see-also"></a>另請參閱

[AD FS 操作](../ad-fs-operations.md)
