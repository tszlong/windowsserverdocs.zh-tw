---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: 虛擬機器負載平衡深入探討
ms.topic: article
manager: eldenc
ms.author: johnmar
author: JasonGerend
ms.date: 09/19/2016
ms.openlocfilehash: 7fc9b449b11b5faf05ac279628f093053e292e8c
ms.sourcegitcommit: 7a8a608df059b4278a974c52ed7b865421a83aa6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/08/2020
ms.locfileid: "91833317"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>虛擬機器負載平衡深入探討

> 適用於：Windows Server 2019、Windows Server 2016

[虛擬機器負載平衡功能](vm-load-balancing-overview.md)可將容錯移轉叢集中節點的使用率優化。 本檔說明如何設定和控制 VM 負載平衡。

## <a name="heuristics-for-balancing"></a><a id="heuristics-for-balancing"></a>平衡的啟發學習法
虛擬機器負載平衡會根據下列啟發學習法來評估節點的負載：
1. 目前的 **記憶體壓力**：記憶體是 hyper-v 主機上最常見的資源限制式
2. 節點 CPU **使用率** 平均超過5分鐘時間範圍：減少叢集中的節點變成過度認可

## <a name="controlling-the-aggressiveness-of-balancing"></a><a id="controlling-aggressiveness-of-balancing"></a>控制平衡的增強
您可以使用叢集通用屬性 ' AutoBalancerLevel ' 來設定以記憶體和 CPU 啟發學習法為基礎的平衡。 若要控制增強功能，請在 PowerShell 中執行下列步驟：

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 侵略 性 | 行為 |
|-------------------|----------------|----------|
| 1 (預設值) | 低度 | 當主機超過80% 載入時移動 |
| 2 | 中 | 當主機超過70% 載入時移動 |
| 3 | 高 | 當主機的平均超過5% 時的平均節點和移動 |

![設定增強平衡的 PowerShell 圖形](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>控制 VM 負載平衡
預設會啟用 VM 負載平衡，而且當負載平衡發生時，可以透過叢集的一般屬性 ' AutoBalancerMode ' 來設定。 若要控制節點公平平衡叢集的時間：

### <a name="using-failover-cluster-manager"></a>使用容錯移轉叢集管理員：
1. 以滑鼠右鍵按一下您的叢集名稱，然後選取 [屬性] 選項  ![ 圖形，以透過容錯移轉叢集管理員選取叢集的屬性](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  選取 ![ 透過容錯移轉叢集管理員選取平衡器選項的 [平衡器] 窗格圖形](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell：
執行下列命令：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行為|
|:----------------:|:----------:|
|0| 已停用|
|1| 節點聯結的負載平衡|
|2 (預設值)| 節點聯結和每隔30分鐘的負載平衡 |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>VM 負載平衡與 System Center Virtual Machine Manager 動態優化
節點公平功能提供現成可用的功能，其以不 System Center Virtual Machine Manager (SCVMM) 的部署為目標。 SCVMM 動態優化是針對 SCVMM 部署平衡叢集中虛擬機器負載的建議機制。 當動態優化啟用時，SCVMM 會自動停用 Windows Server VM 負載平衡。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡總覽](vm-load-balancing-overview.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [Hyper-v 總覽](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
