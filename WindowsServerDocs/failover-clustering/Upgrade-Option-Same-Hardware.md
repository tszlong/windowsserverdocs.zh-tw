---
title: 使用相同硬體升級容錯移轉叢集
description: 本文說明如何使用相同硬體升級2節點的容錯移轉叢集
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 02/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 1c45fe1ea576946e499013c3dccb04cfef90c9a2
ms.sourcegitcommit: d1815253b47e776fb96a3e91556fd231bef8ee6d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/29/2021
ms.locfileid: "99042514"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>升級相同硬體上的容錯移轉叢集

> 適用於：Windows Server 2019、Windows Server 2016

容錯移轉叢集是由獨立電腦組成的群組，它們共同運作以提升服務和應用程式的可用性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一個叢集節點失敗，另一個節點即會開始提供服務 (稱為容錯移轉的處理程序)。 這樣可將使用者感受到服務中斷的時間降到最低。

本指南說明使用相同硬體從舊版將叢集節點升級至 Windows Server 2019 或 Windows Server 2016 的步驟。

## <a name="overview"></a>概觀

只有在從 Windows Server 2016 升級至 Windows 2019 時，才支援在現有的容錯移轉叢集上升級作業系統。  如果容錯移轉叢集執行的是舊版（如 Windows Server 2012 R2 和更早版本），則在執行叢集服務時升級時，將不會允許將節點聯結在一起。  如果使用相同的硬體，則可以採取步驟，將其移至較新的版本。

在您的容錯移轉叢集升級之前，請參閱 [Windows Server 升級內容](../upgrade/upgrade-overview.md)。  當您就地升級 Windows Server 時，您會從現有的作業系統版本移至較新的版本，同時維持在相同的硬體上。 您可以就地升級 Windows Server，但有時可以升級兩個版本。 例如，Windows Server 2012 R2 和 Windows Server 2016 可就地升級至 Windows Server 2019。  也請記住，您可以使用叢集 [遷移嚮導](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) ，但最多隻支援兩個版本。 下圖顯示 Windows Server 的升級路徑。 向下箭號代表從舊版升級至 Windows Server 2019 所支援的升級路徑。

![就地升級圖表](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

下列步驟是使用相同硬體從 Windows Server 2012 容錯移轉叢集伺服器到 Windows Server 2019 的範例。

開始任何升級之前，請確定目前的備份（包括系統狀態）已完成。  此外，請確定所有驅動程式和固件都已更新為您將使用之作業系統的認證層級。  這兩個筆記將不在此討論。

在下列範例中，容錯移轉叢集的名稱為 CLUSTER，而節點名稱為節點1和節點2。

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>步驟1：收回第一個節點並升級至 Windows Server 2016

1. 在容錯移轉叢集管理員中，以滑鼠右鍵按一下節點，然後選取 [ **暫停** 並 **清空角色**]，將所有資源從節點1清空至節點2。  或者，您可以使用 PowerShell 命令 [暫止-get-clusternode](/powershell/module/failoverclusters/suspend-clusternode)。

    ![顯示 [暫停 > 清空角色] 選項之容錯移轉叢集管理員的螢幕擷取畫面。](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. 以滑鼠右鍵按一下節點，然後選取 [ **其他動作** ] 和 [ **收回**]，從叢集收回節點1。  或者，您可以使用 PowerShell 命令 [移除-get-clusternode](/powershell/module/failoverclusters/remove-clusternode)。

    ![容錯移轉叢集管理員的螢幕擷取畫面，其中顯示 > 收回] 選項的其他動作。](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. 基於預防措施，請從您所使用的儲存體卸離節點1。  在某些情況下，中斷電腦的儲存纜線將已足夠。  如有需要，請洽詢您的存放裝置廠商以取得適當的表示中斷連結步驟。  這可能不是必要的，視您的儲存空間而定。

4. 將節點1重建為 Windows Server 2016。  確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

5. 使用節點1建立稱為 CLUSTER1 的新叢集。  開啟容錯移轉叢集管理員，然後在 [ **管理** ] 窗格中，選擇 [ **建立** 叢集]，然後依照嚮導中的指示進行。

    ![容錯移轉叢集管理員的 [管理] 窗格螢幕擷取畫面，其中顯示已呼叫的 [建立叢集] 選項。](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. 一旦建立叢集之後，就必須將角色從原始叢集遷移到這個新的叢集。  在新叢集上，以滑鼠右鍵按一下叢集名稱 (CLUSTER1) 然後選取 [ **其他動作** ] 和 [ **複製叢集角色**]。  遵循嚮導中的步驟來遷移角色。

    ![容錯移轉叢集管理員的螢幕擷取畫面，其中顯示 [複製叢集] > [複製] 選項的其他動作。](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  當所有資源都遷移之後，請關閉節點 2 (原始叢集) 並中斷存放裝置的連線，如此就不會造成任何干擾。  將存放裝置連接到節點1。  一旦連線之後，請將所有資源上線，並確保其正常運作。

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>步驟2：將第二個節點重建為 Windows Server 2019

一旦您確認一切都正常運作，即可將節點2重建到 Windows Server 2019 並加入叢集。

1. 在節點2上執行 Windows Server 2019 的全新安裝。 確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

2. 現在，原始叢集 (叢集) 已消失，您可以將新的叢集名稱保留為 CLUSTER1，或返回原始名稱。  如果您想要回到原始名稱，請遵循下列步驟：

   a. 在節點1上，以滑鼠右鍵按一下容錯移轉叢集管理員 (CLUSTER1) 的名稱，然後選擇 [ **屬性**]。

   b. 在 [ **一般** ] 索引標籤上，將叢集重新命名為 cluster。

   c. 選擇 [確定] 或 [套用] 時，您會看到下列對話方塊快顯視窗。

    ![[請確認動作] 對話方塊的螢幕擷取畫面。](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. 叢集服務將會停止，且需要重新開機才能完成重新命名。

3. 在節點1上，開啟容錯移轉叢集管理員。  以滑鼠右鍵按一下 **節點** ，然後選取 [ **新增節點**]。  請流覽嚮導，將節點節點新增至叢集。

4. 將儲存體附加至節點2。 這可能包括重新連接存放裝置纜線。

5. 以滑鼠右鍵按一下節點，然後選取 [ **暫停** 並 **清空角色**]，以將所有資源從節點1清空至節點2。  或者，您可以使用 PowerShell 命令 [暫止-get-clusternode](/powershell/module/failoverclusters/suspend-clusternode)。  確定所有資源皆已上線，且正常運作。

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>步驟3：將第一個節點重建為 Windows Server 2019

1. 從叢集中收回節點1，並以您先前的方式中斷儲存體與節點的連線。

2. 將節點1重建或升級至 Windows Server 2019。  確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

3. 重新連接儲存體，並將節點1加入至叢集。

4. 將所有資源移至節點1，並確定它們上線並視需要運作。

5. 目前的叢集功能等級維持在 Windows 2016。  使用 PowerShell 命令 [更新-CLUSTERFUNCTIONALLEVEL](/powershell/module/failoverclusters/update-clusterfunctionallevel)，將功能等級更新為 Windows 2019。

您現在正在執行功能完整的 Windows Server 2019 容錯移轉叢集。

## <a name="additional-notes"></a>其他注意事項

- 如先前所述，可能不需要中斷連接存放裝置。  在我們的檔中，我們想要小心錯誤。  請洽詢您的存放裝置廠商。
- 如果您的起始點是 Windows Server 2008 或 2008 R2 叢集，則可能需要額外的執行步驟。
- 如果叢集正在執行虛擬機器，請務必在使用 PowerShell 命令 [更新-VMVERSION](/powershell/module/hyper-v/update-vmversion)完成叢集功能等級之後，升級虛擬機器層級。
- 請注意，如果您執行的應用程式（例如 SQL Server、Exchange Server 等），將不會使用 [複製叢集角色] 嚮導來遷移應用程式。  您應諮詢您的應用程式廠商，以取得應用程式的適當遷移步驟。