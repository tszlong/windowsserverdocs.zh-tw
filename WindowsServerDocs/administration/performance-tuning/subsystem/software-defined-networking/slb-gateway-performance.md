---
title: SLB 閘道中的效能微調軟體定義網路
description: SLB 閘道效能微調指導方針 SDN 網路
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829929"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>SLB 閘道中的效能微調軟體定義網路

軟體負載平衡是由網路控制卡 Vm、 HYPER-V 虛擬交換器和一組負載平衡器 Multixplexor (Mux) Vm 的負載平衡器管理員的組合來提供。

沒有其他效能微調，才可設定網路控制站或負載平衡超越的 HYPER-V 主機所述[軟體定義網路](index.md)區段中，除非您要使用 SR-IOV 的多工器，如下所述。

## <a name="slb-mux-vm-configuration"></a>SLB Mux VM 組態

在主動-主動組態中部署 SLB Mux 虛擬機器。  這表示每個已部署並加入至網路控制站的 Mux VM 可以處理連入要求。  因此，所有連線的整體輸送量總計只受限於您已部署的 Mux Vm 的數目。  

個別連線至虛擬 IP (VIP) 會一律傳送至相同的 Mux，假設多工器的數目會維持不變，而且如此一來，其輸送量會受限於單一的 Mux VM 的輸送量。  多工器只會處理送往 VIP 的流量。  回應封包可能直接從傳送回應至實體交換器會將它轉送至用戶端 VM。

在某些情況下當要求的來源是來自已加入至相同的網路控制站管理的 VIP，SDN 主機時更進一步最佳化的要求的輸入路徑也會執行可讓大部分的封包，直接從移動用戶端到伺服器，並完全略過 Mux VM。  不不需要此最佳化，以進行任何其他設定。

必須調整大小的每個 SLB Mux VM，根據提供的 SDN 基礎結構虛擬機器角色需求一節中的指導方針[規劃軟體定義網路基礎結構](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)主題。

## <a name="single-root-io-virtualization-sr-iov"></a>單一根 IO 虛擬化 (SR-IOV)

當使用 40Gbit 乙太網路，Mux VM 的程序封包之虛擬交換器的功能就會變成 Mux VM 輸送量的限制因素。  因此建議您使用 SLB VM 的 VM 網路介面卡上啟用 SR-IOV，以確保虛擬交換器不是瓶頸。

若要啟用 SR-IOV，您必須啟用它的虛擬交換器時已建立虛擬交換器。  在此範例中，我們要使用交換器內嵌小組 (SET) 和 SR-IOV 建立虛擬交換器：
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
然後，SLB Mux VM 的虛擬網路介面卡處理的資料流量上必須啟用它。  在此範例中，SR-IOV 被啟用所有介面卡上：
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
