---
title: Hyper-V 設定
description: HYPER-V 設定考量效能微調
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: e5e55ad492439fcb7150469d9a35b639f5ff9a2f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830509"
---
# <a name="hyper-v-configuration"></a>Hyper-V 設定

## <a name="hardware-selection"></a>選擇硬體

通常執行 HYPER-V 之伺服器的硬體考量類似的非虛擬化伺服器，但是對執行 HYPER-V 的伺服器可以展現更高的 CPU 使用量、 耗用更多記憶體，並需要更大的 I/O 頻寬，因為伺服器彙總。

-   **處理器**

    Windows Server 2016 中的 HYPER-V 會將邏輯處理器呈現成一或多個虛擬處理器，每個作用中的虛擬機器。 HYPER-V 現在需要支援第二層位址轉譯 (SLAT) 技術，例如擴充 Page Tables (EPT) 或巢狀 Page Tables (NPT) 的處理器。

-   **快取**

    HYPER-V 可以受益於較大的處理器快取，特別是針對大型工作集記憶體中和在虛擬機器設定虛擬處理器與邏輯處理器的比例很高的負載。

-   **記憶體**

    實體伺服器需要足夠的記憶體，根和子資料分割。 在根磁碟分割需要有效率地執行 I/o，代表的虛擬機器和作業，例如虛擬機器快照的記憶體。 HYPER-V 可確保有足夠的記憶體根磁碟分割，可以使用，而讓剩餘的記憶體指派給子磁碟分割。 子磁碟分割應該根據每個虛擬機器的預期負載需求調整大小。

-   **儲存空間**

    存放裝置硬體應該擁有足夠的 I/O 頻寬和容量以符合目前和未來需求的虛擬機器的實體伺服器主機。 當您選取儲存體控制器和磁碟，並選擇 RAID 組態，請考慮這些需求。 將耗用大量磁碟的工作負載的虛擬機器放置在不同的實體磁碟上可能會改善整體效能。 比方說，如果四個虛擬機器共用單一磁碟，並主動使用它，每個虛擬機器可能會產生只有 25%的磁碟頻寬。

## <a name="power-plan-considerations"></a>電源計劃考量

核心技術，為虛擬化是功能強大的工具有助於增加伺服器工作負載密度、 減少必要的實體伺服器，在您的資料中心的數目、 提高操作效率和降低電源耗用量成本。 電源管理是很重要的成本管理。 

在理想的資料中心環境中，功率耗用量的管理方式合併到電腦的工作，直到他們大多很忙，並再關閉閒置的機器。 如果這個方法並不實用，系統管理員可以運用在實體主機上以確保它們不會耗用比所需的更多電源的電源計劃。 

伺服器電源管理技術需要付出代價，尤其是與租用戶工作負載不受信任來決定主機服務提供者的實體基礎結構有關的原則。 主機層級軟體會保留給推斷如何最大化輸送量，同時降低功率耗用量。 在大部分是閒置的機器，這可能造成結束中等 power 繪製適用時，在執行比它們否則可能速度緩慢的個別租用戶工作負載所產生的實體基礎結構。

Windows Server 會使用各種不同的案例中的虛擬化。 從輕 IIS 伺服器至中度忙線中的 SQL Server，到雲端主機與 HYPER-V 執行數以百計的每一部伺服器的虛擬機器。 每個案例可能會有唯一的硬體、 軟體和效能需求。 根據預設，Windows Server 會使用和建議**平衡**可藉由相應的處理器效能可節約用電電源計劃根據 CPU 使用量。

具有**平衡**只有較忙碌的實體主機時，才套用電源計劃、 最高的電源狀態 （與最低的回應延遲，在租用戶工作負載）。 如果您的值具決定性、 低延遲的回應所有租用戶工作負載，您應該考慮從預設切換**平衡**電源計劃**高效能**電源計劃。 **高效能**電源計劃會處理器以全速一直執行，有效地停用需求為基礎的切換，以及其他電源管理技術，並最佳化效能，所有電源節約。

客戶感到滿意減少實體伺服器數目的成本節省，且想要確保他們達到其虛擬化工作負載的最大效能，您應該考慮使用**高效能**電源計劃。

如需其他建議和深入了解利用最佳化基礎結構的電源計劃，讀取[建議平衡電源計劃參數快速回應時間](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Server Core 安裝選項

Windows Server 2016 功能的 Server Core 安裝選項。 Server Core 提供的最小的環境來裝載一組精選的包括 HYPER-V 的伺服器角色。 它的特色其主機 OS，較小的磁碟使用量較小的攻擊和服務介面。 因此，我們強烈建議為 HYPER-V 虛擬化伺服器使用 Server Core 安裝選項。

Server Core 安裝在使用者登入，但 HYPER-V 遠端管理功能，包括公開 （expose） 時，才提供的主控台視窗[Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx)讓系統管理員可以從遠端管理。

## <a name="dedicated-server-role"></a>專用的伺服器角色

在根磁碟分割應專門用於 HYPER-V。 執行 HYPER-V 的伺服器上執行額外的伺服器角色可能會影響效能的虛擬化伺服器，特別是當它們耗用大量的 CPU、 記憶體或 I/O 頻寬。 最小化在根磁碟分割中的伺服器角色中還有其他好處，例如縮小攻擊面。

系統管理員應該仔細考慮哪些軟體已安裝在根磁碟分割，因為某些軟體可能會影響執行 HYPER-V 之伺服器的整體效能。

## <a name="guest-operating-systems"></a>客體作業系統

HYPER-V 支援，而且已針對幾個不同的客體作業系統的調整。 針對每個來賓所支援的虛擬處理器數目取決於客體作業系統。 如需支援的客體作業系統的清單，請參閱 < [HYPER-V 概觀](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="cpu-statistics"></a>CPU 統計資料

HYPER-V 會發佈效能計數器，可幫助描述虛擬化伺服器的行為和報告資源使用狀況。 一組標準的 Windows 中檢視效能計數器的工具包括效能監視器和 Logman.exe，這些可以顯示和記錄 HYPER-V 效能計數器。 相關計數器物件的名稱前面會加上**HYPER-V**。

您應該一律使用 HYPER-V Hypervisor 邏輯處理器效能計數器測量實體系統的 CPU 使用量。 CPU 使用率計數器根目錄中的該工作管理員和效能監視報表和子磁碟分割不會反映了實際的實體 CPU 使用量。 您可以使用下列效能計數器來監視效能：

-   **HYPER-V Hypervisor 邏輯處理器 (\*)\\%執行時間總計**邏輯處理器的總非閒置時間

-   **HYPER-V Hypervisor 邏輯處理器 (\*)\\客體執行時間百分比**所花費的時間內的客體或主機內執行的循環

-   **HYPER-V Hypervisor 邏輯處理器 (\*)\\Hypervisor 執行時間百分比**hypervisor 中執行花費的時間

-   **HYPER-V Hypervisor 根虛擬處理器 (\*)\\ \*** 測量根磁碟分割的 CPU 使用量

-   **HYPER-V Hypervisor 的虛擬處理器 (\*)\\ \*** 測量來賓磁碟分割的 CPU 使用量


## <a name="see-also"></a>另請參閱

-   [HYPER-V 術語](terminology.md)

-   [HYPER-V 架構](architecture.md)

-   [HYPER-V 處理器效能](processor-performance.md)

-   [HYPER-V 記憶體效能](memory-performance.md)

-   [HYPER-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [HYPER-V 網路 I/O 效能](network-io-performance.md)

-   [虛擬化環境中偵測瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
