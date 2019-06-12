---
title: 軟體和硬體 (SH) 整合功能和技術
description: 這些功能有軟體和硬體元件。 軟體會緊密繫結所需的功能運作的硬體功能。 其中的範例包括 VMMQ、 VMQ，傳送端 IPv4 總和檢查碼卸載、 和 RSS。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/12/2018
ms.openlocfilehash: 98857a5a6d665728c1aab2a6a2df64997d4166b0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446220"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>軟體和硬體 (SH) 整合功能和技術

這些功能有軟體和硬體元件。 軟體會緊密繫結所需的功能運作的硬體功能。 其中的範例包括 VMMQ、 VMQ，傳送端 IPv4 總和檢查碼卸載、 和 RSS。

>[!TIP]
>如果已安裝的 NIC 支援此提供 SH 和主控功能。 下列功能描述將討論如何判斷您的 NIC 是否支援此功能。

## <a name="converged-nic"></a>交集的 NIC 

交集的 NIC 是一種技術，可讓 HYPER-V 主機，以公開 RDMA 服務主機處理程序中的虛擬 Nic。 Windows Server 2016 已不再需要個別的 Nic 的 RDMA。 交集的 NIC 功能可讓虛擬 Nic，主應用程式在資料分割中 (Vnic) 公開 （expose） 至主機分割區的 RDMA 與合理且容易管理的方式共用之間的 RDMA 流量和 VM 和其他的 TCP/UDP 流量的 Nic 的頻寬。

![使用 SDN 交集的 NIC](../../media/Converged-NIC/conv-nic-sdn.png)

您可以管理交集的 NIC 作業，透過 VMM 或 Windows PowerShell。 PowerShell cmdlet 是使用 RDMA （如下所示） 相同的 cmdlet。

若要使用的交集的 NIC 功能：

1.  請確定將 DCB 註冊主機。

2.  若要啟用 RDMA nic，請確定，或將小組設定，在 Nic 繫結至 HYPER-V 交換器。 

3.  請確定在主機中指定的 RDMA Vnic 上啟用 RDMA。 

如需進一步瞭解 RDMA 和設定的詳細資訊，請參閱[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="data-center-bridging-dcb"></a>資料中心橋接 (DCB) 

DCB 是一套美國電機暨電子工程師學會 (IEEE) 標準，讓聚合式資料中心內的網狀架構。 DCB 提供合作相鄰的交換器的主機中的硬體佇列為主的頻寬管理。 儲存體的所有流量，網路、 叢集進行處理序間通訊 (IPC) 和管理都共用資料相同的乙太網路基礎結構。 在 Windows Server 2016 中，DCB 可以個別套用至任何 NIC 和 nic 繫結至 HYPER-V 交換器。

DCB，Windows Server 會使用優先順序型流量控制 (PFC)，於 IEEE 802.1Qbb 標準化。 PFC 會藉由防止溢位的流量類別內建立的不失真 （幾乎） 網路網狀架構。 Windows Server 也會使用增強的傳輸選取項目 (ETS)，於 IEEE 802.1Qaz 標準化。 ETS 啟用頻寬分成保留部分的流量的最多八個類別。 每個流量類別有它自己的傳輸佇列，並透過使用 PFC，可以啟動和停止類別內的傳輸。

如需詳細資訊，請參閱 <<c0> [ 資料中心橋接 (DCB)](https://docs.microsoft.com/windows-server/networking/technologies/dcb/dcb-top)。

## <a name="hyper-v-network-virtualization"></a>Hyper-V 網路虛擬化

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1)**       |                     Windows Server 2012 中引入，HYPER-V 網路虛擬化 (HNV) 可讓共用的實體網路基礎結構之上將客戶網路虛擬化。 幾乎不需要變更需要在實體網路網狀架構上，HNV 讓服務提供者來部署和跨三個雲端的任何位置移轉租用戶工作負載彈性： 服務提供者雲端、 私人雲端或 Microsoft Azure 公用雲端。                     |
| **v2 NVGRE (HNVv2 NVGRE)** | 在 Windows Server 2016 和 System Center Virtual Machine Manager 中，Microsoft 會提供一個端對端網路虛擬化解決方案，可包含 RAS 閘道、 軟體負載平衡、 網路控制站，以及更多。 如需詳細資訊，請參閱 < [Windows Server 2016 中的 HYPER-V 網路虛擬化概觀](https://technet.microsoft.com/windows-server-docs/networking/sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server)。 |
| **v2 VxLAN (HNVv2 VxLAN)** |                                                                                                                                                                                        在 Windows Server 2016 中，是 SDN-擴充功能，您透過網路控制卡管理的一部分。                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a>IPsec 工作卸載 (IPsecTO) 

IPsec 工作卸載是 NIC 功能可讓作業系統在 NIC 上的處理器用於 IPsec 加密工作。

>[!IMPORTANT] 
>IPsec 工作卸載舊的技術，不支援大部分的網路介面卡，而其中存在，它預設會停用。

## <a name="private-virtual-local-area-network-pvlan"></a>私人虛擬區域網路 (PVLAN)。 

Pvlan 可讓您只會在相同的虛擬化伺服器上的虛擬機器之間的通訊。 私人虛擬網路未繫結至實體網路介面卡。 私人虛擬網路是隔離的虛擬化伺服器上，所有外部網路流量，以及管理作業系統和外部網路之間的任何網路流量。 當您需要建立隔離的網路環境 (例如隔離的測試網域) 時，此類型的網路相當實用。 HYPER-V 和 SDN 堆疊支援僅限 PVLAN 隔離連接埠模式。

如需 PVLAN 隔離的詳細資訊，請參閱[System Center:Virtual Machine Manager 工程部落格](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)。

## <a name="remote-direct-memory-access-rdma"></a>遠端直接記憶體存取 (RDMA) 

RDMA 是一種網路技術，可提供高輸送量、 低延遲降至最低的 CPU 使用量的通訊。 RDMA 支援零複製網路啟用網路介面卡，將資料直接從應用程式的記憶體。 具備 RDMA 功能表示 NIC （實體或虛擬） 可以公開至 RDMA 的用戶端的 RDMA。 相反地，RDMA 功能，表示具備 RDMA 功能的 NIC 所公開的堆疊上的 RDMA 介面。

如需進一步瞭解 RDMA 的詳細資訊，請參閱[遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)。

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS) 

RSS 是 NIC 功能，以隔離不同的資料流，並將其交給不同的處理器進行處理。 RSS 平行處理的網路處理，可讓主應用程式調整為非常高的資料速率。 

如需詳細資訊，請參閱 <<c0> [ 接收端調整 (RSS)](https://docs.microsoft.com/windows-hardware/drivers/network/introduction-to-receive-side-scaling)。

## <a name="single-root-input-output-virtualization-sr-iov"></a>單一根目錄輸入輸出虛擬化 (SR-IOV) 

SR-IOV 可讓 VM 直接從 NIC 移至 VM 而不會經過 HYPER-V 主機的流量。 SR-IOV 是一項絕佳改善 vm 效能，但缺少供主機用來管理該管道的能力。 只能使用 SR-IOV 時工作負載行為良好，信任的網站和一般的唯一 VM 主應用程式中。

使用 SR-IOV 的流量都會略過 HYPER-V 交換器，這表示任何原則，比方說，Acl，或將不會套用頻寬管理。 SR-IOV 流量也不能被傳遞給任何網路虛擬化功能，因此無法套用內華達州拉斯維加斯 GRE 或 VxLAN 封裝。 只在特定情況中的受信任工作負載使用 SR-IOV。 此外，您無法使用主應用程式原則、 頻寬管理，以及虛擬化技術。

在未來，這兩項技術可讓 SR-IOV:一般流程資料表 (GFT) 和硬體 QoS 卸載 （頻寬中的管理 NIC） – 一旦我們生態系統中的 Nic 支援它們。 這兩項技術的組合會使 SR-IOV 適用於所有的 Vm，可讓原則、 虛擬化，以及要套用的頻寬管理規則，並可能導致絕佳 leaps 轉送的一般應用程式的 SR-IOV。

如需詳細資訊，請參閱 <<c0> [ 概觀的單一根目錄 I/O 虛擬化 (SR-IOV)](https://docs.microsoft.com/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-)。

## <a name="tcp-chimney-offload"></a>TCP Chimney 卸載

TCP Chimney 卸載，也稱為 TCP 引擎卸載 (TOC)，是一種技術，可讓主應用程式卸載至 nic。 處理的所有 TCP 因為 Windows Server 的 TCP 堆疊幾乎總是會比較比 TOE 引擎有效率，TCP Chimney 卸載不建議使用。

>[!IMPORTANT]
>TCP Chimney 卸載是已被取代的技術。 我們建議您不要使用 TCP Chimney 卸載，Microsoft 可能會停止在未來支援它。

## <a name="virtual-local-area-network-vlan"></a>虛擬區域網路 (VLAN) 

VLAN 是乙太網路框架標頭，若要啟用 LAN 分割為多個 Vlan，每個使用它自己的位址空間的擴充功能。 Windows Server 2016 中的 HYPER-V 交換器，或藉由在 NIC 小組的小組設定小組介面連接埠上設定 Vlan。 如需詳細資訊，請參閱 < [NIC 小組與虛擬區域網路 (Vlan)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-and-vlans)。

## <a name="virtual-machine-queue-vmq"></a>虛擬機器佇列 (VMQ) 

Vmq 是 NIC 功能可針對每個 VM 配置的佇列。 每當您有已啟用; HYPER-V您也必須啟用 VMQ。 在 Windows Server 2016 中，Vmq 使用 NIC 交換器 vPorts 與指派給 vPort 單一佇列提供相同的功能。 如需詳細資訊，請參閱 <<c0> [ 虛擬接收端調整 (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top)並[NIC 小組](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming)。

## <a name="virtual-machine-multi-queue-vmmq"></a>虛擬機器多佇列 (VMMQ) 

VMMQ 是 NIC 功能可讓 vm 分散到多個佇列，每個不同的實體處理器所處理的流量。 流量接著會傳遞至 VM 中的多個 LPs，因為它可以在 vRSS，是用來提供大量的網路頻寬的 vm。

---