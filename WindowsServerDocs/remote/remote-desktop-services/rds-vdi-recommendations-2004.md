---
title: 針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 2004 最佳化
description: 將當作 VDI 映像的 Windows 10 版本 1909 桌面的額外負荷降至最低的建議設定和組態。
ms.prod: windows-server
ms.reviewer: robsmi, timuessi
ms.technology: remote-desktop-services
author: Heidilohr
ms.author: helohr
ms.topic: article
manager: ''
ms.date: 09/24/2020
ms.openlocfilehash: b39207461d9ee99a2da0bdf07a9190c769b7511f
ms.sourcegitcommit: fa74f7297855ff1c9cb7df4b784b946cdce99e22
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2020
ms.locfileid: "91206397"
---
# <a name="optimizing-windows-10-version-2004-for-a-virtual-desktop-infrastructure-vdi-role"></a>針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 2004 最佳化

This article is intended to provide suggestions for configurations for Windows 10, build 2004, for optimal performance in Virtualized Desktop environments, including Virtual Desktop Infrastructure (VDI) and Windows Virtual Desktop. 本指南中的所有設定皆為供您參考的建議，而並非需求。

The information in this guide is pertinent to Windows 10, version 2004, operating system (OS) build 19041.

在 VDI 環境中，將 Windows 10 效能最佳化的主要方式為盡可能減少應用程式圖形重新繪製、減少對 VDI 環境沒有顯著效益的背景活動，以及將整體執行的處理序減少至最低限度。 次要目標是要將基底映像中的磁碟空間使用量降至最低限度。 透過 VDI 實作，可能的最小基底 (或「黃金」映像大小) 可稍微減少 Hypervisor 的記憶體使用量，並且可小幅減少將桌面映像提供給取用者所需的整體網路作業。

No optimizations should reduce the user experience. Each optimization setting has been carefully reviewed to ensure that there is no appreciable degradation to the user experience.

> [!NOTE]
> The settings in this article can be applied to other Windows 10 installations, such as version 1909, physical devices, or other virtual machines. There are no recommendations in this article that should affect supportability of Windows 10 in a virtual desktop environment.

## <a name="vdi-optimization-principles"></a>VDI 最佳化原則

A "full" virtual desktop environment can present a complete desktop session, including applications, to a computer user over a network. 網路傳遞車輛可以是內部部署網路或網際網路。 VDI 環境通常會使用基底作業系統映像，而此映像後續將成為桌面的基礎，供使用者工作之用。 There are variations of virtual desktop implementations such as "persistent", "non-persistent", and "desktop session." 持續性類型會將 VDI 桌面 OS 的變更保留到下一個工作階段。 非持續性類型則不會將 VDI 桌面作業系統的變更保留到下一個工作階段。 對使用者而言，此桌面除了可透過網路存取以外，與其他虛擬或實體裝置相較也稍有不同。

最佳化設定會在參考裝置上進行。 VM 是建置映像的理想位置，因為您可以在這裡儲存狀態、建立檢查點並進行備份。 預設的 OS 安裝是在基礎 VM 上執行。 該基礎 VM 接著會移除不必要的應用程式、安裝 Windows 更新、安裝其他更新、刪除暫存檔案，以及套用設定來進行最佳化。

還有其他類型的 VDI，例如遠端桌面工作模式 (RDS) 和最近發行的 [Windows 虛擬桌面](https://azure.microsoft.com/services/virtual-desktop/)。 有關這些技術的深入討論已超出本文的範圍。 本文著重於 Windows 基礎映像設定，而不需參考環境中的其他因素，例如主機最佳化。

對於其產品和服務，安全性和穩定性一向是 Microsoft 首要的優先考慮項目。 In the virtual desktop realm, security is not handled much differently than physical devices. Enterprise customers may choose to utilize the built-in to Windows services of Windows Security, which comprises a suite of services that work well connected or not connected to the Internet. 對於未連線到網際網路的 VDI 環境，系統每天可以下載安全性簽章數次，因為 Microsoft 可能每天會發行一個以上的簽章更新。 接著，您可以將這些簽章提供給 VDI VM，並排程在生產期間進行安裝，無論持續性或非持續性均適用。 如此一來，VM 保護就會盡可能維持在最新狀態。

有些安全性設定不適用於未連線到網際網路的 VDI 環境，因此無法參與具備雲端功能的安全性。 還有其他「正常」 Windows 裝置可能會利用的各項設定，例如雲端體驗、Windows Store 等等。 移除未使用功能的存取，可減少使用量、降低網路頻寬和受到攻擊的可能性。

Regarding updates, Windows 10 utilizes a monthly update rhythm. 在大多數情況下，VDI 管理員會透過關閉以「主要」或「黃金」映像為基礎的 VM 來控制更新的流程，將唯讀的映像解密、修補映像，然後重新密封並帶回生產環境。 因此，VDI VM 不需要檢查 Windows Update。 However, there are cases where normal patching procedures take place, like the case of persistent "personal" virtual desktop devices. In some cases, Windows Update can be utilized. In some cases, Intune could be utilized. In some cases Microsoft Endpoint Configuration Manager (formerly SCCM) is utilized to handle update and other package delivery. It is up to each organization to determine the best approach to updating virtual desktop devices, while reducing overhead cycles.

The local policy settings, as well as many other settings in this guide, can be overridden with domain-based policy. It is recommended to go through the policy settings thoroughly and remove or not use any that are not desired or applicable to your environment. The settings listed in this document try to achieve the best balance of performance optimization in virtual desktop environments, while maintaining a quality user experience.

> [!NOTE]
> There is a set of scripts available at GitHub.com, that will do all the work items documented in this paper. The Internet URL for the optimization scripts can be found at [https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool](https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool). This script was designed to be easily customizable for your environment and requirements. 主要的程式碼是 PowerShell，並使用輸入檔案 (純文字) 與本機群組原則物件 (LGPO) 工具匯出檔案來完成作業。 這些檔案包含要移除的應用程式清單，以及要停用的各項服務清單。 If you don't want to remove a particular app or disable a particular service, you can edit the corresponding text file and remove the item you do not want acted upon. Finally, there is an export of local policy settings that can be imported into your environment machines. 建議您在基礎映像中使用某些設定，而不是透過群組原則套用設定；因為某些設定會在下次重新開機時生效，或在第一次使用元件時生效。

### <a name="persistent-virtual-desktop-environments"></a>Persistent virtual desktop environments

持續性 VDI 屬於基本層級，是會在重新開機後仍保留作業系統狀態的 VM。 VDI 解決方案的其他軟體層可讓使用者輕鬆且順暢地存取其指派的 VM (通常是使用單一登入解決方案)。

持續 VDI 有數種不同的實作：

- 傳統虛擬機器的 VM 具有其本身的虛擬磁碟檔案，可以正常啟動，並將一個工作階段的變更儲存到下一個工作階段。 其差別在於使用者存取此 VM 的方式。 There may be a web portal the user signs in to that automatically directs the user to one or more virtual desktop devices (VMs) assigned to them.
- 依需求可搭配個人虛擬磁碟的映像式持續性虛擬機器。 在此類型的實作中，一或多個主機伺服器上會有基底/黃金映像。 在 VM 建立後，會建立一或多個虛擬磁碟，並指派給此磁碟供持續性儲存之用。
  - 在 VM 啟動時，系統會將基礎映像的複本讀取到 VM 的記憶體中。 同時，持續性虛擬磁碟會連同透過複雜的程序合併的任何既有作業系統變更，指派給該 VM。
  - Changes such as event log writes, log writes, and so on. are redirected to the read/write virtual disk assigned to that VM.
  - 在此情況下，作業系統和應用程式維護可使用傳統維護軟體 (例如 Windows Server Update Services) 或其他管理技術正常運作。
- The difference between a persistent virtual desktop device and a "normal" virtual desktop device is the relationship to the master/gold image. 在某些時候，更新必須套用至主要映像。 It is at this point where organizations decide how the user persistent changes are handled. In some cases, the disk with the user changes is discarded or reset. 也可能使用者所做的變更會透過每月的品質更新保留，而基礎會在功能更新之後重設。

### <a name="non-persistent-virtual-desktop-environments"></a>Non-persistent virtual desktop environments

非持續性 VDI 實作以基底或「黃金」映像為基礎時，最佳化主要會在基底映像上執行，其次才是透過本機設定與本機原則執行。

With image-based non-persistent (NP) virtual desktop environments, the base image is read-only. When an NP virtual desktop device (VM) is started, a copy of the base image is streamed to the VM. 在啟動期間及其後到下次重新開機之間所發生的活動，都會重新導向至暫存位置。 使用者通常會獲得用來儲存其資料的網路位置。 在某些情況下，使用者的設定檔會與標準 VM 合併，而為使用者提供其設定。

對於以單一映像為基礎的非持續性 VDI 而言，維護是其重要層面之一。 作業系統和元件的更新通常會每個月提供一次。 在映像式 VDI，要取得映像的更新必須執行一組程序：

- 在指定的主機上，所有衍生自基礎映像的 VM 都必須關閉。 這表示使用者會重新導向至其他 VM。

- In some implementations, this is referred to as "draining." The virtual machine or session host, when set to draining mode, stops accepting new requests, but continues servicing users currently connected to the device.

- In draining mode, when the last user logs off the device, that device is then ready for servicing operations.
- 隨後，基底映像會開啟並啟動。 All maintenance activities are then performed, such as OS updates, .NET updates, app updates, and so on.
- 任何要套用的新設定都會在此時套用。
- 任何其他維護也都會在此時執行。
- 其後，基底映像會關閉。
- 基底映像會進行密封，並依設定回到生產環境。
- 使用者可以重新登入。

> [!NOTE]
> Windows 10 會定期自動執行一組維護工作。 有一項排定工作依預設會在每天凌晨 3:00 執行。 這項排定工作會執行一系列的工作，包括 Windows Update 清理。 您可以使用下列 PowerShell 命令，檢視所有會自動執行的維護工作類別：
>
> ```powershell
> Get-ScheduledTask | Where-Object {$_.Settings.MaintenanceSettings}
> ```

非持續性 VDI 的挑戰之一，是當使用者登出時，幾乎所有的作業系統活動都會捨棄。 使用者的設定檔和/或狀態可能會集中儲存到一個位置，但虛擬機器本身會捨棄前次開機後所做的絕大多數變更。 因此，預計要對可將狀態保留到下一個工作階段的 Windows 電腦進行的最佳化，將不適用。

Depending on the architecture of virtual desktop device, things like PreFetch and SuperFetch are not going to help from one session to the next, as all the optimizations are discarded on VM restart. 編製索引可能會浪費資源，傳統磁碟重組之類的任何磁碟最佳化也是一樣。

> [!NOTE]
> 如果使用虛擬化來準備映像，而且在映像建立過程中連線到網際網路，則在第一次登入時，您應該前往設定 Windows Update 來延遲功能更新。

### <a name="to-sysprep-or-not-sysprep"></a>是否使用 Sysprep

Windows 10 has a built-in capability called the [System Preparation Tool](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview), also known as sysprep. Sysprep 工具可用來為自訂的 Windows 10 映像進行複製準備。 Sysprep 程序可確保產生的作業系統會有適當的獨特性可在生產環境中執行。

執行 Sysprep 各有其優缺點。 以 VDI 為例，您可能會希望能夠自訂預設使用者設定檔，以供後續使用此映像登入的使用者作為設定檔範本。 您可能會想要安裝某些應用程式，但同時又希望能控制個別應用程式的設定。

替代方式是使用標準 .ISO 進行安裝；您可以搭配使用自動的安裝回應檔案，以及用來安裝應用程式或移除應用程式的工作序列。 您也可以使用工作序列來設定映像中的本機原則設定；或許可搭配使用[本機群組原則物件公用程式 (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0) 工具。

To learn more about image preparation for Azure, see [Prepare a Windows VHD or VHDX to upload to Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)

### <a name="supportability"></a>可支援性

每當 Windows 預設值變更時，就會發生有關可支援性的問題。 自訂 VDI 映像 (VM 或工作階段) 之後，每次對映像所做的變更都必須在變更記錄檔中追蹤。 If a time comes to troubleshoot, often an image can be isolated in a pool and configured for problem analysis. 一旦追蹤到問題的根本原因之後，系統會先將該變更推到測試環境，最後再到生產工作負載。

本文件刻意避免觸及影響安全性的系統服務、原則或工作。 之後就是 Windows 服務。 已移除維護期間以外的 VDI 映像服務，因為維護期間會在 VDI 環境中進行大部分服務事件時發生，「除了」  安全性軟體更新。 Microsoft 已針對 VDI 環境中的 Windows 安全性發佈指引。

如需詳細資訊，請參閱在虛擬桌面基礎結構 (VDI) 環境中部署 Windows Defender 防毒軟體的指南。

改變預設的 Windows 設定時，請考慮可支援性。 Occasionally difficult to solve problems arise when altering system services, policies, or scheduled tasks, in the name of hardening, "lightening," and so on. Consult the Microsoft Knowledge Base for current known issues regarding altered default settings. 本文件中的指導方針以及 GitHub 上相關的程式碼，將會在已知問題發生時持續更新。 此外，您可以透過數種方式向 Microsoft 回報問題。

您可以使用慣用的搜尋引擎，使用 “"start value" site:support.microsoft.com” 一詞，提出有關服務預設起始值的已知問題。

您可能會注意到，本文件和 GitHub 上相關的指令碼並不會修改任何預設權限。 If you are interested in increasing your security settings in a supported and robust manner, start with the project known as "**AaronLocker**", which can be found at [ANNOUNCING: Application whitelisting with "AaronLocker"](https://blogs.msdn.microsoft.com/aaron_margosis/2018/06/26/announcing-application-whitelisting-with-aaronlocker/)

### <a name="virtual-desktop-optimization-categories"></a>Virtual desktop optimization categories

- 通用 Windows 平台 (UWP) 應用程式清理
- 選用功能清理
- 本機原則設定
- 系統服務
- 排定的工作
- 套用 Windows (和其他) 更新
- 自動 Windows 追蹤
- Windows Defender 對 VDI 的最佳化
- 依登錄設定進行的用戶端網路效能微調
- Windows 受限流量有限功能基準指引中的其他設定
- [磁碟清理]

### <a name="universal-windows-platform-uwp-application-cleanup"></a>通用 Windows 平台 (UWP) 應用程式清除

One of the goals of a virtual desktop image is to be as light as possible with respect to persistent storage. 縮小映像大小的方式之一，是移除環境中不會使用的 UWP 應用程式。 使用 UWP 應用程式時，會有主要的應用程式檔案，也就是承載。 每個使用者設定檔中都會儲存少量的資料，供應用程式的特定設定使用。 此外，「所有使用者」設定檔中也會有少量的資料。

In addition, all UWP apps are registered at either the user or machine level at some point after startup for the device, and login for the user. The UWP apps, which include the Start Menu and the Windows Shell, perform various tasks at or after installation, and again when a user logs in for the first time, and to a lesser extent at subsequent logins. For all UWP apps, there are occasional evaluations that take place, such as:

- Do you need to update the app to the latest version?
- The app, if pinned to the Start Menu, might have live tile data to download
- Does the app have a cache of data that needs to be updated, such as maps or weather?
- Does the app have persistent data from the user's profile that needs to be presented at login (for example, Sticky Notes)

With a default installation of Windows 10, not all UWP apps may be used by an organization. Therefore, if those apps are removed, there are fewer evaluations that need to take place, less caching, and so on. The second method here is to direct Windows to disable "consumer experiences." This reduces Store activity by having to check for every user what apps are installed, what apps are available, and then to start downloading some UWP apps. The performance savings can be significant when there are hundreds or thousands of users, all start work at approximately the same time, or even starting work at rolling times across time zones.

就 UWP 應用程式清理而言，連線能力和時機是重要的因素。 如果您將基礎映像部署到沒有網路連線的裝置，在您嘗試將應用程式解除安裝時，Windows 10 將無法連線至 Microsoft Store 下載應用程式並嘗試加以安裝。 這項策略可讓您有時間自訂映像，然後在建立映像程序的後續階段中更新其餘的內容。

如果您修改用來安裝 Windows 10 的基底 .WIM，並在安裝之前從 .WIM 中移除了不必要的 UWP 應用程式，則這些應用程式將不會安裝並啟動，而您的設定檔建立時間應該會縮短。 本節稍後的內容中會說明關於如何從安裝 .WIM 檔案中移除 UWP 應用程式的資訊。

就 VDI 而言，佈建您在基底映像中需要的應用程式，並在其後限制或封鎖對 Microsoft Store 的存取，是不錯的策略。 在一般的電腦上，市集應用程式會定期在背景更新。 UWP 應用程式可在維護期間，於系統套用其他更新時進行更新。

#### <a name="delete-the-payload-of-uwp-apps"></a>刪除 UWP 應用程式的承載

不需要的 UWP 應用程式仍會在檔案系統中耗用少量的磁碟空間。 針對永遠不會用到的應用程式，您可以使用 PowerShell 命令從基底映像中移除非必要 UWP 應用程式的承載。 事實上，如果您使用本節後續提供的連結從安裝 .WIM 檔案中移除這類承載，您應該能夠以非常精簡的 UWP 應用程式清單從頭開始。

執行下列命令可列舉執行中的作業系統已佈建的 UWP 應用程式，如 PowerShell 所產生的下列節錄範例輸出所示：

```powershell
Get-AppxProvisionedPackage -Online

DisplayName  : Microsoft.3DBuilder
Version      : 13.0.10349.0  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
Regions      :

DisplayName  : Microsoft.Appconnector
Version      : 2015.707.550  
Architecture : neutral
ResourceId   : \~
PackageName  : Microsoft.Appconnector_2015.707.550.0_neutral_\~_8wekyb3d8bbwe
Regions      :
...
```

UWP apps that are provisioned to a system can be removed during OS installation as part of a task sequence, or later after the OS is installed. This may be the preferred method because it makes the overall process of creating or  maintaining an image modular. 在您開發指令碼後，如果某個項目在後續組建中有所變更，您可以編輯現有的指令碼，而非從頭重複相關程序。

If you want to learn more, here are some resources that can help you:

- [在工作序列期間移除 Windows 10 內建應用程式](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)
- [使用 Powershell 1.3 版移除 Windows 10 WIM 檔案中的內建應用程式](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)
- [Windows 10 1607：使應用程式不會在部署功能更新時再次出現](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/)
- [在工作序列期間移除 Windows 10 內建應用程式](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/)

然後，執行 Remove-AppxProvisionedPackage PowerShell 命令以移除 UWP 應用程式承載：

```powershell
Remove-AppxProvisionedPackage -Online - PackageName MyAppxPackage
```

As a final note on this topic, each UWP app should be evaluated for applicability in each unique environment. 您可以安裝 Windows 10 版本 1909 的預設安裝，然後記下哪些應用程式正在執行且耗用記憶體。 例如，您可以考慮移除會自動啟動的應用程式，或是自動在 [開始] 功能表中顯示資訊 (例如氣象和新聞)、但在您的環境中可能不會用到的應用程式。

> [!NOTE]
> 如果使用來自 GitHub 的指令碼，您可以在執行指令碼之前，輕鬆地控制要移除哪些應用程式。 下載指令碼檔案之後，找出檔案 ‘Win10_1909_AppxPackages.txt’ 並加以編輯，然後移除您想要保留的應用程式項目，例如計算機、自黏便箋等等。

## <a name="windows-optional-features-cleanup"></a>針對 Windows 選用功能

### <a name="managing-optional-features-with-powershell"></a>使用 PowerShell 管理選用功能

如需詳細資訊，請參閱 [Windows 10：使用 PowerShell 管理選用功能](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx)。

您可以使用 PowerShell 管理 Windows 選用功能。 若要列舉目前已安裝的 Windows 功能，請執行下列 PowerShell 命令：

```powershell
Get-WindowsOptionalFeature -Online
```

Using PowerShell, an enumerated Windows Optional Feature can be configured as enabled or disabled, as in the following example:

```powershell
Enabled-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

Here's an example command that disables the Windows Media Player feature in the virtual desktop image:

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```

接下來，建議您移除 Windows Media Player 套件。 This example command will show you how to do that:

```powershell

PS C:\> Get-WindowsPackage -Online -PackageName *media*

PackageName              : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153
Applicable               : True
Copyright                : Copyright (c) Microsoft Corporation. All Rights Reserved
Company                  :
CreationTime             :
Description              : Play audio and video files on your local machine and on the Internet.
InstallClient            : DISM Package Manager Provider
InstallPackageName       : Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153.mum
InstallTime              : 5/11/2020 5:43:37 AM
LastUpdateTime           :
DisplayName              : Windows Media Player
ProductName              : Microsoft-Windows-MediaPlayer-Package
ProductVersion           :
ReleaseType              : OnDemandPack
RestartRequired          : Possible
SupportInformation       : http://support.microsoft.com/?kbid=777777
PackageState             : Installed
CompletelyOfflineCapable : Undetermined
CapabilityId             : Media.WindowsMediaPlayer~~~~0.0.12.0
Custom Properties        :

Features                 : {}
```

如果您想移除 Windows Media Player 套件 (以釋放大約 60 MB 的磁碟空間)：

```powershell
PS C:\Windows\system32> Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153 -Online

Path          :
Online        : True
RestartNeeded : False
```

### <a name="enable-of-disabling-windows-features-using-dism"></a>Enable of disabling Windows features using DISM

您可以使用內建的 Dism.exe 工具來列舉及控制 Windows 選用功能。 在作業系統安裝工作進行期間，可以開發並執行 Dism.exe 指令碼。 此處所涉及的 Windows 技術稱為功能隨選安裝。 See the following article for more about Features on Demand in Windows:

Microsoft: [Features on Demand](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)

### <a name="default-user-settings"></a>預設使用者設定

You can customize the Windows registry file at `C:\Users\Default\NTUSER.DAT`. 對此檔案所做的任何設定，都會套用到從執行此映像的裝置建立的任何後續使用者設定檔。 您可以編輯檔案 ‘Win10_1909_DefaultUserSettings.txt’，控制要套用至預設使用者設定檔的設定。

此外，若要減少透過 VDI 基礎結構傳輸影像的能力，您可以將預設背景設定為純色，而不是預設的 Windows 10 影像。 您也可以將登入畫面設定為純色，並在登入時關閉不透明的模糊效果。

下列設定會套用到預設的使用者設定檔登錄區，主要是為了減少動畫效果。 如果您不想進行部分或全部設定，請根據此影像刪除不會套用至新使用者設定檔的設定。 這些設定的目標是要啟用下列對等設定：

- Show shadows under mouse pointer
- Show shadows under windows
- Smooth edges of screen fonts

![A screenshot of the performance options menu with the relevant items selected.](media/performance-options.png)

And, new to this version of settings is a method to disable the following two privacy settings for any user profile created after you run the optimization:

- 透過讓網站存取我的語言清單以提供當地相關內容
- Show me suggested content in the Settings app

![A screenshot of the privacy settings window. The two disabled settings are highlighted in red.](media/privacy-settings.png)

以下是套用至預設使用者設定檔登錄區的最佳化設定，目的是讓效能最佳化： Note that this operation is performed by first loading the default user profile registry hive **NTUser.dat**, as the ephemeral key name **Temp**, and then making the below listed modifications:

```regedit
Load HKLM\Temp C:\Users\Default\NTUSER.DAT
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer" /v ShellState /t REG_BINARY /d 240000003C2800000000000000000000 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v IconsOnly /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewAlphaSelect /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ListviewShadow /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowCompColor /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v ShowInfoTip /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced" /v TaskbarAnimations /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\Explorer\VisualEffects" /v VisualFXSetting /t REG_DWORD /d 3 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v EnableAeroPeek /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\DWM" /v AlwaysHiberNateThumbnails /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v DragFullWindows /t REG_SZ /d 0 /f
add "HKLM\Temp\Control Panel\Desktop" /v FontSmoothing /t REG_SZ /d 2 /f
add "HKLM\Temp\Control Panel\Desktop" /v UserPreferencesMask /t REG_BINARY /d 9032078010000000 /f
add "HKLM\Temp\Control Panel\Desktop\WindowMetrics" /v MinAnimate /t REG_SZ /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338393Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353694Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-353696Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338388Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SubscribedContent-338389Enabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\ContentDeliveryManager" /v SystemPaneSuggestionsEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Control Panel\International\User Profile" /v HttpAcceptLanguageOptOut /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.Windows.Photos_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.SkypeApp_kzf8qxf38zg5c" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.YourPhone_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v Disabled /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\BackgroundAccessApplications\Microsoft.MicrosoftEdge_8wekyb3d8bbwe" /v DisabledByUser /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitInkCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization" /v RestrictImplicitTextCollection /t REG_DWORD /d 1 /f
add "HKLM\Temp\Software\Microsoft\Personalization\Settings" /v AcceptedPrivacyPolicy /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\InputPersonalization\TrainedDataStore" /v HarvestContacts /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
add "HKLM\Temp\Software\Microsoft\Windows\CurrentVersion\UserProfileEngagement" /v ScoobeSystemSettingEnabled /t REG_DWORD /d 0 /f
Unload HKLM\Temp
```

Another series of default user settings recently added is to disable several Windows apps from starting and running in the background. While not significant on a single device, Windows 10 starts up a number of processes for each user session on a given device (host). The Skype app is one example, and Microsoft Edge is another. The settings included turn off several apps from being able to run in the background. If this functionality is desired as it is, just delete out the lines in the "DefaultUserSettings.txt" file that include the app names "**Windows.Photos**," "**SkypeApp**," "**YourPhone**," and/or "**MicrosoftEdge**."

### <a name="local-policy-settings"></a>本機原則設定

VDI 環境中的許多 Windows 10 最佳化都可使用 Windows 原則來進行。 本節的表格中所列的設定可套用至基礎/黃金映像。 而且，如果未以任何其他方式 (例如藉由群組原則) 指定等同的設定，就仍會套用這些設定。

Note that some decisions may be based on the specifics of the environment.

- VDI 環境是否可存取網際網路？
- Is the virtual desktop solution persistent or non-persistent?

下列設定確保不會與任何關係到安全性的設定發生牴觸或衝突。 我們選擇移除這些設定或停用可能不適用於 VDI 環境的功能。

| 原則設定 | 項目 | 子項目 | 可能的設定和註解 |
|--------------|----|--------|----------------------------|
| 本機電腦原則 \\ 電腦設定 \\ Windows 設定 \\ 安全性設定 | N/A | N/A | N/A |
| 網路清單管理員原則 | 所有網路內容 | 網路位置 | **User cannot change location** (This is set to prevent the right-hand side pop-up when a new network is detected) |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 控制台 | N/A | N/A |
| 控制台 | 允許線上秘訣 | N/A  | 已停用 (設定將不會聯繫 Microsoft 內容服務以擷取秘訣和說明內容) |
| 控制台 個人化 | 強制使用特定的預設鎖定畫面和登入影像 | N/A | Enabled (This setting allows you to force a specific default lock screen and logon image by entering the path (location) of the image file. The same image will be used for both the lock and logon screens. <p>The reason for this recommendation is to reduce bytes transmitted over the network for virtual desktop environments. This setting can be removed or customized for each environment.)|
|控制台 地區及語言選項手寫個人化|關閉自動學習| N/A |已啟用 (如果啟用此原則設定，將會停止自動學習，並且刪除任何已儲存的資料。 使用者無法在控制台中設定此設定)|
|本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 網路|N/A|N/A|N/A|
|背景智慧型傳送服務 (BITS)|允許 BITS 對等快取| N/A |**Disabled** (This policy setting determines if the Background Intelligent Transfer Service (BITS) peer caching feature is enabled on a specific computer.)|
|背景智慧型傳送服務 (BITS)|不允許 BITS 用戶端使用 Windows 分支快取|N/A|**Enabled** (With this policy setting enabled, the BITS client does not use Windows Branch Cache.)<p>The reason for this recommendation is so that virtual desktop devices are not used for content caching, and the devices will not be allowed to use the network bandwidth to do so.|
|背景智慧型傳送服務 (BITS)|不允許此電腦做為 BITS 對等快取用戶端|N/A|**Enabled** (With this policy setting enabled, the computer will no longer use the BITS peer caching feature to download files; files will be downloaded only from the origin server.)|
背景智慧型傳送服務 (BITS)|不允許此電腦做為 BITS 對等快取伺服器|N/A|**Enabled** (With this policy setting enabled, the computer will no longer cache downloaded files and offer them to its peers.)|
|BranchCache|開啟 BranchCache|N/A|**Disabled** (With this selection disabled, BranchCache is turned off for all client computers where the policy is applied.)|
|*字型|Enabled Font Providers|N/A|**Disabled** (With this setting disabled, Windows does not connect to an online font provider and only enumerates locally installed fonts)|
|作用區驗證|啟用作用區驗證| N/A |**Disabled** (This policy setting defines whether WLAN hotspots are probed for Wireless Internet Service Provider roaming (WISPr) protocol support. With this policy setting disabled, WLAN hotspots are not probed for WISPr protocol support, and users can only authenticate with WLAN hotspots using a web browser.)|
|Microsoft 對等網路服務|關閉 Microsoft 對等網路服務|N/A|**Enabled** (This setting turns off Microsoft Peer-to-Peer Networking Services in its entirety and will cause all dependent applications to stop working. If you enable this setting, peer-to-peer protocols will be turned off.)|
|網路連線狀態指示器<p>網路連線狀態指示器 (請注意，此區段中有其他設定可以在隔離的網路中使用)|指定被動輪詢|Disable passive poling (**checkbox**)|**Enabled** (This Policy setting enables you to specify passive polling behavior. NCSI polls various measurements throughout the network stack on a frequent interval to determine if network connectivity has been lost. Use the options to control the passive polling behavior.)<P>Disabling NCIS passive polling can improve CPU workload on servers or other machines whose network connectivity is static.|
|離線檔案|允許或禁止使用離線檔案功能|N/A|**Disabled** (This policy setting determines whether the Offline Files feature is enabled. Offline Files saves a copy of network files on the user's computer for use when the computer is not connected to the network.With this policy setting disabled, Offline Files feature is disabled and users cannot enable it.)|
|TCPIP 設定 IPv6 轉換技術| 設定 Teredo 狀態|已停用狀態|**Enabled** (With this setting enabled, and set to "Disabled State", no Teredo interfaces are present on the host)|
WLAN 服務  WLAN 設定|允許 Windows 自動連線至建議的開放式熱點、連絡人共用的網路，及提供付費服務的熱點。|N/A|**Disabled** (This policy setting determines whether users can enable the following WLAN settings: "Connect to suggested open hotspots," "Connect to networks shared by my contacts," and "Enable paid services." 已停用 ([連線至建議的開放式熱點]  、[連線至我的連絡人共用的網路]  和 [啟用付費服務]  將會關閉，且此裝置上的使用者將無法加以啟用。)|
|WWAN Service\ Cellular Data Access|Let Windows apps access cellular data|所有應用程式的預設值：強制拒絕|已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取動作資料，且您組織中的員工無法加以變更。)|
|本機電腦原則  電腦設定  系統管理範本  開始功能表和工作列|N/A|N/A|
|*通知|關閉通知網路使用方式| N/A |已啟用 (如果啟用此原則設定，應用程式和系統功能將無法透過網路從 WNS 或使用通知輪詢 API 接收通知。)|
|本機電腦原則  電腦設定  系統管理範本  系統| N/A | N/A |N/A|
|裝置安裝|當裝置上已安裝標準驅動程式時，不要傳送 Windows 錯誤報告| N/A |**Enabled** (With this policy setting enabled, an error report is not sent when a generic driver is installed.)|
|裝置安裝|在通常會提示建立系統還原點的裝置活動期間，防止建立系統還原點| N/A |**Enabled** (With this policy setting enabled, Windows does not create a system restore point when one would normally be created.)|
|裝置安裝|防止從網際網路擷取裝置中繼資料| N/A |**Enabled** (This policy setting allows you to prevent Windows from retrieving device metadata from the Internet. With this policy setting enabled, Windows does not retrieve device metadata for installed devices from the Internet. This policy setting overrides the setting in the Device Installation Settings dialog box (Control Panel > System and Security > System > Advanced System Settings > Hardware tab).)|
|裝置安裝|在裝置安裝期間關閉 [找到新硬體] 註解方塊| N/A |**Enabled** (This policy setting allows you to turn off "Found New Hardware" balloons during device installation. With this policy setting enabled, "Found New Hardware" balloons do not appear while a device is being installed.)|
|檔案系統NTFS|簡短名稱建立選項|Short name creation options: Disabled on all volumes|**Enabled** (These settings provide control over whether or not short names are generated during file creation. Some applications require short names for compatibility, but short names have a negative performance impact on the system. With short names disabled on all volumes then they will never be generated.)|
|群組原則|在此裝置上繼續體驗| N/A |**Disabled** (This policy setting determines whether the Windows device is allowed to participate in cross-device experiences (continue experiences). Disabling this policy prevents this device from being discoverable by other devices, and thus cannot participate in cross-device experiences.)|
|網際網路通訊管理  網際網路通訊設定|關閉事件檢視器 "Events.asp" 連結| N/A |**Enabled** (This policy setting specifies whether "Events.asp" hyperlinks are available for events within the Event Viewer application.)|
|網際網路通訊管理  網際網路通訊設定|關閉個人化手寫資料共用| N/A |**Enabled** (Turns off data sharing from the handwriting recognition personalization tool.)|
|網際網路通訊管理  網際網路通訊設定|關閉手寫辨識錯誤報告| N/A |**Enabled** (Turns off the handwriting recognition error reporting tool.)|
|網際網路通訊管理  網際網路通訊設定|關閉說明及支援中心 Microsoft 知識庫搜尋| N/A |**Enabled** (This policy setting specifies whether users can perform a Microsoft Knowledge Base search from the Help and Support Center.)|
|網際網路通訊管理  網際網路通訊設定|如果 URL 連線正在參照 Microsoft.com 時，關閉網際網路連線精靈| N/A |**Enabled** (This policy setting specifies whether the Internet Connection Wizard can connect to Microsoft to download a list of Internet Service Providers (ISPs).)|
|網際網路通訊管理  網際網路通訊設定|關閉網頁發佈和線上訂購精靈的網際網路下載| N/A |**Enabled** (This policy setting specifies whether Windows should download a list of providers for the web publishing and online ordering wizards.)|
|網際網路通訊管理  網際網路通訊設定|關閉網際網路檔案關聯服務| N/A |**Enabled** (This policy setting specifies whether to use the Microsoft Web service for finding an application to open a file with an unhandled file association.)|
|網際網路通訊管理  網際網路通訊設定|如果 URL 連線正在參照 Microsoft.com 時，關閉註冊| N/A |**Enabled** (This policy setting specifies whether the Windows Registration Wizard connects to Microsoft.com for online registration.)|
|網際網路通訊管理  網際網路通訊設定|關閉搜尋小幫手內容檔更新：| N/A |**Enabled** (This policy setting specifies whether Search Companion should automatically download content updates during local and Internet searches.)|
|網際網路通訊管理  網際網路通訊設定|關閉「訂購沖印」圖片工作| N/A |**Enabled** (If you enable this policy setting, the task "Order Prints Online" is removed from Picture Tasks in File Explorer folders.)|
網際網路通訊管理  網際網路通訊設定|關閉檔案及資料夾的「發佈到網站」工作| N/A |關閉檔案及資料夾的 [發佈到網站] 工作：指定 Windows 資料夾的 [檔案和資料夾工作]   是否有 [將此檔案發佈到網站]、[將此資料夾發佈到網站] 及 [將選取項目發佈到網站] 工作。|
|網際網路通訊管理  網際網路通訊設定|關閉 Windows 客戶經驗改進計畫| N/A |客戶經驗改進計畫 (以下稱「CEIP」) 會從 Configuration Manager 主控台收集有關您的硬體設定以及如何使用我們的軟體與服務的基本資訊，從中找出趨勢與使用模式。 如果您啟用這個原則設定，所有使用者都不能加入 Windows 客戶經驗改進計畫。|
|網際網路通訊管理  網際網路通訊設定|關閉 Windows 錯誤報告| N/A |**Enabled** (This policy setting controls whether or not errors are reported to Microsoft. If you enable this policy setting, users are not given the option to report errors.)|
|網際網路通訊管理  網際網路通訊設定|關閉 Windows Update 裝置驅動程式搜尋| N/A |**Enabled** (This policy setting specifies whether Windows searches Windows Update for device drivers when no local drivers for a device are present. If you enable this policy setting, Windows Update is not searched when a new device is installed.)|
|登入|Do not display the Getting Started welcome screen at logon| N/A |**Enabled** (With this setting enabled, the welcome screen is hidden from the user logging on to a Windows device.)|
|登入|Do not enumerate connected users on domain-joined computers| N/A |**Enabled** (With this setting enabled, the Logon UI will not enumerate any connected users on domain-joined computers.)|
|登入|Enumerate local users on domain-joined computers| N/A |**Disabled** (With this setting disabled, the Logon UI will not enumerate local users on domain-joined computers.)|
|登入|Show clear logon background| N/A |**Enabled** (This policy setting disables the acrylic blur effect on logon background image. With this setting enabled, the logon background image shows without blur.)|
|登入|顯示首次登入動畫| N/A |**Disabled** (This policy setting allows you to control whether users see the first sign-in animation when signing in to the computer for the first time. This applies to both the first user of the computer who completes the initial setup and users who are added to the computer later. It also controls if Microsoft account users will be offered the opt-in prompt for services during their first sign-in.<p>With this setting disabled, users will not see the first logon animation and Microsoft account users will not see the opt-in prompt for services.)|
|登入|關閉鎖定畫面上的應用程式通知| N/A |**Enabled** (This policy setting allows you to prevent app notifications from appearing on the lock screen. With this setting enabled, no app notifications are displayed on the lock screen.)|
|電源管理|選取使用中電源計劃|Active Power Plan: High Performance|**Enabled** (If you enable this policy setting, specify a power plan from the Active Power Plan list.) <p>With the "Power" service disabled, the Powercfg.cpl UI is not able to display these power options, and instead returns an RPC error.|
|Power Management \ Video and Display Settings|Turn on desktop background slideshow (plugged-in)| N/A |**Disabled** (This policy setting allows you to specify if Windows should enable the desktop background slideshow.) With this setting disabled, the desktop background slideshow is disabled. This setting likely has no effect on a VM.|
|修復|允許將系統還原到預設狀態| N/A |**Disabled** (With this setting disabled, the items "Use a system image you created earlier to recover your computer" and "Reinstall Windows" (or "Return your computer to factory condition") in Recovery (in Control Panel) will be unavailable.)|
|*儲存空間健全狀況|允許下載磁碟失敗預測模型的更新| N/A |已停用 (不會下載磁碟失敗預測模型的更新)|
|[系統還原]|Turn off System Restore| N/A |**Enabled** (With this setting enabled, System Restore is turned off, and the System Restore Wizard cannot be accessed. The option to configure System Restore or create a restore point through System Protection is also disabled.).)|
|疑難排解與診斷 排程維護|設定排程維護行為| N/A |**Disabled** (Determines whether scheduled diagnostics will run to proactively detect and resolve system problems. With this policy setting disabled, Windows will not be able to detect, troubleshoot or resolve problems on a scheduled basis.)|
|Troubleshooting and Diagnostics\ Scripted Diagnostics|Troubleshooting: Allow users to access and run Troubleshooting wizards| N/A |**Disabled** (With this setting disabled, users cannot access or run the troubleshooting tools from the Control Panel.)|
|Troubleshooting and Diagnostics\ Scripted Diagnostics|Troubleshooting: Allow users to access online troubleshooting content on Microsoft servers from the Troubleshooting Control Panel (via the Windows Online Troubleshooting Service – WOTS)| N/A |**Disabled** (With this setting disabled, users can only access and search troubleshooting content that is available locally on their computers, even if they are connected to the Internet. They are prevented from connecting to the Microsoft servers that host the Windows Online Troubleshooting Service.|
|疑難排解與診斷  Windows 開機效能診斷|設定狀況執行層級| N/A |**Disabled** (Determines the execution level for Windows Boot Performance Diagnostics. If you disable this policy setting, Windows will not be able to detect, troubleshoot or resolve any Windows Boot Performance problems that are handled by the DPS.)<p>This setting can be very useful during design, test, development, or maintenance phases. This setting could be enabled on an isolated VM or session host, measurements taken, and results noted in event logs under "Microsoft-Windows-Diagnostics-Performance/Operational" Source: Diagnostics-Performance, Task Category "Boot Performance Monitoring."<p>**ALSO**: With the DPS service disabled, this setting has no effect, as Windows will not be logging performance data.|
|疑難排解與診斷  Windows 記憶體流失診斷|設定狀況執行層級| N/A |**Disabled** (This policy setting determines whether Diagnostic Policy Service (DPS) diagnoses memory leak problems. With this setting disabled, the DPS is not able to diagnose memory leak problems.) <p>Many diagnostics modes can be enabled, and tools used such as WPT, though these are usually done in dev/test/maintenance scenarios and not enabled and used on production VMs or sessions|
|Troubleshooting and Diagnostics\ Windows Performance PerfTrack|Enable/Disable PerfTrack| N/A |**Disabled** (This policy setting specifies whether to enable or disable tracking of responsiveness events. With this setting disabled, responsiveness events are not processed.)|
|疑難排解與診斷  Windows 資源耗損偵測與解析|設定狀況執行層級| N/A |**Disabled** (Determines the execution level for Windows Resource Exhaustion Detection and Resolution. With this setting disabled, Windows will not be able to detect, troubleshoot or resolve any Windows Resource Exhaustion problems that are handled by the DPS.)|
|疑難排解與診斷  Windows 關機效能診斷|設定狀況執行層級| N/A |**Disabled** (Determines the execution level for Windows Shutdown Performance Diagnostics. With this setting disabled, Windows will not be able to detect, troubleshoot or resolve any Windows Shutdown Performance problems that are handled by the DPS.)|
|疑難排解與診斷  Windows 待命/繼續效能診斷|設定狀況執行層級| N/A |**Disabled** (Determines the execution level for Windows Standby/Resume Performance Diagnostics. With this setting disabled, Windows will not be able to detect, troubleshoot or resolve any Windows Standby/Resume Performance problems that are handled by the DPS.)|
|疑難排解與診斷  Windows 系統回應效能診斷|設定狀況執行層級| N/A |**Disabled** (Determines the execution level for Windows System Responsiveness Diagnostics. With this setting disabled, Windows will not be able to detect, troubleshoot or resolve any Windows System Responsiveness problems that are handled by the DPS.)|
*使用者設定檔|關閉廣告識別碼| N/A |**Enabled** (With this setting enabled, the advertising ID is turned off. 應用程式無法跨應用程式使用識別碼。|
|本機電腦原則  電腦設定  系統管理範本  Windows 元件| N/A | N/A | N/A |
|*應用程式隱私權|讓 Windows 應用程式存取其他應用程式的診斷資訊|所有應用程式的預設值：強制拒絕|**Enabled** (With this setting enabled, and using the "Force Deny" option, Windows apps are not allowed to get diagnostic information about other apps and employees in your organization cannot change it.)|
|*應用程式隱私權|讓 Windows App 存取位置|所有應用程式的預設值：強制拒絕|**Enabled** (With this setting enabled, and using the "Force Deny" option, Windows apps are not allowed to access location and users cannot change the setting.|
|*應用程式隱私權|讓 Windows 應用程式存取動作資料|所有應用程式的預設值：強制拒絕|**Enabled** (With this setting enabled, and using the "Force Deny" option, Windows apps are not allowed to access motion data and users cannot change the setting.)|
|*應用程式隱私權|讓 Windows 應用程式存取通知|所有應用程式的預設值：強制拒絕|**Enabled** (With this setting enabled, and using the "Force Deny" option, Windows apps are not allowed to access notifications and users cannot change the setting)|
|*應用程式隱私權|Let Windows apps activate with voice|所有應用程式的預設值：強制拒絕|**Enabled** (This policy setting specifies whether Windows apps can be activated by voice.)|
|*應用程式隱私權|Let Windows apps activate with voice while the system is locked|所有應用程式的預設值：強制拒絕|**Enabled** (This policy setting specifies whether Windows apps can be activated by voice while the system is locked.)|
|*應用程式隱私權|Let Windows apps control radios|所有應用程式的預設值：強制拒絕|已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無權控制無線通訊，且您組織中的員工無法加以變更。)|
|應用程式相容性|Turn off Inventory Collector| N/A |**Enabled** (This policy setting controls the state of the Inventory Collector. The Inventory Collector inventories applications, files, devices, and drivers on the system and sends the information to Microsoft. With this policy setting enabled, the Inventory Collector will be turned off and data will not be sent to Microsoft. Collection of installation data through the Program Compatibility Assistant is also disabled.)|
|自動播放原則|設定 AutoRun 的預設行為|不執行任何 AutoRun 命令|**Enabled** (This policy setting sets the default behavior for Autorun commands.)|
|自動播放原則|關閉自動播放|All drives|**Enabled** (If you enable this policy setting, Autoplay is disabled on all drives.)|
|*雲端內容|不顯示 Windows 祕訣| N/A |已啟用 (此原則設定可防止對使用者顯示 Windows 秘訣。)|
|*雲端內容|關閉 Microsoft 消費者體驗| N/A |如果啟用此原則設定，使用者將不會再看到 Microsoft 提供的個人化建議，和其 Microsoft 帳戶的相關通知。|
|*資料收集與預覽組建|允許遙測|0 – 安全性 [僅限企業版]|**Enabled** (Setting a value of 0 applies to devices running Enterprise, Education, IoT, or Windows Server editions only, and reduces telemetry sent to the most basic level supported)|
|資料收集與預覽版|Configure collection of browsing data for Desktop Analytics|Configure telemetry collection: Do not allow sending intranet or internet history|**Enabled** (You can configure Microsoft Edge to send intranet history only, internet history only, or both to Desktop Analytics for enterprise devices with a configured Commercial ID. If disabled or not configured, Microsoft Edge does not send browsing history data to Desktop Analytics.)|
|*資料收集與預覽組建|不顯示意見反應通知| N/A |**Enabled** (This policy setting allows an organization to prevent its devices from showing feedback questions from Microsoft.)|
|傳遞最佳化|下載模式|下載模式：簡單 (99)|99 = 沒有對等傳輸的簡單下載模式。 傳遞最佳化只會使用 HTTP 進行下載，且不會嘗試連線至傳遞最佳化雲端服務。|
|桌面視窗管理員|不允許視窗動畫| N/A |**Enabled** (This policy setting controls the appearance of window animations such as those found when restoring, minimizing, and maximizing windows. With this policy setting enabled, window animations are turned off.)|
|桌面視窗管理員|使用純色做為開始背景| N/A |**Enabled** ((This policy setting controls the Start background visuals. With this policy setting enabled, the Start background will use a solid color.)|
|邊緣 UI|允許邊緣撥動| N/A |**Disabled** (If you disable this policy setting, users will not be able to invoke any system UI by swiping in from any screen edge.)|
|邊緣 UI|停用說明秘訣| N/A |**Enabled** (If this setting is enabled, Windows will not show any help tips to the user.)|
|檔案總管|不顯示已安裝新應用程式通知| N/A |**Enabled** (This policy removes the end-user notification for new application associations. These associations are based on file types (for example, TXT files) or protocols (for example, HTTP). If this policy is enabled, no notifications will be shown to the end-user)|
|檔案歷程記錄|Turn off File History| N/A |**Enabled** (With this policy setting enabled, File History cannot be activated to create regular, automatic backups.)|
|*尋找我的裝置|開啟/關閉尋找我的裝置| N/A |已停用 (當 [尋找我的裝置] 關閉時，將不會註冊裝置及其位置，且 [尋找我的裝置] 功能將無法運作。 使用者也無法在其裝置上檢視其作用中的數位板最後一次使用的位置。)|
|家用群組|防止電腦加入家用群組| N/A |**Enabled** (If you enable this policy setting, users cannot add computers to a homegroup. This policy setting does not affect other network sharing features.)|
|*Internet Explorer|當使用者在網址列輸入時，允許 Microsoft 服務提供加強式建議| N/A |已停用 (當使用者在網址列輸入時將不會收到加強式建議。 此外，使用者將無法變更 [建議] 設定。|
|Internet Explorer|停用定期檢查以進行 Internet Explorer 軟體更新| N/A |**Enabled** (Prevents Internet Explorer from checking whether a new version of the browser is available.)|
|Internet Explorer|停用顯示啟動顯示畫面的功能| N/A |**Enabled** (Prevents the Internet Explorer splash screen from appearing when users start the browser.)|
|Internet Explorer|自動安裝新版 Internet Explorer| N/A |**Disabled** (This policy setting configures Internet Explorer to automatically install new versions of Internet Explorer when they are available.)|
|Internet Explorer|防止參與客戶經驗改進計畫| N/A |**Enabled** (This policy setting prevents the user from participating in the Customer Experience Improvement Program (CEIP).)|
|Internet Explorer|防止執行 [首次執行] 精靈|直接前往首頁|**Enabled** (This policy setting prevents Internet Explorer from running the First Run wizard the first time a user starts the browser after installing Internet Explorer or Windows.)|
|Internet Explorer|設定索引標籤程序成長速率|低|**Enabled** (This policy setting allows you to set the rate at which Internet Explorer creates new tab processes.)|
|Internet Explorer|關閉附加元件效能通知| N/A |**Enabled** (This policy setting prevents Internet Explorer from displaying a notification when the average time to load all the user's enabled add-ons exceeds the threshold.)|
|Internet Explorer|Turn off Automatic Crash Recovery| N/A |**Enabled** (This policy setting turns off Automatic Crash Recovery. With this policy setting enabled, Automatic Crash Recovery does not prompt the user to recover his or her data after a program stops responding.)|
|*Internet Explorer|關閉瀏覽器地理位置| N/A |**Enabled** (With this policy setting enabled, browser geolocation support is turned off)|
|*Internet Explorer|開啟建議的網站| N/A |如果停用此原則設定，就會關閉與此功能相關聯的進入點和功能。|
|*Internet Explorer  網際網路控制台  進階頁面|播放網頁動畫| N/A |**Disabled** (This policy setting allows you to manage whether Internet Explorer will display animated pictures found in Web content. Generally only animated GIF files are affected by this setting; active Web content such as java applets are not.)|
|*Internet Explorer  網際網路控制台  進階頁面|播放網頁音效| N/A |**Disabled** (With this policy setting disabled, Internet Explorer will not play or download sounds in Web content, helping pages display more quickly.)|
|*Internet Explorer  網際網路控制台  進階頁面|播放網頁影片| N/A |**Disabled** (If you disable this policy setting, Internet Explorer will not play or download videos, helping pages display more quickly.)|
|*Internet Explorer  網際網路控制台  進階頁面|關閉在背景載入網站和內容以最佳化效能| N/A |**Enabled** (With this policy setting enabled, IE doesn’t load any websites or content in the background.)|
|*Internet Explorer  網際網路控制台  進階頁面|關閉快速翻頁和頁面預測功能| N/A |已啟用 (Microsoft 會收集您的瀏覽歷程記錄來改善快速翻頁與頁面預測的運作方式。 如果啟用這個原則設定，則會關閉快速翻頁與頁面預測，而且背景中不會載入下一個網頁。)|
|Internet Explorer 網際網路設定 進階設定 瀏覽|關閉電話號碼偵測| N/A |**Enabled** (This policy setting determines whether phone numbers are recognized and turned into hyperlinks, which can be used to invoke the default phone application on the system. 如果停用這個原則設定，將會啟用電話號碼偵測。 使用者將無法修改此設定。|
|Internet Information Services|Prevent IIS installation| N/A |**Enabled** (With this policy setting enabled, IIS cannot be installed, and you will not be able to install Windows components or applications that require IIS.)|
|*定位和感應器|關閉定位| N/A |如果啟用此原則設定，將會關閉定位功能，且這部電腦上的所有程式都無法使用定位功能的位置資訊。|
|定位和感應器|關閉感應器| N/A |**Enabled** (This policy setting turns off the sensor feature for this device. With this policy setting enabled, the sensor feature is turned off, and all programs on this computer cannot use the sensor feature.)|
|定位和感應器 / Windows 定位提供者|關閉 Windows 定位提供者| N/A |**Enabled** (This policy setting turns off the Windows Location Provider feature for this device.)|
|*地圖|關閉自動下載與更新地圖資料| N/A |已啟用 (如果啟用此設定，將會關閉地圖資料的自動下載和更新。)|
|*地圖|關閉 [離線地圖] 設定頁面上非要求的網路流量| N/A |已啟用 (如果啟用此原則設定，將會關閉 [離線地圖] 設定頁面上產生網路流量的功能。 注意：這可能會關閉整個設定頁面。|
|*訊息中心|允許訊息服務雲端同步| N/A |已停用 (此原則設定可讓您將行動電話簡訊備份和還原至 Microsoft 的雲端服務。)|
|*Microsoft Edge|允許書籍媒體櫃的設定更新| N/A |**Disabled** (With this setting disabled, Microsoft Edge won't automatically download updated configuration data for the Books Library.)|
|*Microsoft Edge|Allow extended telemetry for the Books tab| N/A |**Disabled** (With this setting disabled, Microsoft Edge only sends basic telemetry data, depending on your device configuration.)|
|Microsoft Edge|Allow Microsoft Edge to pre-launch at Windows startup, when the system is idle, and each time Microsoft Edge is closed|Configure pre-launch: Prevent pre-launching|**Enabled** (With this setting enabled and configured to prevent pre-launch, Microsoft Edge won’t pre-launch during Windows sign in, when the system is idle, or each time Microsoft Edge is closed.)|
|Microsoft Edge|Allow Microsoft Edge to start and load the Start and New Tab page at Windows startup and each time Microsoft Edge is closed|Configure tab preloading: Prevent tab-preloading|**Enabled** (This policy setting lets you decide whether Microsoft Edge can load the Start and New Tab page during Windows sign in and each time Microsoft Edge is closed. By default this setting is to allow preloading. With preloading disabled, Microsoft Edge won’t load the Start or New Tab page during Windows sign in and each time Microsoft Edge is closed.)|
|Microsoft Edge|允許在新索引標籤上顯示網頁內容| N/A |**Disabled** (With this setting disabled, Edge opens a new tab with a blank page. If this setting is configured, users cannot change the setting.)|
|*Microsoft Edge|防止 Microsoft Edge 開啟 [首次執行] 網頁| N/A |**已啟用** (使用者在初次開啟 Microsoft Edge 時不會看見 [首次執行] 頁面。)|
|OneDrive|防止 OneDrive 產生網路流量直到使用者登入 OneDrive| N/A |**已啟用** (啟用此設定可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。)|
|Online Assistance|Turn off Active Help| N/A |**Enabled** (With this policy setting enabled, active content links are not rendered. The text is displayed, but there are no clickable links for these elements.)|
|OOBE|Don’t launch privacy settings experience on user logon| N/A |**Enabled** (When logging into a new user account for the first time or after an upgrade in some scenarios, that user may be presented with a screen or series of screens that prompts the user to choose privacy settings for their account. Enable this policy to prevent this experience from launching.)|
|RSS 摘要|防止自動探索摘要和網頁快訊| N/A |**Enabled** (This policy setting prevents users from having Internet Explorer automatically discover whether a feed or Web Slice is available for an associated webpage.)|
|*RSS 摘要|關閉摘要和網頁快訊的背景同步處理| N/A |**已啟用** (如果啟用此原則設定，將會停用在背景中同步處理摘要和網頁快訊的功能。)|
|*搜尋|允許 Cortana| N/A |**Disabled** (This policy setting specifies whether Cortana is allowed on the device. 當 Cortana 已關閉時，使用者仍可使用搜尋功能尋找裝置上的項目。|
|搜尋|允許在鎖定畫面上使用 Cortana| N/A |**Disabled** (This policy setting determines whether or not the user can interact with Cortana using speech while the system is locked.)|
|*搜尋|允許搜尋和 Cortana 使用定位| N/A |**Disabled** (This policy setting specifies whether search and Cortana can provide location aware search and Cortana results.)|
|搜尋|Control rich previews for attachments|Control Rich Previews for Attachments:**.docx;.xlsx;.txt;.xls**|**Enabled** (Enabling this policy defines a semicolon-delimited list of file extensions which will be allowed to have rich attachment previews.)<p>**NOTE**: This setting can be used to limit what types of attachments are previewed, which can also help prevent automatically previewing some potentially dangerous contents types.|
|搜尋|不允許 Web 搜尋| N/A |**Enabled** (Enabling this policy removes the option of searching the Web from Windows Desktop Search.)|
|*搜尋|不要在 [搜尋] 中搜尋網路或顯示網路搜尋結果| N/A |**已啟用** (如果啟用此原則設定，則不會在 Web 上執行查詢，且在使用者使用 [搜尋] 執行查詢時將不會顯示 Web 結果。)|
|搜尋|Enable indexing uncached Exchange folders| N/A |**Disabled** (Enabling this policy allows indexing of mail items on a Microsoft Exchange server when Microsoft Outlook is not running in cached mode. The default behavior for search is to not index uncached Exchange folders. Disabling this policy will block any indexing of uncached Exchange folders.)|
|搜尋|防止對離線檔案快取中的檔案編製索引| N/A |**Enabled** (If enabled, files on network shares made available offline are not indexed. 否則會予以忽略。 預設為停用。|
|*搜尋|設定 [搜尋] 中可以分享的資訊|匿名資訊|共用使用資訊，但不共用搜尋歷程記錄、Microsoft 帳戶資訊或特定位置。|
|搜尋|Stop indexing in the event of limited hard drive space|MB Limit: **5000**|**Enabled** (Enabling this policy prevents indexing from continuing after less than the specified amount of hard drive space is left on the same drive as the index location. Select between 0 and 2147483647 MB.)|
|*軟體保護平台|Turn off KMS Client Online AVS Validation| N/A |**Enabled** (With this setting enabled, the device does not send data to Microsoft regarding its activation state)|
|*語音|允許自動更新語音資料| N/A |**Disabled** (Specifies whether the device will receive updates to the speech recognition and speech synthesis models.)|
|市集|關閉更新至最新版 Windows 的服務| N/A |**Enabled** (Enables or disables the Store offer to update to the latest version of Windows. If you enable this setting, the Store application will not offer updates to the latest version of Windows.)|
|文字輸入|改進筆跡及輸入辨識| N/A |**Disabled** (This policy setting controls the ability to send inking and typing data to Microsoft to improve the language recognition and suggestion capabilities of apps and services running on Windows.)|
|Windows 錯誤報告|停用 Windows 錯誤報告| N/A |**Enabled** (With this policy setting enabled, Windows Error Reporting does not send any problem information to Microsoft. Additionally, solution information is not available in Security and Maintenance in Control Panel.)|
|Windows 遊戲錄影和廣播|啟用或停用 Windows 遊戲錄影和廣播| N/A |**Disabled** (With this setting disabled, Windows Game Recording will not be allowed.)|
|Windows Ink 工作區|Windows Ink 工作區|請選擇下列其中一個動作：|**Enabled** (With this setting enabled and sub-setting set to disabled, Windows Ink Workspace functionality is unavailable.)|
|Windows Installer|控制基準檔案快取的大小上限|5|**Enabled** (This policy controls the percentage of disk space available to the Windows Installer baseline file cache. With this policy setting enabled you can modify the maximum size of the Windows Installer baseline file cache.)|
|Windows Installer|關閉系統還原檢查點建立功能| N/A |**Enabled** (With this policy setting enabled, the Windows Installer does not generate System Restore checkpoints when installing applications.)|
|Windows 行動中心|關閉 Windows 行動中心| N/A |**Enabled** (With this policy setting enabled, the user is unable to invoke Windows Mobility Center. The Windows Mobility Center UI is removed from all shell entry points and the .exe file does not launch it.)|
|Windows 可靠性分析|設定可靠性 WMI 提供者| N/A |**Disabled** (With this policy setting disabled, Reliability Monitor will not display system reliability information, and WMI-capable applications will be unable to access reliability information from the listed providers.)|
|Windows Security \ Notifications|Hide non-critical notifications| N/A |**Enabled** (With this setting enabled, local users will only see critical notifications from Windows Security. They will not see other types of notifications, such as regular PC or device health information.)|
|Windows Update|開啟軟體通知| N/A |**Disabled** (This policy setting allows you to control whether users see detailed enhanced notification messages about featured software from the Microsoft Update service. 增強通知訊息會傳達選用軟體的價值，並鼓勵使用者安裝及使用。 此原則設定適用於管理不嚴格、且允許使用者存取 Microsoft Update 服務的環境中。|
|*Windows Update  商務用 Windows Update|管理預覽版|Set the behavior for receiving preview builds: **Disable preview builds**|已啟用 (選取 [停用預覽版]  會防止在裝置上安裝預覽版。 這將導致使用者無法透過 [設定] -> [更新與安全性] 選擇加入 Windows 測試人員計畫。|
|*Windows Update  商務用 Windows Update|選取接收預覽版和功能更新的時機|Select the Windows readiness level for the updates you want to receive:<p>**半年通道**<p>After a Preview Build or Feature Update is released, defer receiving it for this many days: **365**<p>Pause Preview Builds or Feature Updates starting: **yyyy-mm-dd**|**已啟用** (啟用此原則可指定要接收的預覽版或功能更新的層級和接收時機。) Semi-Annual Channel: Receive feature updates when they are released to the general public.<p> When Selecting Semi-Annual Channel:<p>您可以將接收這些功能更新的時間自其發行日起最多延遲 365 天。<p>- To prevent Feature Updates from being received on their scheduled time, you can temporarily pause them. The pause will remain in effect for 35 days from the start time provided.<p> - To resume receiving Feature Updates which are paused, clear the start date field.)|
|Windows Update  商務用 Windows Update|選取接收品質更新的時機|將 [在功能更新發行之後，延遲下列天數而不接收]  調整方塊設定為 [180 天]  。<p>從 yyyy-mm-dd 開始暫停品質更新|**Enabled** (Enable this policy to specify when to receive quality updates.<p>您可以將接收這些品質更新的時間自其發行日起最多延遲 30 天。<p>To prevent quality updates from being received on their scheduled time, you can temporarily pause quality updates. The pause will remain in effect for 35 days or until you clear the start date field.<p>To resume receiving Quality Updates which are paused, clear the start date field.)<p>This recommendation is to help control when updates are applied, and to ensure updates don’t get offered and installed unexpectedly|
|本機電腦原則  使用者設定  系統管理範本| N/A | N/A | N/A |
|控制台  地區及語言選項|關閉在我輸入時提供文字預測| N/A |**Enabled** (This policy turns off the offer text predictions as I type option. This does not, however, prevent the user or an application from changing the setting programmatically. With this policy setting enabled, the option will be locked to not offer text predictions.)|
|[桌上型電腦]|不要將最近開啟的文件共用新增到 [網路位置] 中| N/A |**Enabled** (With this setting enabled, shared folders are not added to Network Locations automatically when you open a document in the shared folder.)|
|[桌上型電腦]|關閉 Aero 搖動視窗最小化滑鼠手勢| N/A |**Enabled** (Prevents windows from being minimized or restored when the active window is shaken back and forth with the mouse. With this policy enabled, application windows will not be minimized or restored when the active window is shaken back and forth with the mouse.)|
|桌面 / Active Directory|Active Directory 搜尋的大小上限|Number of objects returned:**1500**|**Enabled** (Specifies the maximum number of objects the system displays in response to a command to browse or search Active Directory. This setting affects all browse displays associated with Active Directory, such as those in Local Users and Groups, Active Directory Users and Computers, and dialog boxes used to set permissions for user or group objects in Active Directory.)|
|開始功能表和工作列|不顯示或追蹤捷徑清單中來自遠端位置的項目| N/A |**Enabled** (This policy setting allows you to control displaying or tracking items in Jump Lists from remote locations.)|
|開始功能表和工作列|Do not search Internet| N/A |**Enabled** (With this policy setting enabled, the Start Menu search box will not search for internet history or favorites.)|
|開始功能表和工作列|不要用以搜尋為主的方法來解析殼層捷徑| N/A |**Enabled** (This policy setting prevents the system from conducting a comprehensive search of the target drive to resolve a shortcut.)|
|開始功能表和工作列|關閉所有球形通知| N/A |**Enabled** (With this policy setting enabled, no notification balloons are shown to the user.)
|開始功能表和工作列|關閉功能通告提示氣球通知| N/A |**Enabled** (With this policy setting enabled, certain notification balloons that are marked as feature advertisements are not shown.)|
|開始功能表和工作列|關閉使用者追蹤| N/A |**Enabled** (With this policy setting enabled, the system does not track the programs that the user runs and does not display frequently used programs in the Start Menu.)|
|開始功能表和工作列 / 通知|關閉快顯通知| N/A |**Enabled** (With this policy setting enabled, applications will not be able to raise toast notifications.)|
|開始功能表和工作列 / 通知|Turn off toast notifications on the lock screen| N/A |**Enabled** (With this policy setting enabled, applications will not be able to raise toast notifications on the lock screen.)|
|本機電腦原則  使用者設定  系統管理範本| N/A | N/A | N/A |
|Windows 元件 / 雲端內容|Configure Windows spotlight on lock screen| N/A |**Disabled** (With this policy disabled, Windows spotlight will be turned off and users will no longer be able to select it as their lock screen. Users will see the default lock screen image and will be able to select another image, unless you have enabled the "Prevent changing lock screen image" policy.)|
|Windows 元件 / 雲端內容|Do not suggest third-party content in Windows spotlight| N/A |**Enabled** (With this policy enabled, Windows spotlight features like lock screen spotlight, suggested apps in Start menu or Windows tips will no longer suggest apps and content from third-party software publishers. Users may still see suggestions and tips to make them more productive with Microsoft features and apps.)|
|Windows 元件 / 雲端內容|不使用獨特體驗的診斷資料| N/A |**Enabled** (With this policy setting enabled, Windows will not use diagnostic data from this device (this data may include browser, app and feature usage, depending on the "diagnostic data" setting value) to customize content shown on lock screen, Windows tips, Microsoft consumer features and other related features.)|
|Windows 元件 / 雲端內容|關閉所有 Windows 焦點功能| N/A |**Enabled** (Windows spotlight on lock screen, Windows tips, Microsoft consumer features and other related features will be turned off. You should enable this policy setting if your goal is to minimize network traffic from target devices.)|
|邊緣 UI|關閉追蹤應用程式使用量| N/A |**Enabled** (This policy setting prevents Windows from keeping track of the apps that are used and searched most frequently. If you enable this policy setting, apps will be sorted alphabetically in:<p> - search results<p> - the Search and Share panes<p> - the drop-down app list in the Picker)|
|檔案總管|關閉縮圖快取處理| N/A |**Enabled** (With this policy setting enabled, thumbnail views are not cached.)|
|檔案總管|Turn off common control and window animations| N/A |**Enabled** (Disabling animations can improve usability for users with some visual disabilities as well as improving performance and battery life in some scenarios.)|
|檔案總管|關閉在 [檔案總管] 搜尋方塊中顯示最近搜尋項目| N/A |**Enabled** (Disables suggesting recent queries for the Search Box and prevents entries into the Search Box from being stored in the registry for future references.)|
|檔案總管|關閉在隱藏的 thumbs.db 檔案中快取縮圖| N/A |**Enabled** (With this policy setting enabled, File Explorer does not create, read from, or write to thumbs.db files.)|

Windows 受限流量有限功能基準指引中的其他設定。

### <a name="system-services"></a>系統服務

如果您考慮停用系統服務以節省資源，請務必仔細確認該服務在某方面是否為其他服務的元件之一。 In this paper and with the available GitHub scripts, some services are not in the list because they cannot be disabled in a supported manner.

這些建議大多與[在包含桌面體驗的 Windows Server 2016 中停用的系統服務指引](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md)中的 Windows Server 2016 桌面體驗方面的建議相對應

許多可能適合停用的服務都設定為手動服務啟動類型。 這表示服務不會自動啟動，且除非有程序或服務對您考慮要停用的服務觸發了要求，才會啟動。 已設定為手動啟動類型的服務通常不會列於此處。

> [!NOTE]
> 您可以使用此 PowerShell 範例程式碼來列舉執行中的服務，只輸出服務的簡短名稱：
>
> ```powershell
> Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object -ExpandProperty Name
> ```

The following table contains some services that may be considered to disable in virtual desktop environments:

| Windows 服務 | 服務名稱 | 項目 | 註解|
| --------------- | ------------ | ---- | ------ |
|Cellular Time|autotimesvc|This service sets time based on NITZ messages from a Mobile Network|Virtual desktop environments may not have such devices available. <p>若要深入了解支援向量機器，請參閱[本文](/windows-hardware/drivers/network/mb-nitz-support)。 |
|GameDVR 和廣播使用者服務|BcastDVRUserService|This (per-user) service is used for Game Recordings and Live Broadcasts|注意：這是每一使用者服務，因此範本服務必須停用。 這項使用者服務用於遊戲錄影和即時廣播<p>若要深入了解支援向量機器，請參閱[本文](/windows-hardware/drivers/network/mb-nitz-support)。 |
|CaptureService|CaptureService|Enables optional screen capture functionality for applications that call the Windows.Graphics.Capture API.|OneCore capture service: enables optional screen capture functionality for applications that call the Windows.Graphics.Capture API<p> 如需詳細資訊，請參閱[這篇文章](/uwp/api/windows.graphics.capture?view=winrt-19041&preserve-view=true)。|
|已連線的裝置平台服務|CDPSvc|此使用者服務適用於連線的裝置平台案例|已連線的裝置平台服務 <p> 若要深入了解支援向量機器，請參閱[本文](/openspecs/windows_protocols/ms-cdp/929c2238-6d49-4ba4-a36a-37e732c4f736)。|
|CDP User Service|CDPUserSvc| N/A |已連線的裝置平台服務 若要深入了解支援向量機器，請參閱[本文](/openspecs/windows_protocols/ms-cdp/f5a15c56-ac3a-48f9-8c51-07b2eadbe9b4)。<p> 此使用者服務適用於連線的裝置平台案例 <br><br>這是個別使用者的服務，因此範本服務必須停用。|
|最佳化磁碟機|defragsvc|將儲存磁碟機上的檔案最佳化，有助於提高電腦執行效率。|VDI 解決方案通常不會受益於磁碟最佳化。 The "drives" are often not traditional drives and often just a temporary storage allocation.|
|Diagnostic Execution Service|DiagSvc|Executes diagnostic actions for troubleshooting support|Disabling this service disables the ability to run Windows diagnostics Diagnostic Execution Service.|
|已連線使用者體驗與遙測|DiagTrack|已連線使用者體驗與遙測服務可啟用支援應用程式內與已連線使用者體驗的功能。 此外，在 [意見反應與診斷] 下啟用診斷與使用方式隱私權選項設定時，此服務會管理診斷與使用方式資訊 (用於改進 Windows 平台的體驗與品質) 的事件導向收集與傳輸。|如果在中斷連線的網路上，請考慮停用。 若要深入了解支援向量機器，請參閱[本文](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)。|
|診斷原則服務|DPS|診斷原則服務能夠針對 Windows 元件進行問題偵測、疑難排解並提供解決方案。 如果停止此服務，診斷將不再運作。|Disabling this service disables the ability to run Windows diagnostics. 如需詳細資訊，請參閱[這篇文章](/uwp/api/Windows.System.Diagnostics?view=winrt-19041&preserve-view=true)。|
|裝置設定管理員|DsmSvc|啟用裝置相關軟體的偵測、下載及安裝。 |如果停用此服務，裝置可能會使用過期的軟體來設定且可能無法正確運作。 <p>Virtual desktop environments very closely control what software is installed and maintain that consistency across the environment.|
|Data Usage service|DusmSvc|Network data usage, data limit, restrict background data, metered networks.| 如需詳細資訊，請參閱[這篇文章](/uwp/schemas/mobilebroadbandschema/dusm/schema-root)。 |
|Windows 行動熱點服務|icssvc|提供與其他裝置共用行動數據連線的能力。|若要深入了解支援向量機器，請參閱[本文](/uwp/api/Windows.Networking.NetworkOperators.NetworkOperatorTetheringAccessPointConfiguration?view=winrt-19041&preserve-view=true)。|
|Microsoft Store 安裝服務|InstallService|提供 Microsoft Store 的基礎結構支援。 |此服務會依需求啟動，而且如果停用，則透過 Microsoft Store 取得的內容將無法正常運作。<p>Consider disabling this service on non-persistent virtual desktop, leave as-is for persistent virtual desktop solutions.|
|地理位置服務|lfsvc|Monitors the current location of the system and manages geofences (a geographical location with associated events). |如果關閉此服務，應用程式將無法用來或接收地理位置或地理柵欄的通知。 若要深入了解支援向量機器，請參閱[本文](/uwp/api/Windows.Devices.Geolocation?view=winrt-19041&preserve-view=true)。|
|已下載的地圖管理員|MapsBroker|適用於應用程式存取已下載地圖的 Windows 服務。 要存取已下載地圖的應用程式會依需求啟動此服務。|停用此服務將讓應用程式無法存取地圖。 若要深入了解支援向量機器，請參閱[本文](/uwp/api/Windows.Services.Maps?view=winrt-19041&preserve-view=true)。|
|MessagingService|MessagingService|支援文字訊息和相關功能的服務。|這是每一使用者服務，因此範本服務必須停用。|
|同步主機|OneSyncSvc|此服務會將郵件、連絡人、行事曆與各種其他使用者資料同步。 |若此服務未執行，相依於此功能的郵件與其他應用程式都將無法正常運作。 <p>這是每一使用者服務，因此範本服務必須停用。|
|連絡人資料|PimIndexMaintenanceSvc|為連絡人資料編製索引，以加快搜尋連絡人的速度。 如果您停止或停用此服務，則搜尋結果中可能會遺漏連絡人資訊。|這是每一使用者服務，因此範本服務必須停用。|
|電源|電源|管理電源原則與電源原則通知傳遞。|Virtual machines have virtually no influence on power properties. If this service is disabled, power management and reporting are not available. 若要深入了解支援向量機器，請參閱[本文](/windows-hardware/drivers/powermeter/user-mode-power-service)。|
|Payments and NFC/SE Manager|SEMgrSvc|Manages payments and Near Field Communication (NFC) based secure elements.|May not need this service for payments, in the enterprise environment.|
|Microsoft Windows SMS 路由器服務。|SmsRouter|Routes messages based on rules to appropriate clients.|May not need this service, if other tools are used for messaging, such as Teams, Skype, or other. 若要深入了解支援向量機器，請參閱[本文](/dotnet/framework/wcf/feature-details/routing-service)。|.
|Superfetch (SysMain)|SysMain|維護並改進一段時間後的系統效能。|Superfetch generally does not improve performance in virtual desktop environments for various reasons. The underlying storage is often virtualized and possibly striped across multiple drives. In some virtual desktop solutions the accumulated user state is discarded when the user logs off. The SysMain feature should be evaluated in each environment.|
|觸控式鍵盤與手寫面板服務|TabletInputService|啟用觸控式鍵盤和手寫面板及筆跡功能|Not needed unless there is an active touchscreen in use, or a handwriting input device.|
|更新 Windows Update 的 Orchestrator 服務|UsoSvc|管理 Windows 更新。 如果停止，您的裝置將無法下載並安裝最新的更新。|Virtual desktop devices are often carefully managed with respect to updates. Servicing is usually performed during maintenance windows. In some cases, an update client may be utilized, such as SCCM. The exception would be security signature updates, that would be applied at any time, to any virtual desktop device, so as to maintain up-to-date signatures. If you disable this service, test to ensure that security signatures are still able to be installed.|
|磁碟區陰影複製|VSS|管理及實作用於備份和其他用途的磁碟區陰影複製。 |如果停止此服務，陰影複製將無法用於備份，且備份可能會失敗。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。 若要深入了解支援向量機器，請參閱[本文](../../../WindowsServerDocs/storage/file-server/volume-shadow-copy-service.md)。|
|診斷系統裝載|WdiSystemHost|診斷原則服務會使用診斷系統裝載，來裝載需要在本機系統內容上執行的診斷。 如果停止此服務，則與其相依的任何診斷都將不再運作。|Disabling this service disables the ability to run Windows diagnostics
|Windows 錯誤報告|WerSvc|允許在程式停止運作或回應時報告錯誤，並允許傳遞現有的解決方案。 此外，也允許產生用於診斷與修復服務的記錄。 如果停止此服務，錯誤報告可能就無法正常運作，而且可能無法顯示診斷服務及修復的結果。|With virtual desktop environments, diagnostics are often performed in an "offline" scenario, and not in mainstream production. In addition, some customers disable WER anyway. WER 會針對許多不同的狀況產生少量資源，包括無法安裝裝置或無法安裝更新等狀況。 若要深入了解支援向量機器，請參閱[本文](/windows/win32/wer/windows-error-reporting)。|
|Windows 搜尋|WSearch|提供檔案、電子郵件和其他內容的內容索引、屬性快取及搜尋結果。|Disabling this service prevents indexing of e-mail and other things. Test before disabling this service. 若要深入了解支援向量機器，請參閱[本文](/windows/win32/search/-search-3x-wds-overview#windows-search-service)。 |
|Xbox Live Auth Manager|XblAuthManager|提供驗證和授權服務以便與 Xbox Live 互動。 |如果停止此服務，某些應用程式可能無法正確運作。
|Xbox Live Game Save|XblGameSave|此服務會針對 Xbox Live 中已啟用儲存功能的遊戲同步處理儲存資料。 |如果停止此服務，遊戲儲存資料將不會上傳至 Xbox Live 或從中下載。
|Xbox Accessory Management Service|XboxGipSvc|This service manages connected Xbox Accessories.| N/A |
|Xbox Live Networking Service|XboxNetApiSvc|This service supports the Windows.Networking.XboxLive application programming interface.| N/A |

#### <a name="per-user-services-in-windows"></a>Windows 中的每一使用者服務

每一使用者服務是會在使用者登入 Windows 或 Windows Server 時建立，並且在該使用者登出時停止並刪除的服務。這些服務執行於使用者帳戶的安全性內容中 - 其資源管理效能優於先前的方法所能提供的，因為過去須在 [總管] 中使用預先設定的帳戶或以工作的形式執行這類服務。 For more information, see [Per-user services in Windows](/windows/application-management/per-user-services-in-windows).

如果您想要變更服務起始值，偏好的方法是開啟已提升權限的 cmd.exe 命令提示字元，並執行服務控制管理員工具 ‘Sc.exe’。 如需詳細資訊，請參閱[](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11))。

### <a name="scheduled-tasks"></a>排定的工作

如同 Windows 中其他項目，在您考慮停用某個項目前，請先確定您不需要該項目。 Some tasks in virtual desktop environments, such as **StartComponentCleanup**, may not be desirable to run in production, but may be good to run during a maintenance window on the "gold image" (reference image).

The following list of tasks includes tasks that perform optimizations or data collections on computers that maintain their state across reboots. When a virtual desktop device reboots and discards all changes since last boot, optimizations intended for physical computers are not helpful.

您可以使用下列 PowerShell 程式碼，取得目前所有排定的工作 (包括描述)：

```powershell
  Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

> [!NOTE]
> There are several tasks that can't be disabled with a script, even when run on an elevated command prompt. The recommendations here, and in the GitHub scripts do not attempt to disable tasks that cannot be disabled with a script.

|排定的工作名稱：|描述|
|-------------------|-----------|
|MNO|Mobile broadband account experience metadata parser|
|AnalyzeSystem|This task analyzes the system looking for conditions that may cause high energy use|
|行動電話通訊|Related to cellular devices|
|相容性|Collects program telemetry information if opted-in to the Microsoft Customer Experience Improvement Program.|
|資訊彙集器|If the user has consented to participate in the Windows Customer Experience Improvement Program, this job collects and sends usage data to Microsoft|
|診斷|(DiskFootprint in task path) 'DiskFootprint' is the combined contribution of all processes that issue storage I/O in the form of storage reads, writes, and flushes.|
|FamilySafetyMonitor|Initializes Family Safety monitoring and enforcement.|
|FamilySafetyRefreshTask|Synchronizes the latest settings with the Microsoft family features service.|
|MapsToastTask|This task shows various Map-related toasts|
|Microsoft-Windows-DiskDiagnosticDataCollector|The Windows Disk Diagnostic reports general disk and system information to Microsoft for users participating in the Customer Experience Program.|
|NotificationTask|Background task for performing per user and web interactions|
|ProcessMemoryDiagnosticEvents|Schedules a memory diagnostic in response to system events|
|Proxy|This task collects and uploads autochk SQM data if opted-in to the Microsoft Customer Experience Improvement Program.|
|QueueReporting|Windows Error Reporting task to process queued reports.|
|RecommendedTroubleshootingScanner|Check for recommended troubleshooting from Microsoft|
|RegIdleBackup|Registry Idle Backup Task|
|RunFullMemoryDiagnostic|Detects and mitigates problems in physical memory (RAM).|
|排程|The Windows Scheduled Maintenance Task performs periodic maintenance of the computer system by fixing problems automatically or reporting them through Security and Maintenance.|
|ScheduledDefrag|This task optimizes local storage drives.|
|SilentCleanup|Maintenance task used by the system to launch a silent auto disk cleanup when running low on free disk space.|
|SpeechModelDownloadTask|
|Sqm-Tasks|This task gathers information about the Trusted Platform Module (TPM), Secure Boot, and Measured Boot.|
|SR|This task creates regular system protection points.|
|StartComponentCleanup|Servicing task that may be better performed during maintenance windows|
|StartupAppTask|Scans startup entries and raises notification to the user if there are too many startup entries.|
|SyspartRepair|
|WindowsActionDialog|Location Notification|
|WinSAT|Measures a system's performance and capabilities|
|XblGameSaveTask|Xbox Live GameSave standby task|

### <a name="apply-windows-and-other-updates"></a>套用 Windows (和其他) 更新

無論是從 Microsoft Update，還是從您的內部資源，都可套用可用的更新，包括 Windows Defender 特徵碼。 這是套用其他可用更新的好時機，包括 Microsoft Office 的更新 (如有安裝)，以及其他軟體更新。 如果 PowerShell 會保留在影像中，您可以執行命令 `Update-Help` 下載 PowerShell 的最新可用說明。

#### <a name="servicing-os-and-apps"></a>Servicing OS and apps

在映像最佳化程序中的某個時間點，應該會套用可用的 Windows 更新。 Windows 10 更新設定中有一個可提供其他更新的設定： You can find it at **Settings** > **Advanced options**. Once there, set **Give me updates for other uMirosoft products when I update Windows** to **On**.

![A screenshot of the advanced options menu. The "Give me updates for other Microsoft products" setting is turned on.](media/rds-vdi-recommendations-1909/servicing.png)

如果您要將 Microsoft Office 之類的 Microsoft 應用程式安裝到基底映像，這會是較理想的設定。 如此，當映像開始運作時，Office 就會處於最新狀態。 此外還有 .NET 更新，以及某些可透過 Windows Update 取得更新的第三方元件 (例如 Adobe)。

One very important consideration for non-persistent virtual desktop devices is security updates, including security software definition files. 這些更新可能會以一天一次甚或更長的間隔發行。

就 Windows Defender 而言，建議即使在非持續性 VDI 中，也應允許更新執行。 The updates are going to apply nearly every time you sign in, but the updates are small and should not be a problem. 此外，由於只會套用最新的可用更新，VM 的更新將不會過時。 第三方定義檔可能也是如此。

>[!NOTE]
> 市集應用程式 (UWP 應用程式) 會透過 Windows 市集進行更新。 新版的 Office (例如 Microsoft 365) 在直接連線至網際網路時會透過其本身的機制進行更新，而在未連線至網際網路時，則會透過管理技術進行更新。

#### <a name="windows-system-startup-event-traces-autologgers"></a>Windows 系統啟動事件追蹤

Windows is configured by default to collect and save diagnostic data. 其目的是要啟用診斷，或是記錄資料以便在需要進一步疑難排解時使用。 您可以在下圖所示的位置找到自動系統追蹤：

![A screenshot of the computer management folder. The System Traces folder is open and the WiFi Session file is selected.](media/rds-vdi-recommendations-1909/system-traces.png)

顯示於 [事件追蹤工作階段]  和 [啟動事件追蹤工作階段]  下方的部分追蹤無法停止，而且也不應停止。 其他追蹤 (例如 ‘WiFiSession’) 則可以停止。 若要停止執行中的追蹤，請在 [事件追蹤工作階段]  下方，以滑鼠右鍵按一下追蹤，然後選取 [停止]  。 若要防止在系統啟動時自動啟動追蹤，請遵循下列步驟：

1. 選取 [啟動事件追蹤工作階段]  資料夾

2. Find and select the trace file you want to look at to open it.

3. 選取 [追蹤工作階段]  索引標籤。

4. 清除標示為 [已啟用]  的方塊。

5. 選取 [確定]  。

The following table lists some system traces that you should consider disabling in your virtual desktop environments:

|名稱|註解|
|---|--------|
|CellCore|[https://docs.microsoft.com/windows-hardware/drivers/network/cellular-architecture-and-driver-model](/windows-hardware/drivers/network/cellular-architecture-and-driver-model)|
|CloudExperienceHostOOBE|Documented [here](/windows/security/identity-protection/hello-for-business/hello-how-it-works-technology#cloud-experience-host).|
|DiagLog|A log generated by the Diagnostic Policy Service, which is documented [here](/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server)|
|RadioMgr|Documented [here](/windows-hardware/drivers/nfc/what-s-new-in-nfc-device-drivers)|
|ReadyBoot|文件在此 \(英文\)。|
|WDIContextLog|Wireless Local Area Network (WLAN) Device Driver Interface, and is documented here. |
|WiFiDriverIHVSession|Documented [here](/windows-hardware/drivers/network/user-initiated-feedback-normal-mode).|
|WiFiSession|Diagnostic log for WLAN technology. If Wi-Fi isn't implemented, there's no need for this logger|
|WinPhoneCritical|Diagnostic log for phone (Windows?). If not using phones, no need for this logger|

#### <a name="windows-defender-optimization-in-the-virtual-desktop-environment"></a>Windows Defender optimization in the virtual desktop environment

For greater details about how to optimize Windows Defender in a virtual desktop environment, check out the [Deployment guide for Windows Defender Antivirus in a virtual desktop infrastructure (VDI) environment](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus).

上述文章提供維護黃金 VDI 映像的程序，並說明如何維護執行中的 VDI 用戶端。 To reduce network bandwidth when virtual desktop devices need to update their Windows Defender signatures, stagger reboots, and schedule reboots during off hours where possible. Windows Defender 特徵碼的更新可包含在內部檔案共用上，且如果可行，請將這些檔案共用放在與 VDI 虛擬機器相同或接近的網路區段上。

#### <a name="client-network-performance-tuning-by-registry-settings"></a>依登錄設定進行的用戶端網路效能微調

有些登錄設定可能會提高網路效能。 在 VDI 或電腦的工作負載多半以網路為基礎的環境中，這項調整格外重要。 建議您套用此節中的設定，為目錄項目之類的項目設定額外的緩衝處理和快取，使效能側重於網路的層面。

> [!NOTE]
> 本節中的某些設定僅以登錄為基礎，且應在映像部署至生產環境之前納入基底映像中。

The following settings are documented in the [Performance tuning guidelines for Windows Server 2016](../../administration/performance-tuning/index.md).

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

適用於 Windows 10。 預設值是 **0**。 根據預設，在某些情況下，SMB 重新導向器會對高延遲網路連線的輸送量進行節流，以避免網路相關逾時。 若將此登錄值設為 1 則會停用此節流，進而透過高延遲網路連接來達到更高的檔案轉送輸送量。 請考慮將此值設定為 **1**。

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileInfoCacheEntriesMax`

適用於 Windows 10。 預設值是 **64**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案中繼資料數量。 提高此值可降低網路流量，並且在存取許多檔案時提升效能。 請嘗試將此值增加到 **1024**。

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DirectoryCacheEntriesMax`

適用於 Windows 10。 預設值是 **16**，有效範圍是 1 至 4096。 此值用來決定用戶端可以快取的目錄資訊數量。 提高此值可降低網路流量，並且在存取大量目錄時提升效能。 請考慮將此值增加到 **1024**。

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\FileNotFoundCacheEntriesMax`

適用於 Windows 10。 預設值是 **128**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案名稱資訊數量。 提高此值可降低網路流量，並且在存取許多檔案名稱時提升效能。 請考慮將此值增加到 **2048**。

#### <a name="dormantfilelimit"></a>DormantFileLimit

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DormantFileLimit`

適用於 Windows 10。 預設值是 **1023**。 此參數可指定在應用程式關閉檔案之後，共用資源上應該保持開啟的檔案數目上限。 有數千個用戶端要連線至 SMB 伺服器時，請考慮將此值縮減至 **256**。

您可以使用 [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration?view=win10-ps&preserve-view=true) 和 [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration?view=win10-ps&preserve-view=true) Windows PowerShell Cmdlet 來設定其中許多 SMB 設定。 您也可以使用 Windows PowerShell 來設定僅限登錄的設定，如下列範例所示：

```powershell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecuritySignature -Value 0 -Force
```

#### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Windows 受限流量有限功能基準指引中的其他設定

Microsoft 已發行使用與 [Windows 安全性基準](/windows/device-security/windows-security-baselines)相同的程序建立的基準，適用於未直接連線至網際網路的環境，或不想將太多資料傳送至 Microsoft 和其他服務的環境。

[Windows 受限流量有限功能基準](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services)設定在群組原則表格中會以星號標示。

#### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>磁碟清理 (包括使用磁碟清理精靈)

磁碟清理對於黃金/主要映像 VDI 實作可能特別有幫助。 在映像備妥、更新並完成設定後，執行磁碟清理是最後應執行的工作之一。 The optimization scripts on Github.com have PowerShell code to perform common disk cleanup tasks

> [!NOTE]
> Disk cleanup settings and are in the Settings category "System" called "Storage." By default, Storage Sense runs when a low disk free space threshold is reached.
>
> To learn more about how to use Storage Sense with Azure custom VHD images, see [Prepare and customize a master VHD image](/azure/virtual-desktop/set-up-customize-master-image).
>
> For Windows Virtual Desktop session host that use Windows 10 Enterprise or Windows 10 Enterprise multi-session, we recommend disabling Storage Sense. You can disable Storage Sense in the Settings menu under **Storage**.

以下是對針對各種磁碟清理工作提供的建議。 在實作之前，應該先測試這些項目：

1. Storage Sense may be utilized manually or automatically. For more information on Storage Sense, see this article: Use OneDrive and Storage Sense in Windows 10 to manage disk space
2. Manually cleanup temporary files and logs. 在提升權限的命令提示字元中，執行下列命令：

   1. `Del C:\*.tmp /s`
   2. `C:\*.etl /s`
   3. `C:\*.evtx /s`

   ```powershell
   Get-ChildItem -Path c:\ -Include *.tmp, *.dmp, *.etl, *.evtx, thumbcache*.db, *.log -File -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\Temp\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportArchive\* -Recurse -Force -ErrorAction SilentlyContinue

   Remove-Item -Path $env:ProgramData\Microsoft\Windows\WER\ReportQueue\* -Recurse -Force -ErrorAction SilentlyContinue

   Clear-RecycleBin -Force -ErrorAction SilentlyContinue

   Clear-BCCache -Force -ErrorAction SilentlyContinue
   ```

3. 使用執行 ，刪除系統上所有未使用的設定檔。

    `wmic path win32_UserProfile where LocalPath="C:\\users\\<users>" Delete`

For any questions or concerns about the information in this paper, contact your Microsoft account team, research the [Microsoft virtual desktop IT Pro blog](https://community.windows.com/stories/virtual-desktop-windows-10), post a message to [Microsoft Virtual Desktop forums](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/bd-p/WindowsVirtualDesktop), or contact [Microsoft](https://support.microsoft.com/contactus/) for questions or concerns.

### <a name="re-enable-windows-update"></a>Re-enable Windows Update

If you would like to enable the use of Windows Update after disabling it, as in the case of persistent virtual desktop, follow these steps:

1. 重新啟用這些群組原則設定：

   - Go to **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **System** > **Internet Communication Management** > **Internet Communication settings**.
     - Turn off access to all Windows Update features by changing the setting from **enabled** to **not configured**.
   - Go to **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows Update**.
     - Remove access to all Windows Update features by changing the setting from **enabled** to **not configured**.
     - Don't connect to any Windows Update Internet locations by changing the setting from **enabled** to **not configured**.
   - Go to **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows Update** > **Windows Update for Business**.
     - 選取收到品質更新時 (從已啟用變更為未設定)
   - Go to **Local Computer Policy** > **Computer Configuration** > **Administrative Templates** > **Windows Components** > **Windows Update** > **Windows Update for Business**.
     - 選取收到預覽組建和功能更新時 (從**已啟用**變更為**未設定**)

2. 重新啟用服務

   - 更新 Orchestrator 服務 (從 停用變更為自動 (延遲啟動) )。

3. Edit the Windows registry (warning, be careful when editing the registry).

   - 前往 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState`。
     - Change **DeferQualityUpdates** from '1' to '0'.
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings`
     - Delete any existing value for **PausedQualityDate** .
   - 移至 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU` 。
     - 將  設成 [停用]****。

4. 重新啟用排定的工作

   - Go to **Task Scheduler Library** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdates**.
   - Go to **Task Scheduler Library** > **Microsoft** > **Windows** > **InstallService** > **ScanForUpdatesAsUser**.

5. Restart your device to make all these settings take effect.

6. 如果不想讓此裝置提供功能更新，請移至設定  Windows Update  高級選項  選擇安裝更新的時間，然後手動設定選項，功能更新包含新功能和改進。如此可將此天數延後到某個非零的值，例如 180、365 等等。

## <a name="additional-information"></a>其他資訊

Learn more about Microsoft's VDI architecture at our [Windows Virtual Desktop documentation](https://azure.microsoft.com/services/virtual-desktop/).

If you need additional help with troubleshooting sysprep, check out [Sysprep fails after you remove or update Microsoft Store apps that include built-in Windows images](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu).
