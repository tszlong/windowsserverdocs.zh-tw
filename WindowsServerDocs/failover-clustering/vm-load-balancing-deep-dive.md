---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: 虛擬機器負載平衡深入探討
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 972e86d5f49f92d090eed1d4130544d0269c1309
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392227"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>虛擬機器負載平衡深入探討

> 適用於：Windows Server 2019、Windows Server 2016

[虛擬機器負載平衡功能](vm-load-balancing-overview.md)可優化容錯移轉叢集中節點的使用率。 本檔說明如何設定和控制 VM 負載平衡。 

## <a id="heuristics-for-balancing"></a>適用于平衡的啟發學習法
虛擬機器負載平衡會根據下列啟發學習法來評估節點的負載：
1. 目前的**記憶體壓力**：記憶體是 Hyper-v 主機上最常見的資源限制
2. 平均超過5分鐘時間範圍之節點的 CPU**使用率**：減少叢集中的節點變得過度認可

## <a id="controlling-aggressiveness-of-balancing"></a>控制平衡的加強
藉由叢集通用屬性 ' AutoBalancerLevel '，可以使用來設定以記憶體和 CPU 啟發學習法為基礎的平衡。 若要控制增強功能，請在 PowerShell 中執行下列動作：

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 進取 | 行為 |
|-------------------|----------------|----------|
| 1 (預設值) | 低 | 當主機已載入 80% 以上時移動 |
| 2 | 中等 | 當主機已載入 70% 以上時移動 |
| 3 | 高 | 當主機大於平均 5% 時的平均節點和移動 | 

![設定增強平衡的 PowerShell 圖形](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-vm-load-balancing"></a>控制 VM 負載平衡
VM 負載平衡預設為啟用，而且當負載平衡發生時，可以由叢集通用屬性 ' AutoBalancerMode ' 來設定。 若要控制節點公平平衡叢集的時機：

### <a name="using-failover-cluster-manager"></a>使用容錯移轉叢集管理員：
1. 以滑鼠右鍵按一下您的叢集名稱，然後選取 [屬性] 選項  
    ![透過容錯移轉叢集管理員選取叢集屬性的圖形](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  選取 [平衡器] 窗格  
    ![透過容錯移轉叢集管理員選取平衡器選項的圖形](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell：
執行下列動作：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行為| 
|:----------------:|:----------:|
|0| 已停用| 
|1| 節點聯結的負載平衡| 
|2（預設值）| 節點聯結和每30分鐘的負載平衡 |

## <a name="vm-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a>VM 負載平衡與System Center Virtual Machine Manager 動態優化
節點公平功能會提供現成可用的功能，其目標為不含 System Center Virtual Machine Manager （SCVMM）的部署。 SCVMM 動態優化是針對 SCVMM 部署在叢集中平衡虛擬機器負載的建議機制。 啟用動態優化時，SCVMM 會自動停用 Windows Server VM 負載平衡。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡總覽](vm-load-balancing-overview.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [Hyper-v 總覽](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
