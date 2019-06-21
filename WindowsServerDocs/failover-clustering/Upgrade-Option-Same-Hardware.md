---
title: 升級容錯移轉叢集使用相同的硬體
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本文說明升級 2 個節點容錯移轉叢集使用相同的硬體
ms.localizationpriority: medium
ms.openlocfilehash: 6787d852cc5075e306373a163814135190f27fd6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280248"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>升級的相同硬體上的容錯移轉叢集

> 適用於：Windows Server 2019，Windows Server 2016

容錯移轉叢集是一組一起運作以提高可用性的應用程式和服務的獨立電腦。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一個叢集節點失敗，另一個節點即會開始提供服務 (稱為容錯移轉的處理程序)。 使用者會感覺的服務中斷。

本指南說明升級到 Windows Server 2019 或 Windows Server 2016 的叢集節點，從使用相同的硬體較早版本的步驟。

## <a name="overview"></a>總覽

升級作業系統，在現有的容錯移轉叢集時，才支援從 Windows Server 2016 轉為 Windows 2019。  如果容錯移轉叢集執行較早的版本，例如 Windows Server 2012 R2 和舊版中，升級的叢集服務正在執行時不會允許聯結在一起的節點。  如果使用相同的硬體，可以採取步驟，以取得較新版本。  

之前的容錯移轉叢集升級程序，請參閱[Windows 升級中心](https://www.microsoft.com/upgradecenter)。  當您 Windows Server 就地升級時，您會將從現有的作業系統版本較新的版本，但仍使用相同硬體上。 升級的就地至少一個和有時候這兩個版本開始，可以是 Windows Server。 例如，可以升級 Windows Server 2012 R2 和 Windows Server 2016 就地升級到 Windows Server 2019。  也請記住[叢集移轉精靈](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/)可用，但僅適用於最多傳回兩個版本。 下圖顯示適用於 Windows Server 的升級路徑。 指標的向下箭號代表移動到 Windows Server 2019 的較舊版本的支援升級路徑。

![就地升級的圖表](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

下列步驟是要使用的相同硬體的 Windows Server 2019 從 Windows Server 2012 容錯移轉叢集伺服器的範例。  

開始之前任何的升級，請確定已完成目前的備份，包括系統狀態。  也請確定所有的驅動程式與韌體更新為經認證的層級，您將使用的作業系統。  這些兩個資訊不涵蓋以下。

在下列範例中，容錯移轉叢集的名稱是叢集，節點名稱是 NODE1 和 NODE2。

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>步驟 1：收回第一個節點，並升級至 Windows Server 2016

1. 在 [容錯移轉叢集管理員] 中，會耗盡所有資源 NODE1 到 NODE2 由以滑鼠右鍵按一下節點並選取**暫停**並**都清空角色**。  或者，您可以使用 PowerShell 命令[SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. 收回叢集以滑鼠右鍵按一下節點，然後選取從 NODE1**其他動作**並**收回**。  或者，您可以使用 PowerShell 命令[移除節點](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. 為求安全起見，您可以從您使用的儲存體中斷連結 NODE1。  在某些情況下，存放裝置纜線中斷連線的電腦就夠了。  洽詢正確中斷連結步驟視存放裝置廠商。  根據您的儲存體，這可能不需要。

4. 重建 NODE1 使用 Windows Server 2016。  請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

5. 建立新的叢集，稱為 CLUSTER1 與 NODE1。  開啟 [容錯移轉叢集管理員] 並在**管理**窗格中，選擇**建立叢集**並遵循精靈中的指示。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. 叢集建立之後，必須從原始的叢集移轉到這個新的叢集角色。  在新叢集上，以滑鼠右鍵滑鼠按一下叢集名稱 (CLUSTER1)，然後選取**其他動作**並**複製叢集角色**。  遵循精靈來移轉角色。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  已移轉的所有資源，一旦關閉 NODE2 （原始叢集），並中斷連線的儲存體，以便不會造成任何干擾。  將存放裝置連線到 NODE1。  所有連接之後，讓所有的資源上線，並確定運作應該如此。

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>步驟 2：重新建置以 Windows Server 2019 的第二個節點

一旦確認一切如預期運作之後，可以重建為 Windows Server 2019 NODE2，並加入叢集。

1. 在 NODE2 上執行 Windows Server 2019 的全新安裝。 請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

2. 原始叢集 （叢集） 已消失，您可以保留 CLUSTER1 為新的叢集名稱，或回到原來的名稱。  如果您想要回到原來的名稱，請遵循下列步驟：
   
   a. 在 [1]，在容錯移轉叢集管理員以滑鼠右鍵按一下叢集 (CLUSTER1) 的名稱，然後選擇**屬性**。
   
   b. 在 **一般**索引標籤上，重新命名為叢集的叢集。

   c. 在選擇 [確定] 或 [套用] 時，您會看到以下快顯對話方塊。

    ![清空節點](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. 叢集服務會停止，且必須重新啟動以完成重新命名。

3. 在 1，開啟 容錯移轉叢集管理員。  以滑鼠右鍵按一下**節點**，然後選取**加入節點**。  瀏覽精靈將 NODE2 加入叢集。

4. 將存放裝置連接到 NODE2。 這可能包括重新連接存放裝置纜線。 

5. 清空所有資源從 NODE1 到 NODE2 以滑鼠右鍵按一下節點並選取**暫停**並**都清空角色**。  或者，您可以使用 PowerShell 命令[SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。  請確定所有資源都都在線上，且應該如此運作。

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>步驟 3：重新建置以 Windows Server 2019 的第一個節點

1. 從叢集收回 NODE1 和儲存體中斷的節點中的方式，從您先前。

2. 重建或升級至 Windows Server 2019 的 NODE1。  請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

3. 重新連接存放裝置，然後將 NODE1 新增回叢集。

4. 前往 NODE1 上的所有資源，並確定上線與視函式。

5. 在 Windows 2016，維持目前的叢集功能等級。  使用 PowerShell 命令，更新為 Windows 2019 的功能等級[UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)。

您現在正在執行包含功能完整的 Windows Server 2019 容錯移轉叢集。

## <a name="additional-notes"></a>其他附註

- 如先前所述，中斷連接的儲存體可能會或可能不需要。  在我們的文件中，我們要不要粗心大意。  請洽詢存放裝置廠商。
- 如果您的起點是 Windows Server 2008 或 2008 R2 叢集，則可能需要透過其他執行的步驟。
- 如果叢集正在執行的虛擬機器，請確定已完成叢集功能等級，但 PowerShell 命令之後，升級的虛擬機器層級[更新 VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)。
- 請注意，是否您執行 SQL Server、 Exchange Server 等應用程式，應用程式將不會移轉使用 [複製叢集角色精靈]。  如需應用程式的適當的移轉步驟，應洽詢您的應用程式廠商。