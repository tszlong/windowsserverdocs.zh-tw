---
title: HNV 閘道中的效能微調軟體定義網路
description: HNV 閘道效能微調指導方針的軟體定義網路
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 217428b84a00b2e2231a15cb3878d0abcec1d9ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827319"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>HNV 閘道中的效能微調軟體定義網路

本主題提供硬體規格與設定伺服器是執行 HYPER-V 且裝載 Windows Server 閘道虛擬機器，除了 Windows Server 閘道虛擬機器 (Vm) 的組態參數的建議. 若要從 Windows Server 閘道 Vm 擷取最佳的效能，應該會遵守這些指導方針。
以下幾節涵蓋部署 Windows Server 閘道時的硬體和設定需求。
1. Hyper-V 硬體建議
2. Hyper-V 主機設定
3. Windows Server 閘道 VM 設定

## <a name="hyper-v-hardware-recommendations"></a>Hyper-V 硬體建議

以下是每一部執行 Windows Server 2016 和 HYPER-V 的伺服器的建議最低硬體組態。

| 伺服器元件               | 規格                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 中央處理器 (CPU)  | 非統一記憶體架構 (NUMA) 節點：2 <br> 如果有多個 Windows Server 閘道 Vm 主機上，為了達到最佳效能，每個閘道 VM 應該有一個 NUMA 節點的完整存取。 它應該是由主機實體介面卡的 NUMA 節點不同。 |
| 每個 NUMA 節點的核心            | 2                                                                                                                                                                                                                                                                               |
| Hyper-Threading                | 已停用。 超執行緒無法改善 Windows Server 閘道的效能。                                                                                                                                                                                           |
| 隨機存取記憶體 (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| 網路介面卡 (NIC) | 兩個 10 GB Nic，閘道效能將取決於列的速率。 如果列速率低於 10 gbps，閘道通道輸送量數字也往相同的因數。                                                                                          |

確保指派給 Windows Server 閘道 VM 的虛擬處理器數目不超過 NUMA 節點上的處理器數目。 例如，如果一個 NUMA 節點有 8 個核心，則虛擬處理器數目應小於或等於 8。 為了達到最佳效能，它應該為 8。 若要查明 NUMA 節點的數目以及每個 NUMA 節點的核心數目，請在每部 Hyper-V 主機執行下列 Windows PowerShell 指令碼：

```PowerShell
$nodes = [object[]] $(gwmi –Namespace root\virtualization\v2 -Class MSVM_NumaNode)
$cores = ($nodes | Measure-Object NumberOfProcessorCores -sum).Sum
$lps = ($nodes | Measure-Object NumberOfLogicalProcessors -sum).Sum


Write-Host "Number of NUMA Nodes: ", $nodes.count
Write-Host ("Total Number of Cores: ", $cores)
Write-Host ("Total Number of Logical Processors: ", $lps)
```

>[!Important]
> 在 NUMA 節點間配置虛擬處理器對 Windows Server 閘道效能可能有負面的影響。 執行多個 VM，每個 VM 都有來自一個 NUMA 節點的虛擬處理器，很可能會比所有虛擬處理器指派單一 VM 提供更好的彙總效能。

一個閘道 VM，具有八個虛擬處理器和至少 8 GB 的 RAM 時，建議選取閘道 Vm 的數目時每個 NUMA 節點有八個核心，每個 HYPER-V 主機上安裝。 在此情況下，一個 NUMA 節點致力於主機電腦。

## <a name="hyper-v-host-configuration"></a>HYPER-V 主機設定

以下是建議的設定，每個伺服器執行 Windows Server 2016 和 Hyper-v 角色和其工作負載是執行 Windows Server 閘道 Vm。 這些設定指示包含使用 Windows PowerShell 命令的範例。 這些範例含有在您的環境中執行命令時必須提供的實際值預留位置。 比方說，網路介面卡名稱預留位置為"NIC1 嗎？ 和"NIC2。 嗎？ 當您執行使用這些預留位置的命令時，請利用伺服器網路介面卡的實際名稱而不是使用此預留位置，否則此命令將會失敗。

>[!Note]
> 若要執行下列 Windows PowerShell 命令，您必須是 Administrators 群組的成員。

| 設定項目                          | Windows Powershell 設定                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 交換器內嵌小組                     | 當您建立了 vswitch 與多個網路介面卡時，它會自動啟用交換器內嵌小組的那些介面卡。 <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> 不支援使用 Windows Server 2016 中的 SDN LBFO 透過傳統小組。 交換器內嵌小組，可讓您可針對您的虛擬流量和 RDMA 流量的 Nic 的同一組。 這不是支援 NIC 小組根據 LBFO。                                                        |
| 實體 NIC 的插斷仲裁       | 使用預設設定。 若要檢查組態，您可以使用下列 Windows PowerShell 命令： ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| 實體 NIC 的接收緩衝區大小       | 您可以驗證實體 Nic 是否支援此參數的組態，執行命令```Get-NetAdapterAdvancedProperty```。 如果它們不支援這個參數，命令的輸出不包含屬性 「 接收緩衝區。 嗎？ 如果 NIC 支援此參數，您可以使用下列 Windows PowerShell 命令設定接收緩衝區大小： <br>```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Receive Buffers�? –DisplayValue 3000``` <br>                          |
| 實體 NIC 的傳送緩衝區大小          | 您可以驗證實體 Nic 是否支援此參數的組態，執行命令```Get-NetAdapterAdvancedProperty```。 如果 Nic 不支援這個參數，命令的輸出不包含屬性 「 傳送緩衝區。 嗎？ 如果 NIC 支援此參數，您可以使用下列 Windows PowerShell 命令設定傳送緩衝區大小： <br> ```Set-NetAdapterAdvancedProperty “NIC1�? –DisplayName “Transmit Buffers�? –DisplayValue 3000``` <br>                           |
| 實體 NIC 的接收端調整 (RSS) | 您可以確認您的實體 Nic 是否有執行 Windows PowerShell 命令 Get-netadapterrss 啟用 RSS。 您可以使用下列 Windows PowerShell 命令來啟用和設定 RSS，您的網路介面卡上： <br> ```Enable-NetAdapterRss “NIC1�?,�?NIC2�?```<br> ```Set-NetAdapterRss “NIC1�?,�?NIC2�? –NumberOfReceiveQueues 16 -MaxProcessors``` <br> 注意：如果已啟用 VMMQ 或 VMQ，RSS 並沒有在實體網路介面卡上啟用。 您可以在主機虛擬網路介面卡上啟用它 |
| VMMQ                                        | 若要啟用 VMMQ vm，請執行下列命令： <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> 注意：並非所有的網路介面卡支援 VMMQ。 目前支援 Chelsio T5，T6，Mellanox CX 3 和 CX-4，QLogic 45xxx 系列上                                                                                                                                                                                                                                      |
| NIC 小組的虛擬機器佇列 (VMQ) | 您可以啟用 VMQ 組小組，使用下列 Windows PowerShell 命令： <br>```Enable-NetAdapterVmq``` <br> 注意：硬體不支援 VMMQ 時，才應該啟用此。 如果支援，VMMQ 應該能夠進行更佳的效能。                                                                                                                                                                                                                                                               |
>[!Note]
> VMQ 和 vRSS 進入圖片，在 VM 上的負載很高，而且利用 CPU 最大值時，才。 則只會至少一個處理器核心最大值時。VMQ 和 vRSS 接著將會有幫助，以便處理負載分散到多個核心。 這並不適用於 IPsec 傳輸為 IPsec 流量會侷限於單一核心。

## <a name="windows-server-gateway-vm-configuration"></a>Windows Server 閘道 VM 設定

這兩個 HYPER-V 主機，您可以設定多個 Vm 設定為使用 Windows Server 閘道的閘道。 您可以使用虛擬交換器管理員建立繫結至 Hyper-V 主機上 NIC 小組的 Hyper-V 虛擬交換器。 請注意，為了達到最佳效能，您應該部署單一閘道 VM 的 HYPER-V 主機上。
以下是對每個 Windows Server 閘道 VM 建議的設定。

| 設定項目                 | Windows Powershell 設定                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記憶體                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| 虛擬網路介面卡數目 | 使用具有下列特定的 3 個 Nic:1 個用於管理作業系統，1 的外部可存取外部網路，1 個用於提供只有內部網路存取的內部使用的管理。                                                                                                                                                            |
| Receive Side Scaling (RSS)         | 您可以保留預設的管理 nic 的 RSS 設定 下列範例設定是針對具有 8 個虛擬處理器的 VM。 對於外部和內部 Nic，您可以啟用 RSS，將 baseprocnumber 設定為 0 並將 MaxRssProcessors 設定為 8，使用下列 Windows PowerShell 命令： <br> ```Set-NetAdapterRss “Internal�?,�?External�? –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| 傳送端緩衝區                   | 您可以保留預設的傳送端緩衝區設定為管理 nic。 內部和外部 Nic 的您可以設定傳送端緩衝區使用 32 MB 的 RAM 使用下列 Windows PowerShell 命令： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Send Buffer Size�? –DisplayValue “32MB�?``` <br>                                                       |
| 接收端緩衝區                | 您可以保留預設的管理 NIC 的接收端緩衝區設定 對於內部和外部 Nic，您可以設定接收端緩衝區使用 16 MB 的 RAM 使用下列 Windows PowerShell 命令： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Receive Buffer Size�? –DisplayValue “16MB�?``` <br>                                            |
| 轉送最佳化               | 您可以保留預設的管理 NIC 的轉送最佳化設定 對於內部和外部 Nic，您可以使用下列 Windows PowerShell 命令啟用轉送最佳化： <br> ```Set-NetAdapterAdvancedProperty “Internal�?,�?External�? –DisplayName “Forward Optimization�? –DisplayValue “1�?``` <br>                                                                      |
