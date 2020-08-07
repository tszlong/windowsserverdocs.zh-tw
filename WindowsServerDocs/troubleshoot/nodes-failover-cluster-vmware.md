---
title: 從作用中容錯移轉叢集成員資格移除節點
description: 本文說明從作用中的容錯移轉叢集成員資格中移除節點的問題。
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: da46b39f853476676a06bcaaa20338dd1a178586
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935568"
---
# <a name="nodes-being-removed-from-failover-cluster-membership-on-vmware-esx"></a>從 VMWare ESX 上的容錯移轉叢集成員資格移除節點

本文說明當節點裝載于 VMWare ESX 時，尋找從作用中的容錯移轉叢集成員資格中移除的節點的問題。

## <a name="symptom"></a>徵狀

當問題發生時，您會在事件檢視器的系統事件記錄檔中看到：

![事件1135](media/nodes-failover-cluster-vmware/1135.png)

## <a name="resolution"></a>解決方法

其中一個特定問題是因為輸入緩衝區設定得太低，而無法處理大量的流量，所以 VMXNET3 介面卡會捨棄輸入網路封包。 我們可以使用效能監視器來查看「網路 Interface\Packets 收到的已捨棄」計數器，輕鬆找出是否有問題。

![新增計數器](media/nodes-failover-cluster-vmware/0527.png)

當您新增此計數器之後，請查看平均值、最小值和最大數目，如果它們是任何高於零的值，則必須為介面卡調整接收緩衝區。 此問題記載于 VMWare 的知識庫中： [ESXi 5.x 中的 VMXNET3 vNIC 上的來賓 OS 層級發生大型封包遺失](https://kb.vmware.com/s/article/2039495)。
