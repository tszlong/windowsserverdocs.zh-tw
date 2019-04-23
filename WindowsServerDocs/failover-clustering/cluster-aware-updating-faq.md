---
ms.assetid: 6416d125-bcaf-433d-971a-2f0283bca2c2
title: 叢集感知更新-常見問題集
ms.topic: article
ms.prod: windows-server-threshold
manager: dongill
ms.author: jgerend
author: JasonGerend
ms.date: 04/28/2017
description: Windows Server 中的叢集感知更新的相關常見問題的解答。
ms.openlocfilehash: f9009811093823554f16295cc1205f1b99ead93f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882519"
---
# <a name="cluster-aware-updating-frequently-asked-questions"></a>叢集感知更新：常見問題集

> 適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

[叢集感知更新](cluster-aware-updating.md) \(CAU\)是協調軟體的功能不會影響服務可用性的方式容錯移轉叢集中的所有伺服器上任何超過規劃的容錯移轉叢集節點的更新。 對於某些應用程式具有持續可用性功能\(例如 Hyper-v\-V 即時移轉，或使用 SMB 3.x 檔案伺服器具有 SMB 透明容錯移轉\)，CAU 可以協調自動的叢集更新，而不會影響對服務可用性。

## <a name="does-cau-support-updating-storage-spaces-direct-clusters"></a>CAU 是否支援更新的儲存空間直接存取叢集？  
是的。 CAU 支援更新[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)不論部署類型的叢集： 超交集或交集。 具體來說，CAU 協調流程可確保，暫停每個叢集節點會等候才是狀況良好的基礎叢集的儲存空間。

## <a name="does-cau-work-with-windowsserver-2008r2-or-windows7"></a>CAU 可以與 Windows Server 2008 R2 或 Windows 7 搭配使用嗎？  
資料分割 CAU 協調叢集更新作業只能從執行 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012 中，Windows 10，Windows 8.1 或 Windows 8 的電腦。 正在更新容錯移轉叢集必須執行 Windows Server 2016、 Windows Server 2012 R2 或 Windows Server 2012。
  
## <a name="is-cau-limited-to-specific-clustered-applications"></a>CAU 僅限於特定叢集的應用程式嗎？  
資料分割 CAU 與叢集應用程式類型無關。 CAU 是外部叢集\-更新為最上層叢集 Api 和 PowerShell cmdlet 的解決方案。 因此，CAU 可以協調更新 Windows Server 容錯移轉叢集中設定任何叢集應用程式。  
  
> [!NOTE]  
> 目前，下列叢集工作負載測試及認證 cau:SMB、 Hyper-v\-V、 DFS 複寫、 DFS 命名空間、 iSCSI 及 NFS。  
  
## <a name="does-cau-support-updates-from-microsoft-update-and-windows-update"></a>CAU 支援 Microsoft Update 與 Windows Update 的更新嗎？  
是的。 根據預設，cau 會具有外掛程式\-中，使用 Windows Update 代理程式\(WUA\)叢集節點上的公用程式 Api。 WUA 基礎結構可以設定為指向 Microsoft Update 及 Windows Update 或 Windows Server Update Services \(WSUS\)做為來源的更新。  
  
## <a name="does-cau-support-wsus-updates"></a>CAU 支援 WSUS 更新嗎？  
是的。 根據預設，cau 會具有外掛程式\-中，使用 Windows Update 代理程式\(WUA\)叢集節點上的公用程式 Api。 WUA 基礎結構可以設定為指向 Microsoft Update 及 Windows Update 或本機的 Windows Server Update Services \(WSUS\)伺服器做為來源的更新。  
  
## <a name="can-cau-apply-limited-distribution-release-updates"></a>CAU 可以套用限量發行版本的更新嗎？  
是的。 有限的發行版本\(LDR\)讓 Windows Update 代理程式無法下載更新，也稱為 hotfix，不會透過 Microsoft Update 或 Windows Update 發行\(WUA\)插入\-，預設會使用 CAU。  
  
不過，CAU 會包含第二個的隨插即用\-，您可以選取要套用 hotfix 更新。 此 hotfix 外掛程式\-中，也可以自訂以套用非\-Microsoft 驅動程式、 韌體與 BIOS 更新。  
  
## <a name="can-i-use-cau-to-apply-cumulative-updates"></a>可以使用 CAU 套用累計更新嗎？  
是的。 如果累計更新是一般發行版本的更新或 LDR 更新，CAU 可以套用它們。  
  
## <a name="can-i-schedule-updates"></a>可以排程更新嗎？  
是的。 CAU 支援下列更新模式，這兩種模式都可以排程更新：  
  
**自助\-更新**讓叢集按照來更新自己定義的設定檔和定期排程，例如每月維護期間。 您也可以啟動自我\-隨時視需要更新執行。 若要啟用自助式\-更新模式，您必須新增 CAU 叢集的角色到叢集。 CAU 自行\-更新功能會執行任何其他叢集工作負載，例如，而且它可以緊密地與更新協調器電腦的計劃性與非計劃性容錯移轉。  
  
**遠端\-更新**可讓您啟動 「 更新執行隨時從執行 Windows 或 Windows Server 的電腦。 您可以啟動 「 更新執行叢集感知更新的視窗或使用**Invoke\-CauRun** PowerShell cmdlet。 遠端\-更新是 cau 更新模式的預設值。 您可以使用工作排程器，從不是叢集節點的其中一部遠端電腦，以自己想要的排程來執行 [Invoke-CauRun](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/invoke-caurun) Cmdlet。  
  
## <a name="can-i-schedule-updates-to-apply-during-a-backup"></a>可以排程要在備份期間套用的更新嗎？  
是的。 在這方面 CAU 不會造成任何條件約束。 不過，在伺服器上執行軟體更新\(相關聯的可能會重新啟動\)伺服器備份時的進度不是 IT 的最佳做法。 請注意，CAU 僅使用叢集 API 來判斷資源容錯移轉與容錯回復，因此，CAU 不會知道伺服器備份的狀態。  
  
## <a name="can-cau-work-with-system-center-configuration-manager"></a>CAU 可以運作使用 System Center Configuration Manager 嗎？  
CAU 是協調叢集節點上的軟體更新的工具和 Configuration Manager 也會執行伺服器軟體更新。 請務必設定這些工具，讓他們不需要在任何資料中心部署，包括使用不同的 Windows Server Update Services 伺服器中的相同伺服器的涵蓋範圍重疊。 這可確保使用 CAU 的目標不小心被破壞，因為 Configuration Manager\-導向更新不會納入叢集感知功能。  
  
## <a name="do-i-need-administrative-credentials-to-run-cau"></a>我需要系統管理認證才能執行 CAU 嗎？  
是的。 為了執行 CAU 工具，CAU 在本機伺服器上需要系統管理認證，或者，在本機伺服器或執行它的用戶端電腦上需要**在驗證後模擬用戶端**使用者權限。 不過，若要協調叢集節點上的軟體更新，CAU 就需要每個節點上的叢集系統管理認證。 雖然未使用的認證，可以啟動 CAU UI，它會提示輸入叢集系統管理認證連接到的叢集執行個體，來預覽或套用更新時。  
  
## <a name="can-i-script-cau"></a>我可以透過指令碼 CAU？  
是的。 CAU 隨附提供一組豐富的指令碼選項的 PowerShell cmdlet。 CAU UI 也是呼叫這些 Cmdlet 來執行 CAU 動作。  

## <a name="what-happens-to-active-clustered-roles"></a>使用中叢集角色會發生什麼事？

叢集角色\(之前稱為應用程式和服務\)時在節點上作用，軟體更新開始之前，容錯移轉到其他節點。 CAU 會使用暫停並清空所有使用中叢集角色節點的維護模式，來協調這些容錯移轉。 當軟體更新完成時，CAU 會繼續讓節點及叢集角色容錯回復到更新的節點。 這樣可確保與節點相關的叢集角色散佈，在叢集的 CAU 更新執行上都保持一致。  
  
## <a name="how-does-cau-select-target-nodes-for-clustered-roles"></a>CAU 如何選取目標節點的叢集角色？

CAU 使用叢集 API 來協調容錯移轉。 叢集 API 實作會依賴內部衡量標準及智慧型定位啟發學習法來選取目標節點\(等工作負載層級\)目標節點。  
  
## <a name="does-cau-load-balance-the-clustered-roles"></a>CAU 負載是否平衡叢集的角色？

CAU 不會進行負載的平衡叢集的節點，但它會試著保留叢集角色的分佈。 在 CAU 完成更新叢集節點時，它會試著將先前主控的叢集角色容錯回復到該節點。 CAU 使用叢集 API 將資源容錯回復到暫停程序的開頭。 因此，在沒有未計劃的容錯移轉和偏好的擁有者設定的情況下，叢集角色的散佈不會變更。  
  
## <a name="how-does-cau-select-the-order-of-nodes-to-update"></a>CAU 如何選取更新節點的順序？  
CAU 預設會依據活動的等級來選取更新節點的順序。 裝載最少叢集角色的節點會最先更新。 不過，系統管理員可以指定更新節點，在 CAU UI 中 「 更新執行指定的參數，或使用 PowerShell cmdlet 的特定順序。  
  
## <a name="what-happens-if-a-cluster-node-is-offline"></a>如果叢集節點離線時，發生什麼事？

啟動「更新執行」的系統管理員可以為離線的節點數量指定可接受的閾值。 因此，即使所有叢集節點都沒有連線，「更新執行」仍然可以在叢集上執行。  
  
## <a name="can-i-use-cau-to-update-only-a-single-node"></a>可以使用 CAU 更新單一節點嗎？  
資料分割 CAU 是叢集\-範圍更新工具，所以它只能讓您選取要更新的叢集。 如果您想要更新單一節點，可以使用 CAU 以外的現有伺服器更新工具。  
  
## <a name="can-cau-report-updates-that-are-initiated-from-outside-cau"></a>CAU 可以報告從 CAU 外面起始的更新嗎？  
資料分割 CAU 只會報告從 CAU 內部執行的更新執行。 不過，在後續 CAU 更新執行啟動時，已透過非安裝的更新\-CAU 方法會適度考慮以判斷可能適用於每個叢集節點的其他更新。  
  
## <a name="can-cau-support-my-unique-it-process-needs"></a>可以 CAU 支援我獨特的 IT 程序需求嗎？  
是的。 CAU 提供下列方面的彈性，來適應企業客戶獨特 IT 程序需求：  
  
**指令碼**「 更新執行 」 可以指定預先\-更新 PowerShell 指令碼和後置\-更新 PowerShell 指令碼。 前\-更新指令碼每個叢集節點上執行之前的節點已暫停。 Post\-節點更新安裝之後，更新指令碼執行每個叢集節點上。  
  
> [!NOTE]  
> 您要執行前的每個叢集節點上，則必須安裝.NET framework 4.6 和 4.5 PowerShell\-更新，並張貼\-更新指令碼。 您也必須啟用 PowerShell 遠端執行功能，在叢集節點上。 如需詳細的系統需求，請參閱 <<c0> [ 需求和叢集感知更新的最佳做法](cluster-aware-updating-requirements.md)。  
  
**進階更新執行 」 選項**系統管理員可以從大量例如更新程序會在每個節點重試次數上限的進階更新執行選項另行指定。 這些選項，可以使用 CAU UI 或 CAU PowerShell cmdlet 來指定。 這些自訂設定可儲存在「更新執行設定檔」中，以供往後的「更新執行」重複使用。  
  
**公用外掛程式\-架構中**CAU 包含 「 註冊、 取消登錄 」 的功能，並選取插入\-集。CAU 隨附兩個預設外掛程式\-集： 一個會協調 Windows Update 代理程式\(WUA\) Api 在每個叢集節點; 第二個適用於手動複製到叢集節點存取檔案共用的 hotfix。 如果企業的獨特的需求，無法滿足這些兩個外掛\-ins，企業可以建立新的 CAU 外掛程式\-在根據公用 API 規格。 如需詳細資訊，請參閱 <<c0> [ 叢集\-感知更新外掛程式\-參考中](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)。  
  
如需設定和自訂 CAU 外掛\-集以支援不同更新案例，請參閱 <<c2> [ 如何插入\-集運作](assetId:///847b571b-12b3-473c-953f-75a5a1f51333)。  
  
## <a name="how-can-i-export-the-cau-preview-and-update-results"></a>如何匯出 CAU 預覽和更新的結果？  
CAU 提供匯出選項，透過命令\-列介面及 UI。  
  
**命令\-列介面選項：**  
  
-   預覽 results by using PowerShell cmdlet **Invoke\-CauScan |ConvertTo\-Xml**。 輸出：XML  
  
-   使用 PowerShell cmdlet 來報告結果**Invoke\-CauRun |ConvertTo\-Xml**。 輸出：XML  
  
-   使用 PowerShell cmdlet 來報告結果**取得\-CauReport |匯出\-CauReport**。 輸出：HTML、CSV  
  
**UI 選項：**  
  
-   從 [預覽更新] 畫面複製報告結果。 輸出：CSV  
  
-   從 [產生報告] 畫面複製報告結果。 輸出：CSV  
  
-   從 [產生報告]  畫面匯出報告結果。 輸出：HTML  
  
## <a name="how-do-i-install-cau"></a>如何安裝 CAU？  
CAU 安裝已緊密整合到容錯移轉叢集功能中。 CAU 的安裝情況如下：  
  
-   當容錯移轉叢集安裝在叢集節點，CAU Windows Management Instrumentation \(WMI\)自動安裝提供者。  
  
-   在伺服器或用戶端電腦上安裝 「 容錯移轉叢集工具 」 功能時，會自動安裝的叢集感知更新的 UI 和 PowerShell cmdlet。
  
## <a name="does-cau-need-components-running-on-the-cluster-nodes-that-are-being-updated"></a>CAU 是否需要正在更新叢集節點上執行的元件？  
CAU 不需要在叢集節點上執行的服務。 不過，CAU 會需要一種軟體元件\(WMI 提供者\)叢集節點上安裝。 這個元件是與容錯移轉叢集功能一起安裝。  
  
若要啟用自助式\-更新模式，CAU 叢集的角色必須也要新增至叢集。  
  
## <a name="what-is-the-difference-between-using-cau-and-vmm"></a>使用 CAU 與 VMM 之間的差異為何？  
  
-   System Center Virtual Machine Manager \(VMM\)著重於更新的只有 Hyper-v\-V 叢集，而 CAU 可以更新任何一種支援的容錯移轉叢集，包括 Hyper-v\-V 叢集。  
  
-   VMM 需要額外授權，而 CAU 已獲得適用於所有 Windows Server。 CAU 功能、工具以及 UI 都與容錯移轉叢集元件一起安裝。  
  
-   如果您已經擁有 System Center 授權，您可以繼續使用 VMM 來更新 Hyper-v\-V 叢集，因為它提供整合式的管理和軟體更新的體驗。  
  
-   只有在執行 Windows Server 2016、 Windows Server 2012 R2 及 Windows Server 2012 的叢集上支援 CAU。 VMM 也支援 Hyper-v\-V 叢集在執行 Windows Server 2008 R2 和 Windows Server 2008 電腦上。  
  
## <a name="can-i-use-remote-updating-on-a-cluster-that-is-configured-for-self-updating"></a>我可以使用遠端\-在叢集上設定自我更新\-更新嗎？  
是的。 在自動容錯移轉叢集\-更新組態可以透過遠端更新\-更新\-要求，就像即使 Windows Update 設定為，您可以在您的電腦上強制 Windows Update 掃描在任何時間自動安裝更新。 不過，您需要確定「更新執行」沒有正在執行。  
  
## <a name="can-i-reuse-my-cluster-update-settings-across-clusters"></a>可以在多個叢集上重複使用叢集更新設定嗎？  
是的。 CAU 支援許多「更新執行」選項；當「更新執行」在更新叢集的時候，這些選項可以決定更新執行的行為。 這些選項會儲存為更新執行設定檔，而且可以在任何叢集上重複使用。 建議您儲存設定，並在具有類似更新需求的容錯移轉叢集上重複使用。 比方說，您可以建立 「 商務\-關鍵 SQL Server 叢集更新執行設定檔 」 支援業務的所有 Microsoft SQL Server 叢集\-重要服務。  
  
## <a name="where-is-the-cau-plug-in-specification"></a>其中是 CAU 外掛程式\-規格中？  
  
-   [叢集\-感知更新外掛程式\-參考中](https://msdn.microsoft.com/library/hh418084(VS.85).aspx)  
  
-   [叢集感知更新外掛程式\-中範例](https://code.msdn.microsoft.com/windowsdesktop/Cluster-Aware-Updating-6a8854c9)  
  
## <a name="see-also"></a>另請參閱  
  
-   [叢集\-感知更新概觀](cluster-aware-updating.md)  
  
