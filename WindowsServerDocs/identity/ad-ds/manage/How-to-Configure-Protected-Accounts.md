---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: "如何設定帳號受保護狀態"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4de7b1c9e3d12556f67c4515467bccd124e5e73e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="how-to-configure-protected-accounts"></a>如何設定帳號受保護狀態

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過 Pass--hash (PtH) 的攻擊，攻擊者可以使用 [基本 NTLM 湊使用者的密碼 （或是其他 credential 衍生） 驗證遠端伺服器或服務。 Microsoft 先前已[發行指導方針](https://www.microsoft.com/download/details.aspx?id=36036)來減少 pass hash 攻擊。  Windows Server 2012 R2 包含新功能，協助您減少進一步這類攻擊。 如需有關其他防範認證竊取的安全性功能的詳細資訊，請[認證保護和管理](https://technet.microsoft.com/library/dn408190.aspx)。 本主題如何設定下列新功能：

-   [受保護的使用者](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [驗證原則](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [驗證原則筒倉](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

您有 Windows 8.1 和 Windows Server 2012 R2 可協助抵禦認證竊取，涵蓋的下列主題中之後建置額外的防護功能：

-   [遠端桌面以受限制的管理模式](http://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [LSA 保護](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>受保護的使用者
受保護的使用者是，您可以新增新的或現有使用者新的安全性的全域群組。 Windows 8.1 的裝置與 Windows Server 2012 R2 主機有特殊此群組以提供更好的防護功能認證竊取成員行為。 適用於群組成員，Windows 8.1 的裝置或 Windows Server 2012 R2 主機不快取不支援的受保護的使用者的認證。 此群組成員有任何額外的保護是否已登入執行的 Windows 版本 Windows 8.1 之前的裝置。

受保護的使用者成員群組人員的登入 Windows 8.1 的裝置與 Windows Server 2012 R2 主機可以*不再*使用：

-   預設的認證委派 (CredSSP)-純文字不快取認證即使**允許將預設的認證委派**支援原則

-   Windows 摘要-認證純文字快取即使在他們的功能

-   不會快取 NTLM-NTOWF

-   Kerberos 長期-Kerberos 票證授與票證 (TGT) 登入以取得和無法重新取得自動

-   登入 offline-登入快取驗證器不會建立

Windows Server 2012 R2 網域功能等級時，在群組成員可以不再：

-   使用 NTLM 驗證驗證

-   使用的資料加密標準 (DES) 或 RC4 密碼套件 F:kerberos 預先驗證

-   使用未限制或限制委派委派

-   續約初始 4 小時期間以外的使用者門票 (Tgt)

將使用者新增到群組中，您可以使用[UI 工具](https://technet.microsoft.com/library/cc753515.aspx)如 Active Directory 系統管理員中心 (ADAC) 或 Active Directory 使用者和電腦的命令列工具，例如[Dsmod 群組](https://technet.microsoft.com/library/cc732423.aspx)，或 Windows PowerShell[新增-ADGroupMember](https://technet.microsoft.com/library/ee617210.aspx) cmdlet。 服務和電腦的*不應該*會受 Users 群組成員。 這些帳號成員資格提供不本機保護因為密碼或憑證都可以在主機上使用。

> [!WARNING]
> 驗證限制有任何因應措施，這表示，例如企業系統管理員或網域管理群組高特殊權限群組成員是受保護 Users 群組的其他成員為相同的限制。 如果這類群組的所有成員都加入保護 Users 群組時，可能是所有那些帳號被鎖定。您應該不會新增所有高度授權的帳號保護 Users 群組直到您擁有完全測試可能影響。

必須能與進階加密標準 （好一段） 使用 Kerberos 驗證保護 Users 群組成員。 這個方法 Active Directory 中 account 需要好一段按鍵。 建的系統管理員會不會有好一段鍵，除非的密碼不在執行 Windows Server 2008 的網域控制站變更或更新版本。 此外，任何帳號，有已變更網域控制站執行較舊版本的 Windows Server 的密碼，請被鎖定。因此，請遵循這些最佳做法：

-   不要測試網域中，除非**所有網域控制站會都執行 Windows Server 2008，或更新版本**。

-   **變更密碼**適用於所有網域帳號所建立的*之前*建立網域。 否則，這些帳號無法通過驗證。

-   **變更密碼**每一位使用者 account 新增至受保護的使用者前群組或確認您的密碼不在執行 Windows Server 2008 的網域控制站最近變更或更新版本。

### <a name="BKMK_Prereq"></a>使用帳號受保護的需求
受保護的帳號有部署下列需求：

-   若要提供 client 端限制受保護的使用者，主機必須執行 Windows 8.1 或 Windows Server 2012 R2。 使用者只有來登入以的受保護的使用者群組成員。 在這種情形下，可以建立保護 Users 群組，[傳輸主要網域控制站 (PDC) 模擬器角色](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)執行 Windows Server 2012 R2 網域控制站。 其他網域控制站複製物件群組之後，可以在執行 Windows Server 的較舊版本的網域控制站裝載 PDC 模擬器角色。

-   若要提供網域控制站端限制的受保護的使用者，這是限制 NTLM 驗證的使用量，以及其他限制，網域功能等級必須 Windows Server 2012 R2。 如需功能層級，請查看[Active Directory Domain Services 了解 (AD DS) 功能的層級](../active-directory-functional-levels.md)。

### <a name="BKMK_TrubleshootingEvents"></a>事件相關受保護的使用者的疑難排解
本節涵蓋新登來協助排解疑難受保護的使用者及如何保護使用者可能會影響到問題的疑難排解任一票證授與門票 (TGT) 到期日或委派變更相關的活動。

#### <a name="new-logs-for-protected-users"></a>新登的受保護的使用者

有兩個新操作管理登了可幫助疑難排解事件相關受保護的使用者： 保護的使用者-Client 登入和保護使用者失敗-網域控制站登入。 這些新登位於事件檢視器和都預設停用。 要登入，請按一下 [**應用程式與服務登**，按一下 [ **Microsoft**，按一下 [ **Windows**，按一下**驗證**，然後按一下 [登入的名稱，再按**動作**（或以滑鼠右鍵按一下 [登入），按一下 [**可以登入**。

如需事件這些登入，請查看[驗證原則和驗證原則筒倉](https://technet.microsoft.com/library/dn486813.aspx)。

#### <a name="troubleshoot-tgt-expiration"></a>疑難排解 TGT 到期
一般而言，網域控制站設定 TGT 期間和續約，根據網域原則下列群組原則編輯器] 管理視窗中所示。

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

適用於**保護使用者**，下列設定是固定：

-   最大期間使用者票證： 240 分鐘

-   最大期間使用者票證續約： 240 分鐘

#### <a name="troubleshoot-delegation-issues"></a>委派問題的疑難排解
之前，是否已無法使用 Kerberos 委派的技術，client account 已檢查是否**機密帳號，無法委派**設定。 不過，如果 account 成員的**受保護的使用者**，不可能會有此設定在 Active Directory 系統管理員中心 (ADAC)。 如此一來時，要檢查的設定和群組成員資格您委派問題進行疑難排解。

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>稽核驗證嘗試
若要稽核明確的成員驗證嘗試**保護使用者**群組中，您可以繼續收集稽核事件或收集的資料新操作管理登入。 如需有關這些事件的詳細資訊，請查看[驗證原則和驗證原則筒倉](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>提供服務和電腦俠端保護
帳號服務和電腦不能成員**保護使用者**。 本章節解釋的網域控制站型保護可供下列帳號：

-   拒絕 NTLM 驗證： 透過只可以設定[NTLM 封鎖原則](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)

-   拒絕 F:kerberos 預先驗證中的資料加密標準 (DES): Windows Server 2012 R2 網域控制站不會接受 DES 的電腦帳號，除非它們因為 Kerberos 與發行 Windows 的每個版本也支援 RC4 DES 的設定。

-   在 F:kerberos 預先驗證拒絕 RC4： 無法進行設定。

    > [!NOTE]
    > 雖然您可以[變更的組態支援的加密類型的](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)，而測試目標環境中變更這些設定電腦帳號不建議。

-   只使用者門票 (Tgt) 的初始 4 小時期間： 使用驗證原則。

-   拒絕未限制或限制委派委派： 若要限制帳號，請打開 Active Directory 系統管理員中心 (ADAC)，然後選取**機密帳號，無法委派**核取方塊。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>驗證原則
驗證原則是新 AD ds，其中包含驗證原則物件的容器。 驗證原則可以指定設定有助於減少遭受認證竊取，例如限制 TGT 期間帳號，或新增其他宣告相關的條件。

在 Windows Server 2012、 動態存取控制導入了稱為提供可以輕鬆地將檔案伺服器設定組織的中央存取原則 Active Directory 樹系範圍物件課程。 在 Windows Server 2012 R2，稱為 [驗證原則 (objectClass msDS-AuthNPolicies) 新物件課程可用來適用於 Windows Server 2012 R2 網域中 account 類別驗證設定。 Active Directory account 類別︰

-   使用者

-   電腦

-   管理服務 Account 與群組管理服務 Account (GMSA)

### <a name="quick-kerberos-refresher"></a>快速 Kerberos 重新整理程式
三種類型的交易所，也就是 subprotocols Kerberos 驗證通訊協定包含：

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   驗證服務 （為） 交換 （KRB_AS_ *）

-   票證授與服務 (TGS) 換貨 （KRB_TGS_ *）

-   Client 伺服器 (AP) 交換 （KRB_AP_ *）

為換貨是 client 位置使用 account 的密碼或私密金鑰來建立驗證要求票證授與票證 (TGT) 發行前器。 此選項出現時，使用者登入或服務票證需要第一次。

TGS 換貨是使用建立驗證要求服務票證 TGT account 的位置。 這就是當您需要驗證的連接。

AP 換貨發生通常是在應用程式通訊協定的資料並不受到驗證原則。

如需詳細資訊，請查看[Kerberos 版本 5 驗證通訊協定的運作方式](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx)。

### <a name="overview"></a>概觀
驗證原則補充受保護的使用者提供一種方式可設定限制帳號，並藉由限制帳號提供的服務和電腦。 驗證原則以換貨或 TGS 期間執行換貨。

您可以藉由設定限制初始驗證或為換貨：

-   TGT 期間

-   存取控制項條件使用者登入，必須符合的裝置，即將為換貨的限制

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

您可以藉由設定限制服務票證要求透過票證授與服務 (TGS) 交換：

-   存取控制項條件 client 使用者、 服務 (電腦） 必須符合的或從中即將 TGS 換貨的裝置

### <a name="BKMK_ReqForAuthnPolicies"></a>使用 [驗證原則的需求

|原則|需求|
|----------|----------------|
|提供自訂 TGT 存留時間| Windows Server 2012 R2 網域正常運作的層級 account 網域|
|限制使用者登入|Windows Server 2012 R2 網域正常運作的層級 account 網域動態存取控制與支援<br />Windows 8、 Windows 8.1、 Windows Server 2012 或 Windows Server 2012 R2 動態存取控制裝置的支援|
|限制使用者 account 及安全性群組為基礎的服務票證發行| Windows Server 2012 R2 網域正常運作的層級資源網域|
|限制服務票證發行根據使用者宣告或裝置帳號，安全性群組或宣告| Windows Server 2012 R2 網域正常運作的層級資源網域動態存取控制與支援|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>限制帳號裝置的特定和主機
以系統管理員權限的高價值 account 應該成員的**保護使用者**群組。 根據預設，不帳號屬於**保護使用者**群組。 您加入該群組帳號之前，設定的網域控制站支援，並建立以確定您不有任何問題，封鎖稽核原則。

#### <a name="configure-domain-controller-support"></a>設定的網域控制站支援

Windows Server 2012 R2 網域等級正常運作 (DFL) 必須使用者 account 網域。 確定已 Windows Server 2012 R2 網域控制站，並使用 Active Directory 網域與信任移到[提高 DFL](https://technet.microsoft.com/library/cc753104.aspx)以 Windows Server 2012 R2。

**若要設定的支援動態存取控制**

1.  在 [預設的網域控制站原則中，按一下 [**啟用**以便**鍵 Distribution 中心 (KDC) client 支援宣告、 複合驗證以及 Kerberos 保護 \**在 [電腦設定 |系統管理範本 |系統 |\ [KDC。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  在**選項**，在下拉式清單中，選取 [**永遠提供宣告**。

    > [!NOTE]
    > **支援的**您也可以設定，但因為網域是在 Windows Server 2012 R2 DFL，有網域控制站永遠提供宣告允許使用者宣告為基礎的存取檢查以使用非宣告注意裝置時，就會發生和主控連接到宣告感知服務。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > 設定**失敗護身的驗證要求**，將導致驗證失敗的任何不支援 Kerberos 保護 \，例如 Windows 7 和舊版的作業系統，作業系統或開頭為 Windows 8，尚未明確設定支援的作業系統。

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>建立驗證原則使用者 account 稽核 ADAC 與

1.  打開 Active Directory 系統管理員中心 (ADAC)。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > 選取**驗證**節點就是在 Windows Server 2012 R2 DFL 網域。 如果未出現] 節點，然後再試一次核對系統管理員使用的是 Windows Server 2012 R2 DFL 網域。

2.  按一下**驗證原則**，然後按一下 [**新**來建立新原則。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    驗證原則必須顯示名稱，而且預設會執行。

3.  建立僅稽核原則，請按**只稽核原則限制**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    驗證原則已經套用 Active Directory account 類型為基礎。 這三 account 種套用單一原則設定為每個輸入。 Account 類型︰

    -   使用者

    -   電腦

    -   管理服務 Account 受管理的服務 Account 和群組

    如果您有延伸架構與新原則，可用來金鑰 Distribution 中心 (KDC)，從接近衍生的 account 類型歸類新 account 類型。

4.  若要設定的使用者帳號 TGT 期間，請選取 [**指定票證授與票證期間帳號的**核取方塊，輸入分鐘的時間。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    例如，如果您想要 10 小時的時間上限 TGT 期間，輸入**600**所示。 如果您不 TGT 期間設定，然後 account 是否屬於**保護使用者**群組中，TGT 期間，而且續約 4 小時。 否則，TGT 期間和更新根據網域原則下列群組原則編輯器] 管理視窗中的預設設定的網域中所見。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  若要限制帳號，以選取裝置，請按一下**編輯**來定義裝置所需要的條件。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  在**編輯存取控制項條件**視窗中，按**[新增條件**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>新增電腦 account 或群組條件

1.  若要設定電腦帳號或群組] 下拉式清單中，選取 [下拉式清單**的每個成員**，然後變更至**的任何成員**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > 本存取控制定義條件主機使用者登入，或裝置。 在存取控制詞彙的裝置或主機的電腦負責是的使用者，便是一例**使用者**是唯一的選項。

2.  按一下**[新增項目**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  若要變更物件的類型，請按一下**物件類型**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  若要選取 [電腦物件 Active Directory 中，按一下 [**電腦**，然後按一下 [ **[確定]**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  輸入名稱的電腦，以限制使用者，然後按一下**檢查名稱]**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  按一下 [確定]，並建立電腦 account 任何其他條件。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  完成時，然後按一下**[確定]** ，將會顯示電腦 account 定義的條件。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>新增電腦宣告條件

1.  若要設定電腦宣告，下拉式群組，選取 [宣告。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    宣告才可以使用已提供給在森林中。

2.  輸入名稱的組織單位，帳號應該限制登入。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  完成時，然後按一下 [確定]，在方塊中會顯示定義條件。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>疑難排解遺失電腦宣告
如果宣告已，但不能使用，則可能只設定適用於**電腦**類別。

假設您想要限制驗證根據單位 （組織單位） 的電腦，已經設定但只適用於**電腦**類別。

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

宣告能讓使用者登入裝置限制，請選取**使用者**核取方塊。

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>提供與 ADAC 驗證原則的使用者 account

1.  從**使用者**帳號，按**原則**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  選取 [**這個過去指派驗證原則**核取方塊。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  然後選取 [驗證原則套用到使用者。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>設定動態存取控制支援的主機上的裝置
您可以設定 TGT 存留時間，而不設定動態存取控制 (DAC)。 只需要 DAC 檢查 AllowedToAuthenticateFrom 和 AllowedToAuthenticateTo。

使用群組原則 」 或 「 本機群組原則編輯器] 中，讓**Kerberos client 支援宣告、 複合驗證以及 Kerberos 保護 \**在 [電腦設定 |系統管理範本 |系統 |Kerberos:

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>疑難排解驗證原則

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>判斷帳號直接指派驗證原則
驗證原則的帳號區段會顯示帳號，直接套用原則。

![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>使用 [驗證原則失敗-網域控制站管理登入
新的**驗證原則失敗網域控制站**底下管理登入**應用程式與服務登** > **Microsoft** > **Windows** > **驗證**已建立，讓它更容易地發現驗證原則因為失敗。 登入預設停用。 若要讓它，以滑鼠右鍵按一下 [登入的名稱，然後按一下**可以登入**。 新事件都很相似 content 中的現有 Kerberos TGT 和稽核事件服務票證。 如需有關這些事件的詳細資訊，請查看[驗證原則和驗證原則筒倉](https://technet.microsoft.com/library/dn486813.aspx)。

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>使用 Windows PowerShell 來管理驗證原則
這個命令建立驗證原則名為**TestAuthenticationPolicy**。 **UserAllowedToAuthenticateFrom**參數指定的使用者可以進行驗證的檔名為 someFile.txt 中 SDDL 字串的裝置。

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

這個命令取得篩選符合所有驗證原則的**篩選**參數指定。

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

這個命令修改描述和**UserTGTLifetimeMins**指定的驗證原則的屬性。

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

這個命令移除驗證原則的**的身分**參數指定。

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

使用這個命令**取得-ADAuthenticationPolicy** cmdlet 的**篩選**將不會執行的所有驗證原則的參數。 結果將會傳送到**移除-ADAuthenticationPolicy** cmdlet。

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>驗證原則筒倉
驗證原則筒倉的使用者、 電腦及服務帳號 AD DS 是新的容器 (objectClass msDS-AuthNPolicySilos)。 它們保護帳號高價值。 雖然所有組織都需要保護群組企業系統管理員，網域管理和架構系統管理員 」 的成員，因為存取森林中的任何項目攻擊者可能會使用這些帳號，其他帳號也可能都需要保護。

某些組織隔離工作負載建立的唯一它們帳號，並套用限制本機和遠端互動式登入和系統管理員權限的群組原則設定。 驗證原則筒倉補充這項工作建立方式定義使用者、 電腦及管理的服務帳號之間的關聯。 一個筒倉只能屬於帳號。 您可以設定為每一種 account 驗證原則為了控制：

1.  非儲值 TGT 期間

2.  存取控制項條件退貨 TGT (請注意： 無法適用於系統因為 Kerberos 保護 \ 必要)

3.  退貨服務票證存取控制項條件

此外，在 [驗證原則筒倉帳號有筒倉理賠要求，可用於透過宣告感知資源，例如檔案伺服器控制。

新的安全性描述您可以控制發行根據服務票證設定：

-   使用者、 使用者安全性群組和/或使用者的宣告

-   裝置、 裝置安全性群組，和/或裝置的宣告

取得此資訊來資源的網域控制站需要動態存取控制：

-   使用者宣告：

    -   Windows 8 和稍後戶端支援動態存取控制

    -   Account 網域支援動態存取控制和宣告

-   裝置和/或裝置安全性群組：

    -   Windows 8 和稍後戶端支援動態存取控制

    -   設定為複合驗證資源

-   裝置宣告：

    -   Windows 8 和稍後戶端支援動態存取控制

    -   裝置網域支援動態存取控制和宣告

    -   設定為複合驗證資源

可以驗證原則套用到所有成員驗證原則筒倉而不是以個人帳號，或另一個驗證原則可在套用到不同類型的帳號筒倉中。 例如一驗證原則可套用至高度授權的帳號，並不同原則可套用至帳號服務。 建立驗證原則筒倉之前，就必須先建立至少一驗證原則。

> [!NOTE]
> 驗證原則可在套用成員驗證原則筒倉，或可在套用獨立筒倉限制特定 account 範圍。 例如保護單一帳號或一組小型帳號，可以設定原則那些帳號上新增筒倉帳號。

您可以使用 Active Directory 管理中心或 Windows PowerShell 來建立驗證原則筒倉。 根據預設，驗證原則筒倉只稽核筒倉原則，相當於指定**WhatIf** Windows PowerShell cmdlet 中的參數。 若是如此，不會套用原則筒倉限制，但稽核會出現，指出是否限制套用發生錯誤。

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>若要使用 Active Directory 管理中心建立驗證原則筒倉

1.  開放**Active Directory 管理中心**，按一下**驗證**，以滑鼠右鍵按一下**驗證原則筒倉**，按一下**新**，然後按一下 [**驗證原則筒倉**。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  在**顯示名稱**，輸入筒倉的名稱。 在**允許帳號**，按一下 [**新增**，輸入帳號的名稱，然後按**[確定]**。 您可以指定的使用者、 電腦或帳號服務。 然後指定要使用單一原則的所有原則或每種主體，與原則的名稱或原則的不同的原則。

    ![帳號受保護狀態](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>使用 Windows PowerShell 來管理驗證原則筒倉
建立驗證原則筒倉物件這個命令，並執行它。

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

這個命令取得所有驗證原則筒倉所指定的篩選器符合**篩選**的參數。 然後傳遞輸出到**格式化表格**cmdlet 顯示的名稱原則和的值為**動作將使用**在每個原則。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

使用這個命令**取得-ADAuthenticationPolicySilo** cmdlet 的**篩選**參數，以取得所有的不執行驗證原則筒倉與管道的篩選器結果**移除-ADAuthenticationPolicySilo** cmdlet。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

這個命令授與的存取權驗證原則筒倉名為*筒倉*以帳號名為*User01*。

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

這個命令撤銷存取驗證原則筒倉名為*筒倉*帳號名為*User01*。 因為**確認**參數設為**$False**，就會顯示無確認訊息。

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

此範例中第一次使用**取得-ADComputer** cmdlet 將篩選符合所有電腦帳號，**篩選**參數指定。 這個命令的輸出傳遞至**設定為 ADAccountAuthenticatinPolicySilo**指派驗證原則筒倉名為*筒倉*和驗證原則名為*AuthenticationPolicy02*給他們。

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```



