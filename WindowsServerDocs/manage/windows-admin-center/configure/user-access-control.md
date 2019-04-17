---
title: 設定使用者存取控制與權限
description: 了解如何設定使用者存取控制與權限使用 Active Directory 或 Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152043"
---
# 設定使用者存取控制與權限

>適用於：Windows Admin Center、Windows Admin Center 預覽版

如果您還沒有這樣做，先熟悉[Windows Admin Center 中的使用者存取控制選項](../plan/user-access-options.md)

>[!NOTE]
> 在工作群組環境中或不受信任的網域上不支援群組基礎 Windows Admin Center 中的存取權。

## 閘道存取角色定義

有兩個角色來存取 Windows Admin Center 閘道服務：

**閘道使用者**可以連線到 Windows Admin Center 閘道服務，來管理伺服器，透過該閘道，但是他們無法變更存取權限，也不用來驗證對閘道的驗證機制。

**閘道系統管理員**可以設定讓哪些人員取得存取以及如何至閘道的使用者進行驗證。 只有閘道的系統管理員可檢視和設定 Windows Admin Center 中的存取權設定。 在閘道電腦上的本機系統管理員一律是 Windows Admin Center 閘道服務的系統管理員。

> [!NOTE]
> 閘道存取並不代表閘道的可見的受管理伺服器的存取權。 若要管理目標伺服器，請連線的使用者必須使用的認證 （無論是透過其傳遞透過 Windows 認證或在 Windows Admin Center 工作階段中使用**做為管理**動作提供認證） 的系統管理權限到該目標伺服器。

## Active Directory 或本機電腦群組

根據預設，Active Directory 或本機電腦群組用來控制閘道的存取。 如果您有 Active Directory 網域，您可以管理閘道的使用者和系統管理員從 Windows Admin Center 介面內存取。

**使用者**] 索引標籤上，您可以控制誰可以存取 Windows Admin Center 閘道使用者身分。 根據預設，而且如果您沒有指定安全性群組，存取閘道 URL 的任何使用者存取。 一旦您到使用者清單中新增一或多個安全性群組，存取權是以限制為那些群組的成員。

如果您不使用 Active Directory 網域，您的環境中，由控制存取權```Users```和```Administrators```在 Windows Admin Center 閘道電腦上的本機群組。

### 智慧卡驗證

您可以藉由指定其他_必要_群組的智慧卡為基礎的安全性群組強制**智慧卡驗證**。 一旦您已新增的智慧卡為基礎的安全性群組，使用者才能存取 Windows Admin Center 服務如果它們是任何安全性群組的成員，而且使用者清單中包含的智慧卡群組。

**系統管理員**] 索引標籤上，您可以控制誰可以存取 Windows Admin Center 閘道系統管理員身分。 在電腦上的本機系統管理員群組一定會有完整的系統管理員存取權，並無法從清單中移除。 藉由新增安全性群組，您必須提供這些群組的權限的成員，來變更 Windows Admin Center 閘道設定。 系統管理員清單中的使用者清單與相同的方式支援智慧卡驗證： 安全性群組與智慧卡群組 AND 條件。

## Azure Active Directory

如果您的組織使用 Azure Active Directory (Azure AD)，您可以選擇將**額外**的安全性層級新增至 Windows Admin Center，藉由需要 Azure AD 驗證來存取閘道。 才能存取 Windows Admin Center，使用者的**Windows 帳戶**也必須閘道伺服器的存取 （即使使用 Azure AD 驗證）。 當您使用 Azure AD 時，您將會管理 Windows Admin Center 使用者和系統管理員存取權限，從 Azure 入口網站，而不是從 Windows Admin Center UI。

### 啟用 Azure AD 驗證時，存取 Windows Admin Center

根據使用的瀏覽器，某些使用者與設定 Azure AD 驗證的存取 Windows Admin Center 將會收到額外提示**從瀏覽器**，它們需要提供他們的 Windows 帳戶認證的電腦上的Windows Admin Center 會安裝。 輸入該資訊後，使用者會取得額外的 Azure Active Directory 驗證提示，要求已被授與存取 Azure AD 應用程式在 Azure 中的 Azure 帳戶的認證。

> [!NOTE]
> 誰的 Windows 帳戶具有**系統管理員權限**在閘道電腦將不會提示您的 Azure AD 驗證的使用者。

### 設定 Azure Active Directory 驗證的 Windows Admin Center 預覽版

移至 Windows Admin Center**設定** > **存取**和使用切換開關來開啟 「 使用 Azure Active Directory，以新增至閘道的安全性層級 」。 如果您沒有註冊至 Azure 閘道，您將會引導您要這樣做，在此階段。

根據預設，Azure AD 租用戶中的所有成員會都具有使用者存取 Windows Admin Center 閘道服務。 只有在閘道電腦上的本機系統管理員有系統管理員存取權的 Windows Admin Center 閘道。 請注意，不可受限制的閘道電腦上的本機系統管理員權限-的本機系統管理員可以執行任何無論是否使用 Azure AD 進行驗證。

如果您想要提供給特定的 Azure AD 使用者或群組閘道使用者或服務 Windows Admin Center 閘道系統管理員存取權，您必須執行下列動作：

1.  藉由提供存取權設定中的超連結，請移至您的 Windows Admin Center Azure AD 應用程式，在 Azure 入口網站。 請注意，Azure Active Directory 驗證已啟用時，才可以使用此超連結。 
    -   您也可以找到 Azure 入口網站中您的應用程式，移至**Azure Active Directory** > **企業應用程式** > **的所有應用程式**和搜尋**WindowsAdminCenter** （Azure AD 應用程式將會在名為WindowsAdminCenter-<gateway name>)。 如果您沒有取得任何搜尋結果，確保**顯示**設為 [**所有應用程式**，**應用程式狀態**設定為 [**任何**並按一下 [套用]，然後嘗試將搜尋。 一旦您找到應用程式，請移至 [**使用者和群組**
2.  在 [屬性] 索引標籤中，設定**所需的使用者指派**為 [是]。
    一旦您已完成此作業，列出**使用者和群組**] 索引標籤中的成員將能夠存取 Windows Admin Center 閘道。
3.  在使用者和群組] 索引標籤中，選取 [**新增使用者**。 您必須為閘道使用者或每個使用者/群組新增閘道系統管理員角色指派。

一旦您開啟 Azure AD 驗證，閘道服務重新啟動，您必須重新整理您的瀏覽器。 您可以更新 SME Azure AD 應用程式在 Azure 入口網站，在任何時候使用者存取。

將會提示使用者在嘗試存取 Windows Admin Center 閘道 URL 時，使用他們的 Azure Active Directory 身分識別登入。 請記住，使用者也必須是存取 Windows Admin Center 閘道伺服器上的本機使用者的成員。

使用者和系統管理員可以檢視其目前登入的帳戶，並也一樣登出後此 Azure AD 的帳戶從 [**帳戶**] 索引標籤的 Windows 系統管理員中心設定。

### 設定 Azure Active Directory 驗證 Windows Admin center

[若要設定 Azure AD 驗證，您必須先登錄與 Azure 閘道](azure-integration.md)（您只需要針對您的 Windows Admin Center 閘道執行此一次動作）。 這個步驟建立的 Azure AD 應用程式，您可以從中管理閘道的使用者和閘道系統管理員存取權。

如果您想要提供給特定的 Azure AD 使用者或群組閘道使用者或服務 Windows Admin Center 閘道系統管理員存取權，您必須執行下列動作：

1.  移至您在 Azure 入口網站的 SME Azure AD 應用程式。 
    -   當您按一下 [**變更存取控制**，然後從 Windows 系統管理員中心存取權設定中選取 [ **Azure Active Directory**時，您可以使用 UI 中提供的超連結，來存取您的 Azure AD 應用程式，在 Azure 入口網站。 此超連結也會提供存取權設定中的之後您按一下 [儲存]，並已選取 Azure AD 做為您的存取控制身分識別提供者。
    -   您也可以找到 Azure 入口網站中您的應用程式，移至**Azure Active Directory** > **企業應用程式** > **的所有應用程式**和搜尋**SME** (Azure AD 應用程式將名為 SME-<gateway>)。 如果您沒有取得任何搜尋結果，確保**顯示**設為 [**所有應用程式**，**應用程式狀態**設定為 [**任何**並按一下 [套用]，然後嘗試將搜尋。 一旦您找到應用程式，請移至 [**使用者和群組**
2.  在 [屬性] 索引標籤中，設定**所需的使用者指派**為 [是]。
    一旦您已完成此作業，列出**使用者和群組**] 索引標籤中的成員將能夠存取 Windows Admin Center 閘道。
3.  在使用者和群組] 索引標籤中，選取 [**新增使用者**。 您必須為閘道使用者或每個使用者/群組新增閘道系統管理員角色指派。

一旦您儲存的 Azure AD 存取控制在**變更存取控制**窗格中，在閘道服務重新啟動，您必須重新整理您的瀏覽器。 您可以更新隨時都在 Azure 入口網站中的 Windows Admin Center Azure AD 應用程式的使用者存取。 

將會提示使用者在嘗試存取 Windows Admin Center 閘道 URL 時，使用他們的 Azure Active Directory 身分識別登入。 請記住，使用者也必須是存取 Windows Admin Center 閘道伺服器上的本機使用者的成員。 

使用 Windows Admin Center 一般設定中的 [ **Azure** ] 索引標籤，使用者和系統管理員可以檢視其目前登入的帳戶並也一樣登出此 Azure AD 帳戶。

### 條件式存取和多重要素驗證

其中一個額外層級為使用 Azure AD，來控制存取 Windows Admin Center 閘道的好處是安全性的，您可以利用 Azure AD 功能強大的安全性功能，例如條件式存取和多重要素驗證。 

[深入了解使用 Azure Active Directory 中設定條件式存取。](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## 設定單一登入

**單一登入時即服務，Windows Server 上部署**

當您在 Windows 10 上安裝 Windows Admin Center 時，它已經準備好使用單一登入。 不過，如果您要在 Windows Server 上使用 Windows Admin Center，您需要設定某種形式的 Kerberos 委派，您的環境中，您可以使用單一登入之前。 委派會閘道電腦設定為信任委派給目標節點。 

若要設定[的資源為基礎的限制委派](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1)您環境中，執行下列 PowerShell cmdlet。 （請注意，這需要執行 Windows Server 2012 網域控制站或更新版本）。

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

在此範例中，Windows Admin Center 閘道伺服器**WindowsAdminCenterGW**上, 已安裝並目標節點名稱**ManagedNode**。

若要移除這種關係，請執行下列 cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## 角色型存取控制

角色型存取控制可讓您為使用者提供具有有限的存取權，而不是讓它們的完整本機系統管理員的電腦。
[角色型存取控制和可用的角色的相關的閱讀更多。](../plan/user-access-options.md#role-based-access-control)

設定 RBAC 2 個步驟所組成： 啟用目標電腦上的支援，並將使用者指派至相關的角色。

> [!TIP]
> 請確定您擁有的電腦上的本機系統管理員權限設定角色型存取控制的支援。

### 適用於在單一電腦角色型存取控制

單一電腦部署模型最適合使用簡單的環境使用只有幾個電腦來管理。
角色型存取控制有關的支援設定電腦將會導致下列變更：
-   PowerShell 模組中使用 Windows Admin Center 所需要的函式將會安裝在系統磁碟機，在`C:\Program Files\WindowsPowerShell\Modules`。 所有模組將會都開始**Microsoft.Sme**
-   所需的狀態設定將會執行一次在電腦上，名為**Microsoft.Sme.PowerShell**設定 Just Enough Administration 端點組態。 此端點定義 Windows Admin Center 所使用的 3 個角色，並將您的暫時的本機系統管理員身分執行，當使用者連接到它。
-   3 個新的本機群組將會建立來控制哪些使用者會受指派存取權哪些角色：
    -   Windows Admin Center 系統管理員
    -   Windows Admin Center HYPER-V 系統管理員
    -   Windows Admin Center 讀卡機

若要啟用支援在單一電腦上的角色型存取控制，請依照下列步驟：

1.  開啟 Windows Admin Center，並連線到您想要使用角色型存取控制使用具有目標電腦上的本機系統管理員權限的帳戶設定的電腦。
2.  在 [**概觀**] 工具中，按一下 [**設定** > **角色型存取控制**。
3.  按一下 [**套用**在頁面底部以啟用目標電腦上的角色型存取控制的支援。 應用程式處理程序會涉及複製的 PowerShell 指令碼，並叫用 （使用 PowerShell 預期狀態設定） 的設定目標電腦上。 它可能需要 10 分鐘的時間才能完成，並且會導致 WinRM 重新啟動。 這將會暫時中斷連接 Windows Admin Center、 PowerShell 和 WMI 的使用者。
4.  重新整理頁面以檢查角色型存取控制的狀態。 可供使用時，將會變更**Applied**狀態。

一旦套用設定時，您可以為使用者指派角色：

1.  開啟**本機使用者和群組**工具，並瀏覽至 [**群組**] 索引標籤。
2.  選取**Windows Admin Center 讀卡機**群組。
3.  在底部*的詳細資料*窗格中，按一下 [**新增使用者**，並輸入應該具有唯讀存取伺服器，透過 Windows Admin Center 使用者或安全性群組群組的名稱。 使用者和群組可以來自本機電腦或您的 Active Directory 網域。
4.  在**Windows Admin Center HYPER-V 系統管理員**和**Windows Admin Center 系統管理員**群組重複步驟 2-3。

您可以也填入這些群組以一致的方式在您的網域使用[受限制的群組原則設定](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29)設定群組原則物件。

### 角色型存取控制套用到多部電腦

在大型企業部署中，您可以使用現有的自動化工具將角色型存取控制功能資訊推送到您的電腦藉由從 Windows Admin Center 閘道下載設定套件。
設定套件設計來搭配 PowerShell 預期狀態設定，但您可以調整它搭配使用您慣用的自動化的解決方案。

#### 下載角色型存取控制設定

若要下載角色型存取控制設定套件，您將需要存取 Windows Admin Center 和 PowerShell 命令提示字元。

如果您正在執行 Windows Admin Center 閘道服務模式中，Windows Server 上，使用下列命令來下載設定套件。
請務必使用正確的一個用於您的環境中更新閘道位址。

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

如果您執行 Windows 10 電腦上的 Windows Admin Center 閘道，請改為執行下列命令：

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

當您將該 zip 封存展開時，您會看到下列資料夾結構：
- InstallJeaFeatures.ps1
- JustEnoughAdministration （目錄）
- 模組 （目錄）
    - Microsoft.SME.\* （目錄）
    - WindowsAdminCenter.Jea （目錄）

若要設定的節點上的角色型存取控制的支援，您需要執行下列動作：
1.  將 JustEnoughAdministration、 Microsoft.SME.\*，以及 WindowsAdminCenter.Jea 模組複製到目標電腦上的 PowerShell 模組目錄中。 這通常是位於`C:\Program Files\WindowsPowerShell\Modules`。
2.  更新**InstallJeaFeature.ps1**檔案，以符合您所需的組態 RBAC 端點。
3.  執行 InstallJeaFeature.ps1 編譯 DSC 資源。
4.  將您的 DSC 設定部署到所有機器套用的設定。

下一節會說明如何使用 PowerShell 遠端執行功能這麼做。

#### 在多部電腦上部署

若要部署到多部電腦上下載的設定，您必須更新以包含您的環境，適當的安全性群組**InstallJeaFeatures.ps1**指令碼檔案複製到每個您的電腦，並叫用設定指令碼。
不過這篇文章將重點放在純 PowerShell 為基礎的方法，您可以使用您慣用的自動化工具，若要這樣做。

根據預設，設定指令碼會建立來控制存取每個角色在電腦上的本機安全性群組。
這是適用於工作群組和網域加入的電腦，但如果您要部署在僅限網域環境中可能會想要直接關聯至網域的安全性群組每個角色。
若要更新設定，以使用網域的安全性群組，開啟**InstallJeaFeatures.ps1**並進行下列變更：

1.  從檔案中移除的 3 個**群組**的資源：
    1.  「 群組 MS 讀卡機-群組 」
    2.  「 群組 MS 超-V-系統管理員-群組 」
    3.  「 群組 MS-系統管理員群組的 」
2.  從 JeaEndpoint **DependsOn**屬性中移除的 3 個群組資源
    1.  「 [群組] MS 讀卡機群組 」
    2.  「 [群組] MS 超-V-系統管理員-群組 」
    3.  「 [群組] MS Administrators 群組 」
3.  您想要的安全性群組來變更 JeaEndpoint **RoleDefinitions**屬性中的群組名稱。 例如，如果您有*CONTOSO\MyTrustedAdmins*應該 Windows Admin Center 系統管理員角色來指派存取權的安全性群組，變更`'$env:COMPUTERNAME\Windows Admin Center Administrators'`到`'CONTOSO\MyTrustedAdmins'`。 您需要更新三個字串都是：
    1.  ' $env: COMPUTERNAME\Windows 系統管理員中心系統管理員
    2.  ' $env: COMPUTERNAME\Windows 系統管理員中心 HYPER-V 系統管理員
    3.  ' $env: COMPUTERNAME\Windows 系統管理員中心讀卡機 '

> [!NOTE]
> 請務必針對每個角色使用唯一的安全性群組。 如果多個角色來指派相同的安全性群組，設定將會失敗。

接下來， **InstallJeaFeatures.ps1**檔案的結尾加入下列行 PowerShell 底部的指令碼：

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

最後，您可以複製包含模組、 DSC 資源和每個目標節點來設定的資料夾，並執行**InstallJeaFeature.ps1**指令碼。
若要這樣做從遠端從您的系統管理員工作站，您可以執行下列命令：

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
