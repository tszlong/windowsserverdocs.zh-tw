---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: 虛擬機器負載平衡概觀
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 60d6c1b10840b5e0f673099c71b72033178aeb2d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957125"
---
# <a name="virtual-machine-load-balancing-overview"></a>虛擬機器負載平衡概觀

> 適用於：Windows Server 2019、Windows Server 2016

私用雲端部署的主要考慮是資本支出 (<abbr title="資本支出">CapEx</abbr>需要 ) 才能進入生產環境。 將冗余新增至私人雲端部署，以避免在生產環境中的尖峰流量達到容量，但這會增加，這是很常見的情況 <abbr title="資本支出">CapEx</abbr>. 冗余的需求是由某些節點裝載更多虛擬機器的私用雲端不平衡所驅動的 (<abbr title="虛擬機器">VM</abbr>) 和其他人都沒有充分利用的 (，例如剛重新開機的伺服器) 。

<strong>快速影片概觀</strong><br> (6 分鐘) <br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a name="what-is-virtual-machine-load-balancing"></a><a id="what-is-vm-load-balancing"></a>什麼是虛擬機器負載平衡？
<abbr title="虛擬機器">VM</abbr> 負載平衡是 Windows Server 2019 和 Windows Server 2016 中的內建功能，可讓您將容錯移轉叢集中節點的使用優化。 它會識別過度認可的節點並重新散發 <abbr title="虛擬機器">VM</abbr> 從這些節點到受認可的節點。 這項功能的一些顯著層面如下：

* *這是零停機解決方案*： <abbr title="虛擬機器">VM</abbr> 會即時移轉至閒置節點。
* *與您現有叢集環境的無縫整合*.. 會接受失敗原則，例如反親和性、容錯網域和可能的擁有者。
* *適用于平衡的啟發學習法*： <abbr title="虛擬機器">VM</abbr> 節點的記憶體壓力和 CPU 使用率。
* *細微控制*：預設為啟用。 可以視需要啟動，或以週期性間隔啟用。
* 增強*臨界*值：根據您的部署特性而提供的三個閾值。

## <a name="the-feature-in-action"></a><a id="feature-in-action"></a>作用中的功能
### <a name="a-new-node-is-added-to-your-failover-cluster"></a><a id="new-node-added"></a>新的節點會新增至您的容錯移轉叢集中
![要加入至容錯移轉叢集的新節點圖形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

當您將新容量新增到容錯移轉叢集時， <abbr title="虛擬機器">VM</abbr> 負載平衡功能會依照下列順序，自動將現有節點的容量與新增的節點進行平衡：

1. 壓力會在容錯移轉叢集中的現有節點上進行評估。
2. 已識別超過閾值的所有節點。
3. 系統會識別具有最高壓力的節點，以判斷平衡的優先順序。
4. <abbr title="虛擬機器">VM</abbr> 是即時移轉 (，不會從超過閾值的節點) 到容錯移轉叢集中新加入的節點。

### <a name="recurring-load-balancing"></a><a id="recurring-load-balancing"></a>週期性負載平衡
![週期性 VM 負載平衡的圖形](media/vm-load-balancing/overview-VM-load-balancing-2.png)

當設定定期平衡時，叢集節點上的壓力會評估為每30分鐘平衡一次。 或者，壓力可以視需要進行評估。 以下是步驟的流程：

1. 此壓力會在私人雲端中的所有節點上進行評估。
2. 已識別超過閾值且低於閾值的所有節點。
3. 系統會識別具有最高壓力的節點，以判斷平衡的優先順序。
4. <abbr title="虛擬機器">VM</abbr> 是即時移轉 (，不會從超過閾值的節點) 到低於下限臨界值的節點。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡深入探討](vm-load-balancing-deep-dive.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [Hyper-v 總覽](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
