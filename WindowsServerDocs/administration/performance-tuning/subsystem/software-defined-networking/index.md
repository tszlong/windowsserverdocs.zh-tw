---
title: 效能微調軟體定義網路
description: 軟體定義網路 (SDN) 效能調整指導方針
ms.topic: article
ms.author: grcusanz; anpaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9c097d9b777676da1caef062e8aee4267d2e4dac
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895950"
---
# <a name="performance-tuning-software-defined-networks"></a>效能微調軟體定義網路

Windows Server 2016 中的軟體定義網路 (SDN) 是由網路控制卡、Hyper-V 主機、軟體負載平衡器閘道和 HNV 閘道的組合所組成。  如需微調上述每個元件，請參閱下列各節：

## <a name="network-controller"></a>網路控制站

網路控制卡是一個 Windows Server 角色，其在設定為使用 SDN 且受網路控制卡控制的主機上執行的虛擬機器上必須加以啟用。

已啟用網路控制卡的三部虛擬機器就足以達到高可用性和最大效能。  您必須根據[規劃軟體定義網路基礎結構](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)主題的 SDN 基礎結構虛擬機器角色需求一節中提供的指導方針，調整每部 VM 的大小。

### <a name="sdn-quality-of-service-qos"></a>SDN 服務品質 (QoS)

若要確保虛擬機器流量有效且公平地設定優先順序，建議您在工作負載虛擬機器上設定 SDN QoS。  如需有關如何設定 SDN QoS 的詳細資訊，請參閱[設定租用戶 VM 網路介面卡的 QoS](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md) 主題。

## <a name="hyper-v-host-networking"></a>Hyper-V 主機網路功能

使用 SDN 時，適用「[Hyper-V 伺服器的效能微調](../../role/remote-desktop/session-hosts.md)」指南中 Hyper-V 網路 I/O 效能一節提供的指導方針，不過這一節涵蓋要確保使用 SDN 時達到最佳效能所必須遵循的其他指導方針。

### <a name="physical-network-adapter-nic-teaming"></a>實體網路介面卡 (NIC) 小組

如需最佳效能和容錯移轉功能，建議您設定要構成小組的實體網路介面卡。  使用 SDN 時，您必須使用 Switch Embedded Teaming (SET) 建立小組。

最佳的小組成員數目是兩個，因為虛擬化流量會分散於輸入和輸出方向的小組成員。  您可以有兩個以上的小組成員，不過輸入流量最多會分散於兩個介面卡。  如果動態負載平衡的預設值仍是設定於虛擬交換器，則輸出流量一律會分散於所有介面卡。


### <a name="encapsulation-offloads"></a>封裝卸載

SDN 仰賴封包的封裝來將網路虛擬化。  為了達到最佳效能，網路介面卡務必針對所使用的封裝格式支援硬體卸載。  不同的封裝格式並沒有顯著的效能優勢。  使用網路控制卡時，預設封裝格式為 VXLAN。

您可以使用下列 PowerShell Cmdlet 來判斷網路控制卡目前使用哪一種封裝格式：

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

為了達到最佳效能，如果傳回 VXLAN，您就必須確定您的實體網路介面卡支援 VXLAN 工作卸載。  如果傳回 NVGRE，則您的實體網路介面卡必須支援 NVGRE 工作卸載。

### <a name="mtu"></a>MTU

封裝會導致每個封包新增額外的位元組。  為了避免這些封包分散，則必須將實體網路設定為使用 jumbo 框架。  MTU 值為 9234 是 VXLAN 或 NVGRE 的建議大小，且必須設定於主機連接埠 (L2) 的實體介面和 VLAN 的路由器介面 (L3) (將透過這些介面傳送封裝的封包) 的實體交換器上。  這包括傳輸、HNV 提供者和管理網路。

Hyper-V 主機上的 MTU 是透過網路介面卡設定，而在 Hyper-V 主機上執行的網路控制卡主機代理程式將會針對封裝額外負荷自動調整 (如果由網路介面卡驅動程式支援)。

流量透過閘道從虛擬網路輸入後，系統就會移除封裝，並使用從 VM 傳送的原始 MTU。

### <a name="single-root-io-virtualization-sr-iov"></a>單一根目錄 IO 虛擬化 (SR-IOV)

使用虛擬交換器中的轉送交換器擴充功能，可在 Hyper-V 主機上實作 SDN。  對於要處理封包的這項交換器擴充功能，SR-IOV 不得使用於設定要搭配網路控制卡使用的虛擬網路介面，因為這會導致 VM 流量略過虛擬交換器。

如有需要，仍會在虛擬交換器上啟用 SR-IOV，並可供不受網路控制卡控制的 VM 網路介面卡使用。  這些 SR-IOV VM 可與網路控制卡所控制且未使用 SR-IOV 的 VM 共存於相同的虛擬交換器。

如果您使用 40Gbit 網路介面卡，建議您在軟體負載平衡 (SLB) 閘道的虛擬交換器上啟用 SR-IOV，以達到最大輸送量。  在[軟體負載平衡器閘道](slb-gateway-performance.md)一節有更詳細的說明。

## <a name="hnv-gateways"></a>HNV 閘道

您可以在 [HVN 閘道](hnv-gateway-performance.md)一節中找到有關微調 HNV 閘道以便搭配 SDN 使用的資訊。

## <a name="software-load-balancer-slb"></a>軟體負載平衡器 (SLB)

SLB 閘道只能搭配網路控制卡和 SDN 使用。  您可以在[軟體負載平衡器閘道](slb-gateway-performance.md)一節中找到更多關於微調 SDN 以便搭配 SLB 閘道使用的資訊。
