---
title: HYPER-V 術語
description: Hyper-v 術語中 HYPER-V 效能微調很有用
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: bc970633ff24827207eb3a27e282656f2486a6eb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841139"
---
# <a name="hyper-v-terminology"></a>Hyper V 術語
本節摘要說明重要術語特定虛擬機器的技術，可在此效能微調主題：

| 詞彙        | 定義           |
| ------------- |:------------|
|*子磁碟分割* | 在根磁碟分割由任何虛擬機器。|
|*裝置虛擬化* | 一種機制，可讓硬體抽取並且在多個取用者之間共用資源。|
|*模擬的裝置*|一種虛擬化的裝置，以模擬實際的實體硬體裝置，好讓來賓可以使用該硬體裝置的一般驅動程式。|
|*enlightenment*|最佳化，以讓它知道虛擬機器環境，並調整其行為的虛擬機器的客體作業系統。|
|*guest*|資料分割中執行的軟體。 它可以是功能完整的作業系統或小型的特殊用途的核心。 Hypervisor 與客體無關。|
|*hypervisor*|位於硬體之上和之下一個或多個作業系統的軟體層。 其主要工作是提供稱為磁碟分割的隔離執行環境。 每個資料分割都有它自己組 （中央處理器或 CPU、 記憶體和裝置） 的虛擬化的硬體資源。 Hypervisor 可控制及仲裁對基礎硬體的存取權。|
|*邏輯處理器*| 處理執行 （指令串流） 的一個執行緒的處理器。 可以是每個處理器核心的一或多個邏輯處理器和每個處理器通訊端的一或多個核心。|
| *穿通磁碟存取*|為客體內虛擬磁碟表示整顆實體磁碟。 資料和命令傳遞到任何中介處理實體磁碟 （透過在根磁碟分割的原生的儲存體堆疊） 由虛擬堆疊。|
|*根磁碟分割*|根磁碟分割先建立和擁有 hypervisor 並不會包括大部分的裝置和系統記憶體的所有資源。 根分割區裝載的虛擬化堆疊和建立及管理子磁碟分割。|
|*Hyper-V 特定裝置*|虛擬化與任何實體硬體類比裝置，因此來賓可能需要的驅動程式 （虛擬化服務用戶端） 該 Hyper-v 特定裝置。 驅動程式可以使用虛擬機器匯流排 (VMBus) 與虛擬化的裝置軟體，在根磁碟分割通訊。|
|*虛擬機器*|虛擬電腦模擬軟體所建立，且具有與實際的電腦相同的特性。|
| *虛擬網路交換器*|（也稱為虛擬交換器）實體網路交換器的虛擬版本。 您可以將虛擬網路設定成可以存取一或多部虛擬機器的本機或外部網路資源。|
|*虛擬處理器*|排程為邏輯處理器上執行的處理器虛擬的抽象概念。 虛擬機器可以有一或多個虛擬處理器。|
|*虛擬化服務用戶端 (VSC)*|來賓載入取用的資源或服務的一種軟體模組。 對於 I/O 裝置虛擬化服務用戶端可能會載入作業系統核心裝置驅動程式。|
| *虛擬化服務提供者 (VSP)*|  提供者公開的根磁碟分割的虛擬化堆疊，提供資源或服務，例如 I/O 子磁碟分割。|
| *虛擬化堆疊*|在根磁碟分割中的軟體元件，會一起運作以支援虛擬機器的集合。 虛擬化堆疊搭配，並位在 hypervisor 上面。 它也會提供管理功能。|
|*VMBus*|間資料分割有多個作用中的虛擬化分割區的系統上的通訊與裝置列舉所用的通道為基礎的通訊機制。 VMBus 會隨著 Hyper-V 整合服務一起安裝。|

## <a name="see-also"></a>另請參閱

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 伺服器組態](configuration.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
