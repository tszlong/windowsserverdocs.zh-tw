---
title: Windows Admin Center 中的封包監視延伸模組
description: 您可以使用此頁面，透過 Windows Admin Center (Pktmon) 來操作和取用封包監視器。
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 85d204d1731ebcf21c55364f48ebe0e7b6dc157c
ms.sourcegitcommit: 8808f871c8cf131f819ef5540286218bd425da96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2020
ms.locfileid: "94632454"
---
# <a name="packet-monitoring-extension-in-windows-admin-center"></a>Windows Admin Center 中的封包監視延伸模組

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視延伸模組可讓您透過 Windows Admin Center 操作和取用封包監視器。 此延伸模組可協助您診斷網路，方法是在容易追蹤和操作的記錄檔中，透過網路堆疊來捕捉和顯示網路流量。

## <a name="what-is-packet-monitor-pktmon"></a>什麼是 (Pktmon) 的封包監視器？
封包監視器 (Pktmon) 是適用于 Windows 的現成、跨元件網路診斷工具。 它可用於封包捕獲、封包捨棄偵測、封包篩選和計數。 此工具在虛擬化案例（例如容器網路和 SDN）中特別有用，因為它可在網路堆疊中提供可見度。

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center？
Windows Admin Center 是在本機部署、以瀏覽器為基礎的管理工具，可讓您管理沒有 Azure 或雲端相依性的 Windows 伺服器。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，這在未連線至網際網路的私人網路上管理伺服器特別有用。 Windows Admin Center 是「隨附」管理工具 (例如伺服器管理員和 MMC) 的現代化演進。

## <a name="before-you-start"></a>開始之前
- 若要使用此工具，目標伺服器必須執行 Windows Server 2019 1903 版 (19H1) 和更新版本。
- [安裝 Windows Admin Center](/windows-server/manage/windows-admin-center/deploy/install)。
- 將伺服器新增至 Windows Admin Center：
  1. 按一下 [所有連線] 底下的 [+ 新增]。
  2. 加入宣告伺服器連接。
  3. 輸入伺服器的名稱，如果出現提示，請輸入要使用的認證。
  4. 按一下 [ **提交** ] 以完成。

伺服器將會新增至 [ **總覽** ] 頁面上的連接清單。

## <a name="getting-started"></a>開始使用

若要取得此工具，請流覽至您在上一個步驟中建立的伺服器，然後移至 [封包監視] 延伸模組。

## <a name="applying-filters"></a>套用篩選

強烈建議您在開始任何封包捕獲之前套用篩選，因為當焦點放在封包的單一資料流程時，針對特定目的地的連線能力會更容易進行。 另一方面，捕捉所有網路流量可能會導致輸出太雜訊而無法分析。 因此，此延伸模組會先引導您前往 [篩選] 窗格，然後再開始捕獲。 您可以按一下 **[下一步]** 以不使用篩選器開始捕獲，來略過此步驟。 [篩選] 窗格會引導您新增3個步驟中的篩選準則。

1. 依網路堆疊元件篩選

   如果您想要捕捉只通過特定元件 (s) 的流量，[篩選] 窗格的第一個步驟會顯示網路堆疊版面配置，讓您可以選取要篩選的元件 () 。 這也是分析和瞭解電腦網路堆疊版面配置的絕佳位置。

   :::image type="content" source="media/filtering-by-networking-stack-components.png" alt-text="依網路堆疊元件篩選的範例" border="true":::

2. 依封包參數篩選

   在第二個步驟中，窗格可讓您依參數的參數篩選封包。 針對要報告的封包，它必須符合至少一個篩選準則中指定的所有條件;一次最多可支援8個篩選準則。 您可以針對每個篩選器指定封包參數，例如 MAC 位址、IP 位址、埠、Ethertype、傳輸通訊協定、VLAN 識別碼。

   - 當指定了兩個 Mac、Ip 或埠時，此工具不會區分來源或目的地;它會捕捉具有兩個值的封包，無論是目的地或來源。 不過，顯示篩選可能會造成這種差異;請參閱下方的 [ [顯示篩選](#display-filters) ] 區段。
   - 若要進一步篩選 TCP 封包，可以提供選擇性的 TCP 旗標清單來進行比對。 支援的旗標為 FIN、SYN、RST、PSH、ACK、URG、ECE 和 CWR。
   - 如果已核取 [ **封裝** ] 方塊，除了外部封包之外，此工具也會將篩選套用至封裝的內部封包。 支援的封裝方法有 VXLAN、GRE、NVGRE 和 IP IP。 自訂 VXLAN 埠是選擇性的，預設值為4789。

   :::image type="content" source="media/filtering-by-packet-parameters.png" alt-text="依封包參數篩選的範例" border="true":::

3. 依封包流程狀態篩選

   依預設，封包監視器會捕獲流動和捨棄的封包。 若只要捕捉捨棄的封包，請選取 [捨棄的封 **包** ]。

   :::image type="content" source="media/filtering-by-packet-flow-status.png" alt-text="依封包流程狀態篩選的範例" border="true":::

   之後會顯示所有選取之篩選準則的摘要，以供審查。 透過 [ **捕捉條件** ] 按鈕啟動 capture 之後，您就可以取出該視圖。

   :::image type="content" source="media/filters-review.png" alt-text="如何只捕獲捨棄的封包" border="true":::

## <a name="capture-log"></a>Capture 記錄檔

結果會顯示在表格中，其中顯示已捕捉封包的主要參數：時間戳記、來源 IP 位址、來源埠、目的地 IP 位址、目的地埠、Ethertype、通訊協定、TCP 旗標、封包是否已卸載，以及捨棄原因。

   - 每個封包的時間戳記也是超連結，會將您重新導向至不同的頁面，您可以在其中找到所選封包的詳細資訊。 請參閱下方的詳細資料頁面一節。
   - 所有捨棄的封包在已卸載的索引標籤中都有 "True" 值、卸載原因，並以紅色文字顯示，以使其更容易 **找出。**
   - 所有索引標籤都可以遞增和遞減排序。
   - 您可以使用搜尋列，在記錄檔中的任何資料行中搜尋值。
   - 您可以使用 [ **重新開機** ] 按鈕，以相同選擇的篩選器重新開機 capture。

   :::image type="content" source="media/capture-log-result.png" alt-text="Capture 記錄結果資料表的範例" border="true":::

## <a name="details-page"></a>詳細資料頁面

此頁面會顯示每個區域網路堆疊的元件所流動的封包快照。 此視圖會顯示封包流程路徑，並可讓您調查封包如何在其傳遞的每個元件處理時變更。

   - 封包快照集會依每個介面卡/交換器堆疊分組;亦即，介面卡/交換器所捕捉的封包快照集、其篩選器驅動程式和其通訊協定驅動程式，將會以介面卡/交換器的名稱分組。 這可讓您更輕鬆地追蹤封包到另一個介面卡的流程。
   - 選取快照集時，會顯示此特定快照集的詳細資料，包括原始的封包標頭。
   - 所有捨棄的封包在已卸載的索引標籤中都有 "True" 值、卸載原因，並以紅色文字顯示，以使其更容易 **找出。**

   :::image type="content" source="media/details-page.png" alt-text="顯示封包快照的詳細資料頁面範例" border="true":::

## <a name="display-filters"></a>顯示篩選

顯示篩選器可讓您在捕捉封包之後篩選記錄檔。 您可以針對每個篩選器指定封包參數，例如 MAC 位址、IP 位址、埠、Ethertype 和傳輸通訊協定。 不同于 capture 篩選：

   - 顯示篩選器可以區分 IP 位址的來源和目的地、MAC 位址和埠。
   - 您可以在套用顯示篩選之後，加以刪除和編輯，以變更記錄檔的視圖。
   - 顯示篩選會在儲存的記錄中反轉。

   :::image type="content" source="media/display-filters.png" alt-text="顯示篩選畫面" border="true":::

## <a name="save-feature"></a>儲存功能

[儲存] 按鈕可讓您將記錄檔儲存在您的本機電腦、遠端電腦或兩者。 顯示篩選會在儲存的記錄檔中反轉。

   - 如果記錄檔儲存在您的本機電腦上，您將能夠以各種格式儲存它：
      - Etl 格式，可使用 Microsoft 網路監視器進行分析。 注意：如需詳細資訊，請參閱此頁面。
      - 可以使用任何文字編輯器（例如 TextAnalysisTool.NET）來分析的文字格式。
      - 您可以使用 Wireshark 之類的工具來分析 Pcapng fomat。
         - 在此轉換期間，大部分的封包監視器中繼資料將會遺失。 如需詳細資訊，[請參閱此頁面](pktmon-pcapng-support.md)。

   :::image type="content" source="media/packet-monitoring-save-feature.png" alt-text="儲存 capture 的本機複本" border="true":::

## <a name="open-feature"></a>開啟功能

開啟的功能可讓您重新開啟五個最後儲存的記錄檔，以透過工具進行分析。

   :::image type="content" source="media/open.png" alt-text="開啟最近的記錄" border="true":::