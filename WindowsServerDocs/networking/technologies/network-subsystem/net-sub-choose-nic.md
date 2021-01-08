---
title: 選擇網路介面卡
description: 瞭解如何瞭解可能會影響購買選項之網路介面卡的一些功能。
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.date: 08/07/2020
ms.openlocfilehash: 0a1730ccf8aef478d3d82ea9d08b1dd8129f3942
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040408"
---
# <a name="choosing-a-network-adapter"></a>選擇網路介面卡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解可能會影響購買選項之網路介面卡的一些功能。

需要大量網路的應用程式需要高效能的網路介面卡。 本節將探討一些選擇網路介面卡的考慮，以及如何設定不同的網路介面卡設定以達到最佳的網路效能。

> [!TIP]
>  您可以使用 Windows PowerShell 來設定網路介面卡設定。 如需詳細資訊，請參閱 [Windows PowerShell 中的網路介面卡 Cmdlet](/powershell/module/netadapter)。

##  <a name="offload-capabilities"></a><a name="bkmk_offload"></a> 卸載功能

將工作從中央處理器 cpu 卸載 \( \) 至網路介面卡可減少伺服器上的 CPU 使用量，進而改善整體系統效能。

如果您選取具有適當卸載功能的網路介面卡，則 Microsoft 產品中的網路堆疊可以將一或多個工作卸載至網路介面卡。 下表概述可在 Windows Server 2016 中使用的不同卸載功能。

|卸載類型|描述|
|------------------|-----------------|
|TCP 的總和檢查碼計算|網路堆疊可以 \( \) 在傳送和接收程式碼路徑上卸載傳輸控制通訊協定 TCP 總和檢查碼的計算和驗證。 它也可以在傳送和接收程式碼路徑上卸載 IPv4 和 IPv6 總和檢查碼的計算和驗證。|
|UDP 的總和檢查碼計算 |網路堆疊可以 \( \) 在傳送和接收程式碼路徑上卸載使用者資料包協定 UDP 總和檢查碼的計算和驗證。|
|IPv4 的總和檢查碼計算 |網路堆疊可以卸載傳送和接收程式碼路徑上 IPv4 總和檢查碼的計算和驗證。 |
|IPv6 的總和檢查碼計算 |網路堆疊可以在傳送和接收程式碼路徑上卸載 IPv6 總和檢查碼的計算和驗證。 |
|分割大型 TCP 封包|TCP/IP 傳輸層支援大型傳送卸載 v2 (LSOv2) 。 使用 LSOv2 時，TCP/IP 傳輸層可以將大型 TCP 封包的分割卸載到網路介面卡。|
|接收端調整 \( RSS\)|RSS 是網路驅動程式技術，可在多處理器系統中的多個 Cpu 上有效率地散發網路接收處理。 本主題稍後會提供更多有關 RSS 的詳細資料。|
|接收區段聯合 \( RSC\)|RSC 可將封包群組在一起，以最小化主機執行所需的標頭處理。 最多可將 64 KB 的已接收承載結合為一個較大的封包，以進行處理。 本主題稍後會提供更多有關 RSC 的詳細資料。|

###  <a name="receive-side-scaling"></a><a name="bkmk_rss"></a> 接收端調整

Windows Server 2016、Windows Server 2012、Windows Server 2012 R2、Windows Server 2008 R2 和 Windows Server 2008 支援接收端調整 \( RSS \) 。

某些伺服器會使用多個邏輯處理器來設定，這些邏輯處理器可共用硬體資源 \( （例如實體核心） \) ，並被視為同時多執行緒 \( SMT \) 對等。 Intel Hyper-Threading 技術即為範例。 RSS 會將網路處理導向每個核心最多一個邏輯處理器。 例如，在具有 Intel 超執行緒、4核心和8個邏輯處理器的伺服器上，RSS 會使用4個以上的邏輯處理器來進行網路處理。

RSS 會在邏輯處理器之間散發連入的網路 i/o 封包，因此屬於相同 TCP 連線的封包會在相同的邏輯處理器上處理，以保留順序。

RSS 也會對 UDP 單播和多播流量進行負載平衡，並將 \( 來源和目的地位址雜湊所決定的相關流程路由傳送 \) 至相同的邏輯處理器，以保留相關抵達的順序。 這有助於針對擁有較少的網路介面卡，而不是符合合格邏輯處理器之伺服器的高用量案例，改善擴充性和效能。

#### <a name="configuring-rss"></a>設定 RSS

在 Windows Server 2016 中，您可以使用 Windows PowerShell Cmdlet 和 RSS 設定檔來設定 RSS。

您可以使用 **get-netadapterrss** Windows PowerShell Cmdlet 的 **– PROFILE** 參數定義 RSS 設定檔。

**Windows PowerShell 設定 RSS 的命令**

下列 Cmdlet 可讓您查看並修改每個網路介面卡的 RSS 參數。

>[!NOTE]
>如需每個 Cmdlet 的詳細命令參考（包括語法和參數），您可以按一下下列連結。 此外，您可以在 Windows PowerShell 提示字元中，將 Cmdlet 名稱傳遞給 **get-help** ，以取得每個命令的詳細資料。

- [停用-get-netadapterrss](/powershell/module/netadapter/Disable-NetAdapterRss)。 此命令會在您指定的網路介面卡上停用 RSS。

- [啟用-get-netadapterrss](/powershell/module/netadapter/Enable-NetAdapterRss)。 此命令會在您指定的網路介面卡上啟用 RSS。

- [Get-netadapterrss](/powershell/module/netadapter/Get-NetAdapterRss)。 此命令會抓取您所指定網路介面卡的 RSS 內容。

- [設定-get-netadapterrss](/powershell/module/netadapter/Set-NetAdapterRss)。 此命令會在您指定的網路介面卡上設定 RSS 屬性。

#### <a name="rss-profiles"></a>RSS 設定檔

您可以使用 Set-NetAdapterRss Cmdlet 的 **– Profile** 參數來指定要將哪些邏輯處理器指派給哪些網路介面卡。 此參數的可用值為：

- **最接近**。 慣用網路介面卡基礎 RSS 處理器附近的邏輯處理器號碼。 使用此設定檔時，作業系統可能會根據負載動態地重新平衡邏輯處理器。

- **ClosestStatic**。 慣用網路介面卡基礎 RSS 處理器附近的邏輯處理器號碼。 使用此設定檔時，作業系統不會根據負載動態地重新平衡邏輯處理器。

- **NUMA**。 通常會在不同的 NUMA 節點上選取邏輯處理器數目，以分散負載。 使用此設定檔時，作業系統可能會根據負載動態地重新平衡邏輯處理器。

- **NUMAStatic**。 這是 **預設設定檔**。 通常會在不同的 NUMA 節點上選取邏輯處理器數目，以分散負載。 使用此設定檔時，作業系統不會根據負載動態地重新平衡邏輯處理器。

- **保守**。 RSS 盡可能使用最少的處理器來承受負載。 此選項有助於減少中斷次數。

根據案例和工作負載特性，您也可以使用 **get-netadapterrss** Windows PowerShell Cmdlet 的其他參數來指定下列各項：

- 在每個網路介面卡上，可用於 RSS 的邏輯處理器數目。
- 邏輯處理器範圍的起始位移。
- 網路介面卡用來配置記憶體的節點。

以下是您可以用來設定 RSS 的其他 **get-netadapterrss** 參數：

>[!NOTE]
>在以下每個參數的語法範例中，網路介面卡名稱 **Ethernet** 會用來作為 **get-netadapterrss** 命令的 **– name** 參數範例值。 當您執行 Cmdlet 時，請確定您使用的網路介面卡名稱適用于您的環境。

- **\* MaxProcessors**：設定要使用的 RSS 處理器最大數目。 這可確保應用程式流量會系結至指定介面上的最大處理器數目。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessors <value>`

- **\* BaseProcessorGroup**：設定 NUMA 節點的基底處理器群組。 這會影響 RSS 所使用的處理器陣列。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorGroup <value>`

- **\* MaxProcessorGroup**：設定 NUMA 節點的最大處理器群組。 這會影響 RSS 所使用的處理器陣列。 設定此項會限制最大處理器群組，讓負載平衡在 k 群組內對齊。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**：設定 NUMA 節點的基本處理器號碼。 這會影響 RSS 所使用的處理器陣列。 這可讓您跨網路介面卡分割處理器。 這是指派給每個介面卡的 RSS 處理器範圍中的第一個邏輯處理器。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**：每個網路介面卡可從中配置記憶體的 NUMA 節點。 這可以位於 k 群組或不同的 k 群組內。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –NumaNodeID <value>`

- **\* NumberofReceiveQueues**：如果您的邏輯處理器似乎使用量過低，例如 \( ，如工作管理員所示， \) 您可以嘗試將 RSS 佇列的數目從預設值2增加到網路介面卡所支援的最大值。 您的網路介面卡可能會有選項可將 RSS 佇列數目變更為驅動程式的一部分。 範例語法：

     `Set-NetAdapterRss –Name "Ethernet" –NumberOfReceiveQueues <value>`

如需詳細資訊，請按一下下列連結以下載 [可擴充的網路功能：消除接收處理瓶頸— RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) 的文字格式簡介。

#### <a name="understanding-rss-performance"></a>瞭解 RSS 效能

調整 RSS 需要瞭解設定和負載平衡邏輯。 若要確認 RSS 設定是否生效，您可以在執行 **get-netadapterrss** Windows PowerShell Cmdlet 時檢查輸出。 以下是此 Cmdlet 的範例輸出。

```

PS C:\Users\Administrator> get-netadapterrss
Name                           : testnic 2
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8

IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4
…
(# indirection table entries are a power of 2 and based on # of processors)
…
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4
```

除了已設定的回顯參數之外，輸出的關鍵區段也是間接資料表輸出。 間接取值表會顯示用來散發連入流量的雜湊表值區。 在此範例中，n:c 標記法指定用來導向連入流量的 Numa K 群組： CPU 索引配對。 我們會看到剛好有2個唯一專案 (0:0 和 0:4) ，分別代表 k 組 0/cpu0 和 k 組 0/cpu 4。

這個系統只有一個 k 群組 (k 群組 0) 和 n (，其中 n <= 128) 間接資料表專案。 由於接收佇列的數目設定為2，因此只會選擇2個處理器 (0:0，0:4) ，即使最大處理器是設定為8。 實際上，間接取值表會將連入流量雜湊成隻使用8個可用的2個 Cpu。

若要充分利用 Cpu，RSS 接收佇列的數目必須等於或大於最大處理器數目。 在上述範例中，接收佇列應設定為8或更高。

#### <a name="nic-teaming-and-rss"></a>NIC 小組和 RSS

您可以使用 NIC 小組，在與另一個網路介面卡組合的網路介面卡上啟用 RSS。 在此案例中，只有基礎實體網路介面卡可以設定為使用 RSS。 使用者無法在組合的網路介面卡上設定 RSS Cmdlet。

###  <a name="receive-segment-coalescing-rsc"></a><a name="bkmk_rsc"></a> (RSC) 接收區段聯合

接收區段聯合 \( RSC \) 可透過減少針對指定的接收資料量所處理的 IP 標頭數目，來協助效能。 它應該用來藉由 \( \) 將較小的封包分組或聯合至較大的單位，來協助調整接收資料的效能。

這種方法可能會影響延遲，而且在輸送量增益中最常看到的優點。 建議使用 RSC 來增加收到的繁重工作負載輸送量。 請考慮部署支援 RSC 的網路介面卡。

在這些網路介面卡上， \( \) 除非您有特定的工作負載（ \( 例如，低延遲、低輸送量的網路， \) 顯示可從 RSC 關閉的情況下），否則請確定 [RSC] 為預設設定。

#### <a name="understanding-rsc-diagnostics"></a>瞭解 RSC 診斷

您可以使用 Windows PowerShell Cmdlet **NetAdapterRsc** 和 **NetAdapterStatistics** 診斷 RSC。

以下是當您執行 Get-NetAdapterRsc Cmdlet 時的輸出範例。

```

PS C:\Users\Administrator> Get-NetAdapterRsc

Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure
                                            Reason
----                           -----------  -----------  --------------- --------------- ----------------- ------------
Ethernet                       True         False        True            False                  NoFailure       NicProperties

```

**Get** Cmdlet 會顯示介面中是否已啟用 rsc，以及 TCP 是否讓 rsc 處於操作狀態。 失敗原因提供無法在該介面上啟用 RSC 的詳細資料。

在先前的案例中，會在介面中支援並操作 IPv4 RSC。 若要瞭解診斷失敗，可以看到造成的合併位元組或例外狀況。 這會提供聯合問題的指示。

以下是當您執行 Get-NetAdapterStatistics Cmdlet 時的輸出範例。

```
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics "myAdapter"
PS C:\Users\Administrator> $x.rscstatistics

CoalescedBytes       : 0
CoalescedPackets     : 0
CoalescingEvents     : 0
CoalescingExceptions : 0

```

#### <a name="rsc-and-virtualization"></a>RSC 和虛擬化

只有在主機網路介面卡未系結至 Hyper-v 虛擬交換器時，實體主機才支援 RSC。 當主機系結至 Hyper-v 虛擬交換器時，作業系統會停用 RSC。 此外，虛擬機器無法取得 RSC 的優點，因為虛擬網路介面卡不支援 RSC。

啟用單一根目錄輸入/輸出虛擬化 Sr-iov 時，可為虛擬機器啟用 RSC \( \) 。 在此情況下，虛擬函式支援 RSC 功能;因此，虛擬機器也會獲得 RSC 的優點。

##  <a name="network-adapter-resources"></a><a name="bkmk_resources"></a> 網路介面卡資源

有幾個網路介面卡會主動管理其資源以達到最佳效能。 數個網路介面卡可讓您使用介面卡的 [[ **Advanced** network] （網路介面卡）索引標籤，手動設定資源。 針對這類介面卡，您可以設定多個參數的值，包括接收緩衝區和傳送緩衝區的數目。

使用下列 Windows PowerShell Cmdlet 可簡化設定網路介面卡資源。

- [NetAdapterAdvancedProperty](/powershell/module/netadapter/Get-NetAdapterAdvancedProperty)

- [設定-NetAdapterAdvancedProperty](/powershell/module/netadapter/Set-NetAdapterAdvancedProperty)

- [啟用-Get-netadapter](/powershell/module/netadapter/Enable-NetAdapte)

- [啟用-NetAdapterBinding](/powershell/module/netadapter/Enable-NetAdapterBinding)

- [啟用-NetAdapterChecksumOffload](/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [啟用-NetAdapterIPSecOffload](/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [啟用-NetAdapterLso](/powershell/module/netadapter/Enable-NetAdapterLso)

- [啟用-NetAdapterPowerManagement](/powershell/module/netadapter/Enable-NetAdapterPowerManagement)

- [啟用-Get-netadapterqos](/powershell/module/netadapter/Enable-NetAdapterQos)

- [啟用-NetAdapterRDMA](/powershell/module/netadapter/Enable-NetAdapterRDMA)

- [啟用-Get-netadaptersriov 命名](/powershell/module/netadapter/Enable-NetAdapterSriov)

如需詳細資訊，請參閱 [Windows PowerShell 中的網路介面卡 Cmdlet](/powershell/module/netadapter)。

如需本指南中所有主題的連結，請參閱 [網路子系統效能微調](net-sub-performance-top.md)。