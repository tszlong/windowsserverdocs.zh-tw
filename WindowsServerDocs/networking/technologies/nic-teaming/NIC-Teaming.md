---
title: NIC 小組
description: 本主題提供 Windows Server 2016 中的 [網路介面卡 (NIC) Teaming 的概觀。
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>NIC 小組

>適用於：Windows Server（以每年次管道）、Windows Server 2016

本主題提供 Windows Server 2016 中的 [網路介面卡 (NIC) Teaming 的概觀。

> [!NOTE]  
> 本主題中，除了下列 NIC 小組 content 使用。  
>   
> - [NIC 小組中虛擬電腦與 #40;Vm 和 #41;](nict-vms.md)
> - [NIC 小組與區域網路與 #40;Vlan 和 #41;](nict-and-vlans.md)
> - [NIC 小組 MAC 位址使用與管理](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [疑難排解 NIC 小組](Troubleshooting-NIC-Teaming.md) 
> - [建立新的 NIC 團隊主機電腦上 VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [Windows PowerShell 中的 (NetLBFO) Cmdlet 的聯合 NIC](https://technet.microsoft.com/library/jj130849.aspx)
> - TechNet 主題館下載：[Windows Server 2016 網路介面卡、切換 Embedded 小組使用者指南](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>NIC 小組概觀  
NIC 小組可讓您一個和通知三十的兩個實體乙太網路介面卡之間一或多個軟體 virtual 網路介面卡插入群組。 這些 virtual 網路介面卡提供快的效能與網路介面卡失敗容錯。  
  
NIC 小組成員網路介面卡必須所有安裝在相同的實體主機電腦處於團隊。  
  
> [!NOTE]  
> NIC 團隊，其中包含一個網路介面卡不能提供負載平衡和容錯移轉。不過一個網路介面卡，您可以使用 NIC 小組的網路流量之分開時也會使用區域網路 (Vlan)。  
  
當您設定網路介面卡插入 NIC 團隊時，這些連接到 NIC 小組方案常見核心，會再顯示一或多個 virtual 介面卡（也稱為小組 Nic [tNICs] 或小組介面）作業系統。 Windows Server 2016 支援最多 32 個小組介面每個小組。 有各種不同的輸出的流量（載入）Nic 之間的演算法。  
  
下圖描述使用多個 tNICs NIC 團隊。  
  
![使用多個 tNICs NIC 小組](../../media/NIC-Teaming/nict_overview.jpg)  
  
此外，您可以在同一個開關切換至或切換不同連接您小組的 Nic。 如果您將 Nic 連接至不同的參數，這兩個參數必須在相同的子網路上。  
  
## <a name="bkmk_avail"></a>NIC 小組可用性  
NIC 小組已在 Windows Server 2016 的所有版本。 此外，您可以使用 Windows PowerShell 命令、遠端桌面與遠端伺服器管理工具來管理 NIC 小組的電腦正在執行的工具的支援 client 作業系統。  
  
## <a name="bkmk_nics"></a>適用於 NIC 小組的支援並不支援 Nic  
您可以使用任何乙太網路 NIC 已在 Windows Server 2016 NIC 小組通過（WHQL 測試）的 Windows 硬體資格和商標測試。  
  
下列 Nic 無法放在 NIC 團隊。  
  
-   HYPER-V Virtual 切換為 Nic 公開主機磁碟分割中的連接埠 HYPER-V virtual 網路介面卡。  
  
    > [!IMPORTANT]  
    > HYPER-V virtual Nic 主機磁碟分割 (vNICs) 中公開必須不放在團隊。 在 [所有設定] 或 [組合不支援小組的磁碟分割主機在 vNICs。 嘗試小組 vNICs 網路問題發生，可能會導致完全遺失的通訊。  
  
-   核心偵錯網路介面卡 (KDNIC)。  
  
-   Nic 所使用的網路開機。  
  
-   使用技術乙太網路，例如 WWAN、WLAN 日 Wi ‑ Fi、藍牙、和 Infiniband，包括網際網路通訊協定 (IPoIB) Infiniband Nic 透過以外的 Nic。  
  
## <a name="bkmk_compat"></a>NIC 小組的相容性  
NIC 小組適用於所有網路技術在 Windows Server 2016 例外如下。  
  
-   **單根 I/O 模擬 (SR IOV)**。 SR-IOV 的資料是直接傳送到而不需要它通過堆疊網路（以主機作業系統，在模擬）。 因此，不可能 NIC 團隊車載或重新導向至其他路徑小組中的資料。  
  
-   **原生主機品質服務 (QoS)**。 當 QoS 原則設定在原生或主機系統和那些原則叫用頻寬下限限制時，會 NIC 團隊的整體輸送量小於就地頻寬原則不會。  
  
-   **TCP Chimney**。 不支援 TCP Chimney NIC 小組因為 TCP Chimney 卸載整個網路堆疊直接 NIC  
  
-   **802.1 x 驗證**。 802.1 不應該使用 X 驗證 NIC 小組的。 有些參數不允許 802.1 X 驗證和 NIC 相同的連接埠的聯合的設定。  
  
若要了解如何使用 NIC 小組中虛擬電腦 (Vm) HYPER-V 主機上執行，請查看[NIC 小組中虛擬電腦與 #40;Vm 和 #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>NIC 小組與一樣佇列 (VMQs)  
VMQ 和 NIC 小組合作。應該支援 VMQ，隨時可以 HYPER-V。 根據切換設定模式，載入 distribution 演算法 NIC 小組將會顯示 VMQ 功能 HYPER-V 開關切換至顯示佇列提供給支援小組（分鐘-佇列模式）中的任何介面卡佇列數最小的數字或總數佇列可用的所有團隊成員（的佇列總和-模式）。  
  
具體而言，如果小組中切換獨立模式和載入 Distribution 的聯合設定為 [HYPER-V 連接埠模式或動態模式，則報告佇列數目的所有可用的小組成員（的佇列總和-模式）; 佇列總和否則佇列回報的是最小數目佇列支援任何（分鐘-佇列模式）小組的成員。  
  
原因如下：  
  
-   HYPER-V 連接埠模式」或「動態模式開關切換至獨立小組時輸入的流量的 HYPER-V 切換連接埠 (VM) 將永遠抵達上相同的小組成員。 主機可預測日控制的成員會接收流量特定 vm 這樣可以在特定的小組成員配置哪些 VMQ 佇列有關更重視 NIC 小組。 NIC 小組，使用 HYPER-V 切換，將 vm VMQ 設定一個小組成員，然後輸入的流量會叫用佇列。  
  
-   小組處於（靜態小組或 LACP 小組）任何切換相關模式小組已連接到切換控制輸入的流量分配。 主機 NIC 小組的軟體不能預測的小組成員將會取得輸入的流量 VM 並有可能，開關切換至分散的資料傳輸 vm 所有小組的成員。 為 NIC 小組軟體，使用 HYPER-V 開關切換至，會在每個小組成員 vm 程式佇列，而不只是其中一個小組成員。  
  
-   當您切換獨立模式中的小組，並使用位址 hash 載入 distribution 演算法時，輸入的流量永遠進上所有的上一個小組成員一個 NIC（主要小組成員-）。 因為它們取得的設計用輸入流量處理其他小組成員不相同佇列做為主要的成員，如果主要成員失敗任何其他團隊成員可以用來輸入流量挑選並佇列已經的地方。  
  
大多數 Nic 有可用於收到側邊縮放比例 (RSS) 或 VMQ，但不是能同時在此同時，佇列。 部分 VMQ 設定看起來似乎 RSS 佇列設定，但其實是設定 RSS 和 VMQ 使用根據的功能目前正在使用中的一般佇列。 每個 NIC 有中有進階屬性，值 * RssBaseProcNumber 和 \*MaxRssProcessors。 以下是一些 VMQ 設定，以提供更好的系統效能。  
  
-   每個 NIC 應該會有理想 * RssBaseProcNumber 為偶數字超過或等於兩個（2)。 這是因為的第一個實體處理器核心 0（邏輯處理器 0 和 1），通常會大部分的系統處理，因此應該會網路處理 steered 原位這個實體處理器。 （此電腦的基底處理器應大於或等於 1 讓某些電腦架構不需要兩個的邏輯處理器每個實體處理器。 如果有疑問取得您的主機使用每個實體處理器架構 2 邏輯處理器。）  
  
-   如果小組的佇列總和-模式中的小組成員處理器應該，範圍實際，不重疊。 例如的團隊的 2 10Gbps Nic 4 核心主機（8 邏輯處理器），您可以設定使用 2 的基底處理器並使用 4 核心; 的第一個第二個會設定為使用 [基本處理器 6 並使用 2 核心。  
  
-   如果最小值-佇列模式中的小組所使用的小組成員處理器集必須是相同。  
  
## <a name="bkmk_hnv"></a>NIC 小組與 HYPER-V 網路模擬 (HNV)  
NIC 小組已完全相容的 HYPER-V 網路模擬 (HNV)。  HNV 管理系統提供的資訊，可將最適合 HNV 資料傳輸方式中的載入的聯合 NIC NIC 小組驅動程式。  
  
## <a name="bkmk_live"></a>NIC 小組與即時移轉  
NIC 小組，在 Vm 中不會影響 Live 移轉。 Live 移轉 NIC 小組已在 VM 中有相同的規則。  
  
## <a name="see-also"></a>也了  
[NIC 小組中虛擬電腦與 #40;Vm 和 #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


