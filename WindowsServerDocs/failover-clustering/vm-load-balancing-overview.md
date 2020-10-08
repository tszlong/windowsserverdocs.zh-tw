---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: 虛擬機器負載平衡概觀
ms.topic: article
manager: eldenc
ms.author: johnmar
author: JasonGerend
ms.date: 09/19/2016
ms.openlocfilehash: 868f5c0646b3842f605447d1fe8fdbe74593c2a6
ms.sourcegitcommit: 7a8a608df059b4278a974c52ed7b865421a83aa6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2020
ms.locfileid: "91833304"
---
# <a name="virtual-machine-load-balancing-overview"></a>虛擬機器負載平衡概觀

> 適用於：Windows Server 2019、Windows Server 2016

私用雲端部署的重要考慮是資本支出 (<abbr title="資本支出">資本</abbr>需要 ) 才能進入生產環境。 將冗余新增至私用雲端部署，以避免在生產環境中尖峰流量的容量不足的情況下，這種情況也很常見。 <abbr title="資本支出">資本</abbr>. 冗余的需求是由某些節點裝載更多虛擬機器的不平衡私用雲端所驅動 (<abbr title="虛擬機器">VM</abbr>) 和其他人的 (，例如全新重新開機的伺服器) 。

<strong>快速影片概觀</strong><br> (6 分鐘) <br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a name="what-is-virtual-machine-load-balancing"></a><a id="what-is-vm-load-balancing"></a>什麼是虛擬機器負載平衡？
<abbr title="虛擬機器">VM</abbr> 負載平衡是 Windows Server 2019 和 Windows Server 2016 中的現成功能，可讓您將容錯移轉叢集中節點的使用率優化。 它會識別過度認可的節點，然後重新分佈 <abbr title="虛擬機器">VM</abbr> 從這些節點到受認可的節點。 這項功能的一些重要層面如下：

* *這是零停機解決方案*： <abbr title="虛擬機器">VM</abbr> 會即時移轉至閒置節點。
* *與您現有的叢集環境緊密整合*：會接受失敗的原則，例如反親和性、容錯網域和可能的擁有者。
* *平衡的啟發學習法*： <abbr title="虛擬機器">VM</abbr> 節點的記憶體壓力和 CPU 使用率。
* *細微控制*：預設為啟用。 可以視需要或週期性間隔來啟用。
* 增強*閾值*：根據您部署的特性，有三個可用的閾值。

## <a name="the-feature-in-action"></a><a id="feature-in-action"></a>作用中的功能
### <a name="a-new-node-is-added-to-your-failover-cluster"></a><a id="new-node-added"></a>新節點新增至您的容錯移轉叢集
![新增至容錯移轉叢集的新節點圖形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

當您將新的容量新增至容錯移轉叢集時， <abbr title="虛擬機器">VM</abbr> 負載平衡功能會依下列順序自動將現有節點的容量平衡至新加入的節點：

1. 這項壓力是在容錯移轉叢集中的現有節點上進行評估。
2. 系統會識別超過閾值的所有節點。
3. 系統會識別具有最高壓力的節點，以判斷平衡的優先順序。
4. <abbr title="虛擬機器">VM</abbr> 是即時移轉的 (，) 從節點超過閾值的節點，到容錯移轉叢集中新增的節點為止。

### <a name="recurring-load-balancing"></a><a id="recurring-load-balancing"></a>週期性負載平衡
![週期性 VM 負載平衡的圖形](media/vm-load-balancing/overview-VM-load-balancing-2.png)

設定為定期平衡時，會評估叢集節點的壓力，以平衡每隔30分鐘的時間。 或者，您也可以視需要評估壓力。 以下是步驟的流程：

1. 在私用雲端中的所有節點上都會評估壓力。
2. 系統會識別超過閾值的所有節點，以及低於閾值的所有節點。
3. 系統會識別具有最高壓力的節點，以判斷平衡的優先順序。
4. <abbr title="虛擬機器">VM</abbr> 是否為即時移轉的 (，而不是從超過閾值的節點) 到最小臨界值下的節點。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡深入探討](vm-load-balancing-deep-dive.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [Hyper-v 總覽](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
