---
title: 針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 1909 最佳化
description: 將當作 VDI 映像的 Windows 10 版本 1909 桌面的額外負荷降至最低的建議設定和組態。
ms.reviewer: robsmi
ms.author: helohr
ms.topic: article
author: heidilohr
manager: lizross
ms.date: 02/19/2020
ms.openlocfilehash: eeadbdea10f08372cd927808b4b433d8ba7ee85f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037826"
---
# <a name="optimizing-windows-10-version-1909-for-a-virtual-desktop-infrastructure-vdi-role"></a>針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 1909 最佳化

本文可協助您針對 Windows 10 版本 1909 (組建 18363) 選擇適當設定，以在在虛擬桌面基礎結構 (VDI) 環境中達到最佳的效能。 本指南中的所有設定皆為供您參考的建議，而並非需求。

在 VDI 環境中，將 Windows 10 效能最佳化的主要方式為盡可能減少應用程式圖形重新繪製、減少對 VDI 環境沒有顯著效益的背景活動，以及將整體執行的處理序減少至最低限度。 次要目標是要將基底映像中的磁碟空間使用量降至最低限度。 透過 VDI 實作，可能的最小基底 (或「黃金」映像大小) 可稍微減少 Hypervisor 的記憶體使用量，並且可小幅減少將桌面映像提供給取用者所需的整體網路作業。

> [!NOTE]
> 這些建議的設定可套用至 Windows 10 版本 1909 的其他安裝，包括實體或其他虛擬裝置上的安裝。 本主題中的建議並不會對 Windows 10 版本 1909 的支援能力產生影響。

## <a name="vdi-optimization-principles"></a>VDI 最佳化原則

VDI 環境可透過網路為電腦使用者提供完整的桌面工作階段，包括應用程式。 網路傳遞車輛可以是內部部署網路或網際網路。 VDI 環境會使用「基礎」作業系統映像，而此映像後續將成為桌面的基礎，呈現在使用者面前。 VDI 實作有多種變化，例如「持續性」、「非持續性」和「桌面工作階段」。 持續性類型會將 VDI 桌面 OS 的變更保留到下一個工作階段。 非持續性類型不會將 VDI 桌面 OS 的變更保留到下一個工作階段。 對使用者而言，此桌面除了可透過網路存取以外，與其他虛擬或實體裝置相較並沒有太大的不同。

最佳化設定會在參考裝置上進行。 VM 是建置映像的理想位置，因為您可以在這裡儲存狀態、建立檢查點並進行備份。 預設的 OS 安裝是在基礎 VM 上執行。 該基礎 VM 接著會移除不必要的應用程式、安裝 Windows 更新、安裝其他更新、刪除暫存檔案，以及套用設定來進行最佳化。

還有其他類型的 VDI，例如遠端桌面工作模式 (RDS) 和最近發行的 [Windows 虛擬桌面](https://azure.microsoft.com/services/virtual-desktop/)。 有關這些技術的深入討論已超出本文的範圍。 本文著重於 Windows 基礎映像設定，而不需參考環境中的其他因素，例如主機最佳化。

對於其產品和服務，安全性和穩定性一向是 Microsoft 首要的優先考慮項目。 企業客戶可以選擇利用內建的 Windows 安全性，這是一套服務，無論是否具備網際網路均可使用。 對於未連線到網際網路的 VDI 環境，系統每天可以下載安全性簽章數次，因為 Microsoft 可能每天會發行一個以上的簽章更新。 接著，您可以將這些簽章提供給 VDI VM，並排程在生產期間進行安裝，無論持續性或非持續性均適用。 如此一來，VM 保護就會盡可能維持在最新狀態。

有些安全性設定不適用於未連線到網際網路的 VDI 環境，因此無法參與具備雲端功能的安全性。 還有其他「正常」 Windows 裝置可能會利用的各項設定，例如雲端體驗、Windows Store 等等。 移除未使用功能的存取，可減少使用量、降低網路頻寬和受到攻擊的可能性。

Windows 10 會利用每月更新演算法進行更新，因此用戶端無需嘗試更新。 在大多數情況下，VDI 管理員會透過關閉以「主要」或「黃金」映像為基礎的 VM 來控制更新的流程，將唯讀的映像解密、修補映像，然後重新密封並帶回生產環境。 因此，VDI VM 不需要檢查 Windows Update。 例如，在某些情況下，持續性 VDI VM 會執行一般的修補程序。 您也可以使用 Windows Update 或 Microsoft Intune。 System Center Configuration Manager 可以用來處理更新和其他套件傳遞。 每個組織可以自行決定最適用的 VDI 更新方法。

> [!TIP]
> 在本主題的討論中用來實作最佳化的指令碼，以及可使用 **LGPO.exe** 匯入的 GPO 匯出檔案，皆可從 GitHub 上的 [TheVDIGuys](https://github.com/TheVDIGuys) 取得。

此指令碼專為符合您的環境和需求而設計。 主要的程式碼是 PowerShell，並使用輸入檔案 (純文字) 與本機群組原則物件 (LGPO) 工具匯出檔案來完成作業。 這些檔案包含要移除的應用程式清單，以及要停用的各項服務清單。 如果您不想移除特定的應用程式或停用特定的服務，請編輯對應的文字檔，並移除該項目。 最後，您可以將本機原則設定匯入您的裝置。 建議您在基礎映像中使用某些設定，而不是透過群組原則套用設定；因為某些設定會在下次重新開機時生效，或在第一次使用元件時生效。

### <a name="persistent-vdi"></a>持續性 VDI

持續性 VDI 屬於基本層級，是會在重新開機後仍保留作業系統狀態的 VM。 VDI 解決方案的其他軟體層可讓使用者輕鬆且順暢地存取其指派的 VM (通常是使用單一登入解決方案)。

持續 VDI 有數種不同的實作：

- 傳統虛擬機器的 VM 具有其本身的虛擬磁碟檔案，可以正常啟動，並將一個工作階段的變更儲存到下一個工作階段。 其差別在於使用者存取此 VM 的方式。 可能會有 Web 入口網站，會在使用者登入後自動將其導向至其一或多個指派的 VDI VM。

- 依需求可搭配個人虛擬磁碟的映像式持續性虛擬機器。 在此類型的實作中，一或多個主機伺服器上會有基底/黃金映像。 在 VM 建立後，會建立一或多個虛擬磁碟，並指派給此磁碟供持續性儲存之用。

    - 在 VM 啟動時，系統會將基礎映像的複本讀取到 VM 的記憶體中。 同時，持續性虛擬磁碟會連同透過複雜的程序合併的任何既有作業系統變更，指派給該 VM。

    - 事件記錄寫入、記錄寫入等變更會重新導向至已指派給該 VM 的讀取/寫入虛擬磁碟。

    - 在此情況下，作業系統和應用程式維護可使用傳統維護軟體 (例如 Windows Server Update Services) 或其他管理技術正常運作。

    - 持續性 VDI 機器和「一般」虛擬機器之間的差異在於主要/黃金映像的關係。 在某些時候，更新必須套用至主要映像。 實作可在此決定如何處理使用者持續性變更。 在某些情況下，系統會捨棄變更過的磁碟並 (或) 重設，因此會設定新的檢查點。 也可能使用者所做的變更會透過每月的品質更新保留，而基礎會在功能更新之後重設。

### <a name="non-persistent-vdi"></a>非持續性 VDI

非持續性 VDI 實作以基底或「黃金」映像為基礎時，最佳化主要會在基底映像上執行，其次才是透過本機設定與本機原則執行。

在映像式非持續性 VDI 中，基底映像是唯讀的。 在非持續性 VM 啟動時，系統會將基礎映像的複本串流處理至 VM。 在啟動期間及其後到下次重新開機之間所發生的活動，都會重新導向至暫存位置。 使用者通常會獲得用來儲存其資料的網路位置。 在某些情況下，使用者的設定檔會與標準 VM 合併，而為使用者提供其設定。

對於以單一映像為基礎的非持續性 VDI 而言，維護是其重要層面之一。 作業系統和元件的更新通常會每個月提供一次。 在映像式 VDI，要取得映像的更新必須執行一組程序：

- 在指定的主機上，所有衍生自基礎映像的 VM 都必須關閉。 這表示使用者會重新導向至其他 VM。

- 隨後，基底映像會開啟並啟動。 接著就會執行所有的維護活動，例如作業系統更新、.NET 更新、應用程式更新等。

- 任何要套用的新設定都會在此時套用。

- 任何其他維護也都會在此時執行。

- 其後，基底映像會關閉。

- 基底映像會進行密封，並依設定回到生產環境。

- 使用者可以重新登入。

> [!NOTE]
> Windows 10 會定期自動執行一組維護工作。 有一項排定工作依預設會在每天凌晨 3:00 執行。 這項排定工作會執行一系列的工作，包括 Windows Update 清理。 您可以使用下列 PowerShell 命令，檢視所有會自動執行的維護工作類別：
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

非持續性 VDI 的挑戰之一，是當使用者登出時，幾乎所有的作業系統活動都會捨棄。 使用者的設定檔和/或狀態可能會集中儲存到一個位置，但虛擬機器本身會捨棄前次開機後所做的絕大多數變更。 因此，預計要對可將狀態保留到下一個工作階段的 Windows 電腦進行的最佳化，將不適用。

根據 VM VDI 的架構，PreFetch 和 SuperFetch 等功能都無法利用跨工作階段的持續性來提供協助，因為在 VM 重新啟動後，所有最佳化都會被捨棄。 編製索引可能會浪費資源，傳統磁碟重組之類的任何磁碟最佳化也是一樣。

> [!NOTE]
> 如果使用虛擬化來準備映像，而且在映像建立過程中連線到網際網路，則在第一次登入時，您應該前往**設定** **Windows Update** 來延遲功能更新。

### <a name="to-sysprep-or-not-sysprep"></a>是否使用 Sysprep

Windows 10 有一項名為[系統準備工具](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)的內建功能 (常簡稱為 "Sysprep")。 Sysprep 工具可用來為自訂的 Windows 10 映像進行複製準備。 Sysprep 程序可確保產生的作業系統會有適當的獨特性可在生產環境中執行。

執行 Sysprep 各有其優缺點。 以 VDI 為例，您可能會希望能夠自訂預設使用者設定檔，以供後續使用此映像登入的使用者作為設定檔範本。 您可能會想要安裝某些應用程式，但同時又希望能控制個別應用程式的設定。

替代方式是使用標準 .ISO 進行安裝；您可以搭配使用自動的安裝回應檔案，以及用來安裝應用程式或移除應用程式的工作序列。 您也可以使用工作序列來設定映像中的本機原則設定；或許可搭配使用[本機群組原則物件公用程式 (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0) 工具。

### <a name="supportability"></a>可支援性

每當 Windows 預設值變更時，就會發生有關可支援性的問題。 自訂 VDI 映像 (VM 或工作階段) 之後，每次對映像所做的變更都必須在變更記錄檔中追蹤。 進行疑難排解時，通常會在集區中隔離映像並設定問題分析。 一旦追蹤到問題的根本原因之後，系統會先將該變更推到測試環境，最後再到生產工作負載。

本文件刻意避免觸及影響安全性的系統服務、原則或工作。 之後就是 Windows 服務。 已移除維護期間以外的 VDI 映像服務，因為維護期間會在 VDI 環境中進行大部分服務事件時發生，「除了」  安全性軟體更新。 Microsoft 已針對 VDI 環境中的 Windows 安全性發佈指引。 如需詳細資訊，請參閱[在虛擬桌面基礎結構 (VDI) 環境中部署 Windows Defender 防毒軟體的指南](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)。

改變預設的 Windows 設定時，請考慮可支援性。 以強化或加速等名義更改系統服務、原則或排定的工作時，可能會導致問題。請參閱 Microsoft 知識庫，以取得有關更改預設設定的目前已知問題。 本文件中的指導方針以及 GitHub 上相關的程式碼，將會在已知問題發生時持續更新。 此外，您可以透過數種方式向 Microsoft 回報問題。

您可以使用慣用的搜尋引擎，使用 “"start value" site:support.microsoft.com” 一詞，提出有關服務預設起始值的已知問題。

您可能會注意到，本文件和 GitHub 上相關的指令碼並不會修改任何預設權限。 如果您想增加安全性設定，請從名為 **AaronLocker** 的專案開始。 如需詳細資訊，請參閱[公告：具有 "AaronLocker" 的應用程式允許清單](/archive/blogs/aaron_margosis/announcing-application-whitelisting-with-aaronlocker)。

#### <a name="vdi-optimization-categories"></a>VDI 最佳化類別

- 全域作業系統設定類別：

    - UWP 應用程式清理

    - 選用功能清理

    - 本機原則設定

    - 系統服務

    - 排定的工作

    - 套用 Windows (和其他) 更新

    - 自動 Windows 追蹤

    - 在完成 (密封) 映像之前清理磁碟

    - 使用者設定

    - Hypervisor/主機設定

### <a name="universal-windows-platform-uwp-application-cleanup"></a>通用 Windows 平台 (UWP) 應用程式清除

VDI 映像的目標之一是要越精簡越好。 縮小映像大小的方式之一，是移除環境中不會使用的 UWP 應用程式。 使用 UWP 應用程式時，會有主要的應用程式檔案，也就是承載。 每個使用者設定檔中都會儲存少量的資料，供應用程式的特定設定使用。 此外，「所有使用者」設定檔中也會有少量的資料。

就 UWP 應用程式清理而言，連線能力和時機是重要的因素。 如果您將基礎映像部署到沒有網路連線的裝置，在您嘗試將應用程式解除安裝時，Windows 10 將無法連線至 Microsoft Store 下載應用程式並嘗試加以安裝。 這項策略可讓您有時間自訂映像，然後在建立映像程序的後續階段中更新其餘的內容。

如果您修改用來安裝 Windows 10 的基礎 .WIM，並在安裝之前從 .WIM 中移除了不必要的 UWP 應用程式，則這些應用程式將不會安裝並啟動，而您的設定檔建立時間應該會縮短。 本節稍後的內容中會說明關於如何從安裝 .WIM 檔案中移除 UWP 應用程式的資訊。

就 VDI 而言，佈建您在基底映像中需要的應用程式，並在其後限制或封鎖對 Microsoft Store 的存取，是不錯的策略。 在一般的電腦上，市集應用程式會定期在背景更新。 UWP 應用程式可在維護期間，於系統套用其他更新時進行更新。 如需詳細資訊，請參閱[通用 Windows 平台應用程式](https://docs.citrix.com/citrix-virtual-apps-desktops/manage-deployment/applications-manage/universal-apps.html)

#### <a name="delete-the-payload-of-uwp-apps"></a>刪除 UWP 應用程式的承載

不需要的 UWP 應用程式仍會在檔案系統中耗用少量的磁碟空間。 針對永遠不會用到的應用程式，您可以使用 PowerShell 命令從基底映像中移除非必要 UWP 應用程式的承載。

事實上，如果您使用本節後續提供的連結從安裝 .WIM 檔案中移除這類承載，您應該能夠以非常精簡的 UWP 應用程式清單從頭開始。

執行下列命令可列舉執行中的作業系統已佈建的 UWP 應用程式，如 PowerShell 所產生的下列節錄範例輸出所示：

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0
    Architecture : neutral
    ResourceId   : \~
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
    ...
```

佈建至系統的 UWP 應用程式可在作業系統安裝期間以工作序列移除，或是在作業系統安裝後移除。 後者可能是較理想的方法，因為它可將建立或維護映像的整體程序模組化。 開發指令碼後，如果某個項目在後續組建中有所變更，您可以編輯現有的指令碼，而非從頭重複相關程序。 以下提供一些連結供您參考本主題的相關資訊：

[在工作序列期間移除 Windows 10 內建應用程式](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)

[使用 Powershell 1.3 版移除 Windows 10 WIM 檔案中的內建應用程式](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607：使應用程式不會在部署功能更新時再次出現](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update)

然後，執行 [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) PowerShell 命令以移除 UWP 應用程式承載：

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

對於每個 UWP 應用程式，您都應評估它在每個獨特環境中的適用性。 您可以安裝 Windows 10 版本 1909 的預設安裝，然後記下哪些應用程式正在執行且耗用記憶體。 例如，您可以考慮移除會自動啟動的應用程式，或是自動在 [開始] 功能表中顯示資訊 (例如氣象和新聞)、但在您的環境中可能不會用到的應用程式。

>[!NOTE]
>如果使用來自 GitHub 的指令碼，您可以在執行指令碼之前，輕鬆地控制要移除哪些應用程式。 下載指令碼檔案之後，找出檔案 ‘Win10_1909_AppxPackages.txt’ 並加以編輯，然後移除您想要保留的應用程式項目，例如計算機、自黏便箋等等。

### <a name="manage-windows-optional-features-using-powershell"></a>使用 PowerShell 管理 Windows 選用功能

您可以使用 PowerShell 管理 Windows 選用功能。 如需詳細資訊，請參閱 [Windows Server PowerShell 論壇](/answers/topics/windows-server-powershell.html)。 若要列舉目前已安裝的 Windows 功能，請執行下列 PowerShell 命令：

```powershell
Get-WindowsOptionalFeature -Online
```

您可以啟用或停用特定 Windows 選用功能，如本範例所示：

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

您可以停用 VDI 映像中的功能，如本範例所示：

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName “WindowsMediaPlayer”
```

接下來，建議您移除 Windows Media Player 套件。 Windows 10 版本 1909 中有兩個 Windows Media Player 套件：

```powershell
Get-WindowsPackage -Online -PackageName *media*

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : DISM Package Manager Provider
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1.mum
InstallTime       : 3/19/2019 6:20:22 AM
...

Features         : {}

PackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449
Applicable        : True
Copyright        : Copyright (c) Microsoft Corporation. All Rights Reserved
Company         :
CreationTime       :
Description       : Play audio and video files on your local device and on the Internet.
InstallClient      : UpdateAgentLCU
InstallPackageName    : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.449.mum
InstallTime       : 10/29/2019 5:15:17 AM
...
```

如果您想移除 Windows Media Player 套件 (以釋放大約 60 MB 的磁碟空間)：

```powershell
 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online

 Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.18362.1 -Online
```

#### <a name="enable-or-disable-windows-features-using-dism"></a>使用 DISM 啟用或停用 Windows 功能

您可以使用內建的 Dism.exe 工具來列舉及控制 Windows 選用功能。 在作業系統安裝工作進行期間，可以開發並執行 Dism.exe 指令碼。 此處所涉及的 Windows 技術稱為[功能隨選安裝](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)。

#### <a name="default-user-settings"></a>預設使用者設定

有一些可對名為 ‘C:\Users\Default\NTUSER.DAT’ 的 Windows 登錄檔案進行的自訂項目。 對此檔案所做的任何設定，都會套用到從執行此映像的裝置建立的任何後續使用者設定檔。 您可以編輯檔案 ‘Win10_1909_DefaultUserSettings.txt’，控制要套用至預設使用者設定檔的設定。 請仔細考慮稱為 **TaskbarSmallIcons** 的設定，此為新加入設定建議的反覆項目。 在實作此設定之前，建議您檢查使用者基底。 **TaskbarSmallIcons** 讓 Windows 工作列變小並耗用較少的螢幕空間，讓圖示更精簡並將搜尋介面最小化，並在前後加以描述，如下圖所示：

圖 1：一般 Windows 10 版本 1909 工作列

![Windows 10 版本 1909 工作列的標準版本](media/rds-vdi-recommendations-1909/standard-taskbar.png)

圖 2：使用小圖示設定的工作列

![使用小圖示設定的工作列](media/rds-vdi-recommendations-1909/taskbar-sm-icons.png)

此外，若要減少透過 VDI 基礎結構傳輸影像的能力，您可以將預設背景設定為純色，而不是預設的 Windows 10 影像。 您也可以將登入畫面設定為純色，並在登入時關閉不透明的模糊效果。

下列設定會套用到預設的使用者設定檔登錄區，主要是為了減少動畫效果。 如果您不想進行部分或全部設定，請根據此影像刪除不會套用至新使用者設定檔的設定。 這些設定的目標是要啟用下列對等設定：

圖 3：最佳化的系統屬性，效能選項

![最佳化的系統屬性，效能選項](media/rds-vdi-recommendations-1909/performance-options.png)

以下是套用至預設使用者設定檔登錄區的最佳化設定，目的是讓效能最佳化：

```
Delete HKLM\Temp\SOFTWARE\Microsoft\Windows\CurrentVersion\Run /v OneDriveSetup /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v WallPaper /t REG_SZ /d "" /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AccentColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v ColorizationColor /t REG_DWORD /d 4292311040 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v EnableAeroPeek /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\DWM /v AlwaysHibernateThumbnails /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v AutoCheckSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v HideIcons /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ListViewShadow /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v ShowInfoTip /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarAnimations /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced /v TaskbarSmallIcons /t REG_DWORD /d 1 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced\People /v PeopleBand /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\AnimateMinMax /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ComboBoxAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\ControlAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMAeroPeekEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\DWMSaveThumbnailEnabled /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\MenuAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\SelectionFade /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TaskbarAnimations /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects\TooltipAnimation /v DefaultApplied /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
```

在本機原則設定中，建議您停用 VDI 中的背景影像。  如果您想保留影像，建議您以較低的色彩深度建立自訂背景影像，以限制用來傳輸影像資訊的網路頻寬。 如果您決定在本機原則中指定 [沒有背景影像]，建議您在設定本機原則之前先設定背景色彩；因為一旦設定原則，使用者就無法變更背景色彩。 將 “(null)” 指定為背景影像可能是較好的方式。 下一節會提到另一項原則設定，說明如何在遠端桌面通訊協定工作階段中使用背景。

### <a name="local-policy-settings"></a>本機原則設定

VDI 環境中的許多 Windows 10 最佳化都可使用 Windows 原則來進行。 本節的表格中所列的設定可套用至基礎/黃金映像。 如果未以任何其他方式 (例如藉由群組原則) 指定等同的設定，就仍會套用這些設定。

某些決策可能取決於環境的詳細資訊，例如：

- VDI 環境是否可存取網際網路？

- VDI 解決方案是持續性還是非持續性的？

下列設定確保不會與任何關係到安全性的設定發生牴觸或衝突。 我們選擇移除這些設定或停用可能不適用於 VDI 環境的功能。

| 原則設定 | 項目 | 子項目 | 可能的設定和註解|
| -------------- | ---- | -------- | ---------------------------- |
| 本機電腦原則 \\ 電腦設定 \\ Windows 設定 \\ 安全性設定 | | | |
| 網路清單管理員原則 | 所有網路內容 | 網路位置 | 使用者不可以變更位置 |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 控制台 | | | |
| *控制台 | 允許線上秘訣 | | 停用。 設定不會聯繫 Microsoft 內容服務以擷取秘訣和說明內容。 |
| \* 控制台\個人化 | 不會顯示鎖定畫面 | 啟用。 此原則設定可控制是否對使用者顯示鎖定畫面。 如果您啟用此原則設定，不需要按 CTRL + ALT + DEL 即可登入的使用者在鎖定電腦後將會看到他們選取的磚。 |
| \* 控制台\個人化 | 強制使用特定的預設鎖定畫面和登入影像 | [![設定鎖定畫面路徑的 UI](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | 啟用。 此設定可讓您指定沒有任何使用者登入時的預設鎖定畫面和登入影像，並將指定的影像設定為所有使用者的預設值 -- 它會取代預設的影像。<p>建議使用解析度較低且並不複雜的影像，如此在每次轉譯影像時透過網路傳輸的資料將會較少。 |
| *控制台\ 地區及語言選項\手寫個人化 | 關閉自動學習 | | 啟用。 如果啟用此原則設定，將會停止自動學習，並且刪除任何已儲存的資料。 使用者無法在控制台中設定此設定。 |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 網路 | | | |
| 背景智慧型傳送服務 (BITS) | 不允許 BITS 用戶端使用 Windows 分支快取 |  | 啟用 |
| 背景智慧型傳送服務 (BITS) | 不允許此電腦做為 BITS 對等快取用戶端 |  | 啟用 |
| 背景智慧型傳送服務 (BITS) | 不允許此電腦做為 BITS 對等快取伺服器 |  | 啟用 |
| 背景智慧型傳送服務 (BITS) | 允許 BITS 對等快取 |  | 停用 |
| BranchCache | 開啟 BranchCache |  | 停用 |
| *字型 | 啟用字型提供者 |  | 停用。 Windows 不會連線至線上字型提供者，而只會列舉本機安裝的字型。 |
| 熱點驗證 | 啟用熱點驗證 |  | 停用 |
| Microsoft 對等網路服務 | 關閉 Microsoft 對等網路服務 |  | 啟用 |
| 網路連線狀態指示器 | 指定被動輪詢。 | 停用被動輪詢 (核取方塊) | 啟用。 若在隔離的網路上請使用此設定，否則請使用靜態 IP 位址。 |
| 離線檔案 | 允許或禁止使用離線檔案。 |  | 停用 |
| TCPIP 設定\\ IPv6 轉換技術 | 設定 Teredo 狀態 | 停用狀態 | 啟用。 在停用狀態中，主機上不會顯示任何 Teredo 介面。 |
| WLAN 服務 \\ WLAN 設定 | 允許 Windows 自動連線至建議的開放式熱點、連絡人共用的網路，及提供付費服務的熱點。 |  | 停用。 **連線至建議的開放式熱點**、**連線至我的連絡人共用的網路**和**啟用付費服務**將會關閉，且此裝置上的使用者將無法啟用這些功能。 |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 開始功能表和工作列 |  |  |  |
| *通知 | 關閉通知網路使用方式 |  | 啟用。 如果啟用此設定，應用程式和系統功能將無法透過網路從 WNS 或使用通知輪詢 API 接收通知。 |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 系統 |  |  |  |
| 裝置安裝 | 當裝置上已安裝標準驅動程式時，不要傳送 Windows 錯誤報告 |  | 啟用 |
| 裝置安裝 | 在通常會提示建立系統還原點的裝置活動期間，防止建立系統還原點。 |  | 啟用 |
| 裝置安裝 | 防止從網際網路擷取裝置中繼資料 |  | 啟用 |
| 裝置安裝 | 當裝置驅動程式在安裝期間要求其他軟體時，防止 Windows 傳送錯誤報告 |  | 啟用 |
| 裝置安裝 | 在裝置安裝期間關閉 [找到新硬體]  註解方塊。 |  | 啟用 |
| 檔案系統 \\ NTFS | 簡短名稱建立選項 | 在所有磁碟區上停用 | 啟用 |
| *群組原則 | 透過應用程式 URL 處理常式設定 Web 對應用程式的連結 |  | 停用。 關閉 Web 至應用程式的連結，並在預設瀏覽器中開啟 http(s) URI，而不是啟動相關聯的應用程式。 |
| *群組原則 | 在此裝置上繼續體驗。 |  | 停用。 Windows 裝置無法讓其他裝置搜尋到，且無法參與跨裝置體驗。 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉對所有 Windows Update 功能的存取 |  |啟用。 如果啟用此原則設定，則會移除所有 Windows Update 功能。 這包括在 https://windowsupdate.microsoft.com 上、從 [開始] 功能表的 Windows Update 超連結，以及在 Internet Explorer 的 [工具] 功能表上封鎖對 Windows Update 網站的存取。 Windows 自動更新也會停用；您不會收到 Windows Update 的相關通知，也不會收到其重大更新。 此原則設定也會防止裝置管理員從 Windows Update 網站自動安裝驅動程式更新。 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉自動根憑證更新 |  |啟用。 如果啟用此原則設定，當您的憑證是由未受信任的根憑證授權單位所簽發時，您的電腦將不會連線至 Windows Update 網站以確認 Microsoft 是否在其信任的授權單位清單中新增了 CA。 注意：如果您除了最新的憑證撤銷清單以外還有替代方法，才可使用此原則。 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉事件檢視器 "Events.asp" 連結 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉個人化手寫資料共用 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉手寫辨識錯誤報告 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉說明及支援中心的「您知道嗎?」 內容 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉說明及支援中心 Microsoft 知識庫搜尋 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 如果 URL 連線正在參照 Microsoft.com 時，關閉網際網路連線精靈 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉網頁發佈和線上訂購精靈的網際網路下載 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉網際網路檔案關聯服務 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 如果 URL 連線正在參照 Microsoft.com 時，關閉註冊 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉「訂購沖印」圖片工作 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉檔案及資料夾的「發佈到網站」工作 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉 Windows Messenger 客戶經驗改進計畫 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉 Windows 客戶經驗改進計畫 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉 Windows 網路連線狀態指示器使用中測試 |  | 啟用。 此原則設定會關閉由 Windows 網路連線狀態指示器 (NCSI) 執行，用以判斷您的電腦是連線至網際網路還是受限網路的使用中測試。在判斷連線層級的過程中，NCSI 會執行兩項使用中測試之一：從專用的 Web 伺服器下載頁面，或發出專用位址的 DNS 要求。 如果啟用此原則設定，NCSI 將不會執行兩項使用中測試。 這可能會降低 NCSI 以及使用 NCSI 判斷網際網路存取的能力) 注意：如果想要使用這項功能，有其他原則可讓您將 NCSI 測試重新導向至內部資源。 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉 Windows 錯誤報告 |  | 啟用 |
| 網際網路通訊管理 \\ 網際網路通訊設定 | 關閉 Windows Update 裝置驅動程式搜尋 |  | 啟用 |
| 登入 | 顯示首次登入動畫 |  | 停用 |
| 登入 | 關閉鎖定畫面上的應用程式通知 |  | 啟用 |
| 登入 | 關閉 Windows 啟動音效 |  | 啟用 |
| 電源管理 | 選取使用中電源計劃 | 高效能 | 啟用 |
| 修復 | 允許將系統還原到預設狀態 |  | 停用 |
| *儲存空間健全狀況 | 允許下載磁碟失敗預測模型的更新 |  | 停用。 不會下載磁碟失敗預測模型的更新。 |
| *Windows 時間服務 \\ 時間提供者 | 啟用 Windows NTP 用戶端 |  | 停用。 如果停用或未設定此原則設定，則本機電腦時鐘的時間不會與 NTP 伺服器同步化。 注意：使用此設定前請謹慎考量。 已加入網域的 Windows 裝置都使用 **NT5DS**。 父系網域 DC 的 DC 可使用 NTP。 PDCe 角色可使用 NTP。 虛擬機器有時會使用「增強功能」或「整合服務」。 |
| 疑難排解與診斷 \\ 排程維護 | 設定排程維護行為 |   | 停用 |
| 疑難排解與診斷 \\ Windows 開機效能診斷 | 設定狀況執行層級 |   | 停用 |
| 疑難排解與診斷 \\ Windows 記憶體流失診斷 | 設定狀況執行層級 |   | 停用 |
| 疑難排解與診斷 \\ Windows 資源耗損偵測與解析 | 設定狀況執行層級 |   | 停用 |
| 疑難排解與診斷 \\ Windows 關機效能診斷 | 設定狀況執行層級 |   | 停用 |
| 疑難排解與診斷 \\ Windows 待命/繼續效能診斷 | 設定狀況執行層級 |   | 停用 |
| 疑難排解與診斷 \\ Windows 系統回應效能診斷 | 設定狀況執行層級 |   | 停用 |
| *使用者設定檔 | 關閉廣告識別碼 |   | 啟用。 如果啟用此原則設定，廣告識別碼將會關閉。 應用程式無法跨應用程式使用識別碼。 |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ Windows 元件 |  |  |  |
| 新增功能到 Windows 10 | 防止精靈執行 |  | 啟用 |
| *應用程式隱私權 | 防止精靈執行 |  | 啟用 |
| *應用程式隱私權 | 讓 Windows App 存取帳戶資訊 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取帳戶資訊，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取通訊記錄 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取通訊記錄，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取連絡人 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取連絡人，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取其他應用程式的診斷資訊 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果停用或未設定此原則設定，您組織中的員工將可使用裝置上的 [設定] > [隱私權] 決定 Windows 應用程式是否可取得關於其他應用程式的診斷資訊。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取電子郵件 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制允許]  選項，Windows 應用程式將可存取電子郵件，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取位置 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取位置，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取訊息中心 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取訊息，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取動作資料 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取動作資料，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取通知 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取通知，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取工作 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取工作，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取行事曆 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取行事曆，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取相機 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取相機，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取麥克風 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取麥克風，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows App 存取信任的裝置 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取受信任裝置，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式與未配對裝置通訊 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法與未配對的無線裝置通訊，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式存取無線通訊 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無權控制無線通訊，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式撥打電話 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法撥打電話，且您組織中的員工無法加以變更。 |
| *應用程式隱私權 | 讓 Windows 應用程式在背景執行 | 所有應用程式的預設值：強制拒絕 | 啟用。 如果選擇 [強制拒絕]  選項，Windows 應用程式將無法在背景執行，且您組織中的員工無法加以變更。 |
| 自動播放原則 | 設定 AutoRun 的預設行為 | 不執行任何 AutoRun 命令 | 啟用 |
| *自動播放原則 | 關閉自動播放 |   | 啟用。 如果啟用此原則設定，光碟機與抽取式媒體磁碟機將會停用自動播放，或所有磁碟機皆停用。 |
| *雲端內容 | 不顯示 Windows 祕訣 | 啟用。 此原則設定可防止對使用者顯示 Windows 秘訣。 |
| *雲端內容 | 關閉 Microsoft 消費者體驗 | 啟用。 如果啟用此原則設定，使用者將不會再看到 Microsoft 提供的個人化建議，和其 Microsoft 帳戶的相關通知。 |
| *資料收集與預覽組建 | 允許遙測 | 0 - 安全性 [僅限企業版] | 啟用。 設定值為 0 時，只會套用至執行企業版、教育版、IoT 版或 Windows Server 版的裝置。 |
| *資料收集與預覽組建 | 不顯示意見反應通知 |  | 啟用 |
| *資料收集與預覽組建 | 切換測試人員組建的使用者控制項  |  | 停用 |
| 傳遞最佳化 | 下載模式 | 下載模式：簡單 (99) | 99 = 沒有對等傳輸的簡單下載模式。 傳遞最佳化只會使用 HTTP 進行下載，且不會嘗試連線至傳遞最佳化雲端服務。 |
| 桌面視窗管理員 |  不允許呼叫 Flip3D |  | 啟用 |
| 桌面視窗管理員 |  不允許視窗動畫 |  | 啟用 |
| 桌面視窗管理員 |  使用純色做為開始背景 |  | 啟用 |
| 邊緣 UI |  允許邊緣撥動 |  | 停用 |
| 邊緣 UI |  停用說明秘訣 |  | 啟用 |
| 邊緣 UI | 關閉追蹤應用程式使用量 |  | 啟用 |
| *檔案總管 |  設定 Windows Defender SmartScreen |  | 停用。 將為所有使用者停用 SmartScreen。 使用者在嘗試從網際網路執行可疑的應用程式時，將不會收到警告。 注意：如果未連線至網際網路，將使電腦無法嘗試聯繫 Microsoft 以取得 SmartScreen 資訊。 |
| 檔案總管 |  不顯示**已安裝新應用程式**通知 |  | 啟用 |
| *尋找我的裝置 |  開啟/關閉尋找我的裝置 |  | 停用。 當 [尋找我的裝置] 關閉時，將不會註冊裝置及其位置，且 [尋找我的裝置] 功能將無法運作。 使用者也無法在其裝置上檢視其作用中的數位板最後一次使用的位置。 |
| 檔案總管 | 關閉縮圖快取處理 |  | 啟用 |
| 檔案總管 | 關閉在 [檔案總管] 搜尋方塊中顯示最近搜尋項目 |  | 啟用 |
| 檔案總管 | 關閉在隱藏的 thumbs.db 檔案中快取縮圖 |  | 啟用 |
| 遊戲總管 | 關閉遊戲資訊下載 |  | 啟用 |
| 遊戲總管 | 關閉遊戲更新 |  | 啟用 |
| 遊戲總管 | 關閉 [遊戲] 資料夾中追蹤最後一次執行遊戲時間的功能 |  | 啟用 |
| 家用群組 | 防止電腦加入家用群組 |  | 啟用 |
| *Internet Explorer | 當使用者在網址列輸入時，允許 Microsoft 服務提供加強式建議 |  | 停用。 當使用者在網址列輸入時將不會收到加強式建議。 此外，使用者將無法變更 [建議] 設定。 |
| Internet Explorer | 停用定期檢查以進行 Internet Explorer 軟體更新 |  | 啟用 |
| Internet Explorer | 停用顯示啟動顯示畫面的功能 |  | 啟用 |
| Internet Explorer | 自動安裝新版 Internet Explorer |  | 停用 |
| Internet Explorer | 防止參與客戶經驗改進計畫 |  | 啟用 |
| Internet Explorer | 防止執行 [首次執行] 精靈 | 直接前往首頁 | 啟用 |
| Internet Explorer | 設定索引標籤程序成長速率 | 低 | 啟用 |
| Internet Explorer | 指定新索引標籤的預設行為 | 新索引標籤頁面 | 啟用 |
| Internet Explorer | 關閉附加元件效能通知 |  | 啟用 |
| *Internet Explorer | 關閉網址的自動完成功能 |  | 啟用。 如果啟用此原則設定，使用者在輸入網址時將不會收到相符項目建議。 使用者無法變更設定網址的自動完成功能。 |
| *Internet Explorer | 關閉瀏覽器地理位置 |  | 啟用。 如果啟用此原則設定，將會關閉瀏覽器地理位置支援。 |
| *Internet Explorer | 關閉 [重新開啟上一次的瀏覽工作階段] |  | 啟用 |
| Internet Explorer | 關閉 [重新開啟上一次的瀏覽工作階段] |  | 啟用 |
| *Internet Explorer | 開啟建議的網站 |  | 停用。 如果停用此原則設定，就會關閉與此功能相關聯的進入點和功能。 |
| *Internet Explorer \\ 相容性檢視 | 關閉相容性檢視 |  | 啟用。 如果啟用此原則設定，使用者將無法使用 [相容性檢視] 按鈕或管理 [相容性檢視] 網站清單。 |
| *Internet Explorer \\ 網際網路控制台 \\ 進階頁面 | 播放網頁動畫 |  | 停用 |
| *Internet Explorer \\ 網際網路控制台 \\ 進階頁面 | 播放網頁影片 |  | 停用 |
| *Internet Explorer \\ 網際網路控制台 \\ 進階頁面 | 關閉快速翻頁和頁面預測功能 |  | 啟用。 Microsoft 會收集您的瀏覽歷程記錄來改善快速翻頁與頁面預測的運作方式。 此功能不適用於 Internet Explorer (傳統型)。 如果啟用這個原則設定，則會關閉快速翻頁與頁面預測，而且背景中不會載入下一個網頁。 |
| Internet Explorer \\ 網際網路設定 \\ 進階設定 \\ 瀏覽 | 關閉電話號碼偵測 |  | 啟用 |
| *定位和感應器 | 關閉定位 |  | 啟用。 如果啟用此原則設定，將會關閉定位功能，且這部電腦上的所有程式都無法使用定位功能的位置資訊。 |
| 定位和感應器 | 關閉感應器 |  | 啟用 |
| 定位和感應器 \\ Windows 定位提供者 | 關閉 Windows 定位提供者 |  | 啟用 |
| *地圖 | 關閉自動下載與更新地圖資料 |  | 啟用。 如果啟用此設定，將會關閉地圖資料的自動下載和更新。 |
| *地圖 | 關閉 [離線地圖] 設定頁面上非要求的網路流量 |  | 啟用。 如果啟用此原則設定，將會關閉 [離線地圖] 設定頁面上產生網路流量的功能。 注意：這可能會關閉整個設定頁面。 |
| *訊息中心 | 允許訊息服務雲端同步 |  | 停用。 此原則設定允許將行動簡訊備份及還原到 Microsoft 雲端服務。 |
| *Microsoft Edge | 允許網址列下拉式清單建議 |  | 停用 |
| *Microsoft Edge | 允許書籍媒體櫃的設定更新 |  | 停用。 關閉 Microsoft Edge 中的相容性清單。 |
| *Microsoft Edge | 允許 Microsoft 相容性清單 |  | 停用。 如果您停用此設定，則在瀏覽瀏覽器期間不會使用「Microsoft 相容性清單」。 |
| *Microsoft Edge | 允許在新索引標籤上顯示網頁內容 |  | 停用。 指示 Edge 在開啟新的索引標籤時以空白內容開啟。 |
| *Microsoft Edge | 設定自動填入 |  | 停用。 停用網址列的自動填寫。 |
| *Microsoft Edge | 設定不要追蹤 |  | 啟用。 如果您啟用此設定，一律會傳送「不要追蹤」要求到要求追蹤資訊的網站。 |
| *Microsoft Edge | 設定密碼管理員 |  | 停用。 如果您停用這項設定，員工則無法使用密碼管理員將密碼儲存在本機。 |
| *Microsoft Edge | 設定網址列中的搜尋建議 |  | 停用。 使用者無法在 Microsoft Edge 的網址列中看到搜尋建議。 |
| *Microsoft Edge | 設定起始畫面 |  | 啟用。 如果啟用這項設定，您可以設定一或多個 [開始] 頁面。 如果啟用此設定，您也必須包含網頁的 URL，並使用此格式與角括弧分隔多個頁面︰<support.contoso.com><support.microsoft.com> Windows 10 版本 1703 或更新版本：如果您不想要將流量傳送至 Microsoft，您可以使用 <about:blank> 值；無論裝置是否已加入網域，如果只有這個已設定的 URL，就會接受此值。 |
| *Microsoft Edge | 設定 Windows Defender SmartScreen |  | 停用。 會關閉 Windows Defender SmartScreen，且員工無法加以開啟。 注意：請考慮在環境中使用此設定。 如果未連線至網際網路，將使電腦無法嘗試聯繫 Microsoft 以取得 SmartScreen 資訊。 |
| *Microsoft Edge | 防止 Microsoft Edge 開啟 [首次執行] 網頁 |  | 啟用。 使用者在初次開啟 Microsoft Edge 時不會看見 [首次執行] 頁面。 |
| OneDrive | 防止 OneDrive 產生網路流量直到使用者登入 OneDrive |  | 啟用。 啟用此設定可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。 |
| *OneDrive | 防止使用 OneDrive 儲存檔案 |  | 啟用。 除非 OneDrive 是在內部部署或外部部署使用。 |
| OneDrive | 預設將文件儲存到 OneDrive |  | 停用。 除非 OneDrive 是在內部部署或外部部署使用。 |
| RSS 摘要 | 防止自動探索摘要和網頁快訊 |  | 啟用 |
|*RSS 摘要 | 關閉摘要和網頁快訊的背景同步處理 |  | 啟用。 如果啟用此原則設定，將會停用在背景中同步處理摘要和網頁快訊的功能。 |
|*搜尋 | 允許 Cortana |  | 停用。 當 Cortana 已關閉時，使用者仍可使用搜尋功能尋找裝置上的項目。 |
|搜尋 | 允許在鎖定畫面上使用 Cortana |   | 停用 |
|*搜尋 | 允許搜尋和 Cortana 使用定位 |  | 停用 |
|搜尋 | 不允許 Web 搜尋 |   | 啟用 |
|*搜尋 | 不要在 [搜尋] 中搜尋網路或顯示網路搜尋結果 |  | 啟用。 如果啟用此原則設定，則不會在 Web 上執行查詢，且在使用者使用 [搜尋] 執行查詢時將不會顯示 Web 結果。 |
|搜尋 | 防止從控制台新增 UNC 位置到索引 |  | 啟用 |
|搜尋 | 防止對離線檔案快取中的檔案編製索引 |  | 啟用 |
|*搜尋 | 設定 [搜尋匿名] 資訊中可以分享的資訊 |  | 啟用。 共用使用資訊，但不共用搜尋歷程記錄、Microsoft 帳戶資訊或特定位置。 |
|*軟體保護平台 | 關閉 KMS 用戶端線上 AVC 驗證 | 啟用。 啟用此設定可防止此電腦傳送與其啟用狀態有關的資料給 Microsoft。 |
|*語音 | 允許自動更新語音資料 |  | 停用。 不會定期檢查是否有更新的語音模型。 |
|*Store | 關閉自動下載與安裝更新 |  | 啟用。 如果啟用此設定，將會關閉應用程式更新的自動下載和安裝。 |
|*Store | 在 Win8 裝置上關閉更新的自動下載 | 啟用。 如果啟用此設定，將會關閉應用程式更新的自動下載。 |
| 市集 | 關閉更新至最新版 Windows 的服務 |  | 啟用 |
|*同步您的設定 | 不要同步 | 允許使用者開啟同步 (未選取) | 啟用。 如果啟用此設定，將會關閉 [同步您的設定]，且在裝置上不會同步任何 [同步您的設定] 群組。 |
| 文字輸入 | 改進筆跡及輸入辨識 |  | 停用 |
| Windows Defender 防毒軟體 \\ MAPS | 加入 Microsoft MAPS |  | 停用。 如果停用或未設定此設定，您將不會加入 Microsoft MAPS。 |
| Windows Defender 防毒軟體 \\ MAPS | 需要進一步分析時發送檔案樣本 | 一律不傳送 | 啟用。 只有在未選擇加入 MAPS 診斷資料時。 |
| Windows Defender 防毒軟體 \\ 報告 | 關閉增強通知 |  | 啟用。 如果啟用此設定，將不會在用戶端上顯示 Windows Defender 防毒軟體增強通知。 |
| Windows Defender 防毒軟體 \\ 特徵碼更新 | 定義下載定義更新的來源順序 | FileShares | 啟用。 如果啟用此設定，將會以指定的順序聯繫定義更新來源。 從一個指定的來源成功下載定義更新後，即不會聯繫清單中的其餘來源。 |
| Windows 錯誤報告 | 自動傳送作業系統產生的錯誤報告的記憶體傾印 |  | 停用 |
| Windows 錯誤報告 | 停用 Windows 錯誤報告 |  | 啟用 |
| Windows 遊戲錄影和廣播 | 啟用或停用 Windows 遊戲錄影和廣播 | | 停用 |
| Windows Installer | 控制基準檔案快取的大小上限 | 5 | 啟用 |
| Windows Installer | 關閉系統還原檢查點建立功能 |  | 啟用 |
| Windows Mail | 關閉社群功能 |  | 啟用 |
| Windows Media Player | 不要顯示 [安裝第一次使用] 對話方塊 |  | 啟用 |
| Windows Media Player | 防止媒體共用 |  | 啟用 |
| Windows 行動中心 | 關閉 Windows 行動中心 |  | 啟用 |
| Windows 可靠性分析 | 設定可靠性 WMI 提供者 |  | 停用 |
| Windows Update | 允許立即安裝自動更新 |  | 啟用 |
| Windows Update | 不連接到任何 Windows Update 網際網路位置 |  | 啟用。 啟用此原則將會停用該功能，且可能會導致對公用服務 (例如 Windows 市集) 的連線停止運作。 注意：這項原則只在此裝置設定為使用 [指定近端內部網路 Microsoft 更新服務的位置] 原則連線到近端內部網路更新服務時套用。 |
| Windows Update | 移除對所有 Windows Update 功能的存取 |   | 啟用 |
| *Windows Update \\ 商務用 Windows Update | 管理預覽版 | 設定接收預覽版的行為： | 啟用。 選取 [停用預覽版] 會防止在裝置上安裝預覽版。 這將導致使用者無法透過 [設定] -> [更新與安全性] 選擇加入 Windows 測試人員計畫。<br>停用。 停用預覽組建。 |
| *Windows Update \\ 商務用 Windows Update | 選取接收預覽版和功能更新的時機 | 半年通道<br>延期期間：365 天<br>暫停開始日期：yyyy-mm-dd。 | 啟用。 啟用此原則可指定要接收的預覽版或功能更新的層級和接收時機。 |
| Windows Update \\ 商務用 Windows Update | 選取接收品質更新的時機 | 1. 30 天<br>2.從 yyyy-mm-dd 開始暫停品質更新 | 啟用 |
| Windows 受限流量自訂原則設定 | 防止 OneDrive 產生網路流量直到使用者登入 OneDrive |  | 啟用。 視需要啟用此設定，可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。 |
| Windows 受限流量自訂原則設定 | 關閉 Windows Defender 通知 |  | 啟用。 如果啟用此原則設定，Windows Defender 將不會傳送含有裝置健康情況和安全性的相關重要資訊的通知。 |
| 本機電腦原則 \\ 使用者設定 \\ 系統管理範本  |  |  |
|控制台 \\ 地區及語言選項 | 關閉在我輸入時提供文字預測 |  | 啟用 |
| [桌上型電腦] | 不要將最近開啟的文件共用新增到 [網路位置] 中 |  | 啟用 |
| [桌上型電腦] | 關閉 Aero 搖動視窗最小化滑鼠手勢 |  | 啟用 |
| 桌面 \\ Active Directory | Active Directory 搜尋的大小上限 | 2500 | 啟用 |
| 開始功能表和工作列 | 不允許釘選市集應用程式到工作列 |  | 啟用 |
| 開始功能表和工作列 | 不顯示或追蹤捷徑清單中來自遠端位置的項目 |  | 啟用 |
| 開始功能表和工作列 | 不要用以搜尋為主的方法來解析殼層捷徑 |  | 啟用。 系統不會執行最後的磁碟機搜尋。 它只會顯示訊息，說明找不到檔案。 |
| 開始功能表和工作列 | 從工作列中移除人員列 |  | 啟用。 將會從工作列中移除人員圖示、從工作列設定頁面中移除對應的設定切換，且使用者無法將人員釘選到工作列。 |
| 開始功能表和工作列 | 關閉功能通告提示氣球通知 |  | 啟用。 使用者無法將市集應用程式釘選到工作列。 如果市集應用程式已釘選到工作列，將會在下次登入時從工作列中移除。 |
| 開始功能表和工作列 | 關閉使用者追蹤 |  | 啟用 |
| 開始功能表和工作列 \\ 通知 | 關閉快顯通知 |  | 啟用 |
| Windows 元件 \\ 雲端內容 | 關閉所有 Windows 焦點功能 |  | 啟用 |

### <a name="notes-about-network-connectivity-status-indicator"></a>網路連線狀態指示器的相關注意事項

上述的群組原則設定，包含將系統是否連線至網際網路的檢查關閉的設定。 如果您的環境完全不會連線至網際網路，或是會以間接方式連線，您可以設定群組原則設定，以從工作列中移除 [網路] 圖示。 如果您關閉網際網路連線檢查後，即使網路可能正常運作，[網路] 圖示上仍會有黃色旗標，您就可能會想要從工作列中移除 [網路] 圖示。 如果您想要透過群組原則設定移除網路圖示，您可以在下列位置找到該設定：

| 原則設定 | 項目 | 子項目 | 可能的設定和註解|
| -------------- | ---- | -------- | ---------------------------- |
| Windows Update 或商務用 Windows Update | 選取接收品質更新的時機 | 1. 30 天<br>2.從 yyyy-mm-dd 開始暫停品質更新 | 啟用 |
| 本機電腦原則 \\ 使用者設定 \\ 系統管理範本 |  |  |  |
| 開始功能表和工作列 | 移除網路圖示 |  | 啟用。 網路圖示不會顯示在系統通知區域中。 |

如需網路連線狀態指示器 (NCSI) 的詳細資訊，請參閱[管理適用於 Windows 10 企業版 1903 的連線端點](/windows/privacy/manage-windows-1903-endpoints)和[管理從 Windows 10 作業系統元件到 Microsoft 服務的連線](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services)。

### <a name="system-services"></a>系統服務

如果您考慮停用系統服務以節省資源，請務必仔細確認該服務在某方面是否為其他服務的元件之一。 請注意，某些服務不在清單中，因為這些服務無法以支援的方式停用。

這些建議大多與[在包含桌面體驗的 Windows Server 2016 中停用的系統服務指引](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md)中的 Windows Server 2016 桌面體驗方面的建議相對應

許多可能適合停用的服務都設定為手動服務啟動類型。 這表示服務不會自動啟動，且除非有程序或服務對您考慮要停用的服務觸發了要求，才會啟動。 已設定為手動啟動類型的服務通常不會列於此處。

> [!NOTE]
> 您可以使用此 PowerShell 範例程式碼來列舉執行中的服務，只輸出服務的簡短名稱：

```powershell
 Get-Service | Where-Object {$_.Status -eq "Running"} | select -ExpandProperty Name
 ```

| Windows 服務 | 項目 | 評價|
| -------------- | ---- | ---------------------------- |
| CDPUserService | 此使用者服務適用於連線的裝置平台案例 | 這是每一使用者服務，因此*範本服務*必須停用。 |
| 已連線使用者體驗與遙測 | 啟用支援應用程式內與已連線使用者體驗的功能。 此外，在 [意見反應與診斷] 下啟用診斷與使用方式隱私權選項設定時，此服務會管理診斷與使用方式資訊 (用於改進 Windows 平台的體驗與品質) 的事件導向收集與傳輸。 | 如果在中斷連線的網路上，請考慮停用。 |
| 連絡人資料 | 為連絡人資料編製索引，以加快搜尋連絡人的速度。 如果您停止或停用此服務，則搜尋結果中可能會遺漏連絡人資訊。 | 這是每一使用者服務，因此*範本服務*必須停用。 |
| 診斷原則服務 | 啟用對 Windows 元件的問題偵測、疑難排解和解決方案。 如果停止此服務，診斷將不再運作。 | |
| 已下載的地圖管理員 | 適用於應用程式存取已下載地圖的 Windows 服務。 要存取已下載地圖的應用程式會依需求啟動此服務。 停用此服務將讓應用程式無法存取地圖。 | |
| 地理位置服務 | 監視目前的系統位置並管理地理柵欄 | |
| GameDVR 和廣播使用者服務 | 這項使用者服務用於遊戲錄影和即時廣播 | 這是個別使用者的服務，因此範本服務必須停用。 |
| MessagingService | 支援文字訊息和相關功能的服務。 | 這是每一使用者服務，因此*範本服務*必須停用。 |
| 最佳化磁碟機 | 將儲存磁碟機上的檔案最佳化，有助於提高電腦執行效率。 | VDI 解決方案通常不會受益於磁碟最佳化。 這些「磁碟機」並不是傳統的磁碟機，且通常只是暫時的儲存體配置。 |
| Superfetch | 維護並改進一段時間後的系統效能。 | 在 VDI 中通常無法改善效能，尤其是非持續性 VDI，因為在每次重新開機後就會捨棄作業系統狀態。 |
| 觸控式鍵盤與手寫面板服務 | 啟用觸控式鍵盤和手寫面板及筆跡功能 | |
| Windows 錯誤報告 | 允許在程式停止運作或回應時報告錯誤，並允許傳遞現有的解決方案。 此外，也允許產生用於診斷與修復服務的記錄。 如果停止此服務，錯誤報告可能就無法正常運作，而且可能無法顯示診斷服務及修復的結果。 | 使用 VDI 時，診斷常會在離線狀態下執行，而不是在主要生產環境中執行。 此外，有些客戶會停用 WER。 WER 會針對許多不同的狀況產生少量資源，包括無法安裝裝置或無法安裝更新等狀況。 |
| Windows Media Player 網路共用服務 | 使用通用隨插即用，與網路上其他的播放器和媒體裝置共用 Windows Media Player 媒體櫃 | 除非客戶在網路上共用 WMP 媒體櫃，否則不需要。 |
| Windows 行動熱點服務 | 提供與其他裝置共用行動數據連線的能力。 | |
| Windows 搜尋 | 提供檔案、電子郵件和其他內容的內容索引、屬性快取及搜尋結果。                                                                    | 可能不需要，尤其是使用非持續性 VDI 時 |

#### <a name="per-user-services-in-windows"></a>Windows 中的每一使用者服務

每一使用者服務是會在使用者登入 Windows 或 Windows Server 時建立，並且在該使用者登出時停止並刪除的服務。這些服務執行於使用者帳戶的安全性內容中 - 其資源管理效能優於先前的方法所能提供的，因為過去須在 [總管] 中使用預先設定的帳戶或以工作的形式執行這類服務。

[Windows 10 和 Windows Server 中的每一使用者服務](/windows/application-management/per-user-services-in-windows)

如果您想要變更服務起始值，偏好的方法是開啟已提升權限的 cmd.exe 命令提示字元，並執行服務控制管理員工具 ‘Sc.exe’。 如需使用 ‘Sc.exe’ 的詳細資訊，請參閱 [Sc](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc754599(v=ws.11))

### <a name="scheduled-tasks"></a>排定的工作

如同 Windows 中其他項目，在考慮停用某個項目前，請先確定您不需要該項目。

以下列出會在電腦上執行最佳化或資料收集，使電腦在重新開機後保有其狀態的工作。 當 VM VDI 工作重新開機並捨棄前次開機後的所有變更時，用於實體電腦的最佳化將沒有幫助。

您可以使用下列 PowerShell 程式碼，取得目前所有排定的工作 (包括描述)：

```powershell
 Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

>[!NOTE]
> 有數個工作無法透過指令碼停用，即使您已提高權限亦然。 建議您不要停用無法使用指令碼停用的工作。

排定的工作名稱：

- 行動電話通訊
- 資訊彙集器
- 診斷
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- MaintenanceTasks
- MapsToastTask
- 相容性
- Microsoft-Windows-DiskDiagnosticDataCollector
- MNO
- NotificationTask
- PerformRemediation
- ProactiveScan
- ProcessMemoryDiagnosticEvents
- ProgramDataUpdater
- Proxy
- QueueReporting
- RecommendedTroubleshootingScanner
- ReconcileFeatures
- ReconcileLanguageResources
- RefreshCache
- RegIdleBackup
- ResPriStaticDbSync
- RunFullMemoryDiagnostic
- ScanForUpdates
- ScanForUpdatesAsUser
- 排程
- ScheduledDefrag
- sihpostreboot
- SilentCleanup
- SmartRetry
- SpaceAgentTask
- SpaceManagerTask
- SpeechModelDownloadTask
- Sqm-Tasks
- SR
- StartComponentCleanup
- StartupAppTask
- StorageSense
- SyspartRepair
- Sysprep
- UninstallDeviceTask
- UpdateLibrary
- UpdateModelTask
- UsbCeip
- Usb-Notifications
- USO_UxBroker
- WiFi
- WIM-Hash-Management
- WindowsActionDialog
- WinSAT
- 資料夾
- WsSwapAssessmentTask
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>套用 Windows (和其他) 更新

無論是從 Microsoft Update，還是從您的內部資源，都可套用可用的更新，包括 Windows Defender 特徵碼。 這是套用其他可用更新的好時機，包括 Microsoft Office 的更新 (如有安裝)，以及其他軟體更新。 如果 PowerShell 會保留在影像中，您可以執行命令 [Update-Help](/powershell/module/microsoft.powershell.core/update-help?view=powershell-7) 下載 PowerShell 的最新可用說明。

#### <a name="servicing-the-operating-system-and-apps"></a>維護作業系統和應用程式

在映像最佳化程序中的某個時間點，應該會套用可用的 Windows 更新。 Windows 10 更新設定中有一個可提供其他更新的設定：

![其他更新](media/rds-vdi-recommendations-1909/servicing.png)

如果您要將 Microsoft Office 之類的 Microsoft 應用程式安裝到基底映像，這會是較理想的設定。 如此，當映像開始運作時，Office 就會處於最新狀態。 此外還有 .NET 更新，以及某些可透過 Windows Update 取得更新的第三方元件 (例如 Adobe)。

安全性更新是非持續性 VDI VM 非常重要的考量之一，其中包括安全性軟體定義檔。 這些更新可能會以一天一次甚或更長的間隔發行。 您可以透過否些方法來保留這些更新，包括 Windows Defender 和第三方元件。

就 Windows Defender 而言，建議即使在非持續性 VDI 中，也應允許更新執行。 更新幾乎在每個登入工作階段都會套用，但這類更新都很小，應該不會有問題。 此外，由於只會套用最新的可用更新，VM 的更新將不會過時。 第三方定義檔可能也是如此。

> [!NOTE]
> 市集應用程式 (UWP 應用程式) 會透過 Windows 市集進行更新。 新版的 Office (例如 Microsoft 365) 在直接連線至網際網路時會透過其本身的機制進行更新，而在未連線至網際網路時，則會透過管理技術進行更新。

### <a name="windows-system-startup-event-traces"></a>Windows 系統啟動事件追蹤

Windows 依預設設定會收集並儲存有限的診斷資料。 其目的是要啟用診斷，或是記錄資料以便在需要進一步疑難排解時使用。 您可以在下圖所示的位置找到自動系統追蹤：

![系統追蹤](media/rds-vdi-recommendations-1909/system-traces.png)

顯示於 [事件追蹤工作階段]  和 [啟動事件追蹤工作階段]  下方的部分追蹤無法停止，而且也不應停止。 其他追蹤 (例如 ‘WiFiSession’) 則可以停止。 若要停止執行中的追蹤，請在 [事件追蹤工作階段]  下方，以滑鼠右鍵按一下追蹤，然後按一下 [停止]。 若要防止在系統啟動時自動啟動追蹤，請遵循下列步驟：

1. 按一下 [啟動事件追蹤工作階段]  資料夾。

2. 找出您要處理的追蹤，然後按兩下該追蹤。

3. 按一下 [追蹤工作階段]  索引標籤。

4. 按一下標示為 [啟用]  的方塊，以移除核取記號。

5. 按一下 [確定]  。

以下是使用 VDI 時可考慮停用的一些系統追蹤：

| 名稱                    | 註解                       |
| ----------------------- | ----------------------------- |
| AppModel | 一個追蹤集合，電話是其中之一 |
| CloudExperienceHostOOBE | |
| DiagLog | |
| NtfsLog | |
| TileStore | |
| UBPM | |
| WiFiDriverIHVSession | 如果未使用 WiFi 裝置 |
| WiFiSession | |
| WinPhoneCritical | |

### <a name="windows-defender-optimization-with-vdi"></a>Windows Defender 對 VDI 的最佳化

Microsoft 近期發佈了關於在 VDI 環境中使用 Windows Defender 的文件。 請參閱[在虛擬桌面基礎結構 (VDI) 環境中部署 Windows Defender 防毒軟體的指南](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)以取得詳細資訊。

上述文章提供維護黃金 VDI 映像的程序，並說明如何維護執行中的 VDI 用戶端。 若要在 VDI 電腦需要更新其 Windows Defender 特徵碼時降低網路頻寬，請錯開重新開機的時間，並盡可能將重新開機排程在離峰時間。 Windows Defender 特徵碼的更新可包含在內部檔案共用上，且如果可行，請將這些檔案共用放在與 VDI 虛擬機器相同或接近的網路區段上。

### <a name="client-network-performance-tuning-by-registry-settings"></a>依登錄設定進行的用戶端網路效能微調

有些登錄設定可能會提高網路效能。 在 VDI 或電腦的工作負載多半以網路為基礎的環境中，這項調整格外重要。 建議您套用此節中的設定，為目錄項目之類的項目設定額外的緩衝處理和快取，使效能側重於網路的層面。

>[!NOTE]
> 本節中的某些設定僅以登錄為基礎，且應在映像部署至生產環境之前納入基底映像中。

下列設定值記載於 [Windows Server 2016 效能微調指導方針](../../administration/performance-tuning/index.md)中，由 Windows 產品小組發佈於 Microsoft.com 上。

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

適用於 Windows 10。 預設值是 **0**。 根據預設，在某些情況下，SMB 重新導向器會對高延遲網路連線的輸送量進行節流，以避免網路相關逾時。 若將此登錄值設為 1 則會停用此節流，進而透過高延遲網路連接來達到更高的檔案轉送輸送量。 請考慮將此值設定為 **1**。

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax` 適用於 Windows 10。 預設值是 **64**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案中繼資料數量。 提高此值可降低網路流量，並且在存取許多檔案時提升效能。 請嘗試將此值增加到 **1024**。

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

適用於 Windows 10。 預設值是 **16**，有效範圍是 1 至 4096。 此值用來決定用戶端可以快取的目錄資訊數量。 提高此值可降低網路流量，並且在存取大量目錄時提升效能。 請考慮將此值增加到 **1024**。

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

適用於 Windows 10。 預設值是 **128**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案名稱資訊數量。 提高此值可降低網路流量，並且在存取許多檔案名稱時提升效能。 請考慮將此值增加到 **2048**。

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

適用於 Windows 10。 預設值是 **1023**。 此參數可指定在應用程式關閉檔案之後，共用資源上應該保持開啟的檔案數目上限。 有數千個用戶端要連線至 SMB 伺服器時，請考慮將此值縮減至 **256**。

您可以使用 [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps) 和 [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps) Windows PowerShell Cmdlet 來設定其中許多 SMB 設定。 您也可以使用 Windows PowerShell 來設定僅限登錄的設定，如下列範例所示：

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

Windows 受限流量有限功能基準指導方針中的其他設定 Microsoft 已發行使用與 [Windows 安全性基準](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps)相同的程序建立的基準，適用於未直接連線至網際網路的環境，或不想將太多資料傳送至 Microsoft 和其他服務的環境。

[Windows 受限流量有限功能基準](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services)設定在群組原則表格中會以星號標示。

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>磁碟清理 (包括使用磁碟清理精靈)

磁碟清理對於黃金/主要映像 VDI 實作可能特別有幫助。 在映像備妥、更新並完成設定後，執行磁碟清理是最後應執行的工作之一。 名為「磁碟清理精靈」的內建工具有助於清理出最有可能節省磁碟空間的區域。 在安裝較少，但已完全修補的 VM 上，執行磁碟清理通常會釋放出 4 GB 的磁碟空間。

以下是對針對各種磁碟清理工作提供的建議。 在實作之前，應該先測試這些項目：

1. 在套用所有更新後執行磁碟清理精靈 (提升權限)。 請納入「傳遞最佳化」和「Windows Update 清理」類別。 您可以使用命令列 `Cleanmgr.exe` 搭配 `/SAGESET:11` 選項以自動進行此程序。 `/SAGESET` 選項會設定後續可用來自動執行磁碟清理的登錄值；屆時會使用磁碟清理精靈中的每個可用選項。

    1. 在測試 VM 上從全新安裝執行 `Cleanmgr.exe /SAGESET:11` 時，會顯示只有兩個依預設啟用的自動磁碟清理選項：

        - 下載的程式檔案

        - Temporary Internet Files

    2. 如果您設定更多選項或所有選項，這些選項會根據前一個命令 (`Cleanmgr.exe /SAGESET:11`) 中提供的**索引**值記錄在登錄中。 在此範例中，我們使用值 `11` 作為索引，用於後續的自動磁碟清理程序。

    3. 執行 `Cleanmgr.exe /SAGESET:11` 之後，您會看到數個磁碟清理選項的類別。 您可以檢查每個選項，然後按一下 [確定]  。 磁碟清理精靈會消失，且您的設定會儲存在登錄中。

2. 如有任何使用中的磁碟區陰影複製儲存體，請加以清理。

    - 開啟提升權限的命令提示字元，然後執行 `vssadmin list shadows` 命令以及 `vssadmin list shadowstorage` 命令。

        如果這些命令的輸出是**找不到符合查詢的項目**，表示沒有任何使用中的 VSS 儲存體。

3. 清理暫存檔案和記錄。 開啟提升權限的命令提示字元，然後執行 `Del C:\*.tmp /s` 命令、`Del C:\Windows\Temp\.` 命令以及 `Del %temp%\.` 命令。

4. 使用執行 `wmic path win32_UserProfile where LocalPath="c:\users\<user>" Delete`，刪除系統上所有未使用的設定檔。

### <a name="remove-onedrive-components"></a>移除 OneDrive 元件

要移除 OneDrive，必須要先移除套件、完成解除安裝，然後將 *.lnk 檔案移除。 下列 PowerShell 程式碼範例可用來協助您從映像移除 OneDrive，且此程式碼範例包含在 GitHub VDI 最佳化指令碼中：

```azurecli

Taskkill.exe /F /IM "OneDrive.exe"
Taskkill.exe /F /IM "Explorer.exe"`
    if (Test-Path "C:\\Windows\\System32\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\System32\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
    if (Test-Path "C:\\Windows\\SysWOW64\\OneDriveSetup.exe")`
     { Start-Process "C:\\Windows\\SysWOW64\\OneDriveSetup.exe"`
         -ArgumentList "/uninstall"`
         -Wait }
Remove-Item -Path
"C:\\Windows\\ServiceProfiles\\LocalService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force
Remove-Item -Path "C:\\Windows\\ServiceProfiles\\NetworkService\\AppData\\Roaming\\Microsoft\\Windows\\Start Menu\\Programs\\OneDrive.lnk" -Force \# Remove the automatic start item for OneDrive from the default user profile registry hive
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Load HKLM\\Temp C:\\Users\\Default\\NTUSER.DAT" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Delete HKLM\\Temp\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Run /v OneDriveSetup /f" -Wait
Start-Process C:\\Windows\\System32\\Reg.exe -ArgumentList "Unload HKLM\\Temp" -Wait Start-Process -FilePath C:\\Windows\\Explorer.exe -Wait
```

對於本文件中的資訊如有任何問題或疑慮，請連絡 Microsoft 帳戶小組、參閱 Microsoft VDI 部落格、將訊息張貼至 Microsoft 論壇，或連絡 Microsoft 以解決問題或疑慮。

## <a name="turn-windows-update-back-on"></a>重新開啟 Windows Update

如果想在持續 VDI 中重新開啟 Windows Update (如本範例)，請遵循下列步驟：

- 重新啟用這些群組原則設定：

    - 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 系統 \\ 網際網路通訊管理 \\ 網際網路通訊設定

        - 關閉所有 Windows Update 功能的存取 (從 **啟用**變更為**未設定**)。

    - 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ Windows 元件 \\ Windows 更新

        - 移除所有 Windows Update 功能的存取 (從 **啟用**變更為**未設定**)

        - 請勿連線到任何 Windows Update 網際網路位置 (從 **啟用**變更為**未設定**)。

    - 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ Windows 元件 \\ Windows 更新 \\ 商務用 Windows Update

        - 選取收到品質更新時 (從已啟用變更為未設定)

    -   本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ Windows 元件 \\ Windows 更新 \\ 商務用 Windows Update

        - 選取收到預覽組建和功能更新時 (從**已啟用**變更為**未設定**)

-  重新啟用服務

    - 更新 Orchestrator 服務 (從 **停用**變更為**自動 (延遲啟動)** )。

    - 編輯以下 Windows 登錄設定：

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState

            - DeferQualityUpdates (從 **1** 變更為 **0**)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings

            - PausedQualityDate (刪除任何現有的值)

        - HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU

            - 停用

-  重新啟用排定的工作

    - 工作排程器程式庫 \\ Microsoft \\ Windows \\ InstallService\\ ScanForUpdates

    - 工作排程器程式庫 \\ Microsoft \\ Windows \\ InstallService \\ ScanForUpdatesAsUser

若要讓所有設定生效，請重新啟動裝置。 如果不想讓此裝置提供功能更新，請移至設定 \\ Windows Update \\ 高級選項 \\ 選擇安裝更新的時間，然後手動設定選項，**功能更新包含新功能和改進。如此可將此天數延後到某個非零的值，例如 180、365 等等。**

### <a name="references"></a>參考

- [什麼是 VDI (虛擬桌面基礎結構)](https://www.citrix.com/glossary/vdi.html)

- [當您移除或更新包含內建 Windows 映像的 Microsoft Store 應用程式之後，Sysprep 會失敗](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu)。