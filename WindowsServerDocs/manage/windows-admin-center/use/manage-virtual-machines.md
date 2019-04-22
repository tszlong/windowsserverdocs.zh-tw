---
title: 管理虛擬機器與 Windows Admin Center
description: 管理 HYPER-V 主機和虛擬機器的 Windows Admin Center （專案檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 41767b9e53c0106931e78f86f8675e413cca0d0a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816879"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>管理虛擬機器與 Windows Admin Center

>適用於：Windows Admin Center，Windows Admin Center 預覽

虛擬機器工具包含在[伺服器](manage-servers.md)，[容錯移轉叢集](manage-failover-clusters.md)或是[Hyper-Converged 叢集](manage-hyper-converged.md)如果伺服器或叢集上啟用 HYPER-V 角色的連線。 您可以使用虛擬機器的工具來管理 HYPER-V 主機執行 Windows Server 2012 或更新版本中，請安裝桌面體驗，或是做為 Server Core。 也支援 Hyper-V Server 2012 和 2016年。

## <a name="key-features"></a>重要功能

在 Windows Admin Center 中的虛擬機器工具的重點包括：

- **高階 HYPER-V 主機資源監視。** 在單一儀表板中檢視整體的 CPU 和記憶體使用量、 IO 效能計量、 VM 健康情況警示和 HYPER-V 主機伺服器或整個叢集的事件。
- **將 HYPER-V 管理員和容錯移轉叢集管理員的功能結合在一起的一致的體驗。** 檢視整個叢集中的所有虛擬機器，並向下切入至單一虛擬機器的進階的管理和疑難排解。
- **簡化，但功能強大的工作流程的虛擬機器管理。** 建立、 管理及複寫虛擬機器的 IT 管理案例量身打造的全新的 UI 體驗。

以下是一些您可以在 Windows Admin Center 中的 HYPER-V 工作：

- [監視 HYPER-V 主機資源和效能](#monitor-hyper-v-host-resources-and-performance)
- [檢視虛擬機器的清查](#view-virtual-machine-inventory)
- [建立新的虛擬機器](#create-a-new-virtual-machine)
- [變更虛擬機器設定](#change-virtual-machine-settings)
- [即時移轉至另一個叢集節點的 虛擬機器](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [進階的管理和針對單一虛擬機器進行疑難排解](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [檢視 HYPER-V 事件記錄檔](#view-hyper-v-event-logs)
- [利用 Azure Site Recovery 保護虛擬機器](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>監視 HYPER-V 主機資源和效能

![虛擬機器摘要 畫面](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 有兩個索引標籤頂端**虛擬機器**工具**摘要** 索引標籤和**清查** 索引標籤。**摘要** 索引標籤提供全面檢視為 HYPER-V 主機資源和效能，目前的伺服器或整個叢集，包括下列：
    - 依狀態-設為 off，執行的 Vm 數目會暫停，並儲存
    - 最近的健康情況警示或 HYPER-V 事件記錄檔事件 （警示僅適用於執行 Windows Server 2016 的超交集叢集或更新版本）
    - 使用主機與客體分解的 CPU 和記憶體使用量
    - 最上層耗用最多 CPU 和記憶體資源的 Vm
    - 即時和歷史的資料行的 IOPS 和 IO 輸送量圖表 （儲存體效能折線圖是只適用於執行 Windows Server 2016 的超交集叢集或更新版本。 歷程記錄資料僅適用於執行 Windows Server 2019 的超交集叢集）

## <a name="view-virtual-machine-inventory"></a>檢視虛擬機器的清查

![虛擬機器的清查 畫面](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 有兩個索引標籤頂端**虛擬機器**工具**摘要** 索引標籤和**清查** 索引標籤。**清查** 索引標籤會列出在目前的伺服器或整個叢集上的虛擬機器，並提供命令來管理個別的虛擬機器。 您可以：
    - 檢視目前的伺服器或叢集上執行的虛擬機器清單。
    - 如果您要檢視適用於叢集的虛擬機器，請檢視虛擬機器的狀態和主機伺服器。 也請檢視主機的觀點而言，包括記憶體不足的壓力、 記憶體需求和指派的記憶體和虛擬機器的執行時間、 活動訊號狀態和使用 Azure Site Recovery 的保護狀態的 CPU 和記憶體使用量。
    - [建立新的虛擬機器](#create-a-new-virtual-machine)。
    - 刪除、 啟動、 關閉、 關閉、 暫停、 繼續、 重設或重新命名的虛擬機器。 也儲存虛擬機器，刪除已儲存的狀態，或建立檢查點。
    - [變更虛擬機器設定](#change-virtual-machine-settings)。
    - 連接到虛擬機器主控台，透過 HYPER-V 主機使用 VMConnect。
    - [將虛擬機器使用 Azure Site Recovery 複寫](#protect-virtual-machines-with-azure-site-recovery)。

注意：如果您連線到叢集，虛擬機器工具只會顯示叢集的虛擬機器。 我們計劃也會顯示非叢集虛擬機器在未來。

## <a name="create-a-new-virtual-machine"></a>建立新的虛擬機器

![建立新的虛擬機器畫面](../media/manage-virtual-machines/new-vm.png)

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 在 [虛擬機器] 工具頂端，選擇**清查**索引標籤，然後按一下**新增**來建立新的虛擬機器。
3. 輸入虛擬機器名稱，然後選擇層代 1 和 2 的虛擬機器之間。
4. 如果您要在叢集上建立虛擬機器，您可以選擇哪一部主機上，一開始建立虛擬機器。 如果您正在 Windows Server 2016 或更新版本中，此工具時，會為您提供的主控件的建議。
5. 選擇虛擬機器檔案的路徑。 從下拉式清單中選擇的磁碟區，或按一下**瀏覽**選擇資料夾，使用資料夾選擇器。 虛擬機器設定檔和虛擬硬碟檔案會儲存在同一個資料夾下`\Hyper-V\\[virtual machine name]`所選磁碟區或路徑的路徑。
6. 是否已啟用巢狀虛擬化，設定記憶體設定、 網路介面卡、 虛擬硬碟，然後選擇您要從.iso 映像檔，或從網路安裝作業系統，請選擇虛擬處理器的數目。
7. 按一下 [建立]，建立虛擬機器。
8. 一旦虛擬機器已建立，並會出現在虛擬機器清單，您可以啟動虛擬機器。
9. 虛擬機器啟動之後，您可以連線到透過 VMConnect 以安裝作業系統的虛擬機器的主控台。 從清單中選取虛擬機器，請按一下**更多** > **Connect**下載.rdp 檔案。 遠端桌面連線應用程式中開啟.rdp 檔案。 因為這連接到虛擬機器的主控台，您必須輸入 HYPER-V 主機的系統管理員認證。

## <a name="change-virtual-machine-settings"></a>變更虛擬機器設定

![虛擬機器 [設定] 畫面](../media/manage-virtual-machines/vm-settings.png)

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 在 虛擬機器 工具頂端，選擇**清查** 索引標籤。從清單中選擇虛擬機器，然後按一下**更多** > **設定**。
3. 切換**一般**，**記憶體**，**處理器**，**磁碟**，**網路**， **開機順序**並**檢查點**索引標籤上，設定必要的設定，然後按一下 **儲存**儲存目前的索引標籤設定。 可用的設定取決於虛擬機器的世代。 此外，某些設定不能變更執行中虛擬機器，您必須先停止虛擬機器。

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>即時移轉至另一個叢集節點的 虛擬機器

如果您連線到叢集，您可以即時虛擬機器移轉至另一個叢集節點。

1. 從容錯移轉叢集或超交集叢集連線中，按一下**虛擬機器**從左側瀏覽窗格的工具。
2. 在 虛擬機器 工具頂端，選擇**清查** 索引標籤。從清單中選擇虛擬機器，然後按一下**更多** > **移動**。
3. 從可用的叢集節點的清單中選擇伺服器，然後按一下**移動**。
4. 中的 Windows Admin Center 右上角，將會顯示移動進度的通知。 如果移動成功，您會看到變更虛擬機器清單中的主機伺服器名稱。

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>進階的管理和針對單一虛擬機器進行疑難排解

![單一虛擬機器詳細資料畫面](../media/manage-virtual-machines/vm-details.png)

您可以檢視的詳細的資訊和單一虛擬機器從單一虛擬機器 頁面的效能圖表。

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 在 虛擬機器 工具頂端，選擇**清查** 索引標籤。按一下虛擬機器清單中的虛擬機器的名稱。
3. 從單一虛擬機器 頁面中，您可以：
    - 檢視虛擬機器的詳細的資訊。
    - 檢視即時和歷程記錄資料列圖針對 CPU、 記憶體、 網路、 IOPS 和 IO 輸送量 （歷程記錄資料僅適用於執行 Windows Server 2019 的超交集叢集）
    - 檢視、 建立、 套用、 重新命名和刪除檢查點。
    - 檢視虛擬機器的虛擬硬碟 (.vhd) 檔案、 網路介面卡和主機伺服器的詳細資料。
    - 刪除、 啟動、 關閉、 關閉、 暫停、 繼續、 重設或重新命名虛擬機器。 也儲存虛擬機器，刪除已儲存的狀態，或建立檢查點。
    - [變更虛擬機器設定](#change-virtual-machine-settings)。
    - 連線到透過 HYPER-V 主機使用 VMConnect 的虛擬機器主控台。
    - [使用 Azure Site Recovery 的虛擬機器複寫](#protect-virtual-machines-with-azure-site-recovery)。

## <a name="view-hyper-v-event-logs"></a>檢視 HYPER-V 事件記錄檔

您可以檢視 HYPER-V 事件記錄檔，直接從虛擬機器的工具。

1. 按一下 **虛擬機器**從左側瀏覽窗格的工具。
2. 在 虛擬機器 工具頂端，選擇**摘要** 索引標籤。在最右邊的 事件 區段中，按一下**檢視所有事件**。
3. 事件檢視器工具會顯示在左窗格中的 HYPER-V 事件通道。 選擇右窗格中檢視事件的通道。 如果您要管理的容錯移轉叢集或超交集叢集，事件記錄檔會顯示所有叢集節點，顯示在主機伺服器的機器資料行中的事件。

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>利用 Azure Site Recovery 保護虛擬機器

若要設定 Azure Site Recovery 及將您的內部部署虛擬機器複寫至 Azure，您可以使用 Windows Admin Center。 [深入了解](azure-services.md)

## <a name="more-coming"></a>更多即將推出

Windows Admin Center 中的虛擬機器管理目前正在開發，並將在不久的將來加入新功能。 您可以檢視 狀態 和 票選功能在 UserVoice 中：

|功能要求|
|-------|
|[匯入/匯出虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[排序資料夾中的虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[支援其他虛擬機器設定](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[HYPER-V 複本支援](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[委派的虛擬機器的擁有權](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[再製虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[從現有的虛擬機器建立範本](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[檢視虛擬機器在 HYPER-V 主機](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[在新的虛擬機器 窗格中設定 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**查看所有或提出新功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
