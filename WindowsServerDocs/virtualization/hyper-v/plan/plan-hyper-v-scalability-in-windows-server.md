---
title: 規劃 Windows Server 2016 和 Windows Server 2019 中的 Hyper-v 擴充性
description: 列出您可以在 Hyper-v 和虛擬機器中新增或移除之元件的最大支援數目，例如記憶體數量和虛擬處理器數目。
ms.topic: article
ms.author: benarm
author: BenjaminArmstrong
ms.date: 09/28/2016
ms.openlocfilehash: 2e7eecbf68a8b08caae2851bce45673ebb09bcef
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745923"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016-and-windows-server-2019"></a>規劃 Windows Server 2016 和 Windows Server 2019 中的 Hyper-v 擴充性

> 適用於：Windows Server 2016、Windows Server 2019

本文提供您可以在 Hyper-v 主機或其虛擬機器（例如虛擬處理器或檢查點）上新增和移除之元件的最大設定的詳細資料。 當您規劃部署時，請考慮適用于每部虛擬機器的最大限制，以及適用于 Hyper-v 主機的最大限制。

記憶體和邏輯處理器最大的增加，與 Windows Server 2012 的增加，是為了回應支援較新案例的要求，例如機器學習和資料分析。 Windows Server blog 最近發行了虛擬機器的效能結果，此虛擬機器具有 5.5 tb 的記憶體和128個虛擬處理器，執行 4 TB 的記憶體內部資料庫。 效能高於實體伺服器效能的95%。 如需詳細資訊，請參閱 [Windows Server 2016 hyper-v 大型 VM 效能以進行記憶體中的交易處理](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/)。 其他數位與適用于 Windows Server 2012 的數位類似。 \(Windows Server 2012 R2 的最大可與 Windows Server 2012 相同。\)

> [!NOTE]
> 如需有關 System Center 虛擬機器管理員 (VMM) 的資訊，請參閱 [Virtual Machine Manager](/system-center/vmm/overview?view=sc-vmm-2019)。 VMM 是 Microsoft 的產品，用來管理虛擬化的資料中心，此產品單獨銷售。

## <a name="maximums-for-virtual-machines"></a>虛擬機器的最大
這些上限適用于每部虛擬機器。 並非所有元件都適用于兩個世代的虛擬機器。 如需有關世代的比較，請參閱 [我應該在 hyper-v 中建立第1代或第2代虛擬機器嗎？](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md)

|元件|最大值|備註|
|-------------|-----------|---------|
|檢查點|50|實際數字可能較低，取決於可用的存放裝置。 每個檢查點都會儲存為使用實體儲存體的 .avhd 檔案。|
|記憶體|第2代的 12 TB; <br>1 TB （代1）|檢閱特定作業系統的需求，以判定需求量下限及建議的需求量。|
|序列 (COM) 埠|2|無。|
|直接連到虛擬機器的實體磁碟大小|不定|大小上限取決於客體作業系統。|
|虛擬光纖通道介面卡|4|我們建議的最佳做法是將每個虛擬光纖通道介面卡連線到不同的虛擬 SAN。|
|虛擬磁碟片裝置|1 個虛擬軟碟機|無。|
|虛擬硬碟容量|適用于 VHDX 格式的 64 TB;<br>2040 GB （適用于 VHD 格式）|每個虛擬硬碟會在實體媒體上儲存為.vhdx 或.vhd 檔案，視虛擬硬碟使用的格式而定。|
|虛擬 IDE 磁碟|4|啟動磁片 (有時稱為開機磁片) 必須附加至其中一個 IDE 裝置。 啟動磁碟可以是虛擬硬碟，也可以是直接連到虛擬機器的實體磁碟。|
|虛擬處理器|適用于層代2的 240;<br>適用于層代1的 64;<br>320適用于主機作業系統 (根磁碟分割) |客體作業系統支援的虛擬處理器數目可能較低。 如需詳細資訊，請參閱針對特定作業系統發佈的資訊。|
|虛擬 SCSI 控制器|4|使用虛擬 SCSI 裝置需要整合服務，這些服務適用于支援的客體作業系統。 如需支援哪些作業系統的詳細資訊，請參閱 [支援的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) ，以及 [支援的 Windows 客體作業系統](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md)。|
|虛擬 SCSI 磁碟|256|每個 SCSI 控制器最多支援 64 部磁碟，表示每個虛擬機器最多可設定為 256 部虛擬 SCSI 磁碟 (4 個控制器 x 每一控制器 64 部磁碟)。|
|虛擬網路介面卡|Windows Server 2016 支援總計12：<br> -8 hyper-v 特定網路介面卡<br>-4 傳統網路介面卡 <br> Windows Server 2019 支援 68 total： <br> -64 hyper-v 特定網路介面卡<br>-4 傳統網路介面卡  |Hyper-v 特定網路介面卡可提供較佳的效能，而且需要整合服務中包含的驅動程式。 如需詳細資訊，請參閱 [規劃 Windows Server 中的 hyper-v 網路功能](plan-hyper-v-networking-in-windows-server.md)。|

## <a name="maximums-for-hyper-v-hosts"></a>最大的 Hyper-v 主機
這些上限適用于每部 Hyper-v 主機。

|元件|最大值|備註|
|-------------|-----------|---------|
|邏輯處理器|512|這兩個都必須在固件中啟用：<p>-硬體輔助虛擬化<br />-硬體-強制執行的資料執行防止 (DEP) <p>主機作業系統 (根磁碟分割) 只會看到最大320的邏輯處理器|  
|記憶體|24 TB|無。|
|網路介面卡小組 (NIC 小組)|Hyper-V 並無強制限制。|如需詳細資訊，請參閱 [NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md)小組。|
|實體網路介面卡|Hyper-V 並無強制限制。|無。|
|每一伺服器執行的虛擬機器|1024|無。|
|儲存體|受限於主機作業系統所支援的功能。 Hyper-V 並無強制限制。|**注意：** 使用 SMB 3.0 時，Microsoft 支援 (NAS) 的網路連接存放裝置。 不支援 NFS 型存放裝置。|
|每一伺服器的虛擬網路交換器連接埠|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|
|每一邏輯處理器的虛擬處理器|Hyper-V 並無強制比例。|無。|
|每一伺服器的虛擬處理器|2048|無。|
|虛擬存放區域網路 (SAN)|Hyper-V 並無強制限制。|無。|
|虛擬交換器|各有不同；Hyper-V 並無強制限制。|實際限制取決於可用的運算資源。|

## <a name="failover-clusters-and-hyper-v"></a>容錯移轉叢集與 Hyper-V
下表列出使用 Hyper-v 和容錯移轉叢集時適用的最大限制。 請務必執行容量規劃，以確保有足夠的硬體資源可在叢集環境中執行所有虛擬機器。

若要深入瞭解容錯移轉叢集的更新，包括虛擬機器的新功能，請參閱 [Windows Server 2016 中容錯移轉叢集的新](../../../failover-clustering/whats-new-in-failover-clustering.md)功能。

|元件|最大值|備註|
|-------------|-----------|---------|
|每一叢集的節點|64|請考量要保留給容錯移轉的節點數目，以及例如套用更新等維護工作。 建議您規劃足夠的資源，將一個節點保留給容錯移轉；這表示該節點將維持閒置，直到其他節點容錯移轉至該節點為止  (這有時稱為被動節點 ) 。如果您想要保留額外的節點，則可以增加此數目。 使用中節點沒有保留節點的建議比率或乘數;唯一的需求是叢集中的節點總數不能超過最大值64。|
|每個叢集的每個節點執行的虛擬機器|每個叢集 8000 個|有幾個因素會影響您可以在一個節點上同時執行的實際虛擬機器數目，例如：<br />-每部虛擬機器正在使用的實體記憶體數量。<br />-網路和儲存體頻寬。<br />-磁片主軸數目，會影響磁片 i/o 效能。|