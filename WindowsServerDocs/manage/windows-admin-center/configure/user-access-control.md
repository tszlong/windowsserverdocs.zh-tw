---
title: 設定使用者存取控制和權限
description: 了解如何設定使用者存取控制和使用 Active Directory 或 Azure AD （專案檀香山） 的權限
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850569"
---
# <a name="configure-user-access-control-and-permissions"></a>設定使用者存取控制和權限

>適用於：Windows Admin Center，Windows Admin Center 預覽

如果您還沒有這麼做，您熟悉[Windows Admin Center 中的使用者存取控制選項](../plan/user-access-options.md)

>[!NOTE]
> 在工作群組環境中或跨未受信任的網域，不支援 Windows Admin Center 中的群組型存取。

## <a name="gateway-access-role-definitions"></a>閘道存取的角色定義

有兩個角色的 Windows Admin Center 閘道服務的存取權：

**閘道使用者**可以連線至 Windows Admin Center 閘道服務，以管理透過該閘道的伺服器，但無法變更存取權限也用來驗證閘道的驗證機制。

**閘道管理員**可以設定使用者取得存取以及如何向閘道的使用者。 只有閘道系統管理員可以檢視，並在 Windows Admin Center 中設定的存取設定。 在閘道電腦上的本機系統管理員都 Windows Admin Center 閘道服務的系統管理員。

> [!NOTE]
> 閘道存取權並不代表存取受管理的伺服器顯示閘道。 若要管理目標伺服器，連線的使用者必須使用認證 (透過其傳遞 Windows 認證或透過在 Windows Admin Center 工作階段中使用提供的認證**做為管理**動作) 具有目標伺服器的系統管理存取。

## <a name="active-directory-or-local-machine-groups"></a>Active Directory 或本機電腦群組

根據預設，Active Directory 或本機電腦群組用來控制閘道的存取。 如果您沒有 Active Directory 網域時，您可以管理閘道使用者和系統管理員的 Windows Admin Center 介面中存取。

在 [**使用者**] 索引標籤可以控制可以存取 Windows Admin Center 為閘道的使用者。 根據預設，而且如果您未指定安全性群組，任何使用者存取閘道器 URL 存取。 一旦您會將一或多個安全性群組新增至使用者清單中，存取限於那些群組的成員。

如果您未使用 Active Directory 網域環境中，存取會受到```Users```和```Administrators```Windows Admin Center 閘道機器上的本機群組。

### <a name="smartcard-authentication"></a>智慧卡驗證

您可以強制執行**智慧卡驗證**藉由指定額外_必要_群組的智慧卡為基礎的安全性群組。 一旦您加入的智慧卡為基礎的安全性群組，使用者只能存取 Windows Admin Center 服務如果它們是任何安全性群組的成員，且智慧卡群組包含在 [使用者] 清單中。

在 [**系統管理員**] 索引標籤可以控制可以存取閘道系統管理員身分的 Windows Admin Center。 在電腦上的本機系統管理員群組一律會有完整的系統管理員存取權，並不能從清單中移除。 藉由新增安全性群組，您可以讓這些群組權限的成員，變更 Windows Admin Center 閘道設定。 系統管理員清單中 [使用者] 清單的相同方式來支援智慧卡驗證： 安全性群組和智慧卡群組的 AND 條件。

## <a name="azure-active-directory"></a>Azure Active Directory

如果您的組織使用 Azure Active Directory (Azure AD)，您可以選擇新增**其他**的 Windows Admin Center 來要求存取閘道的 Azure AD 驗證的安全性層級。 若要存取 Windows Admin Center，使用者的**Windows 帳戶**（即使使用 Azure AD 驗證），則也必須有閘道伺服器的存取權。 當您使用 Azure AD 時，您要管理 Windows Admin Center 使用者和系統管理員存取權限，從 Azure 入口網站中，而不是從 Windows Admin Center UI。

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>啟用 Azure AD 驗證時，存取 Windows Admin Center

根據使用的瀏覽器，存取使用 Azure AD 驗證設定的 Windows Admin Center 部分使用者會收到額外的提示字元**從瀏覽器**，他們必須提供其 Windows 帳戶認證在其已安裝的 Windows Admin Center 的電腦。 輸入該資訊後，使用者會收到額外的 Azure Active Directory 驗證提示，這需要 Azure 帳戶已被授與存取在 Azure AD 應用程式，在 Azure 中的認證。

> [!NOTE]
> Windows 使用者帳戶具有**系統管理員權限**在閘道機器不會提示進行 Azure AD 驗證。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>設定 Azure Active Directory 驗證的 Windows Admin Center 預覽

前往 Windows Admin Center**設定** > **存取**並使用切換開關來開啟 「 使用將安全性層級新增至閘道的 Azure Active Directory"。 如果您尚未註冊至 Azure 的閘道，將會引導您這麼做的這一次。

根據預設，Azure AD 租用戶的所有成員都有使用者存取 Windows Admin Center 閘道服務。 只有本機管理員在閘道電腦上的具有系統管理員存取權的 Windows Admin Center 閘道。 請注意，在閘道電腦上的本機系統管理員的權限不能限制-本機系統管理員可以執行任何動作，不論是否使用 Azure AD 進行驗證。

如果您想要讓特定 Azure AD 使用者或群組 gateway 使用者或閘道系統管理員存取權的 Windows Admin Center 服務，您必須執行下列作業：

1.  使用提供的存取設定中的超連結，請移至您在 Azure 入口網站中的 Windows Admin Center，Azure AD 應用程式。 請注意在啟用 Azure Active Directory 驗證時，才可用這個超連結。 
    -   您也可以找到在 Azure 入口網站中您的應用程式，方法是前往**Azure Active Directory** > **企業應用程式** > **的所有應用程式**並搜尋**WindowsAdminCenter** (Azure AD 應用程式將會命名為 WindowsAdminCenter-<gateway name>)。 如果您沒有得到任何搜尋結果，請確定**顯示**設為**所有應用程式**，**應用程式狀態**設定為**任何**並按一下 [套用]然後再次嘗試您的搜尋。 一旦您找到應用程式，請移至**使用者和群組**
2.  在 [屬性] 索引標籤中，設定**需要使用者指派**為 [是]。
    一旦您已這麼做，只有成員列中**使用者和群組** 索引標籤都能夠存取的 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取**新增使用者**。 您必須指派閘道的使用者或閘道系統管理員角色，新增每個使用者/群組。

一旦您開啟 Azure AD 驗證時，閘道服務重新啟動時，您必須重新整理您的瀏覽器。 您可以更新 SME Azure AD 應用程式在 Azure 入口網站中，在任何時間的使用者存取權。

將會提示使用者在嘗試存取的 Windows Admin Center 閘道 URL 時使用其 Azure Active Directory 身分識別登入。 請記住，使用者也必須在閘道伺服器來存取 Windows Admin Center 本機 Users 的成員。

使用者和系統管理員可以檢視其目前登入的帳戶和以及來自此 Azure AD 帳戶，登出**帳戶**Windows Admin Center 設定 索引標籤。

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>設定 Azure Active Directory 驗證的 Windows Admin Center

[若要設定 Azure AD 驗證，您必須先向您的閘道 Azure](azure-integration.md) （您只需要進行一次為您的 Windows Admin Center 閘道）。 這個步驟會建立閘道的使用者和閘道系統管理員存取權，您可以管理 Azure AD 應用程式。

如果您想要讓特定 Azure AD 使用者或群組 gateway 使用者或閘道系統管理員存取權的 Windows Admin Center 服務，您必須執行下列作業：

1.  請移至您在 Azure 入口網站中的 SME Azure AD 應用程式。 
    -   當您按一下 **變更存取控制**，然後選取**Azure Active Directory**從 Windows Admin Center 存取設定中，您可以使用提供的 UI 中的超連結來存取您的 Azure AD在 Azure 入口網站中的應用程式。 此超連結也會提供在 存取設定的之後您按一下 儲存，並已選取 Azure AD 為您的存取控制身分識別提供者。
    -   您也可以找到在 Azure 入口網站中您的應用程式，方法是前往**Azure Active Directory** > **企業應用程式** > **的所有應用程式**並搜尋**SME** (Azure AD 應用程式將會命名為 SME-<gateway>)。 如果您沒有得到任何搜尋結果，請確定**顯示**設為**所有應用程式**，**應用程式狀態**設定為**任何**並按一下 [套用]然後再次嘗試您的搜尋。 一旦您找到應用程式，請移至**使用者和群組**
2.  在 [屬性] 索引標籤中，設定**需要使用者指派**為 [是]。
    一旦您已這麼做，只有成員列中**使用者和群組** 索引標籤都能夠存取的 Windows Admin Center 閘道。
3.  在 [使用者和群組] 索引標籤中，選取**新增使用者**。 您必須指派閘道的使用者或閘道系統管理員角色，新增每個使用者/群組。

一旦您儲存 Azure AD 中的存取控制**變更存取控制** 窗格中，閘道服務會重新啟動，您必須重新整理您的瀏覽器。 您可以更新 Windows Admin Center，Azure AD 應用程式在 Azure 入口網站中，在任何時間的使用者存取權。 

將會提示使用者在嘗試存取的 Windows Admin Center 閘道 URL 時使用其 Azure Active Directory 身分識別登入。 請記住，使用者也必須在閘道伺服器來存取 Windows Admin Center 本機 Users 的成員。 

使用**Azure**索引標籤中的 Windows Admin Center 一般設定、 使用者和系統管理員可以檢視其目前登入的帳戶和也為登出此 Azure AD 帳戶。

### <a name="conditional-access-and-multi-factor-authentication"></a>條件式存取和 multi-factor authentication

使用 Azure AD 作為額外，來控制存取權的 Windows Admin Center 閘道的好處之一是安全性的，您可以利用 Azure AD 的強大安全性功能，例如條件式存取和 multi-factor authentication。 

[深入了解與 Azure Active Directory 中設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>設定單一登入

**單一登入部署為 Windows 伺服器上的服務時**

當您在 Windows 10 上安裝 Windows Admin Center 時，它已準備好使用單一登入。 如果您打算使用 Windows Server 上的 Windows Admin Center，不過，您需要先設定單一登入，您可以使用某種形式的環境中的 Kerberos 委派。 委派會將閘道電腦設定為受信任可委派至目標節點。 

若要設定[資源型限制委派](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1)在環境中，執行下列 PowerShell cmdlet。 （要注意，這需要網域控制站執行 Windows Server 2012 或更新版本）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此範例中，伺服器上安裝的 Windows Admin Center 閘道**WindowsAdminCenterGW**，目標節點名稱，而且**ManagedNode**。

若要移除此關聯性，請執行下列 cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>角色型存取控制

角色型存取控制可讓您為使用者提供的有限存取權的機器，而不是將它們完整的本機系統管理員。
[深入的了解角色型存取控制和可用的角色。](../plan/user-access-options.md#role-based-access-control)

設定 RBAC 2 個步驟所組成： 啟用目標電腦上的支援，並將使用者指派給相關的角色。

> [!TIP]
> 請確定您的電腦上擁有本機系統管理員權限設定角色型存取控制的支援。

### <a name="apply-role-based-access-control-to-a-single-machine"></a>將角色型存取控制套用至一部電腦

單一電腦部署模型非常適合只有少數電腦來管理使用簡單的環境。
設定機器與角色型存取控制的支援，會導致下列變更：
-   使用函式所需的 Windows Admin Center PowerShell 模組將會安裝在您的系統磁碟機底下`C:\Program Files\WindowsPowerShell\Modules`。 所有模組會從都開始**Microsoft.Sme**
-   將執行一次性的組態來設定 Just Enough Administration 端點上名為的機器的 Desired State Configuration **Microsoft.Sme.PowerShell**。 此端點會定義 3 個 Windows Admin Center 所使用的角色，並將您的暫存的本機系統管理員身分執行，當使用者連線到它。
-   將會建立 3 個新的本機群組，來控制哪些使用者被指派哪些角色的存取權：
    -   Windows Admin Center 系統管理員
    -   Windows Admin Center HYPER-V 系統管理員
    -   Windows Admin Center 讀者

若要啟用支援在單一機器上的角色型存取控制，請遵循下列步驟：

1.  開啟 Windows Admin Center，並連接至您想要設定與使用的帳戶在目標電腦上的本機系統管理員權限的角色型存取控制的電腦。
2.  在 **概觀**工具中，按一下**設定** > **角色型存取控制**。
3.  按一下 **套用**底部的頁面，即可啟用目標電腦上的角色型存取控制的支援。 應用程式程序會包含在目標電腦上複製 PowerShell 指令碼，並叫用 （使用 PowerShell Desired State Configuration） 的組態。 可能需要 10 分鐘的時間才能完成，而且將會導致重新啟動 WinRM。 這會暫時中斷 Windows Admin Center、 PowerShell 和 WMI 的使用者的連線。
4.  重新整理頁面，即可檢查的角色型存取控制狀態。 可供使用時，狀態會變更為**套用**。

一旦套用此設定時，您可以將使用者指派給角色：

1.  開啟**本機使用者和群組**工具，並瀏覽至**群組** 索引標籤。
2.  選取  **Windows Admin Center 讀者**群組。
3.  在 *詳細資料* 窗格底部，按一下 **新增使用者**並輸入使用者或安全性群組的群組應該透過 Windows Admin Center 具有唯讀存取伺服器的名稱。 使用者和群組可以來自本機電腦或 Active Directory 網域。
4.  重複步驟 2-3 **Windows Admin Center HYPER-V 系統管理員**並**Windows Admin Center 系統管理員**群組。

您也可以填入這些群組以一致的方式在您的網域設定群組原則物件與[限制的群組原則設定](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)。

### <a name="apply-role-based-access-control-to-multiple-machines"></a>將角色型存取控制套用至多部電腦

在大型企業部署中，您可以使用現有的自動化工具，若要推送的角色型存取控制功能至您的電腦從 Windows Admin Center 閘道下載設定套件。
組態套件旨在搭配 PowerShell Desired State Configuration，但您可以調整它以搭配您慣用的自動化解決方案。

#### <a name="download-the-role-based-access-control-configuration"></a>下載角色型存取控制設定

若要下載的角色型存取控制設定套件，您必須能夠存取 Windows Admin Center 和 PowerShell 提示字元。

如果您在服務模式中執行的 Windows Admin Center 閘道，在 Windows Server 上，使用下列命令來下載組態封裝。
請務必更新您的環境適用的正確訂的閘道位址。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果您在 Windows 10 電腦上執行的 Windows Admin Center 閘道，請改為執行下列命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

當您展開 zip 封存時，您會看到下列資料夾結構：
- InstallJeaFeatures.ps1
- JustEnoughAdministration （目錄）
- 模組 （目錄）
    - Microsoft.SME。\* （目錄）
    - WindowsAdminCenter.Jea (directory)

若要設定的節點上的角色型存取控制支援，您需要執行下列動作：
1.  將複製的 JustEnoughAdministration Microsoft.SME。\*，和在目標電腦上的 PowerShell 模組目錄的 WindowsAdminCenter.Jea 模組。 一般而言，這是位於`C:\Program Files\WindowsPowerShell\Modules`。
2.  更新**InstallJeaFeature.ps1**檔案，以符合您所需的組態，RBAC 端點。
3.  執行 InstallJeaFeature.ps1 編譯 DSC 資源。
4.  將您的 DSC 組態部署到您所有的機器套用設定。

下一節會說明如何使用 PowerShell 遠端執行此動作。

#### <a name="deploy-on-multiple-machines"></a>部署多部電腦上

若要部署您下載到多部電腦上的組態，您將需要更新**InstallJeaFeatures.ps1**指令碼，包括為您的環境適當的安全性群組，將檔案複製到每台電腦，然後叫用的設定指令碼。
不過，本文將著重在純 PowerShell 為基礎的方法，您可以達成此目的，使用您慣用的自動化工具。

根據預設，設定指令碼會建立本機安全性群組來控制存取權的每個角色的電腦上。
這是適用於工作群組和網域加入機器，但如果您要部署可能會想要直接為僅在網域上執行的環境中建立網域安全性群組關聯與每個角色。
若要更新組態以使用網域安全性群組，開啟**InstallJeaFeatures.ps1**並進行下列變更：

1.  移除 3**群組**檔案中的資源：
    1.  [群組-群組 MS-讀取器]
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  「 群組 MS-系統管理員群組的 」
2.  3 個群組的資源移除 JeaEndpoint **DependsOn**屬性
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  變更群組名稱在 JeaEndpoint **RoleDefinitions**屬性至所需的安全性群組。 例如，如果您有安全性群組*CONTOSO\MyTrustedAdmins*應為 Windows Admin Center 系統管理員角色，變更指派給存取`'$env:COMPUTERNAME\Windows Admin Center Administrators'`至`'CONTOSO\MyTrustedAdmins'`。 若要更新所需的三個字串是：
    1.  ' $env: COMPUTERNAME\Windows Admin Center 系統管理員
    2.  ' $env: COMPUTERNAME\Windows Admin Center HYPER-V 系統管理員
    3.  ' $env: COMPUTERNAME\Windows Admin Center 讀者

> [!NOTE]
> 請務必使用唯一的安全性群組，每個角色。 如果相同的安全性群組指派給多個角色，組態將會失敗。

接下來，在結尾**InstallJeaFeatures.ps1**檔案中，新增下列幾行的 PowerShell 指令碼的底部：

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

最後，您可以複製包含模組、 DSC 資源和設定，以每個目標節點的資料夾，並執行**InstallJeaFeature.ps1**指令碼。
若要這樣做從遠端管理工作站，您可以執行下列命令：

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
