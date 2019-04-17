---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: "一樣負載平衡深度探討"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: 6ee092a2e51e2181a2203bc263377c4a8ec60246
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>一樣負載平衡深度探討

> 適用於：Windows Server（以每年次通道）、Windows Server 2016

Windows Server 2016 引入了[一樣負載平衡功能](vm-load-balancing-overview.md)最佳化的節點容錯移轉叢集使用量。 本文件告訴您如何設定及控制<abbr title="一樣">VM</abbr>負載平衡。 

## <a id="heuristics-for-balancing"></a>適用於平衡 heuristics
<abbr title="一樣">VM</abbr>一樣負載平衡評估節點負載平衡下列 heuristics:
1. 目前**記憶體不足壓力時**： 記憶體是最常見的資源限制 HYPER-V 主機上
2. <abbr title="中央處理">CPU</abbr> **使用**節點平均經過 5 分鐘視窗的： 降低叢集成為覆致力節點

## <a id="controlling-aggressiveness-of-balancing"></a>控制侵略平衡
您可以使用設定的平衡根據的記憶體和 CPU heuristics 侵略叢集常見屬性 'AutoBalancerLevel'。 若要控制侵略執行中的 PowerShell:

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 侵略 | 行為 |
|-------------------|----------------|----------|
| 1 （預設值） | 低 | 載入更多個 80%主機時移動 |
| 2 | 媒體 | 載入更多個 70%主機時移動 |
| 3 | 高 | 載入更多個 60%主機時移動 | 

![設定的平衡侵略 PowerShell 的圖形](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>控制<abbr title="一樣">VM</abbr>負載平衡
<abbr title="一樣">VM</abbr>負載平衡預設會讓和負載平衡發生時可以叢集常見屬性 'AutoBalancerMode' 來設定。 若要控制時節點公平性餘額叢集︰

### <a name="using-failover-cluster-manager"></a>使用容錯移轉叢集管理員：
1. 您叢集名稱上按一下滑鼠右鍵，然後選取 [「 屬性] 選項  
    ![選取 [適用於叢集透過容錯移轉叢集管理員屬性的圖形](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  選取 [平衡] 窗格  
    ![選取 [平衡] 選項透過容錯移轉叢集管理員的圖形](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell:
執行下列動作：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行為| 
|:----------------:|:----------:|
|0| 停用| 
|1| 在節點加入負載平衡| 
|2 （預設值）| 負載平衡在節點加入，每隔 30 分鐘 |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="一樣">VM</abbr>負載平衡與 System Center 一樣管理員動態最佳化
節點公平性功能，提供方塊中的功能，這衝部署不 System Center 一樣 Manager (<abbr title="系統中心一樣管理員">SCVMM</abbr>)。 <abbr title="System Center 一樣管理員">SCVMM</abbr>動態最佳化是建議的機制平衡一樣載入您叢集適用於<abbr title="系統中心一樣管理員">SCVMM</abbr>部署。 <abbr title="System Center 一樣管理員">SCVMM</abbr>自動停用 Windows Server<abbr title="一樣">VM</abbr>負載平衡時支援動態最佳化。

## <a name="see-also"></a>也了
* [一樣負載平衡概觀](vm-load-balancing-overview.md)
* [容錯](failover-clustering-overview.md)
* [HYPER-V 概觀](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
