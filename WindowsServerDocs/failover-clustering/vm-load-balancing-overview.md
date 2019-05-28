---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: 虛擬機器負載平衡概觀
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 125dd7421cc1876c07983016498a9689d8a507ac
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475993"
---
# <a name="virtual-machine-load-balancing-overview"></a>虛擬機器負載平衡概觀

> 適用於：Windows Server 2019，Windows Server 2016

私用雲端部署的重要考量是資本支出 (<abbr title="資本支出">CapEx</abbr>) 才能進入實際執行環境。 是將備援新增至私人雲端部署，以在生產環境中的尖峰流量期間避免不足容量很常見，但這會增加<abbr title="資本支出">CapEx</abbr>。 備援性需求由不對稱的私人雲端，其中某些節點會裝載更多虛擬機器 (<abbr title="虛擬機器">Vm</abbr>) 和其他使用量過低 （例如重新啟動全新的伺服器）。

<strong>簡短影片概觀</strong><br>（6 分）<br>
> [!VIDEO https://channel9.msdn.com/Blogs/windowsserver/Virtual-Machine-Load-Balancing-in-Windows-Server-2016/player]

## <a id="what-is-vm-load-balancing"></a>什麼是虛擬機器負載平衡？
<abbr title="虛擬機器">VM</abbr>負載平衡是 Windows Server 2019 和可讓您最佳化容錯移轉叢集中節點的使用量的 Windows Server 2016 中的內建功能。 它會識別過度認可的節點並重新分散<abbr title="虛擬機器">Vm</abbr>從這些節點，以在認可的節點。 這項功能的主要層面有些，如下所示：

* *它是零停機時間的解決方案*:<abbr title="虛擬機器">Vm</abbr>是即時移轉到閒置的節點。
* *與您現有的叢集環境的完美整合*:會遵守例如反親和性、 容錯網域和可能的擁有者的失敗原則。
* *啟發學習法進行平衡*:<abbr title="虛擬機器">VM</abbr>記憶體不足的壓力和節點的 CPU 使用率。
* *更精確地控制*:預設為啟用。 可以視需要或定期啟動。
* *加強閾值*:三個臨界值可根據您部署的特性。

## <a id="feature-in-action"></a>動作中的功能
### <a id="new-node-added"></a>新的節點新增至您的容錯移轉叢集
![新節點新增至您的容錯移轉叢集的圖形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

當您將新的容量新增至容錯移轉叢集，<abbr title="虛擬機器">VM</abbr>自動平衡負載平衡功能的容量，從現有的節點，以新加入的節點，以下列順序：

1. 容錯移轉叢集中現有的節點上評估的壓力。
2. 識別在超過臨界值的所有節點。
3. 具有最高的壓力節點識別判定優先順序的平衡。
4. <abbr title="虛擬機器">Vm</abbr>是即時移轉 （不需要停機） 從超過臨界值設定為新加入的節點，容錯移轉叢集中的節點。

### <a id="recurring-load-balancing"></a>重複執行的負載平衡
![重複執行的 VM 負載平衡的圖形](media/vm-load-balancing/overview-VM-load-balancing-2.png)

當設定為定期平衡，叢集節點上的壓力會評估平衡每隔 30 分鐘。 或者，不足的壓力，可以將隨評估。 以下是步驟的流程：

1. 私人雲端中的所有節點上評估的壓力。
2. 識別超過臨界值和臨界值以下的所有節點。
3. 具有最高的壓力節點識別判定優先順序的平衡。
4. <abbr title="虛擬機器">Vm</abbr>是即時移轉 （不需要停機） 從超過閾值，才有最小臨界值之下的節點的節點。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡深入探討](vm-load-balancing-deep-dive.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [HYPER-V 概觀](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
