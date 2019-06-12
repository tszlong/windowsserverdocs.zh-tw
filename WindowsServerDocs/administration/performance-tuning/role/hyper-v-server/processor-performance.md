---
title: Hyper V 處理器效能
description: 在 HYPER-V 效能微調的處理器效能考量
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: f16ee9cff9c244a8c579e008bced1e90b1a20673
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435596"
---
# <a name="hyper-v-processor-performance"></a>Hyper V 處理器效能


## <a name="virtual-machine-integration-services"></a>虛擬機器整合服務

虛擬機器整合服務會包含已啟用的驅動程式的額外負荷相較於模擬的裝置的 i/o 會大幅降低 CPU 的 Hyper-v 特定 I/O 裝置。 您應該在每個支援的虛擬機器中安裝最新版的虛擬機器整合服務。 來賓，從閒置的 CPU 使用量來賓到大量的服務減少使用來賓，並提高 I/O 輸送量。 這是執行 HYPER-V 之伺服器的微調效能的第一個步驟。 如需支援的客體作業系統的清單，請參閱 < [HYPER-V 概觀](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="virtual-processors"></a>虛擬處理器

240 每台虛擬機器的虛擬處理器最多可支援 Windows Server 2016 中的 HYPER-V。 具有不需要大量 CPU 的負載的虛擬機器應該設定為使用一個虛擬處理器中。 這是因為多個虛擬處理器的詳細資訊，例如客體作業系統中的其他同步處理成本與相關聯的額外負荷。

如果虛擬機器需要的處理尖峰負載下的多個 CPU，請增加虛擬處理器數目。

## <a name="background-activity"></a>背景活動

最小化在閒置的虛擬機器中之背景活動釋放 CPU 循環，可供其他虛擬機器的其他位置。 Windows 來賓在閒置時，通常會使用小於 1%的 CPU。 以下是幾個背景的虛擬機器的 CPU 使用量降到最低的最佳作法：

-   安裝最新版的虛擬機器整合服務。

-   移除模擬的網路介面卡，透過虛擬機器設定 對話方塊中 （使用 Microsoft Hyper-v 特定配接器）。

-   移除未使用的裝置，例如 CD-ROM 和 COM 連接埠，或中斷其媒體連線。

-   保持登入畫面上的 Windows 客體作業系統，它未使用時，停用螢幕保護裝置。

-   檢閱排程的工作和服務，預設會啟用。

-   檢閱位於預設執行的 ETW 追蹤提供者**logman.exe 查詢-ets**

-   改善伺服器應用程式，以減少定期活動 （例如計時器）。

-   關閉主機和客體作業系統上的 [伺服器管理員]。

-   不會保留為 HYPER-V 管理員執行，因為它經常會重新整理虛擬機器的縮圖。

以下是設定的其他最佳作法*用戶端版本*的 Windows 虛擬機器中以減少整體 CPU 使用率：

-   停用 SuperFetch 等 Windows Search 的背景服務。

-   停用排程的工作，例如已排程的磁碟重組。

## <a name="virtual-numa"></a>虛擬 NUMA

若要啟用虛擬化大型向上延展工作負載，在 Windows Server 2016 的 HYPER-V 擴充虛擬機器大小限制。 可以指派單一的虛擬機器，最多 240 個虛擬處理器與 12 TB 的記憶體。 在建立這類大型的虛擬機器時，將可能利用主機系統上的多個 NUMA 節點的記憶體。 在這類虛擬機器組態中，如果虛擬處理器和記憶體不會配置相同的 NUMA 節點中，從工作負載可能不正確的效能，因為無法充分利用 NUMA 最佳化。

在 Windows Server 2016 中，HYPER-V 會提供給虛擬機器的虛擬 NUMA 拓撲。 根據預設，這個虛擬 NUMA 拓撲已最佳化，以符合基礎主機電腦的 NUMA 拓撲。 公開到虛擬機器的虛擬 NUMA 拓撲，可讓客體作業系統以及在其中執行的任何 NUMA 感知應用程式利用 NUMA 效能最佳化，就如同在實體電腦上執行時一樣。

就工作負載的角度而言，虛擬和實體 NUMA 沒有區別。 在虛擬機器內，當工作負載將本機記憶體配置給資料，並存取相同的 NUMA 節點中該資料時，基礎實體系統上可以快速存取本機記憶體。 由於存取遠端記憶體而犧牲效能的情況可以成功避免。 NUMA 感知的應用程式有何助益的 vNUMA。

Microsoft SQL Server 是 NUMA 感知應用程式的範例。 如需詳細資訊，請參閱 <<c0> [ 了解非統一記憶體存取](https://technet.microsoft.com/library/ms178144.aspx)。

虛擬 NUMA 和動態記憶體功能無法同時使用。 有效地啟用「動態記憶體」的虛擬機器只有一個虛擬 NUMA 節點，並且不論虛擬 NUMA 設定為何，都不會對虛擬機器提供任何 NUMA 拓撲。

如需虛擬 NUMA 的詳細資訊，請參閱[HYPER-V 虛擬 NUMA 概觀](https://technet.microsoft.com/library/dn282282.aspx)。

## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
