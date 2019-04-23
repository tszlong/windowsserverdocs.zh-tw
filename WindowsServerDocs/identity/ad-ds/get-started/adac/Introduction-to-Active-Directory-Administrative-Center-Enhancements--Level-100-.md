---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduction to Active Directory Administrative Center Enhancements (Level 100)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d16447425b86fef979732c4785db27d1542116b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840169"
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction to Active Directory Administrative Center Enhancements (Level 100)

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

在 Windows Server Active Directory 管理中心內會包含下列管理功能：

- [Active Directory 資源回收筒](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)
- [更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)
- [Windows PowerShell 歷程記錄檢視器](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Active Directory 資源回收筒

意外刪除 Active Directory 物件是 Active Directory 網域服務 (AD DS) 與 Active Directory 輕量型目錄服務 (AD LDS) 的使用者經常會發生的狀況。 在過去的版本之前的 Windows Server、 Windows Server 2008 R2，可以復原意外刪除的物件，在 Active Directory，但此解決方案有其缺點。

在 Windows Server 2008 中，您可以使用 Windows Server Backup 功能與 **ntdsutil** 權威還原命令將物件標示為權威，以確保還原的資料會複寫整個網域。 權威還原解決方案的缺點是必須在目錄服務還原模式 (DSRM) 中執行。 在 DSRM 期間，所還原的網域控制站必須保持離線。 因此，無法為用戶端要求提供服務。

在 Windows Server 2003 Active Directory 與 Windows Server 2008 AD DS 中，您可以透過重新引發標記來復原刪除的 Active Directory 物件。 不過，實際上已移除的重新引發物件的連結值屬性 (例如，使用者帳戶的群組成員資格) 和已清除的非連結值屬性並無法復原。 因此，系統管理員無法以重新引發標記做為意外刪除物件的根本解決方案。 如需重新引發標記的詳細資訊，請參閱 [重新引發 Active Directory 標記物件](https://go.microsoft.com/fwlink/?LinkID=125452)。

從 Windows Server 2008 R2 開始，提供的 Active Directory 資源回收筒是建立在現有的標記重新引發基礎結構上，可以增強保存和復原意外刪除 Active Directory 物件的能力。

啟用 Active Directory 資源回收筒時，會保留已刪除之 Active Directory 物件的所有連結值與非連結值屬性，並將物件完整還原為與刪除之前相同的邏輯狀態。 例如，還原的使用者帳戶會自動重新取得它們在刪除之前，在網域內外擁有的所有群組成員資格和對應的存取權限。 Active Directory 資源回收筒可在 AD DS 與 AD LDS 環境中運作。 如需 Active Directory 資源回收筒的詳細說明，請參閱[的 AD DS 的新功能：Active Directory 資源回收筒](https://technet.microsoft.com/library/dd391916(WS.10).aspx)。

**新功能？** 在 Windows Server 2012 及更新版本的 Active Directory 資源回收筒功能已增強新的圖形化使用者介面，讓使用者管理和還原已刪除的物件。 使用者現在可以看到已刪除物件的清單，並將它們還原到原始位置或您想要的位置。

如果您打算啟用 Active Directory 資源回收筒中 Windows Server，請考慮下列各項：

- Active Directory 資源回收筒預設為停用。 若要啟用它，您必須先提高 AD DS 或 AD LDS 環境為 Windows Server 2008 R2 或更高的樹系功能等級。 這又需要樹系中的所有網域控制站或裝載 AD LDS 組態集的執行個體的所有伺服器是執行 Windows Server 2008 R2 或更高版本。
- 啟用 Active Directory 資源回收筒的程序是無法還原的。 當您在環境中啟用 Active Directory 資源回收筒之後，就無法將它停用。
- 若要管理資源回收筒功能，透過使用者介面，您必須安裝 Windows Server 2012 中的 Active Directory 管理中心版本。

    > [!NOTE]
    > 您可以使用**伺服器管理員**安裝遠端伺服器管理工具 (RSAT) 來管理資源回收筒使用者介面中使用正確的 Active Directory 管理中心版本。
    >
    > 如需安裝 RSAT 的詳細資訊，請參閱文章[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory 資源回收筒逐步說明

在下列步驟中，您將使用 ADAC 來執行 Windows Server 2012 中的下列的 Active Directory 資源回收筒工作：

- [步驟 1：提高樹系功能等級](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)
- [步驟 2：啟用資源回收筒](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)
- [步驟 3：建立測試使用者、 群組及組織單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)
- [步驟 4：還原已刪除物件](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> 若要執行下列步驟，需要 Enterprise Admins 群組的成員資格或同等權限。

### <a name="bkmk_raise_ffl"></a>步驟 1:提高樹系功能等級

在這個步驟中，您將提高樹系功能等級。 您必須先提高到最小值是 Windows Server 2008 R2，才能啟用 Active Directory 資源回收筒目標樹系功能等級。

#### <a name="to-raise-the-functional-level-on-the-target-forest"></a>提高目標樹系的功能等級

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在左瀏覽窗格中按一下目標網域，然後在 [工作] 窗格中按一下 [提高樹系功能等級]。 選取的樹系功能等級至少 Windows Server 2008 R2 或更高版本，然後按一下 **確定**。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

針對 **-身分識別**引數，指定完整的 DNS 網域名稱。

### <a name="bkmk_enable_recycle_bin"></a>步驟 2:啟用資源回收筒

在這個步驟中，您將啟用資源回收筒，以還原在 AD DS 中刪除的物件。

#### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>在目標網域的 ADAC 中啟用 Active Directory 資源回收筒

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在 [工作] 窗格中，按一下 [工作] 窗格中的 [啟用資源回收筒...]，按一下警告訊息方塊上的 [確定]，再按一下 [確定] 以重新整理 ADAC 訊息。

4. 按 F5 重新整理 ADAC。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>步驟 3:建立測試使用者、群組及組織單位

在下列程序中，您將建立兩個測試使用者。 接著建立一個測試群組，並將測試使用者新增到該群組。 此外，您也將建立組織單位。

#### <a name="to-create-test-users"></a>建立測試使用者

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在 [工作] 窗格中，依序按一下 [新增] 和 [使用者]。

    ![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4. 在 [帳戶] 下方輸入下列資訊，然後按一下 [確定]：

   - 全名：test1
   - 使用者 SamAccountName 登入：test1
   - 密碼： p@ssword1
   - 確認密碼： p@ssword1

5. 重複上述步驟建立第二個使用者 test2。

#### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>建立測試群組，並將使用者新增到該群組。

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。
2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。
3. 在 [工作] 窗格中，依序按一下 [新增] 和 [群組]。
4. 在 [群組] 下方輸入下列資訊，然後按一下 [確定]：

    -   **群組名稱： group1**

5. 按一下 [group1]，然後按一下 [工作] 窗格下方的 [內容]。
6. 依序按一下 [成員] 、[新增] ，輸入 **test1;test2**，然後按一下 [確定] 。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Add-ADGroupMember -Identity group1 -Member test1
```

#### <a name="to-create-an-organizational-unit"></a>建立組織單位

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。
2. 按一下 **管理**，按一下**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**對話方塊，然後按一下 * * 確定
3. 在 [工作]  窗格中，依序按一下 [新增]  和 [組織單位] 。
4. 在 [組織單位] 下方輸入下列資訊，然後按一下 [確定]：

   - **NameOU1**

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>步驟 4:還原刪除的物件

在下列程序中，您要將 [Deleted Objects]  容器中的刪除物件還原到其原始位置和其他位置。

#### <a name="to-restore-deleted-objects-to-their-original-location"></a>將刪除的物件還原到其原始位置

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 選取使用者 **test1** 與 **test2**，按一下 [工作]  窗格中的 [刪除]  ，然後按一下 [是]  確認刪除。

    ![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

    下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

    ```powershell
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4. 瀏覽至 [Deleted Objects]  容器，選取 **test2** 與 **test1** ，然後按一下 [工作]  窗格中的 [還原]  。

5. 為確認物件已還原到其原始位置，瀏覽至目標網域，確認使用者帳戶已經列出。

    > [!NOTE]
    > 如果您瀏覽至使用者帳戶 **test1** 與 **test2** 的 [內容]  並按一下 [成員隸屬] ，您會看到它們的群組成員資格也已還原。

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

#### <a name="to-restore-deleted-objects-to-a-different-location"></a>將刪除的物件還原到其他位置

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 選取使用者 **test1** 與 **test2**，按一下 [工作]  窗格中的 [刪除]  ，然後按一下 [是]  確認刪除。

4. 瀏覽至 [Deleted Objects] 容器，選取 **test2** 與 **test1**，然後按一下 [工作] 窗格中的 [還原到]。

5. 選取 [OU1]，然後按一下 [確定]。

6. 為確認物件已還原到 [OU1] ，瀏覽至目標網域，按兩下 [OU1]  ，確認使用者帳戶已經列出。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>更細緻的密碼原則

Windows Server 2008 作業系統可以讓組織為網域中的不同使用者集合定義不同的密碼和帳戶鎖定原則。 在 Windows Server 2008 以前的 Active Directory 網域中，網域中的所有使用者只能套用一個密碼原則和帳戶鎖定原則。 這些原則是在網域的 "Default Domain Policy" 中指定。 因此，需針對不同使用者集合設定不同密碼和帳戶鎖定設定的組織，就必須建立密碼篩選器或部署多個網域。 這兩種方法都是相當耗費成本的選項。

您可以使用更細緻的密碼原則在單一網域內指定多個密碼原則，然後將密碼和帳戶鎖定原則的不同限制套用到網域中的不同使用者集合。 例如，您可以將較嚴格的設定套用到權限帳戶，然後將較不嚴格的設定套用到其他使用者的帳戶。 在其他狀況下，您可能想為密碼與其他資料來源同步的帳戶套用特殊的密碼原則。 如需更細緻密碼原則的詳細說明，請參閱[AD DS:更細緻的密碼原則](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**新功能？**

Windows Server 2012 和較新且更細緻的密碼原則管理會更容易且更具視覺效果提供 AD DS 系統管理員在 ADAC 中管理它們的使用者介面。 系統管理員現在可以檢視指定的使用者的結果原則、 檢視和排序指定的網域內的所有密碼原則和以視覺化方式管理個別的密碼原則。

如果您打算在 Windows Server 2012 中使用更細緻的密碼原則，請考慮下列各項：

- 更細緻的密碼原則只會套用到全域安全性群組和使用者物件 (或 inetOrgPerson 物件，如果使用它們代替使用者物件的話)。 根據預設，只有 Domain Admins 群組的成員可以設定更細緻的密碼原則。 不過，您也可以將設定這些原則的能力委派給其他使用者。 網域功能等級必須是 Windows Server 2008 或更高。

- 您必須使用 Windows Server 2012 或較新版本的 Active Directory 管理中心來管理更細緻的密碼原則，透過圖形化使用者介面。

    > [!NOTE]
    > 您可以使用**伺服器管理員**安裝遠端伺服器管理工具 (RSAT) 來管理資源回收筒使用者介面中使用正確的 Active Directory 管理中心版本。
    >
    > 如需安裝 RSAT 的詳細資訊，請參閱文章[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

### <a name="fine-grained-password-policy-step-by-step"></a>更細緻的密碼原則逐步說明

在下列步驟中，您將使用 ADAC 來執行下列更細緻的密碼原則工作：

- [步驟 1：提高網域功能等級](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)
- [步驟 2：建立測試使用者、 群組及組織單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)
- [步驟 3：建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)
- [步驟 4：檢視使用者的原則結果組](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)
- [步驟 5：編輯更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)
- [步驟 6：刪除更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> 若要執行下列步驟，需要 Domain Admins 群組的成員資格或同等權限。

#### <a name="bkmk_raise_dfl"></a>步驟 1:提高網域功能等級

在下列程序中，您會將提高網域功能等級為 Windows Server 2008 或更高的目標網域。 Windows Server 2008 或更高版本的網域功能等級，才能啟用更細緻的密碼原則。

##### <a name="to-raise-the-domain-functional-level"></a>提高網域功能等級

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在左瀏覽窗格中按一下目標網域，然後在 [工作] 窗格中按一下 [提高網域功能等級]。 選取的樹系功能等級至少 Windows Server 2008 或更新版本，然後按一下**確定**。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>步驟 2:建立測試使用者、群組及組織單位

若要建立的測試使用者和群組需要此步驟，請依照下列程序：[步驟 3：建立測試使用者、 群組及組織單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)（您不需要建立組織單位，可以示範更細緻的密碼原則）。

#### <a name="bkmk_create_fgpp"></a>步驟 3:建立更細緻的新密碼原則

在下列程序中，您將使用 ADAC 的 UI 來建立更細緻的密碼原則。

##### <a name="to-create-a-new-fine-grained-password-policy"></a>建立新的更細緻的密碼原則

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在 ADAC [瀏覽窗格] 中，開啟 [System] 容器，然後按一下 [Password Settings Container]。

4. 在 [工作] 窗格中，依序按一下 [新增] 和 [密碼設定]。

    填入或編輯內容頁面的欄位，以建立新的 [密碼設定]  物件。 [名稱]  和 [優先順序]  欄位是必要欄位。

    ![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5. 底下**直接套用至**，按一下**新增**，型別**group1**，然後按一下 **確定**。

    這樣會將密碼原則物件與您為測試環境建立的全域群組成員關聯。

6. 按一下 [確定]  以提交建立。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>步驟 4:檢視使用者的原則結果組

在下列程序中，您將檢視使用者的指派更細緻的密碼原則中的群組成員的結果密碼設定[步驟 3:建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

##### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>檢視使用者的原則結果組

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 選取使用者時， **test1**屬於群組**group1**相關聯中更細緻的密碼原則[步驟 3:建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

4. 按一下 [工作] 窗格中的 [檢視結果原則設定]。

5. 檢查密碼設定原則，然後按一下 [取消] 。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>步驟 5:編輯更細緻的密碼原則

在下列程序中，您將編輯您在中建立的更細緻的密碼原則[步驟 3:建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

##### <a name="to-edit-a-fine-grained-password-policy"></a>編輯更細緻的密碼原則

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在 ADAC [瀏覽窗格] 中，展開 [System]，然後按一下 [Password Settings Container]。

4. 選取您在中建立的更細緻的密碼原則[步驟 3:建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)，按一下 **屬性**中**工作**窗格。

5. 在 [強制密碼歷程記錄]下方，將 [記住的密碼數目] 值變更為 **30**。

6. 按一下 [確定] 。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>步驟 6:刪除更細緻的密碼原則

##### <a name="to-delete-a-fine-grained-password-policy"></a>刪除更細緻的密碼原則

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 在 ADAC [瀏覽窗格] 中，展開 [System]  ，然後按一下 [Password Settings Container] 。

4. 選取您在中建立的更細緻的密碼原則[步驟 3:建立新的更細緻的密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)然後在**任務**窗格中，按一下**屬性**。

5. 清除 [保護以防止被意外刪除] 核取方塊，然後按一下 [確定]。

6. 選取更細緻的密碼原則，然後在 [工作] 窗格按一下 [刪除]。

7. 按一下確認對話方塊中的 [確定]。

![AD 系統管理中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 對等命令 * * *

下列 Windows PowerShell Cmdlet 執行與前述程序相同的功能。 在單一行中，輸入各個 Cmdlet (即使因為格式限制，它們可能會在這裡出現自動換行成數行)。

```powershell
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Windows PowerShell 歷程記錄檢視器

ADAC 是建立在 Windows PowerShell 之上的使用者介面工具。 Windows Server 2012 及更新版本，IT 系統管理員可以在 adac 中使用 Windows PowerShell 歷程記錄檢視器，了解 Windows PowerShell 的 Active Directory cmdlet。 當使用者在使用者介面執行動作時，可以在 Windows PowerShell 歷程記錄檢視器中看到對等的 Windows PowerShell 命令。 這樣能讓系統管理員建立自動化的指令碼並減少重複性的工作，因此可以提升 IT 產能。 此外，這項功能可減少了解 Windows PowerShell 的 Active Directory 的時間，並增加正確性的自動化指令碼的使用者的信心。

使用 Windows PowerShell 歷程記錄檢視器，在 Windows Server 2012 或更新版本時請考慮下列各項：

- 若要使用 Windows PowerShell 指令碼檢視器，您必須使用 Windows Server 2012 或較新版本的 ADAC

    > [!NOTE]
    > 您可以使用**伺服器管理員**安裝遠端伺服器管理工具 (RSAT) 來管理資源回收筒使用者介面中使用正確的 Active Directory 管理中心版本。
    >
    > 如需安裝 RSAT 的詳細資訊，請參閱文章[遠端伺服器管理工具](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools)。

- 了解基本的 Windows PowerShell。 例如，您必須了解 Windows PowerShell 中的管線輸送如何運作。 如需 Windows PowerShell 中管線輸送的詳細資訊，請參閱 [Windows PowerShell 中的管線輸送與管線](https://technet.microsoft.com/library/ee176927.aspx)。

### <a name="windows-powershell-history-viewer-step-by-step"></a>Windows PowerShell 歷程記錄檢視器逐步說明

在下列程序中，您將在 ADAC 中使用 Windows PowerShell 歷程記錄檢視器來建構 Windows PowerShell 指令碼。  開始這個程序之前，請從 **group1** 群組中移除使用者 **test1**。

#### <a name="to-construct-a-script-using-powershell-history-viewer"></a>使用 PowerShell 歷程記錄檢視器建構指令碼

1. 以滑鼠右鍵按一下 [Windows PowerShell] 圖示，再按一下**系統管理員身分執行**並輸入**dsac.exe**以開啟 ADAC。

2. 依序按一下 [管理] 、[新增瀏覽節點]  ，在 [新增瀏覽節點]  對話方塊中選取適當的目標網域，然後按一下 [確定] 。

3. 展開 ADAC 畫面下方的 [Windows PowerShell 歷程記錄]  窗格。

4. 選取使用者 **test1**。

5. 按一下 **加入群組...** 中**工作**窗格。

6. 瀏覽至 [group1]，按一下對話方塊中的 [確定]。

7. 瀏覽至 [Windows PowerShell 歷程記錄]  窗格，尋找剛才產生的命令。

8. 複製命令並貼到您使用的編輯器，以建構您的指令碼。

    例如，您可以修改命令以將其他使用者新增到 **group1**，或將 **test1** 新增到其他群組。

## <a name="see-also"></a>另請參閱

[進階 AD DS 管理使用 Active Directory Administrative Center&#40;技術等級 200&#41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)
