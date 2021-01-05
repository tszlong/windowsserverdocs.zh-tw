---
title: Windows Admin Center 中的 SDN 資料路徑診斷擴充功能
description: 您可以使用本主題，利用 Windows Admin Center 中的 SDN 資料路徑診斷延伸模組，將以封包監視器為基礎的封包捕獲自動化
ms.topic: how-to
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: 54a65147847a28a9820589521f7c94ea1c4b08cd
ms.sourcegitcommit: b0c10eaffaa5de3eeff44c433580b41270c27d32
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/30/2020
ms.locfileid: "97826175"
---
# <a name="sdn-data-path-diagnostics-extension-in-windows-admin-center"></a>Windows Admin Center 中的 SDN 資料路徑診斷擴充功能

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

SDN 資料路徑診斷是 Windows Admin Center SDN 監視延伸模組中的一項工具，可根據各種 SDN 案例將以封包監視器為基礎的封包捕獲自動化，並以單一視圖呈現輸出，方便您追蹤和操作。

## <a name="what-is-packet-monitor-pktmon"></a>什麼是 (Pktmon) 的封包監視器？
封包監視器 (Pktmon) 是適用于 Windows 的現成、跨元件網路診斷工具。 它可用於封包捕獲、封包捨棄偵測、封包篩選和計數。 此工具在虛擬化案例（例如容器網路和 SDN）中特別有用，因為它可在網路堆疊中提供可見度。

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center？
Windows Admin Center 是在本機部署、以瀏覽器為基礎的管理工具，可讓您管理沒有 Azure 或雲端相依性的 Windows 伺服器。 Windows Admin Center 可讓您完整控制伺服器基礎結構的各個層面，這在未連線至網際網路的私人網路上管理伺服器特別有用。 Windows Admin Center 是「隨附」管理工具 (例如伺服器管理員和 MMC) 的現代化演進。

## <a name="before-you-start"></a>開始之前
- 若要使用此工具，目標伺服器必須執行 Windows Server 2019 1903 版 (19H1) 和更新版本。
- [安裝 Windows Admin Center](/windows-server/manage/windows-admin-center/deploy/install)。
- 將叢集新增至 Windows Admin Center：
  1. 按一下 [所有連線] 底下的 [+ 新增]。
  2. 選擇新增 Hyper-Converged 叢集連接。
  3. 輸入叢集的名稱，並在出現提示時，輸入要使用的認證。
  4. 核取 **[設定網路控制** 站繼續]。
  5. 輸入網路控制站 URI，然後按一下 [ **驗證**]。
  6. 按一下 [ **新增** ] 以完成。

叢集將會新增至連接清單。 按一下以啟動儀表板。

<center>

:::image type="content" source="media/add-sdn-enabled-hci-connection.png" alt-text="使用 Windows Admin Center 新增 SDN 啟用的 HCI 連接" border="true" lightbox="media/add-sdn-enabled-hci-connection.png":::

</center>

## <a name="getting-started"></a>開始使用

若要取得此工具，請流覽至您在上一個步驟中建立的叢集，然後流覽至 [SDN 監視] 延伸模組，再流覽至 [資料路徑診斷] 索引標籤。

## <a name="selecting-scenarios"></a>選取案例

第一頁列出所有分類為工作負載案例和基礎結構案例的 SDN 案例，如下圖所示。 若要開始，請選取需要診斷的 SDN 案例。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-main-page.png" alt-text="SDN 監視-診斷案例頁面" border="true" lightbox="media/sdn-data-path-diagnostics-main-page.png":::

</center>

## <a name="scenario-parameters"></a>案例參數

選擇案例之後，請填寫必要參數和選擇性參數清單以開始捕獲。 這些基本參數會將工具指向需要診斷的連接。 然後，此工具會使用這些參數來執行成功的捕獲，而不需要使用者介入以找出預期的封包流程、案例中涉及的機器、其在叢集中的位置，或要套用在每部電腦上的 capture 篩選器。 必要的參數可讓 capture 執行，而且選擇性參數有助於篩選掉某些雜訊。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-scenario-parameters.png" alt-text="SDN 監視-捕捉條件頁面" border="true" lightbox="media/sdn-data-path-diagnostics-scenario-parameters.png":::

</center>

## <a name="capture-log"></a>Capture 記錄檔

啟動 capture 之後，擴充功能將會顯示正在啟動 capture 的電腦清單。 如果未儲存您的認證，系統可能會提示您登入這些電腦。 您可以藉由捕捉相對的封包，開始重現您嘗試診斷的 ping 或問題。 攔截封包之後，擴充功能將會在已捕捉封包的電腦旁邊顯示標示。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-loading-wheel2.png" alt-text="啟動封包捕獲" border="true" lightbox="media/sdn-data-path-diagnostics-loading-wheel2.png":::

</center>

停止捕捉之後，所有機器的記錄都會顯示在單一頁面中，並除以電腦標題。 每個標題會包含電腦名稱稱、其在案例中的角色，以及在虛擬機器 (Vm) 的情況下的主機。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-log.png" alt-text="停止捕獲後的資料路徑診斷記錄" border="true" lightbox="media/sdn-data-path-diagnostics-log.png":::

</center>

結果會顯示在表格中，其中顯示已捕捉封包的主要參數：時間戳記、來源 IP 位址、來源埠、目的地 IP 位址、目的地埠、Ethertype、通訊協定、TCP 旗標、封包是否已卸載，以及捨棄原因。

   - 每個封包的時間戳記也是超連結，會將您重新導向至不同的頁面，您可以在其中找到所選封包的詳細資訊。 請參閱下方的詳細資料頁面一節。
   - 所有捨棄的封包在捨棄的索引標籤中都有 "True" 值、卸載原因，並以紅色文字顯示，讓您更容易 **找出。**
   - 所有索引標籤都可以遞增和遞減排序。
   - 您可以使用搜尋列，在記錄檔中的任何資料行中搜尋值。
   - 您可以使用 [ **重新開機** ] 按鈕，以相同選擇的篩選器重新開機該捕獲。

## <a name="details-page"></a>詳細資料頁面

如果您有不正確的封包傳播問題或設定錯誤的問題，此頁面中的資訊就特別有用，因為您可以透過網路堆疊的每個元件來調查封包的流程。 每個封包躍點都有詳細資料，其中包括封包參數和原始封包詳細資料。

- 躍點會根據相關元件分組在一起。 每個介面卡和其頂端的驅動程式都會依介面卡的名稱分組。 這可讓您更輕鬆地透過這些群組標題來追蹤封包的高層級。
- 所有捨棄的封包也會以紅色文字顯示，使其更容易找出。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page.png" alt-text="資料路徑診斷詳細資料頁面" border="true" lightbox="media/sdn-data-path-diagnostics-details-page.png":::

</center>

選取躍點以查看更多詳細資料。 在封裝和 NAT (網路位址轉譯) 案例中，這項功能可讓您在通過網路堆疊時查看封包的變更，並檢查是否有任何設定錯誤的問題。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page-with-pane1.png" alt-text="查看特定躍點的詳細資料" border="true" lightbox="media/sdn-data-path-diagnostics-details-page-with-pane1.png":::

</center>

向下滾動以查看原始封包詳細資料：

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-details-page-with-pane-raw-packet1.png" alt-text="查看特定躍點的原始封包詳細資料" border="true" lightbox="media/sdn-data-path-diagnostics-details-page-with-pane-raw-packet1.png":::

</center>

## <a name="display-filters"></a>顯示篩選

顯示篩選器可讓您在捕捉封包之後篩選記錄檔。 您可以針對每個篩選器指定封包參數，例如 MAC 位址、IP 位址、埠、Ethertype 和傳輸通訊協定。

   - 顯示篩選器可以區分 IP 位址的來源和目的地、MAC 位址和埠。
   - 您可以在套用顯示篩選之後，加以刪除和編輯，以變更記錄檔的視圖。
   - 顯示篩選會在儲存的記錄中反轉。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-display-filters.png" alt-text="使用顯示篩選篩選記錄" border="true" lightbox="media/sdn-data-path-diagnostics-display-filters.png":::

</center>

## <a name="save"></a>儲存

[儲存] 按鈕可讓您將記錄儲存在本機電腦上，以透過其他工具進行進一步的分析。 顯示篩選會在儲存的記錄檔中反轉。 記錄可以不同的格式儲存：

   - ETL 格式，可使用 Microsoft 網路監視器進行分析。 注意：如需詳細資訊，請 [參閱此頁面](pktmon-netmon-support.md) 。
   - 可以使用任何文字編輯器（例如 TextAnalysisTool.NET）來分析的文字格式。
   - 您可以使用 Wireshark 之類的工具來分析 Pcapng fomat。
      - 在此轉換期間，大部分的封包監視器中繼資料將會遺失。 注意：如需詳細資訊，請 [參閱此頁面](pktmon-pcapng-support.md) 。

<center>

:::image type="content" source="media/sdn-data-path-diagnostics-save.png" alt-text="在本機儲存記錄" border="true" lightbox="media/sdn-data-path-diagnostics-save.png":::

</center>
