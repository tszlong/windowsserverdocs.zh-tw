---
title: Hyper-v 處理器效能
description: Hyper-v 效能調整中的處理器效能考慮
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: ab68975c84e1b299914e09045eddbc302ca9697f
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87896072"
---
# <a name="hyper-v-processor-performance"></a>Hyper-v 處理器效能


## <a name="virtual-machine-integration-services"></a>虛擬機器整合服務

虛擬機器 Integration Services 包含適用于 Hyper-v 特定 i/o 裝置的啟用 rms 驅動程式，可大幅減少與模擬裝置相較之下，i/o 的 CPU 額外負荷。 您應該在每個支援的虛擬機器中安裝最新版本的虛擬機器 Integration Services。 服務會降低來賓的 CPU 使用量，從閒置的來賓到大量使用的來賓，並改善 i/o 輸送量。 這是在執行 Hyper-v 的伺服器中調整效能的第一步。 如需支援的客體作業系統清單，請參閱[Hyper-v 總覽](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="virtual-processors"></a>虛擬處理器

Windows Server 2016 中的 hyper-v 支援每個虛擬機器最多240個虛擬處理器。 若虛擬機器的負載不需要大量 CPU，則應該設定為使用一個虛擬處理器。 這是因為與多個虛擬處理器相關聯的額外負荷，例如客體作業系統中的其他同步處理成本。

如果虛擬機器在尖峰負載下需要一個以上的處理 CPU，請增加虛擬處理器的數目。

## <a name="background-activity"></a>背景活動

將閒置虛擬機器中的背景活動降到最低會釋放其他虛擬機器可在其他地方使用的 CPU 週期。 Windows 來賓在閒置時，通常會使用少於一% 的 CPU。 以下是將虛擬機器的背景 CPU 使用量降到最低的幾個最佳作法：

-   安裝最新版本的虛擬機器 Integration Services。

-   透過 [虛擬機器設定] 對話方塊移除模擬的網路介面卡 (使用 Microsoft Hyper-v 專用介面卡) 。

-   移除未使用的裝置（例如，CD-ROM 和 COM 埠），或中斷其媒體連線。

-   不使用 Windows 客體作業系統時，請將它保留在登入畫面上，並停用螢幕保護裝置。

-   檢查預設啟用的排程工作和服務。

-   執行**logman.exe 查詢-ets** ，以檢查預設開啟的 ETW 追蹤提供者

-   改善伺服器應用程式，以減少週期性的活動 (例如計時器) 。

-   關閉主機和客體作業系統上的伺服器管理員。

-   請勿讓 Hyper-v 管理員繼續執行，因為它會持續重新整理虛擬機器的縮圖。

以下是在虛擬機器中設定 Windows*用戶端版本*以減少整體 CPU 使用量的其他最佳作法：

-   停用背景服務，例如 SuperFetch 和 Windows 搜尋。

-   停用排程的工作，例如排程的重組。

## <a name="virtual-numa"></a>虛擬 NUMA

若要啟用虛擬化大規模的大規模工作負載，Windows Server 2016 中的 Hyper-v 已擴充虛擬機器調整限制。 一部虛擬機器最多可以指派240個虛擬處理器和 12 TB 的記憶體。 建立這類大型虛擬機器時，可能會使用來自主機系統上多個 NUMA 節點的記憶體。 在這類虛擬機器設定中，如果虛擬處理器和記憶體不是從相同的 NUMA 節點配置，工作負載可能會因為無法利用 NUMA 優化而造成效能不佳。

在 Windows Server 2016 中，Hyper-v 會將虛擬 NUMA 拓撲提供給虛擬機器。 根據預設，這個虛擬 NUMA 拓撲已最佳化，以符合基礎主機電腦的 NUMA 拓撲。 公開到虛擬機器的虛擬 NUMA 拓撲，可讓客體作業系統以及在其中執行的任何 NUMA 感知應用程式利用 NUMA 效能最佳化，就如同在實體電腦上執行時一樣。

從工作負載的觀點來看，虛擬和實體 NUMA 之間並沒有差別。 在虛擬機器內，當工作負載將本機記憶體配置給資料，並存取相同的 NUMA 節點中該資料時，基礎實體系統上可以快速存取本機記憶體。 由於存取遠端記憶體而犧牲效能的情況可以成功避免。 只有 NUMA 感知應用程式可以受益于 Vnuma 解決方案。

Microsoft SQL Server 是 NUMA 感知應用程式的範例。 如需詳細資訊，請參閱[瞭解非統一記憶體存取](https://technet.microsoft.com/library/ms178144.aspx)。

虛擬 NUMA 和動態記憶體功能無法同時使用。 有效地啟用「動態記憶體」的虛擬機器只有一個虛擬 NUMA 節點，並且不論虛擬 NUMA 設定為何，都不會對虛擬機器提供任何 NUMA 拓撲。

如需虛擬 NUMA 的詳細資訊，請參閱[Hyper-v 虛擬 Numa 總覽](https://technet.microsoft.com/library/dn282282.aspx)。

## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
