---
ms.assetid: 5b5bab7a-727b-47ce-8efa-1d37a9639cba
title: 虛擬機器負載平衡深入探討
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: bhattacharyaz
manager: eldenc
ms.author: subhatt
ms.date: 09/19/2016
ms.openlocfilehash: f90802a8e77ade0b9f282730e7cb61c73a246018
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853419"
---
# <a name="virtual-machine-load-balancing-deep-dive"></a>虛擬機器負載平衡深入探討

> 適用於：Windows Server （半年通道），Windows Server 2016

Windows Server 2016 引進[虛擬機器負載平衡功能](vm-load-balancing-overview.md)最佳化容錯移轉叢集中節點的使用量。 本文件說明如何設定和控制<abbr title="虛擬機器">VM</abbr>負載平衡。 

## <a id="heuristics-for-balancing"></a>啟發學習法進行平衡
<abbr title="虛擬機器">VM</abbr>負載平衡虛擬機器會評估下列啟發學習法為基礎的節點的負載：
1. 目前**記憶體不足的壓力**:記憶體是 HYPER-V 主機上最常見的資源條件約束
2. <abbr title="中央處理器">CPU</abbr> **使用率**平均 5 分鐘時段的節點：降低變得過度認可的叢集中的節點

## <a id="controlling-aggressiveness-of-balancing"></a>控制此加強值的平衡
您可以使用設定此加強值平衡的記憶體和 CPU 啟發學習法為基礎的叢集一般屬性 'AutoBalancerLevel'。 若要控制此加強值，在 PowerShell 中執行下列命令：

```PowerShell
(Get-Cluster).AutoBalancerLevel = <value>
```

| AutoBalancerLevel | 加強 | 行為 |
|-------------------|----------------|----------|
| 1 (預設值) | 低 | 移動主機時超過 80%，載入 |
| 2 | 中等 | 移動超過 70%，載入主機時 |
| 3 | 高 | 平均節點與主應用程式時高於平均超過 5%的移動 | 

![設定此加強值的平衡的 PowerShell 的圖形](media/vm-load-balancing/detailed-VM-load-balancing-1.jpg)

## <a name="controlling-abbr-titlevirtual-machinevmabbr-load-balancing"></a>控制<abbr title="虛擬機器">VM</abbr>負載平衡
<abbr title="虛擬機器">VM</abbr>預設會啟用負載平衡，負載平衡發生時可以進行設定的叢集一般屬性 'AutoBalancerMode'。 若要控制當節點公平性平衡叢集：

### <a name="using-failover-cluster-manager"></a>使用容錯移轉叢集管理員：
1. 您的叢集名稱上按一下滑鼠右鍵，然後選取 [屬性] 選項  
    ![選取叢集透過容錯移轉叢集管理員中的屬性的圖形](media/vm-load-balancing/detailed-VM-load-balancing-2.jpg)

2.  選取 「 平衡器 」 窗格  
    ![選取 透過容錯移轉叢集管理員中的 平衡器 選項的圖形](media/vm-load-balancing/detailed-VM-load-balancing-3.jpg)

### <a name="using-powershell"></a>使用 PowerShell:
執行下列命令：
```powershell
(Get-Cluster).AutoBalancerMode = <value>
```

|AutoBalancerMode |行為| 
|:----------------:|:----------:|
|0| 已停用| 
|1| 在節點加入負載平衡| 
|2 （預設值）| 負載平衡節點聯結和每隔 30 分鐘 |

## <a name="abbr-titlevirtual-machinevmabbr-load-balancing-vs-system-center-virtual-machine-manager-dynamic-optimization"></a><abbr title="虛擬機器">VM</abbr>負載平衡的 vs。System Center Virtual Machine Manager 動態最佳化
節點公平性 」 功能，提供內建功能，針對部署不含 System Center Virtual Machine Manager (<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>)。 <abbr title="System Center Virtual Machine Manager">SCVMM</abbr>動態最佳化會是平衡的叢集中的虛擬機器負載的建議的機制<abbr title="System Center Virtual Machine Manager">SCVMM</abbr>部署。 <abbr title="System Center Virtual Machine Manager">SCVMM</abbr>會自動停用 Windows Server<abbr title="虛擬機器">VM</abbr>負載平衡時啟用動態最佳化。

## <a name="see-also"></a>另請參閱
* [虛擬機器負載平衡概觀](vm-load-balancing-overview.md)
* [容錯移轉叢集](failover-clustering-overview.md)
* [HYPER-V 概觀](../virtualization/hyper-v/Hyper-V-on-Windows-Server.md)
