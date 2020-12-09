---
title: 叢集感知更新概觀
description: Cluster-Aware 更新 (CAU) 在執行 Windows Server 的叢集上自動安裝軟體更新。
ms.topic: article
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.date: 08/06/2018
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: e729c3e20da1c66581275fb12ec4a5f848475724
ms.sourcegitcommit: d08965d64f4a40ac20bc81b14f2d2ea89c48c5c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96865737"
---
# <a name="cluster-aware-updating-overview"></a>叢集感知更新概觀

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題概要說明「叢集 \- 感知更新 \( CAU」 \) ，這項功能可將叢集伺服器上的軟體更新程式自動化，同時維持可用性。

> [!NOTE]
> 更新 [儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md) 叢集時，建議使用 Cluster-Aware 更新。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
Cluster-Aware 更新是一種自動化的功能，可讓您在更新程式期間，更新 [容錯移轉](failover-clustering-overview.md) 叢集中的伺服器，而且幾乎不會遺失可用性。 在「更新執行」期間，Cluster-Aware 以透明的方式更新會執行下列工作：

1. 將叢集的每個節點放入節點維護模式。
2. 將叢集角色移出節點。
3. 安裝更新和任何相依的更新。
4. 視需要執行重新開機。
5. 將節點移出維護模式。
6. 還原節點上的叢集角色。
7. 移動以更新下一個節點。

對於叢集中的許多叢集角色而言，自動化更新程序會觸發計劃的容錯移轉。 這對連線的用戶端會造成暫時性的服務中斷。 不過，在持續可用的工作負載（例如 \- 具有即時移轉的 hyper-v 或具有 SMB 透明容錯移轉的檔案伺服器）的情況下，Cluster-Aware 更新可以協調叢集更新，而不會影響到服務可用性。

## <a name="practical-applications"></a>實際應用

-   CAU 可減少叢集服務的服務中斷、減少手動更新因應措施的需求，並讓 \- \- 系統管理員更可靠地更新端對端叢集。 當 CAU 功能與持續可用的叢集工作負載搭配使用時，如持續可用的檔案伺服器 \( 檔案伺服器工作負載（具有 SMB 透明容錯移轉 \) 或 hyper-v \- ），就可以執行叢集更新，對用戶端的服務可用性不會有任何影響。

-   CAU 讓整個企業採用一致的 IT 程序。 您可以針對不同的容錯移轉叢集類別建立更新執行設定檔，然後在檔案共用集中進行管理，以確保整個 IT 組織的 CAU 部署會以一致的方式套用更新，即使叢集是由不同 \- 的 \- 企業或系統管理員所管理。

-   CAU 的「更新執行」排程可以是每天、每週、或每月的間隔，幫助協調叢集更新與其他 IT 管理程序。

-   CAU 提供可延伸的架構，以感知叢集的方式更新叢集軟體清查 \- 。 發行者可以使用這項功能來協調未發佈至 Windows Update 或 Microsoft Update 的軟體更新安裝，或是 Microsoft 未提供的軟體更新安裝，例如，非 \- microsoft 設備磁碟機的更新。

-   CAU 自行 \- 更新模式可讓「一個箱中的叢集」設備 \( 成為一組叢集實體機器，通常封裝在一個底座中 \) 以進行更新。 一般來說，這類應用裝置會部署在分公司，只需要最少的當地 IT 支援，就可以管理叢集。 自我 \- 更新模式在這些部署案例中提供絕佳的價值。

## <a name="important-functionality"></a>重要功能
以下是重要 Cluster-Aware 更新功能的描述：

-   使用者介面 \( UI \) -叢集感知更新視窗，以及您可以用來預覽、套用、監視及報告更新的一組 Cmdlet

-   \- \- \- \( \) 由一或多個更新協調器電腦協調的叢集更新作業的端對端自動化

-   預設外掛程式 \- ，可與 Windows Server 中現有的 Windows Update 代理程式 \( WUA \) 和 Windows Server Update Services \( WSUS \) 基礎結構整合，以套用重要的 Microsoft 更新

-   第二個外掛程式， \- 可用來套用 microsoft 的修正程式，而且可以自訂以套用非 \- Microsoft 更新

-   使用「更新執行」選項 (如每個節點重試更新的次數上限) 來設定「更新執行設定檔」。 「更新執行設定檔」可以讓您在整個「更新執行」時，快速重複使用相同的設定，而且輕鬆地與其他容錯移轉叢集共用更新設定。

-   支援新外掛程式開發的可延伸架構 \- ，可協調整個叢 \- 集中的其他節點更新工具，例如自訂軟體安裝程式、BIOS 更新工具，以及網路介面卡或主機匯流排介面卡 \( HBA \) 更新工具。

Cluster-Aware 更新可以在兩種模式中協調完整的叢集更新作業：

-   這種模式的 **自我 \- 更新模式**，會將 CAU 叢集角色設定為要更新之容錯移轉叢集上的工作負載，並定義相關聯的更新排程。 叢集會使用預設或自訂的「更新執行」設定檔，在排定的時間自行更新。 在「更新執行」期間，CAU 更新協調器程序先從目前擁有 CAU 叢集角色的節點開始，然後依序在每個叢集節點上執行更新。 若要更新目前的叢集節點，CAU 叢集角色會容錯移轉到其他叢集節點，而該節點上新的更新協調器程序則會取得「更新執行」的控制權。 在自行 \- 更新模式中，CAU 可以使用完全自動化的端 \- 對端更新程式來更新容錯移轉叢集 \- 。 系統管理員也可以 \- 在此模式中視需要觸發更新，或直接使用遠端 \- 更新方法（如有需要）。 在自行 \- 更新模式中，系統管理員可以藉由連接到叢集並執行 **get \- invoke-caurun** Windows PowerShell Cmdlet，來取得正在進行更新執行的相關摘要資訊。

-   此模式的 **遠端 \- 更新模式**（稱為「更新協調器」的遠端電腦）是使用 CAU 工具進行設定。 更新協調器不是「更新執行」期間更新的叢集成員。 從遠端電腦，系統管理員會 \- 使用預設或自訂更新執行設定檔來觸發隨選更新執行。 遠端 \- 更新模式適用于監視在「 \- 更新執行」期間的即時進度，以及在 Server Core 安裝上執行的叢集。

## <a name="hardware-and-software-requirements"></a>硬體和軟體需求

CAU 可以在所有 Windows Server 版本上使用，包括 Server Core 安裝。 如需詳細的需求資訊，請參閱叢集 [感知更新需求和最佳作法](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安裝 Cluster-Aware 更新
若要使用 CAU，請在 Windows Server 中安裝容錯移轉叢集功能，並建立容錯移轉叢集。 支援 CAU 功能的元件會自動安裝在每個叢集節點上。

若要安裝容錯移轉功能，可以使用下列工具：
- 伺服器管理員中的 [新增角色及功能精靈]。
- [Install-](/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps) Windows PowerShell Cmdlet
- 部署映像服務與管理 (DISM) 命令列工具

如需詳細資訊，請參閱 [安裝容錯移轉叢集功能](create-failover-cluster.md#install-the-failover-clustering-feature)。

您也必須安裝容錯移轉叢集工具（這些工具是遠端伺服器管理工具的一部分），而且預設會在伺服器管理員中安裝容錯移轉叢集功能時安裝。 容錯移轉叢集工具組含 Cluster-Aware 更新使用者介面和 PowerShell Cmdlet。

您必須如下所示安裝容錯移轉叢集工具，以支援不同的 CAU 更新模式：

- 若要在自行更新模式中使用 CAU \- ，請在每個叢集節點上安裝容錯移轉叢集工具。

- 若要啟用遠端 \- 更新模式，請在有網路連線到容錯移轉叢集的電腦上安裝容錯移轉叢集工具。

> [!NOTE]
> -   您無法在 Windows Server 2012 上使用容錯移轉叢集工具來管理較新版本 Windows Server 上的 Cluster-Aware 更新。
> -   若只要在遠端更新模式中使用 CAU \- ，則不需要在叢集節點上安裝容錯移轉叢集工具。 不過，有些 CAU 功能將無法使用。 如需詳細資訊，請參閱叢集 [ \- 感知更新的需求和最佳作法](cluster-aware-updating-requirements.md)。
> -   除非您只在自行更新模式中使用 CAU \- ，否則安裝 cau 工具和協調更新的電腦不能是容錯移轉叢集的成員。

### <a name="enabling-self-updating-mode"></a>啟用自行更新模式
若要啟用自行更新模式，您必須將 Cluster-Aware 更新叢集角色新增至容錯移轉叢集。 若要執行此動作，請使用下列其中一種方法：
- 在伺服器管理員中，選取 [**工具**  >  **群集感知更新**]，然後在 [Cluster-Aware 更新] 視窗中，選取 [**設定叢集自行更新選項**]。
- 在 PowerShell 會話中，執行 [add-cauclusterrole](/powershell/module/clusterawareupdating/Add-CauClusterRole) Cmdlet。

若要卸載 CAU，請使用伺服器管理員、 [uninstall](/powershell/module/servermanager/Uninstall-WindowsFeature) CMDLET 或 DISM 命令列工具卸載容錯移轉叢集功能或容錯移轉叢集工具 \- 。

### <a name="additional-requirements-and-best-practices"></a>其他需求和最佳做法

若要確保 CAU 可以順利更新叢集節點，以及取得設定容錯移轉叢集環境以使用 CAU 的其他指導方針，可以執行 CAU 最佳做法分析程式。

如需使用 CAU 的詳細需求和最佳做法，以及執行 CAU 最佳做法分析程式的相關資訊，請參閱叢集 [ \- 感知更新的需求和最佳作法](cluster-aware-updating-requirements.md)。

### <a name="starting-cluster-aware-updating"></a>開始 Cluster-Aware 更新

##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>從伺服器管理員開始 Cluster-Aware 更新

1.  啟動伺服器管理員。

2.  請執行下列其中一項：

    -   按一下 [ **工具** ] 功能表上的 [叢集 **\- 感知更新**]。

    -   如果有一或多個叢集節點（或叢集）新增至伺服器管理員，請在 [ **所有伺服器** ] 頁面上，以滑鼠右鍵 \- 按一下節點名稱 \( 或叢集名稱，然後 \) 按一下 [ **更新** 叢集]。

## <a name="additional-references"></a>其他參考資料
下列連結提供有關使用 Cluster-Aware 更新的詳細資訊。

-   [叢集感知更新的需求和最佳作法 \-](cluster-aware-updating.md)

-   [叢集 \- 感知更新：常見問題](cluster-aware-updating-faq.md)

-   [CAU 的進階選項和更新執行設定檔](cluster-aware-updating-options.md)

-   [CAU 外掛程式的 \- 運作方式](cluster-aware-updating-plug-ins.md)

-   [叢集 \- 感知更新 Windows PowerShell 中的 Cmdlet](/powershell/module/clusterawareupdating/)

-   [叢集 \- 感知更新外掛程式 \- 參考](/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)
