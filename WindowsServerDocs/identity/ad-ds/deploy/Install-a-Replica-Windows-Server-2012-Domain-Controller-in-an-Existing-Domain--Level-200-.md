---
ms.assetid: e6da5984-d99d-4c34-9c11-4a18cd413f06
title: "安裝複本 Windows Server 2012 網域控制站在現有的網域 (層級 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: e3151a8beee2870ecc747a64241df9d562ad4cc2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-replica-windows-server-2012-domain-controller-in-an-existing-domain-level-200"></a>安裝複本 Windows Server 2012 網域控制站在現有的網域 (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題涵蓋將現有的樹系或網域升級到 Windows Server 2012，使用 Windows PowerShell 或伺服器管理員所需的步驟。 它涵蓋如何加入網域控制站執行 Windows Server 2012 加入現有的網域。  
  
-   [升級及複本工作流程](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [升級及複本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_Dep)  
  
## <a name="BKMK_Workflow"></a>升級及複本工作流程  
下圖顯示 Active Directory Domain Services 設定程序，當您之前已經安裝 AD DS 角色，以及您已經開始進行 Active Directory Domain Services 組態精靈使用現有的網域中建立新的網域控制站伺服器管理員。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/adds_forestupgrade.png)  
  
## <a name="BKMK_PS"></a>升級及複本 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|安裝-AddsDomainController|-SkipPreChecks<br /><br />***-網域名稱***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-站台名稱*<br /><br />*-ADPrepCredential*<br /><br />-ApplicationPartitionsToReplicate<br /><br />*-AllowDomainControllerReinstall*<br /><br />-確認<br /><br />*-CreateDNSDelegation*<br /><br />***認證***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-推動<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />-NoDnsOnNetwork<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />-站台名稱<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-UseExistingAccount*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-認證**引數只有需要如果您不已登入為企業系統管理員和架構管理員群組 （如果您在升級樹系） 或群組網域系統管理員 」 的成員 （如果您現有的網域新增新的 DC）。  
  
## <a name="BKMK_Dep"></a>部署  
  
### <a name="deployment-configuration"></a>部署設定  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDeployConfig.png)  
  
伺服器管理員會開始使用每個網域控制站升級**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。  
  
升級現有的樹系或加入現有的網域新增寫入網域控制站，請按一下**現有的網域中加入的網域控制站**，按一下 [**選取 [**以**指定這個網域的網域資訊**。 伺服器管理員會提示您輸入正確的認證視。  
  
升級樹系需要認證群組成員資格包含 Windows Server 2012 中的企業系統管理員 」 和 「 架構管理員群組。 Active Directory Domain Services 組態精靈會提示您稍後如果您目前的認證，不需要的適當權限或群組成員資格。  
  
自動 Adprep 處理程序有只操作不同加入網域控制站現有的 Windows Server 2012 網域和執行網域控制站較舊版本的 Windows Server 網域。  
  
部署設定 ADDSDeployment cmdlet 和引數︰  
  
```  
Install-AddsDomainController  
-domainname <string>  
-credential <pscredential>  
```  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeSelectDomain.png)  
  
在每個頁面上，其中一些重複稍後為不同的必要條件檢查執行特定測試。 例如，如果選取的網域不符合最低功能的等級，您不必一直促銷前往必要條件檢查以了解：  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeFFLError.png)  
  
### <a name="domain-controller-options"></a>網域控制站選項  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptions.png)  
  
**網域控制站選項**頁面上指定的新的網域控制站的網域控制站功能。 可設定的網域控制站功能的**的 DNS 伺服器**，**通用**，並**唯讀網域控制站**。 Microsoft 建議所有網域控制站都提供 DNS 和 GC 服務的可用性分散式環境中。 GC 隨時選取預設，如果現有的網域主機已經在其 Dc DNS 根據授權起始查詢預設選取 DNS 伺服器。 **網域控制站選項**頁面上也可讓您選擇適當的 Active Directory 邏輯**網站名稱**的樹系設定。 根據預設，它會選取最正確的子網路的網站。 只有一個網站時，會自動選取。  
  
> [!NOTE]  
> 如果伺服器不屬於 Active Directory 子網路，而且有一個以上的 Active Directory 網站，就選取任何項目和**下一步**按鈕，即表示，直到您選擇的網站清單。  
  
指定**Directory 服務還原模式密碼**必須遵守密碼原則套用到伺服器。 隨時複雜的密碼或最好複雜密碼。  
  
**網域控制站選項**ADDSDeployment 引數：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-sitename <string>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 網站名稱必須存在時引數提供**-站台名稱**。 **安裝-AddsDomainController** cmdlet 不會建立的網站。 您可以使用 cmdlet**新 adreplicationsite**來建立新的網站。  
  
**SafeModeAdministratorPassword**引數的作業會特殊：  
  
-   如果*未指定*引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
    例如，treyresearch.net 網域中建立網域控制站，並提示您輸入並確認密碼遮罩：  
  
    ```  
    Install-ADDSDomainController "DomainName treyresearch.net "credential (get-credential)  
    ```  
  
-   如果指定*的值，*，值必須安全字串。 執行 cmdlet 互動時，這是不慣用的使用方式。  
  
例如，您可以手動提示密碼使用**朗讀主機**cmdlet 提示安全字串的使用者：  
  
```  
-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)  
```  
  
> [!WARNING]  
> 在前一個選項不會確認密碼、 小心謹慎： 看不到密碼。  
  
您也可以提供安全字串為轉換明文變數，雖然這是非常不建議使用。  
  
```  
-safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force)  
  
```  
  
最後，您可能會將模糊的密碼儲存在檔案，並再重複使用之後，清除文字並不會顯示密碼。 例如：  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建議提供或儲存清除或模糊文字密碼。 任何人指令碼執行這個命令或在您身邊尋找知道網域控制站 DSRM 的密碼。  任何人的存取權檔案無法反向模糊的密碼。 有了這個認知，他們可以登入以 DSRM 開始 DC 及最後模擬網域控制站本身他們的權限提高 Active Directory 森林中的最高層級。 步驟使用另一組**System.Security.Cryptography**來將檔案加密資料建議但是超出範圍。 最好的做法是完全避免儲存的密碼。  
  
ADDSDeployment cmdlet 提供略過 DNS client 設定、 轉送程式，以及根提示自動設定的其他選項。 您不能略過此組態選項時使用伺服器管理員。 此引數重要只有當您在安裝前設定的網域控制站伺服器的 DNS 伺服器角色：  
  
```  
-SkipAutoConfigureDNS  
```  
  
**網域控制站選項**頁面會警告您無法建立朗讀只網域控制站如果現有的網域控制站執行 Windows Server 2003。 這會如預期般，以及您可以關閉警告。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDCOptionsError.png)  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項]，然後 DNS 的認證委派  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeDNSOptions.png)  
  
**DNS 選項**頁面上，可讓您設定 DNS 委派，如果您選取**DNS 伺服器**上選項*網域控制站選項*頁面上，如果您指向可以 DNS 委派區域。 您可能需要提供其他成員的使用者認證**DNS 管理員**群組。  
  
**DNS 選項**ADDSDeployment cmdlet 引數：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeCreds.png)  
  
如需有關您是否需要建立 DNS 委派的詳細資訊，請查看[了解區域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他選項  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeAdditionalOptions.png)  
  
**的其他選項**頁面上提供設定選項，來命名網域控制站為複寫來源，或您可以使用任何網域控制站為複寫來源。  
  
您也可以選擇備份使用安裝媒體 (IFM) 選項從媒體安裝網域控制站使用。 **安裝媒體的**核取方塊提供選取一次瀏覽] 選項，您必須按**驗證**以確保所提供有效的媒體。 從其他現有 Windows Server 2012 電腦; IFM 選項使用媒體建立與 Windows Server 備份或 Ntdsutil.exe您無法建立 Windows Server 2012 網域控制站媒體使用 Windows Server 2008 R2 或先前的作業系統。 如需變更 IFM 的詳細資訊，請查看[簡化管理附錄](../../ad-ds/deploy/Simplified-Administration-Appendix.md)。 如果使用的 media 受 SYSKEY，伺服器管理員會在驗證期間影像的密碼提示。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_NtdsutilIFM.png)  
  
**的其他選項**ADDSDeployment cmdlet 引數：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-syskey <secure string>  
```  
  
### <a name="paths"></a>路徑  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
**路徑**頁面上，可讓您覆寫預設資料夾位置的 AD DS 資料庫中資料庫交易登，並 SYSVOL 分享。 預設位置都之隱藏資料夾中。  
  
Active Directory 路徑 ADDSDeployment cmdlet 引數︰  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>準備選項  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptions.png)  
  
**準備選項**頁面上，系統會通知您 AD DS 設定，包括擴充架構 (forestprep) 及更新的網域 （準備網域）。  樹系和網域不已經準備手動執行 Adprep.exe 或上一個 Windows Server 2012 網域控制站安裝時，只會看到此頁面。 例如，Active Directory Domain Services 組態精靈會如果現有的 Windows Server 2012 樹系根網域中新增新的網域控制站隱藏此頁面。  
  
延伸架構和更新網域不會發生當您按一下**下一步**。 只有在安裝期間發生這些事件。 此頁面只要帶來在安裝之後會發生的事件有關感知。  
  
這個頁面也驗證的目前使用者的認證管理員架構與企業系統管理員群組成員您需要成員資格擴充架構或準備網域這些群組中。 按一下**變更**頁面會通知您目前的憑證，並不提供不足權限時，提供的適當的使用者的認證。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrepOptionsCreds.png)  
  
其他選項 ADDSDeployment cmdlet 引數是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 為使用舊版的 Windows Server 執行 Windows Server 2012 」 的網域控制站自動化的網域此處不會執行 GPPREP。 執行**adprep.exe /gpprep**以手動方式的所有先前已未準備適用於 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的網域。 您應該先執行一次 GPPrep 歷史不是每份升級與加入網域中。 Adprep.exe 不會執行 /gpprep 自動因為其作業都可能造成所有檔案和資料夾重新複寫所有網域控制站 SYSVOL 資料夾中。  
>   
> 當您升級第一階段未 RODC 網域中的，將會執行自動 RODCPrep。 不是當您第一次的寫入 Windows Server 2012 網域控制站升級。 您也仍然以手動方式可以**adprep.exe /rodcprep**如果您要部署唯讀網域控制站計劃。  
  
### <a name="review-options-and-view-script"></a>檢視選項]，然後檢視指令碼  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeReviewOptions.png)  
  
**評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先其符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 此頁面上可讓您檢查並確認您的設定，才能繼續設定。  
  
**評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。  此程序會建立進一步修改或直接使用有效且語法正確範例。  
  
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
> 伺服器管理員通常會填入所有引升級後不會依賴預設值 （因為它們可能會改變之間未來版本 Windows 的 service pack） 的值。 有一個例外此**-safemodeadministratorpassword**引數。 若要強制確認提示忽略值執行 cmdlet 互動時  
>   
> 使用選擇性**Whatif**以引數**安裝-ADDSDomainController** cmdlet 檢視設定的資訊。 這可讓您查看 cmdlet 引數明確和隱含的值。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSWhatIf.png)  
  
### <a name="prerequisites-check"></a>必要條件核取  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradePrereqCheck.png)  
  
**請必要條件**是 AD DS 網域設定中的新功能。 這個新階段驗證的網域和樹系的新的 Windows Server 2012 網域控制站的支援。  
  
當您安裝新的網域控制站伺服器管理員 Active Directory Domain Services 組態精靈叫用一系列序列化模組測試。 這些測試提醒建議的修復選項。 您可以視需要執行測試。 無法繼續網域控制站程序，直到所有必要條件測試傳遞。  
  
**請必要條件**也會呈現相關資訊，例如安全性變更會影響較舊的作業系統。  
  
如需有關的特定的必要條件檢查，請查看[必要條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
您無法略過**必要條件檢查**時使用伺服器管理員中，但您可以跳過此程序使用 [使用下列引數 AD DS 部署 cmdlet 時：  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 會阻礙重覆它會導致部分網域控制站升級或損壞 AD DS 森林略過必要條件檢查。  
  
按一下**安裝**若要開始網域控制站升級程序。 這是最後取消安裝的機會。 開始後，您就無法取消升級程序。 電腦將會在升級，無論促銷結果結尾自動重新開機。**請必要條件**頁面會顯示在程序和指導方針解析問題時遇到任何問題。  
  
### <a name="installation"></a>安裝  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_UpgradeInstallProgress.png)  
  
當**安裝**頁面會顯示，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
-   %systemroot%\debug\adprep\logs  
  
-   %systemroot%\debug\netsetup.log （如果伺服器工作群組中）  
  
若要安裝新的 Active Directory 森林使用 ADDSDeployment 模組，使用下列 cmdlet:  
  
```  
Install-addsdomaincontroller  
```  
  
查看[升級及複本 Windows PowerShell](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md#BKMK_PS)選用和引數。  
  
**安裝-AddsDomainController** cmdlet 僅有兩個階段 （必要條件檢查並安裝）。 有兩個下方的數字顯示安裝階段檔最低的**的網域名稱**和**-認證**。 請注意 Adprep 作業會自動的第一個 Windows Server 2012 網域控制站加入現有的 Windows Server 2003 樹系的一部分：  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSGetCred.png)  
  
請注意，就像伺服器管理員中，**安裝-ADDSDomainController** ，升級將會自動重新開機伺服器提醒您。 若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion**引數。  
  
> [!WARNING]  
> 覆寫在重新開機，建議。 網域控制站必須重新開機才能正確運作。  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeConfirm.gif)  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeProgress.png)  
  
若要設定的網域控制站從遠端使用 Windows PowerShell，包含**安裝-adddomaincontroller** cmdlet*中*的**叫用命令**cmdlet。 這需要使用大括弧。  
  
```  
invoke-command {install-addsdomaincontroller "domainname <domain> -credential (get-credential)} -computername <dc name>  
```  
  
例如：  
  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_PSUpgradeExample.gif)  
  
> [!NOTE]  
> 如需有關如何安裝及 Adprep 程序運作方式的詳細資訊，請查看[進行疑難排解的網域控制站部署](../../ad-ds/deploy/Troubleshooting-Domain-Controller-Deployment.md)。  
  
### <a name="results"></a>結果  
![安裝複本](media/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 如果成功，網域控制站將會自動重新開機之後 10 秒。  
  
與之前版本的 Windows Server、 執行 Windows server 2012 的網域控制站自動化的網域此處無法執行 GPPREP。 執行**adprep.exe /gpprep**以手動方式的所有先前已未準備適用於 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的網域。 您應該先執行一次 GPPrep 歷史不是每份升級與加入網域中。 Adprep.exe 不會執行 /gpprep 自動因為其作業都可能造成所有檔案和資料夾重新複寫所有網域控制站 SYSVOL 資料夾中。  
  

