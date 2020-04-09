---
title: Azure Site Recovery Services 整合
description: 說明如何使用 Windows Server Essentials
ms.date: 10/01/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 29db78fdf38a6fab23d9a5ec5539c0606e2fbbaa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814481"
---
# <a name="azure-site-recovery-services-integration"></a>Azure Site Recovery Services 整合

>適用于： Windows Server 2016 Essentials

[Azure Site Recovery 服務](https://docs.microsoft.com/azure/site-recovery/)是 Microsoft Azure 提供的一項服務，可讓您將虛擬機器（VM）即時複寫到 Azure 中的備份保存庫。 如果您的伺服器或網站因硬體或其他失敗而關閉，您可以故障切換至 Azure，其中儲存在備份保存庫中的 VM 映射將會布建為 Azure 中執行中的 VM。 與 Azure 虛擬網路結合，在容錯移轉至 Azure 的情況下，先前連線到內部部署伺服器的用戶端電腦將會以透明的方式連接到在 Azure 中執行的伺服器。

將 Azure Site Recovery 服務與 Windows Server Essentials 整合，會以設定[Azure 虛擬網路](azure-virtual-network-integration.md)的相同方式啟動。 從 [儀表板] 的 [ **Microsoft Cloud 服務整合**] 頁面中，按一下 [儀表板] 右邊的 [**與 Azure Site Recovery 服務整合**]：

![螢幕擷取畫面，顯示 [Windows Server Essentials 儀表板] 首頁上的 [開始使用] 索引標籤。 在 [開始使用] 索引標籤上，已選取 [服務] 區段，而儀表板會在 [Microsoft Cloud 服務整合] 下方指出 Azure 復原目前已停用。](media/azure-site-recovery-1.PNG)

如同 Azure 虛擬網路整合和 Azure 備份整合，您必須使用現有的 Azure 帳戶登入 Azure，或建立新的帳戶：

![顯示 [啟用複寫至 Azure] 的 [登入 Microsoft Azure] 頁面的螢幕擷取畫面。 [登入] 按鈕會顯示，因為使用者尚未登入 Microsoft Azure。](media/azure-site-recovery-2.PNG)

成功登入 Azure 之後，您會看到一個畫面，詢問您想要與 Azure Site Recovery 服務相關聯的訂用帳戶，以及您的 VM 將儲存和裝載的 Azure 區域：

![顯示 [啟用複寫至 Azure] 的 [登入 Microsoft Azure] 頁面的螢幕擷取畫面。 因為使用者已登入 Microsoft Azure，此頁面會提供選項來選取訂用帳戶、儲存體帳戶和區域。](media/azure-site-recovery-3.PNG)

選取訂用帳戶和區域之後，名為**Azure**復原的**Windows Server Essentials 儀表板**中會出現新的索引標籤。 網路掃描是用來識別和列舉受支援的主機伺服器（執行 Windows Server Hyper-v 2012 R2 和更新版本），以及個別主機下的虛擬機器（來賓）：

![螢幕擷取畫面，其中顯示 [Windows Server Essentials 儀表板] 的 [Azure 復原] 頁面。 會顯示兩部 Hyper-v 主機，以及在這些主機上執行的虛擬機器。 主機 RAM 上名為 ramh157v01 的虛擬機器已選取，而目前已停用此虛擬機器的複寫至 Azure。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>啟用來賓虛擬機器以進行保護

選取 [Azure 復原] 視窗中的虛擬機器時，您可以按一下儀表板右側的 [**啟用複寫至 azure** ]，以準備並將虛擬機器 &trade;的映射複製到 azure：

![顯示 [啟用複寫至 Azure] 對話方塊的螢幕擷取畫面。 在新增主機時，會顯示進度列。](media/azure-site-recovery-5.PNG)

在此程式期間，會在主機伺服器上安裝 Azure Site Recovery 服務代理程式、建立將儲存來賓 VM 映射的備份保存庫，並開始將映射複寫到 Azure。 視複寫的 VM 大小而定，完成複寫程式可能需要數小時或數天。 在整個 VM 映射和最新的差異複寫至 Azure 之前，容錯回復工作無法使用，且 VM 不受保護。 複寫完成後，Azure 復原**視窗中的**[保護狀態] 欄將會從複寫變更為 [**已啟用**]：

![螢幕擷取畫面，其中顯示 [Windows Server Essentials 儀表板] 的 [Azure 復原] 頁面。 會顯示兩部 Hyper-v 主機，以及在這些主機上執行的虛擬機器。 主機 RAM 上名為 ramh12v02 的虛擬機器-H1-2 已選取，且目前已為此虛擬機器啟用複寫到 Azure。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>將來賓 VM 容錯移轉至 Azure

![顯示 [容錯移轉 VM 至 Azure] 頁面的螢幕擷取畫面。](media/azure-site-recovery-7.PNG)

當受保護的虛擬機器失敗，或受保護虛擬機器執行所在的主機伺服器失敗時，就可以完成容錯移轉至 Azure，以維持商務持續性，直到內部部署虛擬機器或主機伺服器已修復並可供使用為止。 如上圖所示，Azure Site Recovery 服務支援三種類型的容錯移轉：

-   **測試容錯移轉**ƒA 良好的嚴重損壞修復計畫包含模擬嚴重損壞的能力，以確保在發生嚴重嚴重損壞時的停機時間降至最低。 測試容錯移轉會使用已複寫至您的復原保存庫的 VM 映射，將其布建為 Azure 中執行中的虛擬機器，並可讓您連接到伺服器以測試適用于企業的案例。 在測試容錯移轉期間，本機虛擬機器會繼續執行，不會中斷，因為不會在嚴重損壞修復測試期間中斷商務。 測試容錯移轉完成之後，您會透過 Azure 入口網站停止測試，這會取消布建虛擬機器並刪除 VHD。 在整個測試容錯移轉期間，您的復原保存庫中的 VM 映射會繼續從站上 VM 複寫，就好像沒有發生任何情況。

-   未規劃的**容錯移轉**會在主機伺服器上執行的受保護主機伺服器或 VM 發生實際錯誤時，進行未計畫的ƒAn 容錯移轉。 從 Windows Server Essentials 儀表板手動觸發容錯移轉，或如果失敗的伺服器本身是執行 Windows Server Essentials 的伺服器，則可直接從 Azure 入口網站觸發。 在此情況下，未規劃的容錯移轉會使用已複寫至您的復原保存庫的 VM 映射、將它布建為 Azure 中執行中的虛擬機器，並可讓您連接到伺服器，以測試適用于企業的案例。 當您的伺服器在內部部署還原時，可以從 Azure 入口網站觸發手動容錯回復，然後將 VM 映射複製到本機伺服器。

-   規劃的**容錯移轉**ƒA 計畫的容錯移轉是一項動作，可在規劃的活動（例如，硬體維護）進行時採取，這會使伺服器關閉。 在規劃的容錯移轉時，在 Azure 中布建複寫的 VM 映射時，會發生相同的程式。 不過，在這麼做之前，本機伺服器會以有條理的方式關閉，以確保所有變更都會在關閉之前複寫到 Azure。 在規劃的維護完成之後，您可以從 Windows Server Essentials 儀表板或 Azure 入口網站手動觸發容錯回復，這會啟動本機伺服器，然後在 Azure 中取消布建 VM，並刪除。VHD 檔案。 從內部部署 VM 複寫至 Azure 之後，將會繼續正常運作。

在上述三種情況中，當 VM 故障切換至 Azure 時，[Windows Server Essentials 儀表板] 會在 Azure 中顯示新的 VM，如下圖所示。

![螢幕擷取畫面，其中顯示 [Windows Server Essentials 儀表板] 的 [Azure 復原] 頁面。 已針對名為 Essentials 的主機啟用複寫至 Azure，而在 Azure 中執行名為 Essentials-Test 的虛擬機器表示主機已故障切換至 Azure。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)
