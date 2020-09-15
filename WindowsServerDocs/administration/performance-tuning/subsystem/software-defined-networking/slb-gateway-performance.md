---
title: 軟體定義網路中的 SLB 閘道效能微調
description: SDN 網路上的 SLB 閘道效能微調指導方針
ms.topic: article
ms.author: grcusanz
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d45a5f585c7da30e4d9bdd4c8ec3c2e2003b7ea0
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90077915"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>軟體定義網路中的 SLB 閘道效能微調

軟體負載平衡是由網路控制站 Vm 中的負載平衡器管理員、Hyper-v 虛擬交換器以及一組 Load Balancer Multixplexor (Mux) Vm 的組合所提供。

除了 [軟體定義的網路](index.md) 功能一節中描述的以外，不需要額外的效能微調來設定網路控制站或 hyper-v 主機以進行負載平衡，除非您將使用 Sr-iov 進行 mux，如下所述。

## <a name="slb-mux-vm-configuration"></a>SLB Mux VM 設定

SLB Mux 虛擬機器部署在主動-主動設定中。  這表示每個已部署並新增至網路控制站的 Mux VM 都可以處理連入要求。  因此，所有連線的匯總輸送量總計只會受到您已部署的 Mux Vm 數目所限制。

虛擬 IP (VIP) 的個別連線一律會傳送至相同的 Mux，假設 mux 數目保持不變，因此其輸送量會限制為單一 Mux VM 的輸送量。  Mux 只會處理目的地為 VIP 的輸入流量。  回應封包會直接從將回應傳送至用戶端的實體交換器傳送回應的 VM。

在某些情況下，當要求來源源自于管理 VIP 的相同網路控制站的 SDN 主機時，也會執行要求的輸入路徑的進一步優化，讓大部分的封包都能直接從用戶端傳送到伺服器，完全略過 Mux VM。  這項優化不需要進行其他設定。

每個 SLB Mux VM 都必須根據「  [規劃軟體定義網路基礎結構](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) 」主題的 SDN 基礎結構虛擬機器角色需求一節中所提供的指導方針來調整大小。

## <a name="single-root-io-virtualization-sr-iov"></a> (SR-IOV) 的單一根 IO 虛擬化

使用 40Gbit Ethernet 時，虛擬交換器處理 Mux VM 的封包的能力會成為 Mux VM 輸送量的限制因素。  因此，建議您在 SLB VM 的 VM 網路介面卡上啟用 SR-IOV，以確保虛擬交換器不是瓶頸。

若要啟用 SR-IOV，您必須在建立虛擬交換器時，在虛擬交換器上啟用它。  在此範例中，我們會建立具有交換器內嵌小組的虛擬交換器 (設定) 和 SR-IOV：
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
然後，必須在處理資料流量的 SLB Mux VM 的虛擬網路介面卡 (s) 上啟用它。  在此範例中，會在所有介面卡上啟用 SR-IOV：
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
