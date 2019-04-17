---
title: 升級使用相同的硬體容錯移轉叢集
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: 本文章說明升級使用相同的硬體 2 節點容錯移轉叢集
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121275"
---
# 升級相同的硬體上的容錯移轉叢集

> 適用於： Windows Server 2019、 Windows Server 2016

容錯移轉叢集是一組獨立的電腦，它們共同運作以提升應用程式和服務的可用性。 各個叢集伺服器 (稱為節點) 是透過實體纜線與軟體連接。 如果其中一個叢集節點失敗，另一個節點開始提供服務 （稱為 「 容錯移轉程序）。 使用者不會感覺到中斷服務的情況。

本指南描述從較早的版本，使用相同的硬體，升級至 Windows Server 2019 或 Windows Server 2016 的叢集節點的步驟。

## 概觀

升級作業系統上現有的容錯移轉叢集時，才支援從 Windows Server 2016 前往 Windows 2019。  如果容錯移轉叢集執行較早的版本，例如，例如 Windows Server 2012 R2 和較舊版本升級叢集服務正在執行時將不會允許一起加入節點。  如果使用相同的硬體，就可以採取步驟以將它放到較新版本。  

之前容錯移轉叢集的任何升級，請參閱[Windows 升級中心](https://www.microsoft.com/upgradecenter)。  當您升級 Windows Server 就地時，移動從現有的作業系統版本至較新版本，但仍使用相同的硬體。 升級的就地至少一個和有時兩個版本往前，可以是 Windows Server。 例如，可以升級 Windows Server 2012 R2 和 Windows Server 2016 就地升級到 Windows Server 2019。  也請記住，可用於[叢集移轉精靈](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/)，但僅支援最多兩個版本上一步。 下圖顯示適用於 Windows Server 的升級路徑。 指標的向下箭號代表從舊版 Windows Server 2019 直到移支援升級路徑。

![就地升級的圖表](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

下列步驟都的 Windows Server 2012 容錯移轉叢集伺服器移至 Windows Server 2019 使用相同的硬體的範例。  

在開始之前升級程序，請確定目前的備份，包括系統狀態完成了。  也請確定所有驅動程式和韌體更新的作業系統，您將使用認證的層級。  以下未會涵蓋這些兩個註解。

在以下範例中的容錯移轉叢集名稱是叢集而且節點名稱都 NODE1 且 NODE2。

## 步驟 1： 收回第一個節點和升級至 Windows Server 2016

1. 在容錯移轉叢集管理員中，清空所有資源 NODE1 從 NODE2 以滑鼠右鍵按一下的節點上，並選取**暫停**和**都清空角色**。  或者，您可以使用 PowerShell 命令[暫停 CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。

    ![清空節點](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. 收回 NODE1 從透過滑鼠右鍵按一下 \ [] 節點，並選取**其他動作**和**收回**叢集。  或者，您可以使用 PowerShell 命令[REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode)。

    ![清空節點](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. 預防措施，請將 NODE1 中斷您正在使用的儲存空間。  在某些情況下，從電腦中斷連線的儲存體纜線便已足夠。  請洽詢適當分隊步驟，視您的存放裝置廠商。  這不可能需要根據您的存放裝置，而定。

4. 重新建置 NODE1 與 Windows Server 2016。  請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

5. 建立名為 CLUSTER1 NODE1 與新的叢集。  開啟 \ [容錯移轉叢集管理員中**管理**\] 窗格中，選擇 [**建立叢集**並依照精靈中的指示。

    ![清空節點](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. 一旦建立叢集時，將需要從原始叢集移轉至這個新的叢集角色。  在新的叢集上滑鼠右鍵按一下叢集名稱 (CLUSTER1) 並選取**其他動作**及**複製叢集角色**。  請依照移轉角色精靈 」 中。

    ![清空節點](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  一旦所有資源都已都移轉，關閉 NODE2 的電源 （原始叢集） 與中斷連接的儲存空間以不會造成任何干擾。  將存放裝置連接到 NODE1 中。  所有連線後，將所有資源連線，並確保它們在視應該會正常運作。

## 步驟 2： 重建以 Windows Server 2019 的第二個節點

一旦您確認一切如預期運作時，可以重建以 Windows Server 2019 NODE2，並加入叢集。

1. NODE2 上執行 Windows Server 2019 的全新安裝。 請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

2. 現在，消失原始叢集 （叢集），您可以保留新叢集名稱為 CLUSTER1 或回復到原來的名稱。  如果您想要回復到原來的名稱，請依照下列步驟：
   
   a. 上 NODE1，在容錯移轉叢集管理員滑鼠右鍵按一下叢集 (CLUSTER1) 的名稱，然後選擇 [**屬性**]。
   
   b。 **一般**索引標籤中，將重新命名為叢集中的叢集。

   c. 在選擇 [確定] 或 [套用] 時，您會看到以下對話方塊快顯視窗。

    ![清空節點](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. 叢集服務將會停止，並需要重新啟動，才能完成重新命名。

3. 在上 NODE1，開啟容錯移轉叢集管理員。  在**節點**上的按一下滑鼠右鍵，然後選取 [**新增節點**。  瀏覽 NODE2 新增至叢集精靈。

4. 附加 NODE2 存放裝置。 這可能包括重新連接的儲存體纜線。 

5. 清空所有資源 NODE1 從 NODE2 以滑鼠右鍵按一下的節點上，並選取**暫停**和**都清空角色**。  或者，您可以使用 PowerShell 命令[暫停 CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode)。  確定所有資源都是在線上且它們應該運作。

## 步驟 3： 重建以 Windows Server 2019 的第一個節點

1. 從叢集收回 NODE1 和存放裝置中斷連接的方式，從節點哪些您先前。

2. 重新建置或升級至 Windows Server 2019 的 NODE1。  請確定您已新增所有必要的角色、 功能、 驅動程式和安全性更新。

3. 重新附加存放裝置，並新增 NODE1 回至叢集。

4. 將所有資源都移至 NODE1，並確保它們上線，以及在必要時運作。

5. 目前叢集功能等級會保持在 Windows 2016。  使用 PowerShell 命令[更新 CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel)Windows 2019 更新的功能層級。

您現在執行完全正常運作的 Windows Server 2019 容錯移轉叢集。

## 其他附註

- 如先前所述，中斷連接的存放裝置可能或可能不需要。  在我們的文件中，我們要放鬆警告。  請洽詢您的存放裝置廠商。
- 如果您的起點是 Windows Server 2008 或 2008 R2 叢集，可能需要透過其他執行的步驟。
- 如果叢集中執行虛擬機器，請確定叢集功能等級具有使用 PowerShell 命令[更新 VMVERSION](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion)已完成後，升級虛擬機器層級。
- 請注意，是否您正在執行的應用程式，例如 SQL Server、 Exchange Server 等，應用程式將不會移轉使用複製叢集角色精靈。  如需應用程式的適當的移轉步驟洽詢您的應用程式廠商。