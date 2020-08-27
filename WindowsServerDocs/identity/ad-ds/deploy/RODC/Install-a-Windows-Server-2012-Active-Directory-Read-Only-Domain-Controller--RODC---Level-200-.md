---
ms.assetid: 66fa945e-598d-4f18-b603-97a39ce0d836
title: 安裝 Windows Server 2012 Active Directory 唯讀網域控制站 (RODC) (等級 200)
description: 本主題說明如何建立執行的 RODC 帳戶，並在 RODC 安裝期間將伺服器連結至該帳戶。 本主題也說明如何不透過執行階段式安裝來安裝 RODC。
author: iainfoulds
ms.author: iainfou
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a0c800d975b36f92d5b4bcf1801d06897cbefac3
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941598"
---
# <a name="install-a-windows-server-2012-active-directory-read-only-domain-controller-rodc-level-200"></a>安裝 Windows Server 2012 Active Directory 唯讀網域控制站 (RODC) (等級 200)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明如何建立執行的 RODC 帳戶，並在 RODC 安裝期間將伺服器連結至該帳戶。 本主題也說明如何不透過執行階段式安裝來安裝 RODC。

## <a name="stage-rodc-workflow"></a>RODC 工作流程階段
階段式唯讀網域控制站 (RODC) 安裝分成兩個不同的階段：

1.  執行未使用的電腦帳戶

2.  在升級期間將 RODC 連結至該帳戶

下圖說明使用 Active Directory 管理中心 (Dsac.exe) 在網域中建立空白的 RODC 電腦帳戶的 Active Directory 網域服務唯讀網域控制站暫存處理程序。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stagedcreation.png)

## <a name="stage-rodc-windows-powershell"></a><a name=BKMK_StagePS></a>RODC Windows PowerShell 階段

| ADDSDeployment Cmdlet | 引數 (**粗體**的引數是必要的。 *斜體*的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| Add-addsreadonlydomaincontrolleraccount | -SkipPreChecks<p>***-DomainControllerAccountName***<p>***-DomainName***<p>***-SiteName***<p>*-AllowPasswordReplicationAccountName*<p>***-Credential***<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>*-NoGlobalCatalog*<p>*-InstallDNS*<p>-ReplicationSourceDC |

> [!NOTE]
> 當您不是以 Domain Admins 群組成員的身分登入時，才需要 **-credential** 引數。

## <a name="attach-rodc-workflow"></a>連結 RODC 工作流程
下圖說明 Active Directory 網域服務設定流程，而您已安裝 AD DS 角色、執行 RODC 帳戶並已使用伺服器管理員啟動 [將此伺服器升級為網域控制站]**** 以在現有的網域中建立新的 RODC，並將其連結至執行的電腦帳戶。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_stageddeploy_beta1.png)

## <a name="attach-rodc-windows-powershell"></a><a name=BKMK_AttachPS></a>連結 RODC Windows PowerShell

| ADDSDeployment Cmdlet | 需要 (**粗體** 引數的引數。 *斜體*的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| Install-AddsDomaincontroller | -SkipPreChecks<p>***-DomainName***<p>*-SafeModeAdministratorPassword*<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>-CriticalReplicationOnly<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>*-InstallationMediaPath*<p>*-LogPath*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>*-SystemKey*<p>*-SYSVOLPath*<p>***-UseExistingAccount*** |

> [!NOTE]
> 當您不是以 Domain Admins 群組成員的身分登入時，才需要 **-credential** 引數。

## <a name="staging"></a>預備
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_PreCreateRODC.png)

開啟 [Active Directory 管理中心] (**Dsac.exe**) 以執行唯讀網域控制站電腦帳戶的執行操作 。 按一下瀏覽窗格中的網域名稱。 按兩下管理清單中的 [網域控制站]****。 按一下工作窗格中的 [預先建立唯讀網域控制站帳戶]****。

如需有關 Active Directory 管理中心的詳細資訊，請參閱 [使用 Active Directory 管理中心 &#40;層級200的 Advanced AD DS 管理&#41;](../../../ad-ds/get-started/adac/Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md) 和審核 [Active Directory 管理中心：消費者入門](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd560651(v=ws.10))。

如果您曾經建立過唯讀網域控制站，您會發現安裝精靈的圖形化介面和從 Windows Server 2008 使用舊版 [Active Directory 使用者和電腦] 嵌入式管理單元時所見的相同，並且使用相同的程式碼，其中包括匯出過時的 dcpromo 所使用的自動安裝檔案格式中的設定。

Windows Server 2012 使用新的 ADDSDeployment Cmdlet 以執行 RODC 電腦帳戶，但精靈不會使用該 Cmdlet 來執行作業。 下列各節顯示對等的 Cmdlet 與引數，讓彼此關聯的資訊更易於了解。

Active Directory 管理中心工作窗格中的 [ **預先建立唯讀網域控制站帳戶** ] 連結相當於 ADDSDeployment Windows PowerShell Cmdlet：

```
Add-addsreadonlydomaincontrolleraccount

```

### <a name="welcome"></a>歡迎使用
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_WelcomeStage1.png)

[歡迎使用 Active Directory 網域服務安裝精靈]**** 對話方塊有一個名為 [使用進階模式安裝]**** 的選項。 選取此選項，然後按 [下一步]**** 會顯示密碼複寫原則選項。 清除此選項可使用密碼複寫原則選項的預設值 (本節稍後將進一步詳細探討)。

### <a name="network-credentials"></a>網路認證
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Creds.png)

[網路認證]**** 對話方塊中的網域名稱選項會顯示 Active Directory 管理中心預設的目標網域。 預設會使用您目前的認證。 如果它們未包含 Domain Admins 群組中的成員資格，請按一下 [備用認證]****，再按一下 [設定]****，以提供精靈屬於 Domain Admins 成員的使用者名稱密碼。

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-credential <pscredential>
```

請記住，暫存系統是 Windows Server 2008 R2 的直接連接埠，並不提供新的 Adprep 功能。 如果您計劃部署執行的 RODC 帳戶，您必須先在該網域部署未執行的 RODC 以使自動 rodcprep 作業執行，或是先手動執行 adprep.exe /rodcprep。

否則，您將會收到錯誤，因為 adprep/rodcprep 尚未執行，所以您將無法在此網域中安裝唯讀網域控制站。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepNotRunError.png)

### <a name="specify-the-computer-name"></a>指定電腦名稱
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1CompName.png)

[指定電腦名稱]**** 對話方塊會要求您輸入不存在的網域控制站的單一標籤 [電腦名稱]****。 您設定的網域控制站和稍後連結至此帳戶的網域控制站必須是相同名稱，否則升級作業將無法偵測執行的帳戶。

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-domaincontrolleraccountname <string>
```

### <a name="select-a-site"></a>選取站台
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Site.png)

[選取站台]**** 對話方塊會顯示目前樹系的 Active Directory 站台清單。 執行的唯讀網域控制站作業會要求您從清單中選取一個站台。 RODC 會使用此資訊在設定磁碟分割中建立其 NTDS 設定物件，然後在部署後第一次啟動時自行加入正確的網站。

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-sitename <string>
```

### <a name="additional-domain-controller-options"></a>其他網域控制站選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DCOptions.png)

[其他網域控制站選項]**** 對話方塊可讓您指定網域控制站執行為 [DNS 伺服器]**** 和包含 [通用類別目錄]****。 Microsoft 建議唯讀網域控制站提供 DNS 與 GC 服務，因此預設會安裝這兩者；使用 RODC 角色的意義是，在分公司案例中，廣域網路可能無法使用，而若沒有這些 DNS 和通用類別目錄服務，分公司中的電腦將無法使用 AD DS 資源及功能。

[唯讀網域控制站 (RODC)]**** 是預先選取的選項，而且無法停用。 對等的 ADDSDeployment Windows PowerShell 引數為：

```
-installdns <string>
-NoGlobalCatalog <{$true | $false}>

```

> [!NOTE]
> 根據預設， **-NoGlobalCatalog** 值為 $false，這表示如果未指定引數，網域控制站將會是通用類別目錄伺服器。

### <a name="specify-the-password-replication-policy"></a>指定密碼複寫原則
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRP.png)

[指定密碼複寫原則]**** 對話方塊可讓您修改允許在此唯讀網域控制站上快取其密碼的預設帳戶清單。 在清單中設定為 [拒絕]**** 或不在清單 (隱含) 的帳戶則無法快取其密碼。 不允許在 RODC 上快取密碼且無法連線可寫入網域控制站並驗證其身分的帳戶，無法存取 Active Directory 所提供的資源或功能。

> [!IMPORTANT]
> 只有當您在歡迎畫面上選取 [使用進階模式安裝]**** 核取方塊時，精靈才會顯示這個對話方塊。 如果您清除此核取方塊，精靈會使用下列預設群組和值：
>
> -   Administrators - 拒絕
> -   Server Operators - 拒絕
> -   Backup Operators - 拒絕
> -   Account Operators - 拒絕
> -   拒絕的 RODC 密碼複寫群組 - 拒絕
> -   允許的 RODC 密碼複寫群組 - 允許

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1PRPAllow.png)

### <a name="delegation-of-rodc-installation-and-administration"></a>RODC 安裝與管理的委派
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1DelegateAdmin.png)

[RODC 安裝與管理的委派]**** 對話方塊可讓您設定使用者或設定包含可將伺服器連結至 RODC 電腦帳戶的群組。 按一下 [設定]**** 以瀏覽網域的使用者或群組。 在此對話方塊中指定的使用者或群組會獲得 RODC 的本機系統管理權限。 指定的使用者或指定群組的成員可以在 RODC 上執行與電腦的 Administrators 群組相等的作業。 他們*不是* Domain Admins 群組的成員或網域內建的 Administrators 群組的成員。

使用這個選項可委派分公司的管理工作，而不需要將分公司系統管理員成員資格授與 Domain Admins 群組。 委派 RODC 管理工作不是必要的。

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-delegatedadministratoraccountname <string>
```

### <a name="summary"></a>摘要
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Summary.png)

[摘要]**** 對話方塊可讓您確認您的設定。 這是在精靈建立執行帳戶前可停止安裝的最後機會。 當您準備好建立執行的 RODC 電腦帳戶時，按一下 [下一步]****。  按一下 [匯出設定]**** 可以過時的 dcpromo 自動安裝檔案格式儲存回應檔案。

### <a name="creation"></a>建立
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1InstallProgress.png)

[Active Directory 網域服務安裝精靈]**** 會在 Active Directory 中建立執行的唯讀網域控制站。 這項操作一旦開始就無法取消。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage1Complete.png)

使用下列 Cmdlet 可利用 ADDSDeployment Windows PowerShell 模組來執行唯讀網域控制站電腦帳戶：

```
Add-addsreadonlydomaincontrolleraccount

```

如需必要引數和選擇性引數的詳細資訊，請參閱[RODC Windows PowerShell 階段](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_StagePS)。

因為 **Add-addsreadonlydomaincontrolleraccount** 只有一個包含兩個階段 (先決條件檢查與安裝) 的動作，下列螢幕擷取畫面顯示含有基本必要引數的安裝階段。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODC.png)

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSAddRODCValidating.png)

RODC 作業階段會在 Active Directory 中建立 RODC 電腦帳戶。 Active Directory 管理中心顯示 [網域控制站類型]**** 為 [未使用的網域控制站帳戶]****。 此網域控制站類型表示執行的 RODC 帳戶已準備好連結伺服器以做為唯讀的網域控制站。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Unoccupied.png)

> [!IMPORTANT]
> 要將伺服器連結至唯讀的網域控制站電腦帳戶，已不再需要 Active Directory 管理中心。 使用伺服器管理員和 Active Directory 網域服務設定精靈或 ADDSDeployment Windows PowerShell 模組 Cmdlet **Install-AddsDomainController**，即可將新的 RODC 連結至其執行的帳戶。 除了執行的 RODC 電腦帳戶包含您執行 RODC 電腦帳戶時決定的設定選項以外，步驟類似於將新的可寫入網域控制站加入現有的網域。

## <a name="attaching"></a>正在附加

### <a name="deployment-configuration"></a>部署組態
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

[伺服器管理員] 會從 [部署設定]**** 頁面開始升級每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。

如果要將唯讀網域控制站新增至現有網域中，請選取 [新增現有網域的網域控制站]，**** 然後依序按一下 [選取]**** 按鈕、[提供此網域的網域資訊]****。 伺服器管理員會自動提示您輸入有效的認證，或者您可以按一下 [變更]****。

連結 RODC 需要 Windows Server 2012 中 Domain Admins 群組的成員資格。 如果您目前的認證不具有適當的權限或群組成員資格，稍後 [Active Directory 網域服務設定精靈] 會提示您。

部署設定**** ADDSDeployment Windows PowerShell Cmdlet 與引數為：

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>網域控制站選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2DCOptions.png)

[網域控制站選項]**** 頁面顯示新網域控制站的網域控制站選項。 當這個頁面載入時，Active Directory 網域服務設定精靈會將 LDAP 查詢傳送到現有的網域控制站，以檢查是否有未使用的帳戶。 如果查詢尋找的非佔用網域控制站電腦帳戶與目前的電腦共用相同的名稱，則嚮導會在頁面頂端顯示一則資訊訊息，以讀取 **與目錄中存在的目標伺服器名稱相符之預先建立的 RODC 帳戶。選擇是要使用現有的 RODC 帳戶，還是重新安裝此網域控制站**。 精靈會使用 [使用現有的 RODC 帳戶]**** 做為預設設定。

> [!IMPORTANT]
> 當網域控制站發生實體問題而無法回復功能時，您可以使用 [重新安裝此網域控制站]****。 這可節省設定取代網域控制站的時間，因為能在 Active Directory 中保留網域控制站電腦帳戶與物件中繼資料。 使用*相同的名稱*安裝新的電腦，並將它升級為網域中的網域控制站。 如果您已從 Active Directory (中繼資料清除) 移除網域控制站物件的中繼資料，則無法使用 [ **重新安裝此網域控制站** ] 選項。

當您將伺服器連結至 RODC 電腦帳戶時，無法設定網域控制站選項。 您要在建立執行的 RODC 電腦帳戶時設定網域控制站選項。

指定的 [目錄服務還原模式密碼]**** 必須遵守套用至伺服器的密碼原則。 務必選擇複雜的強式密碼，或者最好是使用複雜密碼。

網域控制站選項**** ADDSDeployment Windows PowerShell 引數為：

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> 提供站台名稱做為 **-sitename** 引數的時候，它必須已經存在。 **install-AddsDomainController** Cmdlet 不會建立站台名稱。 您可以使用 Cmdlet **new-adreplicationsite** 來建立新的站台。

如未指定，**Install-ADDSDomainController** 引數會使用與伺服器管理員相同的預設值。

**SafeModeAdministratorPassword** 引數的操作方式比較特殊：

-   如果*沒有指定*為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。

    例如，在 corp.contoso.com 中建立新的 RODC，並提示輸入和確認不顯示字元的密碼：

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   如果*使用值*指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以使用 **Read-Host** Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串：

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> 因為前一個選項不會確認密碼，所以請務必小心使用：密碼是看不到的。

您也可以提供轉換的純文字變數當做安全字串，不過我們不鼓勵這種做法。

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

最後，您可以將模糊化密碼儲存到檔案中以在稍後重複使用，而不顯示純文字密碼。 例如：

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> 不建議提供或儲存純文字或模糊化的密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。  任何能夠存取該檔案的人都可以回復模糊化的密碼。 一旦具備該知識，他們可以登入以 DSRM 啟動的網域控制站，最終模擬網域控制站本身，並將他們在 AD 樹系中的權限提升至最高。 有一組建議的額外步驟使用 **System.Security.Cryptography** 將文字檔資料加密，不過這不在本文的討論範圍內。 最佳做法是完全避免儲存密碼。

### <a name="additional-options"></a>其他選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2AdditionalOptions.png)

[其他選項]**** 頁面提供可將網域控制站指定為複寫來源的設定選項，或者您可以使用任一網域控制站做為複寫來源。

您也可以使用「從媒體安裝」(IFM) 選項，使用備份的媒體來安裝網域控制站。 選取 [從媒體安裝]**** 核取方塊時會提供瀏覽選項，您必須按一下 [驗證]**** 以確保提供的路徑是有效的媒體。

IFM 來源的指導方針：
*    IFM 選項所使用的媒體是使用與相同作業系統版本之另一個現有 Windows Server 網域控制站的 Windows Server Backup 或 Ntdsutil.exe 所建立。 例如，您不能使用 Windows Server 2008 R2 或舊版作業系統來建立 Windows Server 2012 網域控制站的媒體。
*    IFM 來源資料應來自可寫入的網域控制站。 雖然來自 RODC 的來源會在技術上用來建立新的 RODC，但是有誤報來源 RODC 不會複寫的錯誤正面複寫警告。

如需 IFM 變更的相關資訊，請參閱[透過媒體變更的 Ntdsutil.exe 安裝](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)。 如果使用以 SYSKEY 保護的媒體，伺服器管理員在驗證期間會提示輸入映像的密碼。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_StagedIFM.png)

[其他選項]**** ADDSDeployment Cmdlet 引數是：

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>路徑
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Paths.png)

[路徑]**** 頁面能讓您覆寫 AD DS 資料庫、資料庫交易記錄以及 SYSVOL 共用的預設資料夾位置。 預設位置一律是在 %systemroot% 的子目錄中。 [路徑]**** ADDSDeployment Cmdlet 引數是：

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="review-options-and-view-script"></a>檢閱選項和檢視指令碼
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2ReviewOptions.png)

[檢閱選項]**** 頁面能讓您驗證設定，並確保它們符合您的需求，然後才開始安裝。 這不是使用 [伺服器管理員] 停止安裝的最後機會。 這個頁面只是讓您檢閱並確認設定，然後才繼續設定。 [伺服器管理員] 中的 [檢閱選項]**** 頁面也提供選用的 [檢視指令碼]**** 按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。 這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。 例如：

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DomainName corp.contoso.com `
-LogPath C:\Windows\NTDS `
-SYSVOLPath C:\Windows\SYSVOL `
-UseExistingAccount:$true `
-Norebootoncompletion:$false
-Force:$true

```

> [!NOTE]
> [伺服器管理員] 通常會在升級時填入所有引數的值，並不會依賴預設值 (因為它們在未來的 Windows 版本或 Service Pack 中可能會變更)。 **-safemodeadministratorpassword** 引數是個例外。 以互動方式執行 Cmdlet 時強制確認提示省略值

使用選擇性的 **Whatif** 引數搭配 **Install-ADDSDomainController** Cmdlet 來檢閱設定資訊。 這可讓您看到 Cmdlet 引數的明確值和隱含值。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2WhatIf.png)

### <a name="prerequisites-check"></a>先決條件檢查
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2PrereqCheck.png)

[先決條件檢查]**** 是 AD DS 網域設定中的新功能。 這個新階段會驗證伺服器設定是否能夠支援新的 AD DS 樹系。

安裝新的樹系根網域時，[伺服器管理員] 的 [Active Directory 網域服務設定精靈] 會叫用一系列序列化的模組化測試。 這些測試會提醒您建議的修復選項。 您可以視需要執行多次測試。 必須通過所有的先決條件測試，網域控制站安裝程序才能繼續。

[先決條件檢查]**** 也會提供諸如影響舊版作業系統之安全性變更的相關資訊。 如需先決條件檢查的詳細資訊，請參閱[先決條件檢查](../../../ad-ds/manage/AD-DS-Simplified-Administration.md#BKMK_PrereuisiteChecking)。

使用 [伺服器管理員] 時無法略過 [先決條件檢查]****，但您可以在使用 AD DS 部署 Cmdlet 時使用下列引數略過該程序：

```
-skipprechecks

```

> [!WARNING]
> Microsoft 建議您不要略過先決條件檢查，因為這樣可能會導致網域控制站升級不完整或 AD DS 樹系損毀。

按一下 [安裝]**** 以開始網域控制站升級程序。 這是取消安裝的最後機會。 升級程序一旦開始就無法取消。 不論升級結果如何，升級結束時電腦都會自動重新開機。

### <a name="installation"></a>安裝
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_Stage2Installation.png)

顯示 [安裝] 頁面時，網域控制站設定程序就開始執行，而且無法暫停或取消。 詳細的作業會顯示此頁面上，而且會寫入到記錄檔：

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

如果要使用 ADDSDeployment 模組安裝新的 Active Directory 樹系，請使用下列 Cmdlet：

```
Install-addsdomaincontroller

```

如需必要引數和選擇性引數的詳細資訊，請參閱[連結 RODC Windows PowerShell](../../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md#BKMK_AttachPS)。

**Install-addsdomaincontroller** Cmdlet 只有兩個階段 (先決條件檢查與安裝)。 下列兩個圖形顯示使用 **-domainname**、**-useexistingaccount** 及 **-credential** 的基本必要引數的安裝階段。 請注意 **Install-ADDSDomainController** 如何提醒您升級會自動將伺服器重新開機 (和伺服器管理員一樣)：

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2.png)

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSStage2Complete.png)

若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion** 引數。

> [!WARNING]
> 建議您不要覆寫重新開機設定。 網域控制站必須重新開機才能正常運作。

### <a name="results"></a>結果
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_ForestSignOff.png)

[結果]**** 頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 網域控制站會在 10 秒後自動重新開機。

## <a name="rodc-without-staging-workflow"></a>沒有執行工作流程的 RODC
下圖說明已安裝過 AD DS 角色，並且使用伺服器管理員啟動 Active Directory 網域服務設定精靈以在現有的 Windows Server 2012 網域中建立新的網域控制站時的 Active Directory 網域服務設定程序。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/adds_rodcdeploy.png)

## <a name="rodc-without-staging-windows-powershell"></a>沒有執行 Windows PowerShell 的 RODC

| ADDSDeployment Cmdlet | 需要 (**粗體** 引數的引數。 *斜體*的引數可以使用 Windows PowerShell 或 [AD DS 設定精靈] 來指定。) |
|--|--|
| Install-AddsDomainController | -SkipPreChecks<p>***-DomainName***<p>*-SafeModeAdministratorPassword*<p>***-SiteName***<p>*-ApplicationPartitionsToReplicate*<p>*-CreateDNSDelegation*<p>***-Credential***<p>*-CriticalReplicationOnly*<p>*-DatabasePath*<p>*-DNSDelegationCredential*<p>-DNSOnNetwork<p>*-InstallationMediaPath*<p>*-InstallDNS*<p>*-LogPath*<p>-MoveInfrastructureOperationMasterRoleIfNecessary<p>*-NoGlobalCatalog*<p>-Norebootoncompletion<p>*-ReplicationSourceDC*<p>-SkipAutoConfigureDNS<p>*-SystemKey*<p>*-SYSVOLPath*<p>*-AllowPasswordReplicationAccountName*<p>*-DelegatedAdministratorAccountName*<p>*-DenyPasswordReplicationAccountName*<p>***-ReadOnlyReplica*** |

> [!NOTE]
> 當您不是以 Domain Admins 群組成員的身分登入時，才需要 **-credential** 引數。

## <a name="rodc-without-staging-deployment"></a>沒有執行部署的 RODC

### <a name="deployment-configuration"></a>部署組態
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDeployConfig.png)

[伺服器管理員] 會從 [部署設定]**** 頁面開始升級每個網域控制站。 這個頁面及後續頁面的剩餘選項及必要欄位會隨著您選取的部署操作而變更。

如果要將未執行的唯讀網域控制站新增至現有的 Windows Server 2012 網域中，請選取 [新增現有網域的網域控制站]，**** 然後依序按一下 [選取]**** 按鈕、[提供此網域的網域資訊]****。 伺服器管理員會自動提示您輸入有效的認證，或者您可以按一下 [變更]****。

連結 RODC 需要 Windows Server 2012 中 Domain Admins 群組的成員資格。 如果您目前的認證不具有適當的權限或群組成員資格，稍後 [Active Directory 網域服務設定精靈] 會提示您。

部署設定**** ADDSDeployment Windows PowerShell Cmdlet 與引數為：

```
Install-AddsDomainController
-domainname <string>
-credential <pscredential>
```

### <a name="domain-controller-options"></a>網域控制站選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCDCOptions.png)

[網域控制站選項]**** 頁面指定新網域控制站的網域控制站功能。 可設定的網域控制站功能包括 [DNS 伺服器]**** 和 [通用類別目錄]****，以及 [唯讀網域控制站]****。 Microsoft 建議所有網域控制站都提供 DNS 與 GC 服務，以在分散式環境中獲得高可用性。 GC 一律是預設選項，而如果目前的網域已在以起始授權 (SOA) 查詢為基礎的網域控制站上代管 DNS，則預設會選取 DNS 伺服器。

[網域控制站選項]**** 頁面也能讓您從樹系設定選擇適當的 Active Directory 邏輯 [站台名稱]****。 它預設會選取包含最正確子網路的站台。 如果只有一個站台，就會自動選取該站台。

> [!IMPORTANT]
> 如果伺服器不屬於某個 Active Directory 子網路且有一個以上的 Active Directory 站台，則不會選取任何站台，而且會等到您從清單選擇一個站台後，[下一步]**** 按鈕才可以使用。

指定的 [目錄服務還原模式密碼]**** 必須遵守套用至伺服器的密碼原則。 請一律選擇強式的複雜密碼或最好是複雜密碼。網域控制站選項 **** ADDSDeployment Windows PowerShell 引數為：

```
-UseExistingAccount <{$true | $false}>
-SafeModeAdministratorPassword <secure string>
```

> [!IMPORTANT]
> 提供站台名稱做為 **-sitename** 引數的時候，它必須已經存在。 **install-AddsDomainController** Cmdlet 不會建立站台名稱。 您可以使用 Cmdlet **new-adreplicationsite** 來建立新的站台。

如未指定，**Install-ADDSDomainController** 引數會使用與伺服器管理員相同的預設值。

**SafeModeAdministratorPassword** 引數的操作方式比較特殊：

-   如果*沒有指定*為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。

    例如，在 corp.contoso.com 中建立新的 RODC，並提示輸入和確認不顯示字元的密碼：

    ```
    Install-ADDSDomainController -DomainName corp.contoso.com -credential (get-credential)
    ```

-   如果*使用值*指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以使用 **Read-Host** Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串：

```
-safemodeadministratorpassword (read-host -prompt Password: -assecurestring)

```

> [!WARNING]
> 因為前一個選項不會確認密碼，所以請務必小心使用：密碼是看不到的。

您也可以提供轉換的純文字變數當做安全字串，不過我們不鼓勵這種做法。

```
-safemodeadministratorpassword (convertto-securestring Password1 -asplaintext -force)
```

最後，您可以將模糊化密碼儲存到檔案中以在稍後重複使用，而不顯示純文字密碼。 例如：

```
$file = c:\pw.txt
$pw = read-host -prompt Password: -assecurestring
$pw | ConvertFrom-SecureString | Set-Content $file

-safemodeadministratorpassword (Get-Content $File | ConvertTo-SecureString)

```

> [!WARNING]
> 不建議提供或儲存純文字或模糊化的密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。  任何能夠存取該檔案的人都可以回復模糊化的密碼。 一旦具備該知識，他們可以登入以 DSRM 啟動的網域控制站，最終模擬網域控制站本身，並將他們在 AD 樹系中的權限提升至最高。 有一組建議的額外步驟使用 **System.Security.Cryptography** 將文字檔資料加密，不過這不在本文的討論範圍內。 最佳做法是完全避免儲存密碼。

### <a name="rodc-options"></a>RODC 選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCOptions.png)

[RODC 選項]**** 頁面可讓您修改下列設定：

-   委派的系統管理員帳戶

-   可複寫密碼至 RODC 的帳戶

-   不可複寫密碼至 RODC 的帳戶

委派的系統管理員帳戶可取得對 RODC 的本機管理權限。 這些使用者可以使用等同于本機電腦之系統管理員群組的許可權來操作。  他們不是 Domain Admins 群組的成員或網域內建的 Administrators 群組的成員。 這個選項對於委派分公司管理權卻不用釋出網域管理權限很有用。 不需要設定管理委派。

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-delegatedadministratoraccountname <string>
```

不允許在 RODC 上快取密碼且無法連線可寫入網域控制站並驗證其身分的帳戶，無法存取 Active Directory 所提供的資源或功能。

> [!IMPORTANT]
> 如未修改，會使用預設的群組和設定：
>
> -   Administrators - 拒絕
> -   Server Operators - 拒絕
> -   Backup Operators - 拒絕
> -   Account Operators - 拒絕
> -   拒絕的 RODC 密碼複寫群組 - 拒絕
> -   允許的 RODC 密碼複寫群組 - 允許

對等的 ADDSDeployment Windows PowerShell 引數為：

```
-allowpasswordreplicationaccountname <string []>
-denypasswordreplicationaccountname <string []>
```

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_SelectDelAdmin.png)

### <a name="additional-options"></a>其他選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCAdditionalOptions.png)

[其他選項]**** 頁面提供可將網域控制站指定為複寫來源的設定選項，或者您可以使用任一網域控制站做為複寫來源。

您也可以使用「從媒體安裝」(IFM) 選項，使用備份的媒體來安裝網域控制站。 選取 [從媒體安裝]**** 核取方塊時會提供瀏覽選項，您必須按一下 [驗證]**** 以確保提供的路徑是有效的媒體。

IFM 來源的指導方針：
*    IFM 選項所使用的媒體是使用與相同作業系統版本之另一個現有 Windows Server 網域控制站的 Windows Server Backup 或 Ntdsutil.exe 所建立。 例如，您不能使用 Windows Server 2008 R2 或舊版作業系統來建立 Windows Server 2012 網域控制站的媒體。
*    IFM 來源資料應來自可寫入的網域控制站。 雖然來自 RODC 的來源會在技術上用來建立新的 RODC，但是有誤報來源 RODC 不會複寫的錯誤正面複寫警告。

如需 IFM 變更的相關資訊，請參閱[透過媒體變更的 Ntdsutil.exe 安裝](../../../ad-ds/deploy/Simplified-Administration-Appendix.md#BKMK_IFM)。 如果使用以 SYSKEY 保護的媒體，伺服器管理員在驗證期間會提示輸入映像的密碼。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSIFM.png)

其他選項 ADDSDeployment Cmdlet 引數是：

```
-replicationsourcedc <string>
-installationmediapath <string>
-systemkey <secure string>
```

### <a name="paths"></a>路徑
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPaths.png)

[路徑]**** 頁面能讓您覆寫 AD DS 資料庫、資料庫交易記錄以及 SYSVOL 共用的預設資料夾位置。 預設位置一律是在 %systemroot% 的子目錄中。 [路徑]**** ADDSDeployment Cmdlet 引數是：

```
-databasepath <string>
-logpath <string>
-sysvolpath <string>
```

### <a name="preparation-options"></a>準備選項
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrepOptions.png)

[準備選項]**** 頁面會警示您 AD DS 設定包括延伸結構描述 (forestprep) 及更新網域 (domainprep)。 只有在舊版的 Windows Server 2012 網域控制站安裝尚未準備樹系或網域時，或是您手動執行 Adprep.exe 時，才會看到這個頁面。 例如，如果您將新的複本網域控制站新增至現有的 Windows Server 2012 樹系根網域，Active Directory 網域服務設定精靈會封鎖這個頁面。

當您按一下 [下一步]**** 時，並不會延伸結構描述和更新網域。 這些事件只會在安裝階段發生。 這個頁面只是提示稍後安裝時將發生的事件。

這個頁面也會驗證目前使用者的認證是否為 Schema Admin 及 Enterprise Admins 群組的成員，因為您需要這些群組的成員資格才能延伸結構描述或準備網域。 如果這個頁面通知您目前的認證未提供足夠的權限，請按一下 [變更]**** 以提供適當的使用者認證。

其他選項 ADDSDeployment Cmdlet 引數為：

```
-adprepcredential <pscredential>
```

> [!IMPORTANT]
> 與舊版 Windows Server 一樣，Windows Server 2012 的自動化網域準備不會執行 GPPREP。 請為之前不是為 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 準備的所有網域手動執行 **adprep.exe /gpprep**。 您應只在網域的歷程記錄中執行一次 GPPrep，而不是每次升級時都執行。 Adprep.exe 不會自動執行 /gpprep，因其結果可能導致重新複寫所有網域控制站上 SYSVOL 資料夾中的所有檔案和資料夾。
>
> 當您升級網域中的第一個未執行的 RODC 時，即會執行自動 RODCPrep。 它並不是在您升級第一個可寫入的 Windows Server 2012 網域控制站時發生。 如果您計劃部署唯讀網域控制站，您也仍然可以手動執行 **adprep.exe /rodcprep**。

### <a name="review-options-and-view-script"></a>檢閱選項和檢視指令碼
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCReviewOptions.png)

[檢閱選項]**** 頁面能讓您驗證設定，並確保它們符合您的需求，然後才開始安裝。 這不是使用 [伺服器管理員] 停止安裝的最後機會。 這個頁面只是讓您檢閱並確認設定，然後才繼續設定。

[伺服器管理員] 中的 [檢閱選項]**** 頁面也提供選用的 [檢視指令碼]**** 按鈕，用來建立一個包含目前的 ADDSDeployment 設定的 Unicode 文字檔，以便做為一個 Windows PowerShell 指令碼。 這樣可以讓您將 [伺服器管理員] 的圖形介面當作 Windows PowerShell 部署工作室一樣操作。 使用 [Active Directory 網域服務設定精靈] 來設定選項、匯出設定，然後取消精靈。 這個程序會建立一個有效且合乎語義的正確範例，以備日後修改或直接使用。 例如：

```
#
# Windows PowerShell Script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSDomainController `
-AllowPasswordReplicationAccountName @(CORP\Allowed RODC Password Replication Group, CORP\Chicago RODC Admins, CORP\Chicago RODC Users and Computers) `
-Credential (Get-Credential) `
-CriticalReplicationOnly:$false `
-DatabasePath C:\Windows\NTDS `
-DelegatedAdministratorAccountName CORP\Chicago RODC Admins `
-DenyPasswordReplicationAccountName @(BUILTIN\Administrators, BUILTIN\Server Operators, BUILTIN\Backup Operators, BUILTIN\Account Operators, CORP\Denied RODC Password Replication Group) `
-DomainName corp.contoso.com `
-InstallDNS:$true `
-LogPath C:\Windows\NTDS `
-ReadOnlyReplica:$true `
-SiteName Default-First-Site-Name `
-SYSVOLPath C:\Windows\SYSVOL
-Force:$true

```

> [!NOTE]
> [伺服器管理員] 通常會在升級時填入所有引數的值，並不會依賴預設值 (因為它們在未來的 Windows 版本或 Service Pack 中可能會變更)。 **-safemodeadministratorpassword** 引數是個例外。 若要強制確認提示，以互動方式執行 Cmdlet 時請省略該值。

使用選擇性的 Whatif 引數搭配 Install-ADDSDomainController Cmdlet 來檢閱設定資訊。 這可讓您看到 Cmdlet 引數的明確值和隱含值。

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCWhatIf.png)

### <a name="prerequisites-check"></a>先決條件檢查
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCPrereqCheck.png)

[先決條件檢查]**** 是 AD DS 網域設定中的新功能。 這個新階段會驗證伺服器設定是否能夠支援新的 AD DS 樹系。

安裝新的樹系根網域時，[伺服器管理員] 的 [Active Directory 網域服務設定精靈] 會叫用一系列序列化的模組化測試。 這些測試會提醒您建議的修復選項。 您可以視需要執行多次測試。 必須通過所有先決條件測試，網域控制站程序才能繼續。

[先決條件檢查]**** 也會提供諸如影響舊版作業系統之安全性變更的相關資訊。

使用 [伺服器管理員] 時無法略過 [先決條件檢查]****，但您可以在使用 AD DS 部署 Cmdlet 時使用下列引數略過該程序：

```
-skipprechecks

```

按一下 [安裝]**** 以開始網域控制站升級程序。 這是取消安裝的最後機會。 升級程序一旦開始就無法取消。 不論升級結果如何，升級結束時電腦都會自動重新開機。

### <a name="installation"></a>安裝
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCInstallation.png)

當 [安裝]**** 頁面顯示時，網域控制站設定程序就開始執行，而且無法暫停或取消。 詳細的作業會顯示此頁面上，而且會寫入到記錄檔：

-   %systemroot%\debug\dcpromo.log

-   %systemroot%\debug\dcpromoui.log

如果要使用 ADDSDeployment 模組安裝新的 Active Directory 樹系，請使用下列 Cmdlet：

```
Install-addsdomaincontroller

```

如需必要引數和選擇性引數，請參閱本節開頭的 [ADDSDeployment Cmdlet]**** 表格。

**Install-addsdomaincontroller** Cmdlet 只有兩個階段 (先決條件檢查與安裝)。 下列兩個圖形顯示使用 **-domainname**、**-readonlyreplica**、**-sitename** 及 **-credential** 的基本必要引數的安裝階段。 請注意 **Install-ADDSDomainController** 如何提醒您升級會自動將伺服器重新開機 (和伺服器管理員一樣)：

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODC.png)

![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_PSInstallRODCProgress.png)

若要自動接受重新開機的提示，請使用 **-force** 或 **-confirm:$false** 引數搭配任一 ADDSDeployment Windows PowerShell Cmdlet。 若要避免伺服器在升級結束時自動重新開機，請使用 **-norebootoncompletion** 引數。

> [!WARNING]
> 建議您不要覆寫重新開機設定。 網域控制站必須重新開機才能正常運作。 如果您登出網域控制站，必須在重新啟動後您才能以互動方式重新登入。

### <a name="results"></a>結果
![安裝 RODC](media/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-/ADDS_SMI_TR_RODCSignoff.png)

[結果]**** 頁面會顯示升級成功或失敗，以及任何重要的系統管理資訊。 網域控制站會在 10 秒後自動重新開機。
