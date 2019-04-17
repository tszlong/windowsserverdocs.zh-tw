---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: "安裝 Windows Server 2012 Active Directory Read-Only 網域控制站 (RODC) (層級 200)"
description: "本主題如何建立分段的 RODC 帳號，並 RODC 安裝期間然後附加至該 account 的伺服器。 此主題也如何安裝 RODC 不需要執行階段的安裝。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 78281ca845f79955aaa25aa45394284c59e639cb
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>安裝 Windows Server 2012 Active Directory Read-Only 網域控制站 (RODC) (層級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題如何建立分段的 RODC 帳號，並 RODC 安裝期間然後附加至該 account 的伺服器。 此主題也如何安裝 RODC 不需要執行階段的安裝。  
  
## <a name="stage-rodc-workflow"></a>階段 RODC 工作流程  
分段獨立的兩個階段中朗讀只網域控制站 (RODC) 安裝運作：  
  
1.  執行位置的電腦 account  
  
2.  升級時，將 RODC 附加至該 account  
  
下圖顯示 Active Directory Domain Services 唯讀網域控制站臨時程序地方 Active Directory 管理中心 (Dsac.exe) 網域中建立空 RODC 電腦帳號。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)  
  
## <a name="BKMK_StagePS"></a>階段 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|新增 addsreadonlydomaincontrolleraccount|-SkipPreChecks<br /><br />***-DomainControllerAccountName***<br /><br />***-網域名稱***<br /><br />***-站台名稱***<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />***認證***<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />*-NoGlobalCatalog*<br /><br />*-InstallDNS*<br /><br />-ReplicationSourceDC|  
  
> [!NOTE]  
> **-認證**引數只有需要如果您不已登入以網域管理群組成員。  
  
## <a name="attach-rodc-workflow"></a>連接 RODC 工作流程  
下圖顯示 Active Directory Domain Services 設定程序地方您已安裝的角色 AD DS，您暫存 RODC 帳號，並開始使用**這個網域控制站伺服器升級**，將它附加到分段的電腦 account 現有網域中建立新的 RODC 使用伺服器管理員。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)  
  
## <a name="BKMK_AttachPS"></a>連接 RODC Windows PowerShell  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|安裝-AddsDomaincontroller|-SkipPreChecks<br /><br />***-網域名稱***<br /><br />*-SafeModeAdministratorPassword*<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***認證***<br /><br />-CriticalReplicationOnly<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />*-InstallationMediaPath*<br /><br />*-LogPath*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />***-UseExistingAccount***|  
  
> [!NOTE]  
> **-認證**引數只有需要如果您不已登入以網域管理群組成員。  
  
## <a name="staging"></a>臨時  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)  
  
您執行的唯讀網域控制站電腦 account 臨時操作打開 Active Directory 管理中心 (**Dsac.exe**)。 按一下瀏覽窗格中的網域名稱。 按兩下**網域控制站**在 [管理] 清單。 按一下**預先建立唯讀網域控制站帳號**在 [工作] 窗格中。  
  
如需有關 Active Directory 管理中心的詳細資訊，請查看[進階 AD DS 使用 Active Directory 系統管理中心和 #40;層級 200 和 #41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)和檢視[Active Directory 管理中心： 開始](https://technet.microsoft.com/library/dd560651(WS.10).aspx)。  
  
如果您尚未建立唯讀網域控制站的體驗，您將會發現在安裝精靈具有相同圖形介面如下所示時使用的是較舊的 Active Directory 使用者與電腦嵌入式管理單元從 Windows Server 2008，並使用相同的程式碼，其中包括匯出設定中的過時帶領所使用的自動安裝檔案格式。  
  
Windows Server 2012 引入新 ADDSDeployment cmdlet 階段 RODC 電腦帳號，但精靈不使用 cmdlet 來執行作業。 下列區段會顯示等 cmdlet 和引數為了讓相關聯的資訊以了解每個變得更容易。  
  
**預先建立唯讀網域控制站帳號**連結在 Active Directory 管理中心的窗格相當於 ADDSDeployment Windows PowerShell cmdlet:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
### <a name="welcome"></a>歡迎使用  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)  
  
**歡迎 Active Directory Domain Services 安裝精靈**對話方塊有一個選項名為**進階模式安裝使用**。 選取這個選項，然後按一下**下一步**來顯示複寫密碼原則選項。 清除此選項以使用密碼 （這討論深入稍後在本區段中） 的複寫原則選項預設值。  
  
### <a name="network-credentials"></a>網路認證  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)  
  
網域名稱] 選項在**網路認證**對話方塊預設會顯示 [的 Active Directory 管理中心目標的網域。 預設會使用您目前的認證。 如果他們未包含成員資格群組網域系統管理員 」 中，按一下 [**替代認證**，按一下 [**設定**精靈提供使用者名稱與密碼網域系統管理員 」 的成員。  
  
相當於 ADDSDeployment Windows PowerShell 引數是：  
  
```  
-credential <pscredential>  
```  
  
請記住，臨時系統會直接從 Windows Server 2008 R2 連接埠，並不提供 Adprep 的新功能。 如果您打算部署分段的 RODC 帳號，您必須第一次部署未分段的 RODC 網域中，讓執行自動 rodcprep 作業，或手動執行 adprep.exe /rodcprep 第一次。  
  
否則，您會收到錯誤 「 您將無法安裝唯讀網域控制站在這個網域中，因為 「 adprep /rodcprep 」 尚未執行 」。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)  
  
### <a name="specify-the-computer-name"></a>指定的電腦名稱  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)  
  
**電腦名稱指定**對話方塊要求您輸入單一標籤**電腦名稱**的網域控制站不存在。 設定並附加此過去之後的網域控制站必須具有相同的名稱，或升級操作將不會偵測分段的 account。  
  
相當於 ADDSDeployment Windows PowerShell 引數是：  
  
```  
-domaincontrolleraccountname <string>  
```  
  
### <a name="select-a-site"></a>選取 [網站  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)  
  
**選擇網站**對話方塊中顯示目前的樹系的 Active Directory 網站清單。 分段唯讀網域控制站操作需要單一網站從清單中選取。 RODC 設定磁碟分割中建立它 NTDS 設定的物件，並加入本身正確的網站以開始之後部署第一次時使用此資訊。  
  
相當於 ADDSDeployment Windows PowerShell 引數是：  
  
```  
-sitename <string>  
```  
  
### <a name="additional-domain-controller-options"></a>其他網域控制站選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)  
  
**其他網域控制站選項**對話方塊可讓您在指定的包含執行為網域控制站**的 DNS 伺服器**並**通用**。 Microsoft 建議唯讀網域控制站提供 DNS 和 GC 服務，同時預設會安裝的;RODC 角色的一個用意是分公司案例，其中可能無法使用的寬形區域網路與這些 DNS 和通用服務、 分支中的電腦不能使用 AD DS 資源及功能。  
  
**唯讀網域控制站 (RODC)**選項預先選取且無法停用。 相當於 ADDSDeployment Windows PowerShell 引數︰  
  
```  
-installdns <string>  
-NoGlobalCatalog <{$true | $false}>  
  
```  
  
> [!NOTE]  
> 根據預設， **-NoGlobalCatalog**價值，是 $false，這表示如果您不指定引數網域控制站將會通用伺服器。  
  
### <a name="specify-the-password-replication-policy"></a>指定密碼複寫原則  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)  
  
**指定密碼原則複製**對話方塊，可讓您修改預設清單帳號，允許快取這個唯讀網域控制站其密碼。 在設定清單中的帳號**拒絕**或不在清單 （隱含） 不要快取他們的密碼。 帳號，不受允許快取 RODC 密碼和無法連接寫入網域控制站驗證無法存取資源或 Active Directory 所提供的功能。  
  
> [!IMPORTANT]  
> 精靈會顯示此對話方塊只是否您選取 [**使用進階模式安裝**核取方塊，在歡迎畫面。 如果您要清除此核取方塊，然後精靈會使用下列預設的群組及值：  
>   
> -   系統管理員-拒絕  
> -   伺服器電信業者-拒絕  
> -   備份電信業者-拒絕  
> -   考慮電信業者-拒絕  
> -   拒絕 RODC 密碼複寫群組-拒絕  
> -   允許 RODC 密碼複寫群組-允許  
  
相當於 ADDSDeployment Windows PowerShell 引數︰  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)  
  
### <a name="delegation-of-rodc-installation-and-administration"></a>RODC 安裝及管理委派  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)  
  
**RODC 委派安裝及管理**對話方塊，可讓您設定的使用者或群組包含伺服器連接 RODC 電腦過去允許的使用者。 按一下**設定**來瀏覽的使用者或群組的網域。 使用者或群組中此對話方塊提高可及範圍本機系統管理員權限 RODC 指定。 指定的使用者或群組成員可以執行的權限等於電腦的系統管理員群組 RODC 作業。 它們的*未*網域系統管理員 」 的網域建系統管理員群組成員。  
  
使用此選項分支 office 管理委派不分支系統管理員的資格授與的網域系統管理員 」 群組。 RODC 管理委派就不需要的。  
  
相當於 ADDSDeployment Windows PowerShell 引數是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
### <a name="summary"></a>摘要  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)  
  
**摘要**對話方塊，可讓您以確認您的設定。 這是一個停止安裝精靈會建立分段的 account 之前機會。 按一下**下一步**當您已經準備好建立分段的 RODC 電腦帳號。  按一下**匯出設定**以儲存回應檔案中的過時帶領自動安裝檔案格式。  
  
### <a name="creation"></a>建立  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)  
  
**Active Directory Domain Services 安裝精靈**在 Active Directory 中建立分段唯讀網域控制站。 開始後，您就無法取消這項操作。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)  
  
使用下列 cmdlet 階段使用 ADDSDeployment Windows PowerShell 模組唯讀網域控制站電腦 account:  
  
```  
Add-addsreadonlydomaincontrolleraccount  
  
```  
  
查看[階段 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS)選用和引數。  
  
因為**新增-addsreadonlydomaincontrolleraccount**只有一個動作以兩個階段 （必要條件檢查並安裝），下列螢幕擷取畫面顯示最低檔安裝階段。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)  
  
階段 RODC 作業建立 RODC 電腦 account Active Directory 中。 Active Directory 管理中心會顯示**網域控制站類型**為**位置網域控制站 Account**。 這個網域控制站類型表示分段的 RODC account 是供讀取只網域控制站附加至該伺服器。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)  
  
> [!IMPORTANT]  
> Active Directory 管理中心不再需要附加電腦 account 唯讀網域控制站伺服器。 使用伺服器管理員或 Active Directory Domain Services 組態精靈 ADDSDeployment Windows PowerShell 模組 cmdlet**安裝-AddsDomainController**新的 RODC 連接其分段過去。 步驟相當類似新增新的寫入網域控制站現有網域，除了分段的 RODC 電腦 account 包含認為您暫存 RODC 電腦 account 同時設定選項。  
  
## <a name="attaching"></a>附加  
  
### <a name="deployment-configuration"></a>部署設定  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
伺服器管理員會開始使用每個網域控制站升級**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。  
  
唯讀網域控制站加入現有的網域中，選取 [**現有的網域中加入的網域控制站**，按一下 [**選取 [**按鈕**指定這個網域的網域資訊**。 伺服器管理員自動提示您輸入有效的憑證，或者您可以按一下**變更**。  
  
附加 RODC 需要 Windows Server 2012 中網域管理員群組成員資格。 Active Directory Domain Services 組態精靈會提示您稍後如果您目前的認證，不需要的適當權限或群組成員資格。  
  
**部署組態**ADDSDeployment Windows PowerShell cmdlet 和引數：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>網域控制站選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)  
  
**網域控制站選項**頁面會顯示網域控制站新的網域控制站的選項。 這個頁面載入時，Active Directory Domain Services 組態精靈會傳送到現有的網域控制站檢查位置帳號 LDAP 查詢。 如果查詢位置的網域控制站尋找電腦的那個共用相同的名稱為目前的電腦，然後精靈會顯示在讀取頁面頂端的訊息]**存在於 directory 預先建立的 RODC account 符合 target 伺服器的名稱。選擇是否要使用此現有 RODC 帳號，或重新安裝這個網域控制站**。 」 精靈會使用**使用現有 RODC account**做為預設設定。  
  
> [!IMPORTANT]  
> 您可以使用**重新安裝這個網域控制站**選項時網域控制站發生問題實體無法傳回功能。 這也可以節省時間設定更換網域控制站保留網域控制站 account 電腦時，以及物件中繼資料在 Active Directory 中。 安裝新的電腦與*名稱相同*，並將它升級為網域控制站網域中。 **重新安裝這個網域控制站**選項，即表示如果您的網域控制站物件中繼資料移除 Active Directory （中繼資料清除）。  
  
當您正在附加伺服器 RODC 電腦過去，您無法設定網域控制站選項。 當您建立分段的 RODC 電腦帳號，您可以設定網域控制站選項。  
  
指定**Directory 服務還原模式密碼**必須遵守密碼原則套用到伺服器。 隨時複雜的密碼或最好複雜密碼。  
  
**網域控制站選項**ADDSDeployment Windows PowerShell 引數：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 網站名稱必須存在時引數提供**-站台名稱**。 **安裝-AddsDomainController** cmdlet 不會建立網站名稱。 您可以使用 cmdlet**新 adreplicationsite**來建立新的網站。  
  
**安裝-ADDSDomainController**如果您不指定引數請遵循相同的預設值為伺服器管理員。  
  
**SafeModeAdministratorPassword**引數的作業會特殊：  
  
-   如果*未指定*引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
    例如，建立新的 RODC corp.contoso.com，並提示您輸入並確認密碼遮罩：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="additional-options"></a>其他選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)  
  
**的其他選項**頁面上提供設定選項，來命名網域控制站為複寫來源，或您可以使用任何網域控制站為複寫來源。  
  
您也可以選擇備份使用安裝媒體 (IFM) 選項從媒體安裝網域控制站使用。 **安裝媒體的**核取方塊提供選取一次瀏覽] 選項，您必須按**驗證**以確保所提供有效的媒體。 從其他現有 Windows Server 2012 電腦; IFM 選項使用媒體建立與 Windows Server 備份或 Ntdsutil.exe您無法建立 Windows Server 2012 網域控制站媒體使用 Windows Server 2008 R2 或先前的作業系統。 如需變更 IFM 的詳細資訊，請查看[Ntdsutil.exe 安裝媒體變更的](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)。 如果使用的 media 受 SYSKEY，伺服器管理員會在驗證期間影像的密碼提示。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)  
  
**的其他選項**ADDSDeployment cmdlet 引數：  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路徑  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)  
  
**路徑**頁面上，可讓您覆寫預設資料夾位置的 AD DS 資料庫中資料庫交易登，並 SYSVOL 分享。 預設位置都之隱藏資料夾中。 **路徑**ADDSDeployment cmdlet 引數：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="review-options-and-view-script"></a>檢視選項]，然後檢視指令碼  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)  
  
**評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先其符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 此頁面上可讓您檢查並確認您的設定，才能繼續設定。 **評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。 此程序會建立進一步修改或直接使用有效且語法正確範例。 例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DomainName "corp.contoso.com" `  
-LogPath "C:\Windows\NTDS" `  
-SYSVOLPath "C:\Windows\SYSVOL" `  
-UseExistingAccount:$true `  
-Norebootoncompletion:$false  
-Force:$true  
  
```  
  
> [!NOTE]  
> 伺服器管理員通常會填入所有引升級後不會依賴預設值 （因為它們可能會改變之間未來版本 Windows 的 service pack） 的值。 有一個例外此**-safemodeadministratorpassword**引數。 若要強制確認提示忽略值執行 cmdlet 互動時  
  
使用選擇性**Whatif**以引數**安裝-ADDSDomainController** cmdlet 檢視設定的資訊。 這可讓您查看 cmdlet 引數明確和隱含的值。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)  
  
### <a name="prerequisites-check"></a>必要條件核取  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)  
  
**請必要條件**是 AD DS 網域設定中的新功能。 這個新階段驗證伺服器設定可以新 AD DS 樹系的支援。  
  
當您安裝新的樹系根網域，伺服器管理員 Active Directory Domain Services 組態精靈會叫用一系列序列化模組測試。 這些測試提醒建議的修復選項。 您可以視需要執行測試。 無法繼續網域控制站的安裝程序，直到所有必要條件測試傳遞。  
  
**請必要條件**也會呈現相關資訊，例如安全性變更會影響較舊的作業系統。 如需的必要條件檢查，請查看[必要條件檢查](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。  
  
您無法略過**必要條件檢查**時使用伺服器管理員中，但您可以跳過此程序使用 [使用下列引數 AD DS 部署 cmdlet 時：  
  
```  
-skipprechecks  
  
```  
  
> [!WARNING]  
> Microsoft 會阻礙重覆它會導致部分網域控制站升級或損壞 AD DS 森林略過必要條件檢查。  
  
按一下**安裝**若要開始網域控制站升級程序。 這是最後取消安裝的機會。 開始後，您就無法取消升級程序。 電腦將會在升級，無論促銷結果結尾自動重新開機。  
  
### <a name="installation"></a>安裝  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)  
  
[安裝] 頁面顯示時，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安裝新的 Active Directory 森林使用 ADDSDeployment 模組，使用下列 cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
查看[連接 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS)選用和引數。  
  
**安裝-addsdomaincontroller** cmdlet 僅有兩個階段 （必要條件檢查並安裝）。 下方的兩個圖表顯示安裝階段檔最低的**的網域名稱**， **-useexistingaccount**，並**-認證**。 請注意，就像伺服器管理員中，**安裝-ADDSDomainController** ，升級將會自動重新開機伺服器提醒您：  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)  
  
若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion**引數。  
  
> [!WARNING]  
> 覆寫在重新開機，建議。 網域控制站必須重新開機才能正確運作。  
  
### <a name="results"></a>結果  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 網域控制站將會自動重新開機之後 10 秒。  
  
## <a name="rodc-without-staging-workflow"></a>RODC 而不需要執行工作流程  
下圖顯示 Active Directory Domain Services 設定程序，當您之前已經安裝 AD DS 角色，以及您已經開始進行 Active Directory Domain Services 組態精靈使用現有的 Windows Server 2012 網域中建立新的非暫存唯讀網域控制站伺服器管理員。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)  
  
## <a name="rodc-without-staging-windows-powershell"></a>不需要執行 Windows PowerShell RODC  
  
|||  
|-|-|  
|**ADDSDeployment Cmdlet**|引數 (**粗體**所需的引數。 *斜體*引數可以使用 Windows PowerShell 或 AD DS 設定精靈指定。)|  
|安裝-AddsDomainController|-SkipPreChecks<br /><br />***-網域名稱***<br /><br />*-SafeModeAdministratorPassword*<br /><br />***-站台名稱***<br /><br />*-ApplicationPartitionsToReplicate*<br /><br />*-CreateDNSDelegation*<br /><br />***認證***<br /><br />*-CriticalReplicationOnly*<br /><br />*-DatabasePath*<br /><br />*-DNSDelegationCredential*<br /><br />-DNSOnNetwork<br /><br />*-InstallationMediaPath*<br /><br />*-InstallDNS*<br /><br />*-LogPath*<br /><br />-MoveInfrastructureOperationMasterRoleIfNecessary<br /><br />*-NoGlobalCatalog*<br /><br />-Norebootoncompletion<br /><br />*-ReplicationSourceDC*<br /><br />-SkipAutoConfigureDNS<br /><br />*-SystemKey*<br /><br />*-SYSVOLPath*<br /><br />*-AllowPasswordReplicationAccountName*<br /><br />*-DelegatedAdministratorAccountName*<br /><br />*-DenyPasswordReplicationAccountName*<br /><br />***-ReadOnlyReplica***|  
  
> [!NOTE]  
> **-認證**引數只有需要如果您不已登入以網域管理群組成員。  
  
## <a name="rodc-without-staging-deployment"></a>不臨時部署 RODC  
  
### <a name="deployment-configuration"></a>部署設定  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)  
  
伺服器管理員會開始使用每個網域控制站升級**部署組態**頁面。 剩餘的選項與所需的欄位變更此頁面上，後續的部署操作根據您選擇的頁面。  
  
未接移唯讀網域控制站加入現有的 Windows Server 2012 網域中，選取 [**現有的網域中加入的網域控制站**，按一下 [**選取 [**按鈕**指定這個網域的網域資訊**。 伺服器管理員自動提示您輸入有效的憑證，或者您可以按一下**變更**。  
  
附加 RODC 需要 Windows Server 2012 中網域管理員群組成員資格。 Active Directory Domain Services 組態精靈會提示您稍後如果您目前的認證，不需要的適當權限或群組成員資格。  
  
**部署組態**ADDSDeployment Windows PowerShell cmdlet 和引數：  
  
```  
Install-AddsDomainController  
-domainname <string>   
-credential <pscredential>  
```  
  
### <a name="domain-controller-options"></a>網域控制站選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)  
  
**網域控制站選項**頁面上指定的新的網域控制站的網域控制站功能。 可設定的網域控制站功能的**的 DNS 伺服器**，**通用**，並**唯讀網域控制站**。 Microsoft 建議所有網域控制站都提供 DNS 和 GC 服務的可用性分散式環境中。 GC 隨時選取預設，如果現有的網域主機已經在其 Dc DNS 根據授權起始查詢預設選取 DNS 伺服器。  
  
**網域控制站選項**頁面上也可讓您選擇適當的 Active Directory 邏輯**網站名稱**的樹系設定。 根據預設，它會選取最正確的子網路的網站。 只有一個網站時，它會選取該網站自動。  
  
> [!IMPORTANT]  
> 如果伺服器不屬於 Active Directory 子網路，而且有一個以上的 Active Directory 網站，就選取任何項目和**下一步**按鈕，即表示，直到您選擇的網站清單。  
  
指定**Directory 服務還原模式密碼**必須遵守密碼原則套用到伺服器。 隨時複雜的密碼或最好複雜密碼。**網域控制站選項**ADDSDeployment Windows PowerShell 引數：  
  
```  
-UseExistingAccount <{$true | $false}>  
-SafeModeAdministratorPassword <secure string>  
```  
  
> [!IMPORTANT]  
> 網站名稱必須存在時引數提供**-站台名稱**。 **安裝-AddsDomainController** cmdlet 不會建立網站名稱。 您可以使用 cmdlet**新 adreplicationsite**來建立新的網站。  
  
**安裝-ADDSDomainController**如果您不指定引數請遵循相同的預設值為伺服器管理員。  
  
**SafeModeAdministratorPassword**引數的作業會特殊：  
  
-   如果*未指定*引數，cmdlet 會提示您輸入並確認遮罩的密碼。 執行 cmdlet 互動時，這是慣用的使用方式。  
  
    例如，建立新的 RODC corp.contoso.com，並提示您輸入並確認密碼遮罩：  
  
    ```  
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)  
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
  
### <a name="rodc-options"></a>RODC 選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)  
  
**RODC 選項**頁面上，可讓您修改設定：  
  
-   委派的管理員  
  
-   帳號允許 rodc 複寫密碼  
  
-   無法從 rodc 複寫密碼帳號  
  
帳號委派的系統管理員取得 RODC 本機系統管理員權限。 這些使用者可以運作權限相當於在本機電腦的系統管理員」群組。  他們並不網域系統管理員」的網域建系統管理員群組成員。 這個選項適用於分支 office 的管理委派不提供網域系統管理員權限。 設定的管理委派就不需要的。  
  
相當於 ADDSDeployment Windows PowerShell 引數是：  
  
```  
-delegatedadministratoraccountname <string>  
```  
  
帳號，不受允許快取 RODC 密碼和無法連接寫入網域控制站驗證無法存取資源或 Active Directory 所提供的功能。  
  
> [!IMPORTANT]  
> 如果不修改，會使用的預設群組和設定：  
>   
> -   系統管理員-拒絕  
> -   伺服器電信業者-拒絕  
> -   備份電信業者-拒絕  
> -   考慮電信業者-拒絕  
> -   拒絕 RODC 密碼複寫群組-拒絕  
> -   允許 RODC 密碼複寫群組-允許  
  
相當於 ADDSDeployment Windows PowerShell 引數︰  
  
```  
-allowpasswordreplicationaccountname <string []>  
-denypasswordreplicationaccountname <string []>  
```  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)  
  
### <a name="additional-options"></a>其他選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)  
  
**的其他選項**頁面上提供設定選項，來命名網域控制站為複寫來源，或您可以使用任何網域控制站為複寫來源。  
  
您也可以選擇備份使用安裝媒體 (IFM) 選項從媒體安裝網域控制站使用。 **安裝媒體的**核取方塊提供選取一次瀏覽] 選項，您必須按**驗證**以確保所提供有效的媒體。 從其他現有 Windows Server 2012 電腦; IFM 選項使用媒體建立與 Windows Server 備份或 Ntdsutil.exe您無法建立 Windows Server 2012 網域控制站媒體使用 Windows Server 2008 R2 或先前的作業系統。  附錄 IFM 變更提供更多的資訊。 如果使用的 media 受 SYSKEY，伺服器管理員會在驗證期間影像的密碼提示。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)  
  
其他選項 ADDSDeployment cmdlet 引數︰  
  
```  
-replicationsourcedc <string>  
-installationmediapath <string>  
-systemkey <secure string>  
```  
  
### <a name="paths"></a>路徑  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)  
  
**路徑**頁面上，可讓您覆寫預設資料夾位置的 AD DS 資料庫中資料庫交易登，並 SYSVOL 分享。 預設位置都之隱藏資料夾中。 **路徑**ADDSDeployment cmdlet 引數：  
  
```  
-databasepath <string>  
-logpath <string>  
-sysvolpath <string>  
```  
  
### <a name="preparation-options"></a>準備選項  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)  
  
**準備選項**頁面上，系統會通知您 AD DS 設定，包括擴充架構 (forestprep) 及更新的網域 （準備網域）。 樹系或網域尚未準備手動執行 Adprep.exe 或上一個 Windows Server 2012 網域控制站安裝時，只會看到此頁面。 例如，Active Directory Domain Services 組態精靈會如果現有的 Windows Server 2012 樹系根網域中新增新的複本網域控制站隱藏此頁面。  
  
延伸架構和更新網域不會發生當您按一下**下一步**。 只有在安裝期間發生這些事件。 此頁面只要帶來在安裝之後會發生的事件有關感知。  
  
這個頁面也驗證的目前使用者的認證管理員架構與企業系統管理員群組成員您需要成員資格擴充架構或準備網域這些群組中。 按一下**變更**頁面會通知您目前的憑證，並不提供不足權限時，提供的適當的使用者的認證。  
  
其他選項 ADDSDeployment cmdlet 引數是：  
  
```  
-adprepcredential <pscredential>  
```  
  
> [!IMPORTANT]  
> 為使用舊版的 Windows Server、 Windows Server 2012 」 的網域自動的準備不會執行 GPPREP。 執行**adprep.exe /gpprep**以手動方式的所有先前已未準備適用於 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的網域。 您應該先執行一次 GPPrep 歷史不是每份升級與加入網域中。 Adprep.exe 不會執行 /gpprep 自動因為其作業都可能造成所有檔案和資料夾重新複寫所有網域控制站 SYSVOL 資料夾中。  
>   
> 當您升級第一階段未 RODC 網域中的，將會執行自動 RODCPrep。 不是當您第一次的寫入 Windows Server 2012 網域控制站升級。 您也仍然以手動方式可以執行**adprep.exe /rodcprep**如果您要部署唯讀網域控制站計劃。  
  
### <a name="review-options-and-view-script"></a>檢視選項]，然後檢視指令碼  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)  
  
**評論選項**頁面上可讓您驗證您的設定，並確保您開始安裝之前，先其符合您的需求。 這不是一個機會停止使用伺服器管理員安裝。 此頁面上可讓您檢查並確認您的設定，才能繼續設定。  
  
**評論選項**在伺服器管理員頁面也提供選擇性**檢視指令碼**按鈕，以建立包含目前 ADDSDeployment 設定成單一的 Windows PowerShell 指令碼 Unicode 文字檔案。 這可讓您在伺服器管理員圖形介面作為 Windows PowerShell 部署 studio。 若要設定選項，匯出設定，然後取消精靈使用 Active Directory Domain Services 組態精靈。 此程序會建立進一步修改或直接使用有效且語法正確範例。 例如：  
  
```  
#  
# Windows PowerShell Script for AD DS Deployment  
#  
  
Import-Module ADDSDeployment  
Install-ADDSDomainController `  
-AllowPasswordReplicationAccountName @("CORP\Allowed RODC Password Replication Group", "CORP\Chicago RODC Admins", "CORP\Chicago RODC Users and Computers") `  
-Credential (Get-Credential) `  
-CriticalReplicationOnly:$false `  
-DatabasePath "C:\Windows\NTDS" `  
-DelegatedAdministratorAccountName "CORP\Chicago RODC Admins" `  
-DenyPasswordReplicationAccountName @("BUILTIN\Administrators", "BUILTIN\Server Operators", "BUILTIN\Backup Operators", "BUILTIN\Account Operators", "CORP\Denied RODC Password Replication Group") `  
-DomainName "corp.contoso.com" `  
-InstallDNS:$true `  
-LogPath "C:\Windows\NTDS" `  
-ReadOnlyReplica:$true `  
-SiteName "Default-First-Site-Name" `  
-SYSVOLPath "C:\Windows\SYSVOL"  
-Force:$true  
  
```  
  
> [!NOTE]  
> 伺服器管理員通常會填入所有引升級後不會依賴預設值 （因為它們可能會改變之間未來版本 Windows 的 service pack） 的值。 有一個例外此**-safemodeadministratorpassword**引數。 若要強制確認的提示，請執行 cmdlet 互動時省略值。  
  
安裝-ADDSDomainController cmdlet 選擇性 Whatif 引數使用檢視設定的資訊。 這可讓您查看 cmdlet 引數明確和隱含的值。  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)  
  
### <a name="prerequisites-check"></a>必要條件核取  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)  
  
**請必要條件**是 AD DS 網域設定中的新功能。 這個新階段驗證伺服器設定可以新 AD DS 樹系的支援。  
  
當您安裝新的樹系根網域，伺服器管理員 Active Directory Domain Services 組態精靈會叫用一系列序列化模組測試。 這些測試提醒建議的修復選項。 您可以視需要執行測試。 無法繼續網域控制站程序，直到所有必要條件測試傳遞。  
  
**請必要條件**也會呈現相關資訊，例如安全性變更會影響較舊的作業系統。  
  
您無法略過**必要條件檢查**時使用伺服器管理員中，但您可以跳過此程序使用 [使用下列引數 AD DS 部署 cmdlet 時：  
  
```  
-skipprechecks  
  
```  
  
按一下**安裝**若要開始網域控制站升級程序。 這是最後取消安裝的機會。 開始後，您就無法取消升級程序。 電腦將會在升級，無論促銷結果結尾自動重新開機。  
  
### <a name="installation"></a>安裝  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)  
  
當**安裝**頁面會顯示，網域控制站設定開始和無法終止或取消。 詳細的作業會顯示在此頁面上，而且寫入登：  
  
-   %systemroot%\debug\dcpromo.log  
  
-   %systemroot%\debug\dcpromoui.log  
  
若要安裝新的 Active Directory 森林使用 ADDSDeployment 模組，使用下列 cmdlet:  
  
```  
Install-addsdomaincontroller  
  
```  
  
查看**ADDSDeployment Cmdlet**在此區域的選擇性和引數 begininng 表。  
  
**安裝-addsdomaincontroller** cmdlet 僅有兩個階段 （必要條件檢查並安裝）。 下方的兩個圖表顯示安裝階段檔最低的**的網域名稱**， **-readonlyreplica**、 **-站台名稱**，和**-credential**。 請注意，就像伺服器管理員中，**安裝-ADDSDomainController** ，升級將會自動重新開機伺服器提醒您：  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)  
  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)  
  
若要自動接受重新開機命令提示字元中，使用**-強制**或**-確認： $false**的任何 ADDSDeployment Windows PowerShell cmdlet 引數。 若要防止伺服器促銷結尾自動重新開機，使用**-norebootoncompletion**引數。  
  
> [!WARNING]  
> 建議您不要覆寫在重新開機。 網域控制站必須重新開機才能正確運作。 如果您的網域控制站關閉登入，您無法登入互動方式直到您重新開機。  
  
### <a name="results"></a>結果  
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)  
  
**結果**頁面會顯示成功或失敗的升級與管理的任何重要資訊。 網域控制站將會自動重新開機之後 10 秒。  
  

