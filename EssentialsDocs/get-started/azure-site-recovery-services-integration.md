---
title: Azure Site Recovery Services 整合
description: 描述如何使用 Windows Server Essentials
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e9dabc1b986fd86b5ee3efc6022023b331046250
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433956"
---
# <a name="azure-site-recovery-services-integration"></a>Azure Site Recovery Services 整合

>適用於：Windows Server 2016 Essentials

[Azure Site Recovery 服務](https://docs.microsoft.com/azure/site-recovery/)是備份保存庫在 Azure 中啟用虛擬機器 (VM) 的即時複寫的 Microsoft Azure 所提供的服務。 您的伺服器或站台因為硬體或其他失敗而當機，您可以容錯移轉至 Azure，其中儲存您的備份保存庫中的 VM 映像將會佈建為在 Azure 中執行的 VM。 結合 Azure 虛擬網路，如果發生容錯移轉至 Azure，用戶端先前連線到內部部署伺服器的電腦會無障礙地連接到 Azure 中執行的伺服器。

整合 Windows Server Essentials 中的 Azure Site Recovery 服務中設定相同的方式來啟動[Azure 虛擬網路](azure-virtual-network-integration.md)。 從**Microsoft 雲端服務整合**儀表板頁面上，按一下**與 Azure Site Recovery 服務進行整合**右邊的儀表板：

![螢幕擷取畫面顯示在 Windows Server Essentials 儀表板首頁上的 [開始] 索引標籤中。 在 [開始] 索引標籤中，已選取 [服務] 區段，並儀表板的指示是在 Microsoft 雲端服務整合而 Azure Recovery 目前已停用。](media/azure-site-recovery-1.PNG)

使用 Azure 虛擬網路整合以及 Azure 備份整合，您必須使用您現有的 Azure 帳戶，登入 Azure，或建立新的帳戶：

![螢幕擷取畫面顯示 [啟用複寫至 Azure 精靈] 的 [登入 Microsoft Azure] 頁面中。 因為使用者有尚未簽署到 Microsoft Azure，則會顯示 [登入] 按鈕。](media/azure-site-recovery-2.PNG)

之後成功登入 Azure，您會看到的畫面，詢問您想要與 Azure Site Recovery 服務，以及儲存和裝載您的 VM 位於 Azure 區域產生關聯的訂用帳戶：

![螢幕擷取畫面顯示 [啟用複寫至 Azure 精靈] 的 [登入 Microsoft Azure] 頁面中。 因為使用者已登入 Microsoft Azure，則此頁面會提供選項，選取訂用帳戶、 儲存體帳戶和區域。](media/azure-site-recovery-3.PNG)

訂用帳戶和地區選取項目之後, 的新索引標籤會出現在**Windows Server Essentials 儀表板**稱為**Azure Recovery**。 網路掃描為了找出並列舉支援的主機伺服器 (執行 Windows Server HYPER-V 2012 R2 和更新版本) 以及虛擬機器 （客體） 在個別的主機：

![螢幕擷取畫面顯示 Azure 復原頁面的 Windows Server Essentials 儀表板中。 連同這些主機上執行的虛擬機器一起顯示兩個 HYPER-V 主機。 此虛擬機器目前停用名為 ramh157v01 選取 RAM-h 1-7，則的主機上的虛擬機器並複寫至 Azure。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>啟用客體虛擬機器進行保護

在選取的虛擬機器出現在 Azure 復原 視窗中的項目，您可以按一下**啟用複寫至 Azure**準備並複製虛擬機器儀表板右側™ s 映像至 Azure:

![螢幕擷取畫面顯示 [啟用複寫至 Azure] 對話方塊中。 如要新增主機，則會顯示進度列。](media/azure-site-recovery-5.PNG)

在此過程中，Azure Site Recovery 服務代理程式安裝在主機伺服器上，備份保存庫，客體會將儲存 VM 的映像已建立，並會開始複寫至 Azure 的映像。 正在複寫的 VM 大小，根據小時或幾天，可能需要完成複寫程序。 直到整個 VM 映像和最新的差異複寫至 Azure 時，容錯移轉的工作不會使用和未受保護 VM。 完成複寫之後，將會變更 Azure 的復原視窗中的 [保護狀態] 欄**複寫**要**已啟用**:

![螢幕擷取畫面顯示 Azure 復原頁面的 Windows Server Essentials 儀表板中。 連同這些主機上執行的虛擬機器一起顯示兩個 HYPER-V 主機。 名為 ramh12v02 選取 RAM-h 1-2，主機上的虛擬機器並複寫至 Azure 目前已啟用此虛擬機器。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>容錯移轉至 Azure VM 客體

![顯示 VM 容錯移轉至 Azure 頁面的螢幕擷取畫面。](media/azure-site-recovery-7.PNG)

受保護的失敗，或主機伺服器，以維護業務持續性，直到可以受保護的虛擬機器上執行的作業失敗，容錯移轉至 Azure 虛擬機器內部部署虛擬機器或主機伺服器時，修復且可用。 圖中顯示，有三種類型的容錯移轉支援與 Azure Site Recovery 服務：

-   **測試容錯移轉**ƒA 理想的災害復原計畫包含模擬真實的災害發生時確保最少停機時間的嚴重損壞的能力。 測試容錯移轉會採用已複寫到您的復原保存庫，會將它當作 Azure 中執行的虛擬機器佈建並可讓您連接到伺服器，以測試適用於企業的案例的 VM 映像。 測試容錯移轉期間本機虛擬機器會繼續使用不中斷，並不影響業務的災害復原測試期間執行。 測試容錯移轉完成之後，您將會停止測試透過 Azure 入口網站中，以取消佈建虛擬機器，並刪除 VHD。 在整個測試容錯移轉期間復原保存庫中的 VM 映像會繼續如同好像曾經發生過，從站台上的 VM 會複寫。

-   **非計劃性容錯移轉**ƒAn 與受保護的主機伺服器或主機伺服器上執行的 VM 的實際錯誤時，就會發生非計劃性容錯移轉。 容錯移轉可能是 Windows Server Essentials 儀表板中，從手動觸發，或在失敗的伺服器本身，執行 Windows Server Essentials 的伺服器是否可觸發從 Azure 入口網站直接。 在此情況下，非計劃性容錯移轉會採用已複寫到您的復原保存庫，為執行中虛擬機器在 Azure 中，為它佈建並可讓您連接到伺服器，以測試適用於企業的案例的 VM 映像。 還原您的伺服器時，在內部部署環境，可以從 Azure 入口網站，然後將複製回至本機伺服器的 VM 映像觸發手動容錯回復。

-   **規劃的容錯移轉**ƒA 規劃的容錯移轉是，計劃的活動，例如硬體維護，必須在進行可能需要向下的伺服器可採取的動作。 在計劃的容錯移轉時相同的程序進行關於您複寫的 VM 映像，在 Azure 中佈建。 不過，在之前這樣做，本機伺服器已關閉以確保在關機之前將所有變更都複寫到 Azure 以進行有條理的方式。 規劃的維護完成後，您可以手動觸發從 Windows Server Essentials 儀表板或 Azure 入口網站中，這會將本機伺服器，然後取消佈建在 Azure 和刪除 VM 容錯回復。VHD 檔案。 從內部部署 VM 複寫至 Azure 然後會再次發生如往常繼續。

在任何上述三種情況下，當 VM 會容錯移轉至 Azure，Windows Server Essentials 儀表板會顯示新的 VM 在 Azure 中執行以下圖所示。

![螢幕擷取畫面顯示 Azure 復原頁面的 Windows Server Essentials 儀表板中。 名為 Essentials，並在 Azure 中執行具名的 Essentials 測試表示主機已容錯移轉至 Azure 虛擬機器的主機已啟用複寫至 Azure。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>另請參閱
--------
[開始使用 Windows Server Essentials](get-started.md)
