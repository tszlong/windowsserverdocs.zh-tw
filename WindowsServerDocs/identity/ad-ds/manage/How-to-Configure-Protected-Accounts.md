---
ms.assetid: 70c99703-ff0d-4278-9629-b8493b43c833
title: 如何設定受保護的帳戶
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d3e6b698bfd799fb1975bfdd20ecc8f85c07f935
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949069"
---
# <a name="how-to-configure-protected-accounts"></a>如何設定受保護的帳戶

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

透過傳遞雜湊 (Pass-the-hash，PtH) 攻擊，攻擊者可以使用使用者的密碼 (或其他認證系出項) 的基礎 NTLM 雜湊來向遠端伺服器或服務驗證。 Microsoft 先前已 [發佈指導方針](https://www.microsoft.com/download/details.aspx?id=36036) 以減輕傳遞雜湊的攻擊。  Windows Server 2012 R2 包含新的功能，可協助您進一步減輕這類攻擊。 如需協助防範認證竊取之安全性功能的詳細資訊，請參閱 [認證保護和管理](https://technet.microsoft.com/library/dn408190.aspx)。 此主題說明如何設定下列新功能：

-   [受保護的使用者](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_AddtoProtectedUsers)

-   [驗證原則](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicies)

-   [驗證原則定址接收器](../../ad-ds/manage/How-to-Configure-Protected-Accounts.md#BKMK_CreateAuthNPolicySilos)

Windows 8.1 與 Windows Server 2012 R2 都內建額外的安全防護功能以協助防範認證竊取，下列主題涵蓋這些功能：

-   [遠端桌面的受限制系統管理員模式](https://blogs.technet.com/b/kfalde/archive/2013/08/14/restricted-admin-mode-for-rdp-in-windows-8-1-2012-r2.aspx)

-   [LSA 保護](https://technet.microsoft.com/library/dn408187)

## <a name="BKMK_AddtoProtectedUsers"></a>受保護的使用者
Protected Users 是新的全域安全性群組，您可以將新的或現有的使用者新增到其中。 Windows 8.1 裝置和 Windows Server 2012 R2 主機對此群組的成員有特殊的行為，以提供更佳的保護以防止認證遭竊。 針對群組的成員，Windows 8.1 裝置或 Windows Server 2012 R2 主機不會快取受保護使用者不支援的認證。 如果此群組的成員登入執行早于 Windows 8.1 之 Windows 版本的裝置，則不會有額外的保護。

已登入 Windows 8.1 裝置和 Windows Server 2012 R2 主機的 Protected Users 群組成員無法*再*使用：

-   預設認證委派 (CredSSP) - 這是即使啟用「允許委派預設認證」 原則，也不會被快取的純文字認證

-   Windows 摘要 - 這是即使啟用也不會被快取的純文字認證

-   NTLM - 不會快取 NTOWF

-   Kerberos 長期金鑰 - 登入時會取得 Kerberos 票證授權票證 (TGT)，而且無法自動重新取得

-   離線登入 - 不會建立快取的登入檢查器

如果網域功能等級是 Windows Server 2012 R2，群組的成員就不能再：

-   使用 NTLM 驗證進行驗證

-   在 Kerberos 預先驗證中使用資料加密標準 (DES) 或 RC4 加密套件

-   受非限制型或限制型委派的委派

-   在初始 4 小時存留期之後更新使用者票證 (TGT)

若要將使用者新增至群組，您可以使用[UI 工具](https://technet.microsoft.com/library/cc753515.aspx)，例如 ACTIVE DIRECTORY 管理中心（ADAC）或 Active Directory 使用者和電腦，或命令列工具（例如[Dsmod 群組](https://technet.microsoft.com/library/cc732423.aspx)）或 Windows PowerShell[add-adgroupmember](https://technet.microsoft.com/library/ee617210.aspx) Cmdlet。 服務和電腦的帳戶 *不應該* 是 Protected Users 群組的成員。 因為主機上的密碼或憑證永遠都可使用，所以那些帳戶的成員資格不提供任何本機的保護。

> [!WARNING]
> 驗證限制沒有因應措施，表示高特殊權限群組 (例如 Enterprise Admins 群組或 Domain Admins 群組) 的成員與 Protected Users 群組的其他成員受到一樣的限制。 如果將這類群組的所有成員新增到 Protected Users 群組，則所有這些帳戶都可能被鎖定。您絕對不應該將所有高許可權的帳戶新增到 Protected Users 群組，直到您徹底測試過可能的影響。

Protected Users 群組的成員必須能夠使用具備進階加密標準 (AES) 的 Kerberos 進行驗證。 此方法需要 Active Directory 中帳戶的 AES 金鑰。 內建的系統管理員沒有 AES 金鑰，除非在執行 Windows Server 2008 或更新版本的網域控制站上變更了密碼。 此外，在執行舊版 Windows Server 的網域控制站上變更密碼的任何帳戶都會被鎖定。因此，請遵循下列最佳作法：

-   請勿在網域中進行測試，除非**所有網域控制站都執行 Windows Server 2008 或更新版本**。

-   為在建立網域**之前** 就建立的所有網域帳戶 *變更密碼* 。 否則，這些帳戶都會無法驗證。

-   請先變更每個使用者的**密碼**，再將帳戶新增到 Protected Users 群組，或確定最近在執行 Windows Server 2008 或更新版本的網域控制站上變更了密碼。

### <a name="BKMK_Prereq"></a>使用受保護帳戶的需求
受保護的帳戶有下列部署需求：

-   若要為受保護的使用者提供用戶端限制，主機必須執行 Windows 8.1 或 Windows Server 2012 R2。 使用者只需以 Protected Users 群組的成員帳戶登入。 在此情況下，可以藉由將[主域控制站（PDC）模擬器角色傳輸](https://technet.microsoft.com/library/cc816944(v=ws.10).aspx)到執行 Windows Server 2012 R2 的網域控制站來建立 Protected Users 群組。 該群組物件複寫到其他網域控制站之後，可以在執行舊版 Windows Server 的網域控制站上裝載 PDC 模擬器角色。

-   若要為受保護的使用者提供網域控制站端限制，也就是限制 NTLM 驗證的使用方式，以及其他限制，網域功能等級必須是 Windows Server 2012 R2。 如需功能等級的詳細資訊，請參閱 [了解 Active Directory 網域服務 (AD DS) 功能等級](../active-directory-functional-levels.md)。

### <a name="BKMK_TrubleshootingEvents"></a>針對與受保護使用者相關的事件進行疑難排解
本節涵蓋的新記錄檔可協助疑難排解與 Protected Users 相關的事件，以及 Protected Users 影響變更的方式，以疑難排解票證授權票證 (TGT) 到期或委派的問題。

#### <a name="new-logs-for-protected-users"></a>Protected Users 的新記錄檔

有兩個新的作業系統記錄管理檔可協助您疑難排解與受保護使用者相關的事件：受保護的使用者-用戶端記錄檔和受保護的使用者失敗-網域控制站記錄檔。 這些新的記錄檔位於 [事件檢視器] 中，且預設為停用。 若要啟用記錄檔，請依序按一下 [應用程式及服務記錄檔]、[Microsoft]、[Windows]、[驗證]，然後按一下記錄檔的名稱，再按一下 [動作] (或在記錄檔上按一下滑鼠右鍵)，按一下 [啟用記錄]。

如需這些記錄檔中的事件的詳細資訊，請參閱 [驗證原則和驗證原則定址接收器](https://technet.microsoft.com/library/dn486813.aspx)。

#### <a name="troubleshoot-tgt-expiration"></a>疑難排解 TGT 到期
一般而言，網域控制站會根據下列 [群組原則管理編輯器] 視窗中顯示的網域原則設定 TGT 存留期和更新。

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

對於 [Protected Users]，下列設定是硬式編碼：

-   使用者票證最長存留期：240 分鐘

-   使用者票證更新的最長存留期：240 分鐘

#### <a name="troubleshoot-delegation-issues"></a>疑難排解委派問題
過去，如果使用 Kerberos 委派的技術失敗，系統會檢查是否已設定用戶端帳戶的 [這是機密帳戶，無法委派] 。 不過，如果帳戶是 **Protected Users**的成員，在 Active Directory 管理中心 (ADAC) 中就不會有此設定。 因此，當您疑難排解委派問題時，請檢查設定與群組成員資格。

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

### <a name="BKMK_AuditAuthNattempts"></a>審核驗證嘗試
若要對 **Protected Users** 群組的成員明確稽核驗證嘗試，您可以繼續收集安全性記錄檔稽核事件或在新的操作系統管理記錄檔中收集資料。 如需這些事件的詳細資訊，請參閱 [驗證原則和驗證原則定址接收器](https://technet.microsoft.com/library/dn486813.aspx)

### <a name="BKMK_ProvidePUdcProtections"></a>提供服務和電腦的 DC 端保護
服務與電腦的帳戶不能是 **Protected Users**的成員。 本節說明可對這些帳戶提供的網域控制站型保護：

-   拒絕 NTLM 驗證：僅可透過 [NTLM 封鎖原則](https://technet.microsoft.com/library/jj865674(v=ws.10).aspx)設定

-   拒絕 Kerberos 預先驗證中的資料加密標準（DES）： Windows Server 2012 R2 網域控制站不接受電腦帳戶的 DES，除非已針對 DES 進行設定，因為使用 Kerberos 發行的每個 Windows 版本也支援RC4.

-   拒絕 Kerberos 預先驗證中的 RC4：無法設定。

    > [!NOTE]
    > 雖然可以 [變更支援之加密類型的設定](https://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx)，但是不建議未在目標環境中測試過，就變更電腦帳戶的這些設定。

-   限制使用者票證 (TGT) 為初始 4 小時存留期：使用驗證原則。

-   使用非限制或限制委派拒絕委派：若要限制帳戶，請開啟 [Active Directory 管理中心 (ADAC)]，然後選取 [這是機密帳戶，無法委派] 核取方塊。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TshootDelegation.gif)

## <a name="BKMK_CreateAuthNPolicies"></a>驗證原則
「驗證原則」是 AD DS 中包含驗證原則物件的新容器。 驗證原則可以指定設定，協助降低認證竊取的機會，例如限制帳戶的 TGT 存留期或新增其他與宣告相關條件。

在 Windows Server 2012 中，Dynamic 存取控制引進了稱為「集中存取原則」的 Active Directory 樹系範圍物件類別，以提供簡單的方式來設定整個組織的檔案伺服器。 在 Windows Server 2012 R2 中，新的物件類別稱為「驗證原則」（objectClass Msds-authnpolicies），可以用來將驗證設定套用至 Windows Server 2012 R2 網域中的帳戶類別。 Active Directory 帳戶類別包括：

-   使用者

-   [電腦] 旁的核取方塊

-   受管理的服務帳戶與群組受管理的服務帳戶 (GMSA)

### <a name="quick-kerberos-refresher"></a>快速複習 Kerberos
Kerberos 驗證通訊協定由三種類型的交換 (也稱為子通訊協定) 組成：

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbRefresher.gif)

-   驗證服務 (AS) 交換 (KRB_AS_*)

-   票證授權服務 (TGS) 交換 (KRB_TGS_ *)

-   用戶端/伺服器 (AP) 交換 (KRB_AP_ *)

AS exchange 是用戶端使用帳戶的密碼或私密金鑰來建立預先驗證器以要求票證授權票證（TGT）的位置。 這會發生在使用者登入或第一次需要服務票證時。

TGS 交換是用來建立驗證器以要求服務票證的帳戶 TGT。 這會發生在需要已驗證的連線時。

AP 交換通常會發生在應用程式通訊協定內的資料，且不會受到驗證原則的影響。

如需詳細資訊，請參閱 [Kerberos 版本 5 驗證通訊協定的運作方式](https://technet.microsoft.com/library/cc772815(v=WS.10).aspx)。

### <a name="overview"></a>概觀
驗證原則提供一種方式可將設定的限制套用到帳戶，並對服務與電腦的帳戶提供限制，來彌補 Protected Users 的不足。 在 AS 交換或 TGS 交換期間，會強制實行驗證原則。

您可以透過設定下列項目，限制初始驗證或 AS 交換：

-   TGT 存留期

-   限制使用者登入，產生 AS 交換的裝置必須符合的存取控制條件

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictAS.gif)

您可以透過設定下列項目，透過票證授權服務 (TGS) 交換來限制服務票證要求：

-   產生 TGS 交換的用戶端 (使用者、服務、電腦) 或裝置必須符合的存取控制條件

### <a name="BKMK_ReqForAuthnPolicies"></a>使用驗證原則的需求

|原則|需求|
|----------|----------------|
|提供自訂 TGT 存留期| Windows Server 2012 R2 網域功能等級帳戶網域|
|限制使用者登入|-Windows Server 2012 R2 網域功能等級帳戶網域（含動態存取控制支援）<br />-Windows 8、Windows 8.1、Windows Server 2012 或具有動態存取控制支援的 Windows Server 2012 R2 裝置|
|根據使用者帳戶與安全性群組來限制服務票證發行| Windows Server 2012 R2 網域功能等級資源網域|
|根據使用者宣告或裝置帳戶、安全性群組或宣告來限制服務票證發行| Windows Server 2012 R2 網域功能等級資源網域（具有動態存取控制支援）|

### <a name="restrict-a-user-account-to-specific-devices-and-hosts"></a>限制使用者帳戶存取特定裝置與主機
具備系統管理權限的高價值帳戶必須是 **Protected Users** 群組的成員。 依照預設，沒有任何帳戶是 **Protected Users** 群組的成員。 在將帳戶新增至群組之前，請設定網域控制站支援並建立稽核原則，以確保沒有任何封鎖問題。

#### <a name="configure-domain-controller-support"></a>設定網域控制站支援

使用者的帳戶網域必須是 Windows Server 2012 R2 網域功能等級（DFL）。 請確定所有網域控制站都是 Windows Server 2012 R2，然後使用 Active Directory 網域和信任，將[DFL 提升](https://technet.microsoft.com/library/cc753104.aspx)至 windows Server 2012 r2。

**設定動態存取控制的支援**

1.  在 [預設網域控制站原則] 中，按一下 [啟用] 以啟用 [電腦設定 | 系統管理範本 | 系統 | KDC] 中的 [宣告、複合驗證與 Kerberos 防護的金鑰發佈中心 (KDC) 用戶端支援]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EnableKDCClaims.gif)

2.  在 [選項]下的下拉式清單方塊中，選取 [永遠提供宣告]。

    > [!NOTE]
    > 也可以設定**支援**，但因為網域是在 Windows Server 2012 R2 DFL，所以當使用非宣告感知裝置和主機連接到宣告感知服務時，dc 一律會提供宣告，讓使用者宣告式存取檢查發生。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AlwaysProvideClaims.png)

    > [!WARNING]
    > 設定**Fail 未受防護驗證要求**會導致來自不支援 Kerberos 防護之任何作業系統（例如 windows 7 和舊版作業系統），或從 windows 8 開始的作業系統（尚未明確設定為支援它）的驗證失敗。

#### <a name="create-a-user-account-audit-for-authentication-policy-with-adac"></a>使用 ADAC 建立驗證原則的使用者帳戶稽核

1.  開啟 [Active Directory 管理中心 (ADAC)]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_OpenADAC.gif)

    > [!NOTE]
    > 在 Windows Server 2012 R2 DFL 的網域中，會顯示選取的 [**驗證**] 節點。 如果節點未出現，請使用 Windows Server 2012 R2 DFL 網域中的網域系統管理員帳戶，再試一次。

2.  按一下 [驗證原則]，然後按一下 [新增] 以建立新的原則。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicy.gif)

    驗證原則都必須要有顯示名稱，而且預設為一定要有。

3.  若要建立僅稽核的原則，請按一下 [僅稽核原則限制]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicyAuditOnly.gif)

    驗證原則會根據 Active Directory 帳戶類型套用。 為每個類型進行設定，單一原則可套用至所有三種帳戶類型。 帳戶類型有：

    -   使用者

    -   [電腦] 旁的核取方塊

    -   受管理的服務帳戶與群組受管理的服務帳戶

    如果您已採用可被金鑰發佈中心 (KDC) 使用的新主體延伸結構描述，則會從最接近的衍生帳戶類型將新的帳戶類型加以分類。

4.  若要設定使用者帳戶的 TGT 存留期，請選取 [指定使用者帳戶的票證授權票證存留期] 核取方塊並輸入時間 (單位為分鐘)。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTLifetime.gif)

    例如，如果您想要以 10 小時做為最大的 TGT 存留期，請如所示輸入 **600** 。 若未設定 TGT 存留期，且帳戶是 **Protected Users** 群組的成員，則 TGT 存留期和更新為 4 小時。 否則，TGT 存留期和更新會根據網域的下列 [群組原則管理編輯器] 視窗中所顯示的網域原則採取預設設定。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_TGTExpiration.png)

5.  若要將使用者帳戶限制到特定裝置，請按一下 [編輯] 定義裝置所需的條件。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_EditAuthNPolicy.gif)

6.  在 [編輯存取控制條件] 視窗中，按一下 [新增條件]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCondition.png)

##### <a name="add-computer-account-or-group-conditions"></a>新增電腦帳戶或群組條件

1.  若要設定電腦帳戶或群組，請在下拉式清單中選取下拉式清單方塊 [成員隸屬每個] 並變更為 [成員隸屬任何]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompMember.png)

    > [!NOTE]
    > 此存取控制定義使用者從其登入的裝置或主機條件。 在存取控制術語中，裝置或主機的電腦帳戶是使用者，這也是為什麼 [使用者] 是唯一的選項。

2.  按一下 [新增項目]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddItems.png)

3.  若要變更物件類型，請按一下 [物件類型]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjects.gif)

4.  若要選取 Active Directory 中的電腦物件，請按一下 [電腦]，然後按一下 [確定]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsComputers.gif)

5.  輸入要限制使用者的電腦名稱，然後按一下 [檢查名稱]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_ChangeObjectsCompName.gif)

6.  按一下 [確定] 並為該電腦帳戶建立任何其他條件。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompAddConditions.png)

7.  完成之後，按一下 [確定] ，就會顯示為電腦帳戶定義的條件。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AddCompDone.png)

##### <a name="add-computer-claim-conditions"></a>新增電腦宣告條件

1.  若要設定電腦宣告，請在 [群組] 下拉式清單中選取宣告。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaim.gif)

    宣告只有在樹系中佈建好之後才可使用。

2.  輸入 OU 的名稱，限制使用者帳戶只能登入。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimOUName.gif)

3.  完成之後，按一下 [確定]，方塊會顯示定義的條件。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CompClaimComplete.gif)

##### <a name="troubleshoot-missing-computer-claims"></a>疑難排解遺失的電腦宣告
如果宣告已佈建但無法使用，它可能只適用於 [電腦] 類別。

假設您想要根據電腦的組織單位（OU）限制驗證，這是已設定但僅適用于**電腦**類別的。

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictComputers.gif)

對於要用來限制使用者登入裝置的宣告，請選取 [使用者] 核取方塊。

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_RestrictUsersComputers.gif)

#### <a name="provision-a-user-account-with-an-authentication-policy-with-adac"></a>使用 ADAC 佈建具驗證原則的使用者帳戶

1.  從 [使用者] 帳戶，按一下 [原則]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicy.gif)

2.  選取 [指派驗證原則至此帳戶] 核取方塊。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicyAssign.gif)

3.  接著，選取要套用到使用者的驗證原則。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_UserPolicySelect.png)

#### <a name="configure-dynamic-access-control-support-on-devices-and-hosts"></a>在裝置與主機上設定動態存取控制支援
您可以設定 TGT 存留期，而不需設定動態存取控制 (DAC)。 只有在檢查 AllowedToAuthenticateFrom 與 AllowedToAuthenticateTo 時才需要 DAC。

使用群組原則或本機群組原則編輯器，啟用 [電腦設定 | 系統管理範本 | 系統 | Kerberos] 中的 [宣告、複合驗證與 Kerberos 防護的 Kerberos 用戶端支援]：

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_KerbClientDACSupport.gif)

### <a name="BKMK_TroubleshootAuthnPolicies"></a>疑難排解驗證原則

#### <a name="determine-the-accounts-that-are-directly-assigned-an-authentication-policy"></a>判斷已直接指派驗證原則的帳戶
[驗證原則] 中的 [帳戶] 區段會顯示已直接套用原則的帳戶。

![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_AccountsAssigned.gif)

#### <a name="use-the-authentication-policy-failures---domain-controller-administrative-log"></a>使用驗證原則失敗-網域控制站系統管理記錄
新的**驗證原則失敗-** [**應用程式及服務記錄**檔] 下的網域控制站系統管理記錄 > **Microsoft** > **Windows** > **驗證**已建立，以讓您更輕鬆地探索因驗證原則而造成的失敗。 該記錄檔預設為停用。 若要啟用它，請在記錄檔名稱上按一下滑鼠右鍵，然後按一下 [啟用記錄]。 新的事件在內容上非常類似現有的 Kerberos TGT 與服務票證稽核事件。 如需這些事件的詳細資訊，請參閱 [驗證原則和驗證原則定址接收器](https://technet.microsoft.com/library/dn486813.aspx)。

### <a name="BKMK_ManageAuthnPoliciesUsingPSH"></a>使用 Windows PowerShell 管理驗證原則
此命令會建立一個名為 **TestAuthenticationPolicy**的驗證原則。 **UserAllowedToAuthenticateFrom** 參數指定使用者可透過名為 someFile.txt 之檔案中的 SDDL 字串，從其驗證的裝置。

```
PS C:\> New-ADAuthenticationPolicy testAuthenticationPolicy -UserAllowedToAuthenticateFrom (Get-Acl .\someFile.txt).sddl
```

此命令可取得符合 **Filter** 參數指定之篩選條件的所有驗證原則。

```
PS C:\> Get-ADAuthenticationPolicy -Filter "Name -like 'testADAuthenticationPolicy*'" -Server Server02.Contoso.com

```

此命令可修改指定之驗證原則的描述和 **UserTGTLifetimeMins** 屬性。

```
PS C:\> Set-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1 -Description "Description" -UserTGTLifetimeMins 45
```

此命令可移除 **Identity** 參數指定的驗證原則。

```
PS C:\> Remove-ADAuthenticationPolicy -Identity ADAuthenticationPolicy1
```

此命令使用 **Get-ADAuthenticationPolicy** Cmdlet 搭配 **Filter** 參數，以取得不會強制執行的所有驗證原則。 系統會使用管線傳送結果集至 **Remove-ADAuthenticationPolicy** cmdlet。

```
PS C:\> Get-ADAuthenticationPolicy -Filter 'Enforce -eq $false' | Remove-ADAuthenticationPolicy
```

## <a name="BKMK_CreateAuthNPolicySilos"></a>驗證原則定址接收器
「驗證原則定址接收器」是使用者、電腦及服務帳戶在 AD DS 中的新容器 (objectClass msDS-AuthNPolicySilos)。 它們可以協助保護高價值的帳戶。 雖然所有組織都必須保護 Enterprise Admins、 Domain Admins 與 Schema Admins 群組的成員，因為那些帳戶可能會被攻擊者用來存取樹系中的任何項目，但是其他帳戶可能也都需要保護。

某些組織會建立專屬的帳戶並套用群組原則設定，限制本機和遠端互動式登入和系統管理員權限，以隔離工作負載。 驗證原則定址接收器則建立一種方式，定義使用者、電腦與受管理的服務帳戶之間的關係，來補足這件工作。 帳戶只能屬於一個定址接收器。 您可以為每個類型的帳戶設定驗證原則以便控制：

1.  不可更新的 TGT 存留期

2.  傳回 TGT 的存取控制條件 (注意：因為需要 Kerberos 防護，因此無法套用到系統)

3.  傳回服務票證的存取控制條件

此外，驗證原則定址接收器中的帳戶有定址接收器宣告，可由具宣告感知 (Claims-Aware) 功能的資源 (例如檔案伺服器) 用來控制存取。

您可以設定新的安全性描述元，根據下列項目控制服務票證的發行：

-   使用者、使用者的安全性群組和/或使用者的宣告

-   裝置、裝置的安全性群組和/或裝置的宣告

將此資訊取得資源的 Dc 需要動態存取控制：

-   使用者宣告：

    -   支援動態存取控制的 Windows 8 和更新版本用戶端

    -   帳戶網域支援動態存取控制和宣告

-   裝置和/或裝置安全性群組：

    -   支援動態存取控制的 Windows 8 和更新版本用戶端

    -   設定為複合驗證的資源

-   裝置宣告：

    -   支援動態存取控制的 Windows 8 和更新版本用戶端

    -   裝置網域支援動態存取控制和宣告

    -   設定為複合驗證的資源

驗證原則可以套用到驗證原則定址接收器的所有成員而不是個別帳戶，或者不同驗證原則可以套用到定址接收器內不同類型的帳戶。 例如，一個驗證原則可以套用到高特殊權限的使用者帳戶，另一個不同的原則可以套用到服務帳戶。 您必須至少先建立一個驗證原則，才能建立驗證原則定址接收器。

> [!NOTE]
> 一個驗證原則可以套用到一個驗證原則定址接收器的多個成員，或者分別套用到多個定址接收器以限制特定的帳戶範圍。 例如，若要保護單一帳戶或一小組帳戶，可針對那些帳戶設定原則，而不需要將那些帳戶加入定址接收器。

您可以使用 Active Directory 管理中心或 Windows PowerShell 來建立驗證原則定址接收器。 根據預設，驗證原則定址接收器只會審核定址接收器原則，這相當於在 Windows PowerShell Cmdlet 中指定**WhatIf**參數。 在此情況下，不會套用原則定址接收器限制，但是會產生稽核，指出若套用限制是否發生失敗。

#### <a name="to-create-an-authentication-policy-silo-by-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心建立驗證原則定址接收器

1.  開啟 [Active Directory 管理中心]，按一下 [驗證]，以滑鼠右鍵按一下 [驗證原則定址接收器]，按一下 [新增]，然後按一下 [驗證原則定址接收器]。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_CreateNewAuthNPolicySilo.gif)

2.  在 [顯示名稱]中，輸入定址接收器的名稱。 在 [允許的帳戶]中，按一下 [新增]，輸入帳戶的名稱，然後按一下 [確定]。 您可以指定使用者、電腦或服務帳戶。 接著，指定要對所有主體使用單一原則，或對每個類型的主體使用不同的原則，以及原則的名稱。

    ![受保護的帳戶](media/How-to-Configure-Protected-Accounts/ADDS_ProtectAcct_NewAuthNPolicySiloDisplayName.gif)

### <a name="BKMK_ManageAuthnSilosUsingPSH"></a>使用 Windows PowerShell 管理驗證原則定址接收器
此命令會建立驗證原則定址接收器物件，並強制使用。

```
PS C:\>New-ADAuthenticationPolicySilo -Name newSilo -Enforce
```

此命令可取得符合由 **Filter** 參數指定之篩選條件的所有驗證原則定址接收器。 輸出會傳遞至 **Format-Table** Cmdlet 以顯示原則的名稱，以及每個原則上 **Enforce** 的值。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Name -like "*silo*"' | Format-Table Name, Enforce -AutoSize

Name  Enforce
----  -------
silo     True
silos   False

```

此命令會使用 **Get-ADAuthenticationPolicySilo** Cmdlet 搭配 **Filter** 參數，以取得不會強制執行的所有驗證原則定址接收器，並且將篩選條件的結果以管線方式輸出到 **Remove-ADAuthenticationPolicySilo** Cmdlet。

```
PS C:\>Get-ADAuthenticationPolicySilo -Filter 'Enforce -eq $False' | Remove-ADAuthenticationPolicySilo
```

此命令會將名為 *Silo* 之驗證原則定址接收器的存取權授與名為 *User01*的使用者帳戶。

```
PS C:\>Grant-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01
```

此命令會撤銷名為 *User01* 的使用者帳戶對名為 *Silo*之驗證原則定址接收器的存取權。 因為 **Confirm** 參數設定為 **$False**，所以不會顯示任何確認訊息。

```
PS C:\>Revoke-ADAuthenticationPolicySiloAccess -Identity Silo -Account User01 -Confirm:$False
```

此範例一開始使用 **Get-ADComputer** Cmdlet 來取得符合 **Filter** 參數指定之篩選條件的所有電腦帳戶。 此命令的輸出會傳遞至 **Set-ADAccountAuthenticatinPolicySilo** 以便將名為 *Silo* 的驗證原則定址接收器和名為 *AuthenticationPolicy02* 的驗證原則指派給它們。

```
PS C:\>Get-ADComputer -Filter 'Name -like "newComputer*"' | Set-ADAccountAuthenticationPolicySilo -AuthenticationPolicySilo Silo -AuthenticationPolicy AuthenticationPolicy02
```



