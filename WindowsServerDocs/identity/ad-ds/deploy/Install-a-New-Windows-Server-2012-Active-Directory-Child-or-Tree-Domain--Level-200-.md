---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: 安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域 (等級 200)
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: f7244b76364c8e2ce7249af8e76825a08b2a75c8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825331"
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>安裝新的 Windows Server 2012 Active Directory 子網域或樹狀目錄網域 (等級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此主題說明如何使用 [伺服器管理員] 或 Windows PowerShell，將子網域與樹狀目錄網域新增至現有的 Windows Server 2012 樹系。  
  
-   [子域與樹狀目錄網域工作流程](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [子域與樹狀目錄網域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="child-and-tree-domain-workflow"></a><a name="BKMK_Workflow"></a>子域與樹狀目錄網域工作流程  
下圖說明當您已安裝 AD DS 角色且使用 [伺服器管理員] 啟動 [Active Directory 網域服務設定精靈] 在現有的樹系中建立新網域時的「Active Directory 網域服務」設定程序。  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="child-and-tree-domain-windows-powershell"></a><a name="BKMK_PS"></a>子域與樹狀目錄網域 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**的引數是必要的。 *斜體*的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。)|  
|**安裝-Install-addsdomain**|-SkipPreChecks<p>***-NewDomainName***<p>***-ParentDomainName***<p>***-SafeModeAdministratorPassword***<p>*-ADPrepCredential*<p>-AllowDomainReinstall<p>-Confirm<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-NoDNSOnNetwork<p>*-DomainMode*<p>***-DomainType***<p>-Force<p>*-InstallDNS*<p>*-LogPath*<p>*-NewDomainNetBIOSName*<p>*-NoGlobalCatalog*<p>-NoNorebootoncompletion<p>*-ReplicationSourceDC*<p>*-SiteName*<p>-SkipAutoConfigureDNS<p>*-SYSVOLPath*<p>*-Whatif*|  
  
> [!NOTE]  
> 只有當您不是以 Enterprise Admins 群組成員登入時才需要 **-credential** 引數。若要變更根據 DNS 網域名稱首碼自動產生的 15 個字元的名稱或超過 15 個字元的名稱，您將需要 **-NewDomainNetBIOSName** 引數。  
  
## <a name="deployment"></a><a name="BKMK_Deployment"></a>部署  
  
### <a name="deployment-configuration"></a>部署設定  
下列螢幕擷取畫面顯示新增子網域的選項：  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
下列螢幕擷取畫面顯示新增樹狀目錄網域的選項：  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
[伺服器管理員] 會從 [部署設定] 頁面開始升級每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。  
  
此主題結合了兩個不同的操作：子網域升級與樹狀目錄網域升級。 這兩個操作的唯一差異是您選擇建立的網域類型。 這兩個操作的其他所有步驟都是一樣的。  
  
-   若要建立新的子網域，請按一下 [新增網域到現有的樹系]，然後選擇 [子網域]。 對於 [父系網域名稱]，請輸入或選取父系網域的名稱。 接著，在 [新網域名稱] 方塊中輸入新網域的名稱。 提供有效的單一標籤子網域名稱 (該名稱必須符合 DNS 網域名稱需求)。  
  
-   若要在現有的樹系中建立樹狀目錄網域，請按一下 [新增網域到現有的樹系]，然後選擇 [樹狀目錄網域]。 輸入樹系根網域的名稱，然後輸入新的網域名稱。 提供有效的完整根網域名稱 (該名稱不可以是單一標籤，而且必須符合 DNS 網域名稱需求)。  
  
如需 DNS 名稱的詳細資訊，請參閱 [Active Directory 中的電腦、網域、網站及 OU 的命名慣例](https://support.microsoft.com/kb/909264)。  
  
如果您目前的認證不是來自於網域，[伺服器管理員] 中的 [Active Directory 網域服務設定精靈] 會提示您輸入網域認證。 按一下 [變更] 以提供要用來執行升級操作的網域認證。  
  
部署設定 ADDSDeployment Cmdlet 與引數是：  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>網域控制站選項  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
在 [網域控制站選項] 頁面中，您可以指定新網域控制站的網域控制站選項。 可以設定的網域控制站選項包括 [DNS 伺服器] 和 [通用類別目錄]；您不能將唯讀網域控制站設定為新網域中的第一部網域控制站。  
  
Microsoft 建議所有網域控制站都提供 DNS 與 GC 服務，以在分散式環境中獲得高可用性。 預設一律會選取 GC；如果目前的網域已在其 DC 上主控 DNS (根據起始授權查詢)，則預設會選取 DNS。 您也必須指定 [網域功能等級]。 預設功能等級是 Windows Server 2012，而且您可以選擇任何其他大於或等於目前樹系功能等級的值。  
  
[網域控制站選項] 頁面也能讓您從樹系設定選擇適當的 Active Directory 邏輯 [站台名稱]。 根據預設，會選取有最正確之子網路的站台。 如果只有一個站台，會自動選取該站台。  
  
> [!IMPORTANT]  
> 如果伺服器不屬於某個 Active Directory 子網路且有一個以上的 Active Directory 站台，則不會選取任何站台，而且會等到您從清單選擇一個站台後，[下一步] 按鈕才可以使用。  
  
指定的 [目錄服務還原模式密碼] 必須遵守套用至伺服器的密碼原則。 務必選擇複雜的強式密碼，或者最好是使用複雜密碼。  
  
[網域控制站選項] ADDSDeployment Cmdlet 的引數是：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> 提供站台名稱做為 **sitename** 引數值的時候，該站台名稱必須已存在。 **install-AddsDomainController** Cmdlet 不會建立站台名稱。 您可以使用 **new-adreplicationsite** Cmdlet 來建立新的站台。  
  
若未指定 **Install-ADDSDomainController** Cmdlet 引數，則會遵循與 [伺服器管理員] 相同的預設值。  
  
**SafeModeAdministratorPassword** 引數的操作方式比較特殊：  
  
-   如果 *沒有指定* 為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。  
  
    例如，若要在 Contoso.com 樹系中建立名為 NorthAmerica 的新子網域，並提示使用者輸入並確認不顯示字元的密碼：  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
    ```  
  
-   如果*使用值*指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。  
  
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
  
最後，您可以將模糊化密碼儲存到檔案中以在稍後重複使用，而不顯示純文字密碼。 例如，  
  
```  
$file = "c:\pw.txt"  
$pw = read-host -prompt "Password:" -assecurestring  
$pw | ConvertFrom-SecureString | Set-Content $file  
  
-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)  
  
```  
  
> [!WARNING]  
> 不建議提供或儲存純文字或模糊化的密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。  任何能夠存取該檔案的人都可以回復模糊化的密碼。 一旦具備該知識，他們可以登入以 DSRM 啟動的網域控制站，最終模擬網域控制站本身，並將他們在 AD 樹系中的權限提升至最高。 有一組建議的額外步驟使用 **System.Security.Cryptography** 將文字檔資料加密，不過這不在本文的討論範圍內。 最佳做法是完全避免儲存密碼。  
  
ADDSDeployment 模組提供略過 DNS 用戶端、轉寄站與根目錄提示自動設定的其他選項。 使用 [伺服器管理員] 時無法進行此設定。 此引數只在您設定網域控制站前就已安裝「DNS 伺服器」服務時有影響：  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項與 DNS 委派認證  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
[DNS 選項] 頁面可讓您提供委派用的替代 DNS 系統管理員認證。  
  
當您在現有樹系中安裝新網域 (您在 [網域控制站選項] 頁面中選取 DNS 安裝) 時，您無法設定任何選項，委派將會自動發生且無法變更。 您可以提供具有更新該結構之權限的替代 DNS 系統管理認證。  
  
[DNS 選項] ADDSDeployment Windows PowerShell 引數是：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
如需有關 DNS 委派的詳細資訊，請參閱 [了解區域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他選項  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
[其他選項] 頁面會顯示網域的 NetBIOS 名稱，並可讓您覆寫它。 根據預設，NetBIOS 網域名稱符合 [部署設定] 頁面所提供之完整網域名稱最左邊的標籤。 例如，如果您提供的完整的網域名稱是 corp.contoso.com，則預設的 NetBIOS 網域名稱是 CORP。  
  
如果名稱為 15 個字元或更短，且不與其他 NetBIOS 名稱衝突，則名稱不變。 如果名稱與其他 NetBIOS 名稱衝突，系統會在名稱後方附加數字。 如果名稱超過 15 個字元，精靈會提供縮短且具唯一性的建議。 在任一狀況下，精靈都會先透過 WINS 查閱與 NetBIOS 廣播來驗證名稱未使用。  
  
如需 DNS 名稱的詳細資訊，請參閱 [Active Directory 中的電腦、網域、網站及 OU 的命名慣例](https://support.microsoft.com/kb/909264)。  
  
若未指定 **Install-AddsDomain** 引數，則會遵循與 [伺服器管理員] 相同的預設值。 **DomainNetBIOSName** 的操作比較特殊：  
  
1.  如果沒有為 **NewDomainNetBIOSName** 引數指定 NetBIOS 網域名稱，且 **DomainName** 引數中的單一標籤首碼網域名稱為 15 個字元或更短，會以自動產生的名稱繼續升級。  
  
2.  如果沒有為 **NewDomainNetBIOSName** 引數指定 NetBIOS 網域名稱，且 **DomainName** 引數中的單一標籤首碼網域名稱為 16 個字元或更長，則升級會失敗。  
  
3.  如果為 **NewDomainNetBIOSName** 引數指定 15 個字元或更短的 NetBIOS 網域名稱，會以指定的名稱繼續升級。  
  
4.  如果為 **NewDomainNetBIOSName** 引數指定 16 個字元或更長的 NetBIOS 網域名稱，則升級會失敗。  
  
[其他選項] ADDSDeployment Cmdlet 引數為：  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>路徑  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
[路徑] 頁面可讓您覆寫 AD DS 資料庫、資料庫交易記錄檔與 SYSVOL 共用的預設資料夾位置。 預設位置一律是在 %systemroot% 的子目錄中。  
  
[路徑] ADDSDeployment Cmdlet 引數是：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>檢閱選項和檢視指令碼  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
[檢閱選項] 頁面可讓您在開始安裝之前先驗證設定，並確保它們符合您的需求。 使用 [伺服器管理員] 時，這不是能停止安裝的最後機會。 這只是可讓您在繼續設定前確認設定的選項。  
  
[伺服器管理員] 中的 [檢閱選項] 頁面也提供選用的 [檢視指令碼] 按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。  這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。 例如，  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomain `  
-NoGlobalCatalog:$false `  
-CreateDNSDelegation `  
-Credential (Get-Credential) `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainMode "Win2012" `  
-DomainType "ChildDomain" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-NewDomainName "research" `  
-NewDomainNetBIOSName "RESEARCH" `  
-ParentDomainName "corp.contoso.com" `  
-Norebootoncompletion:$false `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> [伺服器管理員] 通常會在升級時填入所有引數的值，並不會依賴預設值 (因為它們在未來的 Windows 版本或 Service Pack 中可能會變更)。 **-safemodeadministratorpassword** 引數是個例外 (刻意在指令碼中省略)。 若要強制確認提示，以互動方式執行 Cmdlet 時請省略該值。  
  
使用選擇性的 **Whatif** 引數搭配 **Install-ADDSForest** Cmdlet 來檢閱設定資訊。 這可讓您看到 Cmdlet 引數的明確值和隱含值。  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>必要條件檢查  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
[先決條件檢查] 是 AD DS 網域設定中的新功能。 這個新的階段會驗證伺服器設定是否能支援新的 AD DS 網域。  
  
安裝新的樹系根網域時，[伺服器管理員] 的 [Active Directory 網域服務設定精靈] 會叫用一系列序列化的模組化測試。 這些測試會提醒您建議的修復選項。 您可以視需要執行多次測試。 必須通過所有先決條件測試，網域控制站程序才能繼續。  
  
[先決條件檢查] 也會提供諸如影響舊版作業系統之安全性變更的相關資訊。  
  
如需特定先決條件檢查的詳細資訊，請參閱[先決條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
使用 [伺服器管理員] 時無法略過 [先決條件檢查] ，但您可以在使用 AD DS 部署 Cmdlet 時使用下列引數略過該程序：  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 建議您不要略過先決條件檢查，因為這樣可能會導致網域控制站升級不完整或 AD DS 樹系損毀。  
  
按一下 [安裝] 以開始網域控制站升級程序。 這是取消安裝的最後機會。 升級程序一旦開始就無法取消。 不論升級結果如何，升級結束時電腦都會自動重新開機。  
  
### <a name="installation"></a>安裝  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
當 [安裝] 頁面顯示時，網域控制站設定程序就開始執行，而且無法暫停或取消。 詳細的作業會顯示此頁面上，而且會寫入到記錄檔：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要使用 ADDSDeployment 模組安裝新的 Active Directory 網域，請使用下列 Cmdlet：  
  
```  
Install-addsdomain  
```  
  
有關必要與選擇性引數，請參閱[子網域與樹狀目錄網域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)。**Install-addsdomain** Cmdlet 只有兩個階段 (先決條件檢查與安裝)。 下列兩個圖形顯示具有 **-domaintype**、 **-newdomainname**、 **-parentdomainname** 與 **-credential** 等最少必要引數的安裝階段。 請注意，就像 [伺服器管理員]，**Install-ADDSDomain** 會提醒您升級將使伺服器自動重新開機。  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion** 引數。  
  
> [!WARNING]  
> 建議您不要覆寫重新開機設定。 網域控制站必須重新開機才能正常運作  
  
### <a name="results"></a>結果  
![安裝新的 AD 子系](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
[結果] 頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 網域控制站會在 10 秒後自動重新開機。  
  

