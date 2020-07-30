---
title: 叢集感知更新概觀
description: 叢集感知更新（CAU）會在執行 Windows Server 的叢集上自動安裝軟體更新。
ms.topic: article
ms.prod: windows-server
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: a889e19947d014bc2008417f64f6be5cc53112e6
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409889"
---
# <a name="cluster-aware-updating-overview"></a>叢集感知更新概觀

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供叢集 \- 感知更新 CAU 的總覽 \( \) ，這項功能會在叢集伺服器上自動執行軟體更新程式，同時維持可用性。

> [!NOTE]
> 更新[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)叢集時，建議使用叢集感知更新。

## <a name="feature-description"></a><a name="BKMK_OVER"></a>功能描述
叢集感知更新是一項自動化功能，可讓您在[容錯移轉](failover-clustering-overview.md)叢集中補救伺服器，而在更新過程中，可用性幾乎不會遺失。 在「更新執行」期間，叢集感知更新會以透明的方式執行下列工作：

1. 將叢集的每個節點放入節點維護模式。
2. 將叢集角色移出節點。
3. 安裝更新以及任何相依的更新。
4. 視需要執行重新開機。
5. 使節點離開維護模式。
6. 還原節點上的叢集角色。
7. 移動以更新下一個節點。

對於叢集中的許多叢集角色而言，自動化更新程序會觸發計劃的容錯移轉。 這對連線的用戶端會造成暫時性的服務中斷。 不過，在持續可用的工作負載（例如 \- 具有即時移轉的 hyper-v 或具有 SMB 透明容錯移轉的檔案伺服器）情況下，叢集感知更新可以協調叢集更新，而不會影響到服務可用性。

## <a name="practical-applications"></a>實際應用

-   CAU 可減少叢集服務中的服務中斷、降低手動更新因應措施的需求，並 \- 讓 \- 系統管理員更可靠地進行端對端叢集更新進程。 當 CAU 功能與持續可用的叢集工作負載搭配使用時（例如持續可用的檔案伺服器 \( 檔案伺服器工作負載與 SMB 透明容錯移轉 \) 或 hyper-v \- ），可以對用戶端的服務可用性不會有任何影響來執行叢集更新。

-   CAU 讓整個企業採用一致的 IT 程序。 您可以針對不同的容錯移轉叢集類別建立更新執行設定檔，然後集中管理檔案共用，以確保整個 IT 組織的 CAU 部署會一致地套用更新，即使叢集是由不同 \- 的企業營運 \- 或系統管理員所管理。

-   CAU 的「更新執行」排程可以是每天、每週、或每月的間隔，幫助協調叢集更新與其他 IT 管理程序。

-   CAU 提供可擴充的架構，以叢集感知的方式更新叢集軟體清查 \- 。 發行者可以使用這項功能來協調未發佈至 Windows Update 或 Microsoft Update 的軟體更新安裝，或不是由 Microsoft 提供，例如，非 \- microsoft 設備磁碟機的更新。

-   CAU 自我 \- 更新模式可讓「機架中的叢集」應用裝置 \( 一組叢集實體機器，通常封裝在一個底座中 \) 以自行更新。 一般來說，這類應用裝置會部署在分公司，只需要最少的當地 IT 支援，就可以管理叢集。 自我 \- 更新模式提供這些部署案例中的絕佳價值。

## <a name="important-functionality"></a>重要功能
以下是重要叢集感知更新功能的說明：

-   使用者介面 \( UI \) -[叢集感知更新] 視窗-以及一組您可以用來預覽、套用、監視和報告更新的 Cmdlet

-   \- \- \- \( \) 由一或多個更新協調器電腦協調的「更新執行」叢集更新操作的端對端自動化

-   預設的外掛程式 \- ，它會與現有的 Windows Update Agent \( WUA \) 和 \( \) Windows Server 中 Windows Server Update Services WSUS 基礎結構整合，以套用重要的 Microsoft 更新

-   第二個外掛程式 \- 可用來套用 microsoft 的修補程式，而且可以自訂以套用非 \- microsoft 更新

-   使用「更新執行」選項 (如每個節點重試更新的次數上限) 來設定「更新執行設定檔」。 「更新執行設定檔」可以讓您在整個「更新執行」時，快速重複使用相同的設定，而且輕鬆地與其他容錯移轉叢集共用更新設定。

-   一種可延伸的架構，支援新 \- 的外掛程式開發，以協調整個叢集的其他節點 \- 更新工具，例如自訂軟體安裝程式、BIOS 更新工具，以及網路介面卡或主機匯流排介面卡 \( HBA \) 更新工具。

叢集感知更新可以兩種模式來協調完整的叢集更新操作：

-   **自我 \- 更新模式**在此模式中，CAU 叢集角色會設定為要更新之容錯移轉叢集上的工作負載，並定義相關聯的更新排程。 叢集會使用預設或自訂的「更新執行」設定檔，在排定的時間自行更新。 在「更新執行」期間，CAU 更新協調器程序先從目前擁有 CAU 叢集角色的節點開始，然後依序在每個叢集節點上執行更新。 若要更新目前的叢集節點，CAU 叢集角色會容錯移轉到其他叢集節點，而該節點上新的更新協調器程序則會取得「更新執行」的控制權。 在自行 \- 更新模式中，CAU 可以使用完全自動化、端 \- 對端更新程式來更新容錯移轉叢集 \- 。 系統管理員也可以視需要 \- 在此模式中觸發更新，或只在 \- 需要時使用遠端更新方法。 在自行 \- 更新模式中，系統管理員可以藉由連接到叢集並執行**get \- get-caurun** Windows PowerShell Cmdlet，來取得正在進行之更新執行的摘要資訊。

-   **遠端 \- 更新模式**在這個模式中，稱為更新協調器的遠端電腦會使用 CAU 工具進行設定。 更新協調器不是「更新執行」期間更新的叢集成員。 系統管理員可以從遠端電腦 \- 使用預設或自訂的「更新執行」設定檔來觸發「隨選更新執行」。 遠端 \- 更新模式很適合用來監視在「 \- 更新執行」期間的即時進度，以及在 Server Core 安裝上執行的叢集。

## <a name="hardware-and-software-requirements"></a>硬體和軟體需求

CAU 可以在所有版本的 Windows Server 上使用，包括 Server Core 安裝。 如需詳細的需求資訊，請參閱叢集[感知更新需求和最佳作法](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安裝叢集感知更新
若要使用 CAU，請在 Windows Server 中安裝容錯移轉叢集功能，並建立容錯移轉叢集。 支援 CAU 功能的元件會自動安裝在每個叢集節點上。

若要安裝容錯移轉功能，可以使用下列工具：
- 伺服器管理員中的 [新增角色及功能精靈]。
- [Install-add-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps)  Windows PowerShell Cmdlet
- 部署映像服務與管理 (DISM) 命令列工具

如需詳細資訊，請參閱[安裝容錯移轉叢集功能](create-failover-cluster.md#install-the-failover-clustering-feature)。

您也必須安裝 [容錯移轉叢集工具]，這是遠端伺服器管理工具的一部分，而且會在您于伺服器管理員中安裝容錯移轉叢集功能時預設安裝。 容錯移轉叢集工具組含叢集感知更新使用者介面和 PowerShell Cmdlet。

您必須如下所示安裝容錯移轉叢集工具，以支援不同的 CAU 更新模式：

- 若要在自行更新模式中使用 CAU \- ，請在每個叢集節點上安裝容錯移轉叢集工具。

- 若要啟用遠端 \- 更新模式，請在具有容錯移轉叢集網路連線能力的電腦上安裝容錯移轉叢集工具。

> [!NOTE]
> -   您無法使用 Windows Server 2012 上的容錯移轉叢集工具來管理較新版本 Windows Server 上的叢集感知更新。
> -   若只要在遠端更新模式中使用 CAU \- ，則不需要在叢集節點上安裝容錯移轉叢集工具。 不過，有些 CAU 功能將無法使用。 如需詳細資訊，請參閱叢集[ \- 感知更新的需求和最佳作法](cluster-aware-updating-requirements.md)。
> -   除非您只在自行更新模式中使用 CAU \- ，否則安裝 cau 工具且協調更新的電腦不能是容錯移轉叢集的成員。

### <a name="enabling-self-updating-mode"></a>啟用自行更新模式
若要啟用自行更新模式，您必須將叢集感知更新叢集角色新增至容錯移轉叢集。 若要執行此動作，請使用下列其中一種方法：
- 在伺服器管理員中，選取 [**工具**] [叢集  >  **感知更新**]，然後在 [叢集感知更新] 視窗中，選取 [**設定叢集自行更新選項**]。
- 在 PowerShell 會話中，執行[add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) Cmdlet。

若要卸載 CAU，請使用伺服器管理員、 [uninstall](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) CMDLET 或 DISM 命令列工具來卸載容錯移轉叢集功能或容錯移轉叢集工具 \- 。

### <a name="additional-requirements-and-best-practices"></a>其他需求和最佳做法

若要確保 CAU 可以順利更新叢集節點，以及取得設定容錯移轉叢集環境以使用 CAU 的其他指導方針，可以執行 CAU 最佳做法分析程式。

如需使用 CAU 的詳細需求和最佳作法，以及執行 CAU 最佳做法分析程式的相關資訊，請參閱叢集[ \- 感知更新的需求和最佳作法](cluster-aware-updating-requirements.md)。

### <a name="starting-cluster-aware-updating"></a>啟動叢集感知更新

##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>從伺服器管理員啟動叢集感知更新

1.  啟動伺服器管理員。

2.  請執行下列其中一項：

    -   在 [**工具**] 功能表上，按一下 [叢集** \- 感知更新**]。

    -   如果一或多個叢集節點或叢集新增至伺服器管理員，請在 [**所有伺服器**] 頁面上，以滑鼠右鍵 \- 按一下節點名稱 \( 或叢集名稱，然後按一下 [ \) **更新**叢集]。

## <a name="additional-references"></a>其他參考
下列連結提供有關使用叢集感知更新的詳細資訊。

-   [叢集感知更新的需求和最佳作法 \-](cluster-aware-updating.md)

-   [叢集 \- 感知更新：常見問題](cluster-aware-updating-faq.md)

-   [CAU 的進階選項和更新執行設定檔](cluster-aware-updating-options.md)

-   [CAU 外掛程式的 \- 工作方式](cluster-aware-updating-plug-ins.md)

-   [\-Windows PowerShell 中的叢集感知更新 Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)

-   [叢集 \- 感知更新外掛程式 \- 參考](/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)


