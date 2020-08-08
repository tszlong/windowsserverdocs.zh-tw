---
title: 軟體和硬體 (SH) 整合功能和技術
description: 這些功能都具有軟體和硬體元件。 軟體的熟知系結至功能所需的硬體功能。 其中的範例包括 VMMQ、VMQ、傳送端 IPv4 總和檢查碼卸載和 RSS。
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/12/2018
ms.openlocfilehash: 3c1b414acaf7487b0a435cfea2891903646c869f
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995263"
---
# <a name="software-and-hardware-sh-integrated-features-and-technologies"></a>軟體和硬體 (SH) 整合功能和技術

這些功能都具有軟體和硬體元件。 軟體的熟知系結至功能所需的硬體功能。 其中的範例包括 VMMQ、VMQ、傳送端 IPv4 總和檢查碼卸載和 RSS。

>[!TIP]
>如果已安裝的 NIC 支援 SH 和 HO 功能，則可以使用。 下面的功能描述將涵蓋如何判斷您的 NIC 是否支援此功能。

## <a name="converged-nic"></a>聚合式 NIC

交集 NIC 是一種技術，可讓 Hyper-v 主機中的虛擬 Nic 將 RDMA 服務公開給主機進程。 Windows Server 2016 不再需要個別的 Nic 來進行 RDMA。 交集式 NIC 功能可讓主機分割區中的虛擬 Nic (Vnic) 向主機分割區公開 RDMA，並以公平且可管理的方式，共用 RDMA 流量與 VM 與其他 TCP/UDP 流量之間的 Nic 頻寬。

![具有 SDN 的聚合式 NIC](../../media/Converged-NIC/conv-nic-sdn.png)

您可以透過 VMM 或 Windows PowerShell 來管理聚合式 NIC 操作。 PowerShell Cmdlet 是用於 RDMA (的相同 Cmdlet，請參閱以下) 。

若要使用聚合式 NIC 功能：

1.  請務必將主機設定為 DCB。

2.  請務必在 NIC 上啟用 RDMA，或在設定小組的案例中，將 Nic 系結至 Hyper-v 交換器。

3.  請務必在針對主機中的 RDMA 指定的 Vnic 上啟用 RDMA。

如需 RDMA 和設定的詳細資訊，請參閱[ (rdma 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="data-center-bridging-dcb"></a>資料中心橋接 (DCB)

DCB 是一組電氣和電子公司工程師， (IEEE) 標準，可在資料中心內啟用聚合網狀架構。 DCB 會在主機中提供以硬體佇列為基礎的頻寬管理，並從連續的交換器進行合作。 儲存體、資料網路、叢集處理序間通訊 (IPC) 和管理的所有流量都會共用相同的 Ethernet 網路基礎結構。 在 Windows Server 2016 中，DCB 可分別套用至任何 NIC，以及系結至 Hyper-v 交換器的 Nic。

針對 DCB，Windows Server 會使用以優先順序為基礎的流量控制 (PFC) ，在 IEEE 802.1 Qbb 中標準化。 PFC 會防止流量類別內的溢位，而建立 (幾乎) 不失真的網路網狀架構。 Windows Server 也會使用增強的傳輸選擇 (ETS) ，在 IEEE 802.1 Qaz 中標準化。 ETS 可將頻寬分割成保留部分，最多八個類別的流量。 每個流量類別都有自己的傳輸佇列，而透過使用 PFC，可以在類別內啟動和停止傳輸。

如需詳細資訊，請參閱[資料中心橋接 (DCB) ](../dcb/dcb-top.md)。

## <a name="hyper-v-network-virtualization"></a>Hyper-V 網路虛擬化

|                            |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|       **v1 (HNVv1) **       |                     Windows Server 2012 中引進的 Hyper-v 網路虛擬化 (HNV) 可讓客戶網路在共用的實體網路基礎結構之上進行虛擬化。 透過實體網路網狀架構上所需的最少變更，HNV 可讓服務提供者在三個雲端的任何位置部署和遷移租使用者工作負載的靈活性：服務提供者雲端、私人雲端或 Microsoft Azure 公用雲端。                     |
| **v2 NVGRE (HNVv2 NVGRE) ** | 在 Windows Server 2016 和 System Center Virtual Machine Manager 中，Microsoft 提供端對端網路虛擬化解決方案，其中包含 RAS 閘道、軟體負載平衡、網路控制卡等等。 如需詳細資訊，請參閱[Windows Server 2016 中的 Hyper-v 網路虛擬化總覽](../../sdn/technologies/hyper-v-network-virtualization/hyperv-network-virtualization-overview-windows-server.md)。 |
| **v2 VxLAN (HNVv2 VxLAN) ** |                                                                                                                                                                                        在 Windows Server 2016 中，是 SDN 延伸模組的一部分，您可以透過網路控制卡管理。                                                                                                                                                                                        |

---

## <a name="ipsec-task-offload-ipsecto"></a> (IPsecTO) 的 IPsec 工作卸載

IPsec 工作卸載是一項 NIC 功能，可讓作業系統使用 NIC 上的處理器進行 IPsec 加密工作。

>[!IMPORTANT]
>IPsec 工作卸載是大部分網路介面卡不支援的舊版技術，而且存在的地方會預設為停用。

## <a name="private-virtual-local-area-network-pvlan"></a>私用虛擬區域網路區域網路 (PVLAN) 。

Pvlan 只允許相同虛擬化伺服器上的虛擬機器之間進行通訊。 私人虛擬網路未系結至實體網路介面卡。 私人虛擬網路會與虛擬化伺服器上的所有外部網路流量隔離，以及管理作業系統與外部網路之間的任何網路流量。 當您需要建立隔離的網路環境 (例如隔離的測試網域) 時，此類型的網路相當實用。 Hyper-v 和 SDN 堆疊僅支援 PVLAN 隔離埠模式。

如需有關 PVLAN 隔離的詳細資訊，請參閱[System Center： Virtual Machine Manager 工程 Blog](https://blogs.technet.microsoft.com/scvmm/2013/06/04/logical-networks-part-iv-pvlan-isolation/)。

## <a name="remote-direct-memory-access-rdma"></a>遠端直接記憶體存取 (RDMA)

RDMA 是一種網路技術，提供高輸送量、低延遲的通訊，可將 CPU 使用量降到最低。 RDMA 藉由啟用網路介面卡，將資料直接傳送至應用程式記憶體或從中傳輸，支援零複製網路。 具備 RDMA 功能的表示 NIC (實體或虛擬) 能夠將 RDMA 公開給 RDMA 用戶端。 另一方面，已啟用 RDMA 的表示具備 RDMA 功能的 NIC 會將 RDMA 介面公開到堆疊上。

如需 RDMA 的詳細資訊，請參閱[ (rdma 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md)。

## <a name="receive-side-scaling-rss"></a>Receive Side Scaling (RSS)

RSS 是一種 NIC 功能，可都會隔離不同組的資料流程，並將它們傳遞至不同的處理器進行處理。 RSS 會平行處理網路處理，讓主機能夠調整成非常高的資料速率。

如需詳細資訊，請參閱[ (RSS) 的接收端調整](/windows-hardware/drivers/network/introduction-to-receive-side-scaling)。

## <a name="single-root-input-output-virtualization-sr-iov"></a>單一根目錄輸入-輸出虛擬化 (SR-IOV) 

SR-IOV 可讓 VM 流量直接從 NIC 移至 VM，而不需要通過 Hyper-v 主機。 SR-IOV 是對 VM 效能的絕佳改進，但缺乏主機管理該管道的能力。 只有在工作負載正常運作、受信任且通常是主機中的唯一 VM 時，才使用 SR-IOV。

使用 SR-IOV 的流量會略過 Hyper-v 交換器，這表示將不會套用任何原則，例如 Acl 或頻寬管理。 SR-IOV 流量也無法透過任何網路虛擬化功能傳遞，因此無法套用 NV 或 VxLAN 封裝。 只有在特定情況下，才將 SR-IOV 用於受到妥善信任的工作負載。 此外，您也無法使用主機原則、頻寬管理和虛擬化技術。

在未來，兩種技術會允許 SR-IOV：一般流程資料表 (GFT) 和硬體 QoS 卸載 (NIC) 中的頻寬管理–一旦生態系統中的 Nic 支援它們即可。 這兩種技術的結合，可讓 SR-IOV 適用于所有 Vm，允許套用原則、虛擬化和頻寬管理規則，而且可能會導致 SR-IOV 的一般應用程式中有很大的進展。

如需詳細資訊，請參閱[單一根目錄 I/o 虛擬化的總覽 (sr-iov) ](/windows-hardware/drivers/network/overview-of-single-root-i-o-virtualization--sr-iov-)。

## <a name="tcp-chimney-offload"></a>TCP 煙囪卸載

TCP 煙囪卸載（也稱為 TCP 引擎卸載 (TOE) ）是一項技術，可讓主機將所有的 TCP 處理卸載至 NIC。 由於 Windows Server TCP 堆疊幾乎比 TOE 引擎更有效率，因此不建議使用 TCP 煙囪卸載。

>[!IMPORTANT]
>TCP 煙囪卸載是已淘汰的技術。 我們建議您不要使用 TCP 煙囪卸載，因為 Microsoft 可能會在未來停止支援。

## <a name="virtual-local-area-network-vlan"></a>虛擬區域網路絡 (VLAN) 

VLAN 是乙太網路框架標頭的延伸模組，可將 LAN 分割成多個 Vlan，每一個都使用自己的位址空間。 在 Windows Server 2016 中，Vlan 是在 Hyper-v 交換器的埠上設定，或是在 NIC 小組團隊上設定小組介面。 如需詳細資訊，請參閱[NIC 小組和虛擬區域網路絡 (vlan) ](../nic-teaming/nic-teaming.md)。

## <a name="virtual-machine-queue-vmq"></a>虛擬機器佇列 (VMQ)

Vmq 數量是為每個 VM 配置佇列的 NIC 功能。 任何時候，當您啟用 Hyper-v 時，您也必須啟用 VMQ。 在 Windows Server 2016 中，Vmq 數量使用 NIC 交換器 vPorts 搭配指派給 vPort 的單一佇列來提供相同的功能。 如需詳細資訊，請參閱[虛擬接收端調整 (vRSS) ](../vrss/vrss-top.md)和[NIC](../nic-teaming/nic-teaming.md)小組。

## <a name="virtual-machine-multi-queue-vmmq"></a>虛擬機器多佇列 (VMMQ) 

VMMQ 是一項 NIC 功能，可讓 VM 的流量分散到多個佇列，每個都由不同的實體處理器處理。 然後流量會傳遞至 VM 中的多個 LPs，如同在 vRSS 中一樣，這可讓您將大量的網路頻寬提供給 VM。

---