---
title: "叢集更新概觀"
ms.topic: article
ms.prod: windows-server-threshold
ms.manager: dongill
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 3/23/2017
description: "叢集更新 (CAU) 會自動執行的軟體更新安裝在執行 Windows Server 叢集上。"
ms.assetid: 3c2993b4-aa81-452b-a5c3-3724ad95d892
ms.openlocfilehash: 68336741accabd6e4cb0da5dbd708be707f68df6
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="cluster-aware-updating-overview"></a>叢集更新概觀

> 適用於：Windows Server（以每年次管道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

本主題提供概觀 Cluster\ 感知更新 \(CAU\)，同時也可維護可用性更新程序叢集伺服器上的軟體會自動執行的功能。

> [!NOTE]
> 當更新[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)我們叢集，建議使用叢集更新。
  
## <a name="BKMK_OVER"></a>描述的功能  
自動的功能，可讓您更新中的伺服器叢集更新是[容錯移轉叢集](failover-clustering-overview.md)中可用的更新程序期間甚至不遺失。 在更新執行，叢集更新無障礙可以執行下列工作：  

1. 每個節點叢集放節點維護模式。
2. 將節點關閉叢集的角色。
3. 安裝更新，任何相關的更新。
4. 如有需要，請執行重新開機。
5. 將節點退出維護模式。
6. 還原節點上的叢集的角色。
7. 移至下一個節點的更新。

許多叢集中的角色叢集，自動更新程序觸發計劃容錯移轉。 這造成連接戶端暫時性服務中斷。 不過，如果持續提供工作負載，例如使用即時移轉 Hyper\ HYPER-V 或 SMB 透明容錯移轉檔案伺服器叢集更新可以調整叢集更新的服務的可用性不會影響。    
  
## <a name="practical-applications"></a>實用的應用程式  
  
-   CAU 降低叢集服務中斷，降低需要手動更新因應措施，並使 end\ to\ 高階叢集更新程序更可靠的系統管理員。 持續提供叢集工作負載，例如持續提供檔案伺服器搭配使用的 CAU 功能時 \（SMB 透明 Failover\ 與檔案伺服器工作負載）或 Hyper\ HYPER-V，叢集戶端與服務的可用性零影響可以執行的更新。  
  
-   企業的 CAU 幫助您採用一致 IT 程序。 執行設定檔的更新為種類容錯建立，然後以確保 CAU 部署 IT 組織套用更新一致，即使叢集由不同 lines\ of\-企業或系統管理員所管理的檔案共用的集中管理。  
  
-   CAU 可以排定更新上執行日常、週或每月定期協助協調叢集與其他 IT 管理程序的更新。  
  
-   CAU 提供更新方式 cluster\ 感知叢集軟體庫存最具擴充性的架構。 這可用於透過發行者協調安裝的軟體更新的不 Windows 更新或 Microsoft Update 發行，或是時無法使用，例如 Microsoft non\ Microsoft 裝置驅動程式更新。  
  
-   CAU self\ 更新模式可讓] 方塊中的叢集「應用裝置 \（叢集所在的電腦，通常會在一個 chassis\ 已封裝的一組）本身更新。 通常是這類裝置在最小本機 IT 支援管理叢集分公司部署。 Self\ 更新模式提供在這些案例中部署變得更好的值。  
  
## <a name="important-functionality"></a>重要的功能  
以下是描述的重要更新叢集功能：  
  
-   在使用者介面 \(UI\)-叢集注意更新視窗-和一系列 cmdlet，您可以使用預覽，用於監視，並更新報告  
  
-   Cluster\ 更新作業的 end\ to\ 高階自動化 \ (更新 Run\)，協調一或多個更新協調器電腦  
  
-   與現有的 Windows 更新代理程式 \(WUA\) 和 Windows Server Update Services \(WSUS\) 在 Windows Server 的基礎結構套用重要整合 plug\ 中的預設值 Microsoft 更新  
  
-   第二 plug\ 中，可以用來適用於 Microsoft hotfix，並，您可以自訂套用 non\ Microsoft 更新  
  
-   更新執行設定檔，您所設定的更新執行選項，例如更新將會每個節點重試次數最大的設定。 執行設定檔更新可讓您快速更新執行所有重複使用相同的設定，並輕鬆地與其他容錯分享的更新設定。  
  
-   自訂軟體的安裝程式，BIOS 更新工具，例如，在叢集協調其他 node\ 更新工具和網路介面卡或主機匯流排配接器 \(HBA\) 更新工具的新 plug\ 在開發的支援延伸架構。  
  
叢集更新可以調整完成叢集更新中有兩種模式操作：  
  
-   **Self\ 更新模式**適用於此模式下，CAU 叢集的角色設定為工作負載的更新、容錯移轉叢集上，而且相關的更新排程定義。 叢集更新本身排定的時間使用的預設值或自訂更新執行設定檔。 在更新執行，CAU 更新協調者處理程序開始目前擁有 CAU 叢集的角色節點上並程序依序執行更新每個叢集節點上。 若要更新目前叢集節點，CAU 叢集的角色失敗上到另一個叢集節點，並新的更新協調者處理程序該節點上假設控制項的更新執行。 Self\ 更新模式，CAU 更新容錯移轉叢集使用完全自動化、end\ to\ 高階更新程序。 系統管理員也可以觸發程序更新 on\ 隨選在此模式下，或只如果 remote\ 更新方式的使用。 在 self\ 更新模式中，系統管理員可以取得摘要中相關資訊更新執行進度連接到叢集並執行**Get\-CauRun** Windows PowerShell cmdlet。  
  
-   **Remote\ 更新模式**對於此模式，CAU 工具設定遠端電腦時，這稱為更新協調器。 更新協調不會在更新執行更新叢集的成員。 從遠端電腦時，系統管理員觸發 on\ 隨選更新執行使用預設或自訂更新執行設定檔。 Remote\ 更新模式是適用於監視進行 real\ 階段期間更新執行，以及叢集伺服器核心安裝上執行。  
  
## <a name="hardware-and-software-requirements"></a>硬體與軟體需求  

Windows Server，包括 Server Core 安裝的所有版本上可 CAU。 適用於需求的詳細的資訊，請查看[更新叢集需求及最佳做法，](cluster-aware-updating-requirements.md)。

### <a name="installing-cluster-aware-updating"></a>安裝更新叢集
若要使用 CAU，安裝容錯功能在 Windows Server 並建立容錯移轉叢集。 在每個叢集節點，會自動安裝支援 CAU 功能的元件。

若要安裝的容錯功能，您可以使用下列工具：
- 伺服器管理員中新增角色與精靈中的功能
- [安裝-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature) Windows PowerShell cmdlet
- 部署映像服務與管理 (DISM) 的命令列工具

如需詳細資訊，請查看[安裝或解除安裝的角色，角色服務或功能](https://technet.microsoft.com/library/hh831809(v=ws.11).aspx)。

您還必須安裝容錯移轉叢集工具，遠端伺服器管理工具的一部分，您在伺服器管理員安裝容錯功能時，預設會安裝。 容錯工具包含叢集更新使用者介面及 PowerShell cmdlet。 

您必須安裝容錯移轉叢集工具，如下所示，以支援其他 CAU 更新模式：

- 若要使用 CAU self\ 更新模式，安裝容錯移轉叢集工具每個叢集節點上。   
  
- 要 remote\ 更新模式，請安裝容錯移轉叢集工具的網路錯誤後移轉叢集連接的電腦上。  
  
> [!NOTE]  
> -   您無法使用容錯移轉叢集工具在 Windows Server 2012 上來管理叢集更新上較新的 Windows Server 版本。 
> -   若要使用 CAU 只能在 remote\ 更新模式下，就不需要叢集節點上容錯移轉叢集工具安裝。 不過，某些 CAU 功能將無法使用。 如需詳細資訊，請查看[需求與最佳做法 Cluster\ 感知更新](cluster-aware-updating-requirements.md)。  
> -   除非您正在使用 CAU self\ 更新模式中，的電腦安裝 CAU 工具和，協調更新無法容錯移轉叢集的成員。  
  
### <a name="enabling-self-updating-mode"></a>讓自我更新模式
若要使用的自動更新模式，請您必須容錯移轉叢集新增叢集更新叢集的角色。 若要這樣做，請使用下列其中一個下列方法：
- 在伺服器管理員中，選取 [**工具** > **叢集更新**，然後在 [叢集更新視窗中，選取 [**設定叢集自我更新選項**。 
- 中的 PowerShell 工作階段，請執行[新增-CauClusterRole](https://technet.microsoft.com/itpro/powershell/windows/cluster-aware-updating/add-cauclusterrole) cmdlet。  
  
解除安裝 CAU、使用解除安裝的容錯功能或容錯移轉叢集工具伺服器管理員中，[WindowsFeature 解除安裝的](https://technet.microsoft.com/itpro/powershell/windows/server-manager/uninstall-windowsfeature)cmdlet，或 DISM command\ 列工具。  
  
### <a name="additional-requirements-and-best-practices"></a>額外需求與最佳做法  

若要確保 CAU 可以更新叢集節點成功，以及其他指導方針容錯移轉叢集環境使用 CAU 設定，您可以執行 CAU 最佳做法分析。  
  
詳細的需求與最佳做法使用 CAU 及執行 CAU 最佳做法分析詳細資訊，請查看[需求與最佳做法 Cluster\ 感知更新](cluster-aware-updating-requirements.md)。  
  
### <a name="starting-cluster-aware-updating"></a>開始更新叢集  
  
##### <a name="to-start-cluster-aware-updating-from-server-manager"></a>若要開始叢集更新從伺服器管理員  
  
1.  伺服器管理員 [開始]。  
  
2.  執行下列其中一個動作：  
  
    -   在**工具**功能表上，按**Cluster\ 感知更新**。  
  
    -   如果有一或多個叢集節點或叢集，會在新增到伺服器管理員中，**所有伺服器]**頁面上，按一下 right\ 節點名稱 \（或 cluster\ 的名稱），然後按一下 [**更新叢集**。  
  
## <a name="see-also"></a>也了  
下列連結提供使用叢集更新的詳細資訊。  
  
-   [需求與最佳做法 Cluster\ 感知更新](cluster-aware-updating.md)  
  
-   [Cluster\ 感知更新：常見問題集](cluster-aware-updating-faq.md)  
  
-   [[進階的選項]，然後 CAU 對更新執行設定檔](cluster-aware-updating-options.md)  
  
-   [CAU Plug\ 集的運作方式](cluster-aware-updating-plug-ins.md)  
  
-   [Windows PowerShell 中的 Cluster\ 感知更新 Cmdlet](https://go.microsoft.com/fwlink/p/?LinkId=237675)  
  
-   [Cluster\ 感知更新 Plug\ 中參考資料](https://msdn.microsoft.com/library/hh418084.aspx)  
  

