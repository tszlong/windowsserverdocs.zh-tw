---
title: 針對虛擬桌面角色最佳化 Windows 10 (組建 2004)
description: 將當作 VDI 映像的 Windows 10 版本 2004 桌面的額外負荷降至最低的建議設定和組態。
ms.prod: windows-server
ms.reviewer: robsmi, timuessi
ms.technology: remote-desktop-services
author: Heidilohr
ms.author: helohr
ms.topic: article
manager: ''
ms.date: 09/24/2020
ms.openlocfilehash: 129bbc8387be655cca563935cb7ff891664da357
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155893"
---
# <a name="optimizing-windows-10-version-2004-for-a-virtual-desktop-infrastructure-vdi-role"></a>針對虛擬桌面基礎結構 (VDI) 角色將 Windows 10 版本 2004 最佳化

本文旨在提供 Windows 10 組建 2004 設定的建議，以在虛擬桌面環境 (包括虛擬桌面基礎結構 (VDI) 和 Windows 虛擬桌面) 中獲得最佳效能。 本指南中的所有設定都只是建議的最佳化設定，且絕不是需求。

本指南中的資訊與 Windows 10 版本 2004、作業系統 (OS) 組建 19041 有關。

在虛擬桌面環境中，將 Windows 10 效能最佳化的指導原則是盡可能減少圖形重新繪製和效果、減少對虛擬桌面環境沒有顯著效益的背景活動，以及將整體執行的處理序減少至最低限度。 次要目標是要將基底映像中的磁碟空間使用量降至最低限度。 透過虛擬桌面實作，可能的最小基底 (或「黃金」映像大小) 可稍微減少主機系統的記憶體使用率，並且可小幅減少將桌面環境提供給取用者所需的整體網路作業。

沒有最佳化應會減少使用者經驗。 每個最佳化設定都經過仔細審核，以確保使用者體驗不會明顯降低。

> [!NOTE]
> 本文中的設定可套用至其他 Windows 10 安裝，例如版本 1909、實體裝置或其他虛擬機器。 本文中沒有任何建議會影響虛擬桌面環境中 Windows 10 的可支援性。

## <a name="vdi-optimization-principles"></a>VDI 最佳化原則

「完整」虛擬桌面環境可透過網路為電腦使用者提供桌面工作階段，包括應用程式。 網路傳遞工具可以是內部部署網路、網際網路或兩者。 虛擬桌面環境的某些實作會使用「基底」作業系統映像，而此映像後續將成為桌面的基礎，供使用者工作之用。 虛擬桌面實作有多種變化，例如「持續性」、「非持續性」和「桌面工作階段」。 持續性類型會將虛擬桌面作業系統的變更保留到下一個工作階段。 非持續性類型則不會將虛擬桌面作業系統的變更保留到下一個工作階段。 對使用者而言，此桌面除了可透過網路存取以外，與其他虛擬或實體裝置相較也稍有不同。

最佳化設定會在參考電腦上進行。 虛擬機器 (VM) 是建置 VM 的理想位置，因為您可以在這裡儲存狀態、建立檢查點、進行備份等等。 預設 OS 安裝是對基礎 VM 執行。 該基礎 VM 接著會移除不需要的應用程式、安裝 Windows 更新、安裝其他更新、刪除暫存檔案、套用設定等等來進行最佳化。

還有其他類型的虛擬桌面技術，例如遠端桌面工作模式 (RDS) 和最近發行的 Microsoft Azure [Windows 虛擬桌面](https://azure.microsoft.com/services/virtual-desktop/)。 有關這些技術的深入討論已超出本文的範圍。 本文著重於 Windows 基礎映像設定，而不需參考環境中的其他因素，例如主機硬體最佳化。

對於其產品和服務，安全性和穩定性一向是 Microsoft 優先考慮項目。 在虛擬桌面領域中，安全性的處理方式與實體裝置極為不同。 企業客戶可以選擇利用 Windows 安全性的內建 Windows 服務，其中包含一套連線或未連線到網際網路時都能妥善運作的服務。 對於未連線到網際網路的虛擬桌面環境，系統每天可以主動下載安全性簽章數次，因為 Microsoft 可能每天會發行一個以上的簽章更新。 接著，您可以將這些簽章提供給虛擬桌面裝置，並排程在生產期間進行安裝，無論持續性或非持續性均適用。 如此一來，VM 保護就會盡可能維持在最新狀態。

有些安全性設定不適用於未連線到網際網路的虛擬桌面環境，因此無法參與具備雲端功能的安全性。 還有其他「正常」Windows 裝置可能會利用的各項設定，例如雲端體驗、Windows Store 等等。 移除未使用功能的存取，可減少使用量、降低網路頻寬和受到攻擊的可能性。

關於更新，Windows 10 會利用每月更新節奏。 在某些情況下，虛擬桌面管理員會透過關閉以「主要」或「黃金」映像為基礎的 VM 來控制更新的流程，將唯讀的映像解密、修補映像，然後重新密封並帶回生產環境。 因此，虛擬桌面裝置不需要檢查 Windows Update。 不過，在某些情況下會發生一般修補程序，例如持續性「個人」虛擬桌面裝置的案例。 在某些情況下，可以利用 Windows Update。 在某些情況下，可以利用 Intune。 在某些情況下，會使用 Microsoft Endpoint Configuration Manager (先前稱為 SCCM) 來處理更新和其他套件傳遞。 由每個組織決定更新虛擬桌面裝置的最佳方法，同時減少額外負荷週期。

本機原則設定以及本指南中的許多其他設定，都可以使用網域型原則覆寫。 建議仔細完成原則設定，並移除或不使用任何不想要或您的環境不適用的設定。 本文件中所列的設定會嘗試達到虛擬桌面環境中效能最佳化的最佳平衡，同時保持有品質的使用者體驗。

> [!NOTE]
> 在 GitHub.com 上可取得一組指令碼，將會執行本文記載的所有工作項目。 您可以在 [https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool](https://github.com/The-Virtual-Desktop-Team/Virtual-Desktop-Optimization-Tool) 找到最佳化指令碼的網際網路 URL。 此指令碼的設計訴求是可針對您的環境和需求輕鬆自訂。 主要程式碼是 PowerShell，並藉由呼叫輸入檔案 (純文字，現在為 .JSON) 以及本機群組原則物件 (LGPO) 工具匯出檔案來完成作業。 這些文字檔案包含要移除的應用程式清單、要停用的服務清單等等。 如果您不想移除特定的應用程式或停用特定的服務，可以編輯對應的文字檔，並移除您不想採取動作的項目。 最後，您可將匯出的本機原則設定匯入您的環境電腦。 建議您在基礎映像中使用某些設定，而不是透過群組原則套用設定；因為某些設定會在下次重新開機時生效，或在第一次使用元件時生效。

### <a name="persistent-virtual-desktop-environments"></a>持續性虛擬桌面環境

持續性虛擬桌面屬於基本層級，是會在重新啟動後仍保留作業系統狀態的裝置。 虛擬桌面解決方案的其他軟體層可讓使用者輕鬆且順暢地存取其指派的 VM (通常是使用單一登入解決方案)。

持續虛擬桌面有數種不同的實作。

- 傳統 VM 的 VM 具有其本身的虛擬磁碟檔案，可以正常啟動，並將一個工作階段的變更儲存到下一個工作階段。 其差別在於使用者存取此 VM 的方式。 有一個使用者登入的 Web 入口網站，會自動將使用者導向至指派給他們的一或多個虛擬桌面裝置 (VM)。
- 依需求可搭配個人虛擬磁碟 (PVD) 的映像式持續性 VM。 在此類型的實作中，一或多個主機伺服器上會有基底/黃金映像。 在 VM 建立後，會建立一或多個虛擬磁碟，並指派給此磁碟供持續性儲存之用。
  - 在 VM 啟動時，系統會將基礎映像的複本讀取到 VM 的記憶體空間中。 同時，持續性虛擬磁碟會連同透過複雜的程序合併的任何既有 OS 差異，指派給該 VM。
  - 事件記錄寫入、記錄寫入等之類的變更。 會重新導向至指派給該 VM 的讀取/寫入虛擬磁碟。
  - 在此情況下，作業系統和應用程式維護可使用傳統維護軟體 (例如 Windows Server Update Services) 或其他管理技術正常運作。
- 持續性虛擬桌面裝置和「一般」虛擬桌面裝置之間的差異在於主要/黃金映像的關係。 在某些時候，更新必須套用至主要映像。 此時，組織會決定如何處理使用者持續性變更。 在某些情況下，會捨棄或重設具有使用者變更的磁碟。 使用者對電腦所做的變更也可能透過每月品質更新保留，而基礎會在功能更新之後重設。

### <a name="non-persistent-virtual-desktop-environments"></a>非持續性虛擬桌面環境

非持續性虛擬桌面實作以基底或「黃金」映像為基礎時，最佳化主要會在基底映像上執行，其次才是透過本機設定與本機原則執行。

在映像式非持續性 (NP) 虛擬桌面環境中，基底映像是唯讀的。 在 NP 虛擬桌面裝置 (VM) 啟動時，系統會將基礎映像的複本串流處理至 VM。 在啟動期間及其後到下次重新開機之間所發生的活動，都會重新導向至暫存位置。 使用者通常會獲得用來儲存其資料的網路位置。 在某些情況下，使用者的設定檔會與標準 VM 合併，而為使用者提供其設定。

對於以單一映像為基礎的 NP 虛擬桌面而言，維護是其重要層面之一。 作業系統 (OS) 和 OS 元件的更新通常會每個月提供一次。 在映像式虛擬桌面環境中，要取得映像的更新必須執行一組程序：

- 在指定的主機上，該主機上所有以基底映像為基礎的 VM 都必須關閉電源或關閉。 這表示使用者會重新導向至其他 VM。

- 在某些實作中，這稱為「清空」。 當虛擬機器或工作階段主機設定為清空模式時，會停止接受新的要求，但繼續為目前連線到裝置的使用者提供服務。

- 在清空模式中，當最後一個使用者登出裝置時，該裝置就會準備好為作業提供服務。
- 隨後，基底映像會開啟並啟動。 接著就會執行所有的維護活動，例如作業系統更新、.NET 更新、應用程式更新等等。
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

非持續性虛擬桌面的挑戰之一，就是當使用者登出時，幾乎所有 OS 活動都會被捨棄。 使用者的設定檔和/或狀態可能會集中儲存到一個位置，但虛擬機器本身會捨棄前次開機後所做的絕大多數變更。 因此，預計要對可將狀態保留到下一個工作階段的 Windows 電腦進行的最佳化，將不適用。

根據虛擬桌面裝置的架構，PreFetch 和 SuperFetch 等功能都無法利用跨工作階段的持續性來提供協助，因為在 VM 重新啟動後，所有最佳化都會被捨棄。 編製索引可能會浪費資源，傳統磁碟重組之類的任何磁碟最佳化也是一樣。

> [!NOTE]
> 如果使用虛擬化來準備映像，而且在映像建立過程中連線到網際網路，則在第一次登入時，您應該前往 [設定] > [Windows Update] 來延遲功能更新。

### <a name="to-sysprep-or-not-sysprep"></a>是否使用 Sysprep

Windows 10 有一項名為[系統準備工具](/windows-hardware/manufacture/desktop/sysprep--system-preparation--overview)的內建功能 (也稱為 Sysprep)。 Sysprep 工具可用來為自訂的 Windows 10 映像進行複製準備。 Sysprep 程序可確保產生的 OS 會有適當的獨特性可在生產環境中執行。

執行 Sysprep 各有其優缺點。 以虛擬桌面環境為例，您可能會希望能夠自訂預設使用者設定檔，以供後續使用此映像登入的使用者作為設定檔範本。 您可能會想要安裝某些應用程式，但同時又希望能控制個別應用程式的設定。

替代方式是使用標準 .ISO 進行安裝；您可以搭配使用自動的安裝回應檔案，以及用來安裝應用程式或移除應用程式的工作序列。 您也可以使用工作序列來設定映像中的本機原則設定；或許可搭配使用[本機群組原則物件公用程式 (LGPO)](/archive/blogs/secguide/lgpo-exe-local-group-policy-object-utility-v1-0) 工具。

若要深入了解 Azure 的映像準備，請參閱[準備 Windows VHD 或 VHDX 以上傳至 Azure](/azure/virtual-machines/windows/prepare-for-upload-vhd-image)

### <a name="supportability"></a>可支援性

每當 Windows 預設值變更時，就會發生有關可支援性的問題。 自訂虛擬桌面映像 (VM 或工作階段) 之後，每次對映像所做的變更都必須在變更記錄檔中追蹤。 進行疑難排解時，通常會在集區中隔離映像並設定問題分析。 一旦追蹤到問題的根本原因之後，系統會先將該變更推到測試環境，最後再到生產工作負載。

本文件刻意避免觸及影響安全性的系統服務、原則或工作。 之後就是 Windows 服務。 已移除維護期間以外的虛擬桌面映像服務，因為維護期間會在虛擬桌面環境中進行大部分服務事件時發生，「除了」安全性軟體更新。 Microsoft 已針對虛擬桌面環境中的 Windows 安全性發佈指引，如下所示：

**Microsoft**：[在虛擬桌面基礎結構環境中部署 Windows Defender 防毒軟體的指南](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)

改變預設的 Windows 設定時，請考慮可支援性。 以強化、「加速」等名義更改系統服務、原則或排定的工作時，有時候會難以解決問題。 請參閱 Microsoft 知識庫，以取得有關更改預設設定的目前已知問題。 本文件中的指導方針以及 GitHub 上相關的程式碼，將會在已知問題發生時持續更新。 此外，您可以透過數種方式向 Microsoft 回報問題。

您可以使用慣用的搜尋引擎，使用 `"start value" site:support.microsoft.com` 一詞，提出有關服務預設起始值的已知問題。

您可能會注意到，本文件和 GitHub 上相關的指令碼並不會修改任何預設權限。 如果您想增加安全性設定，請從名為 **AaronLocker** 的專案開始。 如需詳細資訊，請參閱 [概觀](https://github.com/microsoft/AaronLocker)。

### <a name="virtual-desktop-optimization-categories"></a>虛擬桌面最佳化類別

- 通用 Windows 平台 (UWP) 應用程式清理
- 選用功能清理
- 本機原則設定
- 系統服務
- 排定的工作
- 套用 Windows (和其他) 更新
- 自動 Windows 追蹤
- Windows Defender 對 VDI 的最佳化
- 依登錄設定進行的用戶端網路效能微調
- 「Windows 受限流量有限功能基準」指引中的其他設定。
- [磁碟清理]

### <a name="universal-windows-platform-uwp-application-cleanup"></a>通用 Windows 平台 (UWP) 應用程式清除

虛擬桌面映像的目標之一是要讓持續性儲存體越精簡越好。 縮小映像大小的方式之一，是移除環境中不會使用的 UWP 應用程式。 使用 UWP 應用程式時，會有主要的應用程式檔案，也就是承載。 每個使用者設定檔中都會儲存少量的資料，供應用程式的特定設定使用。 此外，「所有使用者」設定檔中也會有少量的資料。

此外，所有 UWP 應用程式都會在裝置啟動和使用者登入後的某個時間點，於使用者或電腦層級進行註冊。 UWP 應用程式 (包括 [開始] 功能表和 Windows Shell) 會在安裝時或安裝後執行各種工作，以及在使用者第一次登入時再次執行工作，而在後續登入時執行較小程度的工作。 對所有 UWP 應用程式而言，偶爾會進行評估，例如：

- 您需要將應用程式更新為最新版本嗎？
- 應用程式 (若已釘選到 [開始] 功能表) 可能有要下載的動態磚資料
- 應用程式是否有需要更新的資料快取，例如地圖或氣象？
- 應用程式是否具有來自使用者設定檔且必須在登入時呈現的持續性資料 (例如，自黏便箋)

使用 Windows 10 的預設安裝，並非所有 UWP 應用程式都可供組織使用。 因此，如果移除這些應用程式，就需要進行較少的評估、較少的快取等等。 這裡的第二種方法是指示 Windows 停用「取用者體驗」。 此種方法藉由必須檢查每位使用者安裝的應用程式、可用的應用程式，然後開始下載一些 UWP 應用程式，以減少 Store 活動。 當有數百或數千個使用者、全部在大約相同的時間開始工作，或甚至在各時區的輪流時間開始工作時，效能節約就會很明顯。

就 UWP 應用程式清理而言，連線能力和時機是重要的因素。 如果您將基礎映像部署到沒有網路連線的裝置，在您嘗試將應用程式解除安裝時，Windows 10 將無法連線至 Microsoft Store 下載應用程式並嘗試加以安裝。 這項策略可讓您有時間自訂映像，然後在建立映像程序的後續階段中更新其餘的內容。

如果您修改用來安裝 Windows 10 的基底 .WIM，並在安裝之前從 .WIM 中移除了不必要的 UWP 應用程式，則這些應用程式將不會從頭開始安裝，而您後續的設定檔建立時間將會縮短。 本節稍後有一個連結，其中含有如何從安裝 .WIM 檔案中移除 UWP 應用程式的資訊。

就虛擬桌面環境而言，佈建您在基底映像中需要的應用程式，並在其後限制或封鎖對 Microsoft Store 的存取，是不錯的策略。 在一般的電腦上，市集應用程式會定期在背景更新。 UWP 應用程式可在維護期間，於系統套用其他更新時進行更新。

#### <a name="delete-the-payload-of-uwp-apps"></a>刪除 UWP 應用程式的承載

不需要的 UWP 應用程式仍會在檔案系統中耗用少量的磁碟空間。 針對永遠不會用到的應用程式，您可以使用 PowerShell 命令從基底映像中移除非必要 UWP 應用程式的承載。 如果您使用本節後續提供的連結從安裝 .WIM 檔案中刪除 UWP 應用程式，您可以非常精簡的 UWP 應用程式清單從頭開始。

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

佈建至系統的 UWP 應用程式可在作業系統安裝期間以工作序列移除，或是在作業系統安裝後移除。 後者可能是較理想的方法，因為它可將建立或維護映像的整體程序模組化。 在您開發指令碼後，如果某個項目在後續組建中有所變更，您可以編輯現有的指令碼，而非從頭重複相關程序。

如果您想要深入了解，以下是一些可協助您的資源：

- [在工作序列期間移除 Windows 10 內建應用程式](/archive/blogs/mniehaus/removing-windows-10-in-box-apps-during-a-task-sequence)
- [使用 PowerShell 1.3 版移除 Windows 10 WIM 檔案中的內建應用程式](https://gallery.technet.microsoft.com/Removing-Built-in-apps-65dc387b)
- [Windows 10 1607：使應用程式不會在部署功能更新時再次出現](https://blogs.technet.microsoft.com/mniehaus/2016/08/23/windows-10-1607-keeping-apps-from-coming-back-when-deploying-the-feature-update/)
- [在工作序列期間移除 Windows 10 內建應用程式](https://blogs.technet.microsoft.com/mniehaus/2015/11/11/removing-windows-10-in-box-apps-during-a-task-sequence/)

然後，執行下列 PowerShell 命令以移除 UWP 應用程式承載：

```powershell
Remove-AppxProvisionedPackage -Online - PackageName MyAppxPackage
```

本主題的最後注意事項：每個 UWP 應用程式都應該針對其在每個獨特環境中的適用性進行評估。 您可以安裝 Windows 10 2004 版的預設安裝，然後記下哪些應用程式正在執行且耗用記憶體。 例如，您可以考慮移除會自動啟動的應用程式，或是自動在 [開始] 功能表中顯示資訊 (例如氣象和新聞)、但在您的環境中可能不會用到的應用程式。

> [!NOTE]
> 如果使用來自 GitHub 的指令碼，您可以在執行指令碼之前，輕鬆地控制要移除哪些應用程式。 下載指令碼檔案之後，找出 AppxPackage.json 檔案並加以編輯，然後移除您想要保留的應用程式項目，例如計算機、自黏便箋等等。

## <a name="windows-optional-features-cleanup"></a>Windows 選用功能清理

### <a name="managing-optional-features-with-powershell"></a>使用 PowerShell 管理選用功能

Microsoft：[Windows 10：使用 PowerShell 管理選用功能](https://social.technet.microsoft.com/wiki/contents/articles/39386.windows-10-managing-optional-features-with-powershell.aspx)

您可以使用 PowerShell 管理 Windows 選用功能。 若要列舉目前已安裝的 Windows 功能，請執行下列 PowerShell 命令：

```powershell
Get-WindowsOptionalFeature -Online
```

使用 PowerShell，可以將列舉的 Windows 選用功能設定為已啟用或已停用，如下列範例所示：

```powershell
Enabled-WindowsOptionalFeature -Online -FeatureName "DirectPlay" -All
```

以下是在虛擬桌面映像中停用 Windows Media Player 功能的範例命令：

```powershell
Disable-WindowsOptionalFeature -Online -FeatureName "WindowsMediaPlayer"
```

接下來，建議您移除 Windows Media Player 套件。 此範例命令會示範如何執行此動作：

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

如果您想移除 Windows Media Player 套件 (以釋放大約 60 MB 的磁碟空間)，可以執行此命令：

```powershell
PS C:\Windows\system32> Remove-WindowsPackage -PackageName Microsoft-Windows-MediaPlayer-Package~31bf3856ad364e35~amd64~~10.0.19041.153 -Online

Path          :
Online        : True
RestartNeeded : False
```

### <a name="enable-of-disabling-windows-features-using-dism"></a>使用 DISM 能夠停用 Windows 功能

您可以使用內建的 Dism.exe 工具來列舉及控制 Windows 選用功能。 在作業系統安裝工作進行期間，可以開發並執行 Dism.exe 指令碼。 此處所涉及的 Windows 技術稱為功能隨選安裝。 請參閱下列文章，以深入了解 Windows 中的「功能隨選安裝」：

Microsoft：[功能隨選安裝](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)

### <a name="default-user-settings"></a>預設使用者設定

您可以在 `C:\Users\Default\NTUSER.DAT` 自訂 Windows 登錄檔。 對此檔案所做的任何設定變更，都會套用到從執行此映像的電腦建立的任何後續使用者設定檔。 您可編輯 **DefaultUserSettings.txt**檔案，以控制要套用至預設使用者設定檔的設定。

若要減少透過虛擬桌面基礎結構傳輸圖形資料的能力，您可以將預設背景設定為純色，而不是預設的 Windows 10 影像。 您也可以將登入畫面設定為純色，並在登入時關閉不透明的模糊效果。

下列設定會套用到預設的使用者設定檔登錄區，主要是為了減少動畫效果。 如果您不想進行部分或全部設定，請根據此影像刪除您不想套用至新使用者設定檔的設定。 這些設定的目標是要啟用下列對等設定：

- 在滑鼠指標下顯示陰影
- 在視窗下顯示陰影
- 平滑的畫面字型邊緣

![[效能選項] 功能表的螢幕擷取畫面，其中已選取相關項目。](media/performance-options.png)

而且，此設定版本的新功能是針對執行最佳化後所建立的任何使用者設定檔停用下列兩個隱私權設定的方法：

- 透過讓網站存取我的語言清單以提供當地相關內容
- 在「設定」應用程式中顯示建議的內容

![[隱私權設定] 視窗的螢幕擷取畫面。 這兩個已停用的設定會以紅色醒目提示。](media/privacy-settings.png)

以下是套用至預設使用者設定檔登錄區的最佳化設定，目的是讓效能最佳化。 請注意，此作業的執行方式是先載入預設使用者設定檔登錄 hive **NTUser.dat**，作為暫時金鑰名稱 **Temp**，然後進行以下所列的修改：

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

最近新增的另一系列預設使用者設定是讓數個 Windows 應用程式無法在背景中啟動並執行。 雖然在單一裝置上並不重要，Windows 10 會針對指定裝置 (主機) 上的每個使用者工作階段啟動一些程序。 Skype 應用程式是其中一個範例，而 Microsoft Edge 是另一個範例。 所包含的設定會關閉數個應用程式，使其無法在背景中執行。 如果希望這項功能保持原樣，只要刪除 "DefaultUserSettings.txt" 檔案中包含應用程式名稱 "**Windows.Photos**"、"**SkypeApp**"、"**YourPhone**"，及/或 "**MicrosoftEdge**" 的各行。

### <a name="local-policy-settings"></a>本機原則設定

虛擬桌面環境中的許多 Windows 10 最佳化都可使用 Windows 原則來進行。 本節的表格中所列的設定可套用至基礎/黃金映像。 然後若未以任何其他方式 (例如藉由群組原則) 指定等同的設定，就仍會套用這些設定。

請注意，某些決策可能取決於環境的詳細資訊。

- 虛擬桌面環境是否可存取網際網路？
- 虛擬桌面解決方案是持續性還是非持續性的？

下列設定確保不會與任何關係到安全性的設定發生牴觸或衝突。 我們選擇移除這些設定或停用可能不適用於虛擬桌面環境的功能。

| 原則設定 | 項目 | 子項目 | 可能的設定和註解 |
|--------------|----|--------|----------------------------|
| 本機電腦原則 \\ 電腦設定 \\ Windows 設定 \\ 安全性設定 | N/A | N/A | N/A |
| 網路清單管理員原則 | 所有網路內容 | 網路位置 | **使用者無法變更位置** (這已設定為在偵測到新的網路時，防止右手邊的快顯視窗) |
| 本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 控制台 | N/A | N/A |
| 控制台 | 允許線上秘訣 | N/A  | **已停用** (設定將不會聯繫 Microsoft 內容服務以擷取秘訣和說明內容) |
| 控制台 \ 個人化 | 強制使用特定的預設鎖定畫面和登入影像 | N/A | 已啟用 (此設定可讓您藉由輸入影像檔案的路徑 (位置) 來強制執行特定的預設鎖定畫面和登入映像。 相同的映像將會用於鎖定和登入畫面。 <p>這項建議的原因是要減少透過網路對虛擬桌面環境傳輸的位元組。 此設定可針對每個環境進行移除或自訂。)|
|控制台 \ 地區及語言選項 \ 手寫個人化|關閉自動學習| N/A |**已啟用** (啟用此原則設定後，自動學習會停止，並且刪除任何已儲存的資料。 使用者無法在控制台中設定此設定)|
|本機電腦原則 \\ 電腦設定 \\ 系統管理範本 \\ 網路|N/A|N/A|N/A|
|背景智慧型傳送服務 (BITS)|允許 BITS 對等快取| N/A |**已停用** (此原則設定可決定是否在特定電腦上啟用 [背景智慧型傳送服務 (BITS)] 對等快取功能。)|
|背景智慧型傳送服務 (BITS)|不允許 BITS 用戶端使用 Windows 分支快取|N/A|**已啟用** (啟用此原則設定後，BITS 用戶端不會使用 Windows 分支快取。)<p>這項建議的原因是為了讓虛擬桌面裝置不會用於內容快取，而且不允許裝置使用網路頻寬來執行這項操作。|
|背景智慧型傳送服務 (BITS)|不允許此電腦做為 BITS 對等快取用戶端|N/A|**已啟用** (啟用此原則設定後，電腦將不再使用 BITS 對等快取功能來下載檔案；只會從原始伺服器下載檔案。)|
背景智慧型傳送服務 (BITS)|不允許此電腦做為 BITS 對等快取伺服器|N/A|**已啟用** (啟用此原則設定後，電腦將不再快取已下載的檔案並將其提供給其對等項。)|
|BranchCache|開啟 BranchCache|N/A|**已停用** (停用此選取項目後，就會針對套用原則的所有用戶端電腦關閉 BranchCache。)|
|*字型|啟用字型提供者|N/A|**已停用** (停用此設定後，Windows 不會連線至線上字型提供者，而只會列舉本機安裝的字型。)|
|作用區驗證|啟用熱點驗證| N/A |**已停用** (此原則設定會定義是否針對無線網際網路服務提供者漫遊 (WISPr) 通訊協定支援探查 WLAN 熱點。 停用此原則設定後，就不會針對 WISPr 通訊協定支援探查 WLAN 熱點，且使用者只能使用網頁瀏覽器向 WLAN 熱點進行驗證。)|
|Microsoft 對等網路服務|關閉 Microsoft 對等網路服務|N/A|**已啟用** (此設定會關閉其整體的 Microsoft 對等網路服務，並會導致所有相依的應用程式停止運作。 如果您啟用此設定，將會關閉對等通訊協定。)|
|網路連線狀態指示器<p>(此區段中有其他設定可以在隔離的網路中使用)|指定被動輪詢|停用被動輪詢 (**核取方塊**)|**已啟用** (此原則設定可讓您指定被動輪詢行為。 NCSI 會定期輪詢整個網路堆疊中的各種測量，以判斷網路連線是否已中斷。 使用此選項來控制被動輪詢行為。)<P>停用 NCIS 被動輪詢可改善伺服器或網路連線為靜態的其他電腦上的 CPU 工作負載。|
|離線檔案|允許或禁止使用離線檔案功能|N/A|**已停用** (此原則設定可決定是否啟用離線檔案功能。 離線檔案會將網路檔案的複本儲存在使用者的電腦上，以便在電腦未連線到網路時使用。停用此原則設定後，離線檔案功能就會停用，且使用者無法加以啟用。)|
|*TCPIP 設定 \ IPv6 轉換技術| 設定 Teredo 狀態|已停用狀態|**已啟用** (啟用此設定後，然後設定為 [已停用狀態]，主機上不會顯示任何 Teredo 介面。)|
*WLAN 服務 \ WLAN 設定|允許 Windows 自動連線至建議的開放式熱點、連絡人共用的網路，及提供付費服務的熱點|N/A|**已停用** (此原則設定可決定使用者是否可以啟用下列 WLAN 設定：[連線到建議的開放式熱點]、[連線至我的連絡人提供共用的網路] 及 [啟用付費服務]。 停用此原則設定後，[連線至建議的開放式熱點]、[連線至我的連絡人共用的網路] 和 [啟用付費服務] 將會關閉，且此裝置上的使用者將無法加以啟用。)|
|WWAN 服務 \ 行動數據存取|允許 Windows 應用程式存取行動數據|所有應用程式的預設值：**強制拒絕**|**已啟用** (如果選擇 [強制拒絕] 選項，Windows 應用程式將無法存取行動數據，且使用者無法加以變更。)|
|本機電腦原則 \ 電腦設定 \ 系統管理範本 \ 開始功能表和工作列|N/A|N/A|
|*通知|關閉通知網路使用方式| N/A |**已啟用** (啟用此原則設定後，應用程式和系統功能將無法透過網路從 WNS 或透過通知輪詢 API 接收通知。)|
|本機電腦原則 \ 電腦設定 \ 系統管理範本 \ 系統| N/A | N/A |N/A|
|裝置安裝|當裝置上已安裝標準驅動程式時，不要傳送 Windows 錯誤報告| N/A |**已啟用** (啟用此原則設定後，安裝一般驅動程式時，不會傳送錯誤報告。)|
|裝置安裝|在通常會提示建立系統還原點的裝置活動期間，防止建立系統還原點| N/A |**已啟用** (啟用此原則設定後，Windows 不會建立通常會建立的系統還原點。)|
|裝置安裝|防止從網際網路擷取裝置中繼資料| N/A |**已啟用** (此原則設定可讓您防止 Windows 從網際網路擷取裝置中繼資料。 啟用此原則設定後，Windows 不會從網際網路擷取已安裝裝置的裝置中繼資料。 此原則設定會覆寫 [裝置安裝設定] 對話方塊中的設定 ([控制台] > [系統和安全性] > [系統] > [進階系統設定] > [硬體] 索引標籤)。)|
|裝置安裝|在裝置安裝期間關閉 [找到新硬體] 註解方塊| N/A |**已啟用** (此原則設定可讓您在裝置安裝期間關閉 [找到新硬體] 註解方塊。 啟用此原則設定後，在安裝裝置時，[找到新硬體] 註解方塊不會出現。)|
|檔案系統 \ NTFS|簡短名稱建立選項|簡短名稱建立選項：在所有磁碟區上停用|**已啟用** (這些設定可讓您控制在檔案建立期間是否產生簡短名稱。 有些應用程式需要簡短名稱以達到相容性，但簡短名稱對系統效能有負面影響。 在所有磁碟區上停用簡短名稱後，就永遠不會再產生。)|
|*群組原則|在此裝置上繼續體驗| N/A |**已停用** (此原則設定可決定是否允許 Windows 裝置參與跨裝置體驗 (繼續體驗)。 停用此原則可防止其他裝置探索此裝置，因此無法參與跨裝置體驗。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉事件檢視器 "Events.asp" 連結| N/A |**已啟用** (此原則設定可指定事件檢視器應用程式內的事件是否有 "Events.asp" 超連結。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉個人化手寫資料共用| N/A |**已啟用** (關閉手寫辨識個人化工具的資料共用。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉手寫辨識錯誤報告| N/A |**已啟用** (關閉手寫辨識錯誤報告工具。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉說明及支援中心 Microsoft 知識庫搜尋| N/A |**已啟用** (此原則設定會指定使用者是否可以從 [說明及支援中心] 執行 Microsoft 知識庫搜尋。)|
|網際網路通訊管理 \ 網際網路通訊設定|如果 URL 連線正在參照 Microsoft.com 時，關閉網際網路連線精靈| N/A |**已啟用** (此原則設定可指定 Internet Connection Wizard 是否可以連線到 Microsoft，以下載網際網路服務提供者 (ISP) 的清單。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉網頁發佈和線上訂購精靈的網際網路下載| N/A |**已啟用** (此原則設定可指定 Windows 是否應該為網頁發佈和線上訂購精靈下載提供者清單。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉網際網路檔案關聯服務| N/A |**已啟用** (此原則設定可指定是否要使用 Microsoft Web 服務來尋找應用程式，以開啟具有未處理檔案關聯的檔案。)|
|網際網路通訊管理 \ 網際網路通訊設定|如果 URL 連線正在參照 Microsoft.com 時，關閉註冊| N/A |**已啟用** (此原則設定可指定 Windows 註冊精靈是否連線至 Microsoft.com 以進行線上註冊。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉搜尋小幫手內容檔更新| N/A |**已啟用** (此原則設定可指定「搜尋小幫手」是否應在本機和網際網路搜尋期間自動下載內容更新。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉「訂購沖印」圖片工作| N/A |**已啟用** (如果啟用此原則設定，則會從 [檔案總管] 資料夾中的 [圖片工作] 中移除 [線上訂購沖印] 工作。)|
網際網路通訊管理 \ 網際網路通訊設定|關閉檔案及資料夾的「發佈到網站」工作| N/A |**已啟用* (此原則設定可指定 Windows 資料夾的 [檔案和資料夾工作] 是否有 [將此檔案發佈到網站]、[將此資料夾發佈到網站] 及 [將選取項目發佈到網站] 工作。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉 Windows 客戶經驗改進計畫| N/A |**已啟用** (Windows 客戶經驗改進計畫(CEIP)會收集有關您硬體設定、您如何使用我們的軟體和服務來識別趨勢和使用模式的資訊。 如果您啟用此原則設定，所有使用者都不能加入 Windows CEIP。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉 Windows 錯誤報告| N/A |**已啟用** (此原則設定可控制是否將錯誤回報給 Microsoft。 如果啟用此原則設定，則不會為使用者提供報告錯誤的選項。)|
|網際網路通訊管理 \ 網際網路通訊設定|關閉 Windows Update 裝置驅動程式搜尋| N/A |**已啟用** (此原則設定可指定當本機沒有任何裝置驅動程式時，Windows 是否要在 Windows Update 搜尋裝置驅動程式。 如果啟用此原則設定，則在安裝新裝置時不會搜尋 Windows Update。)|
|登入|登入時不顯示開始使用歡迎畫面| N/A |**已啟用** (啟用此設定後，使用者登入 Windows 裝置時會隱藏歡迎畫面。)|
|登入|不列舉已加入網域電腦上的連線使用者| N/A |**已啟用** (啟用此設定後，登入 UI 將不會列舉已加入網域電腦上的任何連線使用者。)|
|登入|列舉已加入網域電腦上的本機使用者| N/A |**已停用** (停用此設定後，登入 UI 將不會列舉已加入網域電腦上的本機使用者。)|
|登入|顯示乾淨的登入背景| N/A |**已啟用** (此原則設定可停用登入背景影像的壓克力模糊效果。 啟用此設定後，登入背景影像就會顯示，而不會模糊。)|
|登入|顯示首次登入動畫| N/A |**已停用** (此原則設定可讓您控制使用者在第一次登入電腦時是否看到第一個登入動畫。 這同時適用於完成初始設定之電腦的第一個使用者，以及稍後新增到電腦的使用者。 其也可控制是否要在 Microsoft 帳戶使用者第一次登入時提供服務的選擇加入提示。<p>停用此設定後，使用者不會看到第一次登入動畫，且 Microsoft 帳戶使用者不會看到服務的選擇加入提示。)|
|登入|關閉鎖定畫面上的應用程式通知| N/A |**已啟用** (此原則設定可讓您防止應用程式通知出現在鎖定畫面上。 啟用此設定後，鎖定畫面上不會顯示任何應用程式通知。)|
|電源管理|選取使用中電源計劃|使用中電源計劃：高效能|**已啟用** (如果啟用此原則設定，請指定 [使用中電源計劃] 清單中的電源計劃。) <p>停用「電源」服務後，Powercfg.cpl UI 無法顯示這些電源選項，而是改為傳回 RPC 錯誤。|
|電源管理 \ 影片和顯示設定|開啟桌面背景投影片 (一般電源)| N/A |**已停用** (此原則設定可讓您指定 Windows 是否應該啟用桌面背景投影片。)停用此設定後，就會停用桌面背景投影片。 此設定可能不會影響 VM。|
|修復|允許將系統還原到預設狀態| N/A |**已停用** (停用此設定後，將無法使用 [復原] (位於 [控制台] 內) 中的 [使用您稍早建立的系統映像來復原您的電腦] 和 [重新安裝 Windows] (或 [將您的電腦回復到原廠條件]) 項目。)|
|*儲存空間健全狀況|允許下載磁碟失敗預測模型的更新| N/A |**已停用** (不會下載磁碟失敗預測模型的更新)|
|[系統還原]|關閉系統還原| N/A |**已啟用** (啟用此設定後，就會關閉系統還原，且無法存取系統還原精靈。 也會停用透過 [系統保護] 設定 [系統還原] 或建立還原點的選項。)|
|疑難排解與診斷 \ 排程維護|設定排程維護行為| N/A |**已停用** (決定是否執行排程診斷，以主動偵測並解決系統問題。 停用此原則設定後，Windows 將無法依照排程偵測、疑難排解或解決問題。)|
|疑難排解與診斷 \ 指令碼診斷|疑難排解：可讓使用者存取和執行疑難排解精靈| N/A |**已停用** (停用此設定後，使用者就無法從 [控制台] 存取或執行疑難排解工具。)|
|疑難排解與診斷 \ 指令碼診斷|疑難排解：允許使用者從 [疑難排解] 控制台 (透過 Windows 線上疑難排解服務 – WOTS) 存取 Microsoft 伺服器上的線上疑難排解內容| N/A |**已停用** (停用此設定後，使用者只能存取和搜尋其電腦本機可用的疑難排解內容，即使已連線到網際網路也一樣。 他們無法連線到裝載 Windows 線上疑難排解服務的 Microsoft 伺服器。|
|疑難排解與診斷 \ Windows 開機效能診斷|設定狀況執行層級| N/A |**已停用** (決定 Windows 開機效能診斷的執行層級。 如果您停用此原則設定，Windows 將無法偵測、疑難排解或解決 DPS 所處理的任何 Windows 開機效能問題。)<p>在設計、測試、開發或維護階段，此設定可能非常有用。 此設定可以在隔離的 VM 或工作階段主機上、採用的度量，以及記載於 "Microsoft-Windows-Diagnostics-Performance/Operational" 來源下事件記錄中的結果上啟用：診斷-效能、工作類別「開機效能監視」。<p>**此外**：停用 DPS 服務後，此設定不會有任何作用，因為 Windows 將不會記錄效能資料。|
|疑難排解與診斷 \ Windows 記憶體流失診斷|設定狀況執行層級| N/A |**已停用** (此原則設定會判斷診斷原則服務 (DPS) 是否診斷記憶體流失問題。 停用此設定後，DPS 就無法診斷記憶體流失問題。) <p>許多診斷模式都可加以啟用，而使用的工具 (例如 WPT) (雖然通常在開發/測試/維護案例中進行) 不會在生產 VM 或工作階段上啟用及使用。|
|疑難排解與診斷 \ Windows 效能 PerfTrack|啟用/停用 PerfTrack| N/A |**已停用** (此原則設定可指定要啟用或停用回應性事件的追蹤。 停用此設定後，就不會處理回應性事件。)|
|疑難排解與診斷 \ Windows 資源耗損偵測與解析|設定狀況執行層級| N/A |**已停用** (判斷 Windows 資源耗損偵測與解析的執行層級。 停用此設定後，Windows 將無法偵測、疑難排解或解決 DPS 所處理的任何 Windows 資源耗損問題。)|
|疑難排解與診斷 \ Windows 關機效能診斷|設定狀況執行層級| N/A |**已停用** (決定 Windows 關機效能診斷的執行層級。 停用此設定後，Windows 將無法偵測、疑難排解或解決 DPS 所處理的任何 Windows 關機效能問題。)|
|疑難排解與診斷 \ Windows 待命/繼續效能診斷|設定狀況執行層級| N/A |**已停用** (決定 Windows 待命/繼續效能診斷的執行層級。 停用此設定後，Windows 將無法偵測、疑難排解或解決 DPS 所處理的任何 Windows 待命/繼續效能問題。)|
|疑難排解與診斷 \ Windows 系統回應效能診斷|設定狀況執行層級| N/A |**已停用** (決定 Windows 系統回應性診斷的執行層級。 停用此設定後，Windows 將無法偵測、疑難排解或解決 DPS 所處理的任何 Windows 系統回應性問題。)|
*使用者設定檔|關閉廣告識別碼| N/A |**已啟用** (啟用此設定後，就會關閉廣告識別碼。 應用程式無法跨應用程式使用識別碼。)|
|本機電腦原則 \ 電腦設定 \ 系統管理範本 \ Windows 元件| N/A | N/A | N/A |
|*應用程式隱私權|讓 Windows 應用程式存取其他應用程式的診斷資訊|所有應用程式的預設值：強制拒絕|**已啟用** (啟用此設定後，使用 [強制拒絕] 選項，不允許 Windows 應用程式取得有關組織中其他應用程式和員工的診斷資訊，且無法加以變更。)|
|*應用程式隱私權|讓 Windows App 存取位置|所有應用程式的預設值：強制拒絕|**已啟用** (啟用此設定後，使用 [強制拒絕] 選項時，不允許 Windows 應用程式存取位置，且使用者無法變更此設定。)|
|*應用程式隱私權|讓 Windows 應用程式存取動作資料|所有應用程式的預設值：強制拒絕|**已啟用** (啟用此設定後，使用 [強制拒絕] 選項時，不允許 Windows 應用程式存取動作資料，且使用者無法變更此設定。)|
|*應用程式隱私權|讓 Windows 應用程式存取通知|所有應用程式的預設值：強制拒絕|**已啟用** (啟用此設定後，使用 [強制拒絕] 選項時，不允許 Windows 應用程式存取通知，且使用者無法變更此設定。)|
|*應用程式隱私權|允許使用語音啟動 Windows 應用程式|所有應用程式的預設值：強制拒絕|**已啟用** (此原則設定指定是否可以使用語音啟動 Windows 應用程式。)|
|*應用程式隱私權|允許在系統鎖定時使用語音啟動 Windows 應用程式|所有應用程式的預設值：強制拒絕|**已啟用** (此原則設定指定當系統鎖定時，是否可以透過語音啟動 Windows 應用程式。)|
|*應用程式隱私權|讓 Windows App 控制無線通訊|所有應用程式的預設值：強制拒絕|**已啟用** (如果選擇 [強制拒絕] 選項，Windows 應用程式將無權控制無線通訊，且您組織中的員工無法加以變更。)|
|應用程式相容性|關閉清查收集器| N/A |**已啟用** (此原則設定可控制清查收集器的狀態。 清查收集器會清查系統上的應用程式、檔案、裝置和驅動程式，並將資訊傳送給 Microsoft。 啟用此原則設定後，清查收集器將會關閉且不會將資料傳送給 Microsoft。 透過 [程式相容性助理] 的安裝資料收集也會停用。)|
|自動播放原則|設定 AutoRun 的預設行為|不執行任何 AutoRun 命令|**已啟用** (此原則設定會設定自動啟動命令的預設行為。)|
|*自動播放原則|關閉自動播放|所有磁碟機|**已啟用** (如果啟用此原則設定，所有磁碟機都會停用自動播放。)|
|*雲端內容|不顯示 Windows 祕訣| N/A |**已啟用** (此原則設定可防止對使用者顯示 Windows 秘訣。)|
|*雲端內容|關閉 Microsoft 消費者體驗| N/A |**已啟用** (啟用此原則設定後，使用者將不會再看到 Microsoft 提供的個人化建議，和其 Microsoft 帳戶的相關通知。)|
|*資料收集與預覽組建|允許遙測|0 – 安全性 [僅限企業版]|**已啟用** (設定值為 0 時，只會套用至執行企業版、教育版、IoT 版或 Windows Server 版的裝置，以及減少傳送給所支援最基本層級的遙測。)|
|資料收集與預覽版|針對電腦分析設定瀏覽資料的收集|設定遙測收集：不允許傳送內部網路或網際網路歷程記錄|**已啟用** (您可以將 Microsoft Edge 設定為僅傳送內部網路歷程記錄、僅傳送網際網路歷程記錄，或針對具有已設定商業識別碼的企業裝置將兩者傳送至電腦分析。 若已停用或未設定，Microsoft Edge 就不會將瀏覽歷程記錄資料傳送至電腦分析。)|
|*資料收集與預覽組建|不顯示意見反應通知| N/A |**已啟用** (此原則設定可讓組織防止其裝置顯示來自 Microsoft 的意見反應問題。)|
|傳遞最佳化|下載模式|下載模式：簡單 (99)|**已啟用** (99 = 沒有對等傳輸的簡單下載模式。 傳遞最佳化只會使用 HTTP 進行下載，且不會嘗試連線至傳遞最佳化雲端服務。)|
|桌面視窗管理員|不允許視窗動畫| N/A |**已啟用** (此原則設定可控制視窗動畫的外觀，例如在還原、最小化和最大化視窗時找到的動畫。 啟用此原則設定後，就會關閉視窗動畫。)|
|桌面視窗管理員|使用純色做為開始背景| N/A |**已啟用** (此原則設定可控制 [開始] 背景視覺效果。 啟用此原則設定後，[開始] 背景會使用純色。)|
|邊緣 UI|允許邊緣撥動| N/A |**已停用** (如果您停用此原則設定，使用者將無法從任何螢幕邊緣撥入來叫用任何系統 UI。)|
|邊緣 UI|停用說明秘訣| N/A |**已啟用** (如果已啟用此設定，Windows 將不會對使用者顯示任何說明提示。)|
|檔案總管|不顯示「已安裝新應用程式」通知| N/A |**已啟用** (此原則會移除新應用程式關聯的終端使用者通知。 這些關聯是以檔案類型 (例如，TXT 檔案) 或通訊協定 (例如 HTTP) 為基礎。 如果已啟用此原則，則不會向使用者顯示任何通知)|
|檔案歷程記錄|關閉檔案歷程記錄| N/A |**已啟用** (啟用此原則設定後，就無法啟用檔案歷程記錄來建立一般的自動備份。)|
|*尋找我的裝置|開啟/關閉尋找我的裝置| N/A |**已停用** (當 [尋找我的裝置] 關閉時，將不會註冊裝置及其位置，且 [尋找我的裝置] 功能將無法運作。 使用者也無法在其裝置上檢視其作用中的數位板最後一次使用的位置。)|
|家用群組|防止電腦加入家用群組| N/A |**已啟用** (如果您啟用此原則設定，使用者就無法將電腦新增至家庭群組。 此原則設定不會影響其他網路共用功能。)|
|*Internet Explorer|當使用者在網址列輸入時，允許 Microsoft 服務提供加強式建議| N/A |**已停用** (當使用者在網址列輸入時將不會收到加強式建議。 此外，使用者將無法變更 [建議] 設定。)|
|Internet Explorer|停用定期檢查以進行 Internet Explorer 軟體更新| N/A |**已啟用** (防止 Internet Explorer 檢查是否有新版的瀏覽器可用。)|
|Internet Explorer|停用顯示啟動顯示畫面的功能| N/A |**已啟用** (當使用者啟動瀏覽器時，防止 Internet Explorer 啟動顯示畫面出現。)|
|Internet Explorer|自動安裝新版 Internet Explorer| N/A |**已停用** (此原則設定會將 Internet Explorer 設為有新版的 Internet Explorer 可用時自動進行安裝。)|
|Internet Explorer|防止參與客戶經驗改進計畫| N/A |**已啟用** (此原則設定讓使用者無法參與客戶經驗改進計畫 (CEIP)。)|
|Internet Explorer|防止執行 [首次執行] 精靈|直接前往首頁|**已啟用** (此原則設定會在安裝 Internet Explorer 或 Windows 之後，使用者第一次啟動瀏覽器時，阻止 Internet Explorer 執行「初次執行」精靈。)|
|Internet Explorer|設定索引標籤程序成長速率|低|**已啟用** (此原則設定可讓您設定 Internet Explorer 建立新索引標籤程序的速率。)|
|Internet Explorer|關閉附加元件效能通知| N/A |**已啟用** (此原則設定會在載入所有使用者已啟用附加元件的平均時間超過閾值時，防止 Internet Explorer 顯示通知。)|
|Internet Explorer|關閉自動損毀復原| N/A |**已啟用** (此原則設定會關閉自動損毀復原。 啟用此原則設定後，自動損毀復原不會在程式停止回應後提示使用者復原其資料。)|
|*Internet Explorer|關閉瀏覽器地理位置| N/A |**已啟用** (啟用此原則設定後，就會關閉瀏覽器地理位置支援。)|
|*Internet Explorer|開啟建議的網站| N/A |**已停用** (停用此原則設定後，就會關閉與此功能相關聯的進入點和功能。)|
|Internet Explorer \ 網際網路控制台 \ 進階頁面|播放網頁動畫| N/A |**已停用** (此原則設定可讓您管理 Internet Explorer 是否會顯示在 Web 內容中找到的動畫圖片。 通常只有動畫 GIF 檔案會受此設定影響；作用中 Web 內容 (例如 java applet) 則不受影響。)|
|Internet Explorer \ 網際網路控制台 \ 進階頁面|播放網頁音效| N/A |**已停用** (停用此原則設定後，Internet Explorer 將不會播放或下載 Web 內容中的音效，有助於更快速顯示頁面。)|
|Internet Explorer \ 網際網路控制台 \ 進階頁面|播放網頁影片| N/A |**已停用** (如果停用此原則設定，Internet Explorer 將不會播放或下載影片，有助於更快速顯示頁面。)|
|Internet Explorer \ 網際網路控制台 \ 進階頁面|關閉在背景載入網站和內容以最佳化效能| N/A |**已停用**(啟用此原則設定後，IE 不會在背景中載入任何網站或內容。)|
|Internet Explorer \ 網際網路控制台 \ 進階頁面|關閉快速翻頁和頁面預測功能| N/A |**已啟用** (Microsoft 會收集您的瀏覽歷程記錄來改善快速翻頁與頁面預測的運作方式。 如果啟用這個原則設定，則會關閉快速翻頁與頁面預測，而且背景中不會載入下一個網頁。)|
|Internet Explorer \ 網際網路設定 \ 進階設定 \ 瀏覽|關閉電話號碼偵測| N/A |**已啟用** (此原則設定可決定是否辨識電話號碼，並將它轉換成超連結，用來叫用系統上的預設電話應用程式。 如果停用這個原則設定，將會啟用電話號碼偵測。 使用者將無法修改此設定。)|
|Internet Information Services|防止安裝 IIS| N/A |**已啟用** (啟用此原則設定後，無法安裝 IIS，而且無法安裝需要 IIS 的 Windows 元件或應用程式。)|
|*定位和感應器|關閉定位| N/A |**已啟用** (啟用此原則設定後，就會關閉定位功能，而且此裝置上的所有程式都無法使用定位功能的位置資訊。)|
|定位和感應器|關閉感應器| N/A |**已啟用** (此原則設定會關閉此裝置的感應器功能。 啟用此原則設定後，就會關閉感應器功能，而且這部電腦上的所有程式都無法使用感應器功能。)|
|定位和感應器 / Windows 定位提供者|關閉 Windows 定位提供者| N/A |**已啟用** (此原則設定會關閉此裝置的 Windows 位置提供者功能。)|
|*地圖|關閉自動下載與更新地圖資料| N/A |**已啟用** (啟用此設定後，就會關閉地圖資料的自動下載和更新。)|
|*地圖|關閉 [離線地圖] 設定頁面上非要求的網路流量| N/A |**已啟用** (啟用此設定後，就會關閉 [離線地圖] 設定頁面上產生網路流量的功能。 注意：這可能會關閉整個設定頁面。)|
|*訊息中心|允許訊息服務雲端同步| N/A |**已停用** (此原則設定可讓您將行動電話簡訊備份和還原至 Microsoft 的雲端服務。)|
|*Microsoft Edge|允許書籍媒體櫃的設定更新| N/A |**已停用** (停用此設定後，Microsoft Edge 不會自動下載書籍媒體櫃的更新設定資料。)|
|*Microsoft Edge|允許 [書籍] 索引標籤的延伸遙測| N/A |**已停用** (停用此設定後，視您的裝置設定而定，Microsoft Edge 只會傳送基本遙測資料。)|
|Microsoft Edge|允許 Microsoft Edge 在 Windows 啟動時、系統閒置時，以及每次 Microsoft Edge 關閉時預先啟動|設定預先啟動：防止預先啟動|**已啟用** (啟用此設定並設定為防止預先啟動後，Microsoft Edge 就不會在 Windows 登入、系統閒置或每次關閉 Microsoft Edge 時預先啟動。)|
|Microsoft Edge|允許 Microsoft Edge 在 Windows 啟動時啟動及每次關閉 Microsoft Edge 時載入 [開始] 和 [新索引標籤] 頁面|設定索引標籤預先載入：防止索引標籤預先載入|**已啟用** (此原則設定可讓您決定 Microsoft Edge 是否可以在 Windows 登入期間和每次關閉 Microsoft Edge 時，載入 [開始] 和 [新索引標籤] 頁面。 根據預設，此設定會允許預先載入。 停用預先載入後，Microsoft Edge 就不會在 Windows 登入期間和每次 Microsoft Edge 關閉時載入 [開始] 或 [新索引標籤] 頁面。)|
|Microsoft Edge|允許在新索引標籤上顯示網頁內容| N/A |**已停用**(停用此設定後，Edge 會使用空白頁面開啟新索引標籤。 如果已設定此設定，使用者就無法變更此設定。)|
|*Microsoft Edge|防止 Microsoft Edge 開啟 [首次執行] 網頁| N/A |**已啟用** (使用者在初次開啟 Microsoft Edge 時不會看見 [首次執行] 頁面。)|
|OneDrive|防止 OneDrive 產生網路流量直到使用者登入 OneDrive| N/A |**已啟用** (啟用此設定可防止 OneDrive 同步用戶端 (OneDrive.exe) 產生網路流量 (檢查更新等)，直到使用者登入 OneDrive 或開始將檔案同步處理至本機電腦。)|
|線上協助|關閉作用中的說明| N/A |**已啟用** (啟用此原則設定後，不會轉譯作用中內容連結。 會顯示文字，但沒有這些元素的可點選連結。)|
|OOBE|不要啟動使用者登入時的隱私權設定體驗| N/A |**已啟用** (在某些情況下，第一次或在升級後登入新的使用者帳戶時，該使用者可能會看到一個畫面或一系列畫面，提示使用者選擇其帳戶的隱私權設定。 啟用此原則以避免啟動此體驗。)|
|RSS 摘要|防止自動探索摘要和網頁快訊| N/A |**已啟用** (此原則設定可防止使用者讓 Internet Explorer 自動探索摘要或網頁快訊是否適用於相關聯的網頁。)|
|*RSS 摘要|關閉摘要和網頁快訊的背景同步處理| N/A |**已啟用** (啟用此原則設定後，就會停用在背景中同步處理摘要和網頁快訊的功能。)|
|*搜尋|允許 Cortana| N/A |**已停用** (此原則設定指定是否允許在裝置上使用 Cortana。 當 Cortana 已關閉時，使用者仍可使用搜尋功能尋找裝置上的項目。)|
|搜尋|允許在鎖定畫面上使用 Cortana| N/A |**已停用** (此原則設定決定使用者是否可在鎖定系統期間，使用語音與 Cortana 互動。)|
|*搜尋|允許搜尋和 Cortana 使用定位| N/A |**已停用** (此原則設定指定搜尋與 Cortana 是否可以提供定位感知搜尋和 Cortana 結果。)|
|搜尋|控制附件的豐富預覽|控制附件的豐富預覽： **.docx;.xlsx;.txt;.xls**|**已啟用** (啟用此原則可定義以分號分隔的副檔名清單，而讓這些副檔名能有豐富的附件預覽。)<p>**注意**：此設定可用來限制預覽的附件類型，這也有助於防止自動預覽一些可能有危險的內容類型。|
|搜尋|不允許 Web 搜尋| N/A |**已啟用** (啟用此原則可移除從 Windows 桌面搜尋來搜尋 Web 的選項。)|
|*搜尋|不要在 [搜尋] 中搜尋網路或顯示網路搜尋結果| N/A |**已啟用** (啟用此原則設定後，就不會在 Web 上執行查詢，且在使用者使用 [搜尋] 執行查詢時將不會顯示 Web 結果。)|
|搜尋|啟用對未快取的 Exchange 資料夾編製索引| N/A |**已停用** (啟用此原則可在 Microsoft Outlook 不是以快取模式執行時，允許在 Microsoft Exchange Server 上編製郵件項目的索引。 搜尋的預設行為是不要對未快取的 Exchange 資料夾編製索引。 停用此原則將會封鎖未快取 Exchange 資料夾的任何索引。)|
|搜尋|防止對離線檔案快取中的檔案編製索引| N/A |**已啟用** (若已啟用，則不會對離線提供的網路共用上的檔案編製索引。 否則會編製索引。 預設為停用。)|
|*搜尋|設定 [搜尋] 中可以分享的資訊|匿名資訊|**已啟用** (匿名資訊：共用使用資訊，但不共用搜尋歷程記錄、Microsoft 帳戶資訊或特定位置。)|
|搜尋|在硬碟空間有限的情況中停止編製索引|MB 限制：**5000**|**已啟用** (啟用此原則後，在與索引位置相同的磁碟機之剩餘硬碟空間低於指定的數量時，可防止繼續編製索引。 請選取 0 到 2147483647 MB 之間的值。)|
|軟體保護平台|關閉 KMS 用戶端線上 AVS 驗證| N/A |**已啟用** (啟用此設定後，裝置不會將其啟用狀態相關資料傳送至 Microsoft)|
|*語音|允許自動更新語音資料| N/A |**已停用** (指定裝置是否會收到語音辨識和語音合成模型的更新。)|
|市集|關閉更新至最新版 Windows 的服務| N/A |**已啟用** (啟用或停用 Store 供應項目，以更新為最新版的 Windows。 如果您啟用此設定，Store 應用程式將不會提供最新版 Windows 的更新。)|
|文字輸入|改進筆跡及輸入辨識| N/A |**已停用** (此原則設定可控制傳送筆跡和鍵入資料給 Microsoft 的功能，以改善 Windows 上執行之應用程式和服務的語言辨識和建議功能。)|
|Windows 錯誤報告|停用 Windows 錯誤報告| N/A |**已啟用** (啟用此原則設定後，Windows 錯誤報告不會將任何問題資訊傳送至 Microsoft。 此外，在 [控制台] 的 [安全性與維護] 中不提供解決方案資訊。)|
|Windows 遊戲錄影和廣播|啟用或停用 Windows 遊戲錄影和廣播| N/A |**已停用** (停用此設定後，將不允許 Windows 遊戲錄影。)|
|Windows Ink 工作區|允許 Windows Ink 工作區|選擇下列其中一個動作：停用|**已啟用** (啟用此設定並將子設定設為 [已停用] 時，無法使用 Windows Ink 工作區功能。)|
|Windows Installer|控制基準檔案快取的大小上限|5|**已啟用** (此原則可控制 Windows Installer 基準檔案快取可用的磁碟空間百分比。 啟用此原則設定後，您可以修改 Windows Installer 基準檔案快取的大小上限。)|
|Windows Installer|關閉系統還原檢查點建立功能| N/A |**已啟用** (啟用此原則設定後，Windows Installer 不會在安裝應用程式時產生系統還原檢查點。)|
|Windows 行動中心|關閉 Windows 行動中心| N/A |**已啟用** (啟用此原則設定後，使用者就無法叫用 Windows 行動中心。 Windows 行動中心 UI 會從所有 Shell 進入點移除，且 .exe 檔案不會啟動該 UI。)|
|Windows 可靠性分析|設定可靠性 WMI 提供者| N/A |**已停用** (停用此原則設定後，可靠性監視器將不會顯示系統可靠性資訊，而且具備 WMI 功能的應用程式將無法從所列的提供者存取可靠性資訊。)|
|Windows 安全性 \ 通知|隱藏非重大通知| N/A |**已啟用** (啟用此設定後，本機使用者只會看到來自 Windows 安全性的重要通知。 他們不會看到其他類型的通知，例如一般電腦或裝置健康情況資訊。)|
|Windows Update|開啟軟體通知| N/A |**已停用**(此原則設定可讓您控制使用者是否會看到與 Microsoft Update 服務提供的精選軟體有關的詳細增強通知訊息。 增強通知訊息會傳達選用軟體的價值，並鼓勵使用者安裝及使用。 此原則設定適用於管理不嚴格、且允許使用者存取 Microsoft Update 服務的環境中。)|
|*Windows Update \ 商務用 Windows Update|管理預覽版|設定接收預覽版的行為：**停用預覽版**|**已啟用** (選取 [停用預覽版] 可防止在裝置上安裝預覽版。 這將導致使用者無法透過 [設定] -> [更新與安全性] 選擇加入 Windows 測試人員計畫。)|
|*Windows Update \ 商務用 Windows Update|選取接收預覽版和功能更新的時機|針對您要接收的更新選取 Windows 就緒程度：<p>**半年通道**<p>在預覽版或功能更新發行之後，延遲下列天數而不接收：**365**<p>暫停開始預覽版或功能更新：**yyyy-mm-dd**|**已啟用** (啟用此原則可指定要接收的預覽版或功能更新的層級和接收時機。) 半年通道：對一般大眾發行功能更新時加以接收。<p> 選取半年通道時：<p>- 您可以將接收功能更新最多延遲 365 天。<p>- 若要避免在排程的時間接收功能更新，您可以暫時將其暫停。 從提供的開始時間起，暫停會持續生效 35 天。<p> - 若要繼續接收已暫停的功能更新，請清除 [開始日期] 欄位。)|
|Windows Update \ 商務用 Windows Update|選取接收品質更新的時機|在品質更新發行之後，延遲下列天數而不接收：**30**<p>從 yyyy-mm-dd 開始暫停品質更新|**已啟用** (啟用此原則可指定接收品質更新的時機。<p>您可以將接收品質更新最多延遲 30 天。<p>若要避免在排定時間接收品質更新，您可以暫時暫停品質更新。 暫停會持續生效 35 天，或直到您清除 [開始日期] 欄位為止。<p>若要繼續接收已暫停的品質更新，請清除 [開始日期] 欄位。)<p>這項建議是為了協助控制何時套用更新，以及確保不會提供更新且意外安裝更新|
|本機電腦原則 \ 使用者設定 \ 系統管理範本| N/A | N/A | N/A |
|控制台 \ 地區及語言選項|關閉在我輸入時提供文字預測| N/A |**已啟用** (此原則會關閉 [在我輸入時提供文字預測] 選項。 不過，這並不會防止使用者或應用程式以程式設計方式變更設定。 啟用此原則設定後，將會鎖定此選項，而不提供文字預測。)|
|[桌上型電腦]|不要將最近開啟的文件共用新增到 [網路位置] 中| N/A |**已啟用** (啟用此設定後，當您在共用資料夾中開啟文件時，不會自動將共用資料夾新增至網路位置。)|
|[桌上型電腦]|關閉 Aero 搖動視窗最小化滑鼠手勢| N/A |**已啟用** (當使用中視窗隨著滑鼠來回搖動時，防止視窗進行最小化或還原。 啟用此原則後，當使用中視窗隨著滑鼠來回搖動時，不會將應用程式視窗最小化或還原。)|
|桌面 / Active Directory|Active Directory 搜尋的大小上限|傳回的物件數目：**1500**|**已啟用** (指定系統顯示的物件數目上限，以回應瀏覽或搜尋 Active Directory 的命令。 此設定會影響與 Active Directory 相關聯的所有瀏覽顯示，例如 [本機使用者和群組]、[Active Directory 使用者和電腦]，以及用來在 Active Directory 中設定使用者或群組物件權限的對話方塊。)|
|開始功能表和工作列|不顯示或追蹤捷徑清單中來自遠端位置的項目| N/A |**已啟用** (此原則設定可讓您控制從遠端位置顯示或追蹤捷徑清單中的項目。)|
|開始功能表和工作列|不要搜尋網際網路| N/A |**已啟用** (啟用此原則設定後，[開始] 功能表搜尋方塊不會搜尋網際網路歷程記錄或我的最愛。)|
|開始功能表和工作列|不要用以搜尋為主的方法來解析殼層捷徑| N/A |**已啟用** (此原則設定可防止系統對目標磁碟機進行全面搜尋以解析捷徑。)|
|開始功能表和工作列|關閉所有球形通知| N/A |**已啟用** (啟用此原則設定後，不會對使用者顯示任何通知氣球。)
|開始功能表和工作列|關閉功能通告提示氣球通知| N/A |**已啟用** (啟用此原則設定後，不會顯示標示為功能廣告的特定通知氣球。)|
|開始功能表和工作列|關閉使用者追蹤| N/A |**已啟用** (啟用此原則設定後，系統不會追蹤使用者執行的程式，而且不會在 [開始] 功能表中顯示常用的程式。)|
|開始功能表和工作列 / 通知|關閉快顯通知| N/A |**已啟用** (啟用此原則設定後，應用程式將無法提出快顯通知。)|
|*開始功能表和工作列 / 通知|關閉鎖定畫面上的快顯通知| N/A |**已啟用** (啟用此原則設定後，應用程式將無法在鎖定畫面上提出快顯通知。)|
|本機電腦原則 / 使用者設定| N/A | N/A | N/A |
|Windows 元件 / 雲端內容|設定鎖定畫面上的 Windows 焦點| N/A |**已停用** (停用此原則後，將會關閉 Windows 焦點，而使用者將無法再將其選取為鎖定畫面。 除非您已啟用「防止變更鎖定畫面影像」原則，否則使用者會看到預設鎖定畫面影像，並且能夠選取另一個影像。)|
|*Windows 元件 / 雲端內容|不建議 Windows Spotlight 中出現第三方內容| N/A |**已啟用** (啟用此原則後，Windows 焦點功能 (如鎖定畫面焦點、[開始] 功能表中建議的應用程式或 Windows 秘訣) 將不再建議來自第三方軟體發行者的應用程式和內容。 使用者可能仍會看到建議和秘訣，讓他們更有效率地使用 Microsoft 功能和應用程式。)|
|Windows 元件 / 雲端內容|不使用獨特體驗的診斷資料| N/A |**已啟用** (啟用此原則設定後，Windows 將不會使用來自此裝置的診斷資料 (視「診斷資料」設定值而定，此資料可能包含瀏覽器、應用程式和功能使用量) 來自訂鎖定畫面上顯示的內容、Windows 提示、Microsoft 消費者功能及其他相關功能。)|
|Windows 元件 / 雲端內容|關閉所有 Windows 焦點功能| N/A |**已啟用** (將會關閉鎖定畫面上的 Windows 焦點、Windows 提示、Microsoft 消費者功能及其他相關功能。 如果您的目標是要將來自目標裝置的網路流量降至最低，您應該啟用此原則設定。)|
|邊緣 UI|關閉追蹤應用程式使用量| N/A |**已啟用** (此原則設定可防止 Windows 追蹤最常使用和搜尋的應用程式。 如果您啟用此原則設定，應用程式將會依字母順序排序：<p> - 搜尋結果<p> - [搜尋] 和 [共用] 窗格<p> - 選擇器中的下拉式應用程式清單)|
|檔案總管|關閉縮圖快取處理| N/A |**已啟用** (啟用此原則設定後，就不會快取縮圖檢視。)|
|檔案總管|關閉通用控制項和視窗動畫| N/A |**已啟用** (停用動畫可以為具有某些視覺障礙的使用者改善其可用性，以及在某些情況下改善效能和電池壽命。)|
|檔案總管|關閉在 [檔案總管] 搜尋方塊中顯示最近搜尋項目| N/A |**已啟用** (停用建議搜尋方塊最近的查詢，並防止搜尋方塊中的項目儲存在登錄中供日後參考。)|
|檔案總管|關閉在隱藏的 thumbs.db 檔案中快取縮圖| N/A |**已啟用** (啟用此原則設定後，檔案總管不會建立、讀取或寫入至 thumbs.db 檔案。)|

\*來自 [Windows 受限流量有限功能基準](https://go.microsoft.com/fwlink/?linkid=828887)。

### <a name="system-services"></a>系統服務

如果您考慮停用系統服務以節省資源，請確定服務不是其他服務的元件。 在本文和可用的 GitHub 指令碼中，有些服務不在清單中，因為這些服務無法以支援的方式停用。

根據[在包含桌面體驗的 Windows Server 2016 中停用系統服務的指引](../../security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server.md)中的指示，這些建議大多與 Windows Server 2016 (與桌面體驗一起安裝) 的建議相對應。

許多可能適合停用的服務都設定為手動服務啟動類型。 這表示服務不會自動啟動，且除非有程序或服務對您考慮要停用的服務觸發了要求，才會啟動。 已設定為手動啟動類型的服務通常不會列於此處。

> [!NOTE]
> 您可以使用此 PowerShell 範例程式碼來列舉執行中的服務，只輸出服務的簡短名稱：
>
> ```powershell
> Get-Service | Where-Object {$_.Status -eq 'Running'} | Select-Object -ExpandProperty Name
> ```

下表包含一些可能被視為要在虛擬桌面環境中停用的服務：

| Windows 服務 | 服務名稱 | 項目 | 註解|
| --------------- | ------------ | ---- | ------ |
|行動電話時間|autotimesvc|此服務會根據來自行動網路的 NITZ 訊息設定時間|虛擬桌面環境可能沒有這類裝置可用。 <p>若要深入了解，請參閱[這篇文章](/windows-hardware/drivers/network/mb-nitz-support)。 |
|GameDVR 和廣播使用者服務|BcastDVRUserService|這項 (每一使用者) 使用者服務用於遊戲錄影和即時廣播|注意：這是每一使用者服務，因此必須停用範本服務。 這項使用者服務用於遊戲錄影和即時廣播。<p>若要深入了解，請參閱[這篇文章](/windows-hardware/drivers/network/mb-nitz-support)。 |
|CaptureService|CaptureService|為呼叫 Windows.Graphics.Capture API 的應用程式啟用選擇性螢幕擷取功能。|OneCore capture 服務：為呼叫 Windows.Graphics.Capture API 的應用程式啟用選擇性螢幕擷取功能<p> 如需詳細資訊，請參閱[這篇文章](/uwp/api/windows.graphics.capture?view=winrt-19041&preserve-view=true)。|
|已連線的裝置平台服務|CDPSvc|此服務適用於連線的裝置平台案例|已連線的裝置平台服務 <p> 若要深入了解，請參閱[這篇文章](/openspecs/windows_protocols/ms-cdp/929c2238-6d49-4ba4-a36a-37e732c4f736)。|
|CDP 使用者服務|CDPUserSvc| N/A |已連線的裝置平台使用者服務。 若要深入了解，請參閱[這篇文章](/openspecs/windows_protocols/ms-cdp/f5a15c56-ac3a-48f9-8c51-07b2eadbe9b4)。<p> 此使用者服務適用於連線的裝置平台案例 <br><br>這是「每一使用者服務」，因此必須停用範本服務 (CDPUserSvc)。|
|最佳化磁碟機|defragsvc|將儲存磁碟機上的檔案最佳化，有助於提高電腦執行效率。|虛擬桌面解決方案通常不會受益於磁碟最佳化。 「磁碟機」通常不是傳統的磁碟機，且通常只是暫時的儲存體配置。|
|診斷執行服務|DiagSvc|執行診斷動作以便進行疑難排解支援|停用此服務會停用執行 Windows 診斷「診斷執行服務」的功能。|
|已連線使用者體驗與遙測|DiagTrack|這項服務可啟用支援應用程式內與已連線使用者體驗的功能。 此外，在 [意見反應與診斷] 下啟用診斷與使用方式隱私權選項設定時，此服務會管理診斷與使用方式資訊 (用於改進 Windows 平台的體驗與品質) 的事件導向收集與傳輸。|如果在中斷連線的網路上，請考慮停用。 若要深入了解，請參閱[這篇文章](/windows/privacy/configure-windows-diagnostic-data-in-your-organization)。|
|診斷原則服務|DPS|診斷原則服務能夠針對 Windows 元件進行問題偵測、疑難排解並提供解決方案。 如果停止此服務，診斷將不再運作。|停用此服務會停用執行 Windows 診斷的功能。 如需詳細資訊，請參閱[這篇文章](/uwp/api/Windows.System.Diagnostics?view=winrt-19041&preserve-view=true)。|
|裝置設定管理員|DsmSvc|啟用裝置相關軟體的偵測、下載及安裝。 |如果停用此服務，裝置可能會使用過期的軟體來設定且可能無法正確運作。 <p>虛擬桌面環境非常密切地控制要安裝的軟體，並在整個環境中維持一致性。|
|資料使用量服務|DusmSvc|網路資料使用量、資料限制、限制背景資料、計量付費網路。| 如需詳細資訊，請參閱[這篇文章](/uwp/schemas/mobilebroadbandschema/dusm/schema-root)。 |
|Windows 行動熱點服務|icssvc|提供與其他裝置共用行動數據連線的能力。|若要深入了解，請參閱[這篇文章](/uwp/api/Windows.Networking.NetworkOperators.NetworkOperatorTetheringAccessPointConfiguration?view=winrt-19041&preserve-view=true)。|
|Microsoft Store 安裝服務|InstallService|提供 Microsoft Store 的基礎結構支援。 |此服務會依需求啟動，而且如果停用，則安裝將無法正常運作。<p>請考慮在非持續性虛擬桌面上停用此服務，讓持續性虛擬桌面解決方案保持原狀。|
|地理位置服務|Lfsvc|監視目前的系統位置並管理地理柵欄 (具有關聯事件的地理位置)。 |如果關閉此服務，應用程式將無法用來或接收地理位置或地理柵欄的通知。 若要深入了解，請參閱[這篇文章](/uwp/api/Windows.Devices.Geolocation?view=winrt-19041&preserve-view=true)。|
|已下載的地圖管理員|MapsBroker|適用於應用程式存取已下載地圖的 Windows 服務。 要存取已下載地圖的應用程式會依需求啟動此服務。|停用此服務將讓應用程式無法存取地圖。 若要深入了解，請參閱[這篇文章](/uwp/api/Windows.Services.Maps?view=winrt-19041&preserve-view=true)。|
|MessagingService|MessagingService|支援文字訊息和相關功能的服務。|這是「每一使用者服務」，因此必須停用範本服務。|
|同步主機|OneSyncSvc|此服務會將郵件、連絡人、行事曆與各種其他使用者資料同步。 |若此服務未執行，相依於此功能的 (UWP) 郵件與其他應用程式都將無法正常運作。 <p>這是「每一使用者服務」，因此必須停用範本服務。|
|連絡人資料|PimIndexMaintenanceSvc|為連絡人資料編製索引，以加快搜尋連絡人的速度。 如果您停止或停用此服務，則搜尋結果中可能會遺漏連絡人資訊。|這是「每一使用者服務」，因此必須停用範本服務。|
|電源|電源|管理電源原則與電源原則通知傳遞。|虛擬機器幾乎不會影響電源屬性。 如果停用此服務，則無法使用電源管理和報告功能。 若要深入了解，請參閱[這篇文章](/windows-hardware/drivers/powermeter/user-mode-power-service)。|
|付款與 NFC/SE 經理|SEMgrSvc|管理付款和近距離無線通訊 (NFC) 型安全元素。|在企業環境中，可能不需要這項服務來進行付款。|
|Microsoft Windows SMS 路由器服務。|SmsRouter|根據規則將訊息路由傳送至適當的用戶端。|如果其他工具用於訊息處理 (例如 Teams、Skype 或其他工具)，則可能不需要此服務。 若要深入了解，請參閱[這篇文章](/dotnet/framework/wcf/feature-details/routing-service)。|.
|Superfetch (SysMain)|SysMain|維護並改進一段時間後的系統效能。|基於各種原因，Superfetch 通常不會改善虛擬桌面環境中的效能。 基礎儲存體通常會虛擬化，而且可能會跨多個磁碟機等量分割。 在某些虛擬桌面解決方案中，累積的使用者狀態會在使用者登出時捨棄。 SysMain 功能應該在每個環境中進行評估。|
|觸控式鍵盤與手寫面板服務|TabletInputService|啟用觸控式鍵盤和手寫面板及筆跡功能|除非有使用中的觸控式螢幕或手寫輸入裝置，否則不需要。|
|更新 Orchestrator 服務|UsoSvc|管理 Windows 更新。 如果停止，您的裝置將無法下載並安裝最新的更新。|虛擬桌面裝置通常會謹慎地管理更新。 服務通常是在維護期間執行。 在某些情況下，可能會利用更新用戶端，例如 SCCM。 例外狀況會是安全性特徵碼更新，其會在任何時間套用至任何虛擬桌面裝置，以便維護最新的特徵碼。 如果您停用此服務，請進行測試以確保仍然能夠安裝安全性特徵碼。|
|磁碟區陰影複製|VSS|管理及實作用於備份和其他用途的磁碟區陰影複製。 |如果停止此服務，陰影複製將無法用於備份，且備份可能會失敗。 如果停用此服務，明確依存於此服務的所有服務都將無法啟動。 若要深入了解，請參閱[這篇文章](../../../WindowsServerDocs/storage/file-server/volume-shadow-copy-service.md)。|
|診斷系統裝載|WdiSystemHost|診斷原則服務會使用診斷系統裝載，來裝載需要在本機系統內容上執行的診斷。 如果停止此服務，則與其相依的任何診斷都將不再運作。|停用此服務會停用執行 Windows 診斷的功能
|Windows 錯誤報告|WerSvc|允許在程式停止運作或回應時報告錯誤，並允許傳遞現有的解決方案。 此外，也允許產生用於診斷與修復服務的記錄。 如果停止此服務，錯誤報告可能就無法正常運作，而且可能無法顯示診斷服務及修復的結果。|使用虛擬桌面環境時，診斷常會在離線狀態下執行，而不是在主要生產環境中執行。 此外，有些客戶會停用 WER。 WER 會針對許多不同的狀況產生少量資源，包括無法安裝裝置或無法安裝更新等狀況。 若要深入了解，請參閱[這篇文章](/windows/win32/wer/windows-error-reporting)。|
|Windows 搜尋|WSearch|提供檔案、電子郵件和其他內容的內容索引、屬性快取及搜尋結果。|停用此服務可防止建立電子郵件和其他項目的索引。 請先測試，再停用此服務。 若要深入了解，請參閱[這篇文章](/windows/win32/search/-search-3x-wds-overview#windows-search-service)。 |
|Xbox Live Auth Manager|XblAuthManager|提供驗證和授權服務以便與 Xbox Live 互動。 |如果停止此服務，某些應用程式可能無法正確運作。
|Xbox Live Game Save|XblGameSave|此服務會針對 Xbox Live 中已啟用儲存功能的遊戲同步處理儲存資料。 |如果停止此服務，遊戲儲存資料將不會上傳至 Xbox Live 或從中下載。
|Xbox Accessory Management Service|XboxGipSvc|此服務會管理已連線的 Xbox 配件。| N/A |
|Xbox Live Networking Service|XboxNetApiSvc|此服務支援 Windows.Networking.XboxLive 應用程式開發介面。| N/A |

#### <a name="per-user-services-in-windows"></a>Windows 中的每一使用者服務

每一使用者服務是會在使用者登入 Windows 或 Windows Server 時建立，並且在該使用者登出時停止並刪除的服務。這些服務執行於使用者帳戶的安全性內容中 - 其資源管理效能優於先前的方法所能提供的，因為過去須在 [總管] 中使用預先設定的帳戶或以工作的形式執行這類服務。 如需詳細資訊，請參閱 [Windows 中的每個使用者服務](/windows/application-management/per-user-services-in-windows)。

如果您想要變更服務起始值，偏好的方法是開啟已提升權限的 .CMD 命令提示字元，並執行服務控制管理員工具 `SC.EXE`。 如需詳細資訊，請參閱 [SC](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc754599(v=ws.11))。

### <a name="scheduled-tasks"></a>排定的工作

如同 Windows 中的其他項目，請在停用排定的工作前，先確定您不需要某個項目。 虛擬桌面環境中的某些工作（例如 **StartComponentCleanup**）可能不適合在生產環境中執行，但在維護期間的「黃金映射」（參考影像）上執行可能是很好的做法。

下列工作清單包含在電腦上執行最佳化或資料收集，使電腦在重新開機後保有其狀態。 當虛擬桌面裝置重新開機並捨棄前次開機後的所有變更時，用於實體電腦的最佳化將沒有幫助。

您可以使用下列 PowerShell 程式碼，取得目前所有排定的工作 (包括描述)：

```powershell
  Get-ScheduledTask | Select-Object -Property TaskPath,TaskName,State,Description
```

> [!NOTE]
> 有數個工作無法透過指令碼停用，即使是在提高權限的命令提示字元上執行也一樣。 這裡和 GitHub 指令碼中的建議不會嘗試停用無法透過指令碼停用的工作。

|排定的工作名稱|描述|
|-------------------|-----------|
|MNO|行動寬頻帳戶體驗中繼資料剖析器|
|AnalyzeSystem|此工作會分析尋找可能導致高能源使用之狀況的系統|
|行動電話通訊|與行動電話裝置相關|
|相容性|如果加入了 Microsoft 客戶經驗改進計畫，請收集程式遙測資訊。|
|資訊彙集器|如果使用者已同意參與 Windows 客戶經驗改進計畫，此作業會收集使用量資料並將其傳送給 Microsoft|
|診斷|(工作路徑中的 DiskFootprint) 'DiskFootprint' 是所有程序的合併貢獻，這些程序會以儲存體讀取、寫入和排清的形式發出儲存體 I/O。|
|FamilySafetyMonitor|初始化系列安全性監視和強制執行。|
|FamilySafetyRefreshTask|同步處理最新的設定與 Microsoft 系列功能服務。|
|MapsToastTask|此工作會顯示各種地圖相關快顯通知|
|Microsoft-Windows-DiskDiagnosticDataCollector|Windows 磁碟診斷會針對參與「客戶經驗計畫」的使用者，向 Microsoft 報告一般磁碟和系統資訊。|
|NotificationTask|執行每個使用者和 Web 互動的背景工作|
|ProcessMemoryDiagnosticEvents|排程記憶體診斷以回應系統事件|
|Proxy|如果您加入 Microsoft 客戶經驗改進計畫，此工作會收集並上傳 autochk SQM 資料。|
|QueueReporting|Windows 錯誤報告工作來處理已排入佇列的報告。|
|RecommendedTroubleshootingScanner|檢查 Microsoft 建議的疑難排解|
|RegIdleBackup|登錄閒置備份工作|
|RunFullMemoryDiagnostic|偵測並減輕實體記憶體 (RAM) 中的問題。|
|排程|Windows 排定的維護工作會自動修正問題，或透過安全性與維護加以報告，以執行電腦系統的定期維護。|
|ScheduledDefrag|此工作可將本機存放區磁碟機最佳化。|
|SilentCleanup|當可用磁碟空間不足時，系統用來啟動無訊息自動磁碟清理的維護工作。|
|SpeechModelDownloadTask|
|Sqm-Tasks|此工作會蒐集有關信賴平台模組 (TPM)、安全開機和測量開機的資訊。|
|SR|此工作會建立一般系統保護點。|
|StartComponentCleanup|在維護期間可能執行效果較佳的服務工作|
|StartupAppTask|如果有太多啟動項目，則會掃描啟動項目並對使用者提供通知。|
|SyspartRepair|
|WindowsActionDialog|位置通知|
|WinSAT|測量系統的效能和功能|
|XblGameSaveTask|Xbox Live GameSave 待命工作|

### <a name="apply-windows-and-other-updates"></a>套用 Windows (和其他) 更新

無論是從 Microsoft Update，還是從您的內部資源，都可套用可用的更新，包括 Windows Defender 特徵碼。 這是套用其他可用更新的好時機，包括 Microsoft Office 的更新 (如有安裝)，以及其他軟體更新。 如果 PowerShell 會保留在影像中，您可以執行命令 `Update-Help` 下載 PowerShell 的最新可用說明。

#### <a name="servicing-os-and-apps"></a>維護作業系統和應用程式

在映像最佳化程序中的某個時間點，應該會套用可用的 Windows 更新。 Windows 10 更新設定中有一個可提供其他更新的設定。 您可以在 [設定] > [進階選項] 找到此設定。 找到後，將 [當我更新 Windows 時提供其他 Microsoft 產品的更新]設為 [開啟]。

![[進階選項] 功能表的螢幕擷取畫面。 [提供其他 Microsoft 產品的更新] 設定已開啟。](media/rds-vdi-recommendations-1909/servicing.png)

如果您要將 Microsoft Office 之類的 Microsoft 應用程式安裝到基底映像，這會是較理想的設定。 如此，當映像開始運作時，Office 就會處於最新狀態。 此外還有 .NET 更新，以及某些可透過 Windows Update 取得更新的第三方元件 (例如 Adobe)。

安全性更新是非持續性虛擬桌面裝置非常重要的考量之一，其中包括安全性軟體定義檔。 這些更新可能會以一天一次或錯的間隔發行。

就 Windows Defender 而言，建議即使在非持續性虛擬桌面環境中，也應允許更新執行。 更新幾乎會在每次登入時套用，但這類更新都很小，應該不會有問題。 此外，由於只會套用最新的可用更新，裝置的更新將不會過時。 第三方定義檔可能也是如此。

>[!NOTE]
> 市集應用程式 (UWP 應用程式) 會透過 Windows 市集進行更新。 新版的 Office (例如 Office 365) 在直接連線至網際網路時會透過其本身的機制進行更新，而在未連線至網際網路時，則會透過管理技術進行更新。

#### <a name="windows-system-startup-event-traces-autologgers"></a>Windows 系統啟動事件追蹤 (AutoLoggers)

Windows 依預設設定會收集並儲存診斷資料。 其目的是要啟用診斷，或是記錄資料以便在需要進一步疑難排解時使用。 您可以在下圖所描述的位置找到自動系統追蹤：

![電腦管理資料夾的螢幕擷取畫面。 [系統追蹤] 資料夾已開啟並已選取 [WiFi 工作階段] 檔案。](media/rds-vdi-recommendations-1909/system-traces.png)

顯示於 [事件追蹤工作階段]  和 [啟動事件追蹤工作階段]  下方的部分追蹤無法停止，而且也不應停止。 其他追蹤 (例如 WiFiSession) 則可以停止。 若要停止執行中的追蹤，請在 [事件追蹤工作階段] 下方，以滑鼠右鍵按一下追蹤，然後選取 [停止]。 若要防止在系統啟動時自動啟動追蹤，請遵循下列步驟：

1. 選取 [啟動事件追蹤工作階段] 資料夾。

2. 尋找並選取您想要查看的追蹤檔案加以開啟。

3. 選取 [追蹤工作階段]  索引標籤。

4. 取消核取標示為 [已啟用] 的方塊。

5. 選取 [確定]  。

下表列出您應該考慮在虛擬桌面環境中停用的一些系統追蹤：

|名稱|註解|
|---|--------|
|Cellcore|[https://docs.microsoft.com/windows-hardware/drivers/network/cellular-architecture-and-driver-model](/windows-hardware/drivers/network/cellular-architecture-and-driver-model)|
|CloudExperienceHostOOBE|記載在[這裡](/windows/security/identity-protection/hello-for-business/hello-how-it-works-technology#cloud-experience-host)。|
|DiagLog|診斷原則服務所產生的記錄，其記載在[這裡](/windows-server/security/windows-services/security-guidelines-for-disabling-system-services-in-windows-server)|
|RadioMgr|記載在[這裡](/windows-hardware/drivers/nfc/what-s-new-in-nfc-device-drivers)|
|ReadyBoot|文件在[這裡](/previous-versions/windows/desktop/xperf/readyboot-analysis)。|
|WDIContextLog|無線區域網路 (WLAN) 裝置驅動程式介面，並記載在這裡。 |
|WiFiDriverIHVSession|記載在[這裡](/windows-hardware/drivers/network/user-initiated-feedback-normal-mode)。|
|WiFiSession|WLAN 技術的診斷記錄。 如果未實行 Wi-Fi，則不需要此記錄器|
|WinPhoneCritical|電話 (Windows?) 的診斷記錄。 如果未使用電話，則不需要此記錄器|

#### <a name="windows-defender-optimization-in-the-virtual-desktop-environment"></a>虛擬桌面環境中的 Windows Defender 最佳化

如需有關如何在虛擬桌面環境中最佳化 Windows Defender 的詳細資訊，請參閱[虛擬桌面基礎結構 (VDI) 環境中的 Windows Defender 防毒軟體部署指南](/windows/security/threat-protection/windows-defender-antivirus/deployment-vdi-windows-defender-antivirus)。

上述文章提供維護「黃金」虛擬桌面映像的程序，並說明如何維護執行中的虛擬桌面用戶端。 若要在虛擬桌面裝置需要更新其 Windows Defender 特徵碼時降低網路頻寬，請錯開重新開機的時間，並盡可能將重新開機排程在離峰時間。 Windows Defender 特徵碼的更新可包含在內部檔案共用上，且如果可行，請將這些檔案共用放在與虛擬桌面裝置相同或接近的網路區段上。

#### <a name="client-network-performance-tuning-by-registry-settings"></a>依登錄設定進行的用戶端網路效能微調

有些登錄設定可能會提高網路效能。 在虛擬桌面裝置或實體電腦的工作負載多半以網路為基礎的環境中，這項調整格外重要。 建議您套用此節中的設定，為目錄項目之類的項目設定額外的緩衝處理和快取，以調整網路工作負載設定檔的效能。

> [!NOTE]
> 本節中的某些設定僅以登錄為基礎，且應在映像部署至生產環境之前納入基底映像中。

下列設定記載於 [Windows Server 2016 效能微調指導方針](../../administration/performance-tuning/index.md)。

#### <a name="disablebandwidththrottling"></a>DisableBandwidthThrottling

`HKLM\System\CurrentControlSet\Services\LanmanWorkstation\Parameters\DisableBandwidthThrottling`

適用於 Windows 10。 預設值是 **0**。 根據預設，在某些情況下，SMB 重新導向器會對高延遲網路連線的輸送量進行節流，以避免網路相關逾時。 若將此登錄值設為 **1** 則會停用此節流，進而透過高延遲網路連接來達到更高的檔案轉送輸送量。 請考慮將此值設定為 **1**。

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

磁碟清理對於黃金/主要映像虛擬桌面實作可能特別有幫助。 在黃金/主要映像備妥、更新並完成設定後，執行磁碟清理是最後應執行的工作之一。 Github.com 上的最佳化指令碼具有 PowerShell 程式碼，可執行一般的磁碟清理工作

> [!NOTE]
> 磁碟清理設定位於稱為「儲存體」的 [設定] 類別「系統」。 根據預設，達到磁碟可用空間不足閾值時，就會執行儲存空間感知器。
>
> 若要深入了解如何使用儲存空間感知器搭配 Azure 自訂 VHD 影像，請參閱 [準備和自訂主要 VHD 映像](/azure/virtual-desktop/set-up-customize-master-image)。
>
> 對於使用 Windows 10 企業版或 Windows 10 企業版多工作階段的 Windows 虛擬桌面工作階段主機，建議停用儲存空間感知器。 您可以在 [儲存體] 下的 [設定] 功能表中停用儲存空間感知器。

以下是對針對各種磁碟清理工作提供的建議。 在實作之前，應該先測試這些項目：

1. 可以手動或自動利用儲存空間感知器。 如需儲存空間感知器的詳細資訊，請參閱這篇文章：在 Windows 10 中使用 OneDrive 和儲存空間感知器來管理磁碟空間
2. 手動清理暫存檔案和記錄。 在提升權限的命令提示字元中，執行下列命令：

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

3. 執行下列命令，以刪除系統上所有未使用的設定檔：

    `wmic path win32_UserProfile where LocalPath="C:\\users\\<users>" Delete`

對於本文件中的資訊如有任何問題或疑慮，請連絡 Microsoft 帳戶小組、參閱 [Microsoft 虛擬桌面 IT 專業人員部落格](https://community.windows.com/stories/virtual-desktop-windows-10)、將訊息張貼至 [Microsoft 虛擬桌面論壇](https://techcommunity.microsoft.com/t5/windows-virtual-desktop/bd-p/WindowsVirtualDesktop)，或連絡 [Microsoft](https://support.microsoft.com/contactus/) 以解決問題或疑慮。

### <a name="re-enable-windows-update"></a>重新啟用 Windows Update

如果您想要在停用 Windows Update 之後加以啟用，就像在持續性虛擬桌面一樣，遵循下列步驟：

1. 重新啟用群組原則設定：

   - 移至 [本機電腦原則] > [電腦設定] > [系統管理範本] > [系統] > [網際網路通訊管理] > [網際網路通訊設定]。
     - 將設定從 [已啟用] 變更為 [未設定]，以關閉所有 Windows Update 功能的存取。
   - 移至 [本機電腦原則] > [電腦設定] > [系統管理範本] > [Windows 元件] > [Windows Update]。
     - 將設定從 [已啟用] 變更為 [未設定]，以移除所有 Windows Update 功能的存取。
     - 請勿藉由將設定從 [已啟用] 變更為 [未設定]來連線到任何 Windows Update 網際網路位置。
   - 移至 [本機電腦原則] > [電腦設定] > [系統管理範本] > [Windows 元件] > [Windows Update] > [商務用 Windows Update]。
     - 選取收到品質更新時 (從 [已啟用] 變更為 [未設定])
   - 移至 [本機電腦原則] > [電腦設定] > [系統管理範本] > [Windows 元件] > [Windows Update] > [商務用 Windows Update]。
     - 選取收到預覽組建和功能更新時 (從**已啟用**變更為**未設定**)

2. 重新啟用服務：

   - 將 [更新 Orchestrator 服務] 從 [已停用] 變更為 [自動 (延遲開始)]。

3. 編輯 Windows 登錄 (警告，編輯登錄時請小心)。

   - 前往 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\PolicyState`。
     - 將 [DeferQualityUpdates] 從 '1' 變更為 '0'。
   - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\WindowsUpdate\UpdatePolicy\Settings`
     - 刪除 **PausedQualityDate** 的任何現有值。
   - 移至 `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\Explorer\WAU` 。
     - 設定為 [已停用]。

4. 重新啟用排定的工作：

   - 移至 [工作排程器程式庫] > [Microsoft] > [Windows] > [InstallService] > [ScanForUpdates]。
   - 移至 [工作排程器程式庫] > [Microsoft] > [Windows] > [InstallService] > [ScanForUpdatesAsUser]。

5. 重新啟動您的裝置，讓所有設定生效。

6. 如果不想讓此裝置提供功能更新，請移至 [設定] > [Windows Update] > [進階選項] > [選擇安裝更新的時間] 並手動設定選項 [功能更新包含新功能和改進]。如此可將此天數延後到任何非零的值，例如 180、365 等等。

## <a name="additional-information"></a>其他資訊

如需深入了解 Microsoft 的 VDI 架構，請參閱 [Windows 虛擬桌面文件](https://azure.microsoft.com/services/virtual-desktop/)。

如果您需要 sysprep 疑難排解的其他協助，請查看 [Sysprep 在您移除或更新包含內建 Windows 映像的 Microsoft Store 應用程式之後失敗](https://support.microsoft.com/help/2769827/sysprep-fails-after-you-remove-or-update-windows-store-apps-that-inclu)。
