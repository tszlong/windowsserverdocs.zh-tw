---
ms.assetid: f0d4cecc-5a03-448c-bef9-86c4730b4eb0
title: "一樣負載平衡概觀"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 0a106db25d476088898b914481e6041f20ce2e9e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-overview"></a>一樣負載平衡概觀

> 適用於：Windows Server（以每年次通道）、Windows Server 2016

按鍵考量的私人雲端部署，是大寫費用 (<abbr title="大寫費用">CapEx</abbr>) 才能進入執行。 常見的私人雲端部署，以避免置中容量實際澳地區的山峰資料傳輸期間新增冗餘但這增加<abbr title="大寫費用">CapEx</abbr>。 需要冗餘的會受到裝載某些節點虛擬更多的電腦不平衡私人雲朵 (<abbr title="虛擬電腦">Vm</abbr>) 與其他人的充分 （例如剛 rebooted 伺服器）。

## <a id="what-is-vm-load-balancing"></a>何謂一樣負載平衡？
<abbr title="一樣">VM</abbr>負載平衡是，可讓您最佳化的節點容錯移轉叢集使用率 Windows Server 2016 的方塊中新功能。 它辨識覆致力的節點並重新散發<abbr title="虛擬電腦">Vm</abbr>在認可節點那些節點。 部分依照此功能的部分如下：

* *0-中斷方案是*:<abbr title="虛擬電腦">Vm</abbr>的動態移轉到閒置節點。
* *與現有叢集環境順暢整合*： 失敗原則，例如反相關性網域錯誤，可能的擁有者會生效。
* *Heuristics 為 [平衡]*:<abbr title="一樣">VM</abbr>記憶體不足壓力時和 CPU 使用率節點。
* *細微控制*： 預設的支援。 隨選或定期可以啟動。
* *侵略閾值*： 三個閾值可根據您的部署的特性。

## <a id="feature-in-action"></a>重要訊息中的功能
### <a id="new-node-added"></a>新增了一個新的節點容錯移轉叢集
![新的節點新增至您容錯移轉叢集的圖形](media/vm-load-balancing/overview-VM-load-balancing-1.png)

當您新增新的容量容錯移轉叢集，<abbr title="一樣">VM</abbr>負載平衡功能自動餘額容量，從現有的節點，以 [新增] 節點以下列順序：

1. 在現有的節點中容錯移轉叢集評估壓力。
2. 都會所有節點超過臨界值。
3. 若要判斷平衡的優先順序都會節點以最高壓力。
4. <abbr title="虛擬電腦">Vm</abbr>有 Live 移轉 （無時間） 節點超過容錯移轉叢集中新加入節點臨界值。

### <a id="recurring-load-balancing"></a>週期性負載平衡
![圖形週期性 VM 負載平衡](media/vm-load-balancing/overview-VM-load-balancing-2.png)

設定為 [平衡] 定期，叢集節點上的壓力評估平衡每 30 分鐘的時間。 或者，壓力可評估的視。 以下是流程步驟：

1. 壓力評估私人雲端中的所有節點上。
2. 都會所有節點超過閾值來改善，以及低於臨界值。
3. 若要判斷平衡的優先順序都會節點以最高壓力。
4. <abbr title="虛擬電腦">Vm</abbr>有 Live 移轉 （無時間） 節點超過至最小臨界值下節點臨界值。

## <a name="see-also"></a>也了
* [一樣負載平衡深度探討](vm-load-balancing-deep-dive.md)
* [容錯](failover-clustering-overview.md)
* [HYPER-V 概觀](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
