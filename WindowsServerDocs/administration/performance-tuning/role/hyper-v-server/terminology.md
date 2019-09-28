---
title: Hyper-v 術語
description: Hyper-v 效能微調中有用的 hyper-v 術語
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: acd61e9edef3ac88027d0cc89618c537fa6aa25f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71385035"
---
# <a name="hyper-v-terminology"></a>Hyper-v 術語
本節將摘要說明在此效能微調主題中使用的虛擬機器技術的主要術語：

| 詞彙        | 定義           |
| ------------- |:------------|
|*子分割* | 根磁碟分割所建立的任何虛擬機器。|
|*裝置虛擬化* | 一種機制，可讓您在多個取用者之間抽象化和共用硬體資源。|
|*模擬裝置*|模擬實際實體硬體裝置的虛擬化裝置，讓來賓可以使用該硬體裝置的典型驅動程式。|
|*啟蒙教學*|對客體作業系統的優化，讓它知道虛擬機器環境，並微調虛擬機器的行為。|
|*guest*|正在磁碟分割中執行的軟體。 它可以是功能完整的作業系統或小型的特殊用途核心。 虛擬程式是與來賓無關的。|
|*hypervisor*|位於硬體和一或多個作業系統之下的一層軟體。 其主要工作是提供稱為磁碟分割的隔離執行環境。 每個分割區都有一組自己的虛擬化硬體資源（中央處理單位或 CPU、記憶體和裝置）。 Hypervisor 可控制及仲裁對基礎硬體的存取權。|
|*邏輯處理器*| 處理一個執行執行緒（指令資料流程）的處理單位。 每個處理器核心可以有一或多個邏輯處理器，以及每個處理器通訊端一或多個核心。|
| *傳遞磁片存取*|做為來賓內虛擬磁片的整個實體磁片的標記法。 資料和命令會傳遞至實體磁片（透過根磁碟分割的原生儲存堆疊），而不會由虛擬堆疊進行任何中間處理。|
|*根磁碟分割*|第一個建立的根磁碟分割，並擁有該管理程式不會擁有的所有資源，包括大部分的裝置和系統記憶體。 根磁碟分割會主控虛擬化堆疊，並建立和管理子分割。|
|*Hyper-v 特定裝置*|不具實體硬體類比的虛擬化裝置，因此來賓可能需要驅動程式（虛擬化服務用戶端），才能使用該 Hyper-v 特定的裝置。 此驅動程式可以使用虛擬機器匯流排（VMBus）與根磁碟分割中的虛擬化裝置軟體進行通訊。|
|*虛擬機器*|由軟體模擬所建立並具有與實際電腦相同特性的虛擬電腦。|
| *虛擬網路交換器*|（也稱為虛擬交換器）實體網路交換器的虛擬版本。 您可以將虛擬網路設定成可以存取一或多部虛擬機器的本機或外部網路資源。|
|*虛擬處理器*|已排程在邏輯處理器上執行之處理器的虛擬抽象概念。 虛擬機器可以有一或多個虛擬處理器。|
|*虛擬化服務用戶端（VSC）*|來賓載入以取用資源或服務的軟體模組。 就 i/o 裝置而言，虛擬化服務用戶端可以是作業系統核心載入的設備磁碟機。|
| *虛擬化服務提供者（VSP）*|  根磁碟分割中的虛擬化堆疊所公開的提供者，可提供資源或服務，例如 i/o 到子磁碟分割。|
| *虛擬化堆疊*|根磁碟分割中的軟體元件集合，可搭配使用以支援虛擬機器。 虛擬化堆疊適用于和位於虛擬程式之上。 它也提供管理功能。|
|*VMBus*|以通道為基礎的通訊機制，用於具有多個作用中虛擬化磁碟分割之系統上的分割區通訊和裝置列舉。 VMBus 會隨著 Hyper-V 整合服務一起安裝。|

## <a name="see-also"></a>另請參閱

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 伺服器設定](configuration.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
