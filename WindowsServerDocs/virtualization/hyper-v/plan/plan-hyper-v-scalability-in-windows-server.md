---
title: 規劃 Windows Server 2016 中的 HYPER-V 延展性
description: 最多支援元件，您可以加入或移除 HYPER-V 和虛擬機器的數字的清單，例如記憶體和虛擬處理器數目。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 03a464269c8aea29c226dee776f0dfacfe48743a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850259"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>規劃 Windows Server 2016 中的 HYPER-V 延展性

> 適用於：Windows Server 2016
  
這篇文章會提供有關元件，您可以新增和移除 HYPER-V 主機或其虛擬機器上，例如虛擬處理器或檢查點的最大組態的詳細資料。 當您規劃部署時，請考慮套用至每個虛擬機器，以及套用至 HYPER-V 主機的最大值。 

記憶體與邏輯處理器的最大值是從 Windows Server 2012，最大增加以回應要求，以支援較新的案例，例如機器學習服務和資料分析。 Windows Server 部落格最近發佈的 5.5 tb 的記憶體和執行 4 TB 的記憶體內資料庫的 128 個虛擬處理器與虛擬機器的效能結果。 效能已超過 95%的實體伺服器的效能。 如需詳細資訊，請參閱 < [Windows Server 2016 HYPER-V 記憶體中的交易處理的大型 VM 效能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 其他數字都類似於適用於 Windows Server 2012。 \(適用於 Windows Server 2012 R2 的最大值是 Windows Server 2012 相同。\) 
  
> [!NOTE]  
> 如需有關 System Center 虛擬機器管理員 (VMM) 的資訊，請參閱 [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm)。 VMM 是 Microsoft 的產品，用來管理虛擬化的資料中心，此產品單獨銷售。  
  
## <a name="maximums-for-virtual-machines"></a>適用於虛擬機器的最大值  
這些最大值套用至每個虛擬機器。 並非所有的元件有兩代虛擬機器。 如需隨層代的比較，請參閱[應該建立 1 或 2 代虛擬機器在 HYPER-V 中？](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Component|最大值|附註|  
|-------------|-----------|---------|  
|檢查點|50|實際數字可能較低，取決於可用的存放裝置。 每個檢查點會儲存為.avhd 檔案，並使用實體的儲存體。|  
|記憶體|層代 2，12 TB <br>第 1 代的 1 TB|檢閱特定作業系統的需求，以判定需求量下限及建議的需求量。|  
|序列 (COM) 埠|2|無。|  
|直接連到虛擬機器的實體磁碟大小|不定|大小上限取決於客體作業系統。|  
|虛擬光纖通道介面卡|4|我們建議的最佳做法是將每個虛擬光纖通道介面卡連線到不同的虛擬 SAN。|  
|虛擬磁碟片裝置|1 個虛擬軟碟機|無。|
|虛擬硬碟容量|64 TB 的 VHDX 格式;<br>VHD 格式的 2040 GB|每個虛擬硬碟會在實體媒體上儲存為.vhdx 或.vhd 檔案，視虛擬硬碟使用的格式而定。|  
|虛擬 IDE 磁碟|4|啟動磁碟 （有時稱為開機磁片） 必須連接到其中一個 IDE 裝置。 啟動磁碟可以是虛擬硬碟，也可以是直接連到虛擬機器的實體磁碟。|  
|虛擬處理器|層代 2，針對 240<br>層代 1; 64<br>320 可用主機 OS （根磁碟分割）|客體作業系統支援的虛擬處理器數目可能較低。 如需詳細資訊，請參閱適用於特定作業系統發行的資訊。|
|虛擬 SCSI 控制器|4|使用虛擬 SCSI 裝置需要整合服務，這是適用於支援的客體作業系統。 支援作業系統的詳細資訊，請參閱[支援的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)並[支援的 Windows 客體作業系統](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)。|  
|虛擬 SCSI 磁碟|256|每個 SCSI 控制器最多支援 64 部磁碟，表示每個虛擬機器最多可設定為 256 部虛擬 SCSI 磁碟 (4 個控制器 x 每一控制器 64 部磁碟)。|  
|虛擬網路介面卡|總共 12 個：<br> -8 的 HYPER-V 特定網路介面卡<br>-4 的傳統網路介面卡|HYPER-V 特定網路介面卡提供更佳的效能，而且需要包含 integration services 中的驅動程式。 如需詳細資訊，請參閱 <<c0> [ 計劃的 Windows Server 中的 HYPER-V 網路功能](plan-hyper-v-networking-in-windows-server.md)。|  
  
## <a name="maximums-for-hyper-v-hosts"></a>HYPER-V 主機的最大值  
這些最大值套用至每個 Hyper-v 主機。  
  
|Component|最大值|附註|  
|-------------|-----------|---------|  
|邏輯處理器|512|必須在韌體中啟用這兩種：<br /><br />-硬體輔助虛擬化<br />硬體-強制執行的資料執行防止 (DEP)<br /><br />主機作業系統 （根磁碟分割） 只會看到最大的 320 個邏輯處理器|  
|記憶體|24 TB|無。|  
|網路介面卡小組 (NIC 小組)|Hyper-V 並無強制限制。|如需詳細資訊，請參閱 < [NIC 小組](../../../networking/technologies/nic-teaming/NIC-Teaming.md)。|  
|實體網路介面卡|Hyper-V 並無強制限制。|無。|  
|每一伺服器執行的虛擬機器|1024|無。|  
|儲存體|受限於所支援的主機作業系統。 Hyper-V 並無強制限制。|**注意：** 使用 SMB 3.0 時，Microsoft 會支援網路連接存放 (NAS)。 不支援 NFS 型存放裝置。|
|每一伺服器的虛擬網路交換器連接埠|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|  
|每一邏輯處理器的虛擬處理器|Hyper-V 並無強制比例。|無。|  
|每一伺服器的虛擬處理器|2048|無。|  
|虛擬存放區域網路 (SAN)|Hyper-V 並無強制限制。|無。|  
|虛擬交換器|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|  
 
## <a name="failover-clusters-and-hyper-v"></a>容錯移轉叢集與 Hyper-V  
下表列出使用 HYPER-V 與容錯移轉叢集時，適用的最大值。 請務必進行容量規劃，以確保會有足夠的硬體資源，在叢集環境中執行的所有虛擬機器。  

若要深入了解更新至容錯移轉叢集，包括新的功能對於虛擬機器，請參閱[What's New in Windows Server 2016 中容錯移轉叢集](../../../failover-clustering/whats-new-in-failover-clustering.md)。

|Component|最大值|附註|  
|-------------|-----------|---------|  
|每一叢集的節點|64|請考量要保留給容錯移轉的節點數目，以及例如套用更新等維護工作。 建議您規劃足夠的資源，將一個節點保留給容錯移轉；這表示該節點將維持閒置，直到其他節點容錯移轉至該節點為止 (這有時也稱為被動節點)。如果想要保留額外節點，則可增加此數字。 沒有任何建議的比率或倍數的保留節點與作用中的節點;唯一的需求是在叢集中的節點總數不得超過 64 的最大值。|  
|每個叢集的每個節點執行的虛擬機器|每個叢集 8000 個|數個因素會影響實際數目的虛擬機器，您可以執行一次一個節點，例如：<br />-每個虛擬機器所使用的實體記憶體數量。<br />-網路和儲存體頻寬。<br />-磁碟的磁針數，這會影響磁碟 I/O 效能。|  
  

