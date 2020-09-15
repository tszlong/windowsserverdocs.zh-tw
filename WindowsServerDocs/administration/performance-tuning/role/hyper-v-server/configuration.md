---
title: Hyper-V 設定
description: 效能微調的 hyper-v 設定考慮
ms.topic: article
ms.author: asmahi
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: a7779b882fe6a704dcf12819ad91042c20381eba
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077215"
---
# <a name="hyper-v-configuration"></a>Hyper-V 設定

## <a name="hardware-selection"></a>硬體選擇

執行 Hyper-v 之伺服器的硬體考慮通常類似于非虛擬化伺服器，但執行 Hyper-v 的伺服器可能會顯示更多的 CPU 使用量，耗用更多記憶體，且需要較大的 i/o 頻寬，因為伺服器合併。

-   **處理器**

    Windows Server 2016 中的 hyper-v 會將邏輯處理器顯示為每個使用中虛擬機器的一或多個虛擬處理器。 Hyper-v 現在需要支援第二層位址轉譯的處理器 (SLAT) 技術，例如擴充的頁面資料表 (EPT) 或嵌套頁面資料表 (NPT) 。

-   **Cache**

    Hyper-v 可受益于較大型的處理器快取，特別是在記憶體中有大型工作集的負載，以及虛擬處理器與邏輯處理器的比率很高的虛擬機器設定。

-   **記憶體**

    實體伺服器需要足夠的記憶體供根和子磁碟分割之用。 根磁碟分割需要記憶體，才能代表虛擬機器和作業（例如虛擬機器快照集）有效率地執行 i/o。 Hyper-v 可確保根磁碟分割有足夠的記憶體可供使用，並允許將剩餘的記憶體指派給子磁碟分割。 子分割區應該根據每個虛擬機器的預期負載需求來調整大小。

-   **存放裝置**

    存放裝置硬體應該有足夠的 i/o 頻寬和容量，以符合實體伺服器所裝載之虛擬機器的目前和未來需求。 當您選取儲存控制器和磁片，並選擇 RAID 設定時，請考慮這些需求。 將具有高度磁片密集型工作負載的虛擬機器放在不同的實體磁片上，可能會改善整體效能。 例如，如果四部虛擬機器共用單一磁片並主動使用它，每部虛擬機器只能產生該磁片的25% 頻寬。

## <a name="power-plan-considerations"></a>電源計劃考慮

作為核心技術，虛擬化是一項功能強大的工具，可用於提高伺服器工作負載密度、減少資料中心內所需的實體伺服器數目、提高營運效率，以及降低耗電量成本。 電源管理是成本管理的關鍵。

在理想的資料中心環境中，電源耗用量的管理方式是將工作合併到電腦上，直到它們大多處於忙碌狀態，然後關閉閒置的機器。 如果此方法不可行，系統管理員可以利用實體主機上的電源計劃，以確保它們不會耗用比所需更多的電源。

伺服器電源管理技術有一種成本，特別是當租使用者工作負載不受信任時，會對主機服務提供者的實體基礎結構提出相關的原則。 主機層軟體可供您推斷如何將輸送量最大化，同時將耗電量降至最低。 在大部分閒置的機器中，這可能會導致實體基礎結構得出適當的中等功率繪製，導致個別的租使用者工作負載執行速度比其他情況還慢。

Windows Server 在各種案例中使用虛擬化。 從輕微載入的 IIS 伺服器到適度忙碌的 SQL Server，到每一部伺服器執行數百部虛擬機器的 Hyper-v 雲端主機。 每個案例都可能有獨特的硬體、軟體和效能需求。 根據預設，Windows Server 會使用並建議 **平衡** 的電源計劃，藉由根據 CPU 使用率調整處理器效能，來節省電源。

在 **平衡** 的電源計劃中，只會在實體主機相當忙碌的情況下，才會套用最高的電源狀態 (和租使用者工作負載中的最低回應延遲) 。 如果您針對所有租使用者工作負載值具決定性的低延遲回應，則應該考慮從預設的 **平衡** 電源計劃切換至 **高效** 能電源計劃。 高效 **能的電源計劃** 將隨時以完整的速度執行處理器，並有效停用以需求為基礎的切換，以及其他電源管理技術，並在省電時優化效能。

對於客戶而言，對於減少實體伺服器數量並想要確保它們達到其虛擬化工作負載的最大效能，您應該考慮使用 **高效** 能電源計劃，以節省成本。

如需運用電源計劃將基礎結構優化的其他建議和見解，請閱讀 [建議的平衡電源計劃參數，以取得快速回應時間](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Server Core 安裝選項

Windows Server 2016 的功能是 Server Core 安裝選項。 Server Core 提供一個基本的環境來裝載一組精選的伺服器角色，包括 Hyper-v。 它為主機 OS 提供較小的磁片使用量，以及較小的攻擊和服務介面。 因此，我們強烈建議 Hyper-v 虛擬化伺服器使用 Server Core 安裝選項。

Server Core 安裝只會在使用者登入時提供主控台視窗，但是 Hyper-v 會公開遠端系統管理功能，包括 [Windows Powershell](/powershell/module/hyper-v/?view=win10-ps) ，讓系統管理員可以從遠端系統管理。

## <a name="dedicated-server-role"></a>專用伺服器角色

根磁碟分割應專門用於 Hyper-v。 在執行 Hyper-v 的伺服器上執行其他伺服器角色可能會對虛擬化伺服器的效能造成負面影響，特別是當它們耗用大量的 CPU、記憶體或 i/o 頻寬時。 將根分割區中的伺服器角色最小化有額外的優點，例如減少受攻擊面。

系統管理員應謹慎考慮根磁碟分割中安裝的軟體，因為有些軟體可能會對執行 Hyper-v 之伺服器的整體效能造成負面影響。

## <a name="guest-operating-systems"></a>客體作業系統

Hyper-v 支援並已針對許多不同的客體作業系統進行調整。 每個來賓支援的虛擬處理器數目取決於客體作業系統。 如需支援的客體作業系統清單，請參閱 [Hyper-v 總覽](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))。

## <a name="cpu-statistics"></a>CPU 統計資料

Hyper-v 會發佈效能計數器，以協助描述虛擬化伺服器的行為，並回報資源使用狀況。 在 Windows 中用來查看效能計數器的標準工具組包含效能監視器和 Logman.exe，可顯示並記錄 Hyper-v 效能計數器。 相關計數器物件的名稱前面會加上 **hyper-v**。

您應該一律使用 Hyper-v 虛擬程式邏輯處理器效能計數器來測量實體系統的 CPU 使用量。 在根和子磁碟分割中工作管理員和效能監視器報告的 CPU 使用率計數器不會反映實際的實體 CPU 使用量。 使用下列效能計數器來監視效能：

- **Hyper-v 虛擬程式邏輯處理器 (\*) % 的總 \\ 執行時間** （邏輯處理器的非閒置時間總數）

- **Hyper-v 虛擬程式邏輯處理器 (\*) \\ % guest 執行時間** 花費在來賓或主機內執行迴圈的時間

- **Hyper-v 虛擬程式邏輯處理器 (\*) \\ % 進程管理執行時間** 花費在執行程式內的時間

- **Hyper-v 虛擬程式根虛擬處理器 (\*) \\ \\ *** 測量根磁碟分割的 CPU 使用量

- **Hyper-v 虛擬處理器虛擬處理器 (\*) \\ \\ *** 測量 guest 磁碟分割的 CPU 使用量


## <a name="additional-references"></a>其他參考資料

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)