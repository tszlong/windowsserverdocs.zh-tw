---
title: 虛擬機器的效能歷程記錄
ms.author: cosdar
manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.openlocfilehash: 418ab095f5f0af35f3aa176614ad73f48d727a35
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85474685"
---
# <a name="performance-history-for-virtual-machines"></a>虛擬機器的效能歷程記錄

> 適用於：Windows Server 2019

[儲存空間直接存取效能歷程記錄](performance-history.md)的子主題會詳細說明針對虛擬機器（VM）所收集的效能歷程記錄。 效能歷程記錄適用于每個執行中的叢集 VM。

   > [!NOTE]
   > 針對新建立或重新命名的 Vm，可能需要幾分鐘的時間才能開始收集。

## <a name="series-names-and-units"></a>數列名稱和單位

系統會針對每個符合資格的 VM 收集這些系列：

| 數列                            | 單位             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | percent          |
| `vm.memory.assigned`              | 位元組            |
| `vm.memory.available`             | 位元組            |
| `vm.memory.maximum`               | 位元組            |
| `vm.memory.minimum`               | 位元組            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | 位元組            |
| `vm.memory.total`                 | 位元組            |
| `vmnetworkadapter.bandwidth.inbound`  | 每秒位數 |
| `vmnetworkadapter.bandwidth.outbound` | 每秒位數 |
| `vmnetworkadapter.bandwidth.total`    | 每秒位數 |

此外，所有虛擬硬碟（VHD）系列（例如 `vhd.iops.total` ）都會針對連接至 VM 的每個 VHD 進行匯總。

## <a name="how-to-interpret"></a>如何解讀


| 數列                            | 描述                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | 虛擬機器正在使用其主機伺服器處理器的百分比。                                   |
| `vm.memory.assigned`              | 指派給虛擬機器的記憶體數量。                                                      |
| `vm.memory.available`             | 已指派數量的可用記憶體數量。                                       |
| `vm.memory.maximum`               | 如果使用動態記憶體，這就是可能指派給虛擬機器的記憶體數量上限。 |
| `vm.memory.minimum`               | 如果使用動態記憶體，這就是可能指派給虛擬機器的最小記憶體數量。 |
| `vm.memory.pressure`              | 虛擬機器要求的記憶體與配置給虛擬機器的記憶體比率。            |
| `vm.memory.startup`               | 虛擬機器啟動所需的記憶體數量。                                            |
| `vm.memory.total`                 | 記憶體總計。 |
| `vmnetworkadapter.bandwidth.inbound`  | 虛擬機器在其所有虛擬網路介面卡上接收的資料速率。                        |
| `vmnetworkadapter.bandwidth.outbound` | 虛擬機器在其所有虛擬網路介面卡上傳送的資料速率。                            |
| `vmnetworkadapter.bandwidth.total`    | 虛擬機器在其所有虛擬網路介面卡上所接收或傳送的資料總速率。          |

   > [!NOTE]
   > 計數器是以整個間隔來測量，而不是取樣。 例如，如果 VM 閒置了9秒，但尖峰在10秒內使用50% 的主機 CPU，則 `vm.cpu.usage` 在此10秒的間隔期間，平均會將其記錄為5%。 這可確保其效能歷程會捕捉所有活動，而且健全于雜訊。

## <a name="usage-in-powershell"></a>PowerShell 中的使用方式

使用[取得 VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) Cmdlet：

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > 「取得 VM」 Cmdlet 只會傳回本機（或指定）伺服器上的虛擬機器，而不是在整個叢集中。

## <a name="additional-references"></a>其他參考

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
