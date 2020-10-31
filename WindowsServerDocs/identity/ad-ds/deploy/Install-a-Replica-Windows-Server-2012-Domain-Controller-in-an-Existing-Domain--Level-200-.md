---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: 在現有網域中安裝複本 Windows Server 2012 網域控制站 (等級 200)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 15ecf8d7434636f6acbf10fe1e30db214167fe1c
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070680"
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>在現有網域中安裝複本 Windows Server 2012 網域控制站 (等級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋使用 [伺服器管理員] 或 Windows PowerShell 將現有的樹系或網域升級到 Windows Server 2012 所需的步驟。 其中涵蓋如何將執行 Windows Server 2012 的網域控制站新增至現有的網域中。

-   [升級與複本工作流程](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)

-   [升級與複本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)

-   [部署](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)

## <a name="upgrade-and-replica-workflow"></a><a name="BKMK_Workflow"></a>升級與複本工作流程
下圖說明已安裝過 AD DS 角色，並且使用伺服器管理員啟動 Active Directory 網域服務設定精靈以在現有的網域中建立新的網域控制站時的 Active Directory 網域服務設定程序。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)

## <a name="upgrade-and-replica-windows-powershell"></a><a name="BKMK_PS"></a>升級與複本 Windows PowerShell

| ADDSDeployment  Cmdlet | 引數 ( **粗體** 的引數是必要的。 *斜體* 的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| Install-AddsDomainController | -SkipPreChecks<p>***-DomainName * *_<p>_ -SafeModeAdministratorPassword* <p> *-SiteName* <p> *-ADPrepCredential* <p> -ApplicationPartitionsToReplicate <p> *-AllowDomainControllerReinstall* <p> -Confirm <p> *-CreateDNSDelegation* <p>*** -Credential * *_<p> -CriticalReplicationOnly <p>_ -DatabasePath*<p>*-DNSDelegationCredential*<p>-Force<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>-NoDnsOnNetwork<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>-SiteName<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-UseExistingAccount*<p>*-Whatif* |

> [!NOTE]
> 當您不是以 Enterprise Admins 和 Schema Admins 群組 (如果您要升級樹系) 或 Domain Admins 群組 (如果您要將新的 DC 新增至現有的網域) 的成員身分登入時，才需要 **-credential** 引數。

## <a name="deployment"></a><a name="BKMK_Dep"></a>部署

### <a name="deployment-configuration"></a>部署組態
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)

[伺服器管理員] 會從 [部署設定]  頁面開始升級每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。

若要升級現有的樹系或將可寫入的網域控制站新增至現有的網域，請按一下 [新增現有網域的網域控制站]  ，再依序按一下 [選取]  、[提供此網域的網域資訊]  。 如果需要，[伺服器管理員] 會提示您輸入有效的認證。

升級樹系需要的認證應包含 Windows Server 2012 Enterprise Admins 和 Schema Admins 群組的群組成員資格。 如果您目前的認證不具有適當的權限或群組成員資格，稍後 [Active Directory 網域服務設定精靈] 會提示您。

將網域控制站新增至現有的 Windows Server 2012 網域和執行舊版 Windows Server 的網域控制站所在的網域，兩者間只有自動 Adprep 程序的作業差異。

部署設定 ADDSDeployment Cmdlet 與引數是：

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)

在每個頁面中，會執行某些測試，其中的一部分是稍後會重複的各別先決條件檢查。 例如，如果所選的網域不符合最低的功能等級，在執行升級程序的先決條件檢查之前即可發現：

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)

### <a name="domain-controller-options"></a>網域控制站選項
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)

[網域控制站選項]  頁面指定新網域控制站的網域控制站功能。 可設定的網域控制站功能包括 [DNS 伺服器]  和 [通用類別目錄]  ，以及 [唯讀網域控制站]  。 Microsoft 建議所有網域控制站都提供 DNS 與 GC 服務，以在分散式環境中獲得高可用性。 GC 一律是預設選項，而如果目前的網域已在以起始授權 (SOA) 查詢為基礎的網域控制站上代管 DNS，則預設會選取 DNS 伺服器。 [網域控制站選項]  頁面也能讓您從樹系設定選擇適當的 Active Directory 邏輯 [站台名稱]  。 它預設會選取包含最正確子網路的站台。 如果只有一個站台，就會自動選取該站台。

> [!NOTE]
> 如果伺服器不屬於某個 Active Directory 子網路且有一個以上的 Active Directory 站台，則不會選取任何站台，而且會等到您從清單選擇一個站台後，[下一步]  按鈕才可以使用。

指定的 [目錄服務還原模式密碼]  必須遵守套用至伺服器的密碼原則。 務必選擇複雜的強式密碼，或者最好是使用複雜密碼。

[網域控制站選項]  ADDSDeployment 引數為：

```
-InstallDNS <{$false | $true}>
-NoGlobalCatalog <{$false | $true}>
-sitename <string>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> 提供站台名稱做為 **-sitename** 引數的時候，它必須已經存在。 **install-AddsDomainController** Cmdlet 不會建立站台。 您可以使用 Cmdlet **new-adreplicationsite** 來建立新的站台。

**SafeModeAdministratorPassword** 引數的操作方式比較特殊：

-   如果 *沒有指定* 為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。

    例如，在 treyresearch.net 網域中建立其他網域控制站，並提示您輸入並確認不顯示字元的密碼：

    ```
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)
    ```

-   如果 *使用值* 指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以使用 **Read-Host** Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串：

```
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)
```

> [!WARNING]
> 因為前一個選項不會確認密碼，所以請務必小心使用：密碼是看不到的。

您也可以提供轉換的純文字變數當做安全字串，不過我們不鼓勵這種做法。

```
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)

```

最後，您可以將模糊化密碼儲存到檔案中以在稍後重複使用，而不顯示純文字密碼。 例如：

```
$file = "c:\pw.txt"
$pw = read-host -prompt "Password:" -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> 不建議提供或儲存純文字或模糊化的密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。  任何能夠存取該檔案的人都可以回復模糊化的密碼。 一旦具備該知識，他們可以登入在 DSRM 啟動的網域控制站，最終模擬網域控制站本身，提高其權限到 Active Directory 樹系中的最高層。 有一組建議的額外步驟使用 **System.Security.Cryptography** 將文字檔資料加密，不過這不在本文的討論範圍內。 最佳做法是完全避免儲存密碼。

ADDSDeployment Cmdlet 提供略過自動設定 DNS 用戶端設定、轉寄站及根目錄提示的額外選項。 使用伺服器管理員時，無法略過此設定選項。 如果您是在設定網域控制站之前安裝 DNS 伺服器角色，才需要這個引數：

```
-SkipAutoConfigureDNS
```

如果您現有的網域執行的是 Windows Server 2003，[網域控制站選項]  頁面會警告您無法建立唯讀的網域控制站。 這是預期行為，您可以關閉警告。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)

### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項與 DNS 委派認證
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)

如果您在  選項，且如果指向的區域允許 DNS 委派，則 [DNS 選項]  頁面可讓您設定 DNS 委派。 您可能需要提供屬於 [DNS Admins]  群組成員的使用者的其他認證。

[DNS 選項]  ADDSDeployment Cmdlet 引數是：

```
-creatednsdelegation
-dnsdelegationcredential <pscredential>
```

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)

如需是否需要建立 DNS 委派的詳細資訊，請參閱[了解區域委派](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11))。

### <a name="additional-options"></a>其他選項
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)

[其他選項]  頁面提供可將網域控制站指定為複寫來源的設定選項，或者您可以使用任一網域控制站做為複寫來源。

您也可以使用「從媒體安裝」(IFM) 選項，使用備份的媒體來安裝網域控制站。 選取 [從媒體安裝]  核取方塊時會提供瀏覽選項，您必須按一下 [驗證]  以確保提供的路徑是有效的媒體。 IFM 選項使用的媒體，必須是從另一部現有的 Windows Server 2012 電腦使用 Windows Server Backup 或 Ntdsutil.exe 建立；您不能使用 Windows Server 2008 R2 或舊版作業系統為 Windows Server 2012 網域控制站建立媒體。 如需 IFM 變更的詳細資訊，請參閱[簡化的系統管理附錄](../../ad-ds/deploy/Simplified-Administration-Appendix.md)。 如果使用以 SYSKEY 保護的媒體，伺服器管理員在驗證期間會提示輸入映像的密碼。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)

[其他選項]  ADDSDeployment Cmdlet 引數是：

```
-replicationsourcedc <string>
-installationmediapath <string>
-syskey <secure string>
```

### <a name="paths"></a>路徑
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)

[路徑]  頁面能讓您覆寫 AD DS 資料庫、資料庫交易記錄以及 SYSVOL 共用的預設資料夾位置。 預設位置一律是在 %systemroot% 的子目錄中。

Active Directory 路徑 ADDSDeployment Cmdlet 引數為：

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="preparation-options"></a>準備選項
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)

[準備選項]  頁面會警示您 AD DS 設定包括延伸結構描述 (forestprep) 及更新網域 (domainprep)。  只有在舊版的 Windows Server 2012 網域控制站安裝尚未準備樹系和網域時或您手動執行 Adprep.exe 時，才會看到這個頁面。 例如，如果您將新的網域控制站新增至現有的 Windows Server 2012 樹系根網域，Active Directory 網域服務設定精靈會封鎖這個頁面。

當您按一下 [下一步]  時，並不會延伸結構描述和更新網域。 這些事件只會在安裝階段發生。 這個頁面只是提示稍後安裝時將發生的事件。

這個頁面也會驗證目前使用者的認證是否為 Schema Admin 及 Enterprise Admins 群組的成員，因為您需要這些群組的成員資格才能延伸結構描述或準備網域。 如果這個頁面通知您目前的認證未提供足夠的權限，請按一下 [變更]  以提供適當的使用者認證。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)

其他選項 ADDSDeployment Cmdlet 引數為：

```
-adprepcredential <pscredential>
```

> [!IMPORTANT]
> 和舊版的 Windows Server 一樣，對於執行 Windows Server 2012 的網域控制站，自動化網域準備不會執行 GPPREP。 請為之前不是為 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 準備的所有網域手動執行 **adprep.exe /gpprep** 。 您應只在網域的歷程記錄中執行一次 GPPrep，而不是每次升級時都執行。 Adprep.exe 不會自動執行 /gpprep，因其結果可能導致重新複寫所有網域控制站上 SYSVOL 資料夾中的所有檔案和資料夾。
>
> 當您升級網域中的第一個未執行的 RODC 時，即會執行自動 RODCPrep。 它並不是在您升級第一個可寫入的 Windows Server 2012 網域控制站時發生。 如果您計劃部署唯讀網域控制站，您也仍然可以手動執行 **adprep.exe /rodcprep** 。

### <a name="review-options-and-view-script"></a>檢閱選項和檢視指令碼
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)

[檢閱選項]  頁面能讓您驗證設定，並確保它們符合您的需求，然後才開始安裝。 這不是使用 [伺服器管理員] 停止安裝的最後機會。 這個頁面只是讓您檢閱並確認設定，然後才繼續設定。

[伺服器管理員] 中的 [檢閱選項]  頁面也提供選用的 [檢視指令碼]  按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。  這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。

例如：

```
#
# Windows PowerShell Script for AD DS Deployment
#
Import-Module ADDSDeployment
Install-ADDSDomainController `
-CreateDNSDelegation `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath "C:\Windows\NTDS" `
-DomainName "root.fabrikam.com" `
-InstallDNS:$true `
-LogPath "C:\Windows\NTDS" `
-SiteName "Default-First-Site-Name" `
-SYSVOLPath "C:\Windows\SYSVOL"
-Force:$true

```

> [!NOTE]
> [伺服器管理員] 通常會在升級時填入所有引數的值，並不會依賴預設值 (因為它們在未來的 Windows 版本或 Service Pack 中可能會變更)。 **-safemodeadministratorpassword** 引數是個例外。 以互動方式執行 Cmdlet 時強制確認提示省略值
>
> 使用選擇性的 **Whatif** 引數搭配 **Install-ADDSDomainController** Cmdlet 來檢閱設定資訊。 這可讓您看到 Cmdlet 引數的明確值和隱含值。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)

### <a name="prerequisites-check"></a>先決條件檢查
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)

[先決條件檢查]  是 AD DS 網域設定中的新功能。 這個新階段會驗證網域和樹系可支援新的 Windows Server 2012 網域控制站。

安裝新的網域控制站時，伺服器管理員 Active Directory 網域服務設定精靈會叫用一系列的序列化模組化測試。 這些測試會提醒您建議的修復選項。 您可以視需要執行多次測試。 必須通過所有先決條件測試，網域控制站程序才能繼續。

[先決條件檢查]  也會提供諸如影響舊版作業系統之安全性變更的相關資訊。

如需特定先決條件檢查的詳細資訊，請參閱[先決條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。

使用 [伺服器管理員] 時無法略過 [先決條件檢查]  ，但您可以在使用 AD DS 部署 Cmdlet 時使用下列引數略過該程序：

```
-skipprechecks

```

> [!WARNING]
> Microsoft 建議您不要略過先決條件檢查，因為這樣可能會導致網域控制站升級不完整或 AD DS 樹系損毀。

按一下 [安裝]  以開始網域控制站升級程序。 這是取消安裝的最後機會。 升級程序一旦開始就無法取消。 無論升級結果為何，升級結束後，電腦將自動重新開機。[先決條件檢查]  頁面會顯示過程中發生的任何問題和解決問題的指導方針。

### <a name="installation"></a>安裝
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)

當 [安裝]  頁面顯示時，網域控制站設定程序就開始執行，而且無法暫停或取消。 詳細的作業會顯示此頁面上，而且會寫入到記錄檔：

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

-   %systemroot%\debug\adprep\logs

-   %systemroot%\debug\netsetup.log (如果伺服器在工作群組中)

如果要使用 ADDSDeployment 模組安裝新的 Active Directory 樹系，請使用下列 Cmdlet：

```
Install-addsdomaincontroller
```

如需必要和選擇性引數的詳細資訊，請參閱[升級和複本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)。

**Install-AddsDomainController** Cmdlet 只有兩個階段 (先決條件檢查與安裝)。 下列兩個圖形顯示使用 **-domainname** 與 **-credential** 的基本必要引數的安裝階段。 請注意，在將第一個 Windows Server 2012 網域控制站新增至現有的 Windows Server 2003 樹系的過程中，Adprep 作業是如何自動執行：

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)

請注意 **Install-ADDSDomainController** 如何提醒您升級會自動將伺服器重新開機 (和伺服器管理員一樣)。 若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion** 引數。

> [!WARNING]
> 建議您不要覆寫重新開機設定。 網域控制站必須重新開機才能正常運作。

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)

若要使用 Windows PowerShell 從遠端設定網域控制站，請將 **uninstall-addsdomaincontroller 指令程式** 包裝在 **invoke 命令** Cmdlet *內* 。 這需要使用大括號。

```
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>
```

例如：

![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)

> [!NOTE]
> 如需安裝與 Adprep 程序運作的詳細資訊，請參閱[疑難排解網域控制站部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。

### <a name="results"></a>結果
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)

[結果]  頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 如果成功，網域控制站會在 10 秒後自動重新開機。

和舊版的 Windows Server 一樣，對於執行 Windows Server 2012 的網域控制站，自動化網域準備不會執行 GPPREP。 請為之前不是為 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 準備的所有網域手動執行 **adprep.exe /gpprep** 。 您應只在網域的歷程記錄中執行一次 GPPrep，而不是每次升級時都執行。 Adprep.exe 不會自動執行 /gpprep，因其結果可能導致重新複寫所有網域控制站上 SYSVOL 資料夾中的所有檔案和資料夾。

