---
title: 設定使用者存取控制與權限
description: 了解如何使用 Active Directory 或 Azure AD 設定使用者存取控制與權限 (Honolulu 專案)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 39af45506ff7023cebe437992e90f6d4ec051333
ms.sourcegitcommit: da6c4fa55a6a72924ac363753d04c5b682cee55b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/26/2020
ms.locfileid: "77624892"
---
# <a name="configure-user-access-control-and-permissions"></a>設定使用者存取控制與權限

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

請先熟悉 [Windows Admin Center 中的使用者存取控制選項](../plan/user-access-options.md) (如果您尚不熟悉的話)

> [!NOTE]
> 在工作群組環境中或不受信任的網域之間，不支援使用 Windows Admin Center 中的群組型存取。

## <a name="gateway-access-role-definitions"></a>閘道存取角色定義

有兩個角色可存取 Windows Admin Center 閘道服務：

**閘道使用者**可以連線到 Windows Admin Center 閘道服務，以透過該閘道來管理伺服器，但無法變更存取權限，也無法變更用來驗證閘道的驗證機制。

**閘道系統管理員**可以設定取得存取權的人員，以及使用者向閘道進行驗證的方式。 只有閘道系統管理員可以在 Windows Admin Center 中檢視和設定存取設定。 閘道機器上的本機系統管理員一律是 Windows Admin Center 閘道服務的系統管理員。

> [!NOTE]
> 可存取閘道並不表示可存取閘道可見的受控伺服器。 若要管理目標伺服器，連線的使用者必須使用具有該目標伺服器系統管理存取權的認證 (透過其傳遞式 Windows 認證，或透過使用**管理身分**在 Windows Admin Center 中提供的認證)。

## <a name="active-directory-or-local-machine-groups"></a>Active Directory 或本機機器群組

根據預設，Active Directory 或本機機器群組會用來控制閘道存取。 如果您有 Active Directory 網域，即可從 Windows Admin Center 介面中管理閘道使用者和系統管理員存取權。

在 [使用者]  索引標籤上，您可以控制誰能夠透過閘道使用者的身分存取 Windows Admin Center。 根據預設，如果您未指定安全性群組，則任何可存取閘道 URL 的使用者都有存取權。 一旦您將一個或多個安全性群組新增至使用者清單，將僅有這些群組的成員具有存取權。

如果您未在您的環境中使用 Active Directory 網域，則存取權會由 Windows Admin Center 閘道機器上的 `Users` 和 `Administrators` 本機群組所控制。

### <a name="smartcard-authentication"></a>智慧卡驗證

您可以為智慧卡安全性群組指定其他「必要」  群組，以強制執行**智慧卡驗證**。 當您新增智慧卡安全性群組之後，只有當使用者屬於任何安全性群組「和」包含在使用者清單中的智慧卡群組時，才能存取 Windows Admin Center 服務。

在 [系統管理員]  索引標籤上，您可以控制誰能夠透過閘道系統管理員的身分存取 Windows Admin Center。 電腦上的本機系統管理員群組一律具有完整的系統管理員存取權，而且無法從清單中移除。 藉由新增安全性群組，您可以將變更 Windows Admin Center 閘道設定的權限提供給這些群組的成員。 系統管理員清單支援智慧卡驗證，且方式與使用者清單相同：使用適用於安全性群組和智慧卡群組的 AND 條件。

## <a name="azure-active-directory"></a>Azure Active Directory

如果您的組織使用 Azure Active Directory (Azure AD)，您可以選擇要求使用 Azure Active Directory (Azure AD) 驗證來存取閘道，以將**其他**安全性層級加入 Windows Admin Center。 若要存取 Windows Admin Center，使用者的 **Windows 帳戶**也必須擁有閘道伺服器的存取權 (即使使用 Azure AD 驗證也是如此)。 當您使用 Azure AD 時，您會從 Azure 入口網站管理 Windows Admin Center 使用者和系統管理員的存取權限，而不是從 Windows Admin Center UI 進行管理。

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>在啟用 Azure AD 驗證時存取 Windows Admin Center

視使用的瀏覽器而定，有些存取 Windows Admin Center 且已設定 Azure AD 驗證的使用者，將會**從瀏覽器**收到額外提示，告知他們必須針對安裝 Windows Admin Center 的機器提供其 Windows 帳戶認證。 輸入該資訊之後，使用者會收到額外的 Azure Active Directory 驗證提示，要求您提供 Azure 帳戶的認證，而該帳戶必須已在 Azure 的 Azure AD 應用程式中獲得存取權。

> [!NOTE]
> 若使用者的 Windows 帳戶在閘道機器上具有**系統管理員權限**，則不會收到 Azure AD 驗證的提示。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>為 Windows Admin Center 預覽版設定 Azure Active Directory 驗證

移至 Windows Admin Center 的 [設定]   > [存取]  ，然後使用切換開關來開啟「使用 Azure Active Directory 將安全性層級新增至閘道」。 如果您尚未向 Azure 註冊閘道，系統會引導您在此時執行此動作。

根據預設，Azure AD 租用戶的所有成員都具有 Windows Admin Center 閘道服務的使用者存取權。 僅閘道機器上的本機系統管理員具有 Windows Admin Center 閘道的系統管理員存取權。 請注意，閘道機器上的本機系統管理員權限不能受到限制 - 不論您是否使用 Azure AD 進行驗證，本機系統管理員都可以執行任何動作。

如果您想要將 Windows Admin Center 服務的閘道使用者或閘道系統管理員存取權提供給特定的 Azure AD 使用者或群組，您必須執行下列動作：

1.  使用 [存取設定] 中提供的超連結，移至 Azure 入口網站中 Windows Admin Center 的 Azure AD 應用程式。 請注意，只有在啟用 Azure Active Directory 驗證時，才可以使用此超連結。 
    -   若要在 Azure 入口網站中找到您的應用程式，您可以前往 [Azure Active Directory]   > [企業應用程式]   > [所有應用程式]  ，然後搜尋 **WindowsAdminCenter** (Azure AD 應用程式將命名為 WindowsAdminCenter-<gateway name>)。 如果您沒有得到任何搜尋結果，請確定 [顯示]  已設定為 [所有應用程式]  ，而且 [應用程式狀態]  已設定為 [任何]  ，並按一下 [套用]，然後嘗試進行搜尋。 找到應用程式之後，請移至 [使用者和群組] 
2.  在 [屬性] 索引標籤中，將 [需要使用者指派]  設定為 [是]。
    完成此動作之後，將只有 [使用者和群組]  索引標籤中列出的成員，才能夠存取 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取 [新增使用者]  。 您必須為每個新增的使用者/群組指派閘道使用者或閘道系統管理員角色。

一旦您開啟 Azure AD 驗證，閘道服務就會重新啟動，而您必須重新整理瀏覽器。 您可以隨時在 Azure 入口網站中更新 SME Azure AD 應用程式的使用者存取權。

當使用者嘗試存取 Windows Admin Center 閘道 URL 時，系統會提示他們使用其 Azure Active Directory 身分識別登入。 請記住，使用者也必須是閘道伺服器上的本機使用者成員，才能存取 Windows Admin Center。

使用者和系統管理員可以從 Windows Admin Center 設定的 [帳戶]  索引標籤中，檢視其目前登入的帳戶及此 Azure AD 帳戶的登出情況。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>為 Windows Admin Center 設定 Azure Active Directory 驗證

[若要設定 Azure AD 驗證，您必須先向 Azure 註冊您的閘道](azure-integration.md) (您只需要針對您的 Windows Admin Center 閘道執行一次此動作)。 此步驟會建立 Azure AD 應用程式，您可以在其中管理閘道使用者和閘道系統管理員存取權。

如果您想要將 Windows Admin Center 服務的閘道使用者或閘道系統管理員存取權提供給特定的 Azure AD 使用者或群組，您必須執行下列動作：

1.  在 Azure 入口網站中，移至您的 SME Azure AD 應用程式。 
    -   當您按一下 [變更存取控制]  ，然後從 Windows Admin Center 存取設定中選取 [Azure Active Directory]  時，您可以使用 UI 中提供的超連結，存取 Azure 入口網站中的 Azure AD 應用程式。 當您按一下 [儲存] 並選取 Azure AD 作為存取控制身分識別提供者之後，[存取設定] 中也會提供此超連結。
    -   若要在 Azure 入口網站中找到您的應用程式，您可以前往 [Azure Active Directory]   > [企業應用程式]   > [所有應用程式]  ，然後搜尋 **SME** (Azure AD 應用程式將命名為 SME-<gateway>)。 如果您沒有得到任何搜尋結果，請確定 [顯示]  已設定為 [所有應用程式]  ，而且 [應用程式狀態]  已設定為 [任何]  ，並按一下 [套用]，然後嘗試進行搜尋。 找到應用程式之後，請移至 [使用者和群組] 
2.  在 [屬性] 索引標籤中，將 [需要使用者指派]  設定為 [是]。
    完成此動作之後，將只有 [使用者和群組]  索引標籤中列出的成員，才能夠存取 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取 [新增使用者]  。 您必須為每個新增的使用者/群組指派閘道使用者或閘道系統管理員角色。

當您在 [變更存取控制]  窗格中儲存 Azure AD 存取控制之後，閘道服務就會重新啟動，而您必須重新整理瀏覽器。 您可以隨時在 Azure 入口網站中更新 Windows Admin Center Azure AD 應用程式的使用者存取權。 

當使用者嘗試存取 Windows Admin Center 閘道 URL 時，系統會提示他們使用其 Azure Active Directory 身分識別登入。 請記住，使用者也必須是閘道伺服器上的本機使用者成員，才能存取 Windows Admin Center。 

使用者和系統管理員可以使用 Windows Admin Center 一般設定的 [Azure]  索引標籤，檢視其目前登入的帳戶及此 Azure AD 帳戶的登出情況。

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和多重要素驗證

使用 Azure AD 作為另一個安全性層級來控制 Windows Admin Center 閘道存取權的其中一項優點是，您可以運用 Azure AD 強大的安全性功能，例如條件式存取和多重要素驗證。 

[深入了解如何使用 Azure Active Directory 來設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>設定單一登入

**在部署為 Windows Server 上的服務時設定單一登入**

當您在 Windows 10 上安裝 Windows Admin Center 時，您便可以使用單一登入。 不過，如果您要在 Windows Server 上使用 Windows Admin Center，您必須在您的環境中設定某種形式的 Kerberos 委派，才能使用單一登入。 委派會將閘道電腦設定為受信任的項目，進而委派給目標節點。 

若要在您的環境中設定[以資源為基礎的限制委派](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview)，請使用下列 PowerShell 範例。 此範例會示範如何設定 Windows Server [node01.contoso.com]，以接受 contoso.com 網域中 Windows Admin Center 閘道 [wac.contoso.com] 的委派。

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount (Get-ADComputer wac)
```

若要移除此關聯性，請執行下列 Cmdlet：

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>角色型存取控制

角色型存取控制可讓您為使用者提供有限的機器存取權，而不是讓他們成為權限完整的本機系統管理員。
[深入了解角色型存取控制和可用的角色。](../plan/user-access-options.md#role-based-access-control)

設定 RBAC 包含 2 個步驟：在目標電腦上啟用支援，並將使用者指派給相關角色。

> [!TIP]
> 請確定您在要設定角色型存取控制支援的機器上具有本機系統管理員權限。

### <a name="apply-role-based-access-control-to-a-single-machine"></a>將角色型存取控制套用至單一機器

單一機器部署模型非常適合只需管理少數電腦的簡單環境。
對機器設定角色型存取控制支援將會產生下列變更：

-   具有 Windows Admin Center 所需功能的 PowerShell 模組，將會安裝在您系統磁碟機的 `C:\Program Files\WindowsPowerShell\Modules` 底下。 所有模組都將以 **Microsoft.Sme** 開頭
-   「預期狀態設定」將會執行一次性設定，以在機器上設定 Just Enough Administration 端點，並命名為 **Microsoft.Sme.PowerShell**。 此端點會定義 Windows Admin Center 所使用的 3 個角色，並在使用者與之連線時，以暫時的本機系統管理員身分執行。
-   3 個新的本機群組會隨之建立，以控制應對哪些使用者指派哪些角色的存取權：
    -   Windows Admin Center 系統管理員
    -   Windows Admin Center Hyper-V 系統管理員
    -   Windows Admin Center 讀取者

若要在單一機器上啟用角色型存取控制的支援，請遵循下列步驟：

1.  開啟 Windows Admin Center，並使用在目標機器上具有本機系統管理員權限的帳戶，連線到您想要設定角色型存取控制的機器。
2.  在 [概觀]  工具上，按一下 [設定]   > [角色型存取控制]  。
3.  按一下頁面底部的 [套用]  ，以在目標電腦上啟用角色型存取控制的支援。 應用程式程序包含複製 PowerShell 指令碼，以及叫用目標機器上的設定 (使用 PowerShell 預期狀態設定)。 此程序最多可能需要 10 分鐘的時間才能完成，並且會導致 WinRM 重新開機。 這會暫時中斷 Windows Admin Center、PowerShell 和 WMI 使用者的連線。
4.  請重新整理頁面，以檢查角色型存取控制的狀態。 當目標機器可供使用時，狀態將會變更為 [已套用]  。

套用設定之後，您可以將使用者指派給角色：

1.  開啟 [本機使用者和群組]  工具，然後瀏覽至 [群組]  索引標籤。
2.  選取 [Windows Admin Center 讀取者]  群組。
3.  在 [詳細資料]  窗格的底部，按一下 [新增使用者]  ，然後輸入應具有伺服器唯讀存取權 (透過 Windows Admin Center) 的使用者或安全性群組名稱。 使用者和群組可以來自本機機器或您的 Active Directory 網域。
4.  請針對 **Windows Admin Center Hyper-V 系統管理員**和 **Windows Admin Center 系統管理員**群組，重複步驟 2-3。

您也可以使用[有限的群組原則設定](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)來設定群組原則物件，以一致的方式在網域中填滿這些群組。

### <a name="apply-role-based-access-control-to-multiple-machines"></a>將角色型存取控制套用至多部機器

在大型企業部署中，您可以藉由從 Windows Admin Center 閘道下載設定套件，使用現有的自動化工具將角色型存取控制功能推送至您的電腦。
設定套件的主要訴求是搭配 PowerShell 預期狀態設定使用，但您也可以使用該套件來搭配您慣用的自動化解決方案。

#### <a name="download-the-role-based-access-control-configuration"></a>下載角色型存取控制設定

若要下載角色型存取控制設定套件，您必須能夠存取 Windows Admin Center 和 PowerShell 提示字元。

如果您是在 Windows Server 上的服務模式中執行 Windows Admin Cente 閘道，請使用下列命令來下載設定套件。
請務必使用適用於您環境的正確閘道位址來更新閘道。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果您是在 Windows 10 機器上執行 Windows Admin Cente 閘道，請改為執行下列命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

當您展開 zip 封存時，您會看到下列資料夾結構：

- InstallJeaFeatures.ps1
- JustEnoughAdministration (目錄)
- 模組 (目錄)
    - Microsoft.SME.\* (目錄)
    - WindowsAdminCenter.Jea (目錄)

若要在節點上設定角色型存取控制的支援，您需要執行下列動作：

1.  將 JustEnoughAdministration、Microsoft.SME.\* 和 WindowsAdminCenter.Jea 模組複製到目標機器上的 PowerShell 模組目錄。 此目錄通常位於 `C:\Program Files\WindowsPowerShell\Modules`。
2.  更新 **InstallJeaFeature.ps1** 檔案，以符合您所需的 RBAC 端點設定。
3.  執行 InstallJeaFeature.ps1 來編譯 DSC 資源。
4.  將 DSC 設定部署到所有機器以套用設定。

下一節將說明如何使用 PowerShell 遠端來執行這項操作。

#### <a name="deploy-on-multiple-machines"></a>在多部機器上部署

若要將您下載的設定部署到多部機器上，您必須更新 **InstallJeaFeatures.ps1** 指令碼，為您的環境包含適當的安全性群組、將檔案複製到每一部電腦，以及叫用設定指令碼。
您可以使用慣用的自動化工具來完成這項操作，但本文將著重於純粹以 PowerShell 為基礎的方法。

根據預設，設定指令碼會在機器上建立本機安全性群組，以控制每個角色的存取權。
這適用於工作群組和加入網域的機器，但如果您要在僅限網域的環境中進行部署，您可以讓網域安全性群組直接與每個角色產生關聯。
若要將設定更新為使用網域安全性群組，請開啟 **InstallJeaFeatures.ps1** 並進行下列變更：

1.  從檔案中移除 3 個**群組**資源：
    1.  "Group MS-Readers-Group"
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  "Group MS-Administrators-Group"
2.  從 JeaEndpoint **DependsOn** 屬性中移除 3 個群組資源
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  將 JeaEndpoint **RoleDefinitions** 屬性中的群組名變更為您想要的安全性群組。 例如，如果您有一個安全性群組：CONTOSO\MyTrustedAdmins  ，而該群組應獲派 Windows Admin Center 系統管理員角色的存取權，請將 `'$env:COMPUTERNAME\Windows Admin Center Administrators'` 變更為 `'CONTOSO\MyTrustedAdmins'`。 您需要更新的三個字串為：
    1.  '$env:COMPUTERNAME\Windows Admin Center Administrators'
    2.  '$env:COMPUTERNAME\Windows Admin Center Hyper-V Administrators'
    3.  '$env:COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> 請務必針對每個角色使用唯一的安全性群組。 如果將相同的安全性群組指派給多個角色，則設定會失敗。

接下來，在 **InstallJeaFeatures.ps1** 檔案結尾處，將下列幾行 PowerShell 新增至指令碼的底部：

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

最後，您可以將包含模組、DSC 資源和設定的資料夾複製到每個目標節點，然後執行 **InstallJeaFeature.ps1**指令碼。
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
