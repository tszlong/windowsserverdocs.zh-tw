---
title: 封包監視器 (Pktmon)
description: 本主題提供 (Pktmon) network diagnostics tool 的封包監視器總覽。
ms.topic: overview
author: khdownie
ms.author: v-kedow
ms.date: 11/12/2020
ms.openlocfilehash: cab9de79c1d53b505acb61020c71472365ded348
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330490"
---
# <a name="packet-monitor-pktmon"></a>封包監視器 \( Pktmon\)

>適用于： Windows Server (半年通道) 、Windows Server 2019、Windows 10、Azure Stack HCI、Azure Stack Hub、Azure

封包監視器 (Pktmon) 是適用于 Windows 的現成、跨元件網路診斷工具。 它可用於封包捕獲、封包捨棄偵測、封包篩選和計數。 此工具在虛擬化案例（例如容器網路和 SDN）中特別有用，因為它可在網路堆疊中提供可見度。 您可以透過 pktmon.exe 命令和 Windows Admin Center 擴充功能來提供此功能。 

## <a name="overview"></a>概觀

透過網路進行通訊的任何機器都至少有一個網路介面卡。 此介面卡和應用程式之間的所有元件都形成網路堆疊：一組可處理和移動網路流量的網路元件。 在傳統案例中，網路堆疊很小，而且所有的封包路由和切換都會發生在外部裝置中。

<center>

:::image type="content" source="media/networking-stack.png" alt-text="傳統案例中的網路堆疊" border="false":::

</center>

不過，隨著網路虛擬化的出現，網路堆疊的大小也會乘以。 此擴充網路堆疊現在包含可處理封包處理和切換的虛擬交換器等元件。 這種彈性的環境可提供更好的資源使用率和安全性隔離，但也有更多空間可讓您難以診斷的設定錯誤。 封包監視器提供網路堆疊內的增強可見度，通常需要用來指出這些錯誤。

<center>

:::image type="content" source="media/packet-capture.png" alt-text="PacketMon 的跨元件封包捕獲" border="false":::

</center>

封包監視器會在網路堆疊中的多個位置攔截封包，並公開封包路由。 如果網路堆疊中受支援的元件捨棄封包，封包監視器將會報告該封包卸載。 這可讓使用者區別某封包的目標群組件，以及干擾封包的元件。 此外，封包監視器將會報告捨棄的原因;例如，MTU Mistmatch 或已篩選的 VLAN 等等。這些捨棄原因可提供問題的根本原因，而不需要耗盡所有的可能性。 封包監視也會為每個攔截點提供封包計數器，以啟用高階封包流程檢查，而不需要耗時的記錄分析。

<center>

:::image type="content" source="media/drop-detection.png" alt-text="PacketMon 的捨棄偵測" border="false":::

</center>

## <a name="best-practices"></a>最佳做法

使用這些最佳作法來簡化您的網路分析。

- 檢查引數和功能的命令列說明 (例如 pktmon start help) 。
- 設定符合您的案例的封包篩選器 (pktmon 篩選新增) 。
- 在實驗期間檢查封包計數器，以進行高階視圖 (pktmon 計數器) 。
- 查看記錄檔以取得詳細的分析 (pktmon format pktmon. etl) 。

## <a name="functionality"></a>功能

封包監視器的功能已透過 Windows 版本發展。 下表顯示其主要功能和對應的 Windows 版本。

| 功能                                                                  | V 1809 (B:17763)  | V 1903 (B:18362)  | V 2004 (B:19041)  |
|:---------------------------------------------------------------------------:|:----------------:|:----------------:|:----------------:|
| 封包監視和依網路堆疊在多個位置進行計數 | &#x2611;         | &#x2611;         | &#x2611;         |
| 多個堆疊位置的封包捨棄偵測                          | &#x2611;         | &#x2611;         | &#x2611;         |
| 彈性執行時間封包篩選                                           | &#x2611;         | &#x2611;         | &#x2611;         |
| 封裝支援                                                       | &#x2610;         | &#x2611;         | &#x2611;         |
| 以 TcpDump 封包剖析為基礎的網路分析                            | &#x2610;         | &#x2611;         | &#x2611;         |
|  (OOB) 分析的封包中繼資料                                              | &#x2610;         | &#x2610;         | &#x2611;         |
| 即時螢幕封包監視                                       | &#x2610;         | &#x2610;         | &#x2611;         |
| 大量記憶體內部記錄                                               | &#x2610;         | &#x2610;         | &#x2611;         |
| Wireshark 和網路監視器格式支援                                | &#x2610;         | &#x2610;         | &#x2611;         |

>[!NOTE]
>Azure Stack HCI 和 Azure Stack Hub 客戶應該預期 Vibranium 的功能。

## <a name="limitations"></a>限制

以下是一些封包監視器最重要的限制。

- 目前只支援乙太網路流量。 在稍後的版本中將會新增無線流量支援。
- 尚未透過封包監視器顯示來自 Windows 防火牆的封包。 

## <a name="get-started-with-packet-monitor"></a>開始使用封包監視器

下列資源可協助您開始使用「封包監視器」。

### <a name="pktmon-command-syntax-and-formatting"></a>[Pktmon 命令語法和格式](pktmon-syntax.md)

封包監視器可透過 Vibranium OS 上的 pktmon.exe 命令 (組建 19041) 取得。 您可以 [使用本主題](pktmon-syntax.md) 來瞭解如何瞭解 pktmon 語法、命令、格式化和輸出。

### <a name="packet-monitoring-extension-in-windows-admin-center"></a>[Windows Admin Center 中的封包監視擴充功能](pktmon-wac-extension.md)

封包監視延伸模組可讓您透過 Windows Admin Center 操作和取用封包監視器。 此延伸模組可協助您診斷網路，方法是在容易追蹤和操作的記錄檔中，透過網路堆疊來捕捉和顯示網路流量。 您可以 [使用本主題](pktmon-wac-extension.md) 來瞭解如何操作工具，並瞭解其輸出。

### <a name="sdn-data-path-diagnostics-extension-in-windows-admin-center"></a>[Windows Admin Center 中的 SDN 資料路徑診斷延伸模組](pktmon-sdn-data-path-wac-extension.md)

SDN 資料路徑診斷是 Windows Admin Center SDN 監視延伸模組內的工具。 此工具會根據各種 SDN 案例，自動進行以封包監視器為基礎的封包捕捉，並以單一視圖呈現輸出，方便您追蹤和操作。 您可以 [使用本主題](pktmon-sdn-data-path-wac-extension.md) 來瞭解如何操作工具，並瞭解其輸出。

### <a name="microsoft-network-monitor-netmon-support"></a>[Microsoft 網路監視器 (Netmon) 支援](pktmon-netmon-support.md)

封包監視器會以 ETL 格式產生記錄。 您可以使用特殊剖析器，利用 Microsoft 網路監視器 (Netmon) 來分析這些記錄。 [本主題](pktmon-netmon-support.md) 說明如何在 Netmon 內分析封包監視器產生的 ETL 檔案。

### <a name="wireshark-pcapng-format-support"></a>[Wireshark (pcapng 格式) 支援](pktmon-pcapng-support.md)

封包監視器可將記錄轉換為 pcapng 格式。 您可以使用 Wireshark (或任何 pcapng analyzer) 來分析這些記錄。 [本主題](pktmon-pcapng-support.md) 說明預期的輸出，以及如何利用它。

## <a name="provide-feedback-to-engineering-team"></a>提供意見反應給工程小組

使用下列步驟報告任何 bug，或透過意見反應中樞提供意見反應：

1.  **Feedback Hub**   透過 [**開始**] 功能表啟動意見反應中樞。

1. 請選取 [回報 **問題** ] 按鈕或 [ **建議功能** ] 按鈕。

1. 在 **摘要您的問題** 方塊中提供有意義的意見反應標題   。

1. 在 [ **提供給我們更多詳細資料**] 方塊中提供重現問題的詳細資料和步驟   。

1. 選取 [ **網路和網際網路**]   作為最上層類別，然後封 **包監視器 ( # A0)** 作為子類別。

1. 為了協助我們更快找出並修正錯誤，請捕獲螢幕擷取畫面、附加 pktmon 的輸出記錄檔，以及/或重新建立問題。

1. 按一下 [ **提交**]。

提交意見/bug 之後，工程團隊將能夠查看意見反應，並加以解決。
