---
title: 針對虛擬桌面基礎結構 (VDI) 角色最佳化 Windows 10 1803 版
description: 將當作 VDI 映像的 Windows 10 1803 桌面的額外負荷降至最低的建議設定和組態
ms.author: robsmi
ms.topic: article
author: jaimeo
ms.openlocfilehash: 38ddd48b6bf5502851615adeb75446f07bc860c1
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90767001"
---
# <a name="optimizing-windows-10-version-1803-for-a-virtual-desktop-infrastructure-vdi-role"></a>針對虛擬桌面基礎結構 (VDI) 角色最佳化 Windows 10 1803 版

本文可協助您針對 Windows 10 1803 版 (組建 17134) 選擇適當設定，以在在虛擬桌面基礎結構 (VDI) 環境中達到最佳的效能。 本指南中的所有設定皆為*供您考量的建議*，而並非需求。

在 VDI 環境中，將 Windows 10 效能最佳化的主要方式為盡可能減少應用程式圖形重新繪製、減少對 VDI 環境沒有顯著效益的背景活動，以及將整體執行的處理序減少至最低限度。 次要目標是要將基底映像中的磁碟空間使用量降至最低限度。 透過 VDI 實作，可能的最小基底 (或「黃金」映像大小) 可稍微減少 Hypervisor 的記憶體使用量，並且可小幅減少將桌面映像提供給取用者所需的整體網路作業。

> [!NOTE]
> 本文建議的設定可套用至 Windows 10 1803 版的其他安裝，包括實體或其他虛擬裝置上的安裝。 本主題中的建議並不會對 Windows 10 1803版的支援能力產生影響。

> [!TIP]
> 在本主題的討論中用來實作最佳化的指令碼，以及可使用 **LGPO.exe** 匯入的 GPO 匯出檔案，皆可從 GitHub 上的 [TheVDIGuys](https://github.com//TheVDIGuys) 取得。

## <a name="vdi-optimization-principles"></a>VDI 最佳化原則

VDI 環境可透過網路為電腦使用者提供完整的桌面工作階段，包括應用程式。 VDI 環境通常會使用基底作業系統映像，而此映像後續將成為桌面的基礎，供使用者工作之用。 VDI 實作有多種變化，例如「持續性」、「非持續性」和「桌面工作階段」。 持續性類型會將 VDI 桌面作業系統的變更保留到下一個工作階段。 非持續性類型則不會將 VDI 桌面作業系統的變更保留到下一個工作階段。 對使用者而言，此桌面除了可透過網路存取以外，與其他虛擬或實體裝置相較也稍有不同。

最佳化設定會在參考裝置上進行。 VM 是建置映像的理想位置，因為您可以儲存狀態、建立檢查點、進行備份，以及執行其他有用的工作。 一開始應先在基底 VM 上安裝預設作業系統，然後移除不需要的應用程式、安裝 Windows 更新、安裝其他更新、刪除暫存檔案、套用設定，以針對 VDI 最佳化基底 VM。

此外還有其他類型的 VDI，例如持續性和遠端桌面服務 (RDS)。 關於這些技術的深入說明不在本主題的討論之列，其重點放在參考環境中其他因素 (例如主機最佳化) 的 Windows 基底映像設定。

### <a name="persistent-vdi"></a>持續性 VDI

持續性 VDI 屬於基本層級，是會在重新啟動後仍保留作業系統狀態的 VM。 VDI 解決方案的其他軟體層可讓使用者輕鬆且順暢地存取其指派的 VM (通常是使用單一登入解決方案)。

持續 VDI 有數種不同的實作：

- 傳統虛擬機器的 VM 具有其本身的虛擬磁碟檔案，可以正常啟動、將一個工作階段的變更儲存到下一個工作階段，且在本質上只是一般的 VM。 其差別在於使用者存取此 VM 的方式。 可能會有 Web 入口網站，會在使用者登入後自動將其導向至其一或多個指派的 VDI VM。

- 各有個人虛擬磁碟的映像式持續性虛擬機器。 在此類型的實作中，一或多個主機伺服器上會有基底/黃金映像。 在 VM 建立後，會建立一或多個虛擬磁碟，並指派給此磁碟供持續性儲存之用。

    - 在 VM 啟動時，系統會將基底映像的複本讀取到 VM 的記憶體中。 同時，持續性虛擬磁碟會連同透過複雜的程序合併的任何既有作業系統變更，指派給該 VM。

    - 事件記錄寫入、記錄寫入等變更會重新導向至已指派給該 VM 的讀取/寫入虛擬磁碟。

    - 在此情況下，作業系統和應用程式維護可使用傳統維護軟體 (例如 Windows Server Update Services) 或其他管理技術正常運作。

### <a name="non-persistent-vdi"></a>非持續性 VDI

非持續性 VDI 實作以基底或「黃金」映像為基礎時，最佳化主要會在基底映像上執行，其次才是透過本機設定與本機原則執行。

在映像式非持續性 VDI 中，基底映像是唯讀的。 在非持續性 VDI VM 啟動時，系統會將基底映像的複本串流處理至 VM。 在啟動期間及其後到下次重新開機之間所發生的活動，都會重新導向至暫存位置。 使用者通常會獲得用來儲存其資料的網路位置。 在某些情況下，使用者的設定檔會與標準 VM 合併，而為使用者提供其設定。

對於以單一映像為基礎的非持續性 VDI 而言，維護是其重要層面之一。 作業系統的更新通常會每個月提供一次。
在映像式 VDI，要取得映像的更新必須執行一組特定程序：

- 在指定的主機上，該主機上所有衍生自基底映像的 VM，都必須關閉電源或關閉。 這表示使用者會重新導向至其他 VM。

- 隨後，基底映像會開啟並啟動。 接著就會執行所有的維護活動，例如作業系統更新、.NET 更新、應用程式更新等。

- 任何要套用的新設定都會在此時套用。

- 任何其他維護也都會在此時執行。

- 其後，基底映像會關閉。

- 基底映像會進行密封，並依設定回到生產環境。

-使用者可以重新登入。

> [!NOTE]
> Windows 10 會定期自動執行一組維護工作。 有一項排定工作依預設會在當地時間的每天凌晨 3:00 執行。 這項排定工作會執行一系列的工作，包括 Windows Update 清理。 您可以使用下列 PowerShell 命令，檢視所有會自動執行的維護工作類別：
>
>```powershell
>Get-ScheduledTask | ? {$_.Settings.MaintenanceSettings}
>```
>

非持續性 VDI 的挑戰之一，是當使用者登出時，幾乎所有的作業系統活動都會捨棄。 使用者的設定檔和/或狀態可能會儲存，但虛擬機器本身會捨棄前次開機後所做的絕大多數變更。 因此，預計要對可將狀態保留到下一個工作階段的 Windows 電腦進行的最佳化，將不適用。

根據 VM VDI 的架構，PreFetch 和 SuperFetch 等功能都無法利用跨工作階段的持續性來提供協助，因為在 VM 重新啟動後，所有最佳化都會被捨棄。 編製索引可能會浪費資源，傳統磁碟重組之類的任何磁碟最佳化也是一樣。

### <a name="to-sysprep-or-not-sysprep"></a>是否使用 Sysprep

Windows 10 有一項名為[系統準備工具](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)的內建功能 (常簡稱為 "Sysprep")。 Sysprep 工具可用來為自訂的 Windows 10 映像進行複製準備。 Sysprep 程序可確保產生的作業系統會有適當的獨特性可在生產環境中執行。
執行 Sysprep 各有其優缺點。 以 VDI 為例，您可能會希望能夠自訂預設使用者設定檔，以供後續使用此映像登入的使用者作為設定檔範本。 您可能會想要安裝某些應用程式，但同時又希望能控制個別應用程式的設定。

替代方式是使用標準 .ISO 進行安裝；您可以搭配使用自動的安裝回應檔案，以及用來安裝應用程式或移除應用程式的工作序列。 您也可以使用工作序列來設定映像中的本機原則設定；或許可搭配使用[本機群組原則物件公用程式 (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0) 工具。

#### <a name="vdi-optimization-categories"></a>VDI 最佳化類別

- 全域作業系統設定

    - UWP 應用程式清理

    - 選用功能清理

    - 本機原則設定

    - 系統服務

    -   排定的工作

    -   套用 Windows 更新

    -   自動 Windows 追蹤

    -   在完成 (密封) 映像之前清理磁碟

-   使用者設定

-   Hypervisor/主機設定

- [使用登錄設定調整 Windows 10 網路效能](#tuning-windows-10-network-performance-by-using-registry-settings)

- [Windows 受限流量有限功能基準](https://go.microsoft.com/fwlink/?linkid=828887)指引中的其他設定。

- [磁碟清理](#disk-cleanup-including-using-the-disk-cleanup-wizard)

### <a name="universal-windows-platform-app-cleanup"></a>通用 Windows 平台應用程式清理

VDI 映像的目標之一是要越小越好。 縮小映像大小的方式之一，是移除環境中將不會使用的 UWP 應用程式。 使用 UWP 應用程式時，會有主要的應用程式檔案，也就是承載。 每個使用者設定檔中都會儲存少量的資料，供應用程式的特定設定使用。 此外，「所有使用者」設定檔中也會有少量的資料。

就 UWP 應用程式清理而言，連線能力和時機是關鍵所在。 如果您將基底映像部署到沒有網路連線的裝置，在您嘗試解除安裝應用程式時，Windows 10 將無法連線至 Microsoft Store 下載應用程式並嘗試加以安裝。

如果您修改用來安裝 Windows 10 的基底 .WIM，並在安裝之前從 .WIM 中移除了不必要的 UWP 應用程式，則這些應用程式將不會安裝並啟動，而您的設定檔建立時間應該會縮短。 您將在本節稍後的內容中找到關於如何從安裝 .WIM 檔案中移除 UWP 應用程式的資訊。

就 VDI 而言，佈建您在基底映像中需要的應用程式，並在其後限制或封鎖對 Microsoft Store 的存取，是不錯的策略。 在一般的電腦上，市集應用程式會定期在背景更新。 UWP 應用程式可在維護期間，於系統套用其他更新時進行更新。

#### <a name="delete-the-payload-of-uwp-apps"></a>刪除 UWP 應用程式的承載

不需要的 UWP 應用程式仍會在檔案系統中耗用少量的磁碟空間。 針對永遠不會用到的應用程式，您可以使用 PowerShell 命令從基底映像中移除非必要 UWP 應用程式的承載。

事實上，如果您使用本節後續提供的連結從安裝 .WIM 檔案中移除這類承載，您應該能夠以非常精簡的 UWP 應用程式清單從頭開始。

執行下列命令可列舉執行中的 Windows 10 作業系統已佈建的 UWP 應用程式，如 PowerShell 所產生的下列節錄範例輸出所示：

```powershell

    Get-AppxProvisionedPackage -Online

    DisplayName  : Microsoft.3DBuilder
    Version      : 13.0.10349.0
    Architecture : neutral
    ResourceId   : \~
    PackageName  : Microsoft.3DBuilder_13.0.10349.0_neutral_\~_8wekyb3d8bbwe
    Regions      :
```

佈建至系統的 UWP 應用程式可在作業系統安裝期間以工作序列移除，或是在作業系統安裝後移除。 後者可能是較理想的方法，因為它可將建立或維護映像的整體程序模組化。 在您開發指令碼後，如果某個項目在後續組建中有所變更，您可以編輯現有的指令碼，而非從頭重複相關程序。 以下提供一些連結供您參考本主題的相關資訊：

[在工作序列期間移除 Windows 10 內建應用程式](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)

[使用 Powershell 1.3 版移除 Windows 10 WIM 檔案中的內建應用程式](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)

[Windows 10 1607：使應用程式不會在部署功能更新時再次出現](/archive/blogs/mniehaus/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update)

然後，執行 [Remove-AppxProvisionedPackage](/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) PowerShell 命令以移除 UWP 應用程式承載：

```powershell
Remove-AppxProvisionedPackage -Online -PackageName
```

對於每個 UWP 應用程式，您都應評估它在每個獨特環境中的適用性。 您可以安裝 Windows 10 1803 版的預設安裝，然後記下哪些應用程式正在執行且耗用記憶體。 例如，您可以考慮移除會自動啟動的應用程式，或是自動在 [開始] 功能表中顯示資訊 (例如氣象和新聞)、但在您的環境中可能不會用到的應用程式。

其中有一個名為「相片」的「內建」UWP 應用程式，具有一項名為 [有新的可用相簿時顯示通知]  的預設設定。  相片應用程式大約會使用 145 MB 的記憶體中 (特別是私人工作集記憶體)，即使未使用時也是如此。  為所有使用者變更 [有新的可用相簿時顯示通知]  設定目前並非可行的做法，因此建議在不需要或不想要使用相片應用程式時加以移除。

### <a name="clean-up-optional-features"></a>清理選用功能

#### <a name="managing-optional-features-with-powershell"></a>使用 PowerShell 管理選用功能

 若要列舉目前已安裝的 Windows 功能，請執行下列 PowerShell 命令：

```powershell
Get-WindowsOptionalFeature -Online
```

您可以啟用或停用特定 Windows 選用功能，如本例所示：

```powershell
Enable-WindowsOptionalFeature -Online -FeatureName "DirectPlay"
```

如需詳細資訊，請參閱 [Windows PowerShell 論壇](/answers/topics/windows-server-powershell.ht)。

#### <a name="enable-or-disable-windows-features-by-using-dism"></a>使用 DISM 啟用或停用 Windows 功能

您可以使用內建的 **Dism.exe** 工具來列舉及控制 Windows 選用功能。 您可以設定會在安裝作業系統的工作序列期間執行的 Dism.exe 指令碼。

### <a name="local-policy-settings"></a>本機原則設定

VDI 環境中的許多 Windows 10 最佳化都可使用 Windows 原則來進行。 此處所列的設定可在本機套用至基底映像。 而且，如果未以任何其他方式 (例如藉由群組原則) 指定等同的設定，就仍會套用這些設定。

某些決策可能取決於環境的詳細資訊，例如：

-   VDI 環境是否可存取網際網路？

-   VDI 解決方案是持續性還是非持續性的？

下列設定確定不會與任何關係到安全性的設定發生牴觸或衝突。 我們選擇以這些設定移除可能不適用於 VDI 環境的設定。

> [!NOTE]
> 在下方的群組原則設定表格中，以星號標示的項目來自於 [Windows 受限流量有限功能基準](https://go.microsoft.com/fwlink/?linkid=828887)。

| 原則設定 | 項目 | 子項目 | 可能的設定和註解 |
|--|--|--|--|
| **本機電腦原則 \\ 電腦設定 \\ Windows 設定 \\ 安全性設定** |  |  |  |
| **網路清單管理員原則** | 所有網路內容 | 網路位置 | 使用者不可以變更位置 |
| **本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 控制台** |  |  |  |
| \***控制台** | 允許線上秘訣 |  | 已停用 (設定將不會聯繫 Microsoft 內容服務以擷取秘訣和說明內容) |
| **\*控制台**\\ 個人化 | 不要顯示鎖定畫面 |  | 已啟用 (此原則設定可控制是否對使用者顯示鎖定畫面。 如果您啟用此原則設定，不需要按 CTRL + ALT + DEL 即可登入的使用者在鎖定電腦後將會看到他們選取的磚。) |
| **\*控制台**\\ 個人化 | 強制使用特定的預設鎖定畫面和登入影像 | [![設定鎖定畫面影像路徑的 UI 影像](media/lock-screen-image-settings.png)](media/lock-screen-image-settings.png) | 已啟用 (此設定可讓您指定沒有任何使用者登入時的預設鎖定畫面和登入影像，並將指定的影像設定為所有使用者的預設值 -- 它會取代預設的影像。)如果影像的解析度較低，且並不複雜，在每次轉譯影像時透過網路傳輸的資料將會較少。 |
| **\*控制台**\\ 地區及語言選項\\手寫個人化 | 關閉自動學習 |  | 已啟用 (如果啟用此原則設定，將會停止自動學習，並且刪除任何已儲存的資料。 使用者無法在控制台中設定此設定) |
| **本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 網路** |  |  |  |
| **背景智慧型傳送服務 (BITS)** | 不允許 BITS 用戶端使用 Windows 分支快取 |  | 啟用 |
| **背景智慧型傳送服務 (BITS)** | 不允許此電腦做為 BITS 對等快取用戶端 |  | 啟用 |
| **背景智慧型傳送服務 (BITS)** | 不允許此電腦做為 BITS 對等快取伺服器 |  | 啟用 |
| **背景智慧型傳送服務 (BITS)** | 允許 BITS 對等快取 |  | 停用 |
| **BranchCache** | 開啟 BranchCache |  | 停用 |
| \***字型** | 啟用字型提供者 |  | 已停用 (Windows 不會連線至線上字型提供者，而只會列舉本機安裝的字型。) |
| **熱點驗證** | 啟用熱點驗證 |  | 停用 |
| **Microsoft 對等網路服務** | 關閉 Microsoft 對等網路服務 |  | 啟用 |
| **網路連線狀態指示器** (請注意，此區段中有其他設定可以在隔離的網路中使用) | 指定被動輪詢 | 停用被動輪詢 (核取方塊) | 已啟用 (在隔離的網路上請使用此設定，否則請使用靜態 IP 位址。) |
| **離線檔案** | 允許或禁止使用離線檔案功能 |  | 停用 |
| **\*TCPIP 設定**\\ IPv6 轉換技術 | 設定 Teredo 狀態 | 已停用狀態 | 已啟用 (在已停用狀態中，主機上不會顯示任何 Teredo 介面。) |
| **\*WLAN 服務**\\ WLAN 設定 | 允許 Windows 自動連線至建議的開放式熱點、連絡人共用的網路，及提供付費服務的熱點 |  | 已停用 ([連線至建議的開放式熱點]  、[連線至我的連絡人共用的網路]  和 [啟用付費服務]  將會關閉，且此裝置上的使用者將無法加以啟用。) |
| **本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 開始功能表和工作列** |  |  |  |
| \***通知** | 關閉通知網路使用方式 |  | 已啟用 (如果啟用此原則設定，應用程式和系統功能將無法透過網路從 WNS 或使用通知輪詢 API 接收通知。) |
| **本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 系統** |  |  |  |
| **裝置安裝** | 當裝置上已安裝標準驅動程式時，不要傳送 Windows 錯誤報告 |  | 啟用 |
| **裝置安裝** | 在通常會提示建立系統還原點的裝置活動期間，防止建立系統還原點 |  | 啟用 |
| **裝置安裝** | 防止從網際網路擷取裝置中繼資料 |  | 啟用 |
| **裝置安裝** | 當裝置驅動程式在安裝期間要求其他軟體時，防止 Windows 傳送錯誤報告 |  | 啟用 |
| **裝置安裝** | 在裝置安裝期間關閉 [找到新硬體]  註解方塊 |  | 啟用 |
| **檔案系統**\\NTFS | 簡短名稱建立選項 | 在所有磁碟區上停用 | 啟用 |
| \***群組原則** | 透過應用程式 URL 處理常式設定 Web 對應用程式的連結 |  | 已停用 (停用 Web 至應用程式的連結，並在預設瀏覽器中開啟 http(s) URI，而不是啟動相關聯的應用程式。) |
| \***群組原則** | 在此裝置上繼續體驗 |  | 已停用 (Windows 裝置無法讓其他裝置搜尋到，且無法參與跨裝置體驗。) |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉對所有 Windows Update 功能的存取 |  | 已啟用 (如果啟用此原則設定，則會移除所有 Windows Update 功能。 這包括在 https://windowsupdate.microsoft.com 上、從 [開始] 功能表的 Windows Update 超連結，以及在 Internet Explorer 的 [工具] 功能表上封鎖對 Windows Update 網站的存取。 Windows 自動更新也會停用；您不會收到 Windows Update 的相關通知，也不會收到其重大更新。 此原則設定也會防止裝置管理員從 Windows Update 網站自動安裝驅動程式更新。) |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉自動根憑證更新 |  | 已啟用 (如果啟用此原則設定，當您的憑證是由未受信任的根憑證授權單位所簽發時，您的電腦將不會連線至 Windows Update 網站以確認 Microsoft 是否在其信任的授權單位清單中新增了 CA。)  **注意：** 如果您除了最新的憑證撤銷清單以外還有替代方法，才可使用此原則。 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉事件檢視器 "Events.asp" 連結 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉個人化手寫資料共用 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉手寫辨識錯誤報告 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉說明及支援中心的「您知道嗎?」 內容 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉說明及支援中心 Microsoft 知識庫搜尋 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 如果 URL 連線正在參照 Microsoft.com 時，關閉網際網路連線精靈 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉網頁發佈和線上訂購精靈的網際網路下載 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉網際網路檔案關聯服務 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 如果 URL 連線正在參照 Microsoft.com 時，關閉註冊 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉「訂購沖印」圖片工作 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉檔案及資料夾的「發佈到網站」工作 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉 Windows Messenger 客戶經驗改進計畫 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉 Windows 客戶經驗改進計畫 |  | 啟用 |
| **\*網際網路通訊管理**\\ 網際網路通訊設定 | 關閉 Windows 網路連線狀態指示器使用中測試 |  | 已啟用 (此原則設定會關閉由 Windows 網路連線狀態指示器 (NCSI) 執行，用以判斷您的電腦是連線至網際網路還是受限網路的使用中測試。在判斷連線層級的過程中，NCSI 會執行兩項使用中測試之一：從專用的 Web 伺服器下載頁面，或發出專用位址的 DNS 要求。 如果啟用此原則設定，NCSI 將不會執行兩項使用中測試。 這可能會降低 NCSI 以及使用 NCSI 判斷網際網路存取的能力) 注意：如果想要使用這項功能，有其他原則可讓您將 NCSI 測試重新導向至內部資源。 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉 Windows 錯誤報告 |  | 啟用 |
| **網際網路通訊管理**\\ 網際網路通訊設定 | 關閉 Windows Update 裝置驅動程式搜尋 |  | 啟用 |
| **登入** | 顯示首次登入動畫 |  | 停用 |
| **登入** | 關閉鎖定畫面上的應用程式通知 |  | 啟用 |
| **登入** | 關閉 Windows 啟動音效 |  | 啟用 |
| **電源管理** | 選取使用中電源計劃 | 高效能 | 啟用 |
| **復原** | 允許將系統還原到預設狀態 |  | 停用 |
| \***存放健全狀況** | 允許下載磁碟失敗預測模型的更新 |  | 已停用 (不會下載磁碟失敗預測模型的更新) |
| \***Windows 時間服務**\\ 時間提供者 | 啟用 Windows NTP 用戶端 |  | 已停用 (如果停用或未設定此原則設定，則本機電腦時鐘的時間不會與 NTP 伺服器同步化) **注意**：*使用此設定前請謹慎考量*。 已加入網域的 Windows 裝置都使用 **NT5DS**。 父系網域 DC 的 DC 可使用 NTP。 PDCe 角色可使用 NTP。 虛擬機器有時會使用「增強功能」或「整合服務」。 |
| **疑難排解與診斷**\\ 排程維護 | 設定排程維護行為 |  | 停用 |
| **疑難排解與診斷**\\ Windows 開機效能診斷 | 設定狀況執行層級 |  | 停用 |
| **疑難排解與診斷**\\ Windows 記憶體流失診斷 | 設定狀況執行層級 |  | 停用 |
| **疑難排解與診斷**\\ Windows 資源耗損偵測與解析 | 設定狀況執行層級 |  | 停用 |
| **疑難排解與診斷**\\ Windows 關機效能診斷 | 設定狀況執行層級 |  | 停用 |
| **疑難排解與診斷**\\ Windows 待命/繼續效能診斷 | 設定狀況執行層級 |  | 停用 |
| **疑難排解與診斷**\\ Windows 系統回應效能診斷 | 設定狀況執行層級 |  | 停用 |
| \***使用者設定檔** | 關閉廣告識別碼 |  | 已啟用 (如果啟用此原則設定，廣告識別碼將會關閉。 應用程式無法跨應用程式使用識別碼。) |
| **本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ Windows 元件** |  |  |  |
| **新增功能到 Windows 10** | 防止精靈執行 |  | 啟用 |
| \***應用程式隱私權** | 讓 Windows App 存取帳戶資訊 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取帳戶資訊，且您組織中的員工無法加以變更) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取通訊記錄 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取通訊記錄，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取連絡人 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取連絡人，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取其他應用程式的診斷資訊 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果停用或未設定此原則設定，您組織中的員工將可使用裝置上的 [設定 \> 隱私權] 決定 Windows 應用程式是否可取得關於其他應用程式的診斷資訊) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取電子郵件 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制允許] 選項，Windows 應用程式將可存取電子郵件，且您組織中的員工無法加以變更) |
| \***應用程式隱私權** | 讓 Windows App 存取位置 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取位置，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取訊息中心 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取位置，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取動作資料 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取動作資料，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取通知 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取通知，且您組織中的員工無法加以變更) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取工作 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取工作，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取行事曆 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取行事曆，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取相機 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取相機，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取麥克風 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取麥克風，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows App 存取信任的裝置 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法存取信任的裝置，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式與未配對裝置通訊 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法與未配對的無線裝置通訊，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式存取無線通訊 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無權控制無線通訊，且您組織中的員工無法加以變更。) |
| **應用程式隱私權** | 讓 Windows 應用程式撥打電話 | 所有應用程式的預設值：強制拒絕 | 已啟用 (Windows 應用程式無法撥打電話，且您組織中的員工無法加以變更。) |
| \***應用程式隱私權** | 讓 Windows 應用程式在背景執行 | 所有應用程式的預設值：強制拒絕 | 已啟用 (如果選擇 [強制拒絕]  選項，Windows 應用程式將無法在背景執行，且您組織中的員工無法加以變更。) |
| **自動播放原則** | 設定 AutoRun 的預設行為 | 不執行任何 AutoRun 命令 | 啟用 |
| \***自動播放原則** | 關閉自動播放 |  | 已啟用 (如果啟用此原則設定，光碟機與抽取式媒體磁碟機將會停用自動播放，或所有磁碟機皆停用。) |
| \***雲端內容** | 不顯示 Windows 祕訣 |  | 已啟用 (此原則設定可防止對使用者顯示 Windows 秘訣。) |
| \***雲端內容** | 關閉 Microsoft 消費者體驗 |  | 已啟用 (如果啟用此原則設定，使用者將不會再看到 Microsoft 提供的個人化建議，和其 Microsoft 帳戶的相關通知。) |
| \***資料收集與預覽版** | 允許遙測 | 0 – 安全性 [僅限企業版] | 已啟用 (設定值為 0 時，只會套用至執行企業版、教育版、IoT 版或 Windows Server 版的裝置。) |
| \***資料收集與預覽版** | 不顯示意見反應通知 |  | 啟用 |
| \***資料收集與預覽版** | 切換測試人員組建的使用者控制項 |  | 停用 |
| **傳遞最佳化** | 下載模式 | 下載模式：簡單 (99) | 99 = 沒有對等傳輸的簡單下載模式。 傳遞最佳化只會使用 HTTP 進行下載，且不會嘗試連線至傳遞最佳化雲端服務。 |
| **桌面視窗管理員** | 不允許呼叫 Flip3D |  | 啟用 |
| **桌面視窗管理員** | 不允許視窗動畫 |  | 啟用 |
| **桌面視窗管理員** | 使用純色做為開始背景 |  | 啟用 |
| **Edge UI** | 允許邊緣撥動 |  | 停用 |
| **Edge UI** | 停用說明秘訣 |  | 啟用 |
| \***檔案總管** | 設定 Windows Defender SmartScreen |  | 已停用 (將為所有使用者停用 SmartScreen。 使用者在嘗試從網際網路執行可疑的應用程式時，將不會收到警告。) |
|  |  |  | **注意**：如果未連線至網際網路，將使電腦無法嘗試聯繫 Microsoft 以取得 SmartScreen 資訊。 |
| **檔案總管** | 不顯示**已安裝新應用程式**通知 |  | 啟用 |
| \***尋找我的裝置** | 開啟/關閉尋找我的裝置 |  | 已停用 (當 [尋找我的裝置] 關閉時，將不會註冊裝置及其位置，且 [尋找我的裝置] 功能將無法運作。 使用者也無法在其裝置上檢視其作用中的數位板最後一次使用的位置。) |
| **遊戲總管** | 關閉遊戲資訊下載 |  | 啟用 |
| **遊戲總管** | 關閉遊戲更新 |  | 啟用 |
| **遊戲總管** | 關閉 [遊戲] 資料夾中追蹤最後一次執行遊戲時間的功能 |  | 啟用 |
| **Homegroup** | 防止電腦加入家用群組 |  | 啟用 |
| \***Internet Explorer** | 當使用者在網址列輸入時，允許 Microsoft 服務提供加強式建議 |  | 已停用 (當使用者在網址列輸入時將不會收到加強式建議。 此外，使用者將無法變更 [建議] 設定。) |
| **Internet Explorer** | 停用定期檢查以進行 Internet Explorer 軟體更新 |  | 啟用 |
| **Internet Explorer** | 停用顯示啟動顯示畫面的功能 |  | 啟用 |
| **Internet Explorer** | 自動安裝新版 Internet Explorer |  | 停用 |
| **Internet Explorer** | 防止參與客戶經驗改進計畫 |  | 啟用 |
| **Internet Explorer** | 防止執行 [首次執行] 精靈 | 直接前往首頁 | 啟用 |
| **Internet Explorer** | 設定索引標籤程序成長速率 | 低 | 啟用 |
| **Internet Explorer** | 指定新索引標籤的預設行為 | 新索引標籤頁面 | 啟用 |
| **Internet Explorer** | 關閉附加元件效能通知 |  | 啟用 |
| \***Internet Explorer** | 關閉網址的自動完成功能 |  | 已啟用 (如果啟用此原則設定，使用者在輸入網址時將不會收到相符項目建議。 使用者無法變更設定網址的自動完成功能。) |
| \***Internet Explorer** | 關閉瀏覽器地理位置 |  | 已啟用 (如果啟用此原則設定，將會關閉瀏覽器地理位置支援。) |
| **Internet Explorer** | 關閉 [重新開啟上一次的瀏覽工作階段] |  | 啟用 |
| \***Internet Explorer** | 開啟建議的網站 |  | 已停用 (如果停用此原則設定，就會關閉與此功能相關聯的進入點和功能。) |
| **\*Internet Explorer**\\ 相容性檢視 | 關閉相容性檢視 |  | 已啟用 (如果啟用此原則設定，使用者將無法使用 [相容性檢視] 按鈕或管理 [相容性檢視] 網站清單。) |
| **Internet Explorer**\\ 網際網路控制台<strong>\\</strong> 進階頁面 | 播放網頁動畫 |  | 停用 |
| **Internet Explorer**\\ 網際網路控制台\\ 進階頁面 | 播放網頁影片 |  | 停用 |
| **\*Internet Explorer**\\ 網際網路控制台\\ 進階頁面 | 關閉快速翻頁和頁面預測功能 |  | 已啟用 (Microsoft 會收集您的瀏覽歷程記錄來改善快速翻頁與頁面預測的運作方式。 此功能不適用於 Internet Explorer (傳統型)。 如果啟用這個原則設定，則會關閉快速翻頁與頁面預測，而且背景中不會載入下一個網頁。) |
| **Internet Explorer**\\ 網際網路設定\\ 進階設定\\ 瀏覽 | 關閉電話號碼偵測 |  | 啟用 |
| **Internet Explorer**\\ 網際網路設定\\ 進階設定\\ 多媒體 | 允許 Internet Explorer 播放使用替代轉碼器的媒體檔案 |  | 停用 |
| \***定位和感應器** | 關閉定位 |  | 已啟用 (如果啟用此原則設定，將會關閉定位功能，且這部電腦上的所有程式都無法使用定位功能的位置資訊。) |
| **定位和感應器** | 關閉感應器 |  | 啟用 |
| **定位和感應器 /** Windows 定位提供者 | 關閉 Windows 定位提供者 |  | 啟用 |
| \***地圖** | 關閉自動下載與更新地圖資料 |  | 已啟用 (如果啟用此設定，將會關閉地圖資料的自動下載和更新。) |
| \***地圖** | 關閉 [離線地圖] 設定頁面上非要求的網路流量 |  | 已啟用 (如果啟用此原則設定，將會關閉 [離線地圖] 設定頁面上產生網路流量的功能。 注意：這可能會關閉整個設定頁面。) |
| \***訊息中心** | 允許訊息服務雲端同步 |  | 已停用 (此原則設定可讓您將行動電話簡訊備份和還原至 Microsoft 的雲端服務。) |
| \***Microsoft Edge** | 允許網址列下拉式清單建議 |  | 停用 |
| \***Microsoft Edge** | 允許書籍媒體櫃的設定更新 |  | 已停用 (關閉 Microsoft Edge 中的相容性清單。) |
| \***Microsoft Edge** | 允許 Microsoft 相容性清單 |  | 已停用 (如果停用此設定，在瀏覽瀏覽器期間將不會使用「Microsoft 相容性清單」。) |
| \***Microsoft Edge** | 允許在新索引標籤上顯示網頁內容 |  | 已停用 (指示 Edge 在開啟新的索引標籤時以空白內容開啟。) |
| \***Microsoft Edge** | 設定自動填入 |  | 已停用 (停用網址列的自動填寫。) |
| \***Microsoft Edge** | 設定不要追蹤 |  | 已啟用 (如果啟用此設定，將一律傳送「不要追蹤」要求到要求追蹤資訊的網站。) |
| \***Microsoft Edge** | 設定密碼管理員 |  | 已停用 (如果停用此設定，員工將無法使用密碼管理員將密碼儲存在本機。) |
| \***Microsoft Edge** | 設定網址列中的搜尋建議 |  | 已停用 (使用者無法在 Microsoft Edge 的網址列中看到搜尋建議。) |
| \***Microsoft Edge** | 設定起始畫面 |  | 已啟用 (如果啟用此設定，您將可設定一或多個 [開始] 頁面。 如果啟用這項設定，您也必須包含網頁的 URL，並使用此格式與角括弧分隔多個頁面︰\<support.contoso.com\>\<support.microsoft.com\> Windows 10 版本 1703 或更新版本：如果您不想要將流量傳送至 Microsoft，您可以使用 \<about:blank\> 值；無論裝置是否已加入網域，如果只有這個已設定的 URL，就會接受此值。 |
| \***Microsoft Edge** | 設定 Windows Defender SmartScreen |  | 已停用 (會關閉 Windows Defender SmartScreen，且員工無法加以開啟。) |
|  |  |  | **注意**：請考慮在環境中使用此設定。 如果未連線至網際網路，將使電腦無法嘗試聯繫 Microsoft 以取得 SmartScreen 資訊。 |
| \***Microsoft Edge** | 防止 Microsoft Edge 開啟 [首次執行] 網頁 |  | **已啟用** (使用者在初次開啟 Microsoft Edge 時不會看見 [首次執行] 頁面。) |
| **OneDrive** | 防止 OneDrive 產生網路流量直到使用者登入 OneDrive |  | **已啟用** (啟用此設定可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。) |
| \***OneDrive** | 防止使用 OneDrive 儲存檔案 |  | **已啟用** (除非 OneDrive 是在內部部署或外部部署使用。) |
| **OneDrive** | 預設將文件儲存到 OneDrive |  | **已停用** (除非 OneDrive 是在內部部署或外部部署使用。) |
| **RSS 摘要** | 防止自動探索摘要和網頁快訊 |  | **已啟用** |
| \***RSS 摘要** | 關閉摘要和網頁快訊的背景同步處理 |  | **已啟用** (如果啟用此原則設定，將會停用在背景中同步處理摘要和網頁快訊的功能。) |
| \***搜尋** | 允許 Cortana |  | **已停用** (當 Cortana 已關閉時，使用者仍可使用搜尋功能尋找裝置上的項目。) |
| **搜尋** | 允許在鎖定畫面上使用 Cortana |  | **已停用** |
| \***搜尋** | 允許搜尋和 Cortana 使用定位 |  | **已停用** |
| **搜尋** | 不允許 Web 搜尋 |  | **已啟用** |
| \***搜尋** | 不要在 [搜尋] 中搜尋網路或顯示網路搜尋結果 |  | **已啟用** (如果啟用此原則設定，則不會在 Web 上執行查詢，且在使用者使用 [搜尋] 執行查詢時將不會顯示 Web 結果。) |
| **搜尋** | 防止從控制台新增 UNC 位置到索引 |  | **已啟用** |
| **搜尋** | 防止對離線檔案快取中的檔案編製索引 |  | **已啟用** |
| \***搜尋** | 設定 [搜尋] 中可以分享的資訊 | 匿名資訊 | **已啟用** (共用使用資訊，但不共用搜尋歷程記錄、Microsoft 帳戶資訊或特定位置。) |
| \***軟體保護平台** | 關閉 KMS 用戶端線上 AVC 驗證 |  | **已啟用** (啟用此設定可防止此電腦傳送與其啟用狀態有關的資料給 Microsoft。) |
| \***語音** | 允許自動更新語音資料 |  | **已停用** (不會定期檢查是否有更新的語音模型) |
| \***市集** | 關閉自動下載與安裝更新 |  | **已啟用** (如果啟用此設定，將會關閉應用程式更新的自動下載和安裝。) |
| \***市集** | 在 Win8 裝置上關閉更新的自動下載 |  | **已啟用** (如果啟用此設定，將會關閉應用程式更新的自動下載。) |
| **市集** | 關閉更新至最新版 Windows 的服務 |  | **已啟用** |
| \***同步您的設定** | 不要同步 | 允許使用者開啟同步 (未選取) | **已啟用** (如果啟用此設定，將會關閉 [同步您的設定]，且在裝置上不會同步任何 [同步您的設定] 群組。) |
| **文字輸入** | 改進筆跡及輸入辨識 |  | **已停用** |
| **Windows Defender 防毒軟體**\\ MAPS | 加入 Microsoft MAPS |  | **已停用** (如果停用或未設定此設定，您將不會加入 Microsoft MAPS。) |
| **Windows Defender 防毒軟體**\\ MAPS | 需要進一步分析時發送檔案樣本 | 一律不傳送 | **已啟用** (只有在未選擇加入 MAPS 診斷資料時) |
| **Windows Defender 防毒軟體**\\ 報告 | 關閉增強通知 |  | **已啟用** (如果啟用此設定，將不會在用戶端上顯示 Windows Defender 防毒軟體增強通知。) |
| **Windows Defender 防毒軟體**\\ 特徵碼更新 | 定義下載定義更新的來源順序 | FileShares | **已啟用** (如果啟用此設定，將會以指定的順序聯繫定義更新來源。 從一個指定的來源成功下載定義更新後，即不會聯繫清單中的其餘來源。) |
| **Windows 錯誤報告** | 自動傳送作業系統產生的錯誤報告的記憶體傾印 |  | **已停用** |
| **Windows 錯誤報告** | 停用 Windows 錯誤報告 |  | **已啟用** |
| **Windows 遊戲錄影和廣播** | 啟用或停用 Windows 遊戲錄影和廣播 |  | **已停用** |
| **Windows Installer** | 控制基準檔案快取的大小上限 | 5 | **已啟用** |
| **Windows Installer** | 關閉系統還原檢查點建立功能 |  | **已啟用** |
| **Windows Mail** | 關閉社群功能 |  | **已啟用** |
| **Windows Media Player** | 不要顯示 [安裝第一次使用] 對話方塊 |  | **已啟用** |
| **Windows Media Player** | 防止媒體共用 |  | **已啟用** |
| **Windows 行動中心** | 關閉 Windows 行動中心 |  | **已啟用** |
| **Windows 可靠性分析** | 設定可靠性 WMI 提供者 |  | **已停用** |
| **Windows Update** | 允許立即安裝自動更新 |  | **已啟用** |
| **Windows Update** | 不連接到任何 Windows Update 網際網路位置 |  | **已啟用** (啟用此原則將會停用該功能，且可能會導致對公用服務 (例如 Windows 市集) 的連線停止運作。) **注意：** 這項原則只在此裝置設定為使用 [指定近端內部網路 Microsoft 更新服務的位置] 原則連線到近端內部網路更新服務時套用。 |
| **Windows Update** | 移除對所有 Windows Update 功能的存取 |  | **已啟用** |
| **\*Windows Update**\\ 商務用 Windows Update | 管理預覽版 | 設定接收預覽版的行為： | **已啟用** (選取 [停用預覽版]  會防止在裝置上安裝預覽版。 這將導致使用者無法透過 [設定 -\> 更新與安全性] 選擇加入 Windows 測試人員計畫。) |
|  |  | 停用預覽版 |  |
| **\*Windows Update**\\ 商務用 Windows Update | 選取接收預覽版和功能更新的時機 | 半年通道 | **已啟用** (啟用此原則可指定要接收的預覽版或功能更新的層級和接收時機。) |
|  |  | 延期期間：365 天， |  |
|  |  | 暫停開始日期：yyyy-mm-dd |  |
| **Windows Update**\\ 商務用 Windows Update | 選取接收品質更新的時機 | 1. 30 天 2. 從 yyyy-mm-dd 開始暫停品質更新 | **已啟用** |
| **Windows 受限流量自訂原則設定** | 防止 OneDrive 產生網路流量直到使用者登入 OneDrive |  | **已啟用** (視需要啟用此設定，可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。) |
| **Windows 受限流量自訂原則設定** | 關閉 Windows Defender 通知 |  | **已啟用** (如果啟用此原則設定，Windows Defender 將不會傳送含有裝置健康情況和安全性的相關重要資訊的通知。) |
| **本機電腦原則 \\ 使用者設定 \\ 系統管理範本** |  |  |  |
| **控制台**\\ 地區及語言選項 | 關閉在我輸入時提供文字預測 |  | **已啟用** |
| **桌上型電腦** | 不要將最近開啟的文件共用新增到 [網路位置] 中 |  | **已啟用** |
| **桌上型電腦** | 關閉 Aero 搖動視窗最小化滑鼠手勢 |  | **已啟用** |
| **桌面** / Active Directory | Active Directory 搜尋的大小上限 | 2500 | **已啟用** |
| **開始功能表和工作列** | 不允許釘選市集應用程式到工作列 |  | **已啟用** |
| **開始功能表和工作列** | 不顯示或追蹤捷徑清單中來自遠端位置的項目 |  | **已啟用** |
| **開始功能表和工作列** | 不要用以搜尋為主的方法來解析殼層捷徑 |  | **已啟用** (系統不會執行最後的磁碟機搜尋。 它只會顯示訊息，說明找不到檔案。) |
| **開始功能表和工作列** | 從工作列中移除人員列 |  | **已啟用** (將會從工作列中移除人員圖示、從工作列設定頁面中移除對應的設定切換，且使用者無法將人員釘選到工作列。) |
| **開始功能表和工作列** | 關閉功能通告提示氣球通知 |  | **已啟用** (使用者無法將市集應用程式釘選到工作列。 如果市集應用程式已釘選到工作列，將會在下次登入時從工作列中移除。) |
| **開始功能表和工作列** | 關閉使用者追蹤 |  | **已啟用** |
| **開始功能表和工作列** / 通知 | 關閉快顯通知 |  | **已啟用** |
| **Windows 元件** / 雲端內容 | 關閉所有 Windows 焦點功能 |  | **已啟用** |
| **Edge UI** | 關閉追蹤應用程式使用量 |  | **已啟用** |
| **檔案總管** | 關閉縮圖快取處理 |  | **已啟用** |
| **檔案總管** | 關閉在 [檔案總管] 搜尋方塊中顯示最近搜尋項目 |  | **已啟用** |
| **檔案總管** | 關閉在隱藏的 thumbs.db 檔案中快取縮圖 |  | **已啟用** ||

### <a name="notes-about-network-connectivity-status-indicator"></a>網路連線狀態指示器的相關注意事項

上述的群組原則設定，包含將系統是否連線至網際網路的檢查關閉的設定。 如果您的環境完全不會連線至網際網路，或是會以間接方式連線，您可以設定群組原則設定，以從工作列中移除 [網路] 圖示。 如果您關閉網際網路連線檢查後，即使網路可能正常運作，[網路] 圖示上仍會有黃色旗標，您就可能會想要從工作列中移除 [網路] 圖示。 如果您想要透過群組原則設定移除網路圖示，您可以在下列位置找到該設定：

| Windows Update 或商務用 Windows Update | 選取接收品質更新的時機 | 1. 30 天 2. 從 yyyy-mm-dd 開始暫停品質更新 | 啟用 |
|--|--|--|--|
| **本機電腦原則 \\ 使用者設定 \\ 系統管理範本** |  |  |  |
| **開始功能表和工作列** | 移除網路圖示 |  | **已啟用** (網路圖示不會顯示在系統通知區域中。) |

如需網路連線狀態指示器 (NCSI) 的詳細資訊，請參閱：[網路連線狀態圖示](https://techcommunity.microsoft.com/t5/networking-blog/bg-p/NetworkingBlog)

### <a name="system-services"></a>系統服務

如果您考慮停用系統服務以節省資源，您必須仔細確認該服務在某方面是否為其他服務的元件之一。

此外，這些建議大多與 Windows Server 2016 桌面體驗方面的建議相對應；如需詳細資訊，[在包含桌面體驗的 Windows Server 2016 中停用的系統服務的指引](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md)。

請注意，許多可能適合停用的服務都設定為手動服務啟動類型。 這表示服務不會自動啟動，且除非有特定應用程式或服務對您考慮要停用的服務觸發了要求，才會啟動。 已設定為手動啟動類型的服務通常不會列於此處。

| Windows 服務 | 項目 | 註解 |
|--|--|--|
| CDPUserService | 此使用者服務適用於連線的裝置平台案例 | 注意：這是每一使用者服務，因此*範本服務*必須停用。 |
| 已連線使用者體驗與遙測 | 啟用支援應用程式內與已連線使用者體驗的功能。 此外，在 [意見反應與診斷] 下啟用診斷與使用方式隱私權選項設定時，此服務會管理診斷與使用方式資訊 (用於改進 Windows 平台的體驗與品質) 的事件導向收集與傳輸。 | 如果在中斷連線的網路上，請考慮停用 |
| 連絡人資料 | 為連絡人資料編製索引，以加快搜尋連絡人的速度。 如果您停止或停用此服務，則搜尋結果中可能會遺漏連絡人資訊。 | (PimIndexMaintenanceSvc) 注意：這是每一使用者服務，因此*範本服務*必須停用。 |
| 診斷原則服務 | 啟用對 Windows 元件的問題偵測、疑難排解和解決方案。 如果停止此服務，診斷將不再運作。 |  |
| 已下載的地圖管理員 | 適用於應用程式存取已下載地圖的 Windows 服務。 要存取已下載地圖的應用程式會依需求啟動此服務。 停用此服務將讓應用程式無法存取地圖。 |  |
| 地理位置服務 | 監視目前的系統位置並管理地理柵欄 |  |
| GameDVR 和廣播使用者服務 | 這項使用者服務用於遊戲錄影和即時廣播 | 注意：這是個別使用者的服務，因此範本服務必須停用。 |
| MessagingService | 支援文字訊息和相關功能的服務。 | 注意：這是每一使用者服務，因此*範本服務*必須停用。 |
| 最佳化磁碟機 | 將儲存磁碟機上的檔案最佳化，有助於提高電腦執行效率。 | VDI 解決方案通常不會受益於磁碟最佳化。 這些「磁碟機」並不是傳統的磁碟機，且通常只是暫時的儲存體配置。 |
| Superfetch | 維護並改進一段時間後的系統效能。 | 在 VDI 中通常無法改善效能，尤其是非持續性 VDI，因為在每次重新開機後就會捨棄作業系統狀態。 |
| 觸控式鍵盤與手寫面板服務 | 啟用觸控式鍵盤和手寫面板及筆跡功能 |  |
| Windows 錯誤報告 | 允許在程式停止運作或回應時報告錯誤，並允許傳遞現有的解決方案。 此外，也允許產生用於診斷與修復服務的記錄。 如果停止此服務，錯誤報告可能就無法正常運作，而且可能無法顯示診斷服務及修復的結果。 | 使用 VDI 時，診斷常會在離線狀態下執行，而不是在主要生產環境中執行。 此外，有些客戶會停用 WER。 WER 會針對許多不同的狀況產生少量資源，包括無法安裝裝置或無法安裝更新等狀況。 |
| Windows Media Player 網路共用服務 | 使用通用隨插即用，與網路上其他的播放器和媒體裝置共用 Windows Media Player 媒體櫃 | 除非客戶在網路上共用 WMP 媒體櫃，否則不需要。 |
| Windows 行動熱點服務 | 提供與其他裝置共用行動數據連線的能力。 |  |
| Windows 搜尋 | 提供檔案、電子郵件和其他內容的內容索引、屬性快取及搜尋結果。 | 可能不需要，尤其是使用非持續性 VDI 時 |

#### <a name="per-user-services-in-windows"></a>Windows 中的每一使用者服務

[每一使用者服務](/windows/application-management/per-user-services-in-windows)是會在使用者登入 Windows 或 Windows Server 時建立，並且在該使用者登出時停止並刪除的服務。這些服務執行於使用者帳戶的安全性內容中 - 其資源管理效能優於先前的方法所能提供的，因為過去須在 [總管] 中使用預先設定的帳戶或以工作的形式執行這類服務。

### <a name="scheduled-tasks"></a>排定的工作

如同 Windows 中其他項目，在您考慮停用某個項目前，請先確定您不需要該項目。

以下列出會在電腦上執行最佳化或資料收集，使電腦在重新開機後保有其狀態的工作。 當 VM VDI 工作重新開機並捨棄前次開機後的所有變更時，用於實體電腦的最佳化將沒有幫助。

您可以使用下列 PowerShell 程式碼，取得目前所有排定的工作 (包括描述)：

`Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description |Export-CSV -Path C:\Temp\W10_1803_SchTasks.csv -NoTypeInformation`

有效的**排定的工作名稱**值包括：

- OneDrive Standalone Update Task v2
- Microsoft Compatibility Appraiser
- ProgramDataUpdater
- StartupAppTask
- CleanupTemporaryState
- Proxy
- UninstallDeviceTask
- ProactiveScan
- 資訊彙集器
- UsbCeip
- 資料完整性掃描
- 損毀修復的資料完整性掃描
- ScheduledDefrag
- SilentCleanup
- Microsoft-Windows-DiskDiagnosticDataCollector
- 診斷
- StorageSense
- DmClient
- DmClientOnScenarioDownload
- 檔案歷程記錄 (維護模式)
- ScanForUpdates
- ScanForUpdatesAsUser
- SmartRetry
- 通知
- WindowsActionDialog
- WinSAT Cellular
- MapsToastTask
- ProcessMemoryDiagnosticEvents
- RunFullMemoryDiagnostic
- MNO 中繼資料剖析器
- LPRemove
- GatherNetworkInfo
- WiFiTask
- Sqm-Tasks
- AnalyzeSystem
- MobilityManager
- VerifyWinRE
- RegIdleBackup
- FamilySafetyMonitor
- FamilySafetyRefreshTask
- IndexerAutomaticMaintenance
- SpaceAgentTask
- SpaceManagerTask
- HeadsetButtonPress
- SpeechModelDownloadTask
- ResPriStaticDbSync
- WsSwapAssessmentTask
- SR
- SynchronizeTimeZone
- Usb-Notifications
- QueueReporting
- UpdateLibrary
- 排定的開始時間
- sih
- XblGameSaveTask

### <a name="apply-windows-and-other-updates"></a>套用 Windows 和其他更新

無論是從 Microsoft Update，還是從您的內部資源，都可套用可用的更新，包括 Windows Defender 特徵碼。 這是套用其他可用更新的好時機，包括 Microsoft Office 的更新 (如有安裝)。

### <a name="automatic-windows-traces"></a>自動 Windows 追蹤

Windows 依預設設定會收集並儲存有限的診斷資料。 其目的是要啟用診斷，或是記錄資料以便在需要進一步疑難排解時使用。 您可以啟動電腦管理應用程式，並展開 [系統工具]  、[效能]  、[資料收集器集合工具]  ，然後選取 [事件追蹤工作階段]  ，藉以尋找自動系統追蹤。

顯示於 [事件追蹤工作階段]  和 [啟動事件追蹤工作階段]  下方的部分追蹤無法停止，而且也不應停止。 其他追蹤 (例如 **WiFiSession**) 則可以停止。 若要停止執行中的追蹤，請在 [事件追蹤工作階段]  下方，以滑鼠右鍵按一下追蹤，然後選取 [停止]  。 若要防止在系統啟動時自動啟動追蹤，請遵循下列步驟：

1.  選取 [啟動事件追蹤工作階段]  資料夾

2.  找出您要處理的追蹤，然後按兩下該追蹤。

3.  選取 [追蹤工作階段]  索引標籤。

4.  清除標示為 [已啟用]  的方塊。

5.  選取 [確定]  。

以下是使用 VDI 時可考慮停用的一些系統追蹤：

| 名稱 | 註解 |
|--|--|
| AppModel | 一個追蹤集合，電話是其中之一 |
| CloudExperienceHostOOBE |  |
| DiagLog |  |
| NtfsLog |  |
| TileStore |  |
| UBPM |  |
| WiFiDriverIHVSession | 如果未使用 WiFi 裝置 |
| WiFiSession |  |

#### <a name="servicing-the-operating-system-and-apps"></a>維護作業系統和應用程式

在映像最佳化程序中的某個時間點，應該會套用可用的 Windows 更新。 您可以設定 Windows Update，以安裝其他 Microsoft 產品以及 Windows 的更新。 若要進行此設定，請開啟 [Windows 設定]  ，然後選取 [更新與安全性]  ，然後選取 [進階選項]  。 選取 [當我更新 Windows 時提供其他 Microsoft 產品的更新]  ，以將其設為 [開啟]  。

如果您要將 Microsoft Office 之類的 Microsoft 應用程式安裝到基底映像，這會是較理想的設定。 如此，當映像開始運作時，Office 就會處於最新狀態。 此外還有 .NET 更新，以及某些可透過 Windows Update 取得更新的非 Microsoft 元件 (例如 Adobe)。

安全性更新是非持續性 VDI VM 非常重要的考量之一，其中包括安全性軟體定義檔。 這些更新可能會以一天一次甚或更長的間隔發行。 您可以透過否些方法來保留這些更新，包括 Windows Defender 和非 Microsoft 元件。

就 Windows Defender 而言，建議即使在非持續性 VDI 中，也應允許更新執行。 更新幾乎在每個登入工作階段都會套用，但這類更新都很小，應該不會有問題。 此外，由於只會套用最新的可用更新，VM 的更新將不會過時。 非 Microsoft 定義檔可能也是如此。

> [!NOTE]
> 市集應用程式 (UWP 應用程式) 會透過 Windows 市集進行更新。 新版的 Office (例如 Microsoft 365) 在直接連線至網際網路時會透過其本身的機制進行更新，而在未連線至網際網路時，則會透過管理技術進行更新。

### <a name="windows-defender-optimization-with-vdi"></a>Windows Defender 對 VDI 的最佳化

Microsoft 近期發佈了關於在 VDI 環境中使用 Windows Defender 的文件。 如需詳細資訊，請參閱[在虛擬桌面基礎結構 (VDI) 環境中部署 Windows Defender 防毒軟體的指南](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)。

上述文章提供維護黃金 VDI 映像的程序，並說明如何維護執行中的 VDI 用戶端。 若要在 VDI 電腦需要更新其 Windows Defender 特徵碼時降低網路頻寬，請錯開重新開機的時間，並盡可能將重新開機排程在離峰時間。 Windows Defender 特徵碼的更新可包含在內部檔案共用上，且如果可行，請將這些檔案共用放在與 VDI 虛擬機器相同或接近的網路區段上。

若想進一步了解如何針對 VDI 將 Windows Defender 最佳化，請參閱本節開頭所列的文件。

### <a name="tuning-windows-10-network-performance-by-using-registry-settings"></a>使用登錄設定調整 Windows 10 網路效能

在 VDI 或實體電腦的工作負載多半以網路為基礎的環境中，這項調整格外重要。 此節中的設定會為目錄項目之類的項目設定額外的緩衝處理和快取，使效能側重於網路的層面。

請注意，本節中的某些設定是「僅以登錄為基礎的」  ，且應在映像部署至生產環境之前納入基底映像中。

下列設定值記載於 [Windows Server 2016 效能微調指導方針](../../administration/performance-tuning/index.md)資訊中，由 Windows 產品小組發佈於 Microsoft.com 上。

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DisableBandwidthThrottling

適用於 Windows 10。 預設值是 **0**。 根據預設，在某些情況下，SMB 重新導向器會對高延遲網路連線的輸送量進行節流，以避免網路相關逾時。 若將此登錄值設為 **1** 將會停用此節流，進而透過高延遲網路連接來達到更高的檔案轉送輸送量，因此在使用此設定前應先考量。

#### <a name="fileinfocacheentriesmax"></a>FileInfoCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileInfoCacheEntriesMax

適用於 Windows 10。 預設值是 **64**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案中繼資料數量。 提高此值可降低網路流量，並且在存取許多檔案時提升效能。 請嘗試將此值增加到 **1024**。

#### <a name="directorycacheentriesmax"></a>DirectoryCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DirectoryCacheEntriesMax

適用於 Windows 10。 預設值是 **16**，有效範圍是 1 至 4096。 此值用來決定用戶端可以快取的目錄資訊數量。 提高此值可降低網路流量，並且在存取大量目錄時提升效能。 請考慮將此值增加到 **1024**。

#### <a name="filenotfoundcacheentriesmax"></a>FileNotFoundCacheEntriesMax

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\FileNotFoundCacheEntriesMax

適用於 Windows 10。 預設值是 **128**，有效範圍是 1 至 65536。 此值用來決定用戶端可以快取的檔案名稱資訊數量。 提高此值可降低網路流量，並且在存取許多檔案名稱時提升效能。 請考慮將此值增加到 **2048**。

#### <a name="dormantfilelimit"></a>DormantFileLimit

HKLM\\System\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters\\DormantFileLimit

適用於 Windows 10。 預設值是 **1023**。 此參數可指定在應用程式關閉檔案之後，共用資源上應該保持開啟的檔案數目上限。 有數千個用戶端要連線至 SMB 伺服器時，請考慮將此值縮減至 **256**。

您可以使用 [Set-SmbClientConfiguration](/powershell/module/smbshare/set-smbclientconfiguration) 和 [Set-SmbServerConfiguration](/powershell/module/smbshare/set-smbserverconfiguration) Windows PowerShell Cmdlet 來設定其中許多 SMB 設定。 您也可以使用 Windows PowerShell 來設定僅限登錄的設定，如下列範例所示：

`Set-ItemProperty -Path "HKLM:\\SYSTEM\\CurrentControlSet\\Services\\LanmanWorkstation\\Parameters" RequireSecuritySignature -Value 0 -Force`

### <a name="additional-settings-from-the-windows-restricted-traffic-limited-functionality-baseline-guidance"></a>Windows 受限流量有限功能基準指引中的其他設定

Microsoft 已發行使用與 [Windows 安全性基準](/windows/device-security/windows-security-baselines)相同的程序建立的基準，適用於未直接連線至網際網路的環境，或不想將太多資料傳送至 Microsoft 和其他服務的環境。

[Windows 受限流量有限功能基準](/windows/privacy/manage-connections-from-windows-operating-system-components-to-microsoft-services)設定在群組原則表格中會以星號標示。

### <a name="disk-cleanup-including-using-the-disk-cleanup-wizard"></a>磁碟清理 (包括使用磁碟清理精靈)

磁碟清理對於主要映像 VDI 實作可能特別有幫助。 在主要映像備妥、更新並完成設定後，執行磁碟清理是最後應執行的工作之一。 內建於 Windows 的磁碟清理精靈有助於清理出最有可能節省磁碟空間的區域。

> [!NOTE]
> 磁碟清理精靈目前已停止開發。 Windows 將使用其他方法來提供磁碟清理功能。



以下是對針對各種磁碟清理工作提供的建議。 在實作其中任何一項工作前，應先進行下列測試：

1. 在套用所有更新後執行磁碟清理精靈 (提升權限)。 請納入**傳遞最佳化**和 **Windows Update 清理**類別。 您可以使用 **Cleanmgr.exe** 搭配 **/SAGESET:11** 選項將此程序自動化。 此選項會設定後續可用來自動執行磁碟清理的登錄值；屆時會使用磁碟清理精靈中的每個可用選項。

   1.  在測試 VM 上從全新安裝執行 **Cleanmgr.exe /SAGESET:11** 時，會顯示只有兩個依預設啟用的自動磁碟清理選項：

   - 下載的程式檔案

   - Temporary Internet Files

   2. 如果您設定更多選項或所有選項，這些選項會根據前一個命令 (**Cleanmgr.exe /SAGESET:11**) 中提供的索引值記錄在登錄中。 在此範例中，我們以值 *11* 作為索引，用於後續的自動磁碟清理程序。

   3. 執行 **Cleanmgr.exe /SAGESET:11** 後，您會看到許多類別的磁碟清理選項。 您可以選取每個選項，然後選取 [確定]  。 您會發現磁碟清理精靈隨即消失。 不過，您選取的設定會儲存在登錄中，並且可藉由執行 **Cleanmgr.exe /SAGERUN:11** 來叫用。

2. 如有任何使用中的磁碟區陰影複製儲存體，請加以清理。 為此，請在提升權限的命令提示字元中執行下列命令：

   - **vssadmin list shadows**

   - **vssadmin list shadowstorage**

       如果這些命令的輸出是「找不到符合查詢的項目。」  ，表示沒有任何使用中的 VSS 儲存體。

3. 清理暫存檔案和記錄。 在提升權限的命令提示字元中，執行下列命令：

   - **Del C:\\\*.tmp /s**

   - **Del C:\\Windows\\Temp\\.**

   - **Del %temp%\\.**

4. 使用下列命令，在系統上刪除任何未使用的設定檔：

   **wmic path win32_UserProfile where LocalPath="c:\\\\users\\\\\<user\>" Delete**

### <a name="remove-onedrive"></a>移除 OneDrive

要移除 OneDrive，必須要先移除套件、完成解除安裝，然後將 \*.lnk 檔案移除。 您可以利用下列範例 PowerShell 程式碼，從映像中移除 OneDrive:

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