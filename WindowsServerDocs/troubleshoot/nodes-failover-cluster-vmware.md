---
title: 從作用中容錯移轉叢集成員資格移除節點
description: 本文說明尋找從主動容錯移轉叢集成員資格中移除之節點的問題。
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.topic: troubleshooting
ms.openlocfilehash: f367575cf828e23c6f1624176d359b4590cf4728
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97948174"
---
# <a name="nodes-being-removed-from-failover-cluster-membership-on-vmware-esx"></a>從 VMWare ESX 上的容錯移轉叢集成員資格中移除的節點

本文說明在 VMWare ESX 上裝載節點時，尋找從主動容錯移轉叢集成員資格移除的節點的問題。

## <a name="symptom"></a>徵狀

當問題發生時，您會在事件檢視器的系統事件記錄檔中看到：

![事件1135](media/nodes-failover-cluster-vmware/1135.png)

## <a name="resolution"></a>解決方案

VMXNET3 介面卡卸載輸入網路封包是一個特定問題，因為輸入緩衝區的設定太低，無法處理大量的流量。 我們可以使用效能監視器來查看「已捨棄的網路 Interface\Packets」計數器，以輕鬆找出這是否有問題。

![新增計數器](media/nodes-failover-cluster-vmware/0527.png)

一旦您新增此計數器，請查看平均值、最小值和最大值，如果它們是任何大於零的值，則需要為介面卡調整接收緩衝區。 此問題記載于 VMWare 的知識庫中：在 [ESXi 5.x/4.x 的 VMXNET3 vNIC 上，客體作業系統層級的大型封包遺失](https://kb.vmware.com/s/article/2039495)。
