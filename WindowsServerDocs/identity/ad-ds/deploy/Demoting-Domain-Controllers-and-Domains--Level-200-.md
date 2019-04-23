---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: 將網域控制站和網域降級 (等級 200)
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 11/14/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9b3db1390e8191451ef270ce29a2a37463b1de3c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869769"
---
# <a name="demoting-domain-controllers-and-domains"></a>降級網域控制站和網域

>適用於：Windows Server

這個主題說明如何使用伺服器管理員或 Windows PowerShell 移除 AD DS。
  
## <a name="ad-ds-removal-workflow"></a>AD DS 移除工作流程

![AD DS 移除工作流程圖表](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  

> [!CAUTION]
> 不支援在升級為網域控制站之後使用 Dism.exe 或 Windows PowerShell DISM 模組移除 AD DS 角色，這樣做將會導致伺服器無法正常開機。
>
> 不同於伺服器管理員或 Windows PowerShell 的 ADDSDeployment 模組，DISM 是原生的服務系統，並未繼承 AD DS 的舊有知識或其組態。 除非伺服器不再是網域控制站，否則請勿使用 Dism.exe 或 Windows PowerShell DISM 模組來解除安裝 AD DS 角色。

## <a name="demotion-and-role-removal-with-powershell"></a>使用 PowerShell 降級和角色移除

|||  
|-|-|  
|**ADDSDeployment 和 ServerManager Cmdlet**|引數 (**粗體**的引數是必要的。 *斜體*的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-Confirm<br /><br />***-Credential***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-Force<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|Uninstall-WindowsFeature/Remove-WindowsFeature|***-Name***<br /><br />***-IncludeManagementTools***<br /><br />*-Restart*<br /><br />-Remove<br /><br />-Force<br /><br />-ComputerName<br /><br />-Credential<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> 只有在您尚未登入為 Enterprise Admins 群組 (降級網域中最後一個 DC) 或 Domain Admins 群組 (降級複本 DC) 的成員時才需要 **-credential** 引數。只有在您想要移除所有 AD DS 管理公用程式時，才需要 **-Includemanagementtools** 引數。  
  
## <a name="demote"></a>降級  
  
### <a name="remove-roles-and-features"></a>移除角色和功能

伺服器管理員提供兩個介面用於移除 Active Directory 網域服務角色：  
  
* 主要儀表板上的 [管理]  功能表 (使用 [移除角色及功能]   

   ![伺服器管理員-移除角色及功能](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  

* 按一下瀏覽窗格上的 [AD DS] 或 [所有伺服器]。 向下捲動到 [角色和功能] 區段。 以滑鼠右鍵按一下 [角色和功能] 清單中的 [Active Directory 網域服務]，然後按一下 [移除角色或功能]。 這個介面會略過 [伺服器選取項目] 頁面。  

   ![伺服器管理員-所有的伺服器移除角色及功能](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  

ServerManager cmdlet **Uninstall-windowsfeature**並**Remove-windowsfeature**會防止您移除 AD DS 角色，直到您降級網域控制站。
  
### <a name="server-selection"></a>伺服器選項

![移除角色及功能精靈選取目的地伺服器](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  

[伺服器選取項目] 對話方塊可讓您從先前已加入集區的伺服器中選擇其中之一 (只要其可供存取)。 執行伺服器管理員的本機伺服器總是可供使用。  

### <a name="server-roles-and-features"></a>伺服器角色與功能

![移除角色及功能精靈-選取要移除的角色](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  

取消選取 [Active Directory 網域服務] 核取方塊來將網域控制站降級。如果伺服器目前是網域控制站，這不會移除 AD DS 角色，而是會切換成提供降級功能的 [驗證結果] 對話方塊。 否則，它只會像任何其他角色功能一樣，只移除二進位檔。  

* 如果您想要立即再次升級網域控制站，請勿移除任何其他的 AD DS 相關角色或功能 (例如 DNS、GPMC 或 RSAT 工具)。 移除其他角色與功能會增加重新升級的時間，因為伺服器管理員會在您重新安裝角色時重新安裝這些功能。  
* 如果您想要永久降級網域控制站，請自行選擇移除不必要的 AD DS 角色與功能。 這需要取消選取那些角色與功能的核取方塊。  

   AD DS 相關角色與功能的完整清單包括：  
  
   * Windows PowerShell 的 Active Directory 模組功能  
   * AD DS 與 AD LDS 工具功能  
   * Active Directory 管理中心功能  
   * AD DS 嵌入式管理單元及命令列工具功能  
   * DNS 伺服器  
   * 群組原則管理主控台  
  
相等的 ADDSDeployment 和 ServerManager Windows PowerShell Cmdlet 為：  
  
```
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
```

![移除角色及功能精靈的確認對話方塊](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  

![移除角色及功能精靈-驗證](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  

### <a name="credentials"></a>認證

![Active Directory 網域服務設定精靈-認證的選取項目](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  

您可以在 [認證]  頁面上設定降級選項。 提供執行下列清單降級所需的認證：  

* 降級其他網域控制站需要 Domain Admin 認證。 選取**強制此網域控制站移除**會降級網域控制站，但不從 Active Directory 移除網域控制站物件的中繼資料。  

   > [!WARNING]  
   > 請不要選取這個選項，除非網域控制站無法連絡其他網域控制站，而且沒有其他正當的方法可以解決這個網路問題。 強制降級會在樹系的剩餘網域控制站上的 Active Directory 中留下孤立的中繼資料。 不僅如此，該網域控制站上所有未複寫的變更 (如密碼或新的使用者帳戶) 都會永遠遺失。 孤立的中繼資料是 Microsoft 客戶支援遇到大部分 AD DS、Exchange、SQL 及其他軟體問題的根本原因。  
   >
   > 如果您強制降級網域控制站，則必須立即手動清理中繼資料。 如需相關步驟，請參閱 [清理伺服器中繼資料](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  

   ![Active Directory 網域服務設定精靈-認證強制移除](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
* 降級網域中最後一個網域控制站需要 Enterprise Admins 群組成員資格，因為它會移除網域本身 (如果這是樹系的最後一個網域，則會移除樹系)。 如果您目前的網域控制站是網域的最後一部網域控制站，[伺服器管理員] 將會通知您。 選取 [網域中最後一個網域控制站] 核取方塊來確認網域控制站是網域中的最後一個網域控制站。  

對等的 ADDSDeployment Windows PowerShell 引數為：  

```
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
```

### <a name="warnings"></a>警告

![Active Directory 網域服務設定精靈認證 FSMO 角色影響](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  

[警告]  頁面會提示您移除此網域控制站之後可能發生的結果。 若要繼續，您必須選取 [繼續移除] 。

> [!WARNING]  
> 如果您先前在 [認證]  頁面上選取 [強制此網域控制站移除]  ，[警告]  頁面就會顯示此網域控制站代管的所有彈性單一主機操作角色。 您 *必須* 在將此伺服器降級之後 *立即* 從另一個網域控制站拿取角色。 如需拿取 FSMO 角色的詳細資訊，請參閱 [拿取操作主機角色](https://technet.microsoft.com/library/cc816779(WS.10).aspx)。

此頁面沒有相等的 ADDSDeployment Windows PowerShell 引數。

### <a name="removal-options"></a>移除選項

![Active Directory 網域服務設定精靈-認證移除 DNS 和應用程式磁碟分割](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  

[移除選項] 頁面會根據先前在 [認證] 頁面上選取 [網域中最後一個網域控制站] 而顯示。 此頁面可讓您設定其他移除選項。 選取 [忽略區域中最後一部 DNS 伺服器] ，[移除應用程式分割] ，以及 [移除 DNS 委派]  來顯示 [下一步]  按鈕。

選項只有在此網域控制站適用時才會顯示。 例如，如果沒有此伺服器的 DNS 委派，就不會顯示該核取方塊。

按一下 [變更] 來指定替代的 DNS 系統管理認證。 按一下 [檢視分割] 來檢視降級時精靈會移除的其他分割。 根據預設，其他分割只有網域 DNS 與樹系 DNS 區域。 所有其他分割都是非 Windows 分割。

相等的 ADDSDeployment Cmdlet 引數為：  

```
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```

### <a name="new-administrator-password"></a>新系統管理員密碼

![Active Directory 網域服務設定精靈認證新的系統管理員密碼](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  

**新的系統管理員密碼**頁面會要求您提供的內建的本機電腦的系統管理員帳戶的密碼，一旦降級完成且電腦成為網域成員伺服器或工作群組電腦。

如果未指定， **Uninstall-ADDSDomainController** Cmdlet 與引數會遵循與伺服器管理員相同的預設值。

**LocalAdministratorPassword** 是特殊的引數：

* 如果 *未指定* 為引數，Cmdlet 就會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。
* 如果指定 *值*，則此值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以手動提示輸入密碼使用**Read-host** cmdlet 來提示使用者輸入安全字串。

```
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> 如先前的兩個選項不會確認密碼，請小心： 看不到密碼。

您也可以提供轉換的純文字變數當做安全字串，不過我們不鼓勵這種做法。 例如: 

```
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)
```

> [!WARNING]
> 不建議提供或儲存純文字密碼。 在指令碼中執行此命令或嚴密監視您的任何人，都會知道該電腦的本機系統管理員密碼。 取得該資訊之後，他們就可以存取所有資料，而且可以模擬伺服器本身。

### <a name="confirmation"></a>確認

![Active Directory 網域服務設定精靈檢閱選項](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)

[確認]  頁面會顯示計劃好的降級作業，但不會列出降級組態選項。 這是開始降級之前精靈所顯示的最後一頁。 [檢視指令碼] 按鈕會建立 Windows PowerShell 降級指令碼。

按一下 [降級] 以執行以下 AD DS 部署 Cmdlet：

```
Uninstall-DomainController
```

搭配使用選擇性 **Whatif** 引數與 **Uninstall-ADDSDomainController** 及 Cmdlet 來檢閱組態資訊。 這可讓您看到明確和隱含的 Cmdlet 引數值。

例如: 

![PowerShell Uninstall-addsdomaincontroller 範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)

使用 ADDSDeployment Windows PowerShell 時，重新啟動提示是您取消此作業的最後機會。 若要覆寫該提示，請使用 **-force** 或 **confirm:$false** 引數。  

### <a name="demotion"></a>降級

![Active Directory 網域服務設定精靈-進行中的降級](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  

在 [降級] 頁面顯示時，就會開始網域控制站組態設定，且無法暫停或取消。 詳細的作業會在此頁面上顯示並寫入記錄檔：  

* %systemroot%\debug\dcpromo.log
* %systemroot%\debug\dcpromoui.log

因為 **Uninstall-AddsDomainController** 和 **Uninstall-WindowsFeature** 只有一個動作，因此它們會在確認階段中在這裡顯示，且僅提供最低需求的引數。 按 ENTER 鍵會啟動不可撤銷的降級程序並重新啟動電腦。

![PowerShell Uninstall-addsdomaincontroller 範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)

![PowerShell 解除安裝 WindowsFeature 範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)

若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion： $false** 引數。

> [!WARNING]
> 建議您不要覆寫重新開機設定。 成員伺服器必須重新開機才能正確運作。

![PowerShell Uninstall-addsdomaincontroller 強制範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)

這裡是使用最低需求的 **-forceremoval** 與 **-demoteoperationmasterrole**引數強制降級的範例。 不需要 **-credential** 引數，因為使用者是以 Enterprise Admins 群組成員身分登入的：

![PowerShell Uninstall-addsdomaincontroller 範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)

這裡是使用最低需求的 **-lastdomaincontrollerindomain** 與 **-removeapplicationpartitions**引數從網域中移除最後一個網域控制站的範例：

![PowerShell Uninstall-addsdomaincontroller-LastDomainControllerInDomain 範例](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)

如果您嘗試要降級伺服器之前移除 AD DS 角色時，Windows PowerShell 會封鎖您發生錯誤：

![解除安裝先決條件步驟失敗移除的 AD 網域服務，並解除安裝無法繼續。 1. 網域控制站必須 Active DirectoryDomain 服務角色可以解除安裝前被降級。](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)

> [!IMPORTANT]
> 您必須在降級伺服器之後重新啟動電腦，才能移除 AD 網域服務角色二進位檔。

### <a name="results"></a>結果

![您即將要登出之後移除 AD DS 的警告](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)

[結果]  頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 網域控制站會在 10 秒後自動重新開機。
