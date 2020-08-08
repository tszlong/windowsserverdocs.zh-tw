---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 叢集感知更新-常見問題
ms.topic: article
manager: lizross
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Windows Server 中叢集感知更新的常見問題解答。
ms.openlocfilehash: 0283f7f29ccc647508530d6cfdbf54b41086b90c
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990909"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>叢集感知更新：常見問題集

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

叢集[感知更新](cluster-aware-updating.md) \(CAU \) 是一項功能，可協調容錯移轉叢集中所有伺服器上的軟體更新，而不會影響到叢集節點的規劃容錯移轉以外的服務可用性。 對於一些具有持續可用性功能的應用程式 \( （例如 \- 具有即時移轉的 hyper-v），或具有 Smb 透明容錯移轉的 smb 3.x 檔案伺服器 \) ，CAU 可以協調自動的叢集更新，而不會影響到服務可用性。

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU 是否支援更新儲存空間直接存取叢集？
是。 CAU 支援更新[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)叢集，而不論部署類型為何：超交集或已聚合。 具體而言，CAU 協調流程可確保暫停每個叢集節點會等待基礎叢集儲存空間狀況良好。

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>CAU 可以與 Windows Server 2008 R2 或 Windows 7 搭配使用嗎？
否。 CAU 只會從執行 Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows 10、Windows 8.1 或 Windows 8 的電腦協調叢集更新作業。 正在更新的容錯移轉叢集必須執行 Windows Server 2016、Windows Server 2012 R2 或 Windows Server 2012。

## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU 僅限於特定叢集應用程式嗎？
否。 CAU 與叢集應用程式類型無關。 CAU 是外部叢集 \- 更新解決方案，會在叢集 api 和 PowerShell Cmdlet 之上進行分層。 因此，CAU 可以協調在 Windows Server 容錯移轉叢集中設定的任何叢集應用程式的更新。

> [!NOTE]
> 目前，下列叢集工作負載已針對 CAU 進行測試及認證： SMB、Hyper-v \- 、DFS 複寫、DFS 命名空間、iSCSI 和 NFS。

## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 支援 Microsoft Update 與 Windows Update 的更新嗎？
是。 根據預設，CAU 會以外掛程式設定 \- ，其會使用叢集節點上的 Windows Update Agent \( WUA \) 公用程式 api。 WUA 基礎結構可以設定為指向 Microsoft Update，Windows Update 或 Windows Server Update Services \( WSUS \) 作為更新的來源。

## <a name="does-cau-support-wsus-updates"></a>CAU 支援 WSUS 更新嗎？
是。 根據預設，CAU 會以外掛程式設定 \- ，其會使用叢集節點上的 Windows Update Agent \( WUA \) 公用程式 api。 WUA 基礎結構可以設定為指向 Microsoft Update，並 Windows Update 或本機 Windows Server Update Services \( WSUS \) 伺服器作為其更新來源。

## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU 可以套用限量發行版本的更新嗎？
是。 有限的散發版本 \( LDR \) 更新（也稱為「修補程式」）不會透過 Microsoft Update 或 Windows Update 發佈，因此 Windows Update Agent \( WUA 外掛程式無法下載 \) \- 該 CAU 使用的預設值。

不過，CAU 包含第二個外掛程式 \- ，您可以選擇套用修補程式更新。 此「修復外掛程式」 \- 也可以自訂，以套用非 \- Microsoft 驅動程式、固件和 BIOS 更新。

## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>可以使用 CAU 套用累計更新嗎？
是。 如果累計更新是一般發行版本的更新或 LDR 更新，CAU 可以套用它們。

## <a name="can-i-schedule-updates"></a>我可以排程更新嗎？
是。 CAU 支援下列更新模式，這兩種模式都可以排程更新：

**自我 \- 更新**可讓叢集根據定義的設定檔和定期排程（例如每月維護期間）來更新自己。 您也可以隨時視需要啟動「自行 \- 更新執行」。 若要啟用自行 \- 更新模式，您必須將 CAU 叢集角色新增至叢集。 CAU 自我 \- 更新功能的執行方式就像任何其他叢集工作負載一樣，而且它可以順暢地與更新協調器電腦的已規劃和未計畫容錯移轉搭配運作。

**遠端 \- 更新**可讓您在執行 Windows 或 Windows Server 的電腦上隨時啟動「更新執行」。 您可以透過叢集感知更新視窗或使用**Invoke \- get-caurun** PowerShell Cmdlet 來啟動「更新執行」。 遠端 \- 更新是 CAU 的預設更新模式。 您可以使用工作排程器，從不是叢集節點的其中一部遠端電腦，以自己想要的排程來執行 [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) Cmdlet。

## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>我可以將更新排程在備份期間套用嗎？
是。 CAU 不會在這方面強加任何條件約束。 不過，當伺服器備份正在進行時，在伺服器上執行軟體更新， \( \) 並不是 IT 最佳作法。 請注意，CAU 僅使用叢集 API 來判斷資源容錯移轉與容錯回復，因此，CAU 不會知道伺服器備份的狀態。

## <a name="can-cau-work-with-configuration-manager"></a>CAU 可以與 Configuration Manager 搭配使用嗎？
CAU 是協調叢集節點上軟體更新的工具，而且 Configuration Manager 也會執行伺服器軟體更新。 請務必設定這些工具，使其在任何資料中心部署中都不會有相同伺服器的重迭涵蓋範圍，包括使用不同的 Windows Server Update Services 伺服器。 這可確保使用 CAU 背後的目標不會不慎失效，因為 Configuration Manager \- 驅動的更新不會納入叢集感知。

## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我需要系統管理認證才能執行 CAU 嗎？
是。 為了執行 CAU 工具，CAU 在本機伺服器上需要系統管理認證，或者，在本機伺服器或執行它的用戶端電腦上需要**在驗證後模擬用戶端**使用者權限。 不過，若要協調叢集節點上的軟體更新，CAU 就需要每個節點上的叢集系統管理認證。 雖然 CAU UI 可以在沒有認證的情況下啟動，但它會在連接到叢集實例以預覽或套用更新時，提示您輸入叢集系統管理認證。

## <a name="can-i-script-cau"></a>我可以編寫 CAU 的腳本嗎？
是。 CAU 隨附 PowerShell Cmdlet，提供一組豐富的腳本選項。 CAU UI 也是呼叫這些 Cmdlet 來執行 CAU 動作。

## <a name="what-happens-to-active-clustered-roles"></a>現用叢集角色會發生什麼事？

叢集角色 \( 先前稱為節點上作用中的應用程式和服務 \) 、故障切換到其他節點，然後才可以開始軟體更新。 CAU 會使用暫停並清空所有使用中叢集角色節點的維護模式，來協調這些容錯移轉。 當軟體更新完成時，CAU 會繼續讓節點及叢集角色容錯回復到更新的節點。 這樣可確保與節點相關的叢集角色散佈，在叢集的 CAU 更新執行上都保持一致。

## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>CAU 如何選取叢集角色的目標節點？

CAU 使用叢集 API 來協調容錯移轉。 叢集 API 執行會藉由依賴內部計量和智慧型放置啟發學習法 \( （例如 \) 跨目標節點的工作負載層級）來選取目標節點。

## <a name="does-cau-load-balance-the-clustered-roles"></a>CAU 是否會對叢集角色進行負載平衡？

CAU 不會對叢集節點進行負載平衡，但會嘗試保留叢集角色的散發。 在 CAU 完成更新叢集節點時，它會試著將先前主控的叢集角色容錯回復到該節點。 CAU 使用叢集 API 將資源容錯回復到暫停程序的開頭。 因此，在沒有未計劃的容錯移轉和偏好的擁有者設定的情況下，叢集角色的散佈不會變更。

## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>CAU 如何選取更新節點的順序？
CAU 預設會依據活動的等級來選取更新節點的順序。 裝載最少叢集角色的節點會最先更新。 不過，系統管理員可以在 CAU UI 中指定更新執行的參數，或使用 PowerShell Cmdlet，藉以指定更新節點的特定順序。

## <a name="what-happens-if-a-cluster-node-is-offline"></a>如果叢集節點離線，會發生什麼事？

啟動「更新執行」的系統管理員可以為離線的節點數量指定可接受的閾值。 因此，即使所有叢集節點都沒有連線，「更新執行」仍然可以在叢集上執行。

## <a name="can-i-use-cau-to-update-only-a-single-node"></a>我可以使用 CAU 來僅更新單一節點嗎？
否。 CAU 是叢集 \- 範圍的更新工具，因此它只允許您選取要更新的叢集。 如果您想要更新單一節點，可以使用 CAU 以外的現有伺服器更新工具。

## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>CAU 會報告從 CAU 外部起始的更新嗎？
否。 CAU 只會報告從 CAU 內部執行的更新執行。 不過，啟動後續的 CAU 更新執行時， \- 會適當地考慮透過非 CAU 方法安裝的更新，以判斷可能適用于每個叢集節點的其他更新。

## <a name="can-cau-support-my-unique-it-process-needs"></a>CAU 是否能夠支援我獨特的 IT 程式需求？
是。 CAU 提供下列方面的彈性，來適應企業客戶獨特 IT 程序需求：

**腳本**「更新執行」可以指定更新前 \- powershell 腳本和更新後 \- powershell 腳本。 在 \- 節點暫停之前，會在每個叢集節點上執行更新前腳本。 在 \- 安裝節點更新之後，會在每個叢集節點上執行更新後腳本。

> [!NOTE]
> 在您要執行 \- 更新前和更新後腳本的每個叢集節點上，都必須安裝 .NET Framework 4.6 或4.5 和 PowerShell \- 。 您也必須在叢集節點上啟用 PowerShell 遠端功能。 如需詳細的系統需求，請參閱叢集[感知更新的需求和最佳作法](cluster-aware-updating-requirements.md)。

**Advanced 更新執行選項**系統管理員可以額外指定一組大量的高階更新執行選項，例如每個節點上重試更新程式的最大次數。 您可以使用 CAU UI 或 CAU PowerShell Cmdlet 來指定這些選項。 這些自訂設定可儲存在「更新執行設定檔」中，以供往後的「更新執行」重複使用。

**公用外掛程式 \- 架構**CAU 包含註冊、取消註冊和選取外掛程式的功能 \- 。 cau 隨附兩個預設外掛程式 \- ：一個會協調 \( 每個叢集節點上的 Windows Update Agent WUA api，第二個則 \) 會套用手動複製到叢集節點可存取之檔案共用的修補程式。 如果企業具有這兩個外掛程式無法滿足的獨特需求 \- ，企業可以 \- 根據公用 API 規格來建立新的 CAU 外掛程式。 如需詳細資訊，請參閱叢集[ \- 感知更新外掛程式 \- 參考](/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)。

如需設定和自訂 CAU 外掛程式 \- 以支援不同更新案例的相關資訊，請參閱[外掛程式的 \- 操作方式](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。

## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何匯出 CAU 預覽和更新的結果？
CAU 透過命令 \- 行介面和 UI 提供匯出選項。

**命令 \- 行介面選項：**

-   使用 PowerShell Cmdlet Invoke Invoke-causcan 預覽結果** \- |Convertto-html \- Xml**。 輸出：XML

-   使用 PowerShell Cmdlet Invoke Get-caurun 來報告**結果 \- |Convertto-html \- Xml**。 輸出：XML

-   使用 PowerShell Cmdlet Get Get-caureport 報告結果** \- |匯出 \- get-caureport**。 輸出：HTML、CSV

**UI 選項：**

-   從 [預覽更新]**** 畫面複製報告結果。 輸出：CSV

-   從 [產生報告]**** 畫面複製報告結果。 輸出：CSV

-   從 [產生報告]**** 畫面匯出報告結果。 輸出：HTML

## <a name="how-do-i-install-cau"></a>如何安裝 CAU？
CAU 安裝已緊密整合到容錯移轉叢集功能中。 CAU 的安裝情況如下：

-   在叢集節點上安裝容錯移轉叢集時， \( \) 會自動安裝 CAU Windows Management Instrumentation WMI 提供者。

-   當伺服器或用戶端電腦上安裝了「容錯移轉叢集工具」功能時，會自動安裝叢集感知更新 UI 和 PowerShell Cmdlet。

## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 是否需要正在更新的叢集節點上執行的元件？
CAU 不需要在叢集節點上執行的服務。 不過，CAU 需要在叢集 \( 節點上安裝 WMI 提供者的軟體元件 \) 。 這個元件是與容錯移轉叢集功能一起安裝。

若要啟用自行 \- 更新模式，CAU 叢集角色也必須新增至叢集。

## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>使用 CAU 與 VMM 有何差異？

-   System Center Virtual Machine Manager \( VMM \) 著重于僅更新 hyper-v 叢集 \- ，而 CAU 可以更新任何類型的支援容錯移轉叢集，包括 hyper-v 叢集 \- 。

-   VMM 需要額外的授權，而 CAU 已獲得所有 Windows Server 的授權。 CAU 功能、工具以及 UI 都與容錯移轉叢集元件一起安裝。

-   如果您已經擁有 System Center 授權，您可以繼續使用 VMM 來更新 Hyper-v 叢集， \- 因為它提供整合式管理和軟體更新體驗。

-   只有執行 Windows Server 2016、Windows Server 2012 R2 和 Windows Server 2012 的叢集才支援 CAU。 VMM 也支援執行 \- Windows server 2008 R2 和 Windows server 2008 之電腦上的 hyper-v 叢集。

## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>我可以 \- 在設定自行更新的叢集中使用遠端更新 \- 嗎？
是。 自我更新設定中的容錯移轉叢集 \- 可以視需要透過遠端 \- 更新來更新 \- ，就像您可以隨時在電腦上強制執行 Windows Update 掃描一樣，即使 Windows Update 設定為自動安裝更新也一樣。 不過，您需要確定「更新執行」沒有正在執行。

## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>可以在多個叢集上重複使用叢集更新設定嗎？
是。 CAU 支援許多「更新執行」選項；當「更新執行」在更新叢集的時候，這些選項可以決定更新執行的行為。 這些選項會儲存為更新執行設定檔，而且可以在任何叢集上重複使用。 建議您儲存設定，並在具有類似更新需求的容錯移轉叢集上重複使用。 例如，您可能會 \- 為所有支援商務關鍵性服務的 Microsoft SQL Server 叢集建立「商務關鍵性 SQL Server 叢集更新執行設定檔」 \- 。

## <a name="where-is-the-cau-plug-in-specification"></a>CAU 外掛程式規格在哪裡 \- ？

-   [叢集 \- 感知更新外掛程式 \- 參考](/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)

-   [叢集感知更新外掛程式 \- 範例](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)

## <a name="additional-references"></a>其他參考資料

-   [叢集 \- 感知更新總覽](cluster-aware-updating.md)