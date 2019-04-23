---
title: 選擇網路介面卡
description: 本主題是 Windows Server 2016 的網路子系統效能調整指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875849"
---
# <a name="choosing-a-network-adapter"></a>選擇網路介面卡

>適用於：Windows Server （半年通道），Windows Server 2016

您可以使用本主題來了解一些可能會影響您的購買選項的網路介面卡的功能。

網路高用量應用程式需要高效能的網路介面卡。 本節將探討一些選擇網路介面卡，以及如何設定不同的網路介面卡設定，以達到最佳的網路效能的考量。

> [!TIP]
>  您可以使用 Windows PowerShell 設定網路介面卡設定。 如需詳細資訊，請參閱 <<c0> [ 在 Windows PowerShell 中的網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a> 卸載功能

將工作從中央處理單元卸載\(CPU\)到網路介面卡可以減少 CPU 使用量，在伺服器上，進而改善整體系統效能。

Microsoft 產品中的網路堆疊可以卸載一或多個工作，如果您選取適當的網路介面卡的網路介面卡卸載功能。 下表提供可在 Windows Server 2016 中使用不同的卸載功能的簡短概觀。
  
|卸載類型|描述|
|------------------|-----------------|  
|TCP 的總和檢查碼計算|計算和驗證的傳輸控制通訊協定，可卸載網路堆疊\(TCP\)總和檢查碼上的傳送和接收的程式碼路徑。 它也可卸載的計算和驗證 IPv4 和 IPv6 總和檢查碼，在傳送和接收的程式碼路徑。|  
|Udp 總和檢查碼計算 |計算和驗證使用者資料包通訊協定，可卸載網路堆疊\(UDP\)總和檢查碼上的傳送和接收的程式碼路徑。|
|適用於 IPv4 的總和檢查碼計算 |計算和驗證 IPv4 上的總和檢查碼傳送和接收的程式碼路徑，可卸載網路堆疊。 |
|適用於 IPv6 的總和檢查碼計算 |計算和驗證總和檢查碼上的傳送和接收的程式碼路徑的 IPv6，則可卸載網路堆疊。 | 
|分割大型的 TCP 封包的|TCP/IP 傳輸層支援大型傳送卸載 v2 (LSOv2)。 LSOv2，使用 TCP/IP 傳輸層可卸載網路介面卡的大型 TCP 封包的分割。|  
|接收端調整\(RSS\)|RSS 是網路驅動程式的一項技術可讓網路有效率散佈接收多個 Cpu 處理，在多處理器系統中。 本主題稍後會提供 RSS 的更詳細。|  
|接收區段聯合\(RSC\)|RSC 是群組的封包在一起，以減少處理的標頭的功能所需的主應用程式執行。 最多 64 kb 的已接收的承載可以結合成單一的較大封包進行處理。 本主題稍後會提供 RSC 的更詳細。|  
  
###  <a name="bkmk_rss"></a> 接收端調整

Windows Server 2016、 Windows Server 2012、 Windows Server 2012 R2、 Windows Server 2008 R2 和 Windows Server 2008 支援接收端調整\(RSS\)。 

某些伺服器設定使用共用的硬體資源的多個邏輯處理器\(等實體核心\)並將其視為多執行緒處理的並行\(SMT\)對等。 Intel 超執行緒技術是範例。 RSS 將導向至每個核心最多一個邏輯處理器的網路處理。 例如，在伺服器上與 Intel 超執行緒，4 核心，8 個邏輯處理器，RSS 會使用進行網路處理不超過 4 個邏輯處理器。  

RSS，以便將屬於相同的 TCP 連線的封包會處理相同的邏輯處理器，會保留順序，會將邏輯處理器之間的連入網路 I/O 封包。 

RSS 也會載入平衡 UDP 單點傳播與多點傳送的流量，並將路由的相關的流程\(這取決於雜湊的來源和目的地位址\)為相同的邏輯處理器，保留 相關的抵達的順序。 這有助於改善延展性和效能有較少的網路介面卡比合格的邏輯處理器的伺服器收到高用量等案例。 

#### <a name="configuring-rss"></a>設定 RSS

在 Windows Server 2016 中，您可以使用 Windows PowerShell cmdlet 和 RSS 設定檔設定 RSS。 

您可以使用，以定義 RSS 設定檔 **– 設定檔**的參數**Set-netadapterrss** Windows PowerShell cmdlet。

**RSS 設定的 Windows PowerShell 命令**

下列指令程式可讓您查看並修改每個網路介面卡的 RSS 參數。
  
>[!NOTE]
>如需詳細的命令參考，針對每個 cmdlet，包括語法與參數，您可以按一下下列連結。 此外，您可以將 cmdlet 名稱以傳遞**Get-help**在 Windows PowerShell 提示字元中，如需有關每個命令。  

- [停用 NetAdapterRss](https://technet.microsoft.com/library/jj130892)。 此命令會在您指定的網路介面卡上，停用 RSS。

- [啟用 NetAdapterRss](https://technet.microsoft.com/library/jj130859)。 此命令會在您指定的網路介面卡啟用 RSS。
  
- [Get-netadapterrss](https://technet.microsoft.com/library/jj130912)。 此命令會擷取您指定的網路介面卡的 RSS 內容。
  
- [Set-netadapterrss](https://technet.microsoft.com/library/jj130863)。 此命令會在您指定的網路介面卡上設定 RSS 內容。  

#### <a name="rss-profiles"></a>RSS 設定檔

您可以使用 **– 設定檔**Set-netadapterrss 指令程式，以指定哪一個邏輯處理器指派給哪個網路介面卡參數。 此參數可用值為：

- **最接近**。 即將基底 RSS 處理器的網路介面卡的邏輯處理器數目較好。 這個設定檔，作業系統可能會重新平衡會根據負載，以動態方式的邏輯處理器。
  
- **ClosestStatic**。 較好的網路介面卡的基本 RSS 處理器附近的邏輯處理器數目。 這個設定檔，作業系統不會不會重新平衡會根據負載，以動態方式的邏輯處理器。
  
- **NUMA**。 邏輯處理器數目通常會將負載分散到選取不同的 NUMA 節點上。 這個設定檔，作業系統可能會重新平衡會根據負載，以動態方式的邏輯處理器。
  
- **NUMAStatic**。 這是**預設設定檔**。 邏輯處理器數目通常會將負載分散到選取不同的 NUMA 節點上。 這個設定檔，作業系統將不會重新平衡會根據負載，以動態方式的邏輯處理器。

- **保守**。 RSS 會盡可以最少的處理器使用來承受負載。 這個選項可減少中斷。

根據案例和工作負載特性，您也可以使用其他參數**Set-netadapterrss** Windows PowerShell cmdlet 來指定下列項目：

- 針對每個網路介面卡，多少個邏輯處理器可以用於 RSS。
- 邏輯處理器的範圍開始的位移。
- 從中的網路介面卡會配置記憶體節點。

以下是額外**Set-netadapterrss**設定 RSS，您可以使用的參數：

>[!NOTE]
>在網路介面卡名稱下方的每個參數的範例語法**Ethernet**做為範例值 **– 名稱**參數**Set-netadapterrss**命令。 當您執行 cmdlet 時，請確定您使用的網路介面卡名稱是適用於您的環境。

- **\* MaxProcessors**:設定要使用的 RSS 處理器的數目上限。 這可確保，應用程式流量會繫結至最大處理器數目在指定的介面。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**:設定基底的處理器群組中的 NUMA 節點。 這會影響使用 RSS 處理器陣列。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**:設定最大處理器群組中的 NUMA 節點。 這會影響使用 RSS 處理器陣列。 將此設定會限制最大處理器群組，使負載平衡一個 k 群組中的對齊。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**:設定 NUMA 節點的基底的處理器數目。 這會影響使用 RSS 處理器陣列。 這可讓網路介面卡跨資料分割的處理器。 這是第一個邏輯處理器範圍的 RSS 處理器中指派給每個介面卡。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**:NUMA 節點的每個網路介面卡可以配置的記憶體。 這可以是 k 群組中，或從不同的 k 群組。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**:如果您的邏輯處理器似乎並未充份使用接收流量\(比方說，做為檢視中 工作管理員\)，您可以嘗試增加至您的網路介面卡是否支援的最大值從預設值是 2 的 RSS 佇列數目. 您的網路介面卡可能有選項可變更 RSS 佇列數目，此驅動程式的一部分。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

如需詳細資訊，請按一下以下的連結下載[Scalable Networking:刪除接收處理瓶頸，簡介 RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) Word 格式。
  
#### <a name="understanding-rss-performance"></a>了解 RSS 效能

微調 RSS 需要了解組態和負載平衡的邏輯。 若要確認的 RSS 設定生效，您可以檢閱輸出，當您執行**Get-netadapterrss** Windows PowerShell cmdlet。 以下是此 cmdlet 的範例輸出。
  
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

除了回應所設定的參數，輸出的重點是間接取值的資料表輸出。 間接表格會顯示用來將連入流量分散雜湊表值區。 在此範例中，n:c 標記法指定 Numa K-用來連入流量導向的群組： CPU 索引組。 我們看到 2 個唯一的項目 (0:0，0:4)，分別代表 k 群組 0/cpu0 和 k 群組 0/cpu 4。

此系統 （k 群組 0） 和 n 的只有一個 k 群組 (其中 n < = 128) 間接表格項目。 因為接收佇列數目設定為 2，只有 2 個處理器 (0:0，0:4) 都已選取-即使最大的處理器設為 8。 實際上，間接表格雜湊連入流量只使用超出 8 所提供的 2 個 Cpu。

若要充分運用 Cpu，RSS 接收佇列的數目必須等於或大於最大處理器。 在上述範例中，接收佇列應該設定為 8 或更高。

#### <a name="nic-teaming-and-rss"></a>NIC 小組和 RSS

可以組合使用另一個網路介面卡使用 NIC 小組之網路介面卡啟用 RSS。 在此案例中，只有基礎實體網路介面卡可以設定為使用 RSS。 使用者無法設定 RSS 的指令程式的組合的網路介面卡。
  
###  <a name="bkmk_rsc"></a> 接收區段聯合 (RSC)

接收區段聯合\(RSC\)有助於減少處理一段指定的接收資料的 IP 標頭數目的效能。 它應該用來協助調整效能的已接收的資料分組\(聯合或\)較小的封包到較大的單位。

這種方法可能會延遲影響大部分看到輸送量提升的優點。 若要增加輸送量接收繁重的工作負載，建議您使用 RSC。 請考慮部署支援 RSC 的網路介面卡。 

這些網路介面卡，請確認 RSC 上\(這是預設值\)，除非您有特定的工作負載\(比方說，低延遲、 低網路輸送量\)RSC 正在關閉該顯示獲益.

#### <a name="understanding-rsc-diagnostics"></a>了解 RSC 診斷

您可以使用 Windows PowerShell cmdlet 來診斷 RSC **Get NetAdapterRsc**並**Get NetAdapterStatistics**。

當您執行 Get NetAdapterRsc cmdlet，以下是範例輸出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**取得**cmdlet 顯示 RSC 在介面中是否已啟用，以及是否 TCP 會啟用 RSC 處於運作狀態。 失敗的原因提供詳細資料以啟用該介面的 RSC 失敗。

在前述案例中，IPv4 RSC 會在介面中是支援且可運作。 若要了解診斷失敗，使用者可以看到聯合的位元組或造成例外狀況。 這會提供聯合的問題的指示。

當您執行 Get NetAdapterStatistics cmdlet，以下是範例輸出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和虛擬化

主機網路介面卡未繫結至 HYPER-V 虛擬交換器時，RSC 才會支援實體的主應用程式中。 主應用程式繫結至 HYPER-V 虛擬交換器時，由作業系統已停用 RSC。 此外，虛擬機器不會取得 RSC 的好處，因為虛擬網路介面卡不支援 RSC。

RSC 可以啟用虛擬機器時單一根目錄輸入/輸出虛擬化\(SR-IOV\)已啟用。 在此情況下，虛擬函式支援 RSC 功能;因此，虛擬機器也會收到 RSC 的優點。

##  <a name="bkmk_resources"></a> 網路介面卡資源

幾個網路介面卡會主動管理資源以達到最佳效能。 數個網路介面卡可讓您使用手動設定資源**進階網路**配接器 索引標籤。 對於這類配接器，您可以設定的值數目的參數，包括接收緩衝區的數目，以及傳送緩衝區。

使用下列 Windows PowerShell cmdlet 簡化設定網路介面卡資源。

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

如需詳細資訊，請參閱 <<c0> [ 在 Windows PowerShell 中的網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

本指南中的所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。