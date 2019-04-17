---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: "安裝 Active Directory Domain Services (層級 100)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f76aa1e5200a9fc2f47a559c4a318aa619d31557
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-active-directory-domain-services-level-100"></a>安裝 Active Directory Domain Services (層級 100)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何安裝 Windows Server 2012 中 AD DS，使用下列方法：  
  
-   [若要執行 Adprep.exe 並安裝 Active Directory Domain Services 認證需求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)  
  
-   [使用 Windows PowerShell 來安裝 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)  
  
-   [使用伺服器管理員安裝 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)  
  
-   [執行暫存 RODC 安裝使用圖形使用者介面](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)  
  
## <a name="BKMK_Creds"></a>若要執行 Adprep.exe 並安裝 Active Directory Domain Services 認證需求  
下列認證，才能執行 Adprep.exe 並安裝 AD DS。  
  
-   若要安裝新的樹系，您必須本機電腦的系統管理員的身分登入。  
  
-   若要安裝新的子女網域或新的網域樹，您必須成員群組企業系統管理員的身分登入。  
  
-   若要安裝的網域控制站現有網域中，您必須網域管理群組成員。  
  
    > [!NOTE]  
    > 如果您無法執行 adprep.exe 命令另行購買，您要安裝 Windows Server 2012 上執行的現有網域或樹系的第一個網域控制站系統會提示您將會提供認證來執行 Adprep 命令。 認證需求如下：  
    >   
    > -   若要介紹的第一個 Windows Server 2012 網域控制站森林中，您需要的架構管理群組，企業系統管理員群組成員提供的認證並網域系統管理員組網域中裝載架構主機。  
    > -   若要介紹的第一個 Windows Server 2012 網域控制站網域中，您需要提供的認證網域管理群組成員。  
    > -   若要介紹的第一個唯讀網域控制站 (RODC) 森林中，您需要提供的認證管理員企業群組成員。  
    >   
    >     > [!NOTE]  
    >     > 如果您已經執行 Windows Server 2008，或 Windows Server 2008 R2 adprep /rodcprep，您不需要再執行一次適用於 Windows Server 2012。  
  
## <a name="BKMK_PS"></a>使用 Windows PowerShell 來安裝 AD DS  
開始使用 Windows Server 2012，您可以安裝 AD DS，使用 Windows PowerShell。 已被 Dcpromo.exe 取代開始使用 Windows Server 2012，但您仍然可以執行 dcpromo.exe 使用回應檔案 (帶領自動 /:<answerfile>或帶領 /answer:<answerfile>)。 請繼續執行 dcpromo.exe 回應檔案的功能提供資源投資現有自動化時間 dcpromo.exe 自動化轉換到 Windows PowerShell 中的組織。 適用於執行 dcpromo.exe 回應檔案的相關詳細資訊，請查看[https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034)。  
  
如需有關移除使用 Windows PowerShell AD DS，請查看[移除使用 Windows PowerShell AD DS](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS)。  
  
開始使用新增使用 Windows PowerShell 中的角色。 這個命令安裝 AD DS 伺服器角色並安裝 AD DS 與廣告 LDS 伺服器管理工具，包括 GUI 為基礎的工具，例如 Active Directory 使用者及電腦和命令列工具，例如 dcdia.exe。 當您使用 Windows PowerShell 預設不會安裝伺服器管理工具。 您需要指定**「 IncludeManagementTools**來管理本機伺服器或安裝[遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=28972)來管理遠端伺服器。  
  
```  
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools  
<<Windows PowerShell cmdlet and arguments>>  
```  
  
還有不 AD DS 安裝完成後，直到需要重新開機。  
  
您可以再執行這個命令，以檢視可用 cmdlet ADDSDeployment 單元。  
  
```  
Get-Command -Module ADDSDeployment
```  
  
若要查看 cmdlet 和語法可以指定引數清單：  
  
```  
Get-Help <cmdlet name>  
```  
  
例如，若要查看的引數建立位置唯讀網域控制站 (RODC) 帳號，輸入  
  
```  
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```  
  
選擇性引數會出現在方括弧。  
  
您也可以下載最新的協助範例及 Windows PowerShell cmdlet 的概念。 如需詳細資訊，請查看[about_Updatable_Help](https://technet.microsoft.com/library/hh847735.aspx)。  
  
您可以執行 Windows PowerShell cmdlet 遠端伺服器功能：  
  
-   Windows PowerShell 中, 使用 ADDSDeployment cmdlet Invoke-Command。 例如，名為 ConDC3 contoso.com 網域中的遠端伺服器上安裝 AD DS，請輸入：  
  
    ```  
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3  
    ```  
  
-或者-  
  
-   在伺服器管理員中，建立伺服器群組包含遠端伺服器。 以滑鼠右鍵按一下 [遠端伺服器的名稱，然後按一下**Windows PowerShell**。  
  
下一節中如何執行安裝 AD DS ADDSDeployment 模組 cmdlet。  
  
-   [ADDSDeployment cmdlet 引數](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)  
  
-   [指定 Windows PowerShell 認證](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)  
  
-   [使用測試 cmdlet](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)  
  
-   [安裝新的樹系根網域使用 Windows PowerShell](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)  
  
-   [安裝新的子女或樹網域使用 Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)  
  
-   [安裝其他 （複本） 網域控制站使用 Windows PowerShell](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)  
  
### <a name="BKMK_Params"></a>ADDSDeployment cmdlet 引數  
下表列出 Windows PowerShell 中 ADDSDeployment cmdlet 引數。 引數以粗體顯示所需項目。 如果稱為 Windows PowerShell 中的其他 dcpromo.exe 相等引數所示括號。  
  
Windows PowerShell 參數接受 $TRUE 或 $FALSE 引數。 不需要指定引數 $TRUE 預設程式。  
  
若要覆寫預設值，您可以指定引數 $False 值。 例如，因為**-installdns**自動執行安裝新的樹系如果未指定的唯一方式*避免*DNS 安裝當您安裝新的樹系是使用：  
  
```  
-InstallDNS:$false  
```  
  
同樣地，因為**「 installdns**有 $False 預設值，如果您安裝的網域控制站在環境不主控 Windows Server DNS 伺服器，您需要安裝 DNS 伺服器，才能指定下列引數：  
  
```  
-InstallDNS:$true  
```  
  
|引數|描述|  
|------------|---------------|  
|**ADPrepCredential <PS Credential> ** **請注意：**所需如果您正在安裝網域中的第一個 Windows Server 2012 網域控制站或森林與目前的使用者的認證的不足，無法執行此作業。|根據的規則企業系統管理員和架構系統管理員可以準備、 樹系的群組成員資格指定 account[取得認證](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 物件。<br /><br />如果未指定值的值**「 認證**使用引數。|  
|AllowDomainControllerReinstall|指定要繼續安裝此寫入網域控制站，雖然的偵測到另一個寫入網域控制站 account 具有相同名稱。<br /><br />使用**$True**僅您是否確定 account 目前無法使用，另一個寫入網域控制站。<br /><br />預設值是**$False**。<br /><br />此引數不正確的 RODC。|  
|AllowDomainReinstall|指定現有的網域是否重新建立。<br /><br />預設值是**$False**。|  
|AllowPasswordReplicationAccountName < 字串 [>|指定帳號，群組帳號，並其密碼可以複製到此 RODC 電腦帳號的名稱。 使用空字串 」 」 如果您想要保留空白的值。 根據預設，允許只允許 RODC 密碼複寫群組，而最初建立空白。<br /><br />提供的值為字串陣列。 例如：<br /><br />程式碼-AllowPasswordReplicationAccountName 」 JSmith 」、 「 JSmithPC 「，「 分支使用者]|  
|ApplicationPartitionsToReplicate < 字串 [>**請注意：**在 UI 不等選項。 如果您安裝使用 UI，或者 IFM，將會複寫所有應用程式的磁碟分割。|指定要複製的應用程式 directory 磁碟分割。 只有當您指定套用此引數**-InstallationMediaPath**若要安裝的媒體 (IFM) 引數。 根據預設，所有應用程式會將磁碟分割依據他們自己的範圍。<br /><br />提供的值為字串陣列。 例如：<br /><br />程式碼-<br /><br />-ApplicationPartitionsToReplicate 」 partition1 」、 「 partition2 」、 「 partition3 」|  
|確認|會提示您先執行 cmdlet 確認。|  
|CreateDnsDelegation**請注意：**當您執行新增-ADDSReadOnlyDomainController cmdlet 不能指定此引數。|表示是否要建立新的 DNS 伺服器，您的網域控制站以及安裝的參考 DNS 委派。 有效的 Active Directory 」 整合 DNS 只。 委派記錄只可以建立的使用者且無障礙 online Microsoft DNS 伺服器。 委派記錄無法建立的最上層網域，例如.com、.gov、.biz、.edu 或兩個字母國家代碼網域.nz 和.au 屬於網域。<br /><br />根據環境自動計算預設值。|  
|**認證<PS Credential> ** **請注意：**需要目前使用者的認證是否不足，無法執行此作業。|指定可以登入網域，核對按照[取得認證](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 物件。<br /><br />如果未指定值，會使用目前的使用者的認證。|  
|CriticalReplicationOnly|指定 AD DS 安裝操作是否會執行只重大複製再重新開機，然後再繼續。 安裝完成後，電腦重新開機，就會發生重要複寫。<br /><br />不建議使用此引數。<br /><br />還有不相當於在使用者介面 (UI)，此選項。|  
|DatabasePath <string>|指定完整非 「 磁碟包含網域資料庫，例如，在本機電腦的硬碟上路徑通用命名規格 (UNC) **C:\Windows\NTDS。**<br /><br />預設值是**%SYSTEMROOT%\NTDS**。 **重要事項：**時，您可以將使用復原檔案系統 (ReFS) 格式化磁碟區 AD DS 資料庫並登入檔案，有任何特定好處裝載上 ReFS AD DS，您取得以外的恢復正常優點裝載 ReFS 上的任何資料。|  
|DelegatedAdministratorAccountName <string>|指定的使用者或群組，可以安裝及管理 RODC 的名稱。<br /><br />根據預設，僅限群組成員的網域系統管理員可以管理 RODC。|  
|DenyPasswordReplicationAccountName < 字串 [>|指定帳號，群組帳號，並其密碼不提供複製到此 RODC 電腦帳號的名稱。 使用空字串 」 」 如果您不希望拒絕複寫的任何使用者或電腦的認證。 根據預設，系統管理員，伺服器電信業者、 備份電信業者、 Account 電信業者，並拒絕 RODC 密碼複寫群組拒絕。 根據預設，拒絕 RODC 密碼複寫群組包含憑證發行者、 網域系統管理員，企業系統管理員、 企業網域控制站、 企業唯讀網域控制站、 群組原則 Creator 擁有者、 krbtgt 帳號及架構系統管理員。<br /><br />提供的值為字串陣列。 例如：<br /><br />程式碼-<br /><br />-DenyPasswordReplicationAccountName 」 RegionalAdmins 」、 「 AdminPCs 」|  
|DnsDelegationCredential <PS Credential> **請注意：** cmdlet 新增-ADDSReadOnlyDomainController 執行時，您就不能指定此引數。|指定的使用者名稱和密碼建立 DNS 委派，按照[取得認證](https://technet.microsoft.com/library/dd315327.aspx)和 PSCredential 物件。|  
|DomainMode <DomainMode> {Win2003 和 #124;Win2008 與 #124;Win2008R2 與 #124;Win2012 與 #124;Win2012R2}<br /><br />或<br /><br />DomainMode <DomainMode> {2 與 #124; 3 和 #124; 4 與 #124; 5 和 #124; 6}|建立新的網域期間指定的網域功能層級。<br /><br />層級不得低於的樹系功能的層級，但很高正常運作的網域。<br /><br />自動計算和為現有的樹系功能等級或值設定為預設值**-ForestMode**。|  
|**網域名稱**<br /><br />所需的安裝-ADDSForest 和安裝-ADDSDomainController cmdlet。|指定您要安裝的其他網域控制站的網域的 FQDN。|  
|**DomainNetbiosName <string>**<br /><br />如果超過 15 字元 FQDN 前置詞名稱所需安裝-ADDSForest。|安裝-ADDSForest 搭配使用。 指定新的樹系根網域 NetBIOS 的名稱。|  
|DomainType <DomainType> {ChildDomain 和 #124;TreeDomain} 或 {子女和 #124; 樹}|表示您想要建立網域類型： 現有中新的網域樹狀結構樹系的現有的網域或新的樹系的子女。<br /><br />DomainType 預設值是 ChildDomain。|  
|推動|當此參數指定允許完成執行 cmdlet 將會隱藏起來任何期間的安裝和加入的網域控制站可能通常會顯示警告。 此參數可能包含指令碼安裝時，很有幫助。|  
|ForestMode <ForestMode> {Win2003 和 #124;Win2008 與 #124;Win2008R2 與 #124;Win2012 與 #124;Win2012R2}<br /><br />或<br /><br />ForestMode <ForestMode> {2 與 #124; 3 和 #124; 4 與 #124; 5 和 #124; 6}|當您建立新的樹系，指定的樹系功能層級。<br /><br />預設值是 Win2012。|  
|InstallationMediaPath|表示的安裝媒體，可用於安裝新的網域控制站的位置。|  
|InstallDns|指定 DNS 伺服器服務應該安裝和設定的網域控制站。<br /><br />新的樹系的預設值是**$True**並安裝 DNS 伺服器。<br /><br />新的子女網域或網域樹，如果已主控家長網域 （或的網域樹森林根網域），會儲存 DNS 網域名稱預設值為此參數則 $True。<br /><br />網域控制站安裝在現有的網域中，此參數是否左未指定及目前網域已經主控儲存網域中的 DNS 名稱，然後的預設值為此參數**$True**。 或者，如果 DNS 網域名稱裝載 Active Directory 以外，預設值是**$False**並不安裝任何 DNS 伺服器。|  
|LogPath <string>|指定，非-注意到 directory 包含網域登入檔案，例如，在本機電腦的硬碟上**C:\Windows\Logs**。<br /><br />預設值是**%SYSTEMROOT%\NTDS**。 **重要事項：**未儲存的資料復原檔案系統 (ReFS) 格式化的磁碟區上的 Active Directory 登入檔案。|  
|MoveInfrastructureOperationMasterRoleIfNecessary|指定要傳輸到您要建立 」 中目前置於主機通用案例 」 並不想讓的網域控制站的網域控制站的基礎結構主機操作主角 （也稱為彈性的單一主機操作或 FSMO），您是否要建立通用伺服器。 指定基礎結構主角傳輸到您所建立轉送是需要; 網域控制站此參數若是如此，指定**NoGlobalCatalog**如果您想要保留目前所在的基礎結構主角選項。|  
|**NewDomainName <string> ** **請注意：**所需的安裝-ADDSDomain 只。|指定新的網域單一網域名稱。<br /><br />例如，如果您想要建立新的子女網域名為**emea.corp.fabrikam.com**，您應該會指定**emea**為這個引數。|  
|**NewDomainNetbiosName <string>**<br /><br />如果超過 15 字元 FQDN 前置詞名稱所需安裝-ADDSDomain。|安裝-ADDSDomain 搭配使用。 將 NetBIOS 名稱指派給新的網域。 預設值源自的值**「 NewDomainName**。|  
|NoDnsOnNetwork|指定 DNS 服務不是可用的網路。 使用此參數才並未設定 IP 設定，這台電腦的網路介面卡的名稱解析 DNS 伺服器的名稱。 這表示 DNS 伺服器，將會在這台電腦的名稱解析安裝。 否則，IP 設定的網路介面卡的第一次必須 DNS 伺服器位址設定。<br /><br />省略 （預設值） 此參數，表示 client 的 TCP/IP 設定此伺服器上的網路介面卡將會使用連絡 DNS 伺服器。 因此，如果您不指定此參數，確定 TCP/IP client 設定的第一次設定慣用 DNS 伺服器位址。|  
|NoGlobalCatalog|指定您不想要為通用伺服器的網域控制站。<br /><br />執行 Windows Server 2012 」 的網域控制站安裝通用使用預設。 亦即，這會自動執行而不需要的計算，除非您指定：<br /><br />程式碼-<br /><br />-NoGlobalCatalog|  
|NoRebootOnCompletion|指定要命令，無論成功完成電腦重新開機。 根據預設，電腦將會重新開機。 若要防止伺服器重新，指定：<br /><br />程式碼-<br /><br />-NoRebootOnCompletion: $True<br /><br />還有不相當於在使用者介面 (UI)，此選項。|  
|**ParentDomainName <string> ** **請注意：**所需的安裝-ADDSDomain cmdlet|指定現有家長網域的 FQDN。 當您安裝新的網域樹或子女網域，您可以使用此引數。<br /><br />例如，如果您想要建立新的子女網域名為**emea.corp.fabrikam.com**，您應該會指定**corp.fabrikam.com**為這個引數。|  
|ReadOnlyReplica|指定要安裝的唯讀網域控制站 (RODC)。|  
|ReplicationSourceDC <string>|代表您要從中複製的網域資訊的合作夥伴網域控制站的 FQDN。 預設會自動運算。|  
|**SafeModeAdministratorPassword <securestring>**|當您的電腦開始使用 「 安全模式 」 或 「 安全模式下，例如 Directory 服務還原模式的 variant 會提供系統管理員密碼。<br /><br />預設會是空白的密碼。 您必須輸入密碼。 必須 System.Security.SecureString 格式，例如所提供的讀取主機-assecurestring 或 ConvertTo-SecureString 提供密碼。<br /><br />SafeModeAdministratorPassword 引數作業特殊： 如果未指定為引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。如果指定不值，而且有其他引數指定給 cmdlet、 cmdlet 會提示您輸入而確認遮罩的密碼。 執行 cmdlet 互動時，這是不慣用的使用方式。如果指定值，值必須安全字串。 執行 cmdlet 互動時，這是不慣用的使用方式。例如您可以手動提示密碼提示安全字串的使用者使用朗讀主機 cmdlet:-safemodeadministratorpassword (讀取主機-命令提示字元中 」 的密碼: 「-assecurestring) 您也可以提供安全字串為轉換明文變數，雖然這是非常不建議使用。 -safemodeadministratorpassword (convertto securestring 「 Password1 「-asplaintext-強制)|  
|**站台名稱 <string>**<br /><br />所需的新增-addsreadonlydomaincontrolleraccount cmdlet|指定的網域控制站會安裝所在的網站。 有任何**「 站台名稱**引數當您執行**安裝-ADDSForest**因為建立的第一個網站預設先網站的名稱。<br /><br />網站名稱必須存在時引數提供**-站台名稱**。 Cmdlet 不會建立該網站。|  
|SkipAutoConfigureDNS|略過 DNS client 設定、 轉送程式，以及根提示自動設定。 此引數才有效的 DNS 伺服器服務已安裝或使用自動安裝**-InstallDNS**。|  
|SystemKey <string>|指定您要從中複製資料媒體系統鍵。<br /><br />預設值是**無**。<br /><br />資料必須朗讀主機-assecurestring 或 ConvertTo-SecureString 所提供的格式。|  
|SysvolPath <string>|例如，在本機電腦的硬碟指定，非-注意到 directory **C:\Windows\SYSVOL**。<br /><br />預設值是**%SYSTEMROOT%\SYSVOL**。 **重要事項：** SYSVOL 無法儲存的資料復原檔案系統 (ReFS) 格式化的磁碟區上。|  
|SkipPreChecks|不會執行開始安裝之前的必要條件檢查。 不建議使用此設定。|  
|WhatIf|如果是執行 cmdlet 會發生的事情顯示。 Cmdlet 不會執行。|  
  
### <a name="BKMK_PSCreds"></a>指定 Windows PowerShell 認證  
您可以指定認證，而不會顯示他們在螢幕上的一般使用[取得認證](https://technet.microsoft.com/library/dd315327.aspx)。  
  
這項操作-SafeModeAdministratorPassword 和 LocalAdministratorPassword 引數是特殊：  
  
-   如果無法為引數指定，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
-   如果指定值，值必須安全字串。 執行 cmdlet 互動時，這是不慣用的使用方式。  
  
例如，您可以手動提示密碼使用**朗讀主機**cmdlet 提示使用者安全字串  
  
```  
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```  
  
> [!WARNING]  
> 在前一個選項不會確認密碼、 小心謹慎： 看不到密碼。  
  
您也可以提供安全字串為轉換明文變數，雖然這是高度建議：  
  
```  
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```  
  
> [!WARNING]  
> 不建議提供或儲存明文密碼。 任何人指令碼執行這個命令或在您身邊尋找知道網域控制站 DSRM 的密碼。 有了這個認知，他們可以模擬本身的網域控制站及他們的權限提高至 Active Directory 森林中的最高層級。  
  
### <a name="BKMK_TestCmdlets"></a>使用測試 cmdlet  
每個 ADDSDeployment cmdlet 已測試 cmdlet 相對應。 測試 cmdlet 執行的必要條件檢查安裝作業。未安裝設定設定。 每個測試 cmdlet 引數是對應安裝 cmdlet，一樣，但**「 SkipPreChecks**不適用於 cmdlet 測試。  
  
|測試 cmdlet|描述|  
|---------------|---------------|  
|Test-ADDSForestInstallation|執行適用於安裝新的 Active Directory 樹系的必要條件。|  
|Test-ADDSDomainInstallation|執行 Active Directory 中安裝新的網域的必要條件。|  
|Test-ADDSDomainControllerInstallation|執行 Active Directory 中安裝網域控制站的必要條件。|  
|Test-ADDSReadOnlyDomainControllerAccountCreation|(RODC) account 執行新增唯讀網域控制站的必要條件。|  
  
### <a name="BKMK_PSForest"></a>安裝新的樹系根網域使用 Windows PowerShell  
適用於安裝新的樹系的命令語法如下所示。 選擇性引數會出現在方括弧。  
  
```  
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> 如果您想要變更的 15 字元名稱所自動根據 DNS 網域名稱前置詞或名稱超過 15 字元需要-DomainNetBIOSName 引數。  
  
例如，安裝新的樹系名 corp.contoso.com 和安全地提供 DSRM 密碼提示，請輸入：  
  
```  
Install-ADDSForest -DomainName "corp.contoso.com"   
```  
  
> [!NOTE]  
> 當您執行安裝-ADDSForest 預設會安裝 DNS 伺服器。  
  
若要安裝新的樹系名 corp.contoso.com、 contoso.com 網域中建立 DNS 委派、 網域正常運作的層級設定為 Windows Server 2008 R2 森林功能層級設定為 Windows Server 2008、 安裝 D:\ 磁碟機上的 Active Directory 資料庫和 SYSVOL、 安裝登入 E:\ 磁碟機上的檔案並將提示提供 Directory 服務還原模式密碼並輸入:  
  
```  
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="BKMK_PSDomain"></a>安裝新的子女或樹網域使用 Windows PowerShell  
適用於安裝新的網域命令語法如下所示。 選擇性引數會出現在方括弧。  
  
```  
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
> [!NOTE]  
> **-認證**引數只有需要當您未目前登入的企業系統管理員群組成員。  
>   
> **-NewDomainNetBIOSName**如果您想要變更自動根據 DNS 網域名稱前置詞的 15 字元名稱或名稱超過 15 字元，則需要引數。  
  
例如使用 corp\EnterpriseAdmin1 的憑證來建立新的子女網域名稱 child.corp.contoso.com，安裝 DNS 伺服器、 corp.contoso.com 網域中建立 DNS 委派、 網域正常運作的層級設定為 Windows Server 2003、 進行網域控制站通用伺服器名休斯頓網站、 DC1.corp.contoso.com 當做複寫來源網域控制站、 安裝 D:\ 磁碟機上的 Active Directory 資料庫和 SYSVOL登入上安裝檔案 E:\ 磁碟機，並提供 Directory 服務還原模式的密碼，但不是會提示您確認命令中，輸入提示：  
  
```  
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False  
```  
  
### <a name="BKMK_PSReplica"></a>安裝其他 （複本） 網域控制站使用 Windows PowerShell  
安裝其他的網域控制站的命令語法如下所示。 選擇性引數會出現在方括弧。  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
若要安裝的網域控制站和 DNS 伺服器 corp.contoso.com 網域中，並提示您提供系統管理員認證網域和 DSRM 密碼，輸入  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```  
  
如果您的電腦已經加入網域與您的網域管理群組成員，您可以使用：  
  
```  
Install-ADDSDomainController -DomainName "corp.contoso.com"  
```  
  
若要會提示輸入的網域名稱，請輸入：  
  
```  
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```  
  
下列命令會使用 Contoso\EnterpriseAdmin1 認證安裝名為波士頓網站寫入網域控制站和通用伺服器、 安裝的 DNS 伺服器、 contoso.com 網域中建立 DNS 委派、 安裝媒體 c:\ADDS IFM 資料夾中儲存的、 安裝 D:\ 磁碟機上的 Active Directory 資料庫和 SYSVOL，請安裝登入 E:\ 磁碟機上的檔案有伺服器會自動重新開機之後 AD DS 安裝完成後，並提示您提供 Directory 服務還原模式的密碼：  
  
```  
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"   
```  
  
### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>執行階段的 RODC 安裝使用 Windows PowerShell  
命令語法建立 RODC account 如下所示。 選擇性引數會出現在方括弧。  
  
```  
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]  
```  
  
以下是命令語法 RODC 過去連接伺服器。 選擇性引數會出現在方括弧。  
  
```  
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]  
```  
  
例如，為您建立 RODC 帳號名 RODC1:  
  
```  
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA  
```  
  
然後執行下列命令，以您想要附加到 RODC1 account 的伺服器上。 無法加入網域的伺服器。 首先，請安裝 AD DS 伺服器角色與管理工具：  
  
```  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```  
  
執行下列命令，以建立 RODC:  
  
```  
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```  
  
按**Y**以確認或包含**[確認**以避免確認提示引數。  
  
## <a name="BKMK_GUI"></a>使用伺服器管理員安裝 AD DS  
AD DS 可以在 Windows Server 2012 中安裝在伺服器管理員中，後面 Active Directory Domain Services 組態精靈，這是在 Windows Server 2012 中的新開始使用新增角色精靈。 Active Directory Domain Services 安裝精靈 (dcpromo.exe) 會取代開始在 Windows Server 2012 中。  
  
下列章節有關如何建立伺服器集區，才能安裝及管理 AD DS 在多部伺服器，以及如何使用精靈安裝 AD DS。  
  
### <a name="BKMK_ServerPools"></a>建立伺服器集區  
伺服器管理員可集區其他網路上的伺服器，只要的電腦上執行伺服器管理員可存取。 之後共用，您可以選擇那些伺服器遠端安裝 AD DS，或在伺服器管理員中可能任何其他設定選項。 伺服器管理員會自動執行電腦集區本身。 如需伺服器集區的詳細資訊，請查看[新增伺服器伺服器管理員以](https://technet.microsoft.com/library/hh831453.aspx)。  
  
> [!NOTE]  
> 為了管理伺服器管理員使用群組伺服器，或相反加入網域的電腦，則需要額外的設定步驟。 如需詳細資訊，請查看 「 新增和管理工作群組中的伺服器 」 中[新增伺服器伺服器管理員以](https://technet.microsoft.com/library/hh831453.aspx)。  
  
### <a name="BKMK_installADDSGUI"></a>安裝 AD DS  
**管理認證**  
  
安裝 AD DS 的認證需求會在您選擇要部署的設定視而有所不同。 如需詳細資訊，請查看[認證需求執行 Adprep.exe 並安裝 Active Directory Domain Services](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
使用下列程序安裝 AD DS 使用 GUI 方法。 在本機或遠端步驟執行。 適用於更多需下列步驟進行，查看下列主題：  
  
-   [部署樹系的伺服器管理員](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)  
  
-   [安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  
-   [安裝新的 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
  
-   [安裝 Windows Server 2012 Active Directory 唯讀網域控制站和 #40;RODC 和 #41;與 #40;層級 200 和 #41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)  
  
##### <a name="to-install-ad-ds-by-using-server-manager"></a>若要使用伺服器管理員安裝 AD DS  
  
1.  在伺服器管理員中，按一下**管理**，按一下 [**新增角色與功能**到開始畫面新增角色精靈。  
  
2.  在**在您開始之前**頁面上，按一下 [**下**。  
  
3.  在**選擇安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。  
  
4.  在**選取目的伺服器**頁面上，按一下 [**選取伺服器伺服器集區的**，按一下您要安裝 AD DS，然後按一下 [伺服器名稱**下一步**。  
  
    若要選取 [遠端伺服器，第一次建立集區伺服器，並加入遠端伺服器。 如需有關建立伺服器集區的詳細資訊，請查看[新增伺服器伺服器管理員以](https://technet.microsoft.com/library/hh831453.aspx)。  
  
5.  在**選取伺服器角色**頁面上，按一下 [ **Active Directory Domain Services**，然後在 [**新增角色與功能精靈**對話方塊中，按一下**新增功能**，，然後按一下 [**下一步**。  
  
6.  在**選擇功能**頁面上，選取您想要安裝按一下任何其他功能**下**。  
  
7.  在**Active Directory Domain Services**頁面上，檢視的資訊，然後按一下**下**。  
  
8.  在**確認安裝選項**頁面上，按**安裝**。  
  
9. 在**結果**頁面上，確認已成功完成，再按一下安裝**此為網域控制站伺服器升級**以開始 Active Directory Domain Services 組態精靈。  
  
    ![安裝 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)  
  
    > [!IMPORTANT]  
    > 如果您不需要從 Active Directory Domain Services 組態精靈關閉新增角色精靈此時，您可以重新它，即可在伺服器管理員中的工作。  
  
    ![安裝 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
10. 在**部署組態**頁面上，選擇下列其中一個選項：  
  
    -   如果您安裝其他網域控制站現有網域中，按一下 [**現有的網域中加入的網域控制站**，並輸入網域 (例如，emea.corp.contoso.com) 的名稱，或按**選取...**選擇加入網域和認證 （例如，指定為網域管理群組成員），然後按一下 [**下**。  
  
        > [!NOTE]  
        > 只有當電腦已加入網域，而且您執行的是本機安裝預設提供的網域目前使用者的認證的名稱。 如果您遠端伺服器上安裝 AD DS，您需要設計來指定憑證。 如果不足以執行安裝目前使用者的認證，請按一下**變更...**以指定不同的認證。  
  
        如需詳細資訊，請查看[安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md).  
  
    -   如果您安裝新的子女網域，按一下 [**現有的樹系新增新的網域**的**選取網域型**，選取**子網域**、 輸入瀏覽至父系網域 DNS 名稱 (例如，corp.contoso.com) 的名稱、 輸入相對新的子女網域名稱 (例如 emea) 輸入認證，以建立新的網域，並按一下 [使用**下一步**。  
  
        如需詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   如果您安裝新的網域樹，按一下 [**新增新的網域現有的樹系**的**選取網域型**，選取**樹網域**輸入根網域 (例如，corp.contoso.com) 的名稱、 輸入新的網域 (例如，fabrikam.com) 輸入認證，以用來建立新的網域，並按一下 [DNS 名稱**下一步**。  
  
        如需詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md).  
  
    -   如果您安裝新的樹系，按**新增新的樹系**，然後輸入名稱根網域 (例如，corp.contoso.com)。  
  
        如需詳細資訊，請查看[安裝新 Windows Server 2012 Active Directory 樹系和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md).  
  
11. 在**網域控制站選項**頁面上，選擇下列其中一個選項：  
  
    -   如果您要建立新的樹系或網域，選擇網域和森林功能層級，請按一下**網域名稱系統 」 (DNS) 伺服器**，指定 DSRM 密碼，然後按**下一步**。  
  
    -   如果您要加入的網域控制站現有的網域，按一下 [**網域名稱系統 」 (DNS) 伺服器**，**全球 Catalog (GC)**，或**朗讀只網域控制站 (RODC)**視需要選擇網站的名稱，並輸入 DSRM 密碼，然後按一下**下一步**。  
  
    適用於相關的選項，在此頁面上的不同條件在無法使用或的詳細資訊，請查看[網域控制站選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)。  
  
12. 在**DNS 選項**（，才會出現在您安裝的 DNS 伺服器） 頁面上，按一下 [**更新 DNS 委派**視。 如果您執行動作時，提供認證所建立的 DNS 委派記錄家長 DNS 區域中的權限。  
  
    如果無法連絡主控家長區域的 DNS 伺服器，**更新 DNS 委派**選項。  
  
    如需有關您是否需要更新 DNS 委派的詳細資訊，請查看[了解區域委派](https://technet.microsoft.com/library/cc771640.aspx)。 如果您嘗試更新 DNS 委派和發生錯誤，請查看[DNS 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。  
  
13. 在**RODC 選項**頁面 （，才會出現在您安裝 RODC）、 群組或使用者將管理 RODC、 新增或移除帳號，從 [允許] 或 [拒絕密碼複寫群組]，然後按一下帳號的名稱指定**下**。  
  
    如需詳細資訊，請查看[密碼複寫原則](https://technet.microsoft.com/library/cc730883(v=ws.10))。  
  
14. 在**的其他選項**頁面上，選擇下列其中一個選項：  
  
    -   如果您要建立新的網域中，輸入新的 NetBIOS 名稱或驗證預設 NetBIOS 網域名稱，然後按一下**下一步**。  
  
    -   如果您要加入的網域控制站現有的網域，選取您想要複寫 AD DS 安裝資料的 （或允許選取任何網域控制站精靈） 的網域控制站。 如果您從媒體安裝，請按一下**安裝媒體路徑的**輸入並確認安裝來源檔案的路徑，然後按一下 [**下**。  
  
        您無法使用安裝媒體 (IFM) 來安裝網域中的第一個網域控制站的。 IFM 跨不同的作業系統版本無法運作。 亦即，才能安裝其他網域控制站使用 IFM 執行 Windows Server 2012，您必須在 Windows Server 2012 網域控制站建立備份的媒體。 如需 IFM 的詳細資訊，請查看[安裝其他的網域控制站使用 IFM 的](https://technet.microsoft.com/library/cc816722(WS.10).aspx)。  
  
15. 在**路徑**頁面上，輸入 Active Directory 資料庫、 登入檔案，以及 SYSVOL 資料夾位置 （或接受預設的位置），按**下**。  
  
    > [!IMPORTANT]  
    > 不要使用復原檔案系統 (ReFS) 格式化是資料磁碟區上儲存的 Active Directory 資料庫、 登入檔案或 SYSVOL 資料夾。  
  
16. 在**準備選項**頁面上，輸入認證，才能執行 adprep 滿足。 如需詳細資訊，請查看[認證需求執行 Adprep.exe 並安裝 Active Directory Domain Services](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。  
  
17. 在**評論選項**頁面，確認您的選取項目，按**檢視指令碼**如果您想要的設定匯出 Windows PowerShell 指令碼，然後按一下 [**下一步**。  
  
18. 在**必要條件檢查**頁面，確認該必要條件驗證完成，然後按**安裝**。  
  
19. 在**結果**頁面上確認為網域控制站伺服器已經設定成功。 伺服器將會自動重新啟動完成 AD DS 安裝。  
  
## <a name="BKMK_UIStaged"></a>執行暫存 RODC 安裝使用圖形使用者介面  
階段的 RODC 安裝可讓您建立 RODC 兩個階段中。 在第一階段，網域管理群組成員建立 RODC account。 在第二個階段中，RODC account 附加伺服器。 第二個階段可以完成的網域管理群組或委派的網域使用者或群組成員。  
  
#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>若要使用的 Active Directory 管理工具建立 RODC account  
  
1.  您可以建立 RODC account 使用 Active Directory 管理中心 Active Directory 使用者或電腦。  
  
    1.  按一下**[開始]**，按一下 [**系統管理工具]**，然後按一下 [ **Active Directory 管理中心**。  
  
    2.  在瀏覽窗格中 （左窗格中），按一下 [的網域名稱。  
  
    3.  在 [管理清單 （中央窗格） 中，按一下 [**網域控制站**組織單位。  
  
    4.  在 [工作] 窗格上 （右窗格），按一下**預先建立唯讀網域控制站帳號**。  
  
    -或者-  
  
    1.  按一下**[開始]**，按一下**系統管理工具]**，然後按一下 [ **Active Directory 使用者與電腦**。  
  
    2.  任一以滑鼠右鍵按一下**網域控制站**單位 （組織單位） 或按一下**網域控制站**組織單位，然後按一下 [**動作**。  
  
    3.  按一下**預先建立唯讀網域控制站 account**。  
  
2.  上**歡迎 Active Directory Domain Services 安裝精靈**頁面上，如果您想要修改密碼複寫原則 (PRP)，選取的預設**進階模式安裝使用**，然後按一下 [**下一步**。  
  
3.  在**網路認證**頁面上，在**指定 account 認證使用來執行安裝**，按一下 [**我目前登入認證**，或按一下**其他憑證**，，然後按一下 [**設定**。 在**Windows 安全性**對話方塊方塊中，可以安裝其他網域控制站 account 提供使用者名稱和密碼。 若要安裝的其他網域控制站，您必須的企業系統管理員或網域管理群組成員。 當您完成提供的認證，請按**下一步**。  
  
4.  在**電腦名稱指定**頁面上，輸入電腦名稱會 RODC 的伺服器。  
  
5.  在**選擇網站**頁面上，從清單中選取網站或選取要安裝的網域控制站台中對應至執行精靈中，電腦的 IP 位址的選項，然後按一下**下**。  
  
6.  在**其他網域控制站選項**頁面，進行下列選項，然後再按一下**下**:  
  
    -   **DNS 伺服器**:，讓您的網域控制站可做的網域名稱系統 」 (DNS) 伺服器預設選取此選項。 如果您不想要的 DNS 伺服器的網域控制站，請清除此選項。 不過，如果您不要安裝 RODC 和 RODC DNS 伺服器角色是分公司只網域控制站、 分公司使用者將無法執行離線中樞網站的寬形區域網路 (WAN) 時的名稱解析。  
  
    -   **通用**： 預設選取此選項。 通用將磁碟分割唯讀 directory 網域控制站新增，它可以讓通用搜尋功能。 如果您不想要通用伺服器的網域控制站，請清除此選項。 不過，如果您不要安裝通用伺服器分公司或讓通用群組成員資格快取的網站，包括 RODC，使用者分公司中的將無法登入網域離線 WAN 中樞網站時。  
  
    -   **唯讀模式網域控制站**。 當您建立 RODC 帳號時，預設會選取此選項，您無法將它清除。  
  
7.  如果您選取 [**使用進階模式安裝**核取方塊**歡迎使用**頁面上，**指定密碼複寫原則**頁面隨即顯示。 根據預設，不 account 密碼會複寫 rodc，，安全性相關 （例如網域管理群組成員） 明確拒絕從得更容易遇到複製到 RODC 其密碼。  
  
    新增其他帳號原則，請按一下**新增**，然後按一下 [**允許複製到此 RODC account 的密碼**或按一下 [**複製到此 RODC 拒絕 account 的密碼**，然後選取 [帳號。  
  
    在完成 （或接受預設設定），按一下 [**下一步**。  
  
8.  在**RODC 委派安裝及管理**頁面上，輸入 [使用者或群組的人員將會伺服器附加至 RODC 帳號，您所建立的名稱。 您可以輸入只有一個的安全性原則的名稱。  
  
    若要搜尋 directory 特定的使用者或群組，請按一下**設定**。 在**選取使用者或群組**中，輸入名稱的使用者或群組。 我們建議您委派 RODC 安裝及管理到群組。  
  
    此使用者或群組也會有本機系統管理員權限在 RODC 之後安裝。 如果您不指定的使用者或群組，將無法伺服器附加至 account 只有的網域管理員群組或企業系統管理員群組成員。  
  
    當您完成時，請按**下一步**。  
  
9. 在**摘要**頁面上，檢視您的選擇。 按一下**回**若要變更任何選取項目。  
  
    若要儲存您選取的設定，您可以使用自動化後續 AD DS 作業，請按一下 [回應檔案**設定匯出**。 輸入您的回應檔案的名稱，然後按一下**儲存**。  
  
    當您確定您的選擇是否正確，請按**下一步**以建立 RODC 帳號。  
  
10. 在**完成 Active Directory Domain Services 安裝精靈**頁面上，按**完成]**。  
  
建立 RODC 帳號之後，您可以完成 RODC 安裝過去附加伺服器。 可以將位於 RODC 分公司完成此第二個階段。 您在執行此程序地方伺服器必須未加入網域。 從 Windows Server 2012 中，您使用新增角色精靈在伺服器管理員中 RODC 過去連接伺服器。  
  
#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>若要使用伺服器管理員 RODC 過去附加伺服器  
  
1.  本機系統管理員身分登入。  
  
2.  在伺服器管理員中，按一下**新增角色與功能**。  
  
3.  在**在您開始之前**頁面上，按一下 [**下**。  
  
4.  在**選擇安裝類型**頁面上，按一下 [**以角色為基礎，或為基礎的功能的安裝**，然後按一下 [**下一步**。  
  
5.  在**選取目的伺服器**頁面上，按一下 [**選取伺服器伺服器集區的**，按一下您要安裝 AD DS，然後按一下 [伺服器名稱**下一步**。  
  
6.  在**選擇伺服器角色**頁面上，按一下 [ **Active Directory Domain Services**，按一下 [**新增功能**，然後按一下**下一步**。  
  
7.  在**選擇功能**頁面上，選取您想要安裝按一下任何其他功能**下**。  
  
8.  在**Active Directory Domain Services**頁面上，檢視的資訊，然後按一下**下**。  
  
9. 在**確認安裝選項**頁面上，按**安裝**。  
  
10. 在**結果**頁面上確認**已成功安裝**，並按一下 [**此為網域控制站伺服器升級**開始 Active Directory Domain Services 組態精靈。  
  
    > [!IMPORTANT]  
    > 如果您不需要從 Active Directory Domain Services 組態精靈關閉新增角色精靈此時，您可以重新它，即可在伺服器管理員中的工作。  
  
    (media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)  
  
11. 在**部署設定**頁面上，按一下 [**現有的網域中加入的網域控制站**，輸入 (例如，emea.contoso.com) 的網域名稱和認證 （例如，指定委派給管理及安裝 RODC account），然後按一下 [**下一步**。  
  
12. 在**網域控制站選項**頁面上，按**使用現有 RODC account**、 輸入並確認 Directory 服務還原模式密碼，然後按一下**下一步**。  
  
13. 在**的其他選項**頁面上，如果您從媒體安裝按**安裝媒體路徑的**輸入並確認安裝來源檔案的路徑，然後選取您想要複寫 AD DS 安裝資料的 （或允許選取任何網域控制站精靈） 的網域控制站，然後按一下 [**下一步**。  
  
14. 在**路徑**頁面上，輸入位置 Active Directory 資料庫、 登入檔案，以及 SYSVOL 資料夾，或接受預設的位置，然後按一下**下**。  
  
15. 在**評論選項**頁面，確認您的選項，按一下 [**檢視指令碼**來設定匯出 Windows PowerShell 指令碼，然後按一下 [**下一步**。  
  
16. 在**必要條件檢查**頁面，確認該必要條件驗證完成，然後按**安裝**。  
  
    若要到 AD DS 安裝完成時，會自動重新伺服器。  
  
## <a name="see-also"></a>也了  
[疑難排解網域控制站部署](Troubleshooting-Domain-Controller-Deployment.md)  
[安裝新的 Windows Server 2012 Active Directory 森林與 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)  
[安裝新的 Windows Server 2012 Active Directory 子女或樹網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)  
[安裝複本 Windows Server 2012 網域控制站在現有的網域和 #40;層級 200 和 #41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)  
  



