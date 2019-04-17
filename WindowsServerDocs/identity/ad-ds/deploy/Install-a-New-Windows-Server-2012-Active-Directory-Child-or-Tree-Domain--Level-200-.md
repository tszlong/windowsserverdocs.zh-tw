---
ms.assetid: e3d55565-ad45-4504-ad73-8103d1a92170
title: "安裝新的 Windows Server 2012 Active Directory 子女或樹網域 (層級 200)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fc0eecc44bbc5f7459f22aceb5ebe41cd61948b6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-new-windows-server-2012-active-directory-child-or-tree-domain-level-200"></a>安裝新的 Windows Server 2012 Active Directory 子女或樹網域 (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何現有的 Windows Server 2012 森林，使用伺服器管理員及 Windows PowerShell 來新增子女和樹網域。  
  
-   [子女和樹網域工作流程](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Workflow)  
  
-   [子女及樹網域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)  
  
-   [部署](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_Deployment)  
  
## <a name="BKMK_Workflow"></a>子女和樹網域工作流程  
下圖顯示 Active Directory Domain Services 設定程序，當您之前已經安裝 AD DS 角色，以及您已經開始進行 Active Directory Domain Services 設定精靈使用伺服器管理員中現有的樹系建立新的網域。  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/adds_childtreedeploy_beta1.png)  
  
## <a name="BKMK_PS"></a>子女及樹網域 Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|**安裝-AddsDomain**|-SkipPreChecks<br /><br />***-NewDomainName***<br /><br />***-ParentDomainName***<br /><br />***-SafeModeAdministratorPassword***<br /><br />*-ADPrepCredential*<br /><br />-AllowDomainReinstall<br /><br />-確認<br /><br />*-CreateDNSDelegation*<br /><br />***認證***<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-NoDNSOnNetwork<br /><br />*-DomainMode*<br /><br />***-DomainType***<br /><br />-推動<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />*-NewDomainNetBIOSName*<br /><br />*-NoGlobalCatalog*<br /><br />-NoNorebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-站台名稱*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SYSVOLPath*<br /><br />*-Whatif*|  
  
> [!NOTE]  
> **-認證**引數只有需要當您未目前登入的企業系統管理員群組成員。**-NewDomainNetBIOSName**如果您想要變更自動根據 DNS 網域名稱前置詞的 15 字元名稱或名稱超過 15 字元，則需要引數。  
  
## <a name="BKMK_Deployment"></a>部署  
  
### <a name="deployment-configuration"></a>部署設定  
下圖顯示 [新增子女網域的選項：  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDeployConfig.png)  
  
下圖顯示 [加入網域樹的選項：  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_TreeDeployConfig.png)  
  
伺服器管理員會開始使用每個網域控制站升級**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。  
  
本主題結合了特定兩項作業： 子女網域促銷和樹網域升級。 只有不同兩項作業時您選擇建立網域型。 所有其他步驟都相同之間兩項作業。  
  
-   建立網域新的子女，請按**現有的樹系加入網域**，然後選擇 [**子網域**。 適用於**家長網域名稱**、 輸入，或選取父系網域名稱。 然後輸入名稱的新的網域中的**新的網域名稱**方塊。 提供有效的、 單一標籤子女網域名稱。名稱必須使用 DNS 網域名稱需求。  
  
-   若要建立樹網域現有的樹系中，按一下**現有的樹系加入網域**，然後選擇 [**樹網域**。 輸入的樹系根網域名稱，然後輸入新的網域名稱。 提供有效的、 完整根網域名稱。必須使用 DNS 網域名稱需求和不是單一標記名稱。  
  
如需 DNS 名稱，請查看[適用於電腦、 網域、 網站及 Ou 命名 Active Directory 規格](https://support.microsoft.com/kb/909264)。  
  
伺服器管理員 Active Directory Domain Services 組態精靈會提示您輸入網域認證如果您目前的憑證並非來自網域。 按一下**變更**提供網域認證升級操作。  
  
部署設定 ADDSDeployment cmdlet 和引數︰  
  
```  
Install-AddsDomain  
-domaintype <{childdomain | treedomain}>  
-parentdomainname <string>  
-newdomainname <string>  
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>網域控制站選項  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_DCOptions_Child.gif)  
  
**網域控制站選項**頁面上指定的新的網域控制站的網域控制站選項。 包含可設定的網域控制站選項**的 DNS 伺服器**和**通用**。您無法在新的網域中的第一個網域控制站設定唯讀網域控制站。  
  
Microsoft 建議所有網域控制站都提供 DNS 和 GC 服務的可用性分散式環境中。 根據預設，則會選取 GC 和 DNS 網域目前已在 [開始] 畫面的授權單位查詢為基礎的網域控制站 DNS 如果已選取預設。 您還必須指定**網域功能等級**。 預設功能層級是 Windows Server 2012，以及您可以選擇任何其他等於或大於目前的樹系功能層級的值。  
  
**網域控制站選項**頁面上也可讓您選擇適當的 Active Directory 邏輯**網站名稱**的樹系設定。 根據預設，選取最正確的子網路的網站。 只有一個網站時，它會自動選取。  
  
> [!IMPORTANT]  
> 如果伺服器不屬於 Active Directory 子網路，而且有一個以上的 Active Directory 網站，就選取任何項目和**下一步**按鈕，即表示，直到您選擇的網站清單。  
  
指定**Directory 服務還原模式密碼**必須遵守密碼原則套用到伺服器。 隨時複雜的密碼或最好複雜密碼。  
  
**網域控制站選項**ADDSDeployment cmdlet 引數：  
  
```  
-InstallDNS <{$false | $true}>  
-NoGlobalCatalog <{$false | $true}>  
-DomainMode <{Win2003 | Win2008 | Win2008R2 | Win2012 | Default}>  
-Sitename <string>  
-SafeModeAdministratorPassword <secure string>  
-Credential <pscredential>  
```  
  
> [!IMPORTANT]  
> 網站名稱必須存在時提供的值為**站台名稱**引數。 **安裝-AddsDomainController** cmdlet 不會建立網站名稱。 您可以使用**新 adreplicationsite** cmdlet 來建立新的網站。  
  
**安裝-ADDSDomainController**如果您不指定 cmdlet 引數請遵循相同的預設值為伺服器管理員。  
  
**SafeModeAdministratorPassword**引數的作業會特殊：  
  
-   如果*未指定*引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
    例如，建立新的子女網域名北美 Contoso.com 森林中的和會提示您輸入並確認密碼遮罩：  
  
    ```  
    Install-ADDSDomain "NewDomainName NorthAmerica "ParentDomainName Contoso.com "DomainType Child  
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
> 不建議提供或儲存清除或模糊文字密碼。 任何人指令碼執行這個命令或在您身邊尋找知道網域控制站 DSRM 的密碼。  任何人的存取權檔案無法反向模糊的密碼。 有了這個認知，他們可以登入以 DSRM 開始 DC 及最後模擬網域控制站本身他們的權限提高 AD 森林中的最高層級。 步驟使用另一組**System.Security.Cryptography**來將檔案加密資料建議但是超出範圍。 最好的做法是完全避免儲存的密碼。  
  
ADDSDeployment 模組提供略過 DNS client 設定、 轉送程式，以及根提示自動設定的其他選項。 這不是可使用伺服器管理員。 此引數重要只有當您已經安裝之前設定的網域控制站的 DNS 伺服器服務：  
  
```  
-SkipAutoConfigureDNS  
  
```  
  
### <a name="dns-options-and-dns-delegation-credentials"></a>DNS 選項]，然後 DNS 的認證委派  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildDNSOptions.png)  
  
**DNS 選項**頁面上，可讓您提供其他 DNS 管理員認證委派。  
  
安裝新的網域中的現有的樹系-上選取 DNS 安裝**網域控制站選項**頁面-您不能設定的任何選項。自動並 irrevocably，就會發生委派。 您有提供權限來更新該結構替代 DNS 管理認證的選項。  
  
**DNS 選項**ADDSDeployment Windows PowerShell 引數：  
  
```  
-creatednsdelegation   
-dnsdelegationcredential <pscredential>  
```  
  
如需 DNS 委派的詳細資訊，請查看[了解區域委派](https://technet.microsoft.com/library/cc771640.aspx)。  
  
### <a name="additional-options"></a>其他選項  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildAdditionalOptions.png)  
  
**的其他選項**頁面顯示 NetBIOS 的網域名稱，可讓您撤銷它。 根據預設，NetBIOS 網域名稱符合上所提供的完整的網域名稱的最左邊標籤**部署組態**頁面。 例如，如果您提供 corp.contoso.com 的完整的網域名稱，預設 NetBIOS 網域名稱是 CORP.  
  
如果 15 字元名稱或較少並不會衝突另一個 NetBIOS 名稱，這是不變。 如果它能與其他 NetBIOS 名稱衝突，數字會附加的名稱。 如果超過 15 字元名稱，精靈將會提供唯一、 被截斷的建議。 不論，精靈先驗證名稱未在使用透過 WINS 查詢，NetBIOS 廣播。  
  
如需 DNS 名稱，請查看[適用於電腦、 網域、 網站及 Ou 命名 Active Directory 規格](https://support.microsoft.com/kb/909264)。  
  
**安裝-AddsDomain**如果您不指定引數請遵循相同的預設值為伺服器管理員。 **DomainNetBIOSName**是特殊的操作：  
  
1.  如果**NewDomainNetBIOSName**未使用 NetBIOS 的網域名稱和單一標籤前置詞網域中的名稱指定引數**網域名稱、** 15 字元或較少，然後升級繼續使用自動的名稱。  
  
2.  如果**NewDomainNetBIOSName**未使用 NetBIOS 的網域名稱和單一標籤前置詞網域中的名稱指定引數**網域名稱、** 16 字元或更多，然後升級失敗。  
  
3.  如果**NewDomainNetBIOSName**指定引數 NetBIOS 網域名稱的 15 字元或較少，然後升級繼續指定名稱。  
  
4.  如果**NewDomainNetBIOSName**指定引數字元 16 NetBIOS 網域名稱或更多，然後升級失敗。  
  
**的其他選項**ADDSDeployment cmdlet 引數是：  
  
```  
-newdomainnetbiosname <string>  
```  
  
### <a name="paths"></a>路徑  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_UpgradePaths.png)  
  
**路徑**頁面上可讓您將會覆寫預設資料夾 AD DS 資料庫、 和的位置資料基底交易登，SYSVOL 共用。 預設位置都之隱藏資料夾中。  
  
**路徑**ADDSDeployment cmdlet 引數：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>檢視選項]，然後檢視指令碼  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildReviewOptions.png)  
  
**評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 這是只要之前繼續進行設定，請先確認您的設定選項  
  
**評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。  此程序會建立進一步修改或直接使用有效且語法正確範例。 例如：  
  
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
> 伺服器管理員通常會填入所有引升級後不會依賴預設值 （因為它們可能會改變之間未來版本 Windows 的 service pack） 的值。 有一個例外此**-safemodeadministratorpassword** （在故意的指令碼省略） 引數。 若要強制確認的提示，請執行 cmdlet 互動時省略值。  
  
使用選擇性**Whatif**以引數**安裝-ADDSForest** cmdlet 檢視設定的資訊。 這可讓您查看 cmdlet 引數明確和隱含的值。  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildWhatIf.png)  
  
### <a name="prerequisites-check"></a>必要條件核取  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildPrereqCheck.png)  
  
**請必要條件**是 AD DS 網域設定中的新功能。 這個新階段驗證伺服器設定不支援 AD DS 新的網域。  
  
當您安裝新的樹系根網域，伺服器管理員 Active Directory Domain Services 組態精靈會叫用一系列序列化模組測試。 這些測試提醒建議的修復選項。 您可以視需要執行測試。 無法繼續網域控制站程序，直到所有必要條件測試傳遞。  
  
**請必要條件**也會呈現相關資訊，例如安全性變更會影響較舊的作業系統。  
  
如需有關的特定的必要條件檢查的詳細資訊，請查看[必要條件檢查](../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
您無法略過**必要條件檢查**時使用伺服器管理員中，但您可以跳過此程序使用 [使用下列引數 AD DS 部署 cmdlet 時：  
  
```  
-skipprechecks  
```  
  
> [!WARNING]  
> Microsoft 會阻礙重覆它會導致部分網域控制站升級或損壞 AD DS 森林略過必要條件檢查。  
  
按一下**安裝**若要開始網域控制站升級程序。 這是最後取消安裝的機會。 開始後，您就無法取消升級程序。 電腦將會在升級，無論促銷結果結尾自動重新開機。  
  
### <a name="installation"></a>安裝  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ChildInstallation.png)  
  
當**安裝**頁面會顯示，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安裝新的 Active Directory domain 使用 ADDSDeployment 模組，使用下列 cmdlet:  
  
```  
Install-addsdomain  
```  
  
查看[子女及樹網域 Windows PowerShell](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md#BKMK_PS)選用和引數。**安裝-addsdomain** cmdlet 僅有兩個階段 （必要條件檢查並安裝）。 下列兩個數據會顯示安裝階段檔最低的**-domaintype**， **-newdomainname**、 **-parentdomainname**，和**-認證**。 請注意，就像伺服器管理員中，**安裝-ADDSDomain** ，升級將會自動重新開機伺服器提醒您。  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomain.png)  
  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_PSInstallADDSDomainProgress.png)  
  
若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion**引數。  
  
> [!WARNING]  
> 建議您不要覆寫在重新開機。 網域控制站必須重新開機才能正常運作  
  
### <a name="results"></a>結果  
![安裝新的廣告子](media/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 網域控制站將會自動重新開機之後 10 秒。  
  

