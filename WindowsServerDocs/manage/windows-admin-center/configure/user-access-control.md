---
title: 設定使用者存取控制和許可權
description: 瞭解如何使用 Active Directory 或 Azure AD 設定使用者存取控制和許可權（Project 檀香山）
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407038"
---
# <a name="configure-user-access-control-and-permissions"></a>設定使用者存取控制和許可權

> 適用于： Windows 系統管理中心、Windows 系統管理中心預覽

如果您還沒有這麼做，請熟悉[Windows 系統管理中心的使用者存取控制選項](../plan/user-access-options.md)

> [!NOTE]
> 工作組環境或跨非信任的網域不支援 Windows 管理中心的群組型存取。

## <a name="gateway-access-role-definitions"></a>閘道存取角色定義

有兩個角色可存取 Windows Admin Center 閘道服務：

**閘道使用者**可以連線到 Windows 管理中心閘道服務，以透過該閘道管理伺服器，但無法變更存取權限或用來驗證閘道的驗證機制。

**閘道系統管理員**可以設定誰取得存取權，以及使用者如何向閘道進行驗證。 只有閘道系統管理員可以在 Windows 系統管理中心中查看和設定存取設定。 閘道電腦上的本機系統管理員一律是 Windows Admin Center 閘道服務的管理員。

> [!NOTE]
> 對閘道的存取並不表示存取閘道所見的受管理伺服器。 若要管理目標伺服器，連接的使用者必須使用具有該目標伺服器之系統管理存取權的認證（透過其傳遞的 Windows 認證，或透過 Windows 管理中心會話中**提供的認證**）。

## <a name="active-directory-or-local-machine-groups"></a>Active Directory 或本機電腦群組

根據預設，Active Directory 或本機電腦群組會用來控制閘道存取。 如果您有 Active Directory 網域，則可以從 Windows 管理中心介面中管理閘道使用者和系統管理員存取權。

在 [**使用者**] 索引標籤上，您可以控制誰可以存取 Windows 管理中心做為閘道使用者。 根據預設，如果您未指定安全性群組，則任何存取閘道 URL 的使用者都有存取權。 一旦您將一或多個安全性群組新增至 [使用者] 清單，存取權就會限制為這些群組的成員。

如果您未在您的環境中使用 Active Directory 網域，則存取是由 Windows 管理中心閘道電腦上的 `Users` 和 `Administrators` 本機群組所控制。

### <a name="smartcard-authentication"></a>智慧卡驗證

您可以為以智慧卡為基礎的安全性群組指定額外的_必要_群組，以強制執行**智慧卡驗證**。 當您新增以智慧卡為基礎的安全性群組之後，如果使用者是任何安全性群組的成員，以及包含在 [使用者] 清單中的智慧卡群組，則只能存取 Windows Admin Center 服務。

在 [系統**管理員**] 索引標籤上，您可以控制誰可以存取 Windows 系統管理中心作為閘道管理員。 電腦上的本機 administrators 群組一律具有完整的系統管理員存取權，無法從清單中移除。 藉由新增安全性群組，您可以授與這些群組的成員變更 Windows 管理中心閘道設定的許可權。 [系統管理員] 清單支援智慧卡驗證，其方式與使用者清單相同：具有安全性群組和智慧卡群組的 AND 條件。

## <a name="azure-active-directory"></a>Azure Active Directory

如果您的組織使用 Azure Active Directory （Azure AD），您可以選擇在 Windows 管理中心新增**額外**的安全性層級，方法是要求 Azure AD 驗證來存取閘道。 若要存取 Windows 管理中心，使用者的**windows 帳戶**也必須能夠存取閘道伺服器（即使使用 Azure AD 驗證）。 當您使用 Azure AD 時，您會從 Azure 入口網站管理 Windows 管理中心使用者和系統管理員的存取權限，而不是從 Windows 系統管理中心 UI 中。

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>在啟用 Azure AD 驗證時存取 Windows 系統管理中心

視使用的瀏覽器而定，某些存取 Windows 管理中心並設定 Azure AD authentication 的使用者將會在**瀏覽器**中收到額外的提示，他們必須為安裝 Windows 系統管理中心的電腦提供其 windows 帳號憑證。 輸入該資訊之後，使用者會收到額外的 Azure Active Directory 驗證提示，這需要已在 Azure 中的 Azure AD 應用程式中授與存取權的 Azure 帳號憑證。

> [!NOTE]
> Windows 帳戶在閘道電腦上具有**系統管理員許可權**的使用者，將不會收到 Azure AD 驗證的提示。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>設定 Windows 管理中心預覽的 Azure Active Directory 驗證

移至 Windows 系統管理中心**設定** > **存取**，並使用切換開關開啟 使用 Azure Active Directory 將安全性層級新增至閘道。 如果您尚未向 Azure 註冊閘道，系統會引導您在此時執行此動作。

根據預設，Azure AD 租使用者的所有成員都具有 Windows Admin Center 閘道服務的使用者存取權。 只有閘道機器上的本機系統管理員才具有 Windows 管理中心閘道的系統管理員存取權。 請注意，閘道機器上的本機系統管理員許可權不能受到限制-本機系統管理員可以執行任何動作，不論是否使用 Azure AD 進行驗證。

如果您想要為特定 Azure AD 使用者或群組閘道使用者或閘道系統管理員授與 Windows Admin Center 服務的存取權，您必須執行下列動作：

1.  使用 [存取設定] 中提供的超連結，移至 Azure 入口網站中的 Windows 系統管理中心 Azure AD 應用程式。 注意：只有在啟用 Azure Active Directory authentication 時，才可以使用此超連結。 
    -   您也可以在 Azure 入口網站中找到您的應用程式，方法是前往**Azure Active Directory** > **企業應用程式** > [**所有應用程式**] 和 [搜尋] **WindowsAdminCenter** （Azure AD 應用程式將命名為 WindowsAdminCenter-<gateway name>）。 如果您沒有取得任何搜尋結果，請確定 [**顯示**] 設定為 [**所有應用程式**]、[**應用程式狀態**] 設為 [**任何**]，然後按一下 [套用]，然後嘗試進行搜尋。 找到應用程式之後，請移至 [**使用者和群組**]
2.  在 [屬性] 索引標籤中，將 [**使用者指派**] 設定為 [是]。
    完成這項作業之後，只有 [**使用者和群組**] 索引標籤中列出的成員才能夠存取 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取 [**新增使用者**]。 您必須為每個新增的使用者/群組指派閘道使用者或閘道系統管理員角色。

一旦您開啟 Azure AD authentication，閘道服務就會重新開機，而您必須重新整理瀏覽器。 您可以隨時在 Azure 入口網站中更新適用于 SME Azure AD 應用程式的使用者存取權。

當使用者嘗試存取 Windows 管理中心閘道 URL 時，系統會提示他們使用其 Azure Active Directory 身分識別進行登入。 請記住，使用者也必須是閘道伺服器上的本機使用者成員，才能存取 Windows 系統管理中心。

使用者和系統管理員可以從 Windows 系統管理中心設定的 [**帳戶**] 索引標籤中，查看其目前登入的帳戶，以及登出此 Azure AD 帳戶。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>設定 Windows 管理中心的 Azure Active Directory 驗證

[若要設定 Azure AD authentication，您必須先向 Azure 註冊您的閘道](azure-integration.md)（您只需要針對您的 Windows 管理中心閘道執行此動作一次）。 此步驟會建立一個 Azure AD 應用程式，您可以在其中管理閘道使用者和閘道系統管理員存取權。

如果您想要為特定 Azure AD 使用者或群組閘道使用者或閘道系統管理員授與 Windows Admin Center 服務的存取權，您必須執行下列動作：

1.  在 Azure 入口網站中，移至您的 SME Azure AD 應用程式。 
    -   當您按一下 [**變更存取控制**]，然後從 [Windows 管理中心] [存取設定] 選取 [ **Azure Active Directory** ] 時，您可以使用 UI 中提供的超連結來存取 Azure 入口網站中的 Azure AD 應用程式。 當您按一下 [儲存] 並選取 Azure AD 做為存取控制身分識別提供者之後，[存取設定] 也會提供此超連結。
    -   您也可以在 Azure 入口網站中找到您的應用程式，方法是前往**Azure Active Directory** > **企業應用程式** > **所有應用程式**，並搜尋**sme** （Azure AD 應用程式將命名為 sme-<gateway>）。 如果您沒有取得任何搜尋結果，請確定 [**顯示**] 設定為 [**所有應用程式**]、[**應用程式狀態**] 設為 [**任何**]，然後按一下 [套用]，然後嘗試進行搜尋。 找到應用程式之後，請移至 [**使用者和群組**]
2.  在 [屬性] 索引標籤中，將 [**使用者指派**] 設定為 [是]。
    完成這項作業之後，只有 [**使用者和群組**] 索引標籤中列出的成員才能夠存取 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取 [**新增使用者**]。 您必須為每個新增的使用者/群組指派閘道使用者或閘道系統管理員角色。

一旦您將 Azure AD 存取控制儲存在 [**變更存取控制**] 窗格中，閘道服務就會重新開機，而您必須重新整理瀏覽器。 您可以隨時在 Azure 入口網站中更新 Windows Admin Center Azure AD 應用程式的使用者存取權。 

當使用者嘗試存取 Windows 管理中心閘道 URL 時，系統會提示他們使用其 Azure Active Directory 身分識別進行登入。 請記住，使用者也必須是閘道伺服器上的本機使用者成員，才能存取 Windows 系統管理中心。 

使用 Windows 管理中心 [一般設定] 的 [ **Azure** ] 索引標籤，使用者和系統管理員可以查看其目前登入的帳戶，以及登出此 Azure AD 帳戶。

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和多重要素驗證

使用 Azure AD 作為另一層安全性以控制 Windows 管理中心閘道存取權的其中一項優點是，您可以利用 Azure AD 強大的安全性功能，例如條件式存取和多重要素驗證。 

[深入瞭解如何使用 Azure Active Directory 來設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>設定單一登入

**在 Windows Server 上部署為服務時的單一登入**

當您在 Windows 10 上安裝 Windows 系統管理中心時，可以使用單一登入。 不過，如果您要在 Windows Server 上使用 Windows 系統管理中心，您必須在您的環境中設定某種形式的 Kerberos 委派，才能使用單一登入。 委派會將閘道電腦設定為受信任，以委派給目標節點。 

若要在您的環境中設定以[資源為基礎的限制委派](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview)，請執行下列 PowerShell Cmdlet。 （請注意，這需要執行 Windows Server 2012 或更新版本的網域控制站）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此範例中，Windows 管理中心閘道是安裝在伺服器**WindowsAdminCenterGW**上，而目標節點名稱是**ManagedNode**。

若要移除此關聯性，請執行下列 Cmdlet：

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>角色型存取控制

以角色為基礎的存取控制可讓您為使用者提供有限的電腦存取權，而不是讓他們成為完整的本機系統管理員。
[深入瞭解角色型存取控制和可用的角色。](../plan/user-access-options.md#role-based-access-control)

設定 RBAC 包含2個步驟：在目的電腦上啟用支援，並將使用者指派給相關角色。

> [!TIP]
> 請確定您在設定角色型存取控制支援的電腦上具有本機系統管理員許可權。

### <a name="apply-role-based-access-control-to-a-single-machine"></a>將角色型存取控制套用至單一電腦

單一機器部署模型非常適合只需要少數電腦來管理的簡單環境。
設定具有角色型存取控制支援的電腦，將會產生下列變更：

-   具有 Windows 管理中心所需功能的 PowerShell 模組，將會安裝在您的系統磁片磁碟機的 [`C:\Program Files\WindowsPowerShell\Modules`] 底下。 所有模組都將以**Microsoft. Sme**開頭
-   Desired State Configuration 將會執行一次性設定，以在名為**Microsoft**的電腦上設定剛好足夠的管理端點。 此端點會定義 Windows 管理中心所使用的3個角色，並會在使用者連線到它時，以暫時的本機系統管理員身分執行。
-   3將會建立新的本機群組，以控制哪些使用者被指派哪些角色的存取權：
    -   Windows 系統管理中心系統管理員
    -   Windows 管理中心 Hyper-v 系統管理員
    -   Windows 系統管理中心讀者

若要在單一電腦上啟用角色型存取控制的支援，請遵循下列步驟：

1.  開啟 Windows 管理中心，並使用在目的電腦上具有本機系統管理員許可權的帳戶，連接到您想要以角色型存取控制設定的電腦。
2.  在 [**總覽**] 工具上，按一下 [**設定**] > [**角色型存取控制**]。
3.  按一下頁面底部的 [套用]，**以在目標**電腦上支援角色型存取控制。 應用程式進程包含複製 PowerShell 腳本，以及叫用目的電腦上的設定（使用 PowerShell Desired State Configuration）。 最多可能需要10分鐘的時間才能完成，並會導致 WinRM 重新開機。 這會暫時中斷 Windows 系統管理中心、PowerShell 和 WMI 使用者的連線。
4.  重新整理頁面，以檢查角色型存取控制的狀態。 當它已準備好可供使用時，狀態將會**變更為 [** 已套用]。

套用設定之後，您可以將使用者指派給角色：

1.  開啟 [**本機使用者和群組**] 工具，然後流覽至 [**群組**] 索引標籤。
2.  選取 [ **Windows 管理中心讀取**者] 群組。
3.  在底部的*詳細資料*窗格中，按一下 [**新增使用者**]，然後輸入應具有伺服器的唯讀存取權的使用者或安全性群組的名稱（透過 Windows 系統管理中心）。 使用者和群組可以來自本機電腦或您的 Active Directory 網域。
4.  針對 [ **Windows 管理中心**] [Hyper-v 系統管理員] 和 [ **windows 系統管理中心系統管理員**] 群組重複步驟2-3。

您也可以使用[受限制的群組原則設定](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)群組原則物件，以一致的方式在網域中填滿這些群組。

### <a name="apply-role-based-access-control-to-multiple-machines"></a>將角色型存取控制套用至多部電腦

在大型企業部署中，您可以使用現有的自動化工具，從 Windows 管理中心閘道下載設定套件，以將角色型存取控制功能推送至您的電腦。
設定套件是設計來搭配 PowerShell Desired State Configuration 使用，但您可以使用它來配合您慣用的自動化解決方案。

#### <a name="download-the-role-based-access-control-configuration"></a>下載以角色為基礎的存取控制設定

若要下載以角色為基礎的存取控制設定套件，您必須能夠存取 Windows 管理中心和 PowerShell 提示字元。

如果您是在 Windows Server 上以服務模式執行 Windows 管理中心閘道，請使用下列命令來下載設定套件。
請務必使用適用于您環境的正確閘道位址來更新它。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果您是在 Windows 10 電腦上執行 Windows 管理中心閘道，請改為執行下列命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

當您展開 zip 封存時，您會看到下列資料夾結構：

- InstallJeaFeatures. ps1
- JustEnoughAdministration （目錄）
- 模組（目錄）
    - \* （目錄）
    - WindowsAdminCenter. Jea （目錄）

若要在節點上設定角色型存取控制的支援，您需要執行下列動作：

1.  複製 JustEnoughAdministration，Microsoft SME。\*，並將 Jea 模組 WindowsAdminCenter 到目的電腦上的 PowerShell 模組目錄。 這通常位於 `C:\Program Files\WindowsPowerShell\Modules`。
2.  更新**InstallJeaFeature**檔案，以符合您所需的 RBAC 端點設定。
3.  執行 InstallJeaFeature 來編譯 DSC 資源。
4.  將 DSC 設定部署到所有電腦，以套用設定。

下一節將說明如何使用 PowerShell 遠端處理來執行這項操作。

#### <a name="deploy-on-multiple-machines"></a>在多部電腦上部署

若要將下載的設定部署到多部電腦上，您必須更新**InstallJeaFeatures**腳本以包含適用于您環境的適當安全性群組、將檔案複製到每一部電腦，以及叫用設定腳本。
您可以使用慣用的自動化工具來完成這項操作，但本文將著重于純 PowerShell 型方法。

根據預設，設定腳本會在電腦上建立本機安全性群組，以控制每個角色的存取權。
這適用于工作組和加入網域的機器，但如果您要在僅限網域的環境中進行部署，您可能會想要將網域安全性群組與每個角色直接產生關聯。
若要將設定更新為使用網域安全性群組，請開啟**InstallJeaFeatures** ，並進行下列變更：

1.  從檔案中移除3個**群組**資源：
    1.  「群組 MS-讀取器-群組」
    2.  「群組 MS-CHAP-Hyper-v-Administrators-群組」
    3.  「群組 MS-Administrators-群組」
2.  從 JeaEndpoint **DependsOn**屬性移除3個群組資源
    1.  "[群組] MS-Readers-群組"
    2.  "[Group] MS-hyper-v-Administrators-Group"
    3.  "[群組] MS-Administrators-群組"
3.  將 JeaEndpoint **RoleDefinitions**屬性中的組名變更為您想要的安全性群組。 例如，如果您的安全性群組*CONTOSO\MyTrustedAdmins*應該被指派 Windows 系統管理中心系統管理員角色的存取權，請將 `'$env:COMPUTERNAME\Windows Admin Center Administrators'` 變更為 `'CONTOSO\MyTrustedAdmins'`。 您需要更新的三個字串為：
    1.  ' $env： COMPUTERNAME\Windows 系統管理中心系統管理員 '
    2.  ' $env： COMPUTERNAME\Windows Admin Center Hyper-v 系統管理員 '
    3.  ' $env： COMPUTERNAME\Windows 系統管理中心讀者 '

> [!NOTE]
> 請務必針對每個角色使用唯一的安全性群組。 如果將相同的安全性群組指派給多個角色，則設定會失敗。

接下來，在**InstallJeaFeatures**的結尾處，將下列幾行 PowerShell 新增至腳本的底部：

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

最後，您可以將包含模組、DSC 資源和設定的資料夾複製到每個目標節點，然後執行**InstallJeaFeature**腳本。
若要從系統管理工作站遠端執行此動作，您可以執行下列命令：

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
