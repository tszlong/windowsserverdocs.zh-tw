---
title: 虛擬機器的效能歷程記錄
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 09/07/2018
Keywords: 儲存空間直接存取
ms.localizationpriority: medium
ms.openlocfilehash: f8072ab5fc853248f2eedd26019956ec864a891d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890869"
---
# <a name="performance-history-for-virtual-machines"></a>虛擬機器的效能歷程記錄

> 適用於：Windows Server Insider Preview

子主題[效能歷程記錄的儲存空間直接存取](performance-history.md)詳細說明適用於虛擬機器 (VM) 所收集的效能歷程記錄。 效能歷程記錄可供每個執行，叢集的 VM。

   > [!NOTE]
   > 可能需要幾分鐘的時間開始針對新建立或重新命名的 Vm 的集合。

## <a name="series-names-and-units"></a>數列名稱與單位

符合資格的每個 VM 會收集這些系列：

| 系列                            | 單位             |
|-----------------------------------|------------------|
| `vm.cpu.usage`                    | 百分比          |
| `vm.memory.assigned`              | 位元組            |
| `vm.memory.available`             | 位元組            |
| `vm.memory.maximum`               | 位元組            |
| `vm.memory.minimum`               | 位元組            |
| `vm.memory.pressure`              | -                |
| `vm.memory.startup`               | 位元組            |
| `vm.memory.total`                 | 位元組            |
| `vmnetworkadapter.bandwidth.inbound`  | 每秒位元 |
| `vmnetworkadapter.bandwidth.outbound` | 每秒位元 |
| `vmnetworkadapter.bandwidth.total`    | 每秒位元 |

此外，所有的虛擬硬碟 (VHD) 序列，例如`vhd.iops.total`，會彙總的每一個 VHD 連結至 VM。

## <a name="how-to-interpret"></a>如何解譯


| 系列                            | 描述                                                                                                  |
|-----------------------------------|--------------------------------------------------------------------------------------------------------------|
| `vm.cpu.usage`                    | 使用百分比虛擬機器與其主機伺服器的處理器。                                   |
| `vm.memory.assigned`              | 指派給虛擬機器的記憶體數量。                                                      |
| `vm.memory.available`             | 剩餘可用的指派數量的記憶體數量。                                       |
| `vm.memory.maximum`               | 如果使用動態記憶體，這是記憶體的可能會指派給虛擬機器最大數量。 |
| `vm.memory.minimum`               | 如果使用動態記憶體，這會是記憶體的最小可能會指派給虛擬機器數量。 |
| `vm.memory.pressure`              | 透過記憶體配置給虛擬機器需要虛擬機器的記憶體的比率。            |
| `vm.memory.startup`               | 若要啟動虛擬機器所需的記憶體數量。                                            |
| `vm.memory.total`                 | 記憶體總數。 |
| `vmnetworkadapter.bandwidth.inbound`  | 跨所有其虛擬網路介面卡接收到的虛擬機器資料的速率。                        |
| `vmnetworkadapter.bandwidth.outbound` | 跨所有其虛擬網路介面卡傳送的虛擬機器資料的速率。                            |
| `vmnetworkadapter.bandwidth.total`    | 總資料的速率所接收或傳送的虛擬機器跨越所有其虛擬網路介面卡。          |

   > [!NOTE]
   > 計數器會測量整個間隔，不取樣。 例如，如果 VM 是 9 秒，但在 10 秒，使用 50%的主機 CPU 尖峰的閒置時間其`vm.cpu.usage`會記錄為 5%平均 10 秒間隔期間。 這可確保其效能歷程記錄會擷取所有的活動，並是強固的雜訊。

## <a name="usage-in-powershell"></a>在 PowerShell 中的使用方式

使用[GET-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm) cmdlet:

```PowerShell
Get-VM <Name> | Get-ClusterPerf
```

   > [!NOTE]
   > GET-VM cmdlet 只會傳回在本機的 （或指定的） 伺服器上，虛擬機器不會分散到叢集中。

## <a name="see-also"></a>另請參閱

- [效能歷程記錄的儲存空間直接存取](performance-history.md)
