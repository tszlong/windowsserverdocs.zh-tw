---
ms.assetid: 65ed5956-6140-4e06-8d99-8771553637d1
title: "降級網域控制站和網域 (層級 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c254a01da5534c1ddc673bc1e60382c166ddeda7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="demoting-domain-controllers-and-domains-level-200"></a>降級網域控制站和網域 (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何 AD DS，使用伺服器管理員及 Windows PowerShell 中移除。  
  
-   [AD DS 移除工作流程](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Workflow)  
  
-   [降級及角色移除 Windows PowerShell](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_PS)  
  
-   [降級](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md#BKMK_Demote)  
  
## <a name="BKMK_Workflow"></a>AD DS 移除工作流程  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/adds_demotedomainforest.png)  
  
> [!CAUTION]  
> 升級為網域控制站不支援後將會防止伺服器通常會開機，請移除 Dism.exe 或 Windows PowerShell DISM 模組 AD DS 角色。  
>   
> 伺服器管理員與或不同的 Windows PowerShell 模組 ADDSDeployment，DISM 是原生維護系統有既有不知道 AD DS 或其設定。 請勿使用 Dism.exe 或 Windows PowerShell DISM 模組除非伺服器不再網域控制站解除安裝 AD DS 角色。  
  
## <a name="BKMK_PS"></a>降級及角色移除 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment 和 ServerManager Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|Uninstall-AddsDomainController|-SkipPreChecks<br /><br />*-LocalAdministratorPassword*<br /><br />-確認<br /><br />***認證***<br /><br />-DemoteOperationMasterRole<br /><br />*-DNSDelegationRemovalCredential*<br /><br />-推動<br /><br />*-ForceRemoval*<br /><br />*-IgnoreLastDCInDomainMismatch*<br /><br />*-IgnoreLastDNSServerForZone*<br /><br />*-LastDomainControllerInDomain*<br /><br />-Norebootoncompletion<br /><br />*-RemoveApplicationPartitions*<br /><br />*-RemoveDNSDelegation*<br /><br />-RetainDCMetadata|  
|解除安裝-WindowsFeature 日移除-WindowsFeature|***名稱***<br /><br />***-IncludeManagementTools***<br /><br />*-重新開機*<br /><br />-中移除<br /><br />-推動<br /><br />-電腦名稱<br /><br />認證<br /><br />-LogPath<br /><br />-Vhd|  
  
> [!NOTE]  
> **-認證**僅需如果您不已登入的企業系統管理員（降級網域中的最後一個 DC）群組成員或（降級複本 DC).The **-includemanagementtools**引數只有如果您想要移除的所有 AD DS 管理公用程式。  
  
## <a name="BKMK_Demote"></a>降級  
  
### <a name="remove-roles-and-features"></a>移除角色與功能  
伺服器管理員會提供介面兩個移除 Active Directory Domain Services 角色：  
  
-   **管理**功能表上的主要儀表板使用**移除角色與功能**  
  
 ![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Manage.png)  
  
-   按一下**AD DS**或**所有伺服器]**上瀏覽窗格。 向下捲動**角色與功能**一節。 以滑鼠右鍵按一下**Active Directory Domain Services**在**角色與功能**清單，然後按**移除角色或功能**。 這個介面略過**選擇伺服器**頁面。  
  
 ![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection.png)  
  
ServerManager cmdlet **WindowsFeature 解除安裝的**和**移除-WindowsFeature**以避免您直到您降級網域控制站移除 AD DS 角色。  
  
### <a name="server-selection"></a>伺服器選取項目  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerSelection2.png)  
  
**選擇伺服器**對話方塊，可讓您選擇其中一集區之前加入伺服器，只要無障礙。 本機伺服器執行伺服器管理員都可供使用。  
  
### <a name="server-roles-and-features"></a>伺服器角色與功能  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ServerRoles.png)  
  
清除**Active Directory Domain Services**核取方塊將網域控制站;目前的網域控制站伺服器，如果這不會移除 AD DS 角色，改為切換到**驗證結果**對話方塊降級提供使用。 或者，只要移除二進位像任何其他角色功能。  
  
-   請勿移除 AD DS 相關角色或功能-DNS、gpmc 中或 RSAT 工具-例如，如果您想要立即再試一次升級的網域控制站。 移除額外的角色及功能隨著時間重新升級，當您重新安裝「角色伺服器管理員重新安裝這些功能。  
  
-   移除不需要的 AD DS 角色及功能自行選擇如果您想要永久降級網域控制站。 這需要清除這些角色與功能的核取方塊。  
  
    包含 AD DS 相關的角色與功能的完整清單：  
  
    -   使用 Windows PowerShell Directory 模組的功能  
  
    -   AD DS 與廣告 LDS 工具功能  
  
    -   Active Directory 管理中心功能  
  
    -   AD DS 嵌入式管理單元及命令列工具功能  
  
    -   DNS 伺服器  
  
    -   群組原則管理主控台  
  
相當於 ADDSDeployment 及 ServerManager Windows PowerShell cmdlet︰  
  
```  
Uninstall-addsdomaincontroller  
Uninstall-windowsfeature  
  
```  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_RemoveFeatures.png)  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demote.png)  
  
### <a name="credentials"></a>認證  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Credentials.png)  
  
您在設定降級選項**認證**頁面。 提供從下列清單執行降級所需的認證：  
  
-   降級額外的網域控制站需要網域管理員認證。 選取 [**強制移除網域控制站的**將網域控制站降級不 Active Directory 中移除網域控制站物件中繼資料。  
  
    > [!WARNING]  
    > 請勿選取此選項，除非網域控制站無法連絡其他網域控制站和有*未合理的方式*解析該網路的問題。 強制的降級離開單獨中繼資料在 Active Directory 中，在森林中的其餘網域控制站。 此外，所有複製未變更密碼] 或 [新增使用者帳號，例如該網域控制站，將會遺失永遠。 單獨中繼資料是在 Microsoft 客戶支援案例的重大百分比 AD DS，Exchange、SQL，及其他軟體的根本原因。  
    >   
    > 如果您強制降級網域控制站您*必須*以手動方式立即執行中繼資料清除。 步驟，檢視[全新向上伺服器中繼資料](https://technet.microsoft.com/library/cc816907(WS.10).aspx)。  
  
   ![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ForceDemote.png)  
  
-   降級網域中的最後一個網域控制站需要企業系統管理員群組成員資格，因為這會移除網域 (如果森林中的最後一個網域，這會移除樹系)。 如果目前的網域控制站網域中的最後一個網域控制站伺服器管理員會通知您。 選取 [**網域中的最後一個網域控制站**核取方塊來確認網域控制站是網域中的最後一個網域控制站。  
  
相當於 ADDSDeployment Windows PowerShell 引數︰  
  
```  
-credential <pscredential>  
-forceremoval <{ $true | false }>  
-lastdomaincontrollerindomain <{ $true | false }>  
  
```  
  
### <a name="warnings"></a>警告  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Warnings.png)  
  
**警告**頁面上通知您移除此網域控制站可能影響。 若要繼續時，您必須選取**繼續移除的**。  
  
> [!WARNING]  
> 如果您先前選取**強制移除網域控制站的**在**認證**頁面上，然後**警告**頁面會顯示此網域控制站裝載所有彈性的單一主機操作角色。 您*必須*抓取從另一部網域控制站的角色*立即*之後降級此伺服器。 如需有關抓取故障，請查看[抓取操作主要角色](https://technet.microsoft.com/library/cc816779(WS.10).aspx)。  
  
此頁面不具有相同 ADDSDeployment Windows PowerShell 引數。  
  
### <a name="removal-options"></a>移除選項  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_ReviewOptions.png)  
  
**移除選項**頁面隨即顯示根據先前選取**網域中的最後一個網域控制站**上**認證**頁面。 本頁可以讓您設定移除其他選項。 選取 [**略過上次 DNS 伺服器區**，**移除應用程式的磁碟分割**，並**移除 DNS 委派**公開**下一步**按鈕。  
  
如果是適用於此網域控制站，只會出現的選項。 例如，是否有此伺服器不 DNS 委派核取方塊會不顯示。  
  
按一下**變更**來指定替代 DNS 管理認證。 按一下**的磁碟分割檢視**若要檢視] 精靈會移除在降級額外的磁碟分割。 根據預設，只額外的磁碟分割的 DNS 網域和森林 DNS 區域。 所有其他磁碟分割的非 Windows 的磁碟分割。  
  
相當於 ADDSDeployment cmdlet 引數︰  
  
```  
-ignorelastdnsserverforzone <{ $true | false }>  
-removeapplicationpartitions <{ $true | false }>  
-removednsdelegation <{ $true | false }>  
-dnsdelegationremovalcredential <pscredential>  
```  
  
### <a name="new-administrator-password"></a>新的系統管理員密碼  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_NewAdminPwd.png)  
  
**系統管理員的新密碼**頁面會要求您提供建本機電腦的系統管理員帳號，密碼之後，請降級完成時，電腦就會網域成員伺服器或工作群組的電腦。  
  
**ADDSDomainController 解除安裝的**cmdlet 和引數如果，請遵循相同的預設值為伺服器管理員未指定。  
  
**LocalAdministratorPassword**引數是特殊：  
  
-   如果*未指定*引數，然後 cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這會是慣用的使用方式  
  
-   如果指定*的值，*，然後值必須安全字串。 這不是執行 cmdlet 互動時慣用的使用方式  
  
例如，您可以手動提示密碼使用**朗讀主機**cmdlet 提示使用者安全字串  
  
```  
-localadministratorpassword (read-host -prompt "Password:" -assecurestring)  
  
```  
  
> [!WARNING]  
> 在前兩個選項不要確認密碼、小心謹慎：看不到密碼  
  
您也可以提供安全字串為轉換明文變數，雖然這是非常不建議使用。 例如：  
  
```  
-localadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
> [!WARNING]  
> 不建議提供或儲存明文密碼。 任何人指令碼執行這個命令或在您身邊尋找知道該電腦的本機系統管理員密碼。 知識，他們可以存取所有的資料與，可模擬伺服器本身。  
  
### <a name="confirmation"></a>確認  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Confirmation.png)  
  
**確認**頁面會顯示降級計劃。在頁面上不會列出降級設定選項。 這是最後一頁降級開始前精靈會顯示。 [檢視指令碼按鈕建立降級 Windows PowerShell 指令碼。  
  
按一下**降級**來執行下列 AD DS 部署 cmdlet:  
  
```  
Uninstall-DomainController  
  
```  
  
使用選擇性**Whatif**以引數**ADDSDomainController 解除安裝的**和 cmdlet 檢視設定的資訊。 這可讓您查看明確和隱含 cmdlet 的引數的值。  
  
例如：  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstall.png)  
  
提示您重新開機是您最後使用 ADDSDeployment Windows PowerShell 取消這項操作機會。 若要覆寫提示，請使用**-強制**或**確認：$false**引數。  
  
### <a name="demotion"></a>降級  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_Demotion.png)  
  
當**降級**頁面會顯示，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
因為**AddsDomainController 解除安裝的**和**WindowsFeature 解除安裝的**只能有一個動作，以讓，它們如下所示確認階段最低檔中。 按下 ENTER 開始冒用降級程序，並在電腦重新開機。  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallConfirm.png)  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallWindowsFeature.png)  
  
若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion: $false**引數。  
  
> [!WARNING]  
> 覆寫在重新開機，建議。 成員伺服器必須重新開機才能正確運作。  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallFinished.png)  
  
以下是範例強制降級的最低需要引數的**-forceremoval**和**-demoteoperationmasterrole**。 **-認證**引數不需要因為成員群組企業系統管理員的身分登入的使用者：  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleForce.png)  
  
以下是範例移除的最低所需的引數網域中的最後一個的網域控制站的**-lastdomaincontrollerindomain**和**-removeapplicationpartitions**:  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallExampleLastDC.png)  
  
如果您嘗試移除 AD DS 角色降級伺服器之前，Windows PowerShell 封鎖您有意錯誤：  
  
```  
Uninstall-WindowsFeature : An uninstallation prerequisite step failed duringthe removal of AD-Domain-Services, and uninstallation cannot continue.1. The domain controller needs to be demoted before the Active DirectoryDomain Services Role can be uninstalled.  
```  
  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_PSUninstallError.png)  
  
> [!IMPORTANT]  
> 您必須重新開機之後降級伺服器，然後才能移除 AD 網域服務角色二進位檔。  
  
### <a name="results"></a>結果  
![降級俠](media/Demoting-Domain-Controllers-and-Domains--Level-200-/ADDS_RRW_TR_DemoteSignoff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 網域控制站將會自動重新開機之後 10 秒。  
  

