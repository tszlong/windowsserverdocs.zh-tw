---
title: Azure Site Recovery Services 整合
description: 說明如何使用 Windows Server Essentials
ms.date: 10/01/2016
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: aa84ae8f3d11631b76d2e85e6a50f309eb83dd74
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622588"
---
# <a name="azure-site-recovery-services-integration"></a>Azure Site Recovery Services 整合

>適用于： Windows Server 2016 Essentials

[Azure Site Recovery 服務](/azure/site-recovery/) 是 Microsoft Azure 所提供的一項服務，可讓您將虛擬機器 (VM) 即時複寫到 Azure 中的備份保存庫。 如果您的伺服器或網站因為硬體或其他失敗而關閉，您可以容錯移轉至 Azure，而備份保存庫中儲存的 VM 映射將會布建為 Azure 中的執行中 VM。 相較于 Azure 虛擬網路，當容錯移轉至 Azure 時，先前連線至內部部署伺服器的用戶端電腦，將會以透明方式連線到在 Azure 中執行的伺服器。

將 Azure Site Recovery Services 與 Windows Server Essentials 整合的啟動方式，與設定 [Azure 虛擬網路](azure-virtual-network-integration.md)的方式相同。 從儀表板的 [ **Microsoft 雲端服務整合** ] 頁面中，按一下 [儀表板] 右邊的 [ **與 Azure Site Recovery 服務整合** ]：

![顯示 [Windows Server Essentials 儀表板] 首頁上 [開始] 索引標籤的螢幕擷取畫面。 在 [開始] 索引標籤上，已選取 [服務] 區段，而 [儀表板] 在 [Microsoft 雲端服務整合] 下指出 Azure 復原目前已停用。](media/azure-site-recovery-1.PNG)

與 Azure 虛擬網路整合和 Azure 備份整合一樣，您必須使用現有的 Azure 帳戶登入 Azure，或建立新的帳戶：

![螢幕擷取畫面，其中顯示 [啟用複寫到 Azure] wizard 的 [登入 Microsoft Azure] 頁面。 [登入] 按鈕會顯示，因為使用者尚未登入 Microsoft Azure。](media/azure-site-recovery-2.PNG)

成功登入 Azure 之後，您會看到一個畫面，詢問您想要與 Azure Site Recovery 服務相關聯的訂用帳戶，以及將儲存和裝載 VM 的 Azure 區域：

![螢幕擷取畫面，其中顯示 [啟用複寫到 Azure] wizard 的 [登入 Microsoft Azure] 頁面。 因為使用者已登入 Microsoft Azure，此頁面會提供選取訂用帳戶、儲存體帳戶和區域的選項。](media/azure-site-recovery-3.PNG)

選取訂用帳戶和區域之後，新的索引標籤會出現在 [ **Windows Server Essentials 儀表板** ] 中，稱為「 **Azure**復原」。 網路掃描是為了識別和列舉支援的主機伺服器 (執行 Windows Server Hyper-v 2012 R2 和更新版本的) ，以及在個別主機下 (來賓) 的虛擬機器：

![顯示 [Windows Server Essentials 儀表板] 之 [Azure 復原] 頁面的螢幕擷取畫面。 這會顯示兩部 Hyper-v 主機，以及在這些主機上執行的虛擬機器。 主機 RAM-H1-7 上名為 ramh157v01 的虛擬機器已選取，且此虛擬機器目前已停用複寫至 Azure。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>啟用來賓虛擬機器以進行保護

當您在 Azure 復原視窗中選取虛擬機器時，您可以按一下儀表板右側的 [ **啟用複寫至 azure** ]，以準備並將虛擬機器 &trade; 映射複製到 azure：

![顯示 [啟用複寫至 Azure] 對話方塊的螢幕擷取畫面。 當加入的主機時，會顯示進度列。](media/azure-site-recovery-5.PNG)

在此程式期間，系統會在主機伺服器上安裝 Azure Site Recovery 的服務代理程式、將會建立來賓 VM 映射的備份保存庫，並開始將映射複寫至 Azure。 視複寫的 VM 大小而定，完成複寫程式可能需要數小時或數天的時間。 在將整個 VM 映射和最新的差異複寫到 Azure 之前，無法使用容錯移轉工作且 VM 未受到保護。 複寫完成後，[Azure 復原] 視窗中的 [保護狀態] 欄將會 **從 [** 複寫] 變更為 [ **已啟用**]：

![顯示 [Windows Server Essentials 儀表板] 之 [Azure 復原] 頁面的螢幕擷取畫面。 這會顯示兩部 Hyper-v 主機，以及在這些主機上執行的虛擬機器。 主機 RAM-H1-2 上名為 ramh12v02 的虛擬機器已選取，且目前已為此虛擬機器啟用 Azure 的複寫。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>將來賓 VM 容錯移轉至 Azure

![顯示容錯移轉 VM 至 Azure 頁面的螢幕擷取畫面。](media/azure-site-recovery-7.PNG)

當受保護的虛擬機器失敗，或受保護虛擬機器執行所在的主機伺服器失敗時，您可以完成容錯移轉至 Azure，以維護商務持續性，直到內部部署虛擬機器或主機伺服器已修復並可供使用為止。 如上圖所示，Azure Site Recovery 服務可支援三種類型的容錯移轉：

-   **測試容錯移轉** ƒA 良好的嚴重損壞修復計畫併入了模擬嚴重損壞的功能，以確保在發生嚴重損毀時的停機時間降到最短。 測試容錯移轉會將已複寫到復原保存庫的 VM 映射，布建為 Azure 中的執行中虛擬機器，並可讓您連接到伺服器，以測試適用于企業的案例。 在測試容錯移轉期間，本機虛擬機器會繼續執行，而不會中斷，因為不會在損毀修復測試期間中斷企業。 測試容錯移轉完成後，您會透過 Azure 入口網站停止測試，這會取消布建虛擬機器並刪除 VHD。 在整個測試容錯移轉期間，復原保存庫中的 VM 映射會繼續從站上 VM 進行複寫，就好像沒有發生任何事一樣。

-   未**計畫的容錯移轉**ƒAn 發生未規劃的容錯移轉時，會在主機伺服器上執行的受保護主機伺服器或 VM 發生實際錯誤時進行。 您可以從 Windows Server Essentials 儀表板手動觸發容錯移轉，或者如果失敗的伺服器本身是 Windows Server Essentials 執行所在的伺服器，則可以直接從 Azure 入口網站觸發。 在此情況下，未規劃的容錯移轉會採用已複寫至復原保存庫的 VM 映射、將它布建為 Azure 中的執行中虛擬機器，並可讓您連線到伺服器，以測試適用于企業的案例。 當您的伺服器在內部部署環境中還原時，可以從 Azure 入口網站觸發手動容錯回復，然後將 VM 映射複製回本機伺服器。

-   **規劃的故障** 轉移ƒA 計畫的容錯移轉是一項動作，可在規劃的活動（例如，硬體維護）必須進行時，讓伺服器停機。 在規劃的容錯移轉時，會在 Azure 中布建已複寫 VM 映射的過程中進行相同的程式。 不過，在這麼做之前，本機伺服器會以有條理的方式關閉，以確保在關機之前將所有變更複寫至 Azure。 完成規劃的維護之後，您可以從 Windows Server Essentials 儀表板或 Azure 入口網站（會啟動本機伺服器，然後在 Azure 中取消布建 VM）中手動觸發容錯回復，並刪除。VHD 檔案。 從內部部署 VM 至 Azure 的複寫會繼續正常執行。

在上述三種案例中的任一種情況下，當 VM 容錯移轉至 Azure 時，Windows Server Essentials 儀表板會顯示 Azure 中執行的新 VM，如下圖所示。

![顯示 [Windows Server Essentials 儀表板] 之 [Azure 復原] 頁面的螢幕擷取畫面。 已針對名為 Essentials 的主機啟用複寫至 Azure，而在 Azure 中執行名為 Essentials-Test 的虛擬機器指出主機已容錯移轉至 Azure。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)