---
title: 選擇 [網路介面卡
description: 本主題是部分的 Windows Server 2016 的網路效能子系統調整節目表。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>選擇 [網路介面卡

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題以了解部分的網路介面卡可能會影響您的購買選擇的功能。

網路大量的應用程式需要高效能網路介面卡。 本章節瀏覽來選擇網路介面卡，以及如何設定不同的網路介面卡設定，以獲得最佳的網路效能的事項。

> [!TIP]
>  您可以使用 Windows PowerShell 來設定網路介面卡設定。 如需詳細資訊，請查看[Windows PowerShell 中的 [網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a>卸載功能

將卸載中央處理中的工作 \(CPU\) 到網路介面卡可以減少 CPU 使用率在伺服器上，這可以改善整體的系統效能。

網路中的堆疊 Microsoft 你可以卸一或多個任務的網路介面卡，如果您選取適當的網路介面卡卸載功能。 下表提供可在 Windows Server 2016 的不同承載功能的簡短的概觀。
  
|卸載類型|描述|
|------------------|-----------------|  
|檢查值計算 tcp|網路堆疊可以卸計算與驗證的傳輸控制項通訊協定 \(TCP\) 總和檢查上傳送和接收驗證碼的路徑。 也可以卸計算和驗證的 [IPv4，並在 [IPv6 總和檢查傳送和接收驗證碼的路徑。|  
|檢查值計算 UDP |網路堆疊可以卸計算及使用者資料流通訊協定 \(UDP\) 總和檢查上的驗證傳送和接收驗證碼的路徑。|
|檢查值計算 IPv4 |網路堆疊可以下載計算和驗證的 IPv4 總和檢查上的傳送和接收驗證碼的路徑。 |
|檢查值計算 ipv6 |網路堆疊可以下載計算和驗證的 IPv6 總和檢查上的傳送和接收驗證碼的路徑。 | 
|大 TCP 封包分割可|TCP/IP 傳輸層支援大型傳送卸載 v2 (LSOv2)。 使用 LSOv2，TCP/IP 傳輸層可以下載分割的網路介面卡大型 TCP 封包。|  
|接收縮放比例 \(RSS\) 側邊|RSS 是讓有效率通訊網路的網路驅動程式技術跨多個 Cpu 處理收到多處理器系統中。 稍後本主題會提供更多詳細資訊的 RSS。|  
|接收聯合 \(RSC\) 區段|RSC 是在一起，以減少處理標頭群組封包的功能需要執行主機。 最多 64 KB 收到裝載的可以聯合成單一的處理大封包。 稍後本主題會提供 RSC 有關更多詳細資料。|  
  
###  <a name="bkmk_rss"></a>接收側邊縮放比例

Windows Server 2016、Windows Server 2012、Windows Server 2012 R2、Windows Server 2008 R2 和 Windows Server 2008 支援收到側邊縮放比例 \(RSS\)。 

會以多個邏輯處理器共用硬體資源設定部分伺服器 \（例如所在 core\)，這會被視為同時多執行緒 \(SMT\) 對等。 Intel 超執行緒技術為範例。 RSS 指示核心每一個最多邏輯處理器網路處理。 例如，含 Hyper-v 的 Intel 執行緒、4 核心和 8 邏輯處理器的伺服器上, RSS 使用超過 4 邏輯處理器網路處理。  

RSS 分散邏輯處理器之間連入網路 I/O 封包，以便在相同的邏輯處理器，可保留訂購處理屬於相同的 TCP 連接封包。 

RSS 也會載入餘額 UDP 單和多點的流量，以及路由相關的流量 \（這由 hashing 來源和目的地 addresses\）以相同的邏輯處理器保留相關 app 的順序。 這可協助改善延展性和效能會收到案例中有超過一般符合資格的邏輯處理器較少的網路介面卡的伺服器。 

#### <a name="configuring-rss"></a>設定 RSS

在 Windows Server 2016 中，您可以設定 RSS 使用 Windows PowerShell cmdlet 和 RSS 設定檔。 

您可以定義 RSS 設定檔，使用**– 設定檔**的參數**設定為 NetAdapterRss** Windows PowerShell cmdlet。

**Windows PowerShell 命令 RSS 設定**

下列 cmdlet 可讓您查看與修改 RSS 參數每網路介面卡。
  
>[!NOTE]
>命令的詳細參考的每個 cmdlet，包括語法與參數，您可以按一下下列連結。 此外，您可以將傳遞 cmdlet 名稱以**取得-協助**在每一個命令的詳細資訊的 Windows PowerShell 命令提示字元。  

- [停用-NetAdapterRss](https://technet.microsoft.com/library/jj130892)。 這個命令停用上指定的網路介面卡的 RSS。

- [讓-NetAdapterRss](https://technet.microsoft.com/library/jj130859)。 這個命令可讓 RSS，指定的網路介面卡上。
  
- [取得-NetAdapterRss](https://technet.microsoft.com/library/jj130912)。 這個命令擷取 RSS 屬性，指定的網路介面卡。
  
- [設定-NetAdapterRss](https://technet.microsoft.com/library/jj130863)。 這個命令設定 RSS 屬性，指定的網路介面卡上。  

#### <a name="rss-profiles"></a>RSS 設定檔

您可以使用**– 設定檔**cmdlet Set-NetAdapterRss 指定的邏輯處理器已指派給的網路介面卡的參數。 使用此參數值︰

- **接近**。 網路介面卡的基底 RSS 處理器附近的邏輯處理器數字的慣用。 使用此設定擋，可能會重新邏輯處理器動態根據負載平衡作業系統。
  
- **ClosestStatic**。 較靠近網路介面卡的基底 RSS 處理器邏輯處理器數字。 使用此設定擋，作業系統會不重新平衡邏輯動態根據載入的處理器。
  
- **努馬**。 在不同努馬節點散發載入通常選取邏輯處理器編號。 使用此設定擋，可能會重新邏輯處理器動態根據負載平衡作業系統。
  
- **NUMAStatic**。 這是**預設設定檔**。 在不同努馬節點散發載入通常選取邏輯處理器編號。 使用此設定擋，作業系統會不會重新平衡根據載入動態邏輯處理器。

- **保守**。 RSS 使用維持載入盡可能較少的處理器。 這個選項可協助您減少中斷。

根據您案例和工作負載特性，您也可以使用其他的參數**設定為 NetAdapterRss** Windows PowerShell cmdlet 指定下列：

- 在每次網路介面卡，多少邏輯處理器可用於 RSS。
- 從時差各種不同的邏輯處理器。
- [網路介面卡將會從的配置記憶體] 節點。

以下是詳細**設定為 NetAdapterRss**參數，可供您設定 RSS:

>[!NOTE]
>在每個參數網路介面卡的名稱下方的範例語法**乙太網路**使用做為範例值**– 名稱**的參數**設定-NetAdapterRss**命令。 當您執行 cmdlet 時，請確定您使用的網路介面卡名稱會根據您的環境。

- **\ * MaxProcessors**：設定 RSS 處理器来使用的最大。 這樣可確保的應用程式流量繫結至上限處理器指定介面。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**：設定努馬節點的基底處理器群組。 這會影響 RSS 使用處理器陣列。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**：設定努馬節點的最大值處理器群組。 這會影響 RSS 使用處理器陣列。 此設定會限制最大的處理器群組，因此負載平衡對齊 k 群組中。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**：設定努馬節點的基底的處理器。 這會影響 RSS 使用處理器陣列。 這可讓所有網路介面卡分割處理器。 這是第一個邏輯處理器在各種不同的 RSS 處理器指派給每個顯示卡。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**：的每個網路介面卡可以配置從記憶體努馬節點。 這可以是 k 群組中，或從不同的 k 群組。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**：您邏輯處理器似乎充分接收流量 \（例如，在檢視中工作 Manager\），您可以嘗試增加 RSS 佇列預設為 2 到您的網路介面卡所支援的最大的數目。 您的網路介面卡可能會有選項] 來變更的數目 RSS 佇列驅動程式的一部分。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

如需詳細資訊，請按一下下列連結來下載[延展性網路：排除收到處理瓶頸 — 簡介 RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc)文字的格式。
  
#### <a name="understanding-rss-performance"></a>了解 RSS 效能

調整 RSS 需要了解設定和負載平衡邏輯。 確認已生效 RSS 設定，您在執行時，您可以檢視輸出到**取得-NetAdapterRss** Windows PowerShell cmdlet。 以下是此 cmdlet 的範例輸出。
  
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

除了回應所設定的參數，輸出主要層面是間接表格輸出。 間接下表顯示 hash 資料表的容量，用來輸入流量。 在此範例中，表示法 n:c 指定努馬 K-用來引導輸入流量的群組：CPU 索引配對。 我們會看到完全 2 獨特的項目 (0:0，0:4)，這代表 k 群組 0 日 cpu0 和 k 群組 0 日 cpu 4 分別。

只有一個 k-群組此系統（k 群組 0）和 n (其中 n < = 128) 間接表項目。 因為佇列接收的數字為 2，2 處理器 (0:0，0:4) 選擇-即使是最大的處理器為 8。 事實上，間接表格 hashing 傳入僅使用 2 Cpu 退出 8 可用的資料傳輸。

若要充分運用 Cpu，等於或大於最大的處理器必須是 RSS 收到佇列數目。 在前一個範例中，會收到佇列應會設定為 8 或更高。

#### <a name="nic-teaming-and-rss"></a>NIC 小組與 RSS

在 [網路介面卡的就使用其他網路介面卡使用 NIC 團隊合作，以用 RSS。 在本案例中，只有基礎實體網路介面卡可以使用 RSS 設定。 使用者無法設定 RSS cmdlet 小組的網路介面卡上。
  
###  <a name="bkmk_rsc"></a>接收區段聯合 (RSC)

減少的 IP 標頭一段指定收到的資料的處理來接收區段聯合 \(RSC\) 協助效能。 它應該會用來協助縮放收到的資料的效能的群組 \(or coalescing\) 較小的封包到較大的單位。

這種方式可以影響延遲的優點大部分輸送量所示。 RSC 增加輸送量收到重裝工作負載的建議。 請考慮部署支援 RSC 網路介面卡。 

這些網路介面卡，請確定 RSC 位於 \（這是預設 setting\），除非您有特定的工作負載 \（例如，低延遲、低輸送量 networking\）RSC 被關閉該顯示的收穫。

#### <a name="understanding-rsc-diagnostics"></a>了解 RSC 診斷

您可以使用 Windows PowerShell cmdlet 診斷 RSC**取得-NetAdapterRsc**和**取得-NetAdapterStatistics**。

當您執行 Get-NetAdapterRsc cmdlet 時，以下是範例輸出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**取得**cmdlet 會顯示在介面 RSC 功能的是否和 TCP 是否讓 RSC 操作狀態。 失敗的原因提供詳細資訊，可讓介面 RSC 失敗。

在前一個案例中，IPv4 RSC 處於支援及操作介面。 若要了解診斷失敗，其中一個可以看到聯合的位元組或例外造成。 這會提供聯合問題的指示。

當您執行 Get-NetAdapterStatistics cmdlet 時，以下是範例輸出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和模擬

RSC 僅支援實體主機主機網路介面卡未繫結至 HYPER-V Virtual 開關切換至時。 當您的主機繫結至 HYPER-V Virtual 開關切換至作業系統已停用 RSC。 此外，虛擬電腦不會取得 RSC 的優點，RSC 不支援 virtual 網路介面卡因為。

RSC 時支援單一根輸入/輸出模擬 \(SR-IOV\) 是可以進行一樣。 若是如此，virtual 功能支援 RSC 功能。因此，虛擬電腦也會收到 RSC 的好處。

##  <a name="bkmk_resources"></a>網路介面卡的資源

幾個網路介面卡正在管理他們的資源達到最佳效能。 幾個網路介面卡，可讓您手動設定使用的資源**進階網路**索引標籤的 [顯示卡。 這類介面卡，您可以設定的參數，包括緩衝區接收的數字的數字的值，並傳送緩衝區。

使用下列的 Windows PowerShell cmdlet 已簡化設定網路介面卡的資源。

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

如需詳細資訊，請查看[Windows PowerShell 中的 [網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

本指南所有主題的連結，會看到[的網路效能子系統調整](net-sub-performance-top.md)。