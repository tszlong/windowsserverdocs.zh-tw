---
title: Hyper-V 架構
description: 效能微調的 hyper-v 架構 condsiderations
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: asmahi; sandysp; jopoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 47ff4a25f67e2b03655d17ab5a57aeaa3274a835
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851831"
---
# <a name="hyper-v-architecture"></a>Hyper-V 架構

Hyper-v 具有一種以類型為基礎的虛擬機器型架構。 虛擬程式會將處理器和記憶體虛擬化，並為根磁碟分割中的虛擬化堆疊提供可管理子磁碟分割（虛擬機器）的機制，並向虛擬機器公開服務，例如 i/o 裝置。

根磁碟分割擁有，且可直接存取實體 i/o 裝置。 根磁碟分割中的虛擬化堆疊提供虛擬機器、管理 Api 和虛擬化 i/o 裝置的記憶體管理員。 它也會執行模擬裝置，例如整合式裝置電子（IDE）磁片控制站和 PS/2 輸入裝置埠，並支援 Hyper-v 特定的綜合裝置，以提高效能並降低額外負荷。

![hyper-v 以虛擬機器為基礎的架構](../../media/perftune-guide-hyperv-arch.png)

Hyper-v 特有的 i/o 架構是由子磁碟分割的根磁碟分割和虛擬化服務用戶端（Vsc）中的虛擬化服務提供者（Vsp）所組成。 每個服務都會透過 VMBus 公開為裝置，其可做為 i/o 匯流排，並可在使用共用記憶體等機制的虛擬機器之間進行高效能通訊。 客體作業系統的隨插即用 manager 會列舉這些裝置，包括 VMBus，並載入適當的設備磁碟機（虛擬服務用戶端）。 I/o 以外的服務也會透過此架構公開。

從 Windows Server 2008 開始，作業系統功能會 enlightenment，以在虛擬機器中執行時優化其行為。 優點包括降低記憶體虛擬化的成本、改善多核心的擴充性，以及降低客體作業系統的背景 CPU 使用量。

下列各節建議的最佳作法，在執行 Hyper-v 角色的伺服器上產生更高的效能。

## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
