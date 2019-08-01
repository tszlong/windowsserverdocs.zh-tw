---
title: Hyper-V 設定
description: 效能微調的 hyper-v 設定考慮
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: Asmahi; SandySp; JoPoulso
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: baea091482818c581414ba1d9c1c01db2a52e3d7
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/31/2019
ms.locfileid: "66435661"
---
# <a name="hyper-v-configuration"></a>Hyper-V 設定

## <a name="hardware-selection"></a>硬體選擇

執行 Hyper-v 之伺服器的硬體考慮通常類似于非虛擬化伺服器, 但執行 Hyper-v 的伺服器可能會顯示增加的 CPU 使用量、耗用較多的記憶體, 而且因為伺服器匯總而需要較大的 i/o 頻寬。

-   **處理器**

    Windows Server 2016 中的 hyper-v 將邏輯處理器呈現成每部作用中虛擬機器的一或多個虛擬處理器。 Hyper-v 現在需要支援第二層位址轉譯 (SLAT) 技術的處理器, 例如延伸頁面表格 (EPT) 或嵌套頁面資料表 (NPT)。

-   **快取**

    Hyper-v 可以從較大的處理器快取中獲益, 特別是在記憶體中有大型工作集的負載, 以及虛擬處理器與邏輯處理器的比率高的虛擬機器設定中。

-   **記憶體**

    實體伺服器需要足夠的記憶體供根和子磁碟分割之用。 根磁碟分割需要記憶體, 才能代表虛擬機器和作業 (例如虛擬機器快照集) 有效率地執行 i/o。 Hyper-v 可確保根磁碟分割可以使用足夠的記憶體, 並允許將剩餘的記憶體指派給子磁碟分割。 子磁碟分割應該根據每個虛擬機器的預期負載需求來調整大小。

-   **儲存空間**

    存放裝置硬體應有足夠的 i/o 頻寬和容量, 以符合實體伺服器所裝載之虛擬機器目前和未來的需求。 當您選取 [存放控制器和磁片] 並選擇 RAID 設定時, 請考慮這些需求。 將具有高度磁片密集型工作負載的虛擬機器放置在不同的實體磁片上, 可能會改善整體效能。 例如, 如果有四部虛擬機器共用單一磁片並主動使用它, 則每部虛擬機器只能產生該磁片的 25% 頻寬。

## <a name="power-plan-considerations"></a>電源計劃考慮

作為核心技術, 虛擬化是一種功能強大的工具, 可用於增加伺服器工作負載密度、減少資料中心內所需的實體伺服器數量、提高營運效率, 並降低耗電量成本。 電源管理對於成本管理很重要。 

在理想的資料中心環境中, 電源耗用量的管理方式是將工作合併到電腦上, 直到它們很忙碌, 然後關閉閒置的機器。 如果這種方法並不實用, 系統管理員可以利用實體主機上的電源計劃, 以確保它們不會耗用超過所需的電源。 

伺服器電源管理技術有成本, 特別是當租使用者工作負載不受信任來聽寫主機服務提供者實體基礎結構的原則時。 主機層軟體會保留以推斷如何將輸送量最大化, 同時將耗電量降至最低。 在大部分的閒置機器中, 這可能會導致實體基礎結構結束一般的電源繪製, 導致個別的租使用者工作負載執行速度比以往更慢。

Windows Server 會在各種不同的案例中使用虛擬化。 從輕量載入的 IIS 伺服器到適度忙碌的 SQL Server, 到具有 Hyper-v 且每部伺服器執行數百部虛擬機器的雲端主機。 這些案例中的每一個都可能有獨特的硬體、軟體和效能需求。 根據預設, Windows Server 會使用並建議**平衡**的電源計劃, 藉由調整以 CPU 使用率為基礎的處理器效能來節省電源。

使用**平衡**電源計劃時, 只有當實體主機相當忙碌時, 才會套用最高的電源狀態 (以及租使用者工作負載中最低的回應延遲)。 如果您針對所有租使用者工作負載, 為具決定性的低延遲回應, 您應該考慮從預設的**平衡**電源計劃切換到**高效**能的電源計劃。 高效  能的電源計劃會隨時以全速的速度執行處理器, 並有效地停用以需求為基礎的切換以及其他電源管理技術, 並透過電力節約優化效能。

對於滿足減少實體伺服器數量並想要確保它們達到虛擬化工作負載的最大效能的客戶, 您應該考慮使用**高效**能的電源計劃。

如需運用電源計劃將基礎結構優化的其他建議和深入解析, 請參閱[建議的平衡電源計劃參數以快速回應時間](../../hardware/power/recommended-balanced-plan-parameters.md)



## <a name="server-core-installation-option"></a>Server Core 安裝選項

Windows Server 2016 的功能為 Server Core 安裝選項。 Server Core 提供裝載一組選取的伺服器角色 (包括 Hyper-v) 的最小環境。 它的功能是主機 OS 的磁片使用量較小, 且攻擊和服務介面較小。 因此, 強烈建議 Hyper-v 虛擬化伺服器使用 Server Core 安裝選項。

Server Core 安裝只有在使用者登入時才會提供主控台視窗, 但 Hyper-v 會公開包含[Windows Powershell](https://technet.microsoft.com/library/hh848559.aspx)的遠端系統管理功能, 讓系統管理員可以從遠端進行管理。

## <a name="dedicated-server-role"></a>專用伺服器角色

根磁碟分割應專門用於 Hyper-v。 在執行 Hyper-v 的伺服器上執行其他伺服器角色可能會對虛擬化伺服器的效能造成負面影響, 特別是當它們耗用大量的 CPU、記憶體或 i/o 頻寬時。 將根分割區中的伺服器角色最小化, 會有額外的好處, 例如減少受攻擊面。

系統管理員應該仔細考慮要在根磁碟分割中安裝的軟體, 因為有些軟體可能會對執行 Hyper-v 之伺服器的整體效能造成負面影響。

## <a name="guest-operating-systems"></a>客體作業系統

Hyper-v 支援並已針對數個不同的客體作業系統進行調整。 每個來賓支援的虛擬處理器數目取決於客體作業系統。 如需支援的客體作業系統清單, 請參閱[Hyper-v 總覽](https://technet.microsoft.com/library/hh831531.aspx)。

## <a name="cpu-statistics"></a>CPU 統計資料

Hyper-v 會發佈效能計數器, 以協助描述虛擬化伺服器的行為, 並回報資源使用狀況。 在 Windows 中用來查看效能計數器的一組標準工具組含效能監視器和 Logman, 可顯示並記錄 Hyper-v 效能計數器。 相關計數器物件的名稱前面會加上**hyper-v**。

您應該一律使用 Hyper-v 虛擬機器邏輯處理器效能計數器來測量實體系統的 CPU 使用量。 「工作管理員」和「效能監視器」報表在根和子分割區中的 CPU 使用率計數器, 並不會反映實際的實體 CPU 使用量。 使用下列效能計數器來監視效能:

- **Hyper-v 虛擬機器邏輯處理器 (\*)\\% 總執行時間**-邏輯處理器的非閒置時間總計

- **Hyper-v 虛擬機器邏輯處理器 (\*)\\% Guest 執行時間**花費在來賓或主機內執行迴圈的時間

- **Hyper-v 虛擬機器邏輯處理器 (\*)\\% 監控程式執行時間**花費在虛擬機器內執行的時間

- **Hyper-v 虛擬機器根虛擬\*處理器 ()\\ \\** * 測量根磁碟分割的 CPU 使用量

- **Hyper-v 虛擬處理器 (\*)\\ \\** * 測量來賓磁碟分割的 CPU 使用量


## <a name="see-also"></a>另請參閱

-   [Hyper-V 術語](terminology.md)

-   [Hyper-V 架構](architecture.md)

-   [Hyper-V 處理器效能](processor-performance.md)

-   [Hyper-V 記憶體效能](memory-performance.md)

-   [Hyper-V 存放裝置 I/O 效能](storage-io-performance.md)

-   [Hyper-V 網路 I/O 效能](network-io-performance.md)

-   [偵測虛擬化環境中的瓶頸](detecting-virtualized-environment-bottlenecks.md)

-   [Linux 虛擬機器](linux-virtual-machine-considerations.md)
