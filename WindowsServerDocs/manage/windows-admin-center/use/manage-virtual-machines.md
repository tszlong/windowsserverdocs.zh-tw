---
title: 使用 Windows Admin Center 管理虛擬機器
description: 管理 HYPER-V 主機和虛擬機器使用 Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 374ee30dcbaf9af3caa60ee85ec59fd3c158206b
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296750"
---
# 使用 Windows Admin Center 管理虛擬機器

>適用於：Windows Admin Center、Windows Admin Center 預覽版

如果伺服器或叢集上啟用 HYPER-V 角色的[伺服器](manage-servers.md)、[容錯移轉叢集](manage-failover-clusters.md)或[超融合式叢集](manage-hyper-converged.md)連線使用虛擬機器工具。 您可以使用虛擬機器工具來管理 HYPER-V 主機執行 Windows Server 2012 或更新版本，無論是安裝含有桌面體驗或 Server Core。 HYPER-V Server 2012、 2016年和 2019年也支援。

## 主要功能

Windows Admin Center 中的虛擬機器工具醒目提示包括：

- **高階 HYPER-V 主機資源監視。** 單一儀表板中檢視整體的 CPU 和記憶體使用量、 IO 效能計量、 VM 健康狀態警示和 HYPER-V 主機伺服器或整個叢集的事件。
- **它將 HYPER-V 管理員和容錯移轉叢集管理員功能一起一致的體驗。** 檢視所有虛擬機器整個叢集和向下切入到單一虛擬機器的進階的管理和疑難排解。
- **適用於虛擬機器管理簡化，但強大工作流程。** 新的 UI 體驗量身訂做，建立、 管理及複寫虛擬機器的 IT 管理案例。

以下是一些您可以在 Windows Admin Center 中的 HYPER-V 工作：

- [監視 HYPER-V 主機資源和效能](#monitor-hyper-v-host-resources-and-performance)
- [檢視虛擬機器清查](#view-virtual-machine-inventory)
- [建立新的虛擬機器](#create-a-new-virtual-machine)
- [變更虛擬機器設定](#change-virtual-machine-settings)
- [即時移轉至另一個叢集節點的虛擬機器](#live-migrate-a-virtual-machine-to-another-cluster-node)
- [進階的管理及疑難排解針對單一虛擬機器](#advanced-management-and-troubleshooting-for-a-single-virtual-machine)
- [管理虛擬機器透過 HYPER-V 主機 (VMConnect)](#manage-a-virtual-machine-through-the-hyper-v-host-vmconnect)
- [變更 HYPER-V 主機設定](#change-hyper-v-host-settings)
- [檢視 HYPER-V 事件記錄檔](#view-hyper-v-event-logs)
- [保護使用 Azure Site Recovery 虛擬機器](#protect-virtual-machines-with-azure-site-recovery)

## 監視 HYPER-V 主機資源和效能

![虛擬機器摘要] 畫面](../media/manage-virtual-machines/virtual-machines-summary.png)

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 有兩個索引標籤頂端的**虛擬機器**工具、**摘要**] 索引標籤和 [**詳細目錄**] 索引標籤。**摘要**] 索引標籤提供的完整檢視的 HYPER-V 主機資源和目前的伺服器或整個叢集，包括下列的效能：
    - 暫停狀態-執行，關閉，以為單位分組的 Vm 數目，並儲存
    - 最近的健康狀態警示或 HYPER-V 事件記錄檔的事件 （警示是僅適用於執行 Windows Server 2016 超融合式叢集或更新版本）
    - CPU 和記憶體使用量與主機與客體明細
    - 頂端耗用的最 CPU 和記憶體資源的 Vm
    - 即時和歷程記錄資料行的 IOPS 及 IO 輸送量圖表 （存放裝置效能行圖表是僅適用於執行 Windows Server 2016 超融合式叢集或更新版本。 歷史資料只適用於執行 Windows Server 2019 的超融合式叢集）

## 檢視虛擬機器清查

![虛擬機器清查畫面](../media/manage-virtual-machines/virtual-machines-inventory.png)

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 有兩個索引標籤頂端的**虛擬機器**工具、**摘要**] 索引標籤和 [**詳細目錄**] 索引標籤。[**詳細目錄**] 索引標籤會列出目前的伺服器或整個叢集上, 可用的虛擬機器，並提供管理個別的虛擬機器的命令。 您可以：
    - 檢視目前的伺服器或叢集上執行的虛擬機器的清單。
    - 如果您目前檢視在叢集中的虛擬機器，請檢視 [虛擬機器的狀態和主機伺服器。 也檢視主機的觀點而言，包括記憶體壓力、 記憶體需求和受指派的記憶體，和虛擬機器的執行時間、 活動訊號狀態，以及使用 Azure Site Recovery 保護狀態的 CPU 和記憶體使用量。
    - [建立新的虛擬機器](#create-a-new-virtual-machine)。
    - 刪除、 啟動、 關閉、 關機、 暫停、 繼續、 重設或重新命名虛擬機器。 也儲存虛擬機器、 刪除儲存的狀態，或建立檢查點。
    - [針對虛擬機器的變更設定](#change-virtual-machine-settings)。
    - 若要使用 VMConnect 透過 HYPER-V 主機的虛擬機器主控台連線。
    - [使用 Azure Site Recovery 進行虛擬機器的複寫](#protect-virtual-machines-with-azure-site-recovery)。
    - 可在多個虛擬機器，例如 [開始] 畫面上, 執行關機，儲存、 暫停，作業刪除，請重設，您可以選取多個虛擬機器並執行一次的作業。

注意： 如果您已連線至叢集，虛擬機器工具只會顯示叢集化的虛擬電腦。 我們計劃也會顯示在未來非叢集虛擬機器。

## 建立新的虛擬機器

![建立新的虛擬機器畫面](../media/manage-virtual-machines/new-vm.png)

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**詳細目錄**] 索引標籤，然後按一下 \ [**新增]** 來建立新的虛擬機器。
3. 輸入的虛擬機器名稱，然後選擇 \ [1 和 2 代虛擬機器。
4. 如果您要在叢集上建立虛擬機器，您可以選擇哪些主機上，一開始建立虛擬機器。 如果您正在執行 Windows Server 2016 或更新版本，此工具會為您提供主機建議。
5. 選擇的虛擬機器檔案的路徑。 從下拉式清單中選擇磁碟區，或按一下 [**瀏覽**選擇資料夾，使用資料夾選擇器。 將下的單一資料夾中儲存的虛擬機器組態檔和虛擬硬碟檔案`\Hyper-V\\[virtual machine name]`選取的磁碟區或路徑的路徑。

>[!Tip]
> 在資料夾選擇器中，您可以瀏覽至任何可用的 SMB 共用網路上藉由在做為**資料夾名稱**] 欄位中輸入路徑```\\server\share```。 使用網路共用做為 VM 的儲存空間將會需要[CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)。

6. 是否啟用巢狀虛擬化，設定記憶體設定、 網路介面卡、 虛擬硬碟，然後選擇是否要從.iso 映像檔案，或從網路安裝作業系統，請選擇虛擬處理器，數目。
7. 按一下 [**建立**來建立虛擬機器。
8. 一旦建立虛擬機器，並出現在虛擬機器清單，您就可以開始在虛擬機器。
9. 一旦啟動虛擬機器時，您可以連線到透過 VMConnect 安裝作業系統的虛擬機器的主控台。 從清單中選取虛擬機器，請按一下 [**更多** > 以下載.rdp 檔案的**連線**。 遠端桌面連線應用程式中開啟.rdp 檔案。 因為這連線到虛擬機器的主控台，您將需要輸入 HYPER-V 主機的系統管理員認證。

## 變更虛擬機器設定

![虛擬機器設定 \] 畫面](../media/manage-virtual-machines/vm-settings.png)

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**詳細目錄**] 索引標籤從清單中選擇在虛擬機器，然後按一下 [**更多** > **設定**。
3. 在**一般**、**安全性**、**記憶體**、**處理器**、**磁碟**、**網路**、**開機順序，** 以及**檢查點**的索引標籤之間切換，設定必要的設定，然後按一下 [**儲存**] 以儲存目前的索引標籤的設定。 可用的設定將會虛擬機器的世代而有所不同。 此外，用來執行虛擬機器無法變更某些設定，您將需要先停止虛擬機器。

## 即時移轉至另一個叢集節點的虛擬機器

如果您已連線至叢集，但您可以將虛擬機器到另一個叢集節點即時移轉。

1. 從容錯移轉叢集或超融合式叢集連線中，按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**詳細目錄**] 索引標籤從清單中選擇在虛擬機器，然後按一下 [**更多** > **移動**。
3. 從可用的叢集節點的清單中選擇一個伺服器，然後按一下 [**移動**。
4. 移動進度的通知將會顯示在 Windows Admin Center 右上角。 如果移動成功，您會看到變更虛擬機器清單中的主機伺服器名稱。

## 進階的管理及疑難排解針對單一虛擬機器

![單一虛擬機器詳細資料] 畫面](../media/manage-virtual-machines/vm-details.png)

您可以檢視的詳細的資訊和效能圖表針對單一虛擬機器從單一虛擬機器頁面。

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，從虛擬機器清單中選擇**詳細目錄**索引標籤。 按一下虛擬機器的名稱。
3. 從單一虛擬機器 \] 頁面，您可以：
    - 檢視虛擬機器的詳細的資訊。
    - 檢視 CPU、 記憶體、 網路、 IOPS 及 IO 輸送量的即時和歷程記錄資料行圖表 （歷史資料只適用於執行 Windows Server 2019 的超融合式叢集）
    - 檢視、 建立、 套用、 重新命名和刪除檢查點。
    - 檢視虛擬機器的虛擬硬碟 (.vhd) 檔案、 網路介面卡和主機伺服器的詳細資料。
    - 刪除、 啟動、 關閉、 關機、 暫停、 繼續、 重設或重新命名虛擬機器。 也儲存虛擬機器、 刪除儲存的狀態，或建立檢查點。
    - [虛擬機器的變更設定](#change-virtual-machine-settings)。
    - 使用 VMConnect 透過 HYPER-V 主機在虛擬機器主控台連線。
    - [複寫虛擬機器使用 Azure Site Recovery](#protect-virtual-machines-with-azure-site-recovery)。

## 管理虛擬機器透過 HYPER-V 主機 (VMConnect)

![透過網頁瀏覽器連線的 VM](../media/manage-virtual-machines/vm-connect.png)

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**詳細目錄**] 索引標籤從清單中選擇在虛擬機器，然後按一下 [**更多** > **Connect**或**下載 RDP 檔案**。 **連線**可讓您透過遠端桌面 web 主控台中，對 Windows Admin Center 整合在客體 VM 與互動。 **下載 RDP 檔案**會下載.rdp 檔，您可以使用遠端桌面連線應用程式 (mstsc.exe) 開啟。 這兩個選項將會使用 VMConnect 連線至客體 VM 透過 HYPER-V 主機，並將會要求您輸入系統管理員認證的 HYPER-V 主機伺服器。

## 變更 HYPER-V 主機設定

![HYPER-V 主機設定 \] 畫面](../media/manage-virtual-machines/host-settings.png)

1. 在伺服器、 超融合式叢集或容錯移轉叢集連線時，按一下左側瀏覽窗格底部的 [**設定**] 功能表。
2. 在 HYPER-V 主機伺服器或叢集，您會看到下列各節的**HYPER-V 主機設定**群組：
    - 一般： 變更虛擬硬碟和虛擬機器的檔案路徑和 hypervisor 排程類型 （如果支援）
    - 加強的工作階段模式
    - NUMA 跨越
    - 即時移轉
    - 存放裝置移轉
3. 如果您進行任何變更設定中的超融合式叢集或容錯移轉叢集連線的 HYPER-V 主機，變更將會套用到所有叢集節點。

## 檢視 HYPER-V 事件記錄檔

您可以檢視 HYPER-V 事件記錄檔直接從虛擬機器工具。

1. 按一下左側瀏覽窗格中的**虛擬機器**工具。
2. 在虛擬機器工具頂端，選擇 [**摘要**] 索引標籤。在右上角事件區段中，按一下 [**檢視所有事件**。
3. 在左窗格中，事件檢視器工具將顯示 HYPER-V 事件通道。 選擇要在右窗格中檢視事件的通道。 如果您要管理容錯移轉叢集或超融合式叢集，事件記錄檔將會顯示所有叢集節點，在 [機器] 欄中顯示主機伺服器的事件。

## 保護使用 Azure Site Recovery 虛擬機器

您可以使用 Windows Admin Center 設定 Azure Site Recovery，並將您的內部部署虛擬機器複寫至 Azure。 [深入了解](../azure/azure-site-recovery.md)

## 未來的更多

Windows Admin Center 中的虛擬機器管理會主動開發和未來會新增的新功能。 您可以檢視 UserVoice 狀態和投票的功能：

|功能要求|
|-------|
|[匯入/匯出虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31481971--virtual-machines-import-export-vms)|
|[排序資料夾中的虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494712--virtual-machines-ability-to-sort-vm-into-folder)|
|[支援額外的虛擬機器設定](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31915264--virtual-machines-expose-all-configurable-setting)|
|[HYPER-V 複本支援](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/32040253--virtual-machines-setup-and-manage-hyper-v-replic)|
|[虛擬機器的擁有權委派](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31663837--virtual-machines-owner-delegation)|
|[複製虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31783288--virtual-machines-add-a-button-to-clone-a-vm)|
|[從現有的虛擬機器建立範本](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31494649--virtual-machines-create-a-template-from-an-exist)|
|[在 HYPER-V 主機上的檢視虛擬機器](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31734559--virtual-machines-find-vms-on-host-screen)|
|[在新的虛擬機器] 窗格中設定 VLAN](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31710979--virtual-machines-new-new-vm-pane-need-vlan-opt)|
|[**查看所有或建議的新功能**](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bvirtual%20machines%5D)|
