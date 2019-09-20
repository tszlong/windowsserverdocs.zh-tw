---
title: 使用相同的硬體升級容錯移轉叢集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本文說明如何使用相同的硬體來升級2個節點的容錯移轉叢集
ms.localizationpriority: medium
ms.openlocfilehash: aa9a31b1faa48a4eaf2a17bc8ecda690b4cf1f12
ms.sourcegitcommit: ccec91c1d32a978159f9b8bb5e39ead5805c26c4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71143789"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>升級相同硬體上的容錯移轉叢集

> 適用於：Windows Server 2019、Windows Server 2016

容錯移轉叢集是一組獨立電腦，共同運作以提升應用程式和服務的可用性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一個叢集節點失敗，另一個節點即會開始提供服務 (稱為容錯移轉的處理程序)。 使用者在服務中遇到最少的中斷。

本指南說明從使用相同硬體的舊版將叢集節點升級至 Windows Server 2019 或 Windows Server 2016 的步驟。

## <a name="overview"></a>總覽

只有在從 Windows Server 2016 到 Windows 2019 時，才支援在現有容錯移轉叢集上升級作業系統。  如果容錯移轉叢集執行的是舊版（例如 Windows Server 2012 R2 和更早版本），則在執行叢集服務時升級時，將不允許將節點聯結在一起。  如果使用相同的硬體，則可以採取步驟，將它取得至較新的版本。  

在容錯移轉叢集的任何升級之前，請參閱[Windows Server 升級內容](../upgrade/upgrade-overview.md)。  當您就地升級 Windows 伺服器時，您可以從現有作業系統版本移至較新的版本，同時維持在相同的硬體上。 Windows Server 可就地升級至少一部，有時也可以轉寄兩個版本。 例如，Windows Server 2012 R2 和 Windows Server 2016 可以就地升級至 Windows Server 2019。  也請記住，您可以使用 [叢集[遷移嚮導]](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) ，但最多隻支援兩個版本。 下圖顯示 Windows Server 的升級路徑。 向下箭號代表從舊版移至 Windows Server 2019 所支援的升級路徑。

![就地升級圖表](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

下列步驟是使用相同硬體從 Windows Server 2012 容錯移轉叢集伺服器到 Windows Server 2019 的範例。  

開始進行任何升級之前，請確定已完成目前的備份，包括系統狀態。  此外，請確定所有驅動程式和固件已更新為您將使用之作業系統的認證等級。  這裡不會涵蓋這兩個注意事項。

在下列範例中，容錯移轉叢集的名稱是叢集，而節點名稱是1和2。

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>步驟 1:收回第一個節點並升級至 Windows Server 2016

1. 在容錯移轉叢集管理員中，以滑鼠右鍵按一下節點，然後選取 [**暫停**和**清空角色**]，將所有資源從節點1清空至節點2。  或者，您可以使用 PowerShell 命令 [[暫止-start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)]。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. 以滑鼠右鍵按一下節點，然後選取 [**更多動作**並**收回**]，從叢集收回節點1。  或者，您可以使用 PowerShell 命令[移除-start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. 做為預防措施，請從您所使用的儲存體卸離節點1。  在某些情況下，中斷電腦的存放裝置纜線的連線就夠了。  如有需要，請洽詢您的存放裝置廠商，以取得適當的表示中斷連結步驟。  視您的儲存空間而定，這可能不是必要的。

4. 重建具有 Windows Server 2016 的節點1。  請確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

5. 使用節點1建立名為 CLUSTER1 的新叢集。  開啟容錯移轉叢集管理員，然後在 [**管理**] 窗格中，選擇 [**建立**叢集] 並遵循嚮導中的指示。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. 建立叢集之後，就必須將角色從原始叢集遷移到這個新叢集。  在新叢集上，以滑鼠右鍵按一下叢集名稱（CLUSTER1），然後選取 [**其他動作**] 和 [**複製叢集角色**]。  依照嚮導的指示來遷移角色。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  所有資源都已遷移之後，請關閉節點2（原始叢集），然後中斷存放裝置的連線，以便不會造成任何干擾。  將存放裝置連接到節點1。  一旦連線之後，請將所有資源上線，並確保其正常運作。

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>步驟 2:將第二個節點重建至 Windows Server 2019

一旦您確認所有專案都能正常運作，您可以將節點2重建到 Windows Server 2019 並加入叢集。

1. 在節點2上執行 Windows Server 2019 的全新安裝。 請確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

2. 現在原始叢集（叢集）已消失，您可以將新的叢集名稱保留為 CLUSTER1，或回到原始名稱。  如果您想要回到原始名稱，請遵循下列步驟：
   
   a. 在節點1上的容錯移轉叢集管理員中，以滑鼠右鍵按一下叢集的名稱（CLUSTER1），然後選擇 [**屬性**]。
   
   b. 在 [**一般**] 索引標籤上，將叢集重新命名為 cluster。

   c. 選擇 [確定] 或 [套用] 時，您會看到下列對話方塊快顯視窗。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. 叢集服務將會停止，並需要重新開機才能完成重新命名。

3. 在節點1上，開啟容錯移轉叢集管理員。  以滑鼠右鍵按一下**節點**，然後選取 [**新增節點**]。  流覽嚮導將節點2新增至叢集。

4. 將存放裝置連接到節點2。 這可能包括重新連接存放裝置纜線。 

5. 以滑鼠右鍵按一下節點，然後選取 [**暫停**並**清空角色**]，將所有資源從節點1清空至節點2。  或者，您可以使用 PowerShell 命令 [[暫止-start-clusternode](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)]。  請確定所有資源都在線上，而且運作正常。

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>步驟 3：重建第一個節點到 Windows Server 2019

1. 從叢集收回節點1，並以您先前的方式從節點中斷存放裝置的連線。

2. 將節點重建或升級至 Windows Server 2019。  請確定您已新增所有必要的角色、功能、驅動程式和安全性更新。

3. 重新附加存放裝置，並將節點1新增回叢集。

4. 將所有資源移至節點1，並確保它們上線，並視需要運作。

5. 目前的叢集功能等級會保留在 Windows 2016。  使用 PowerShell 命令[更新-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)，將功能等級更新為 Windows 2019。

您目前正在使用功能完整的 Windows Server 2019 容錯移轉叢集來執行。

## <a name="additional-notes"></a>其他備註

- 如先前所述，中斷存放裝置的連線可能不是必要的。  在我們的檔中，我們想要在注意時錯誤。  請洽詢您的存放裝置廠商。
- 如果您的起點是 Windows Server 2008 或 2008 R2 叢集，則可能需要執行額外的步驟。
- 如果叢集正在執行虛擬機器，請確定在使用 PowerShell 命令[更新-VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)完成叢集功能等級之後，升級虛擬機器層級。
- 請注意，如果您正在執行 SQL Server、Exchange Server 等應用程式，應用程式將不會使用 [複製叢集角色] wizard 進行遷移。  您應洽詢應用程式廠商，以取得應用程式的適當遷移步驟。