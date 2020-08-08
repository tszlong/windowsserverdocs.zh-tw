---
title: 使用 Azure Site Recovery 和 Windows 系統管理中心保護您的 Hyper-v 虛擬機器
description: 與 Azure Site Recovery 搭配使用 Windows Admin Center (Project Honolulu)，以保護 Hyper-V VM。
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 651eff1819a7ec867febd86005415e5044e6bbd0
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996823"
---
# <a name="protect-your-hyper-v-virtual-machines-with-azure-site-recovery-and-windows-admin-center"></a>使用 Azure Site Recovery 和 Windows 系統管理中心保護您的 Hyper-v 虛擬機器

>適用於：Windows Admin Center 預覽版、Windows Admin Center

[深入瞭解 Azure 與 Windows 管理中心的整合。](./index.md)

Windows Admin Center 簡化在您的 Hyper-V 伺服器或叢集上複寫虛擬機器的程序，讓您更輕鬆地從自己的資料中心運用 Azure 的強大功能。 若要將安裝工作自動化，您可以將 Windows Admin Center 閘道連線至 Azure。

使用下列資訊設定複寫設定，並從 Azure 入口網站中建立復原計劃，讓 Windows Admin Center 可以開始 VM 複寫並保護您的 VM。

## <a name="what-is-azure-site-recovery-and-how-does-it-work-with-windows-admin-center"></a>什麼是 Azure Site Recovery，以及它如何與 Windows Admin Center 搭配使用？

**Azure Site Recovery** 是一個 Azure 服務，可複寫 VM 上執行的工作負載，這樣您的業務關鍵基礎結構就會在發生嚴重損壞時受到保護。  [深入瞭解 Azure Site Recovery](/azure/site-recovery/site-recovery-overview)。

Azure Site Recovery 由兩個元件組成：「複寫」  和「容錯移轉」  。 複寫部分將目標 VM 的 VHD 複寫至 Azure 儲存體帳戶，以備於發生嚴重損壞時保護您的 VM。 您可以接著容錯移轉這些 VM，並在發生嚴重損壞時使用 Azure 執行它們。 您也可以在不影響主要 VM 的情況下執行測試容錯移轉，以測試 Azure 中的復原程序。

單靠完成複寫元件的安裝即足以在發生嚴重損壞時保護您的 VM。 不過，在設定容錯移轉部分以前，您無法在 Azure 中啟動 VM。 當您想要容錯移轉至 Azure VM 時，您可以設定容錯移轉部分 (初始安裝過程中不需要這項作業)。 如果主機伺服器關閉，而您尚未設定容錯移轉元件時，您可以在那時設定此元件，並存取受保護 VM 的工作負載。 不過，在發生嚴重損壞之前容錯移轉相關設定，才是正常做法。


## <a name="prerequisites-and-planning"></a>必要條件和規劃

- 裝載所要保護之 VM 的目標伺服器必須可以存取網際網路，才能複寫至 Azure。
- [將 Windows Admin Center 閘道連線至 Azure](azure-integration.md)。
- 請[參閱容量規劃工具，以評估成功複寫和容錯移轉的需求](/azure/site-recovery/hyper-v-site-walkthrough-capacity)。

## <a name="step-1-set-up-vm-protection-on-your-target-host"></a>步驟 1:在目標主機上設定 VM 保護

> [!NOTE]
> 您必須對每個包含所要保護之目標 VM 的主機伺服器或叢集執行此步驟一次。

1. 瀏覽至裝載所要保護 (使用伺服器管理員或超融合式叢集管理員來保護) 之 VM 的伺服器或叢集。
2. 移至**虛擬機器**  >  **清查**。
3. 選取任何 VM (這不一定要是您想保護的 VM)。
4. 選取 [**更多**] [  >  **設定 VM 保護**]。
5. 登入 Azure 帳戶。
6. 輸入必要資訊：

   - **訂閱：** 您要在此主機上用於複寫 VM 的 Azure 訂用帳戶。
   - **位置：** 應在其中建立 ASR 資源的 Azure 區域。
   - **儲存體帳戶：** 儲存此主機上已複寫 VM 工作負載所在的儲存體帳戶。
   - **保存庫：** 選擇此主機上受保護 VM 之 Azure Site Recovery 保存庫的名稱。

7. 選取 **\[安裝 ASR\]**。
8. 等候到下列通知出現：**Site Recovery 設定完成**。

這項作業可能需要 10 分鐘的時間。 您可以移至**\[通知\]** (右上方鈴鐺圖示) 觀看進度。

>[!NOTE]
> 此步驟會自動將 ASR 代理程式安裝到目標伺服器或節點 (若在叢集上進行設定)，使用指定的 **\[儲存體帳戶\]** 及 **\[保存庫\]** 在指定的 **\[位置\]** 中建立 **\[資源群組\]**。 這也會向 ASR 服務註冊目標主機，並設定預設複寫原則。

## <a name="step-2-select-virtual-machines-to-protect"></a>步驟 2：選取要保護的虛擬機器

1. 瀏覽回到您在上述步驟 2 中設定的伺服器或叢集，然後移至 **\[虛擬機器\] > \[清查\]**。
2. 選取您想要保護的 VM。
3. 選取 [**更多**  >  **保護 VM**]。
4. 檢閱[保護 VM 的容量需求](/azure/site-recovery/site-recovery-capacity-planner)。

    如果您想要使用 premium 儲存體帳戶，請[在 Azure 入口網站中建立一個](/azure/storage/common/storage-premium-storage)。 Windows Admin Center 窗格中提供的 **\[新建\]** 選項會建立標準儲存體帳戶。

5. 輸入要用於此 VM 複寫的 **\[儲存體帳戶\]** 的名稱，然後選取 **\[保護 VM\]**。 此步驟會啟用所選虛擬機器的複寫。

6. ASR 將會開始複寫。 當 **\[虛擬機器清查\]** 方格中 **\[受保護\]** 欄的值變更為 **\[是\]** 時，複寫已完成且 VM 已受保護。 這可能需要數分鐘的時間。

## <a name="step-3-configure-and-run-a-test-failover-in-the-azure-portal"></a>步驟 3：在 Azure 入口網站中設定和執行測試容錯移轉

 雖然開始 VM 複寫 (VM 只是使用複寫即已受保護) 時您不需要完成此步驟，但還是建議您在設定 Azure Site Recovery 時設定容錯移轉時設定。 如果您想要準備 Azure VM 的容錯移轉，請完成下列步驟：

1. [設定 Azure 網路](/azure/site-recovery/hyper-v-site-walkthrough-prepare-azure)容錯移轉 VM 將會附加至此 VNET。 請注意，連結頁面列出的其他步驟已由 Windows Admin Center 自動完成；您只需要設定 Azure 網路。

2. [執行測試容錯移轉](/azure/site-recovery/hyper-v-site-walkthrough-test-failover)。

## <a name="step-4-create-recovery-plans"></a>步驟 4：建立復原計劃

**復原計劃**是 Azure Site Recovery 中的一項功能，可讓您容錯移轉和復原整個組成 VM 集合的應用程式。 雖然可以個別個別復原受保護的 VM，但是將組成應用程式的 VM 新增至復原計劃，您就可以透過復原計劃容錯移轉整個應用程式。 您也可以使用復原計劃的測試容錯移轉功能來測試應用程式的復原。 復原計劃可讓您將 VM 組成群組、將它們依其應在容錯移轉期間提出的順序排列，以及將其他要在復原過程中執行的步驟自動化。 VM 受到保護後，您即可移至 Azure 入口網站中的 Azure Site Recovery 保存庫，並建立這些 VM 的復原計劃。 [深入瞭解復原方案](/azure/site-recovery/site-recovery-create-recovery-plans)。

## <a name="monitoring-replicated-vms-in-azure"></a>在 Azure 中監視複寫的 VM ##

若要確認伺服器註冊中沒有失敗，請移至**Azure 入口網站**  >  **所有資源**復原  >  **服務保存庫**， (您在步驟 2) > 作業] **Jobs**  >  **Site Recovery 作業**中指定的所有資源。

您可以前往 [復原**服務保存庫**] [複寫的專案] 來監視 VM 複寫  >  ** **。

若要查看已向保存庫註冊的所有伺服器，請移至 [復原**服務保存庫**]，  >  **Site Recovery**  >  [hyper-v 網站]) 區段下的 [基礎結構] [**hyper-v 主機**] (。

## <a name="known-issue"></a>已知問題 ##

向叢集註冊 ASR 時，如果節點無法安裝 ASR 或註冊到 ASR 服務，則您的 VM 可能並未受到保護。 藉由前往復原**服務保存庫**  >  **作業**  >  **Site Recovery 作業**，確認叢集中的所有節點都已註冊到 Azure 入口網站中。