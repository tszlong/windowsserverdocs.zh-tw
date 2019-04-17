---
title: "Azure 網站修復服務整合"
description: "告訴您如何使用 Windows Server Essentials"
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
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Azure 網站修復服務整合

>適用於： Windows Server 2016 Essentials

[Azure 網站修復服務](https://azure.microsoft.com/en-us/documentation/services/site-recovery/)保存在 Azure 備份庫是讓您虛擬的電腦 (VM) 的即時複寫 Microsoft Azure 所提供的服務。 您的伺服器或網站因為硬體或其他失敗已關閉，您可以容錯移轉至 Azure 位置將會提供 VM 映像儲存在您的備份保存庫中執行 VM 中 Azure 為。 加上發生錯誤後移轉至 Azure，Azure Virtual 網路 client 之前先伺服器連接的電腦將會無障礙連接到執行 Azure 伺服器。

與 Windows Server Essentials 整合 Azure 網站修復服務開始在相同的方式來設定[Azure Virtual 網路](azure-virtual-network-integration.md)。 從**Microsoft 雲端服務整合**頁面儀表板上，按一下 [**整合 Azure 網站修復服務的**右邊的儀表板：

![在 Windows Server Essentials 儀表板中的 [首頁] 頁面顯示 [開始] 索引標籤螢幕擷取畫面。 在 [開始] 索引標籤，選取 [服務] 區段，並儀表板表示底下 Microsoft 雲端服務整合 Azure 修復目前已停用。](media/azure-site-recovery-1.PNG)

使用 Azure Virtual 網路整合和 Azure 備份整合，您必須使用您現有 Azure 帳號，登入 Azure 或建立新的帳號：

![顯示登入至 Microsoft Azure 頁面來 Azure 讓複寫精靈的螢幕擷取畫面。 由於使用者有尚未不登入 Microsoft Azure，會顯示 [登入] 按鈕。](media/azure-site-recovery-2.PNG)

成功登入 Azure 之後, 您會看到的畫面會詢問您想要與 Azure 網站修復服務，以及 Azure 地區儲存和裝載您 VM 的裝機費：

![顯示登入至 Microsoft Azure 頁面來 Azure 讓複寫精靈的螢幕擷取畫面。 使用者已登入 Microsoft Azure，因為此頁面提供裝機費、 儲存帳號，並地區選取的選項。](media/azure-site-recovery-3.PNG)

之後裝機費，地區選擇新的索引標籤將會出現在**Windows Server Essentials 儀表板**名為**Azure 修復**。 網路掃描為了找出並列舉伺服器支援的主機 (執行 Windows Server HYPER-V 2012 R2 和上述) 以及個人主機中虛擬電腦 （來賓）：

![顯示網頁 Azure 修復的 Windows Server Essentials 儀表板螢幕擷取畫面。 有兩個 HYPER-V 主機會顯示這些主機上執行虛擬電腦。 命名 ramh157v01 RAM-H1 7 已選取，主機上的一樣與複寫 Azure 目前停用這個一樣。](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>讓來賓虛擬機器的保護

選取一樣 Azure 修復視窗中，您可以按一下**讓複寫 Azure**右側的 [準備和複製一樣儀表板上™ Azure s 映像：

![顯示要 Azure 讓複寫對話方塊螢幕擷取畫面。 新增的主機會顯示進度列。](media/azure-site-recovery-5.PNG)

在此過程中，Azure 網站修復客服已安裝於主機伺服器，備份保存庫位置建立的來賓 VM 將會儲存的影像，並複寫的影像，Azure 來開始。 根據複寫 VM 的大小，小時或幾天，可能需要複寫程序完成。 整個 VM 影像和 delta 最新的複製到 Azure，直到容錯移轉工作不會使用和 VM 未受保護。 保護狀態] 欄 Azure 修復視窗中的複寫在完成後，將會從變更**Replicating**以**啟用**:

![顯示網頁 Azure 修復的 Windows Server Essentials 儀表板螢幕擷取畫面。 有兩個 HYPER-V 主機會顯示這些主機上執行虛擬電腦。 目前支援此一樣的名 ramh12v02 RAM-H1-2 已選取，主機上的一樣，以及 Azure 複寫。](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>容錯來賓以 Azure VM 的移轉

![顯示容錯移轉 VM Azure 頁面的螢幕擷取畫面。](media/azure-site-recovery-7.PNG)

一樣的受保護的失敗或維持業務持續性，直到進行受保護的一樣上執行的作業會失敗，無法移轉至 Azure 主機伺服器上場所的電腦或主機 isp 時修復和可用。 顯示上述圖中為有三種類型的容錯移轉支援 Azure 網站修復服務：

-   **測試容錯移轉**ƒA 告訴您一個好復原計劃結合模擬嚴重損壞，以確保最低中斷發生嚴重損壞真正的能力。 測試容錯 VM 影像已經已複製到您的修復保存庫 provisions 它為 Azure 中執行一樣，可讓您連接至測試案例適用於企業的伺服器。 期間測試故障，本機一樣持續執行至於不能商務干擾損壞復原測試期間不被迫中斷作業。 測試容錯移轉完成之後，您將會停止透過 Azure 入口網站，取消 provisions 一樣，刪除 VHD 測試。 在整個測試容錯移轉您復原保存庫 VM 影像持續複寫從現場 VM 如同以往發生執行任何動作。

-   **意外的錯誤後移轉**ƒAn 實際錯誤受保護的主機伺服器或 VM 主機伺服器上執行時，就會發生意外的錯誤後移轉。 錯誤後的移轉手動觸發的是 Windows Server Essentials 儀表板，或是失敗的伺服器本身是否執行 Windows Server Essentials 的伺服器可以會觸發從 Azure 入口網站直接。 若是如此，計畫容錯 VM 映像，已經已複製到您的修復保存庫、 provisions 它為執行一樣在 Azure，可讓您連接至測試案例適用於企業的伺服器。 您的伺服器上場所還原時，可以手動回復觸發從 Azure 入口網站，然後將複製回復到本機伺服器 VM 映像。

-   **規劃容錯移轉**計劃 ƒA 容錯移轉是可以取得該計畫的活動，例如 HW 維護，必須才會生效下耗用伺服器的動作。 萬一計劃故障，相同的程序發生有關您複寫 VM 影像，Azure 中提供。 但是之前這樣做，本機伺服器會關閉以重新以確保所有變更都複寫 Azure 到之前關機。 計畫的維護在完成後，您可以手動觸發從 Windows Server Essentials 儀表板或 Azure 入口網站時帶上本機伺服器，取消提供 VM 中 Azure delete 回復。VHD 檔案。 從場所上 VM 複寫 Azure 會再繼續進行到再試一次正常。

在任何上方的三個案例中，當 VM 移轉到 Windows Server Essentials 儀表板，Azure 將會新 VM 中顯示 Azure 執行如圖所示。

![顯示網頁 Azure 修復的 Windows Server Essentials 儀表板螢幕擷取畫面。 已複寫 Azure 支援主機名基本資訊，以及 Azure 執行命名的 Essentials 測試指出主機已無法透過 Azure 一樣。](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>也了
--------
[開始使用 Windows Server Essentials](get-started.md)
