---
title: 叢集感知更新概觀
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 08/06/2018
description: 叢集感知更新 (CAU) 會自動在執行 Windows Server 的叢集上的軟體更新安裝。
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 77ccfe7dc9a769d602ff069d5f1d8e77522aa001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827419"
---
# <a name="cluster-aware-updating-overview"></a>叢集感知更新概觀

> 適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

本主題提供叢集的概觀\-感知更新\(CAU\)，這個功能可以自動化的軟體更新程序，在叢集伺服器，同時維持可用性。

> [!NOTE]
> 更新時[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)叢集，建議使用叢集感知更新。
  
## <a name="BKMK_OVER"></a>功能描述  
叢集感知更新是自動化的功能，可讓您更新中的伺服器[容錯移轉叢集](failover-clustering-overview.md)幾乎不會遺失在更新程序期間的可用性。 更新執行 」 期間，叢集感知更新會明確地會執行下列工作：  

1. 每個叢集節點放入節點維護模式。
2. 將叢集的角色從節點移除。
3. 安裝更新以及任何相依的更新。
4. 如有必要，請執行重新啟動。
5. 將節點移出維護模式。
6. 還原的節點上的叢集的角色。
7. 移至下一個節點的更新。

對於叢集中的許多叢集角色而言，自動化更新程序會觸發計劃的容錯移轉。 這對連線的用戶端會造成暫時性的服務中斷。 不過，在持續可用的工作負載，例如 Hyper-v 的情況下\-與即時移轉或檔案伺服器具有 SMB 透明容錯移轉、 叢集感知更新的 V 可以協調叢集更新，而不會影響到服務可用性。    
  
## <a name="practical-applications"></a>實際應用  
  
-   CAU 會減少叢集服務的服務中斷時間，可減少需要進行手動更新因應措施，並結束\-至\-端叢集更新程序，更可靠的系統管理員。 CAU 功能與持續可用叢集工作負載，如持續可用的檔案伺服器一起使用時\(具有 SMB 透明容錯移轉的檔案伺服器工作負載\)或 Hyper-v\-V，叢集與用戶端對服務可用性沒有任何影響，就可以執行更新。  
  
-   CAU 讓整個企業採用一致的 IT 程序。 更新執行設定檔可以針對建立容錯移轉叢集的不同類別，並接著在檔案共用，以確保整個 IT 組織的 CAU 部署會以一致的方式套用更新即使叢集都由不同的行上集中管理\-的\-商務或系統管理員。  
  
-   CAU 的「更新執行」排程可以是每天、每週、或每月的間隔，幫助協調叢集更新與其他 IT 管理程序。  
  
-   CAU 提供可延伸的架構，來更新叢集軟體清查，在叢集中\-的方式。 這可供 「 發行者 」 來協調安裝的軟體更新，不會發行到 Windows Update 或 Microsoft Update，或不是可從 Microsoft 取得，例如，更新非\-Microsoft 裝置驅動程式。  
  
-   CAU 自行\-更新模式可讓 「 箱內叢集 」 設備\(叢集的實體機器，通常封裝在一個底座中一組\)自行更新。 一般來說，這類應用裝置會部署在分公司，只需要最少的當地 IT 支援，就可以管理叢集。 自助\-更新模式提供絕佳的價值，這些部署案例中。  
  
## <a name="important-functionality"></a>重要功能  
以下是重要的叢集感知更新功能的說明：  
  
-   使用者介面\(UI\) -叢集感知更新的窗口-和一組 cmdlet，可以用來預覽、 套用、 監視以及報告更新  
  
-   結束\-要\-結尾的叢集中的自動化\-更新作業\(「 更新執行\)、 協調的一或多個更新協調器 」 電腦  
  
-   預設外掛程式\-中，整合了現有的 Windows Update agent \(WUA\)和 Windows Server Update Services \(WSUS\)套用重要的 Microsoft Windows Server 中的基礎結構更新  
  
-   第二個外掛程式\-單元，可用來套用 Microsoft hotfix，並可自訂以套用非\-Microsoft 更新  
  
-   使用「更新執行」選項 (如每個節點重試更新的次數上限) 來設定「更新執行設定檔」。 「更新執行設定檔」可以讓您在整個「更新執行」時，快速重複使用相同的設定，而且輕鬆地與其他容錯移轉叢集共用更新設定。  
  
-   支援新的隨插即用的可延伸架構\-來協調其他節點的開發\-BIOS 更新工具，以及網路介面卡或主機匯流排介面卡分散到叢集中，如自訂軟體安裝程式、更新工具\(HBA\)更新工具。  
  
叢集感知更新來協調完整的叢集更新作業，在兩種模式：  
  
-   **自助\-更新模式**此模式中，CAU 叢集的角色設定成要更新之容錯移轉叢集上的工作負載，並定義相關聯的更新排程。 叢集會使用預設或自訂的「更新執行」設定檔，在排定的時間自行更新。 在「更新執行」期間，CAU 更新協調器程序先從目前擁有 CAU 叢集角色的節點開始，然後依序在每個叢集節點上執行更新。 若要更新目前的叢集節點，CAU 叢集角色會容錯移轉到其他叢集節點，而該節點上新的更新協調器程序則會取得「更新執行」的控制權。 在自我\-更新模式，CAU 可以更新容錯移轉叢集使用完全自動化，結束\-到\-更新程序的結尾。 系統管理員也可以在觸發更新\-要求在此模式中，或直接使用遠端\-更新方法，如有需要。 在自我\-更新模式，系統管理員可以取得 「 更新執行的摘要資訊中連線到叢集並執行**取得\-CauRun** Windows PowerShell cmdlet。  
  
-   **遠端\-更新模式**遠端的電腦，稱為更新協調器，此模式中，會設定使用 CAU 工具。 更新協調器不是「更新執行」期間更新的叢集成員。 從遠端電腦，系統管理員就會觸發上\-要求使用預設或自訂的 「 更新執行設定檔的 更新執行。 遠端\-更新模式適合用來監視真實\-時間進行更新執行 」 期間，以及的 Server Core 安裝執行的叢集。  
  
## <a name="hardware-and-software-requirements"></a>硬體與軟體需求  

CAU 可以用於所有版本的 Windows Server，包括 Server Core 安裝。 如需詳細的需求詳細資訊，請參閱[叢集感知更新需求和最佳做法](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安裝叢集感知更新
若要使用 CAU，安裝 Windows Server 中的 「 容錯移轉叢集 」 功能，並建立容錯移轉叢集。 支援 CAU 功能的元件會自動安裝在每個叢集節點上。

若要安裝容錯移轉功能，可以使用下列工具：
- 伺服器管理員中的 [新增角色及功能精靈]。
- [Install-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/Install-WindowsFeature?view=winserver2012r2-ps&viewFallbackFrom=win10-ps) Windows PowerShell cmdlet
- 部署映像服務與管理 (DISM) 命令列工具

如需詳細資訊，請參閱 <<c0> [ 安裝容錯移轉叢集功能](create-failover-cluster.md#install-the-failover-clustering-feature)。

您也必須安裝 「 容錯移轉叢集工具，這是遠端伺服器管理工具的一部分，並在 [伺服器管理員] 中安裝 「 容錯移轉叢集 」 功能時預設會安裝。 容錯移轉叢集工具包含叢集感知更新使用者介面和 PowerShell cmdlet。 

您必須如下所示安裝容錯移轉叢集工具，以支援不同的 CAU 更新模式：

- 若要使用 CAU 中自我\-更新模式中，每個叢集節點上安裝容錯移轉叢集工具。   
  
- 若要啟用遠端\-更新模式中，具有網路連線到容錯移轉叢集的電腦上安裝容錯移轉叢集工具。  
  
> [!NOTE]  
> -   您無法使用 Windows Server 2012 上的容錯移轉叢集工具來管理較新版的 Windows Server 上的 叢集感知更新。 
> -   若要使用 CAU 只在遠端\-更新模式，在叢集節點上的容錯移轉叢集工具是不需要安裝。 不過，有些 CAU 功能將無法使用。 如需詳細資訊，請參閱 <<c0> [ 需求和叢集的最佳做法\-感知更新](cluster-aware-updating-requirements.md)。  
> -   除非您使用 CAU，只能在自我\-更新模式，在其安裝 CAU 工具，且協調更新的電腦不能在容錯移轉叢集的成員。  
  
### <a name="enabling-self-updating-mode"></a>啟用自行更新模式
若要啟用自行更新模式，您必須新增叢集感知更新的叢集的角色容錯移轉叢集。 若要這樣做，請使用下列方法之一：
- 在 [伺服器管理員] 中，選取**工具** > **叢集感知更新**，然後在 [叢集感知更新] 視窗中，選取**設定叢集自行更新選項**. 
- 在 PowerShell 工作階段中，執行[Add-cauclusterrole](https://docs.microsoft.com/powershell/module/clusterawareupdating/Add-CauClusterRole?view=win10-ps) cmdlet。  
  
若要解除安裝 CAU，請使用 [伺服器管理員] 來解除安裝容錯移轉叢集功能或容錯移轉叢集工具[Uninstall-windowsfeature](https://docs.microsoft.com/powershell/module/servermanager/Uninstall-WindowsFeature?view=win10-ps) cmdlet 或 DISM 命令\-列工具。  
  
### <a name="additional-requirements-and-best-practices"></a>其他需求和最佳做法  

若要確保 CAU 可以順利更新叢集節點，以及取得設定容錯移轉叢集環境以使用 CAU 的其他指導方針，可以執行 CAU 最佳做法分析程式。  
  
如需詳細的需求和使用 CAU，以及執行 CAU 最佳做法分析程式相關資訊的最佳做法，請參閱[需求和叢集的最佳做法\-感知更新](cluster-aware-updating-requirements.md)。  
  
### <a name="starting-cluster-aware-updating"></a>啟動叢集感知更新  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>若要啟動叢集感知更新從 伺服器管理員  
  
1.  啟動 [伺服器管理員]。  
  
2.  執行下列其中一項：  
  
    -   在 **工具**功能表上，按一下**叢集\-感知更新**。  
  
    -   如果在一或多個叢集節點或叢集新增至 伺服器管理員 中，在**所有伺服器**頁面上，從右\-按一下 節點名稱\(或叢集的名稱\)，然後按一下  **更新叢集**。  
  
## <a name="see-also"></a>另請參閱  
下列連結提供有關使用叢集感知更新的詳細資訊。  
  
-   [需求和最佳做法適用於叢集\-感知更新](cluster-aware-updating.md)  
  
-   [叢集\-感知更新：常見問題集](cluster-aware-updating-faq.md)  
  
-   [進階的選項和 cau 更新執行設定檔](cluster-aware-updating-options.md)  
  
-   [CAU 如何插入\-單元工作](cluster-aware-updating-plug-ins.md)  
  
-   [叢集\-感知更新 Windows PowerShell 中的 Cmdlet](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps&viewFallbackFrom=winserverr2-ps)  
  
-   [叢集\-感知更新外掛程式\-參考中](https://docs.microsoft.com/previous-versions/windows/desktop/mscs/cluster-aware-update-plug-in-interfaces-and-classes)  
  

