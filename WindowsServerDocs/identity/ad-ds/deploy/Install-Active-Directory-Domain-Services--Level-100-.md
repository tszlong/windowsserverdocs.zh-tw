---
ms.assetid: ae241ed8-ef19-40a9-b2d5-80b8391551ff
title: 安裝 Active Directory 網域服務 (層級 100)
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7fbce2f844e4bd2764d3d42f993e83fb2d11265f
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93071300"
---
# <a name="install-active-directory-domain-services-level-100"></a>安裝 Active Directory 網域服務 (層級 100)

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題說明如何使用下列任何一種方法，在 Windows Server 2012 中安裝 AD DS：

-   [執行 Adprep.exe 及安裝 Active Directory 網域服務的認證需求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)

-   [使用 Windows PowerShell 安裝 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PS)

-   [使用 [伺服器管理員] 安裝 AD DS](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_GUI)

-   [使用圖形化使用者介面執行分段 RODC 安裝](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_UIStaged)

## <a name="credential-requirements-to-run-adprepexe-and-install-active-directory-domain-services"></a><a name="BKMK_Creds"></a>執行 Adprep.exe 及安裝 Active Directory 網域服務的認證需求
執行 Adprep.exe 及安裝 AD DS 時需要下列認證。

-   若要安裝新樹系，您必須以電腦的本機系統管理員身分登入。

-   若要安裝新的子網域或新的網域樹狀目錄，您必須以 Enterprise Admins 群組成員的身分登入。

-   若要在現有的網域中安裝其他網域控制站，您必須是 Domain Admins 群組的成員。

    > [!NOTE]
    > 如果您未個別執行 adprep.exe 命令，而且要在現有的網域或樹系中安裝執行 Windows Server 2012 的第一個網域控制站，系統會提示您提供認證來執行 Adprep 命令。 認證需求如下所示：
    >
    > -   若要在樹系中引進第一個 Windows Server 2012 網域控制站，您需要提供 Enterprise Admins 群組成員、Schema Admins 群組，以及裝載架構主機之網域中的 Domain Admins 群組的認證。
    > -   若要在網域中引進第一個 Windows Server 2012 網域控制站，您需要提供 Domain Admins 群組成員的認證。
    > -   若要在樹系中引入第一個唯讀網域控制站 (RODC)，您需要提供 Enterprise Admins 群組成員的認證。
    >
    >     > [!NOTE]
    >     > 如果您已在 Windows Server 2008 或 Windows Server 2008 R2 中執行 adprep/rodcprep，則不需要再次針對 Windows Server 2012 執行。

## <a name="installing-ad-ds-by-using-windows-powershell"></a><a name="BKMK_PS"></a>使用 Windows PowerShell 安裝 AD DS
從 Windows Server 2012 開始，您可以使用 Windows PowerShell 來安裝 AD DS。 從 Windows Server 2012 開始已淘汰 Dcpromo.exe，但您仍然可以使用 (dcpromo/unattend： <answerfile> 或 dcpromo/answer：) 的回應檔案來執行 dcpromo.exe <answerfile> 。 使用回應檔案繼續執行 dcpromo.exe 的功能，讓已經將資源投入現有自動化時間的公司，能夠將自動化工作從 dcpromo.exe 轉換到 Windows PowerShell。 如需使用回應檔案執行 dcpromo.exe 的詳細資訊，請參閱 [https://support.microsoft.com/kb/947034](https://support.microsoft.com/kb/947034) 。

如需使用 Windows PowerShell 移除 AD DS 的詳細資訊，請參閱[使用 Windows PowerShell 移除 AD DS](assetId:///99b97af0-aa7e-41ed-8c81-4eee6c03eb4c#BKMK_RemovePS)。

使用 Windows PowerShell 開始新增角色。 這個命令會安裝 AD DS 伺服器角色，以及安裝 AD DS 和 AD LDS 伺服器系統管理工具，包括使用 GUI 的工具 (如 Active Directory 使用者和電腦) 及命令列工具 (如 dcdia.exe)。 使用 Windows PowerShell 時，預設不會安裝伺服器系統管理工具。 您必須指定「 **IncludeManagementTools** 來管理本機伺服器，或安裝 [遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=28972) 以管理遠端伺服器。

```
Install-windowsfeature -name AD-Domain-Services -IncludeManagementTools
<<Windows PowerShell cmdlet and arguments>>
```

AD DS 安裝完成之後，需要重新開機。

接著，您可以執行這個命令，查看 ADDSDeployment 模組中可用的 Cmdlet。

```
Get-Command -Module ADDSDeployment
```

若要查看可以為 Cmdlet 和語法指定的引數清單：

```
Get-Help <cmdlet name>
```

例如，若要查看可以建立未使用的唯讀網域控制站 (RODC) 帳戶的引數，請輸入

```
Get-Help Add-ADDSReadOnlyDomainControllerAccount
```

選用的引數顯示在方括弧內。

您也可以下載 Windows PowerShell Cmdlet 的最新說明範例及概念。 如需詳細資訊，請參閱 [about_Updatable_Help](/powershell/module/microsoft.powershell.core/about/about_updatable_help?view=powershell-5.1)。

您可以針對遠端伺服器執行 Windows PowerShell Cmdlet：

-   在 Windows PowerShell 中，使用 Invoke-Command 搭配 ADDSDeployment Cmdlet。 例如，若要將 AD DS 安裝到 contoso.com 網域中名為 ConDC3 的遠端伺服器上，請輸入：

    ```
    Invoke-Command { Install-ADDSDomainController -DomainName contoso.com -Credential (Get-Credential) } -ComputerName ConDC3
    ```

-或-

-   在 [伺服器管理員] 中，建立一個包含遠端伺服器的伺服器群組。 在遠端伺服器名稱上按一下滑鼠右鍵，然後按一下 [Windows PowerShell]  。

以下幾節說明如何執行 ADDSDeployment 模組 Cmdlet 來安裝 AD DS。

-   [ADDSDeployment Cmdlet 引數](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Params)

-   [指定 Windows PowerShell 認證](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSCreds)

-   [使用測試 Cmdlet](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_TestCmdlets)

-   [使用 Windows PowerShell 安裝新的樹系根網域](../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSForest)

-   [使用 Windows PowerShell 安裝新的子網域或樹狀目錄網域](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSDomain)

-   [使用 Windows PowerShell 安裝其他 (複本) 網域控制站](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_PSReplica)

### <a name="addsdeployment-cmdlet-arguments"></a><a name="BKMK_Params"></a>ADDSDeployment Cmdlet 引數
下表列出 Windows PowerShell 中 ADDSDeployment Cmdlet 的引數。 粗體的引數表示必要引數。 dcpromo.exe 的對等引數如果在 Windows PowerShell 中有不同的名稱，則以括號註明。

Windows PowerShell 參數可以接受 $TRUE 或 $FALSE 引數。 預設 $TRUE 的引數不需要指定。

若要覆寫預設值，可以使用 $False 值指定引數。 例如，因為如果未指定，就會為新的樹系安裝自動執行 **-installdns** ，所以安裝新樹系時 *防止* DNS 安裝的唯一方式是使用：

```
-InstallDNS:$false
```

同樣地，因為「 **installdns** 有預設值 $False 如果您在未裝載 WINDOWS server DNS 伺服器的環境中安裝網域控制站，則必須指定下列引數，才能安裝 DNS 伺服器：

```
-InstallDNS:$true
```


|                                                                                                                 引數                                                                                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              描述                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ADPrepCredential <PS Credential>****注意：** 如果您要在網域或樹系中安裝第一個 Windows Server 2012 網域控制站，而且目前使用者的認證不足以執行此作業，則需要此項。 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    根據 [Get-Credential](/previous-versions/dd315327(v=technet.10)) 的規則和 PSCredential 物件，指定可以準備樹系的 Enterprise Admins 和 Schema Admins 群組成員資格帳戶。<p>如果未指定任何值，則會使用 **"credential** 引數的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                      AllowDomainControllerReinstall                                                                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    指定是否繼續安裝這個可寫入的網域控制站，無論是否偵測到另一個具有相同名稱的可寫入網域控制站。<p>只在您確定另一個可寫入的網域控制站目前沒有使用這個帳戶時，才使用 **$True** 。<p>預設值為 **$False** 。<p>這個引數對 RODC 無效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                           AllowDomainReinstall                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        指定是否重新建立現有的網域。<p>預設值為 **$False** 。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
|                                                                                             AllowPasswordReplicationAccountName <字串 []>                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         指定可將其密碼複寫到這部 RODC 的使用者帳戶、群組帳戶以及電腦帳戶的名稱。 如果想要將這個值保留空白，請指定空的字串 ""。 預設只允許 Allowed RODC Password Replication Group，而此群組最初會以空白建立。<p>提供值做為字串陣列。 例如：<p>程式碼-AllowPasswordReplicationAccountName "JSmith"、"JSmithPC"、"Branch Users"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|                      ApplicationPartitionsToReplicate <string [] > **注意：** UI 中沒有對等的選項。 如果使用 UI 或 IFM 進行安裝，則會複寫所有應用程式分割。                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    指定要複寫的應用程式目錄分割。 只有指定 **-InstallationMediaPath** 引數以便「從媒體安裝」(IFM) 時，才會套用這個引數。 根據預設，所有的應用程式分割都會依據自己的領域進行複寫。<p>提供值做為字串陣列。 例如：<p>準則<p>-ApplicationPartitionsToReplicate "partition1"、"partition2"、"partition3"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                                 確認                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        在執行 Cmdlet 前提示您確認。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                                         CreateDnsDelegation **注意：** 當您執行 Add-ADDSReadOnlyDomainController Cmdlet 時，無法指定此引數。                                                         |                                                                                                                                                                                                                                                                                                                                                                                                                                   指示是否建立 DNS 委派，參照與網域控制站一起安裝的新 DNS 伺服器。 僅適用于 Active Directory 「整合式 DNS」。 只能在連線和可存取的 Microsoft DNS 伺服器上建立委派記錄。 不能為直接從屬於最上層網域 (如 .com、.gov、.biz、.edu) 的網域或兩個字母國碼 (地區碼) 網域 (如 .nz 和 .au) 的網域建立委派記錄。<p>預設值會根據環境自動計算。                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                 **認證 <PS Credential>****注意：** 只有在目前使用者的認證不足以執行此作業時才需要。                                                 |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                根據 [Get-Credential](/previous-versions/dd315327(v=technet.10)) 的規則和 PSCredential 物件，指定可以登入網域的網域帳戶。<p>如果未指定值，將使用目前使用者的認證。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                                         CriticalReplicationOnly                                                                                                          |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         指定 AD DS 安裝作業在重新開機之前是否只執行關鍵性複寫，然後繼續進行。 安裝完成並重新啟動電腦之後，才會進行非關鍵性複寫。<p>不建議使用這個引數。<p>使用者介面 (UI) 中沒有對等選項。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|                                                                                                          DatabasePath <string>                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                      指定完整、非「通用命名慣例」 (UNC) 路徑，指向包含網域資料庫之本機電腦的固定磁片上的目錄，例如 **例如 c:\windows\ntds。**<p>預設路徑為 **%SYSTEMROOT%\NTDS** 。 **重要事項：** 雖然您可以將 AD DS 資料庫和記錄檔儲存在具有復原檔案系統 (ReFS) 的磁片區上，但是除了在 ReFS 上裝載任何資料的恢復功能一般優點之外，在 ReFS 上裝載 AD DS 沒有特定的優點。                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                DelegatedAdministratorAccountName <string>                                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                指定可以安裝和管理 RODC 的使用者或群組名稱。<p>根據預設，只有 Domain Admins 群組的成員可以管理 RODC。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                              DenyPasswordReplicationAccountName <字串 []>                                                                                              |                                                                                                                                                                                                                                                                                                           指定不要將其密碼複寫到這部 RODC 的使用者帳戶、群組帳戶以及電腦帳戶的名稱。 如果不要拒絕複寫任何使用者或電腦的認證，請使用空字串 ""。 預設會拒絕 Administrators、Server Operators、Backup Operators、Account Operators 以及 Denied RODC Password Replication Group。 Denied RODC Password Replication Group 預設包括 Cert Publishers、Domain Admins、Enterprise Admins、Enterprise Domain Controllers、Enterprise Read-Only Domain Controllers、Group Policy Creator Owners、krbtgt 帳戶以及 Schema Admins。<p>提供值做為字串陣列。 例如：<p>準則<p>-DenyPasswordReplicationAccountName "RegionalAdmins","AdminPCs"                                                                                                                                                                                                                                                                                                            |
|                                               DnsDelegationCredential <PS Credential> **注意：** 當您執行 Add-ADDSReadOnlyDomainController Cmdlet 時，無法指定此引數。                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      根據 [Get-Credential](/previous-versions/dd315327(v=technet.10)) 的規則和 PSCredential 物件，指定用來建立 DNS 委派的使用者名稱和密碼。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                        DomainMode <DomainMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<p>Or<p>DomainMode <DomainMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             指定建立新網域時的網域功能等級。<p>網域功能等級不能低於、但可以高於樹系功能等級。<p>預設值是自動計算出來的，並會設定成現有樹系功能等級或設定成為 **-ForestMode** 設定的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|                                                                   **DomainName**<p>Install-ADDSForest 和 Install-ADDSDomainController Cmdlet 的必要引數。                                                                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定要安裝額外網域控制站之網域的 FQDN。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                       **DomainNetbiosName <string>**<p>如果 FQDN 首碼名稱超過 15 個字元，Install-ADDSForest 則需要這個引數。                                                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           與 Install-ADDSForest 搭配使用。 將 NetBIOS 名稱指定給新的樹系根網域。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                                                              DomainType <DomainType> {ChildDomain &#124; TreeDomain} 或 {child &#124; tree}                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  指示要建立的網域類型：現有樹系中的新網域樹狀目錄、現有網域的子網域，或新的樹系。<p>DomainType 的預設值為 ChildDomain。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                                                  Force                                                                                                                   |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             指定這個參數時，會隱藏通常在安裝和新增網域控制站時出現的任何警告，以允許 Cmdlet 完成它的執行。 編寫安裝指令碼時包含這個參數很有用。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                        ForestMode <ForestMode> {Win2003 &#124; Win2008 &#124; Win2008R2 &#124; Win2012 &#124; Win2012R2}<p>Or<p>ForestMode <ForestMode> {2 &#124; 3 &#124; 4 &#124; 5 &#124; 6}                        |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              建立新樹系時指定樹系功能等級。<p>預設值為 Win2012。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
|                                                                                                          InstallationMediaPath                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指出用來安裝新網域控制站之安裝媒體的位置。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|                                                                                                                InstallDns                                                                                                                |                                                                                                                                                                                                                                                                                                                      指定是否應該在網域控制站安裝和設定 DNS 伺服器服務。<p>對於新的樹系，預設值為 **$True** 並會安裝 DNS 伺服器。<p>對於新的子網域或網域樹狀目錄，如果父系網域 (或網域樹狀目錄的樹系根網域) 已經裝載並儲存網域的 DNS 名稱，則這個參數的預設值會是 $True。<p>對於現有網域中的網域控制站安裝，如果未指定這個參數，而且目前的網域已經裝載並儲存網域的 DNS 名稱，則這個參數的預設值會是 **$True** 。 否則，如果 DNS 網域名稱是裝載在 Active Directory 外部，則預設值會是 **$False** 並且不會安裝 DNS 伺服器。                                                                                                                                                                                                                                                                                                                      |
|                                                                                                             LogPath <string>                                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            指定目錄的完整非 UNC 路徑，而此目錄位在內含網域記錄檔之本機電腦的固定式磁碟上，例如， **C:\Windows\Logs** 。<p>預設路徑為 **%SYSTEMROOT%\NTDS** 。 **重要事項：** 請勿將 Active Directory 記錄檔儲存在具有復原檔案系統 (ReFS) 格式的資料磁片區上。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                             MoveInfrastructureOperationMasterRoleIfNecessary                                                                                             |                                                                                                                                                                                                                                                                                                                                                                                                  指定是否要將基礎結構主機操作主機角色 (也稱為彈性單一主機操作或 FSMO) 至您要建立的網域控制站（如果它目前裝載于通用類別目錄伺服器上），而且您不打算讓您建立通用類別目錄伺服器的網域控制站。 指定這個參數會在基礎結構主機角色需要轉移時，將它轉移到您正在建立的網域控制站；在這種情況下，如果您希望基礎結構主機角色維持在目前的位置，請指定 **NoGlobalCatalog** 選項。                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                **NewDomainName <string>****注意：** 只需要安裝-Install-addsdomain。                                                                                |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           為新的網域指定單一網域名稱。<p>例如，如果您想要建立名稱為 **emea.corp.fabrikam.com** 的新子網域，應該指定 **emea** 做為這個引數的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
|                                                      **NewDomainNetbiosName <string>**<p>如果 FQDN 首碼名稱超過 15 個字元，Install-ADDSDomain 則需要這個引數。                                                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               與 Install-ADDSDomain 搭配使用。 將 NetBIOS 名稱指定給新網域。 預設值是衍生自 **"NewDomainName"** 的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|                                                                                                              NoDnsOnNetwork                                                                                                              |                                                                                                                                                                                                                                                                                                                                    指定網路上不提供的 DNS 服務。 只有當這部電腦上網路介面卡的 IP 設定沒有設定 DNS 伺服器名稱來進行名稱解析時，才使用這個參數。 它會指示在這部電腦上安裝 DNS 伺服器以便進行名稱解析。 否則，網路介面卡的 IP 設定必須先設定 DNS 伺服器的位址。<p>省略這個參數 (預設值) 會指示使用這部伺服器電腦上網路介面卡的 TCP/IP 用戶端設定來連絡 DNS 伺服器。 因此，如果您不指定這個參數，請確定先將 TCP/IP 用戶端設定設定成慣用 DNS 伺服器位址。                                                                                                                                                                                                                                                                                                                                     |
|                                                                                                             NoGlobalCatalog                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   指定您不想將網域控制站做為通用類別目錄伺服器。<p>執行 Windows Server 2012 的網域控制站預設會與通用類別目錄一併安裝。 也就是說，它會不經過運算就自動執行，除非您指定：<p>準則<p>-NoGlobalCatalog                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|                                                                                                           NoRebootOnCompletion                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定是否在命令完成時重新啟動電腦 (無論是否成功)。 根據預設，電腦會重新啟動。 若要防止伺服器重新啟動，請指定：<p>準則<p>-NoRebootOnCompletion： $True<p>使用者介面 (UI) 中沒有對等選項。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|                                                                              **ParentDomainName <string>****注意：** Install-ADDSDomain Cmdlet 所需                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 指定現有父系網域的 FQDN。 安裝子網域或新的網域樹狀目錄時使用這個引數。<p>例如，如果您想要建立名稱為 **emea.corp.fabrikam.com** 的新子網域，應該指定 **corp.fabrikam.com** 做為這個引數的值。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|                                                                                                             ReadOnlyReplica                                                                                                              |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   指定是否安裝唯讀網域控制站 (RODC)。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                       ReplicationSourceDC <string>                                                                                                       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              指出從中複寫網域資訊之夥伴網域控制站的 FQDN。 預設值是自動計算的。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                                             **SafeModeAdministratorPassword <securestring>**                                                                                             | 提供以安全模式或不同安全模式 (例如目錄服務還原模式) 啟動電腦時之 Administrator 帳戶的密碼。<p>預設值是空的密碼。 您必須提供密碼。 密碼必須以 System.Security.SecureString 格式提供，如 read-host -assecurestring 或 ConvertTo-SecureString 提供的一樣。<p>SafeModeAdministratorPassword 引數的操作很特別：如果沒有指定為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。如果不用值指定且沒有對 Cmdlet 指定其他引數，Cmdlet 會提示您輸入不顯示字元的密碼但不用確認。 這不是以互動方式執行 Cmdlet 時的慣用用法。如果使用值指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。例如，您可以使用 Read-Host Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串：-safemodeadministratorpassword (read-host -prompt "Password:" -assecurestring)；您也可以提供安全字串當做轉換的純文字變數，不過我們不鼓勵這種做法。 -safemodeadministratorpassword (convertto-securestring "Password1" -asplaintext -force) |
|                                                                     **SiteName <string>**<p>Add-addsreadonlydomaincontrolleraccount Cmdlet 的必要引數                                                                     |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  指定將要安裝網域控制站的站台。 當您執行 **安裝-install-addsforest** 時，沒有任何 **"sitename** 引數，因為第一個建立的網站是預設的網站名稱。<p>提供站台名稱做為 **-sitename** 引數的時候，它必須已經存在。 Cmdlet 將不會建立站台。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|                                                                                                           SkipAutoConfigureDNS                                                                                                           |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           略過 DNS 用戶端設定、轉寄站及根目錄提示的自動設定。 只有當 DNS 伺服器服務已經安裝或自動與 **-InstallDNS** 一起安裝時，這個引數才有效。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                                            SystemKey <string>                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            指定要從中複寫資料之媒體的系統金鑰。<p>預設值為 **none** 。<p>資料必須是 read-host -assecurestring 或 ConvertTo-SecureString 提供的格式。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|                                                                                                           SysvolPath <string>                                                                                                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     指定目錄的完整非 UNC 路徑，而此目錄位在本機電腦的固定式磁碟上，例如， **C:\Windows\SYSVOL** 。<p>預設路徑為 **%SYSTEMROOT%\SYSVOL** 。 **重要事項：** 無法將 SYSVOL 儲存在以復原檔案系統 (ReFS) 格式化的資料磁片區上。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|                                                                                                              SkipPreChecks                                                                                                               |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              不要在開始安裝之前執行先決條件檢查。 不建議使用這個設定。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|                                                                                                                  WhatIf                                                                                                                  |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   顯示執行 Cmdlet 後會發生的情況。 Cmdlet 並不會執行。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

### <a name="specifying-windows-powershell-credentials"></a><a name="BKMK_PSCreds"></a>指定 Windows PowerShell 認證
使用 [Get-credential](/previous-versions/dd315327(v=technet.10)) 可以指定不要在螢幕上以純文字顯示認證。

-SafeModeAdministratorPassword 和 LocalAdministratorPassword 引數的操作很特別：

-   如果沒有指定為引數，Cmdlet 會提示您輸入並確認不顯示字元的密碼。 這是以互動方式執行 Cmdlet 時的慣用用法。

-   如果使用值指定，則這個值必須是安全字串。 這不是以互動方式執行 Cmdlet 時的慣用用法。

例如，您可以使用 **Read-Host** Cmdlet 手動提示輸入密碼，提示使用者輸入安全字串。

```
-SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString)
```

> [!WARNING]
> 因為前一個選項不會確認密碼，所以請務必小心使用：密碼是看不到的。

您也可以提供安全字串當做轉換的純文字變數，不過我們不鼓勵這種做法：

```
-SafeModeAdministratorPassword (ConvertTo-SecureString "Password1" -AsPlainText -Force)
```

> [!WARNING]
> 不建議提供或儲存純文字密碼。 在指令碼中執行這個命令或監視您的任何人都會知道這個網域的 DSRM 密碼。 他們知道了密碼，就可以模擬網域控制站，將自己的權限提升到 Active Directory 樹系中的最高等級。

### <a name="using-test-cmdlets"></a><a name="BKMK_TestCmdlets"></a>使用測試 Cmdlet
每個 ADDSDeployment Cmdlet 都有對應的測試 Cmdlet。 測試 Cmdlet 只為安裝操作執行先決條件檢查；不會設定任何安裝設定。 每個測試 Cmdlet 的引數與對應安裝 Cmdlet 的引數相同，但是「 **SkipPreChecks** 不適用於測試 Cmdlet。

|測試 Cmdlet|描述|
|---------------|---------------|
|Test-ADDSForestInstallation|為安裝新 Active Directory 樹系執行先決條件檢查。|
|Test-ADDSDomainInstallation|為在 Active Directory 中安裝新網域執行先決條件檢查。|
|Test-ADDSDomainControllerInstallation|為在 Active Directory 中安裝網域控制站執行先決條件檢查。|
|Test-ADDSReadOnlyDomainControllerAccountCreation|為新增唯讀網域控制站 (RODC) 帳戶執行先決條件檢查。|

### <a name="installing-a-new-forest-root-domain-using-windows-powershell"></a><a name="BKMK_PSForest"></a>使用 Windows PowerShell 安裝新的樹系根網域
安裝新樹系的命令語法如下。 選用的引數顯示在方括弧內。

```
Install-ADDSForest [-SkipPreChecks] -DomainName <string> -SafeModeAdministratorPassword <SecureString> [-CreateDNSDelegation] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-DomainNetBIOSName <string>] [-ForestMode <ForestMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [-InstallDNS] [-LogPath <string>] [-NoRebootOnCompletion] [-SkipAutoConfigureDNS] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

> [!NOTE]
> 如果您想要變更依據 DNS 網域名稱首碼自動產生的 15 個字元名稱，或是這個名稱超過 15 個字元，必須使用 -DomainNetBIOSName 引數。

例如，若要安裝名稱為 corp.contoso.com 的新樹系並確保出現提供 DSRM 密碼的提示，請輸入：

```
Install-ADDSForest -DomainName "corp.contoso.com"
```

> [!NOTE]
> 執行 Install-ADDSForest 時預設會安裝 DNS 伺服器。

若要安裝名稱為 corp.contoso.com 的新樹系，在 contoso.com 網域中建立 DNS 委派，將網域功能等級設定為 Windows Server 2008 R2，將樹系功能等級設定為 Windows Server 2008，將 Active Directory 資料庫和 SYSVOL 安裝到 D:\ 磁碟機，將記錄檔安裝到 E:\ 磁碟機，出現提供目錄服務還原模式密碼的提示，請輸入：

```
Install-ADDSForest -DomainName corp.contoso.com -CreateDNSDelegation -DomainMode Win2008 -ForestMode Win2008R2 -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"
```

### <a name="installing-a-new-child-or-tree-domain-using-windows-powershell"></a><a name="BKMK_PSDomain"></a>使用 Windows PowerShell 安裝新的子網域或樹狀目錄網域
安裝新網域的命令語法如下。 選用的引數顯示在方括弧內。

```
Install-ADDSDomain [-SkipPreChecks] -NewDomainName <string> -ParentDomainName <string> -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainReinstall] [-CreateDNSDelegation] [-Credential <PS Credential>] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-DomainMode <DomainMode> {Win2003 | Win2008 | Win2008R2 | Win2012}] [DomainType <DomainType> {Child Domain | TreeDomain} [-InstallDNS] [-LogPath <string>] [-NoGlobalCatalog] [-NewDomainNetBIOSName <string>] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-Systemkey <SecureString>] [-SYSVOLPath] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

> [!NOTE]
> 當您目前不是以 Enterprise Admins 群組成員的身分登入時，才需要 **-credential** 引數。
>
> 如果您想要變更依據 DNS 網域名稱首碼自動產生的 15 個字元名稱，或是這個名稱超過 15 個字元，必須使用 **-NewDomainNetBIOSName** 引數。

例如，若要使用 corp\EnterpriseAdmin1 的認證來建立名稱為 child.corp.contoso.com 的新子網域，安裝 DNS 伺服器，在 corp.contoso.com 網域中建立 DNS 委派，將網域功能等級設定為 Windows Server 2003，讓網域控制站成為 Houston 站台中的通用類別目錄伺服器，使用 DC1.corp.contoso.com 做為複寫來源網域控制站，將 Active Directory 資料庫和 SYSVOL 安裝到 D:\ 磁碟機，將記錄檔安裝到 E:\ 磁碟機，出現提供目錄服務還原模式密碼的提示但不出現確認命令的提示，請輸入：

```
Install-ADDSDomain -SafeModeAdministratorPassword -Credential (get-credential corp\EnterpriseAdmin1) -NewDomainName child -ParentDomainName corp.contoso.com -InstallDNS -CreateDNSDelegation -DomainMode Win2003 -ReplicationSourceDC DC1.corp.contoso.com -SiteName Houston -DatabasePath "d:\NTDS" "SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs" -Confirm:$False
```

### <a name="installing-an-additional-replica-domain-controller-using-windows-powershell"></a><a name="BKMK_PSReplica"></a>使用 Windows PowerShell 安裝其他 (複本) 網域控制站
安裝其他網域控制站的命令語法如下。 選用的引數顯示在方括弧內。

```
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-AllowDomainControllerReinstall] [-ApplicationPartitionsToReplicate <string[]>] [-CreateDNSDelegation] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-DNSDelegationCredential <PS Credential>] [-NoDNSOnNetwork] [-NoGlobalCatalog] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SiteName <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

若要在 corp.contoso.com 網域中安裝網域控制站和 DNS 伺服器，並出現提供網域 Administrator 認證及 DSRM 密碼的提示，請輸入：

```
Install-ADDSDomainController -Credential (Get-Credential CORP\Administrator) -DomainName "corp.contoso.com"
```

如果電腦已經加入網域而且您是 Domain Admins 群組的成員，您可以使用：

```
Install-ADDSDomainController -DomainName "corp.contoso.com"
```

若要出現網域名稱的提示，請輸入：

```
Install-ADDSDomainController -Credential (Get-Credential) -DomainName (Read-Host "Domain to promote into")
```

下列命令將會使用 Contoso\EnterpriseAdmin1 的認證，在名稱為 Boston 的站台中安裝一個可寫入的網域控制站和一個通用類別目錄伺服器，安裝 DNS 伺服器，在 contoso.com 網域中建立 DNS 委派，從儲存在 c:\ADDS IFM 資料夾的媒體安裝，將 Active Directory 資料庫和 SYSVOL 安裝到 D:\ 磁碟機，將記錄檔安裝到 E:\ 磁碟機，讓伺服器在 AD DS 安裝完成之後自動重新啟動，然後出現提供目錄服務還原模式密碼的提示：

```
Install-ADDSDomainController -Credential (Get-Credential CONTOSO\EnterpriseAdmin1) -CreateDNSDelegation -DomainName corp.contoso.com -SiteName Boston -InstallationMediaPath "c:\ADDS IFM" -DatabasePath "d:\NTDS" -SYSVOLPath "d:\SYSVOL" -LogPath "e:\Logs"
```

### <a name="performing-a-staged-rodc-installation-using-windows-powershell"></a>使用Windows PowerShell 執行分段 RODC 安裝
建立 RODC 帳戶的命令語法如下。 選用的引數顯示在方括弧內。

```
Add-ADDSReadOnlyDomainControllerAccount [-SkipPreChecks] -DomainControllerAccuntName <string> -DomainName <string> -SiteName <string> [-AllowPasswordReplicationAccountName <string []>] [-NoGlobalCatalog] [-Credential <PS Credential>] [-DelegatedAdministratorAccountName <string>] [-DenyPasswordReplicationAccountName <string []>] [-InstallDNS] [-ReplicationSourceDC <string>] [-Force] [-WhatIf] [-Confirm] [<Common Parameters>]
```

將伺服器連結到 RODC 帳戶的命令語法如下。 選用的引數顯示在方括弧內。

```
Install-ADDSDomainController -DomainName <string> [-SkipPreChecks] -SafeModeAdministratorPassword <SecureString> [-ADPrepCredential <PS Credential>] [-ApplicationPartitionsToReplicate <string[]>] [-Credential <PS Credential>] [-CriticalReplicationOnly] [-DatabasePath <string>] [-NoDNSOnNetwork] [-InstallationMediaPath <string>] [-InstallDNS] [-LogPath <string>] [-MoveInfrastructureOperationMasterRoleIfNecessary] [-NoRebootOnCompletion] [-ReplicationSourceDC <string>] [-SkipAutoConfigureDNS] [-SystemKey <SecureString>] [-SYSVOLPath <string>] [-UseExistingAccount] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

例如，若要建立名稱為 RODC1 的 RODC 帳戶：

```
Add-ADDSReadOnlyDomainControllerAccount -DomainControllerAccountName RODC1 -DomainName corp.contoso.com -SiteName Boston DelegatedAdministratoraccountName PilarA
```

接著在想要連結到 RODC1 帳戶的伺服器上執行下列命令。 這部伺服器不能加入網域。 首先，安裝 AD DS 伺服器角色和管理工具：

```
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools
```

執行下列命令以建立 RODC：

```
Install-ADDSDomainController -DomainName corp.contoso.com -SafeModeAdministratorPassword (Read-Host -Prompt "DSRM Password:" -AsSecureString) -Credential (Get-Credential Corp\PilarA) -UseExistingAccount
```

請按 **Y** 來確認或包含 **"confirm** 引數以防止確認提示。

## <a name="installing-ad-ds-by-using-server-manager"></a><a name="BKMK_GUI"></a>使用 [伺服器管理員] 安裝 AD DS
AD DS 可以使用伺服器管理員中的 [新增角色]，然後使用 [Active Directory Domain Services 設定] （Windows Server 2012 中的新功能），在 Windows Server 2012 中安裝。 從 Windows Server 2012 開始，Active Directory Domain Services 安裝精靈 ( # A0) 已淘汰。

下列小節說明如何建立伺服器集區以便在多部伺服器上安裝和管理 AD DS，以及如何使用精靈來安裝 AD DS。

### <a name="creating-server-pools"></a><a name="BKMK_ServerPools"></a>建立伺服器集區
只要從執行 [伺服器管理員] 的電腦上可以存取網路上的其他伺服器，[伺服器管理員] 就可以將這些伺服器變成一個集區。 成為集區之後，您可以在 [伺服器管理員] 內選擇要從遠端安裝 AD DS 的伺服器或任何其他可能的設定選項。 執行 [伺服器管理員] 的電腦會自動將自己放入集區。 如需伺服器集區的詳細資訊，請參閱[將伺服器新增到伺服器管理員](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831453(v=ws.11))。

> [!NOTE]
> 若要在工作群組伺服器中管理使用伺服器管理員且已加入網域的電腦 (或反之亦然)，需要進行其他的設定步驟。 如需詳細資訊，請參閱 [新增伺服器至伺服器管理員](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831453(v=ws.11))中的「在工作組中新增和管理伺服器」。

### <a name="installing-ad-ds"></a><a name="BKMK_installADDSGUI"></a>安裝 AD DS
**系統管理認證**

安裝 AD DS 的認證需求取決於您選擇的部署設定。 如需詳細資訊，請參閱[執行 Adprep.exe 及安裝 Active Directory 網域服務的認證需求](../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。

使用下列程序，透過 GUI 方法來安裝 AD DS。 這些步驟可以在本機或遠端執行。 如需這些步驟的詳細說明，請參閱下列主題：

-   [使用 [伺服器管理員] 部署樹系](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md#BKMK_SMForest)

-   [在現有的網域中安裝複本 Windows Server 2012 網域控制站 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)

-   [安裝新的 Windows Server 2012 Active Directory 子域或樹狀目錄網域 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)

-   [&#40;RODC 安裝 Windows Server 2012 Active Directory Read-Only 網域控制站&#41; &#40;層級 200&#41;](../../ad-ds/deploy/RODC/Install-a-Windows-Server-2012-Active-Directory-Read-Only-Domain-Controller--RODC---Level-200-.md)

##### <a name="to-install-ad-ds-by-using-server-manager"></a>使用 [伺服器管理員] 安裝 AD DS

1.  在 [伺服器管理員] 中，依序按一下 [管理]  和 [新增角色及功能]  ，啟動 [新增角色精靈]。

2.  在 [在您開始前]  頁面上，按一下 [下一步]  。

3.  在 [選取安裝類型]  頁面上，按一下 [角色型或功能型安裝]  ，然後按 [下一步]  。

4.  在 [選取目的地伺服器]  頁面上，依序按一下 [從伺服器集區選取伺服器]  和您要安裝 AD DS 的伺服器名稱，再按 [下一步]  。

    若要選取遠端伺服器，請先建立伺服器集區並將遠端伺服器新增到其中。 如需建立伺服器集區的詳細資訊，請參閱[將伺服器新增到伺服器管理員](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831453(v=ws.11))。

5.  在 [選取伺服器角色]  頁面上，按一下 [Active Directory 網域服務]  ，然後在 [新增角色及功能精靈]  對話方塊中，按一下 [新增功能]  ，再按 [下一步]  。

6.  在 [選取功能]  頁面上，選取想要安裝的任何其他功能，然後按 [下一步]  。

7.  檢閱 [Active Directory 網域服務]  頁面上提供的資訊，然後按 [下一步]  。

8.  在 [確認安裝選項]  頁面上，按一下 [安裝]  。

9. 在 [結果]  頁面上，確認安裝已經成功，然後按一下 [將此伺服器升級為網域控制站]  ，啟動 [Active Directory 網域服務設定精靈]。

    ![安裝 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_SMPromotes.gif)

    > [!IMPORTANT]
    > 如果您在這個時候關閉 [新增角色精靈] 而沒有啟動 [Active Directory 網域服務設定精靈]，只要按一下 [伺服器管理員] 中的 [工作] 就可以重新啟動它。

    ![安裝 AD DS](media/Install-Active-Directory-Domain-Services--Level-100-/ADDS_SMI_Tasks.gif)

10. 在 [部署設定]  頁面上，選擇下列其中一個選項：

    -   如果您要在現有網域中安裝其他網域控制站，請按一下 [ **新增現有網域的網域控制站** ]，並輸入網域 (的名稱，例如 emea.corp.contoso.com) 或按一下 [ **選取** ]。若要選擇網域和認證 (例如，請指定屬於 domain Admins 群組成員的帳戶) 然後按 **[下一步]** 。

        > [!NOTE]
        > 只在電腦已加入網域且您是執行本機安裝時，預設才會提供網域名稱及目前使用者的認證。 如果您是在遠端伺服器上安裝 AD DS，在設計上則需要指定認證。 如果目前的使用者認證不足以執行安裝，請按一下 [ **變更** ]，以指定不同的認證。

        如需詳細資訊，請參閱 [在現有網域中安裝複本 Windows Server 2012 網域控制站 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)。

    -   如果您是安裝新的子網域，請按一下 [新增網域到現有的樹系]  ，在 [選取網域類型]  中選取 [子網域]  ，輸入或瀏覽到父系網域 DNS 名稱 (例如 corp.contoso.com)，輸入新子網域的相對名稱 (例如 emea)，輸入用來建立新網域的認證，然後按 [下一步]  。

        如需詳細資訊，請參閱 [安裝新的 Windows Server 2012 Active Directory 子域或樹狀目錄網域 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。

    -   如果您是安裝新的網域樹狀目錄，請按一下 [新增網域到現有的樹系]  ，在 [選取網域類型]  中選取 [樹狀目錄網域]  ，輸入根網域的名稱 (例如 corp.contoso.com)，輸入新網域的 DNS 名稱 (例如 fabrikam.com)，輸入用來建立新網域的認證，然後按 [下一步]  。

        如需詳細資訊，請參閱 [安裝新的 Windows Server 2012 Active Directory 子域或樹狀目錄網域 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md)。

    -   如果您是安裝新樹系，請按一下 [新增樹系]  ，然後輸入根網域的名稱 (例如 corp.contoso.com)。

        如需詳細資訊，請參閱 [安裝新的 Windows Server 2012 Active Directory 樹系 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md)。

11. 在 [網域控制站選項]  頁面上，選擇下列其中一個選項：

    -   如果您是建立新的樹系或網域，請選取網域和樹系功能等級，按一下 [網域名稱系統 (DNS) 伺服器]  ，指定 DSRM 密碼，然後按 [下一步]  。

    -   如果您是新增網域控制站到現有網域，請依需要按一下 [網域名稱系統 (DNS) 伺服器]  、[通用類別目錄 (GC)]  或 [唯讀網域控制站 (RODC)]  ，選擇站台名稱，輸入 DSRM 密碼，然後按 [下一步]  。

    如需這個頁面在不同情況下會出現什麼選項的詳細資訊，請參閱[網域控制站選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DCOptionsPage)。

12. 在 [DNS 選項]  頁面上 (只在安裝 DNS 伺服器時才會出現)，依需要按一下 [更新 DNS 委派]  。 如果這樣做，請提供有權限在父系 DNS 區域中建立 DNS 委派記錄的認證。

    如果無法連線裝載父系區域的 DNS 伺服器，則不會顯示 [更新 DNS 委派]  選項。

    如需是否需要更新 DNS 委派的詳細資訊，請參閱[了解區域委派](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771640(v=ws.11))。 如果您嘗試更新 DNS 委派但發生錯誤，請參閱 [DNS 選項](../../ad-ds/deploy/AD-DS-Installation-and-Removal-Wizard-Page-Descriptions.md#BKMK_DNSOptionsPage)。

13. 在 [RODC 選項]  頁面上 (只在安裝 RODC 時才會出現)，指定將要管理 RODC 的群組或使用者的名稱，在允許或拒絕的密碼複寫群組中新增或移除帳戶，然後按 [下一步]  。

    如需詳細資訊，請參閱[密碼複寫原則](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc730883(v=ws.10))。

14. 在 [其他選項]  頁面上，選擇下列其中一個選項：

    -   如果您是建立新的網域，請輸入新的 NetBIOS 名稱或確認網域的預設 NetBIOS 名稱，然後按 [下一步]  。

    -   如果您是新增網域控制站到現有網域，請選取想要從該處複寫 AD DS 安裝資料的網域控制站 (或允許精靈選取任何網域控制站)。 如果您是從媒體安裝，請按一下 [從媒體安裝路徑]  ，輸入並確認安裝來源檔案的路徑，然後按 [下一步]  。

        您不能使用「從媒體安裝」(IFM) 在網域中安裝第一個網域控制站。 IFM 不能跨不同的作業系統版本運作。 換句話說，若要使用 IFM 安裝執行 Windows Server 2012 的額外網域控制站，您必須在 Windows Server 2012 網域控制站上建立備份媒體。 如需 IFM 的詳細資訊，請參閱[使用 IFM 安裝其他網域控制站](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816722(v=ws.10))。

15. 在 [路徑]  頁面上，輸入 Active Directory 資料庫，記錄檔以及 SYSVOL 資料夾的位置 (或接受預設位置)，然後按 [下一步]  。

    > [!IMPORTANT]
    > 不要將 Active Directory 資料庫，記錄檔或 SYSVOL 資料夾儲存在使用復原檔案系統 (ReFS) 格式化的資料磁碟區。

16. 在 [準備選項]  頁面上，輸入能夠執行 adprep 的認證。 如需詳細資訊，請參閱[執行 Adprep.exe 及安裝 Active Directory 網域服務的認證需求](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md#BKMK_Creds)。

17. 在 [檢閱選項]  頁面上，確認您的選取，按一下 [檢視指令碼]  (如果想要將設定匯出到 Windows PowerShell 指令碼)，然後按 [下一步]  。

18. 在 [先決條件檢查]  頁面上，確認先決條件驗證已完成，然後按一下 [安裝]  。

19. 在 [結果]  頁面上，確認伺服器已成功設定為網域控制站。 伺服器將會自動重新啟動以完成 AD DS 安裝。

## <a name="performing-a-staged-rodc-installation-using-the-graphical-user-interface"></a><a name="BKMK_UIStaged"></a>使用圖形化使用者介面執行分段 RODC 安裝
分段 RODC 安裝能讓您用兩個階段建立 RODC。 在第一個階段中，Domain Admins 群組成員會建立一個 RODC 帳戶。 在第二個階段中，會將伺服器連結到 RODC 帳戶。 第二個階段可以由 Domain Admins 群組成員或委派的網域使用者或群組完成。

#### <a name="to-create-an-rodc-account-by-using-the-active-directory-management-tools"></a>使用 Active Directory 管理工具建立 RODC 帳戶

1.  使用 Active Directory 管理中心或 [Active Directory 使用者和電腦] 可以建立 RODC 帳戶。

    1.  依序按一下 [開始]  和 [系統管理工具]  ，然後按一下 [Active Directory 管理中心]  。

    2.  在瀏覽窗格 (左窗格) 中，按一下網域的名稱。

    3.  在管理清單 (中央窗格) 中，按一下 [Domain Controllers]  OU。

    4.  在 [工作] 窗格 (右窗格) 中，按一下 [預先建立唯讀網域控制站帳戶]  。

    -或-

    1.  依序按一下 [開始]  、[系統管理工具]  及 [Active Directory 使用者和電腦]  。

    2.  在 [Domain Controllers]  組織單位 (OU) 上按一下滑鼠右鍵，或按一下 [Domain Controllers]  OU，然後按一下 [動作]  。

    3.  按一下 [預先建立唯讀網域控制站帳戶]  。

2.  在 [歡迎使用 Active Directory 網域服務安裝精靈]  頁面上，若您要修改預設的密碼複寫原則 (PRP)，請選取 [使用進階模式安裝]  ，然後按 [下一步]  。

3.  在 [網路認證]  頁面的 [請指定用來執行安裝的帳戶認證]  之下，按一下 [我目前登入的認證]  ，或按一下 [備用認證]  ，然後按一下 [設定]  。 在 [Windows 安全性]  對話方塊中，提供可安裝其他網域控制站之帳戶的使用者名稱和密碼。 若要安裝其他網域控制站，您必須是 Enterprise Admins 群組或 Domain Admins 群組的成員。 完成提供認證之後，按 [下一步]  。

4.  在 [指定電腦名稱]  頁面上，輸入將做為 RODC 之伺服器的電腦名稱。

5.  在 [選取站台]  頁面上，從清單中選取站台，或選取在對應至執行此精靈之電腦 IP 位址的站台中安裝網域控制站的選項，然後按 [下一步]  。

6.  在 [其他網域控制站選項]  頁面上，選取下列項目，然後按 [下一步]  ：

    -   **DNS 伺服器** ：預設會選取這個選項，這樣網域控制站即可當作網域名稱系統 (DNS) 伺服器。 如果您不要讓網域控制站當做 DNS 伺服器，請取消選取此選項。 不過，若您不在 RODC 上安裝 DNS 伺服器角色，而且 RODC 是您分公司的唯一網域控制站，則當與中樞站台之間的廣域網路 (WAN) 連線中斷時，分公司的使用者將無法執行名稱解析。

    -   **通用類別目錄** ：預設會選取此選項。 它會新增通用類別目錄，唯讀目錄磁碟分割至網域控制站，並啟用通用類別目錄搜尋功能。 如果您不要讓網域控制站當做通用類別目錄伺服器，請取消選取此選項。 不過，若您不在分公司安裝通用類別目錄伺服器，或未啟用包含 RODC 之站台的萬用群組成員資格快取，則當與中樞站台之間的 WAN 連線中斷時，分公司的使用者將無法登入網域。

    -   **唯讀網域控制站** 。 當您建立 RODC 帳戶時，預設會選取這個選項，而且您不能取消選取。

7.  如果您在 [歡迎使用]  頁面上選取 [使用進階模式安裝]  核取方塊，就會顯示 [指定密碼複寫原則]  頁面。 預設不會將帳戶密碼複寫至 RODC，而且明確拒絕讓安全性帳戶 (例如，Domain Admins 群組的成員) 將其密碼複寫至 RODC。

    若要在原則中新增其他帳戶，請按一下 [新增]  ，按一下 [允許將帳戶的密碼複寫至此 RODC]  或按一下 [拒絕將帳戶的密碼複寫至此 RODC]  ，然後選取帳戶。

    完成 (或接受預設設定) 後，按 [下一步]  。

8.  在 [RODC 安裝與管理的委派]  頁面上，輸入會將伺服器連結到您正在建立之 RODC 帳戶的使用者或群組的名稱。 您只能輸入一個安全性主體的名稱。

    若要搜尋特定使用者或群組的目錄，請按一下 [設定]  。 在 [選取使用者或群組]  中，輸入使用者或群組的名稱。 建議您將 RODC 安裝與管理委派給群組。

    安裝之後，這個使用者或群組也將擁有 RODC 的本機系統管理權限。 如果您未指定使用者或群組，則只有 Domain Admins 群組或 Enterprise Admins 群組的成員才能將伺服器連接到帳戶。

    完成後，請按 [下一步]  。

9. 在 [摘要]  頁面上檢閱您的選擇。 必要時請按 [上一步]  變更任何選擇。

    若要將您選取的設定存成回應檔案，以便用來自動執行後續的 AD DS 操作，請按一下 [匯出設定]  。 輸入回應檔案的名稱，然後按一下 [儲存]  。

    當您確定選擇正確時，按 [下一步]  以建立 RODC 帳戶。

10. 在 [完成 Active Directory 網域服務安裝精靈]  頁面上，按一下 [完成]  。

建立 RODC 帳戶之後，您可以將伺服器連結到帳戶以完成 RODC 安裝。 這個第二個階段可以在將要放置 RODC 的分公司完成。 執行此程序的伺服器不可以加入網域。 從 Windows Server 2012 開始，您可以使用伺服器管理員中的 [新增角色]，將伺服器附加到 RODC 帳戶。

#### <a name="to-attach-a-server-to-an-rodc-account-using-server-manager"></a>使用 [伺服器管理員] 將伺服器連結到 RODC 帳戶

1.  以本機系統管理員身分登入。

2.  在 [伺服器管理員] 中，按一下 [新增角色及功能]  。

3.  在 [在您開始前]  頁面上，按一下 [下一步]  。

4.  在 [選取安裝類型]  頁面上，按一下 [角色型或功能型安裝]  ，然後按 [下一步]  。

5.  在 [選取目的地伺服器]  頁面上，依序按一下 [從伺服器集區選取伺服器]  和您要安裝 AD DS 的伺服器名稱，再按 [下一步]  。

6.  在 [選取伺服器角色]  頁面上，依序按一下 [Active Directory 網域服務]  、[新增功能]  ，然後按 [下一步]  。

7.  在 [選取功能]  頁面上，選取想要安裝的任何其他功能，然後按 [下一步]  。

8.  檢閱 [Active Directory 網域服務]  頁面上提供的資訊，然後按 [下一步]  。

9. 在 [確認安裝選項]  頁面上，按一下 [安裝]  。

10. 在 [結果]  頁面上，確認 [安裝成功]  ，然後按一下 [將此伺服器升級為網域控制站]  ，啟動 [Active Directory 網域服務設定精靈]。

    > [!IMPORTANT]
    > 如果您在這個時候關閉 [新增角色精靈] 而沒有啟動 [Active Directory 網域服務設定精靈]，只要按一下 [伺服器管理員] 中的 [工作] 就可以重新啟動它。

     (媒體/安裝-Active Directory-Domain Services--Level-100-/ADDS_SMI_Tasks.gif) 

11. 在 [部署設定]  頁面上，按一下 [新增現有網域的網域控制站]  ，輸入網域名稱 (例如 emea.contoso.com) 和認證 (例如，指定委派來管理和安裝 RODC 的帳戶)，然後按 [下一步]  。

12. 在 [網域控制站選項]  頁面上，按一下 [使用現有的 RODC 帳戶]  ，輸入並確認目錄服務還原模式密碼，然後按 [下一步]  。

13. 在 [其他選項]  頁面上，如果您是從媒體安裝，按一下 [從媒體安裝路徑]  ，輸入並確認安裝來源檔案的路徑，選取想要從該處複寫 AD DS 安裝資料的網域控制站 (或允許精靈選取任何網域控制站)，然後按 [下一步]  。

14. 在 [路徑]  頁面上，輸入 Active Directory 資料庫、記錄檔以及 SYSVOL 資料夾的位置 (或接受預設位置)，然後按 [下一步]  。

15. 在 [檢閱選項]  頁面上，確認您的選取，按一下 [檢視指令碼]  將設定匯出到 Windows PowerShell 指令碼，然後按 [下一步]  。

16. 在 [先決條件檢查]  頁面上，確認先決條件驗證已完成，然後按一下 [安裝]  。

    為了完成 AD DS 安裝，伺服器會自動重新啟動。

## <a name="see-also"></a>另請參閱
[疑難排解網域控制站部署](Troubleshooting-Domain-Controller-Deployment.md) 
[安裝新的 Windows Server 2012 Active Directory 樹系 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Forest--Level-200-.md) 
[安裝新的 Windows Server 2012 Active Directory 子域或樹狀目錄網域 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-New-Windows-Server-2012-Active-Directory-Child-or-Tree-Domain--Level-200-.md) 
[在現有的網域中安裝複本 Windows Server 2012 網域控制站 &#40;層級 200&#41;](../../ad-ds/deploy/Install-a-Replica-Windows-Server-2012-Domain-Controller-in-an-Existing-Domain--Level-200-.md)
