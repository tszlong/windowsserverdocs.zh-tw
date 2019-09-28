---
title: 規劃 Windows Server 2016 中的 Hyper-v 擴充性
description: 列出您可以在 Hyper-v 和虛擬機器中新增或移除之元件的最大支援數目，像是多少記憶體和多少個虛擬處理器。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 3d94d8475f5de8d6b3d1d3f0bc549a8791e1d0c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364056"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>規劃 Windows Server 2016 中的 Hyper-v 擴充性

> 適用於：Windows Server 2016、Windows Server 2019
  
本文提供您可以在 Hyper-v 主機或其虛擬機器（例如虛擬處理器或檢查點）上新增和移除之元件最大設定的詳細資料。 當您規劃部署時，請考慮套用至每部虛擬機器的最大限制，以及適用于 Hyper-v 主機的上限。 

記憶體和邏輯處理器的最大數目，與 Windows Server 2012 的增加，是為了回應支援較新案例（例如機器學習和資料分析）的要求。 Windows Server blog 最近發佈了虛擬機器的效能結果，其中包含 5.5 tb 的記憶體和128虛擬處理器，並執行 4 TB 記憶體內部資料庫。 效能大於實體伺服器效能的 95%。 如需詳細資訊，請參閱[記憶體中交易處理的 Windows Server 2016 hyper-v 大型 VM 效能](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 其他數位與適用于 Windows Server 2012 的號碼類似。 Windows Server 2012 R2 的 @no__t 0Maximums 與 Windows Server 2012 相同。 \) 
  
> [!NOTE]  
> 如需有關 System Center 虛擬機器管理員 (VMM) 的資訊，請參閱 [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm)。 VMM 是 Microsoft 的產品，用來管理虛擬化的資料中心，此產品單獨銷售。  
  
## <a name="maximums-for-virtual-machines"></a>虛擬機器的最大上限  
這些最大上限適用于每部虛擬機器。 並非所有元件都可用於兩代的虛擬機器。 如需各層代的比較，請參閱[應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Component|最大值|注意|  
|-------------|-----------|---------|  
|檢查點|50|實際數字可能較低，取決於可用的存放裝置。 每個檢查點都會儲存為使用實體儲存體的 .avhd 檔案。|  
|記憶體|第2代的 12 TB; <br>1 TB 的層代1|檢閱特定作業系統的需求，以判定需求量下限及建議的需求量。|  
|序列 (COM) 埠|2|無。|  
|直接連到虛擬機器的實體磁碟大小|不定|大小上限取決於客體作業系統。|  
|虛擬光纖通道介面卡|4|我們建議的最佳做法是將每個虛擬光纖通道介面卡連線到不同的虛擬 SAN。|  
|虛擬磁碟片裝置|1 個虛擬軟碟機|無。|
|虛擬硬碟容量|適用于 VHDX 格式的 64 TB;<br>2040 GB 的 VHD 格式|每個虛擬硬碟會在實體媒體上儲存為.vhdx 或.vhd 檔案，視虛擬硬碟使用的格式而定。|  
|虛擬 IDE 磁碟|4|啟動磁片（有時稱為開機磁片）必須連接到其中一個 IDE 裝置。 啟動磁碟可以是虛擬硬碟，也可以是直接連到虛擬機器的實體磁碟。|  
|虛擬處理器|240代表層代 2;<br>適用于層代1的 64;<br>320可供主機 OS （根磁碟分割）使用|客體作業系統支援的虛擬處理器數目可能較低。 如需詳細資訊，請參閱針對特定作業系統發佈的資訊。|
|虛擬 SCSI 控制器|4|使用虛擬 SCSI 裝置需要整合服務，適用于支援的客體作業系統。 如需支援哪些作業系統的詳細資訊，請參閱[支援的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)和[支援的 Windows 客體作業系統](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)。|  
|虛擬 SCSI 磁碟|256|每個 SCSI 控制器最多支援 64 部磁碟，表示每個虛擬機器最多可設定為 256 部虛擬 SCSI 磁碟 (4 個控制器 x 每一控制器 64 部磁碟)。|  
|虛擬網路介面卡|Windows Server 2016 支援總共12個：<br> -8 個 hyper-v 專用網路介面卡<br>-4 傳統網路介面卡 <br> Windows Server 2019 支援總計68： <br> -64 hyper-v 專用網路介面卡<br>-4 傳統網路介面卡  |Hyper-v 特有的網路介面卡提供較佳的效能，而且需要整合服務中包含的驅動程式。 如需詳細資訊，請參閱[在 Windows Server 中規劃 hyper-v 網路功能](plan-hyper-v-networking-in-windows-server.md)。|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Hyper-v 主機的最大上限  
這些最大限制適用于每個 Hyper-v 主機。  
  
|Component|最大值|注意|  
|-------------|-----------|---------|  
|邏輯處理器|512|這兩個都必須在固件中啟用：<br /><br />-硬體輔助虛擬化<br />-硬體強制的資料執行防止（DEP）<br /><br />主機 OS （根磁碟分割）只會看到最大320的邏輯處理器|  
|記憶體|24 TB|無。|  
|網路介面卡小組 (NIC 小組)|Hyper-V 並無強制限制。|如需詳細資訊，請參閱[NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md)小組。|  
|實體網路介面卡|Hyper-V 並無強制限制。|無。|  
|每一伺服器執行的虛擬機器|1024|無。|  
|儲存體|受限於主機作業系統支援的功能。 Hyper-V 並無強制限制。|**注意：** 使用 SMB 3.0 時，Microsoft 支援網路連接儲存裝置（NAS）。 不支援 NFS 型存放裝置。|
|每一伺服器的虛擬網路交換器連接埠|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|  
|每一邏輯處理器的虛擬處理器|Hyper-V 並無強制比例。|無。|  
|每一伺服器的虛擬處理器|2048|無。|  
|虛擬存放區域網路 (SAN)|Hyper-V 並無強制限制。|無。|  
|虛擬交換器|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|  
 
## <a name="failover-clusters-and-hyper-v"></a>容錯移轉叢集與 Hyper-V  
下表列出使用 Hyper-v 和容錯移轉叢集時適用的最大上限。 請務必進行容量規劃，以確保有足夠的硬體資源可執行叢集環境中的所有虛擬機器。  

若要瞭解容錯移轉叢集的更新（包括虛擬機器的新功能），請參閱[Windows Server 2016 中容錯移轉叢集的新](../../../failover-clustering/whats-new-in-failover-clustering.md)功能。

|Component|最大值|注意|  
|-------------|-----------|---------|  
|每一叢集的節點|64|請考量要保留給容錯移轉的節點數目，以及例如套用更新等維護工作。 建議您規劃足夠的資源，將一個節點保留給容錯移轉；這表示該節點將維持閒置，直到其他節點容錯移轉至該節點為止 (這有時也稱為被動節點)。如果想要保留額外節點，則可增加此數字。 不建議將保留節點的比率或乘數用於作用中節點;唯一的要求是叢集中的節點總數不能超過最大值64。|  
|每個叢集的每個節點執行的虛擬機器|每個叢集 8000 個|有數個因素會影響您可以在一個節點上同時執行的實際虛擬機器數目，例如：<br />-每部虛擬機器正在使用的實體記憶體數量。<br />-網路和儲存體頻寬。<br />-磁片軸的數目，會影響磁片 i/o 效能。|  
  

