---
description: '深入瞭解如何使用 Active Directory 管理中心 (級200進行 Advanced AD DS 管理) '
ms.assetid: 4d21d27d-5523-4993-ad4f-fbaa43df7576
title: Advanced AD DS Management Using Active Directory Administrative Center (Level 200)
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 56f97929f14c1d8e9ff8819b6d821dade79c2535
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043416"
---
# <a name="advanced-ad-ds-management-using-active-directory-administrative-center-level-200"></a>Advanced AD DS Management Using Active Directory Administrative Center (Level 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此主題涵蓋更新之 Active Directory 管理中心的新 Active Directory 資源回收筒、更細緻的密碼原則與 Windows PowerShell 歷程記錄檢視器的詳細資料，包括架構、常見工作的範例與疑難排解資訊。 如需簡介，請參閱 [Active Directory 管理中心增強功能 &#40;層級 100&#41;的簡介 ](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)。

- [Active Directory 管理中心架構](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Arch)
- [使用 Active Directory 管理中心來啟用及管理 Active Directory 資源回收筒](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_EnableRecycleBin)
- [使用 Active Directory 管理中心來設定及管理更細緻的密碼原則](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_FGPP)
- [使用 Active Directory 管理中心 Windows PowerShell 歷程記錄檢視器](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_HistoryViewer)
- [疑難排解 AD DS 管理](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md#BKMK_Tshoot)

## <a name="active-directory-administrative-center-architecture"></a><a name="BKMK_Arch"></a>Active Directory 管理中心架構

### <a name="active-directory-administrative-center-executables-dlls"></a>Active Directory 管理中心可執行檔，Dll

Active Directory 管理中心的模組和基礎架構並未隨著新的資源回收筒、FGPP 與歷程記錄檢視器功能而變更。

- Microsoft.ActiveDirectory.Management.UI.dll
- Microsoft.ActiveDirectory.Management.UI.resources.dll
- Microsoft.ActiveDirectory.Management.dll
- Microsoft.ActiveDirectory.Management.resources.dll
- ActiveDirectoryPowerShellResources.dll

下面說明新資源回收筒功能的基礎 Windows PowerShell 和操作層：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/adds_adrestore.png)

## <a name="enabling-and-managing-the-active-directory-recycle-bin-using-active-directory-administrative-center"></a><a name="BKMK_EnableRecycleBin"></a>使用 Active Directory 管理中心來啟用及管理 Active Directory 資源回收筒

### <a name="capabilities"></a>功能

- Windows Server 2012 或更新版本的 Active Directory 管理中心可讓您設定及管理樹系中任何網域磁碟分割的 Active Directory 資源回收筒。 不再需要使用 Windows PowerShell 或 Ldp.exe 來啟用 Active Directory 資源回收筒或還原網域分割中的物件。
- Active Directory 管理中心提供進階篩選條件，可讓您在具有許多刻意刪除之物件的大型環境中較容易執行目標性還原。

### <a name="limitations"></a>限制

- 由於 Active Directory 管理中心只能管理網域分割，因此它無法從設定、網域 DNS 或樹系 DNS 分割 (您無法從架構分割刪除物件) 還原已刪除的物件。 若要從非網域磁碟分割還原物件，請使用 [Restore-ADObject](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee617262(v=technet.10))。

- Active Directory 管理中心無法以單一動作還原物件的樹狀子目錄。 例如，如果您刪除含巢狀 OU、使用者、群組與電腦的 OU，還原基底 OU 時並不會還原子物件。

    > [!NOTE]
    > Active Directory 管理中心批次還原作業只會在 *選取範圍內* 對已刪除的物件進行「最大努力」的排序，讓父代在還原清單的子系之前排序。 在簡單的測試案例中，可以用單一動作還原物件的樹狀子目錄。 但角落的案例（例如，包含部分樹狀結構的選取專案）包含部分樹狀結構樹狀結構，其中某些已刪除的父節點遺失或錯誤案例，例如父還原失敗時略過子物件，可能無法如預期般運作。 基於這個理由，您應該一律在還原父物件之後，以個別的動作還原物件的子樹狀目錄。

Active Directory 資源回收筒需要 Windows Server 2008 R2 樹系功能等級，而且您必須是 Enterprise Admins 群組的成員。 啟用 Active Directory 資源回收筒之後，您便無法將它停用。 Active Directory 資源回收筒會使得樹系中每個網域控制站上的 Active Directory 資料庫 (NTDS.DIT) 大小增加。 資源回收筒所使用的磁碟空間會隨著時間持續增加，因為它會保留物件與其所有屬性資料。

### <a name="enabling-active-directory-recycle-bin-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心來啟用 Active Directory 資源回收筒

若要啟用 Active Directory 資源回收筒，請開啟 [Active Directory 管理中心]，然後按一下瀏覽窗格中您樹系的名稱。 從 [工作] 窗格中，按一下 [啟用資源回收筒]。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBin.png)

Active Directory 管理中心會顯示 [啟用資源回收筒確認] 對話方塊。 這個對話方塊會警告您啟用資源回收筒是無法還原的動作。 按一下 [確定] 來啟用 Active Directory 資源回收筒。 Active Directory 管理中心會顯示另一個對話方塊，提醒您 Active Directory 資源回收筒要等到所有網域控制站都複寫設定變更之後，才能完全正常運作。

> [!IMPORTANT]
> 在下列情況下，無法使用啟用 Active Directory 資源回收筒的選項：
>
> - 樹系功能等級低於 Windows Server 2008 R2
> - 選項已啟用

對等的 Active Directory Windows PowerShell Cmdlet 是：

```powershell
Enable-ADOptionalFeature
```

如需有關使用 Windows PowerShell 來啟用 Active Directory 資源回收筒的詳細資訊，請參閱 [Active Directory 資源回收筒逐步指南](./introduction-to-active-directory-administrative-center-enhancements--level-100-.md#active-directory-recycle-bin-step-by-step)。

### <a name="managing-active-directory-recycle-bin-using-active-directory-administrative-center"></a>使用 Active Directory 管理中心來管理 Active Directory 資源回收筒

本節使用名為 **corp.contoso.com** 的現有網域做為範例。 此網域會將使用者組織成名為 **UserAccounts** 的父系 OU。 **UserAccounts** OU 包含三個依部門命名的子系 OU，每個皆包含進一步的 OU、使用者與群組。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_EnableRecycleBinExampleOU.png)

#### <a name="storage-and-filtering"></a>儲存及篩選

Active Directory 資源回收筒會保留樹系中已刪除的所有物件。 它會根據 **msDS-deletedObjectLifetime** 屬性儲存這些物件，這個屬性預設是設定為符合樹系的 **tombstoneLifetime** 的屬性。 在任何使用 Windows Server 2003 SP1 或更新版本建立的樹系中，**tombstoneLifetime** 的值預設設定為 180 天。 在任何從 Windows 2000 升級或隨 Windows Server 2003 (不含 Service Pack) 安裝的樹系中，預設並未設定 tombstoneLifetime 屬性，因此 Windows 會使用內部預設值 (60 天)。 所有這些都是可設定的。您可以使用 Active Directory 管理中心從樹系的網域分割還原任何已刪除的物件。 您必須繼續使用 **Restore-ADObject** Cmdlet 從其他分割 (例如「設定」) 還原已刪除的物件。啟用 Active Directory 資源回收筒會在 Active Directory 管理中心內每個網域分割下面顯示 [刪除的物件] 容器。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_DeletedObjectsContainer.png)

「刪除的物件」容器會顯示該網域分割中所有可還原的物件。 存留期超過 **msDS-deletedObjectLifetime** 的已刪除物件稱為「已回收的物件」。 Active Directory 管理中心不會顯示已回收的物件，而且您也無法使用 Active Directory 管理中心來還原這些物件。

如需資源回收筒架構與處理規則的較深入說明，請參閱 [AD 資源回收筒：了解、實作、最佳做法以及疑難排解](/archive/blogs/askds/the-ad-recycle-bin-understanding-implementing-best-practices-and-troubleshooting)。

Active Directory 管理中心以人為方式將一個容器傳回的預設物件數目限制為 20,000 個物件。 您可以按一下 [管理] 功能表，然後按一下 [管理清單選項]，將此限制最高提高至 100,000 個物件。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_MgmtList.png)

#### <a name="restoration"></a>還原

##### <a name="filtering"></a>篩選

Active Directory 管理中心提供功能強大的條件和篩選選項，您應該先熟悉這些條件和選項，以便在您需要於實際還原中使用它們時使用。 網域會刻意刪除許多超過存留期的物件。如果可能刪除之物件的存留期為 180 天，當發生意外時，您將無法很簡單地就復原所有物件。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_AddCriteria.png)

與其撰寫複雜的 LDAP 篩選條件並將 UTC 值轉換成日期和時間，不如使用基本和進階 [篩選] 功能表只列出相關物件。 如果您知道刪除的日期、物件的名稱或任何其他關鍵資料，請利用該資料來進行篩選。 請按一下搜尋方塊右邊的＞形箭號來切換進階篩選選項。

還原操作與任何其他搜尋相同，支援所有標準的篩選條件選項。 在內建的篩選條件中，還原物件的重要篩選條件通常包括：

- *ANR (模糊名稱解析-功能表中沒有列出，但在您輸入 * * * * 篩選器 * * * * 方塊時，會使用什麼)*
- 上次修改時間 (介於指定日期之間)
- 物件是使用者/inetorgperson/電腦/群組/組織單位
- 名稱
- 刪除時
- 上次已知父系
- 類型
- 描述
- City
- 國家 / 地區
- department
- 員工識別碼
- 名字
- 職稱
- 姓氏
- SAMaccountname
- 州/省
- 電話號碼
- UPN
- 郵遞區號

您可以新增多個條件。 例如，您可以在2012年9月24日從芝加哥刪除所有使用者物件，並以經理職稱的職稱伊利諾州。

您也可以在評估要復原哪些物件時，新增、修改或重新排列欄標題來提供更多詳細資料。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ColumnHeaders.png)

如需有關模糊名稱解析的詳細資訊，請參閱 [ANR 屬性](/windows/win32/adschema/attributes-anr)。

##### <a name="single-object"></a>單一物件

還原已刪除的物件一直都是單一操作。  Active Directory 管理中心可以讓該操作更簡單。 若要還原已刪除的物件，例如單一使用者：

1. 按一下 Active Directory 管理中心瀏覽窗格中的網域名稱。
2. 按兩下管理清單中的 [刪除的物件]。
3. 在物件上按一下滑鼠右鍵，然後按一下 [還原]，或按一下 [工作] 窗格中的 [還原]。

物件會還原至其原始位置。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreSingle.gif)

按一下 [ **還原至** ] 以變更還原位置。 如果刪除的物件的父容器也已刪除，但是您不想還原父系，這會很有用。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestoreToSingle.gif)

##### <a name="multiple-peer-objects"></a>多個對等物件

您可以還原多個對等層級物件，例如某個 OU 中的所有使用者。 按住 CTRL 鍵，然後按一下一或多個您要還原的已刪除物件。 按一下 [工作] 窗格中的 [還原]。 您也可以按住 CTRL 鍵和 A 鍵來選取所有顯示的物件，或使用 SHIFT 鍵並按一下來選取某個範圍的物件。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RestorePeers.png)

##### <a name="multiple-parent-and-child-objects"></a>多個父物件和子物件

了解多重父系子系還原的還原程序相當重要，因為 Active Directory 管理中心無法以單一動作還原已刪除之物件的巢狀樹狀目錄。

1. 還原樹狀目錄中最頂端的已刪除物件。
2. 還原該父物件的直屬子系。
3. 還原那些父物件的直屬子系。
4. 視需要重複步驟，直到還原所有物件為止。

您無法先還原子物件，再還原其父系。 嘗試執行此還原作業會傳回下列錯誤：

**無法執行操作，因為物件的父系可能無法建立例項或已被刪除。**

「上次已知父系」屬性會顯示每個物件的父系關係。 在還原父系之後重新整理 Active Directory 管理中心時，「上次已知父系」屬性會從已刪除位置變更為已還原位置。 因此，當父物件的位置不再顯示 [已刪除物件] 容器的分辨名稱時，可以還原該子物件。

考量一下系統管理員不小心刪除包含子系 OU 和使用者的 Sales OU 案例。

首先，請觀察所有已刪除使用者的 **最後已知父** 屬性值，以及它如何讀取 **OU = Sales\0ADEL：*<guid + deleted objects 容器辨別名稱> * * *：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParent.gif)

篩選模糊名稱 Sales 以傳回已刪除的 OU (這是您稍後要還原的 OU)：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSales.png)

重新整理 Active Directory 管理中心，以查看已刪除的使用者物件最近已知的父屬性變更為還原的銷售 OU 辨別名稱：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesRestored.gif)

篩選所有 Sales 使用者。 按住 CTRL 鍵和 A 鍵來選取所有已刪除的 Sales 使用者。 按一下 [還原] 將物件從 [刪除的物件] 容器移到 Sales OU，其中物件的群組成員資格和屬性保持不變。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_LastKnownParentSalesUndelete.png)

如果 **Sales** OU 包含自己的子系 OU，則您將先還原這些子系 OU，再還原它們的子系 OU 等等。

若要透過指定已刪除的父容器來還原所有巢狀的已刪除物件，請參閱 [附錄 B：還原多個已刪除的 Active Directory 物件 (範例指令碼)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379504(v=ws.10))。

用於還原已刪除之物件的 Active Directory Windows PowerShell Cmdlet 是：

```powershell
Restore-adobject
```

**Restore-ADObject** Cmdlet 功能在 Windows Server 2008 R2 與 Windows Server 2012 之間並沒有變更。

##### <a name="server-side-filtering"></a>伺服器端篩選

在中型和大型企業中，「刪除的物件」容器在經過一段時間後，很可能會累積超過 20,000 (或甚至 100,000) 個物件，而無法顯示所有物件。 由於 Active Directory 管理中心的篩選機制依賴用戶端篩選，因此它無法顯示這些額外的物件。 若要解決這個限制，請使用下列步驟來執行伺服器端搜尋：

1. 在 [刪除的物件] 容器上按一下滑鼠右鍵，然後按一下 [在此節點下搜尋]。
2. 按一下＞形箭號以顯示 [+新增條件] 功能表，選取並新增 [上次修改時間介於指定日期之間]。 上次修改時間 (**whenChanged** 屬性) 是刪除時間的近似值，它們在大多數環境中都一樣。 這個查詢會執行伺服器端搜尋。
3. 在結果中使用進一步的顯示篩選、排序等等來找出要還原的已刪除物件，然後正常地還原它們。

## <a name="configuring-and-managing-fine-grained-password-policies-using-active-directory-administrative-center"></a><a name="BKMK_FGPP"></a>使用 Active Directory 管理中心來設定及管理更細緻的密碼原則

### <a name="configuring-fine-grained-password-policies"></a>設定更細緻的密碼原則

Active Directory 管理中心可讓您建立及管理更細緻的密碼原則 (FGPP) 物件。 Windows Server 2008 導入了 FGPP 功能，但是 Windows Server 2012 最先具有它的圖形化管理介面。 您是在網域層級套用更細緻的密碼原則，它可以讓您覆寫 Windows Server 2003 所需的單一網域密碼。 透過以不同的設定建立不同的 FGPP，個別的使用者或群組便可在網域中取得相異的密碼原則。

如需有關更細緻的密碼原則之資訊，請參閱 [AD DS 更細緻的密碼與帳戶鎖定原則逐步指南 (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770842(v=ws.10))。

在瀏覽窗格中，依序按一下樹狀檢視、您的網域、[系統]、[密碼設定容器]，然後在 [工作] 窗格中，按一下 [新增] 和 [密碼設定]。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_PasswordSettings.png)

### <a name="managing-fine-grained-password-policies"></a>管理更細緻的密碼原則

建立新的 FGPP 或編輯現有的 FGPP 會顯示 [密碼設定] 編輯器。 您可以從這裡設定所有想要的密碼原則，就如同在 Windows Server 2008 或 Windows Server 2008 R2 中一樣，只有現在是使用具有特殊用途的編輯器。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_CreatePasswordSettings.png)

填寫所有必要 (紅色星號) 的欄位和任何選用的欄位，然後按一下 [新增] 以設定接收這個原則的使用者或群組。 FGPP 會覆寫這些指定之安全性主體的預設網域原則設定。 在上圖中，有一個限制性極高的原則僅套用至內建的 Administrator 帳戶，用來防止洩露。 該原則對標準使用者來說太過複雜而難以遵守，但是非常適合只有 IT 專業人員才會使用的高風險帳戶。

您也可以設定指定網域內原則的優先順序，以及原則要套用至哪些使用者和群組。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_Precedence.png)

用於更細緻的密碼原則之 Active Directory Windows PowerShell Cmdlet 是：

```powershell
Add-ADFineGrainedPasswordPolicySubject
Get-ADFineGrainedPasswordPolicy
Get-ADFineGrainedPasswordPolicySubject
New-ADFineGrainedPasswordPolicy
Remove-ADFineGrainedPasswordPolicy
Remove-ADFineGrainedPasswordPolicySubject
Set-ADFineGrainedPasswordPolicy
```

更細緻的密碼原則 Cmdlet 功能在 Windows Server 2008 R2 與 Windows Server 2012 之間並沒有變更。 為了方便起見，下圖說明 Cmdlet 的相關引數：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPP.gif)

Active Directory 管理中心也可讓您針對特定的使用者尋找已套用之 FGPP 的結果組。 以滑鼠右鍵按一下任何使用者，然後按一下 [ **查看結果密碼設定** ]，以開啟透過隱含或明確指派套用至該使用者的 [ *密碼設定* ] 頁面：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RSOP.png)

檢查任何使用者或群組的 [內容] 會顯示 [直接關聯的密碼設定]，這些是明確指派的 FGPP：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_FGPPSettings.gif)

隱含 FGPP 指派不會顯示在此處;若要這樣做，您必須使用 [ **查看結果密碼設定 ...** ] 選項。

## <a name="using-the-active-directory-administrative-center-windows-powershell-history-viewer"></a><a name="BKMK_HistoryViewer"></a>使用 Active Directory 管理中心 Windows PowerShell 歷程記錄檢視器

Windows 管理未來會透過 Windows PowerShell 完成。 透過在工作自動化架構上建構分層圖形工具，使得最複雜分散式系統的管理變得一致且有效率。 您必須了解 Windows PowerShell 的運作方式，才能完全發揮您的潛能並將您的運算投資發揮到淋漓盡致。

Active Directory 管理中心現在提供它所執行之所有 Windows PowerShell Cmdlet 的完整歷程記錄及它們的引數和值。 您可以將 Cmdlet 歷程記錄複製到其他地方供研究或修改及重複使用。 您可以建立工作附註來協助隔離 Active Directory 管理中心命令在 Windows PowerShell 中造成的結果。 您也可以篩選歷程記錄來尋找感興趣的點。

Active Directory 管理中心 Windows PowerShell 歷程記錄檢視器的目的是要讓您透過實務經驗來了解。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_HistoryViewer.gif)

按一下＞形箭號來顯示 Windows PowerShell 歷程記錄檢視器。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RaiseViewer.png)

接著，建立使用者或修改群組的成員資格。 歷程記錄檢視器會不斷更新，提供 Active Directory 管理中心用指定引數所執行的每個 Cmdlet 的摺疊檢視。

展開任何感興趣的明細項目，即可查看提供給 Cmdlet 引數的所有值：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ViewArgs.png)

在使用 Active Directory 管理中心來建立、修改或刪除物件之前，請先按一下 [開始工作] 功能表來建立手動注釋。 輸入您剛才進行的工作。  完成變更之後，請選取 [結束工作]。 工作附註會將所有這些執行的動作組成一個可供您用來便於了解的可摺疊附註。

例如，若要查看用來變更某個使用者密碼及將該使用者從群組移除的 Windows PowerShell 命令：

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_RemoveUser.gif)

選取 [全部顯示] 核取方塊也會顯示只擷取資料的 Get-* 動詞命令 Windows PowerShell Cmdlet。

![Advanced AD DS 管理](media/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-/ADDS_ADAC_TR_ShowAll.png)

歷程記錄檢視器會顯示 Active Directory 管理中心執行的常值命令，而且您可能會注意到某些 Cmdlet 的執行似乎並非必要。 例如，您可以使用下列命令建立新的使用者：

```powershell
new-aduser
```

而不需要使用：

```powershell
set-adaccountpassword
enable-adaccount
set-aduser
```

Active Directory 管理中心的設計只需使用最低限度的程式碼和模組性。 因此，它不會使用一組函式來建立新的使用者，使用另一組函式來修改現有的使用者，而是會以最低限度的方式執行每一個函式，然後以 Cmdlet 將它們鏈結在一起。 在學習 Active Directory Windows PowerShell 時，請記住這一點。 您也可以利用這點做為一個學習技巧，了解您可以如何簡單地使用 Windows PowerShell 來完成單一工作。

## <a name="troubleshooting-ad-ds-management"></a><a name="BKMK_Tshoot"></a>疑難排解 AD DS 管理

### <a name="introduction-to-troubleshooting"></a>疑難排解簡介

由於對現有的客戶環境來說，疑難排解相對較新且缺乏使用，因此 Active Directory 管理中心已限制疑難排解選項。

### <a name="troubleshooting-options"></a>疑難排解選項

#### <a name="logging-options"></a>記錄選項

Active Directory 管理中心現在包含內建記錄，作為追蹤設定檔的一部分。 請在 dsac.exe 所在的資料夾中建立/修改下列檔案：

**dsac.exe.config**

請建立下列內容：

```xml
<appSettings>
  <add key="DsacLogLevel" value="Verbose" />
</appSettings>
<system.diagnostics>
 <trace autoflush="false" indentsize="4">
  <listeners>
   <add name="myListener"
    type="System.Diagnostics.TextWriterTraceListener"
    initializeData="dsac.trace.log" />
   <remove name="Default" />
  </listeners>
 </trace>
</system.diagnostics>
```

**DsacLogLevel** 的詳細資訊等級為 「無」、「錯誤」、「警告」、「資訊」及「詳細資訊」。 您可以設定輸出檔案名稱，這個檔案會寫入至 dsac.exe 所在的資料夾。 該輸出可以讓您深入了解 ADAC 的運作方式、它連線的網域控制站有哪些、執行的 Windows PowerShell 命令有哪些、有什麼回應，以及進一步的詳細資料。

例如，使用「資訊」等級時，會傳回除了追蹤等級詳細資訊以外的所有結果：

- DSAC.exe 啟動
- 記錄開始
- 網域控制站被要求傳回初始網域資訊

   ```
   [12:42:49][TID 3][Info] Command Id, Action, Command, Time, Elapsed Time ms (output), Number objects (output)
   [12:42:49][TID 3][Info] 1, Invoke, Get-ADDomainController, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADDomainController-Discover:$null-DomainName:"CORP"-ForceDiscover:$null-Service:ADWS-Writable:$null
   ```

- 從網域 Corp 傳回網域控制站 DC1
- 載入 PS AD 虛擬磁碟機

   ```
   [12:42:49][TID 3][Info] 1, Output, Get-ADDomainController, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] Found the domain controller 'DC1' in the domain 'CORP'.
   [12:42:49][TID 3][Info] 2, Invoke, New-PSDrive, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] New-PSDrive-Name:"ADDrive0"-PSProvider:"ActiveDirectory"-Root:""-Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 2, Output, New-PSDrive, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 3, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   ```

- 取得網域根目錄 DSE 資訊

   ```
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 3, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 4, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   ```

- 取得網域 AD 資源回收筒資訊

   ```
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 4, Output, Get-ADOptionalFeature, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 5, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 5, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 6, Invoke, Get-ADRootDSE, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADRootDSE -Server:"dc1.corp.contoso.com"
   [12:42:49][TID 3][Info] 6, Output, Get-ADRootDSE, 2012-04-16T12:42:49, 1
   [12:42:49][TID 3][Info] 7, Invoke, Get-ADOptionalFeature, 2012-04-16T12:42:49
   [12:42:49][TID 3][Info] Get-ADOptionalFeature -LDAPFilter:"(msDS-OptionalFeatureFlags=1)" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 7, Output, Get-ADOptionalFeature, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 8, Invoke, Get-ADForest, 2012-04-16T12:42:50
   ```

- 取得 AD 樹系

   ```
   [12:42:50][TID 3][Info] Get-ADForest -Identity:"corp.contoso.com" -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 8, Output, Get-ADForest, 2012-04-16T12:42:50, 1
   [12:42:50][TID 3][Info] 9, Invoke, Get-ADObject, 2012-04-16T12:42:50
   ```

- 取得所支援加密類型的結構描述資訊、FGPP、特定的使用者資訊

   ```
   [12:42:50][TID 3][Info] Get-ADObject
   -LDAPFilter:"(|(ldapdisplayname=msDS-PhoneticDisplayName)(ldapdisplayname=msDS-PhoneticCompanyName)(ldapdisplayname=msDS-PhoneticDepartment)(ldapdisplayname=msDS-PhoneticFirstName)(ldapdisplayname=msDS-PhoneticLastName)(ldapdisplayname=msDS-SupportedEncryptionTypes)(ldapdisplayname=msDS-PasswordSettingsPrecedence))"
   -Properties:lDAPDisplayName
   -ResultPageSize:"100"
   -ResultSetSize:$null
   -SearchBase:"CN=Schema,CN=Configuration,DC=corp,DC=contoso,DC=com"
   -SearchScope:"OneLevel"
   -Server:"dc1.corp.contoso.com"
   [12:42:50][TID 3][Info] 9, Output, Get-ADObject, 2012-04-16T12:42:50, 7
   [12:42:50][TID 3][Info] 10, Invoke, Get-ADObject, 2012-04-16T12:42:50
   ```

- 取得要對按下網域最上層節點之系統管理員顯示的所有網域物件相關資訊。

   ```
   [12:42:50][TID 3][Info] Get-ADObject
   -IncludeDeletedObjects:$false
   -LDAPFilter:"(objectClass=*)"
   -Properties:allowedChildClassesEffective,allowedChildClasses,lastKnownParent,sAMAccountType,systemFlags,userAccountControl,displayName,description,whenChanged,location,managedBy,memberOf,primaryGroupID,objectSid,msDS-User-Account-Control-Computed,sAMAccountName,lastLogonTimestamp,lastLogoff,mail,accountExpires,msDS-PhoneticCompanyName,msDS-PhoneticDepartment,msDS-PhoneticDisplayName,msDS-PhoneticFirstName,msDS-PhoneticLastName,pwdLastSet,operatingSystem,operatingSystemServicePack,operatingSystemVersion,telephoneNumber,physicalDeliveryOfficeName,department,company,manager,dNSHostName,groupType,c,l,employeeID,givenName,sn,title,st,postalCode,managedBy,userPrincipalName,isDeleted,msDS-PasswordSettingsPrecedence
   -ResultPageSize:"100"
   -ResultSetSize:"20201"
   -SearchBase:"DC=corp,DC=contoso,DC=com"
   -SearchScope:"Base"
   -Server:"dc1.corp.contoso.com"
   ```

設定「詳細資訊」等級也會顯示每個函式的 .NET 堆疊，但是除了在對發生存取違規或損毀的 Dsac.exe 進行疑難排解時有幫助以外，這些堆疊所包含的資料並不足以提供特別有用的資訊。 此問題的兩個可能原因包括：

- ADWS 服務沒有在任何可存取的網域控制站上執行。
- 從執行 Active Directory 管理中心的電腦上，對 ADWS 服務封鎖了網路通訊。

> [!IMPORTANT]
> 該服務還有一個頻外版本，稱為 [Active Directory 管理閘道](https://www.microsoft.com/download/en/details.aspx?displaylang=en&id=2852)，這個閘道可在 Windows Server 2008 SP2 和 Windows Server 2003 SP2 上執行。
>

當沒有任何 Active Directory Web 服務執行個體可用時，顯示的錯誤是：

|[錯誤]|作業|
| --- | --- |
|「 無法連線到任何網域。 請重新整理或在連線可用之後再試一次。」|在 Active Directory 管理中心應用程式啟動時顯示|
|「在執行 *<NetBIOS domain name>* Active Directory Web 服務 (ADWS) 」的網域中找不到可用的伺服器。|嘗試選取 Active Directory 管理中心內的某個網域節點時顯示|

若要針對此問題進行疑難排解，請使用下列步驟：

1. 確認 Active Directory Web 服務至少在網域中的一部網域控制站 (最好是樹系中的所有網域控制站) 上啟動。 也請確定將它在所有網域控制站上都設定為自動啟動。
2. 從執行 Active Directory 管理中心的電腦，確認您可以透過執行這些 NLTest.exe 命令來找出執行 ADWS 的伺服器：

   ```
   nltest /dsgetdc:<domain NetBIOS name> /ws /force
   nltest /dsgetdc:<domain fully qualified DNS name> /ws /force
   ```

   如果那些測試失敗 (即使 ADWS 服務有在執行)，問題通常是出在名稱解析或 LDAP，而不是出在 ADWS 或 Active Directory 管理中心。 如果 ADWS 沒有在任何網域控制站上執行，這個測試就會因發生 "1355 0x54B ERROR_NO_SUCH_DOMAIN" 錯誤而失敗，因此在下任何結論之前，請先仔細檢查。

3. 在 NLTest 所傳回的網域控制站上，使用下列命令傾印接聽連接埠清單：

   ```
   Netstat -anob > ports.txt
   ```

   檢查 ports.txt 檔案，並確認 ADWS 服務在連接埠 9389 上進行接聽。 範例：

   ```
   TCP    0.0.0.0:9389    0.0.0.0:0    LISTENING    1828
   [Microsoft.ActiveDirectory.WebServices.exe]

   TCP    [::]:9389       [::]:0       LISTENING    1828
   [Microsoft.ActiveDirectory.WebServices.exe]
   ```

   如果有在接聽，請驗證 Windows 防火牆規則，並確保它們允許 9389 TCP 輸入。 根據預設，網域控制站會啟用防火牆規則「Active Directory Web 服務 (TCP-in) 」。 如果沒有在接聽，請再次確認服務在此伺服器上執行，然後重新啟動服務。 確認沒有任何其他處理程序已經在連接埠 9389 上進行接聽。

4. 在執行 Active Directory 管理中心的電腦和 NLTEST 所傳回的網域控制站上，安裝 NetMon 或其他網路擷取公用程式。 從您啟動 Active Directory 管理中心的兩部電腦收集同時網路擷取，並在停止擷取之前先查看錯誤。 確認用戶端能夠透過連接埠 TCP 9389 傳送至網域控制站及從此網域控制站接收。 如果已傳送封包但一直沒送達，或是有送達且網域控制站有回覆，但是用戶端從未收到，則可能是網路上電腦之間有防火牆將該連接埠上的封包捨棄。 這個防火牆可能是軟體或硬體，也可能是協力廠商端點保護 (防毒) 軟體的一部分。

## <a name="see-also"></a>另請參閱

[AD 資源回收筒、更細緻的密碼原則和 PowerShell 歷程記錄](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md)
