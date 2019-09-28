---
title: 使用 Windows 系統管理中心管理虛擬機器
description: 使用 Windows 系統管理中心管理 Hyper-v 主機和虛擬機器（Project 檀香山）
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 02a33383b466e8bade2db0bbddaff66f0196954c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356863"
---
# <a name="managing-virtual-machines-with-windows-admin-center"></a>使用 Windows 系統管理中心管理虛擬機器

>適用於：Windows Admin Center、Windows Admin Center 預覽版

如果伺服器或叢集上已啟用 Hyper-v 角色，則[伺服器](manage-servers.md)、[容錯移轉](manage-failover-clusters.md)叢集或[超](manage-hyper-converged.md)交集叢集連線中都會提供虛擬機器工具。 您可以使用 [虛擬機器] 工具來管理執行 Windows Server 2012 或更新版本的 Hyper-v 主機，其安裝時使用桌面體驗或做為 Server Core。 也支援 hyper-v 伺服器2012、2016和2019。

## <a name="key-features"></a>重要功能

Windows 系統管理中心內的虛擬機器工具的重點包括：

- **高階 Hyper-v 主機資源監視。** 在單一儀表板中，查看 Hyper-v 主機伺服器或整個叢集的整體 CPU 和記憶體使用量、IO 效能計量、VM 健康情況警示和事件。
- **整合體驗讓 Hyper-v 管理員和容錯移轉叢集管理員的功能。** 查看整個叢集中的所有虛擬機器，並向下切入至單一虛擬機器，以進行先進的管理和疑難排解。
- **簡化但功能強大的虛擬機器管理工作流程。** 針對 IT 系統管理案例量身打造的新 UI 體驗，以建立、管理和複寫虛擬機器。

以下是您可以在 Windows 管理中心執行的一些 Hyper-v 工作：

- [監視 Hyper-v 主機資源和效能](#monitor-hyper-v-host-resources-and-performance)
- [查看虛擬機器清查](#view-virtual-machine-inventory)
- [建立新的虛擬機器](#create-a-new-virtual-machine)
- [變更虛擬機器設定](#change-virtual-machine-settings)
- [將虛擬機器即時移轉至另一個叢集節點](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [單一虛擬機器的先進管理和疑難排解](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [透過 Hyper-v 主機（VMConnect）管理虛擬機器](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [變更 Hyper-v 主機設定](#change-hyper-v-host-settings)
- [查看 Hyper-v 事件記錄檔](#view-hyper-v-event-logs)
- [使用 Azure Site Recovery 保護虛擬機器](#protect-virtual-machines-with-azure-site-recovery)

## <a name="monitor-hyper-v-host-resources-and-performance"></a>監視 Hyper-v 主機資源和效能

![虛擬機器摘要畫面](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. [**虛擬機器**] 工具頂端有兩個索引標籤，[**摘要**] 索引標籤和 [**清查**] 索引標籤。[**摘要**] 索引標籤提供 hyper-v 主機資源的整體觀點，以及目前伺服器或整個叢集的效能，包括下列各項：
    - 依狀態（執行中、已關閉、已暫停和已儲存）分組的 Vm 數目
    - 最近的健康情況警示或 Hyper-v 事件記錄檔事件（警示僅適用于執行 Windows Server 2016 或更新版本的超融合叢集）
    - 具有主機與來賓細目的 CPU 和記憶體使用量
    - 耗用最多 CPU 和記憶體資源的最佳 Vm
    - IOPS 和 IO 輸送量的即時和歷程記錄資料折線圖（儲存體效能折線圖僅適用于執行 Windows Server 2016 或更新版本的超融合叢集）。 歷程記錄資料僅適用于執行 Windows Server 2019 的超融合式叢集）

## <a name="view-virtual-machine-inventory"></a>查看虛擬機器清查

![虛擬機器清查畫面](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. [**虛擬機器**] 工具頂端有兩個索引標籤，[**摘要**] 索引標籤和 [**清查**] 索引標籤。[**清查**] 索引標籤會列出目前伺服器或整個叢集上可用的虛擬機器，並提供用來管理個別虛擬機器的命令。 您可以：
    - 查看目前伺服器或叢集上執行的虛擬機器清單。
    - 如果您要查看叢集的虛擬機器，請參閱虛擬機器的狀態和主機伺服器。 也從主機的觀點來看 CPU 和記憶體使用量，包括記憶體壓力、記憶體需求和指派的記憶體，以及虛擬機器的執行時間、使用 Azure Site Recovery 的心跳狀態和保護狀態。
    - [建立新的虛擬機器](#create-a-new-virtual-machine)。
    - 刪除、啟動、關閉、關閉、暫停、繼續、重設或重新命名虛擬機器。 另請儲存虛擬機器、刪除儲存的狀態，或建立檢查點。
    - [變更虛擬機器的設定](#change-virtual-machine-settings)。
    - 透過 Hyper-v 主機使用 VMConnect 連線到虛擬機器主控台。
    - [使用 Azure Site Recovery 複寫虛擬機器](#protect-virtual-machines-with-azure-site-recovery)。
    - 針對可在多個 Vm 上執行的作業（例如 [開始]、[關機]、[儲存]、[暫停]、[刪除]、[重設]），您可以選取多個 Vm，並同時執行操作

注意：如果您已連線到叢集，虛擬機器工具將只會顯示叢集虛擬機器。 我們預計未來也會顯示非叢集化的虛擬機器。

## <a name="create-a-new-virtual-machine"></a>建立新的虛擬機器

![[建立新的虛擬機器] 畫面](../media/manage-virtual-machines/new-vm.png)

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤，然後按一下 [**新增**] 以建立新的虛擬機器。
3. 輸入虛擬機器名稱，然後選擇 [第1代] 和 [2 部] 虛擬機器。
4. 如果您要在叢集上建立虛擬機器，您可以選擇初始建立虛擬機器的主機。 如果您執行的是 Windows Server 2016 或更新版本，此工具會為您提供主機建議。
5. 選擇虛擬機器檔案的路徑。 從下拉式清單中選擇磁片區，或按一下 **[流覽]** 以選擇使用資料夾選擇器的資料夾。 虛擬機器組態檔和虛擬硬碟檔案會儲存在所選磁片區或路徑`\Hyper-V\\[virtual machine name]`路徑底下的單一資料夾中。

   >[!Tip]
   > 在資料夾選擇器中，您可以在 [**資料夾名稱**] 欄位中輸入路徑，如 ```\\server\share```，以流覽到網路上任何可用的 SMB 共用。 使用 VM 儲存體的網路共用需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)。

6. 選擇虛擬處理器數目，不論您是否要啟用嵌套虛擬化、設定記憶體設定、網路介面卡、虛擬硬碟，以及選擇要從 .iso 映像檔案或從網路安裝作業系統。
7. 按一下 [建立]，建立虛擬機器。
8. 一旦建立虛擬機器並出現在虛擬機器清單中，您就可以啟動虛擬機器。
9. 虛擬機器啟動之後，您可以透過 VMConnect 連線至虛擬機器的主控台，以安裝作業系統。 從清單中選取虛擬機器，按一下 [**更多** > **連接]** 以下載 .rdp 檔案。 在遠端桌面連線應用程式中開啟 .rdp 檔案。 因為這是連接到虛擬機器的主控台，所以您必須輸入 Hyper-v 主機的系統管理員認證。

## <a name="change-virtual-machine-settings"></a>變更虛擬機器設定

![虛擬機器設定畫面](../media/manage-virtual-machines/vm-settings.png)

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤。從清單中選擇虛擬機器，然後按一下 [**更多** > **設定**]。
3. 在 [**一般**]、 **[安全性**]、[**記憶體**]、[**處理器**]、[**磁片** **]、[** **開機順序**] 和 [**檢查點**] 索引標籤之間切換，設定必要的設定，**然後按一下**目前索引標籤的設定。 可用的設定會根據虛擬機器的世代而有所不同。 此外，某些設定無法變更以執行虛擬機器，而且您必須先停止虛擬機器。

## <a name="live-migrate-a-virtual-machine-to-another-cluster-node"></a>將虛擬機器即時移轉至另一個叢集節點

如果您已連線到叢集，您可以將虛擬機器即時移轉至另一個叢集節點。

1. 從容錯移轉叢集或超融合叢集連線，按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤。從清單中選擇虛擬機器，然後按一下 [**更多** >  個] [**移動**]。
3. 從可用的叢集節點清單中選擇伺服器，然後按一下 [**移動**]。
4. 移動進度的通知會顯示在 Windows 系統管理中心的右上角。 如果移動成功，您會在虛擬機器清單中看到主機伺服器名稱已變更。

## <a name="advanced-management-and-troubleshooting-for-a-single-virtual-machine"></a>單一虛擬機器的先進管理和疑難排解

![單一虛擬機器詳細資料畫面](../media/manage-virtual-machines/vm-details.png)

您可以從 [單一虛擬機器] 頁面，查看單一虛擬機器的詳細資訊和效能圖表。

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤。從 [虛擬機器] 清單中，按一下虛擬機器的名稱。
3. 從 [單一虛擬機器] 頁面，您可以：
    - 查看虛擬機器的詳細資訊。
    - 查看 CPU、記憶體、網路、IOPS 和 IO 輸送量的即時和歷程記錄資料折線圖（歷程記錄資料僅適用于執行 Windows Server 2019 的超融合叢集）
    - View、create、apply、rename 和 delete 檢查點。
    - 查看虛擬機器的虛擬硬碟（.vhd）檔案、網路介面卡和主機伺服器的詳細資料。
    - 刪除、啟動、關閉、關閉、暫停、繼續、重設或重新命名虛擬機器。 另請儲存虛擬機器、刪除儲存的狀態，或建立檢查點。
    - [變更虛擬機器的設定](#change-virtual-machine-settings)。
    - 透過 Hyper-v 主機使用 VMConnect 連線到虛擬機器主控台。
    - [使用 Azure Site Recovery 複寫虛擬機器](#protect-virtual-machines-with-azure-site-recovery)。

## <a name="manage-a-virtual-machine-through-the-hyper-v-host-vmconnect"></a>透過 Hyper-v 主機（VMConnect）管理虛擬機器

![VM 透過您的網頁瀏覽器連接](../media/manage-virtual-machines/vm-connect.png)

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**清查**] 索引標籤。從清單中選擇虛擬機器，然後按一下 [**更多** >  **] [連線] 或 [** **下載 RDP**檔案]。 **[連線]** 可讓您透過遠端桌面 web 主控台（整合至 Windows 管理中心）與來賓 VM 互動。 **下載 rdp**檔案將會下載您可以使用遠端桌面連線應用程式（mstsc）開啟的 .rdp 檔案。 這兩個選項都會使用 VMConnect 透過 Hyper-v 主機連接到來賓 VM，而且需要您輸入 Hyper-v 主機伺服器的系統管理員認證。

## <a name="change-hyper-v-host-settings"></a>變更 Hyper-v 主機設定

![Hyper-v 主機設定畫面](../media/manage-virtual-machines/host-settings.png)

1. 在伺服器、超交集叢集或容錯移轉叢集連線上，按一下左側流覽窗格底部的 [**設定**] 功能表。
2. 在 Hyper-v 主機伺服器或叢集上，您會看到包含下列各節的**Hyper-v 主機設定**群組：
    - 大體變更虛擬硬碟和虛擬機器檔案路徑，以及管理程式排程類型（如果支援）
    - 增強的會話模式
    - NUMA 跨越
    - 即時移轉
    - 儲存體遷移
3. 如果您在超融合式叢集或容錯移轉叢集連線中進行任何 Hyper-v 主機設定變更，變更將會套用至所有叢集節點。

## <a name="view-hyper-v-event-logs"></a>查看 Hyper-v 事件記錄檔

您可以直接從 [虛擬機器] 工具查看 Hyper-v 事件記錄檔。

1. 按一下左側流覽窗格中的 [**虛擬機器**] 工具。
2. 在 [虛擬機器] 工具的頂端，選擇 [**摘要**] 索引標籤。在 [最右側事件] 區段中，按一下 [**查看所有事件**]。
3. [事件檢視器] 工具會在左窗格中顯示 Hyper-v 事件通道。 選擇通道以在右窗格中查看事件。 如果您要管理容錯移轉叢集或超交集叢集，事件記錄檔會顯示所有叢集節點的事件，並在 [電腦] 資料行中顯示主機伺服器。

## <a name="protect-virtual-machines-with-azure-site-recovery"></a>使用 Azure Site Recovery 保護虛擬機器

您可以使用 Windows 管理中心來設定 Azure Site Recovery，並將內部部署虛擬機器複寫至 Azure。 [深入了解](../azure/azure-site-recovery.md)

## <a name="more-coming"></a>更多即將推出

Windows 管理中心的虛擬機器管理目前正在開發中，而新功能將在不久的未來新增。 您可以在 UserVoice 中查看功能的狀態和投票：

- [匯入/匯出虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)
- [排序資料夾中的虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)
- [支援其他虛擬機器設定](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)
- [Hyper-v 複本支援](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)
- [委派虛擬機器擁有權](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)
- [複製虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)
- [從現有的虛擬機器建立範本](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)
- [跨 Hyper-v 主機查看虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)
- [在 [新增虛擬機器] 窗格中設定 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)

[查看全部或提議新功能](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)。
