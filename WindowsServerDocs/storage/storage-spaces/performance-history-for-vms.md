---
title: 虛擬機器的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: Storage Spaces Direct
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: d31e266130b3b082372f7af4024e6089cb347d74
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2018
ms.locfileid: "4239215"
---
# 虛擬機器的效能歷程記錄

> 適用於： Windows Server Insider Preview

[儲存空間直接存取的效能記錄](performance-history.md)的此子主題說明中的效能記錄收集到的虛擬機器 (VM) 的詳細資料。 效能歷程記錄只適用於每個執行中，叢集化的 VM。

   > [!NOTE]
   > 可能需要幾分鐘的時間來開始新建立或重新命名 vm 的集合。

## 系列名稱與單位

這些系列會針對每個符合資格的 VM 收集：

| 系列                            | 單位             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | 百分比          |
| `vm.memory.assigned`              |  位元組            |
| `vm.memory.available`             |  位元組            |
| `vm.memory.maximum`               |  位元組            |
| `vm.memory.minimum`               |  位元組            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               |  位元組            |
| `vm.memory.total`                 |  位元組            |
| `vmnetworkadapter.bandwidth.inbound`  | 每秒的位元 |
| `vmnetworkadapter.bandwidth.outbound` | 每秒的位元 |
| `vmnetworkadapter.bandwidth.total`    | 每秒的位元 |

此外，所有的虛擬硬碟 (VHD) 系列，例如`vhd.iops.total`，針對每個 VHD 附加到 VM 彙總。

## 如何解譯


| 系列                            | 描述                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | 百分比虛擬機器正在使用的其主機伺服器的處理器。                                   |
| `vm.memory.assigned`              | 指派給虛擬機器的記憶體數量。                                                      |
| `vm.memory.available`             | 仍然可以使用，受指派的所需的記憶體數量。                                       |
| `vm.memory.maximum`               | 如果使用動態記憶體，這是記憶體的最大的可能會指派給虛擬機器數量。 |
| `vm.memory.minimum`               | 如果使用動態記憶體，這是記憶體的最小的可能會指派給虛擬機器數量。 |
| `vm.memory.pressure`              | 記憶體需求的虛擬機器記憶體配置給虛擬機器上的比例。            |
| `vm.memory.startup`               | 啟動虛擬機器所需的記憶體數量。                                            |
| `vm.memory.total`                 | 總記憶體。 |
| `vmnetworkadapter.bandwidth.inbound`  | 虛擬機器所接收到其所有虛擬網路介面卡的資料速率。                        |
| `vmnetworkadapter.bandwidth.outbound` | 虛擬機器傳送到其所有虛擬網路介面卡的資料速率。                            |
| `vmnetworkadapter.bandwidth.total`    | 總資料速率收到或虛擬機器傳送到其所有虛擬網路介面卡。          |

   > [!NOTE]
   > 計數器是透過整個間隔，不取樣測量。 例如，如果 VM 是閒置 9 秒，但在 10 秒，使用 50%的主機 CPU 尖峰其`vm.cpu.usage`會記錄為 5%平均 10 秒的時間間隔內。 這樣可確保其效能歷程記錄會擷取所有活動，並以雜訊強固。

## 在 PowerShell 中的使用方式

使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > GET-VM cmdlet 只會傳回在本機 （或指定） 伺服器上，虛擬機器不在叢集間。

## 請參閱

- [儲存空間直接存取的效能歷程記錄](performance-history.md)
