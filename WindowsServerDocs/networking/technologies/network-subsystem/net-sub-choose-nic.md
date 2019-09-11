---
title: 選擇網路介面卡
description: 本主題是 Windows Server 2016 的網路子系統效能微調指南的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 19318bfa9807d209bd9a195b668c1787bd3aff25
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871924"
---
# <a name="choosing-a-network-adapter"></a>選擇網路介面卡

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解可能會影響購買選擇之網路介面卡的一些功能。

需要大量網路的應用程式需要高效能的網路介面卡。 本節將探討選擇網路介面卡的一些考慮，以及如何設定不同的網路介面卡設定，以達到最佳的網路效能。

> [!TIP]
>  您可以使用 Windows PowerShell 來設定網路介面卡。 如需詳細資訊，請參閱[Windows PowerShell 中的網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

##  <a name="bkmk_offload"></a>卸載功能

將工作從中央處理器\(CPU\)卸載到網路介面卡，可以減少伺服器上的 cpu 使用量，進而提升整體系統效能。

如果您選取具有適當卸載功能的網路介面卡，Microsoft 產品中的網路堆疊可以將一或多個工作卸載到網路介面卡。 下表簡要介紹 Windows Server 2016 中可用的各種卸載功能。
  
|卸載類型|描述|
|------------------|-----------------|  
|TCP 的總和檢查碼計算|網路堆疊可以在傳送和接收程式碼路徑上，卸載\(傳輸\)控制通訊協定 TCP 總和檢查碼的計算和驗證。 它也可以在傳送和接收程式碼路徑上，卸載 IPv4 和 IPv6 總和檢查碼的計算和驗證。|  
|UDP 的總和檢查碼計算 |網路堆疊可以在傳送和接收程式碼路徑上，卸載\(使用者\)資料包協定 UDP 總和檢查碼的計算和驗證。|
|IPv4 的總和檢查碼計算 |網路堆疊可以在傳送和接收程式碼路徑上卸載 IPv4 總和檢查碼的計算和驗證。 |
|IPv6 的總和檢查碼計算 |網路堆疊可以在傳送和接收程式碼路徑上卸載 IPv6 總和檢查碼的計算和驗證。 | 
|大型 TCP 封包的分割|TCP/IP 傳輸層支援大型傳送卸載 v2 （LSOv2）。 透過 LSOv2，TCP/IP 傳輸層可以將大型 TCP 封包的分割卸載到網路介面卡。|  
|接收端調整\(RSS\)|RSS 是一種網路驅動程式技術，可讓您在多處理器系統中，有效率地將網路接收處理散發到多個 Cpu。 本主題稍後會提供更多有關 RSS 的詳細資料。|  
|接收區段\(聯合 RSC\)|RSC 能夠將封包群組在一起，以最小化執行主機所需的標頭處理。 最多可將 64 KB 的已接收承載結合成一個較大的封包以進行處理。 本主題稍後會提供有關 RSC 的更多詳細資料。|  
  
###  <a name="bkmk_rss"></a>接收端調整

Windows server 2016、windows server 2012、windows server 2012 R2、windows server 2008 R2 和 windows server 2008 支援接收端調整\(RSS。\) 

某些伺服器會設定多個邏輯處理器，以共用硬體\(資源（例如實體核心\) ），並被視為同時多執行緒\(SMT\)對等。 Intel 超執行緒技術就是一個例子。 RSS 會將網路處理導向每個核心最多一個邏輯處理器。 例如，在具有 Intel 超執行緒、4核心和8個邏輯處理器的伺服器上，RSS 不會使用4個以上的邏輯處理器來進行網路處理。  

RSS 會將傳入的網路 i/o 封包分散到邏輯處理器，以便在相同的邏輯處理器上處理屬於相同 TCP 連線的封包，以保留順序。 

RSS 也會對 UDP 單播和多播流量進行負載平衡，並將\(來源和目的地位址\)雜湊所決定的相關流程路由傳送至相同的邏輯處理器，以保留相關抵達的順序。 這有助於改善接收密集型案例的擴充性和效能，這些伺服器的網路介面卡比執行合格的邏輯處理器少。 

#### <a name="configuring-rss"></a>設定 RSS

在 Windows Server 2016 中，您可以使用 Windows PowerShell Cmdlet 和 RSS 設定檔來設定 RSS。 

您可以使用**Set-netadapterrss** Windows PowerShell Cmdlet 的 **– Profile**參數來定義 RSS 設定檔。

**適用于 RSS 設定的 Windows PowerShell 命令**

下列 Cmdlet 可讓您查看和修改每個網路介面卡的 RSS 參數。
  
>[!NOTE]
>如需每個 Cmdlet 的詳細命令參考，包括語法和參數，您可以按一下下列連結。 此外，您可以在 Windows PowerShell 提示字元中傳遞 Cmdlet 名稱，以**取得**每個命令的詳細資料。  

- [停用-set-netadapterrss](https://technet.microsoft.com/library/jj130892)。 此命令會在您指定的網路介面卡上停用 RSS。

- [啟用-set-netadapterrss](https://technet.microsoft.com/library/jj130859)。 此命令會在您指定的網路介面卡上啟用 RSS。
  
- [Set-netadapterrss](https://technet.microsoft.com/library/jj130912)。 此命令會抓取您指定之網路介面卡的 RSS 屬性。
  
- [Set-netadapterrss](https://technet.microsoft.com/library/jj130863)。 此命令會在您指定的網路介面卡上設定 RSS 屬性。  

#### <a name="rss-profiles"></a>RSS 設定檔

您可以使用 Set-netadapterrss Cmdlet 的 **– Profile**參數，指定要將哪些邏輯處理器指派給哪一個網路介面卡。 此參數的可用值為：

- **最接近**。 偏好位於網路介面卡基底 RSS 處理器附近的邏輯處理器編號。 使用此設定檔，作業系統可能會根據負載動態地重新平衡邏輯處理器。
  
- **ClosestStatic**。 偏好網路介面卡基底 RSS 處理器附近的邏輯處理器編號。 使用此設定檔時，作業系統不會根據負載動態地重新平衡邏輯處理器。
  
- **NUMA**。 通常會在不同的 NUMA 節點上選取邏輯處理器編號，以分散負載。 使用此設定檔，作業系統可能會根據負載動態地重新平衡邏輯處理器。
  
- **NUMAStatic**。 這是**預設設定檔**。 通常會在不同的 NUMA 節點上選取邏輯處理器編號，以分散負載。 使用此設定檔時，作業系統將不會根據負載動態重新平衡邏輯處理器。

- **保守**。 RSS 會盡可能使用最少的處理器來承受負載。 此選項可協助減少中斷的次數。

根據案例和工作負載特性而定，您也可以使用**Set-netadapterrss** Windows PowerShell Cmdlet 的其他參數來指定下列各項：

- 以每個網路介面卡為基礎，可用於 RSS 的邏輯處理器數目。
- 邏輯處理器範圍的起始位移。
- 網路介面卡配置記憶體的來源節點。

以下是您可以用來設定 RSS 的額外**set-netadapterrss**參數：

>[!NOTE]
>在以下每個參數的範例語法中，網路介面卡名稱**Ethernet**會當做**set-netadapterrss**命令的 **– name**參數的範例值使用。 當您執行 Cmdlet 時，請確定您使用的網路介面卡名稱適用于您的環境。

- **MaxProcessors\*** ：設定要使用的 RSS 處理器最大數目。 這可確保應用程式流量會系結至指定介面上的處理器數目上限。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **BaseProcessorGroup\*** ：設定 NUMA 節點的基礎處理器群組。 這會影響 RSS 所使用的處理器陣列。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **MaxProcessorGroup\*** ：設定 NUMA 節點的最大處理器群組。 這會影響 RSS 所使用的處理器陣列。 設定此項會限制最大處理器群組，讓負載平衡在 k 群組內對齊。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **BaseProcessorNumber\*** ：設定 NUMA 節點的基本處理器號碼。 這會影響 RSS 所使用的處理器陣列。 這可讓您跨網路介面卡分割處理器。 這是指派給每個介面卡的 RSS 處理器範圍中的第一個邏輯處理器。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **NumaNode\*** ：每個網路介面卡可以配置記憶體的 NUMA 節點。 這可以在 k 群組中，或來自不同的 k 群組。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **NumberofReceiveQueues\*** ：如果您的邏輯處理器似乎未充分利用以接收\(流量（例如在工作管理員\)中查看），您可以嘗試將 RSS 佇列的數目從預設值2增加到網路介面卡所支援的最大值. 您的網路介面卡可能有選項可將 RSS 佇列的數目變更為驅動程式的一部分。 範例語法：

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

如需詳細資訊，請按一下下列連結以[下載可調整的網路功能：排除接收處理瓶頸--Word 格式](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc)的 RSS 簡介。
  
#### <a name="understanding-rss-performance"></a>瞭解 RSS 效能

調整 RSS 需要瞭解設定和負載平衡邏輯。 若要確認 RSS 設定已生效，您可以在執行**Set-netadapterrss** Windows PowerShell Cmdlet 時，檢查輸出。 以下是此 Cmdlet 的範例輸出。
  
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

除了已設定的回應參數之外，輸出的主要層面是間接取值表輸出。 間接取值表會顯示用來散發連入流量的雜湊表值區。 在此範例中，n:c 標記法會指定用來引導傳入流量的 Numa K 群組： CPU 索引配對。 我們會看到正好2個唯一專案（0:0 和0:4），分別代表 k-群組 0/cpu0 和 k-群組 0/cpu 4。

此系統只有一個 k 群組（k-群組0）和 n （其中 n < = 128）間接資料表專案。 因為接收佇列的數目設定為2，所以只會選擇2個處理器（0:0、0:4），即使最大處理器是設定為8。 實際上，間接取值資料表會雜湊傳入流量，只使用8個可用的 Cpu。

若要充分利用 Cpu，RSS 接收佇列的數目必須等於或大於最大處理器。 在上述範例中，接收佇列應該設定為8或更大。

#### <a name="nic-teaming-and-rss"></a>NIC 小組和 RSS

可以在使用 NIC 小組與另一個網路介面卡組合的網路介面卡上啟用 RSS。 在此案例中，只有基礎實體網路介面卡可以設定為使用 RSS。 使用者無法在組合的網路介面卡上設定 RSS Cmdlet。
  
###  <a name="bkmk_rsc"></a>接收區段聯合（RSC）

接收區段\(聯合 RSC\)藉由減少針對指定的已接收資料量所處理的 IP 標頭數目，來協助效能。 它應該用來將較小的封包分組\( \)或結合成較大的單位，以協助調整已接收資料的效能。

這種方法可能會影響輸送量增益中大部分所見到的延遲。 建議使用 RSC 來增加所接收之繁重工作負載的輸送量。 請考慮部署支援 RSC 的網路介面卡。 

在這些網路介面卡上，請確定 rsc 是\( \)預設值，除非您有特定的工作負載\(，例如，低延遲、低輸送量的網路\) ，顯示從 RSC 關閉的好處.

#### <a name="understanding-rsc-diagnostics"></a>瞭解 RSC 診斷

您可以使用 Windows PowerShell Cmdlet **NetAdapterRsc**和**NETADAPTERSTATISTICS**來診斷 RSC。

以下是當您執行 NetAdapterRsc Cmdlet 時的範例輸出。

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

**Get** Cmdlet 會顯示介面中是否已啟用 rsc，以及 TCP 是否讓 rsc 處於操作狀態。 失敗原因會提供有關無法在該介面上啟用 RSC 之失敗的詳細資料。

在先前的案例中，IPv4 RSC 在介面中是受支援且可運作的。 若要瞭解診斷失敗，可以看到引發的合併位元組或例外狀況。 這提供了聯合問題的指示。

以下是當您執行 NetAdapterStatistics Cmdlet 時的範例輸出。

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC 和虛擬化

只有當主機網路介面卡未系結至 Hyper-v 虛擬交換器時，實體主機才支援 RSC。 當主機系結至 Hyper-v 虛擬交換器時，作業系統會停用 RSC。 此外，虛擬機器不會獲得 RSC 的優點，因為虛擬網路介面卡不支援 RSC。

啟用單一根目錄輸入/輸出虛擬化\(sr-iov\)時，可以為虛擬機器啟用 RSC。 在此情況下，虛擬函式支援 RSC 功能;因此，虛擬機器也會獲得 RSC 的優勢。

##  <a name="bkmk_resources"></a>網路介面卡資源

有些網路介面卡會主動管理其資源，以達到最佳效能。 數個網路介面卡可讓您使用介面卡的 [ **Advanced** network] 索引標籤手動設定資源。 針對這類介面卡，您可以設定多個參數的值，包括接收緩衝區的數目和傳送緩衝區。

使用下列 Windows PowerShell Cmdlet 可簡化設定網路介面卡資源。

- [NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [設定-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [啟用-Get-netadapter](https://technet.microsoft.com/library/jj130876.aspx)

- [啟用-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [啟用-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [啟用-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [啟用-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [啟用-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [啟用-Get-netadapterqos](https://technet.microsoft.com/library/jj130866.aspx)

- [啟用-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [啟用-Get-netadaptersriov](https://technet.microsoft.com/library/jj130899.aspx)

如需詳細資訊，請參閱[Windows PowerShell 中的網路介面卡 Cmdlet](https://technet.microsoft.com/library/jj134956.aspx)。

如需本指南中所有主題的連結，請參閱[網路子系統效能調整](net-sub-performance-top.md)。