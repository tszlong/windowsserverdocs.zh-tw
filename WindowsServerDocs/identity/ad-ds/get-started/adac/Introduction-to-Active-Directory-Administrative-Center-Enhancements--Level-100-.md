---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: "Active Directory 系統管理員中心調節 (層級 100) 簡介"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Active Directory 系統管理員中心調節 (層級 100) 簡介

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Windows Server 2012 中的 ADAC 包含管理下列功能：

-   [Active Directory 資源回收筒](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [微調密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Windows PowerShell 歷史檢視器](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Active Directory 資源回收筒
Active Directory 物件意外刪除是常見使用者的 Active Directory Domain Services (AD DS) 和 Active Directory 輕量型 Directory Services (AD LDS)。 在過去版本的 Windows Server、 Windows Server 2008 R2 前一個復原意外刪除 Active Directory 物件，但方案他們缺點。

在 Windows Server 2008，您可以使用 Windows Server 備份的功能和**ntdsutil**來標記授權還原命令物件可靠，以確保您的整個網域複寫還原的資料。 授權方案缺點是原先執行中 Directory 服務還原模式 (DSRM)。 期間 DSRM，必須維持離線網域控制站還原。 因此，不能服務 client 要求。

在 Windows Server 2003 Active Directory 和 Windows Server 2008 AD DS，您無法復原透過重新引發標記刪除 Active Directory 物件。 不過，重新引發物件的連結值 （例如群組成員資格帳號） 的屬性移除了實體和清除的非連結值屬性，所以無法復原。 因此，系統管理員可能會依賴重新引發標記為意外刪除物件方案旗艦版。 如需重新引發標記的詳細資訊，請查看[重新引發 Active Directory 標記物件](https://go.microsoft.com/fwlink/?LinkID=125452)。

Active Directory 資源回收筒]，開始在 Windows Server 2008 R2 上的現有標記重新引發基礎結構及美化您保留和復原意外刪除 Active Directory 物件的能力。

當您讓 Active Directory 資源回收筒] 所有連結值非連結值刪除的 Active Directory 物件的屬性會保留並物件會還原為相同的邏輯一致狀態將它們之前刪除完整。 例如，還原的帳號會自動重新取得所有群組成員資格和立即之前刪除中和跨網域對應存取權限。 Active Directory 資源回收桶適用於廣告 LDS 和 AD DS 環境。 為 Active Directory 資源回收筒的詳細描述，請查看[AD DS 中的新功能： Active Directory 資源回收筒]](https://technet.microsoft.com/library/dd391916(WS.10).aspx)。

**新功能？** 在 Windows Server 2012，已使用新的圖形使用者介面使用者管理和還原刪除的物件改良 Active Directory 資源回收筒的功能。 使用者可以現在看起來找出刪除物件的清單，並將它們還原至其原始或想要的位置。

如果想要讓 Active Directory 資源回收桶 Windows Server 2012 中，請考慮下列動作：

-   根據預設，Active Directory 資源回收筒 」 已停用。 要讓它，必須先提高 AD DS 或廣告 LDS 環境 Windows Server 2008 R2 或更高的樹系功能層級。 這會需要森林中的所有網域控制站或主機的廣告 LDS 組態集執行個體的所有伺服器會執行 Windows Server 2008 R2 或更高版本。

-   程序的 Active Directory 資源回收筒的就是無法還原。 您可以在您的環境中的 Active Directory 資源回收筒] 之後，您無法停用。

-   若要管理透過使用者介面的資源回收筒 」 功能，您必須安裝版本的 Active Directory 管理中心 Windows Server 2012 中。

    > [!NOTE]
    > 您可以使用**伺服器管理員**在 [資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的 Windows Server 2012 電腦上安裝遠端伺服器管理工具 (RSAT)。
    > 
    > 您可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) windows&reg; 8 資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的電腦。

### <a name="active-directory-recycle-bin-step-by-step"></a>Active Directory 資源回收桶逐步
在下列步驟，您將會使用 ADAC Windows Server 2012 中執行下列 Active Directory 資源回收桶工作：

-   [步驟 1： 提高森林功能等級](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [步驟 2： 讓資源回收筒](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [步驟 3： 建立測試使用者、 群組和單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [步驟 4： 還原刪除物件](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> 成員資格群組企業系統管理員或相當權限，才能執行下列步驟。

### <a name="bkmk_raise_ffl"></a>步驟 1： 提高森林功能等級
在此步驟，您將會提升樹系正常運作。 您必須先提高目標樹系之前，您可以 Active Directory 資源回收桶，至少會 Windows Server 2008 R2 上的功能層級。

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>以提升上目標樹系功能

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  按一下目標網域在左側的瀏覽窗格中，在**工作**窗格中，按**提高森林功能等級**。 選取至少森林功能層級 Windows Server 2008 R2 或更高版本，然後按一下**[確定]**。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

適用於**層的身分**引數指定 DNS 的完整的名稱。

### <a name="bkmk_enable_recycle_bin"></a>步驟 2： 讓資源回收筒
在此步驟，您將會讓資源回收筒中 AD DS 還原刪除物件。

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>若要讓 Active Directory 資源回收桶 ADAC 目標網域中

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在**工作**窗格中，按**可讓資源回收筒...**在**工作**窗格中，按一下**[確定]**上警告訊息方塊，然後按一下 [ **[確定]**來重新整理 ADAC 訊息。

4.  按下 F5 來重新整理 ADAC。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>步驟 3： 建立測試使用者、 群組和單位
下列程序，您將會建立兩個測試的使用者。 然後，您將建立測試群組並測試使用者新增到群組。 此外，您將會建立組織單位。

##### <a name="to-create-test-users"></a>若要建立的使用者測試

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在**工作**窗格中，按一下 [**新增]** ，然後按一下 [**使用者**。

    ![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  輸入下列資訊在**Account** ，然後按一下 [確定]:

    -   完整名稱： test1

    -   使用者 SamAccountName 登入： test1

    -   密碼：p@ssword1

    -   確認密碼：p@ssword1

5.  重複上一個步驟建立第二個使用者 test2。

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>若要建立測試群組，並將使用者新增到群組

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在**工作**窗格中，按一下 [**新增]** ，然後按一下 [**群組**。

4.  輸入下列資訊在**群組**，然後按一下 [ **[確定]**:

    -   **群組的名稱︰ 群組 1**

5.  按一下**群組 1**，然後在**工作**] 窗格中，按一下 [**屬性**。

6.  按一下**成員**，按一下 [**新增**，輸入**test1; test2**，然後按一下 [ **[確定]**。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>若要建立組織單位

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在**工作**窗格中，按一下 [**新增]** ，然後按一下 [**組織單位**。

4.  輸入下列資訊在**組織單位**，然後按一下 [ **[確定]**:

    -   **NameOU1**

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>步驟 4： 還原刪除物件
下列程序，您將會還原刪除的物件從**刪除物件**容器到其原始位置，以不同的位置。

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>若要還原刪除到其原始位置的物件

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  選取 [使用者] **test1**和**test2**，按一下 [ **Delete**在**工作**窗格中，然後按一下 [**是**確認刪除。

    ![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

    下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  瀏覽至**刪除物件**容器中，選取**test2**並**test1** ，然後按一下**還原**中**工作**窗格。

5.  若要確認物件還原到其原始位置，瀏覽至目標網域，確認帳號列出。

    > [!NOTE]
    > 如果您瀏覽至**屬性**的帳號**test1**和**test2** ，然後按一下**成員隸屬**，您將會看到他們群組成員資格也還原。

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>若要還原刪除物件不同的位置

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  選取 [使用者] **test1**和**test2**，按一下 [ **Delete**在**工作**窗格中，然後按一下 [**是**確認刪除。

4.  瀏覽至**刪除物件**容器中，選取**test2**並**test1** ，然後按一下**還原到**中**工作**窗格。

5.  選取 [ **OU1** ，然後按一下 [ **[確定]**。

6.  若要確認物件還原到**OU1**，瀏覽至目標網域，按兩下 [ **OU1** ，並確認帳號會列出。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>微調密碼原則
Windows Server 2008 作業系統提供一種方法來定義不同的密碼和 account 鎖定原則的不同設定的使用者網域中的組織。 在 Windows Server 2008 之前 Active Directory 網域中，只有一個密碼原則和 account 鎖定原則可套用至網域中的所有使用者。 這些原則指定中網域預設網域原則。 如此一來，希望不同的使用者的設定的不同的密碼和 account 鎖定設定的組織必須建立密碼篩選或部署多個網域。 兩者都高的選項。

您可以指定單一網域中的多個密碼原則，並將不同的限制的密碼，以及 account 鎖定原則套用到不同設定的網域中的使用者使用細緻密碼原則。 例如您可以將有特殊權限帳號更嚴格設定和較嚴格設定套用到帳號，其他使用者。 有時候，您可能想要套用的特殊密碼原則帳號密碼同步處理的其他資料來源。 如 Fine-Grained 密碼原則的詳細描述， [AD DS: Fine-Grained 密碼原則](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**新功能？** 在 Windows Server 2012，精細密碼原則管理是由變得更容易及更多視覺化藉由 AD DS ADAC 中管理他們的系統管理員的使用者介面。 系統管理員現在可以檢視原則特定的使用者的結果、 檢視與排序指定網域中的所有密碼原則和視覺管理個人密碼原則。

如果想要在 Windows Server 2012 中使用細緻密碼原則，請考慮下列動作：

-   細緻密碼原則套用僅限安全性的全域群組和使用者物件 （或需要物件如果他們使用使用者物件而）。 根據預設，僅限群組成員的網域系統管理員可以設定細緻密碼原則。 不過，您也可以委派這些原則設定的其他使用者的功能。 網域功能等級必須 Windows Server 2008，或更高版本。

-   您必須使用 Windows Server 2012 版本的 Active Directory 管理中心管理細緻密碼原則圖形使用者介面透過。

    > [!NOTE]
    > 您可以使用**伺服器管理員**在 [資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的 Windows Server 2012 電腦上安裝遠端伺服器管理工具 (RSAT)。
    > 
    > 您可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) windows&reg; 8 資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的電腦。

### <a name="fine-grained-password-policy-step-by-step"></a>逐步精細密碼原則
在下列步驟，您將使用 ADAC，執行下列精細密碼原則工作：

-   [步驟 1： 提升網域功能](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [步驟 2： 建立測試使用者、 群組，以及單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [步驟 4： 檢視結果原則使用者的設定](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [步驟 5： 編輯精細密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [步驟 6: Delete 精細密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> 成員資格群組網域系統管理員或相當權限，才能執行下列步驟。

#### <a name="bkmk_raise_dfl"></a>步驟 1： 提升網域功能
下列程序，您將會提升網域正常運作的目標網域到 Windows Server 2008，或更高版本。 網域功能層級的 Windows Server 2008，或更高版本，才能讓細緻密碼原則。

###### <a name="to-raise-the-domain-functional-level"></a>以提升網域功能

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  按一下目標網域在左側的瀏覽窗格中，在**工作**窗格中，按**提高網域功能等級**。 選取至少森林功能層級 Windows Server 2008，或更高版本，然後按一下**[確定]**。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>步驟 2： 建立測試使用者、 群組，以及單位
若要建立測試使用者和群組需要此步驟，請依照下列程序，位置如下：[執行 「 步驟 3： 建立測試使用者、 群組和單位](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)（您不需要建立組織單位，示範精細密碼原則）。

#### <a name="bkmk_create_fgpp"></a>步驟 3： 建立新的細緻密碼原則
下列程序，您將會建立新的細微密碼原則 ADAC 使用 UI。

###### <a name="to-create-a-new-fine-grained-password-policy"></a>若要建立新的正常地細微的密碼原則

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  ADAC 瀏覽窗格中，請打開**系統**容器，然後按**的密碼設定容器**。

4.  在**工作**窗格中，按**新**，，然後按一下 [**設定密碼**。

    填入或編輯欄位中屬性頁面，以建立新的**設定密碼**物件。 **名稱**和**優先順序**所需的欄位。

    ![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  在**直接適用於**，按一下 [**新增**，輸入**群組 1**，然後按一下 [ **[確定]**。

    這將與您的測試環境建立的全域群組成員關聯密碼原則物件。

6.  按一下**[確定]**來提交建立。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>步驟 4： 檢視結果原則使用者的設定
下列程序，您將會檢視為使用者指派給正常地細微的密碼原則中的群組成員結果密碼設定[執行 「 步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>若要檢視結果原則使用者的設定

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  選取一位使用者， **test1**群組中，所屬的**群組 1**相關聯的細緻密碼原則中[執行 「 步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)。

4.  按一下**檢視結果密碼設定**在**工作**窗格。

5.  檢查 [密碼原則設定，然後按一下**取消**。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>步驟 5： 編輯精細密碼原則
下列程序，您將會編輯正常地細微的密碼原則您建立[執行 「 步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>若要編輯精細密碼原則

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在 [ADAC**瀏覽窗格中**，展開 [**系統**，然後按一下 [**密碼設定容器**。

4.  選取 [建立您的正常地細微的密碼原則[執行 「 步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)，按一下 [**屬性**中**工作**窗格。

5.  在**執行密碼歷史**，變更的值**記住密碼的數字**以**30**。

6.  按一下**[確定]**。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>步驟 6: Delete 精細密碼原則

###### <a name="to-delete-a-fine-grained-password-policy"></a>若要 delete 精細密碼原則

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  在 [ADAC 瀏覽窗格中，展開**系統**，然後按一下 [**的密碼設定容器**。

4.  選取 [建立您的正常地細微的密碼原則[執行 「 步驟 3： 建立新的細緻密碼原則](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)在**工作**窗格中按一下 [**屬性**。

5.  清除**以防止誤刪除保護**核取方塊，按一下 [ **[確定]**。

6.  選取 [細緻細微的密碼原則，在**工作**窗格中按一下 [ **Delete**。

7.  按一下**[確定]**在 [確認] 對話方塊。

![廣告系統管理員中心簡介](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell 相當於命令 * * *

下列 Windows PowerShell cmdlet 執行上述程序相同的功能。 輸入每個 cmdlet 上一行，，即使它們可能會出現換透過以下幾個行因為格式設定的限制。

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Windows PowerShell 歷史檢視器
ADAC 是建置 Windows PowerShell 上方的使用者介面工具。  在 Windows Server 2012 時，IT 系統管理員可以利用 ADAC 學習 Windows PowerShell cmdlet Active Directory 使用 Windows PowerShell 歷史檢視器。 在使用者介面中執行的動作，相同的 Windows PowerShell 命令會顯示在 Windows PowerShell 歷史檢視器中的使用者。 這可讓建立自動化的指令碼，並減少重複的工作，因此增加生產力 IT 系統管理員。  這項功能降低 Active Directory 了 Windows PowerShell 的時間，，他們自動化的指令碼的正確性增加使用者的信心。

Windows Server 2012 中使用 Windows PowerShell 歷史檢視器考慮下列方法：

-   若要使用 Windows PowerShell 指令碼檢視器，您必須使用 ADAC 的 Windows Server 2012 版本

    > [!NOTE]
    > 您可以使用**伺服器管理員**在 [資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的 Windows Server 2012 電腦上安裝遠端伺服器管理工具 (RSAT)。
    > 
    > 您可以使用[RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) windows&reg; 8 資源回收筒] 管理透過使用者介面中使用 Active Directory 管理中心的正確版本的電腦。

-   有一些基本的 Windows PowerShell 知識。 例如，您需要知道傳送 Windows PowerShell 中的運作方式。 如需關於 Windows PowerShell 中的 [傳送的資訊，請查看[傳送及 Windows PowerShell 中的管線](https://technet.microsoft.com/library/ee176927.aspx)。

### <a name="windows-powershell-history-viewer-step-by-step"></a>Windows PowerShell 歷史檢視器逐步
在下列程序，您將使用 Windows PowerShell 歷史檢視器中 ADAC 建構 Windows PowerShell 指令碼。  此程序開始之前，請移除使用者] **test1**群組中，**群組 1**。

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>建立指令碼使用 PowerShell 歷史檢視器

1.  Windows PowerShell 圖示上按一下滑鼠右鍵，請按一下**系統管理員身分執行**並輸入**dsac.exe**打開 ADAC。

2.  按一下**管理**，按一下 [**新增瀏覽節點**，然後選取適當的目標網域中**新增瀏覽節點**] 對話方塊中，然後按一下**[確定]**。

3.  展開**Windows PowerShell 歷史**窗格中 ADAC 畫面的底部。

4.  選取使用者] **test1**。

5.  按一下**群組新增...**的**工作**窗格。

6.  瀏覽至**群組 1** ，按一下 [ **[確定]**對話方塊中。

7.  瀏覽至**Windows PowerShell 歷史**窗格中，並找出剛剛產生的命令。

8.  複製命令，並將它貼到您想要建立指令碼編輯器。

    例如，您可以修改新增到不同的使用者命令**群組 1**，或新增**test1**到不同群組。

## <a name="see-also"></a>也了
[使用 Active Directory 系統管理員中心和 #40; 進階的 AD DS 管理層級 200 和 #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


