---
title: 軟體定義網路中的 HNV 閘道效能微調
description: HNV 軟體定義網路的閘道效能微調指導方針
ms.topic: article
ms.author: grcusanz
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: de6a83d883efbdf592c7a21524c36b9d9086b7cc
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077965"
---
# <a name="hnv-gateway-performance-tuning-in-software-defined-networks"></a>軟體定義網路中的 HNV 閘道效能微調

除了 (Vm) 的 Windows Server 閘道虛擬機器的設定參數之外，本主題還提供執行 Hyper-v 和裝載 Windows Server 閘道虛擬機器之伺服器的硬體規格和設定建議。 若要從 Windows Server 閘道 Vm 解壓縮最佳效能，您應該遵循這些指導方針。
以下幾節涵蓋部署 Windows Server 閘道時的硬體和設定需求。
1. Hyper-V 硬體建議
2. Hyper-V 主機設定
3. Windows Server 閘道 VM 設定

## <a name="hyper-v-hardware-recommendations"></a>Hyper-V 硬體建議

以下是每個執行 Windows Server 2016 和 Hyper-v 之伺服器的建議最低硬體設定。

| 伺服器元件               | 規格                                                                                                                                                                                                                                                                   |
|--------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 中央處理器 (CPU)  |  (NUMA) 節點的非統一記憶體架構：2 <br> 如果主機上有多個 Windows Server 閘道 Vm，為了達到最佳效能，每個閘道 VM 都應該具有一個 NUMA 節點的完整存取權。 它應該與主機實體介面卡所使用的 NUMA 節點不同。 |
| 每個 NUMA 節點的核心            | 2                                                                                                                                                                                                                                                                               |
| 超執行緒                | 停用。 超執行緒無法改善 Windows Server 閘道的效能。                                                                                                                                                                                           |
| 隨機存取記憶體 (RAM)     | 48 GB                                                                                                                                                                                                                                                                           |
| 網路介面卡 (NIC) | 2 10 GB 的 Nic，閘道效能將取決於線路速率。 如果行速率小於10Gbps，閘道通道的輸送量數位也會依相同因素減少。                                                                                          |

確保指派給 Windows Server 閘道 VM 的虛擬處理器數目不超過 NUMA 節點上的處理器數目。 例如，如果一個 NUMA 節點有 8 個核心，則虛擬處理器數目應小於或等於 8。 為了達到最佳效能，它應該是8。 若要查明 NUMA 節點的數目以及每個 NUMA 節點的核心數目，請在每部 Hyper-V 主機執行下列 Windows PowerShell 指令碼：

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

當每個 NUMA 節點有八個核心時，當您選取要在每部 Hyper-v 主機上安裝的閘道 vm 數目時，建議使用一個具有8個虛擬處理器和至少 8GB RAM 的閘道 VM。 在此情況下，一個 NUMA 節點專用於主機電腦。

## <a name="hyper-v-host-configuration"></a>Hyper-v 主機設定

以下是執行 Windows Server 2016 和 Hyper-v 之每部伺服器的建議設定，而其工作負載是要執行 Windows Server 閘道 Vm。 這些設定指示包含使用 Windows PowerShell 命令的範例。 這些範例含有在您的環境中執行命令時必須提供的實際值預留位置。 例如，網路介面卡名稱預留位置為 "NIC1" 和 "NIC2"。 當您執行使用這些預留位置的命令時，請利用伺服器網路介面卡的實際名稱而不是使用此預留位置，否則此命令將會失敗。

>[!Note]
> 若要執行下列 Windows PowerShell 命令，您必須是 Administrators 群組的成員。

| 設定項目                          | Windows Powershell 設定                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 交換器內嵌小組                     | 當您建立具有多張網路介面卡的 vswitch 時，它會自動為這些介面卡啟用交換器內嵌小組。 <br> ```New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"``` <br> Windows Server 2016 中的 SDN 不支援透過 LBFO 的傳統小組。 交換器 Embedded 組合可讓您針對虛擬流量和 RDMA 流量使用相同的 Nic 組。 以 LBFO 為基礎的 NIC 小組不支援此作業。                                                        |
| 實體 NIC 的插斷仲裁       | 使用預設設定。 若要檢查設定，您可以使用下列 Windows PowerShell 命令： ```Get-NetAdapterAdvancedProperty```                                                                                                                                                                                                                                                                                                                                                                    |
| 實體 NIC 的接收緩衝區大小       | 您可以藉由執行命令來確認實體 Nic 是否支援此參數的設定  ```Get-NetAdapterAdvancedProperty``` 。 如果它們不支援此參數，則命令的輸出不會包含「接收緩衝區」屬性。 如果 NIC 支援此參數，您可以使用下列 Windows PowerShell 命令設定接收緩衝區大小： <br>```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Receive Buffers" –DisplayValue 3000``` <br>                          |
| 實體 NIC 的傳送緩衝區大小          | 您可以藉由執行命令來確認實體 Nic 是否支援此參數的設定 ```Get-NetAdapterAdvancedProperty``` 。 如果 Nic 不支援此參數，則命令的輸出不會包含「傳送緩衝區」屬性。 如果 NIC 支援此參數，您可以使用下列 Windows PowerShell 命令設定傳送緩衝區大小： <br> ```Set-NetAdapterAdvancedProperty "NIC1" –DisplayName "Transmit Buffers" –DisplayValue 3000``` <br>                           |
| 實體 NIC 的接收端調整 (RSS) | 您可以執行 Get-NetAdapterRss Windows PowerShell 命令，驗證實體 NIC 是否啟用 RSS。 您可以使用下列 Windows PowerShell 命令，在您的網路介面卡上啟用和設定 RSS： <br> ```Enable-NetAdapterRss "NIC1","NIC2"```<br> ```Set-NetAdapterRss "NIC1","NIC2" –NumberOfReceiveQueues 16 -MaxProcessors``` <br> 注意：如果已啟用 VMMQ 或 VMQ，則不需要在實體網路介面卡上啟用 RSS。 您可以在主機虛擬網路介面卡上啟用它 |
| VMMQ                                        | 若要啟用 VM 的 VMMQ，請執行下列命令： <br> ```Set-VmNetworkAdapter -VMName <gateway vm name>,-VrssEnabled $true -VmmqEnabled $true``` <br> 注意：並非所有網路介面卡都支援 VMMQ。 目前，在 T5 和 T6、Mellanox CX-3 和 CX-4 及 QLogic 45xxx 系列上支援它                                                                                                                                                                                                                                      |
| NIC 小組的虛擬機器佇列 (VMQ) | 您可以使用下列 Windows PowerShell 命令，在您的小組集合上啟用 VMQ： <br>```Enable-NetAdapterVmq``` <br> 注意：只有在硬體不支援 VMMQ 時，才會啟用此功能。 如果支援，則應該啟用 VMMQ 以獲得更好的效能。                                                                                                                                                                                                                                                               |
>[!Note]
> 只有當 VM 上的負載很高，且 CPU 使用率已達最大值時，VMQ 和 vRSS 才會進入圖片。 然後，至少會有一個處理器核心最大值。VMQ 和 vRSS 接著將有助於將處理負載分散到多個核心。 這不適用於 IPsec 流量，因為 IPsec 流量會限制為單一核心。

## <a name="windows-server-gateway-vm-configuration"></a>Windows Server 閘道 VM 設定

在這兩部 Hyper-v 主機上，您可以設定多個 Vm，並將其設定為具有 Windows Server 閘道的閘道。 您可以使用虛擬交換器管理員建立繫結至 Hyper-V 主機上 NIC 小組的 Hyper-V 虛擬交換器。 請注意，為了達到最佳效能，您應該在 Hyper-v 主機上部署單一閘道 VM。
以下是對每個 Windows Server 閘道 VM 建議的設定。

| 設定項目                 | Windows Powershell 設定                                                                                                                                                                                                                                                                                                                                                               |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 記憶體                             | 8 GB                                                                                                                                                                                                                                                                                                                                                                                           |
| 虛擬網路介面卡數目 | 3個具有下列特定用途的 Nic：1個用於管理作業系統所使用的管理，一個可提供外部網路存取的外部網路，1個提供內部網路的存取。                                                                                                                                                            |
| Receive Side Scaling (RSS)         | 您可以保留管理 NIC 的預設 RSS 設定。 下列範例設定是針對具有 8 個虛擬處理器的 VM。 針對外部和內部 Nic，您可以使用下列 Windows PowerShell 命令啟用將 BaseProcNumber 設為0，並將 MaxRssProcessors 設定為8的 RSS： <br> ```Set-NetAdapterRss "Internal","External" –BaseProcNumber 0 –MaxProcessorNumber 8``` <br> |
| 傳送端緩衝區                   | 您可以保留管理 NIC 的預設傳送端緩衝區設定。 對於內部和外部 Nic，您可以使用下列 Windows PowerShell 命令設定具有 32 MB RAM 的傳送端緩衝區： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Send Buffer Size" –DisplayValue "32MB"``` <br>                                                       |
| 接收端緩衝區                | 您可以保留管理 NIC 的預設接收端緩衝區設定。 對於內部和外部 Nic，您可以使用下列 Windows PowerShell 命令，設定具有 16 MB RAM 的接收端緩衝區： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Receive Buffer Size" –DisplayValue "16MB"``` <br>                                            |
| 轉送最佳化               | 您可以保留管理 NIC 的預設向前優化設定。 對於內部和外部 Nic，您可以使用下列 Windows PowerShell 命令啟用向前優化： <br> ```Set-NetAdapterAdvancedProperty "Internal","External" –DisplayName "Forward Optimization" –DisplayValue "1"``` <br>                                                                      |
