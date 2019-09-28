---
title: 軟體定義網路中的 SLB 閘道效能微調
description: SDN 網路上的 SLB 閘道效能微調指導方針
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: 9a0d239da2ca321333ec757db22bbaf9a9b8ba30
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383465"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>軟體定義網路中的 SLB 閘道效能微調

軟體負載平衡是由網路控制站 Vm、Hyper-v 虛擬交換器和一組 Load Balancer Multixplexor （Mux） Vm 中的負載平衡器管理員組合所提供。

除了[軟體定義的網路](index.md)功能一節中所述，設定網路控制站或 hyper-v 主機以進行負載平衡時，不需要進行任何額外的效能微調，除非您將使用 Sr-iov 進行 mux，如下所述。

## <a name="slb-mux-vm-configuration"></a>SLB Mux VM 設定

SLB Mux 虛擬機器部署在主動-主動設定中。  這表示每個部署並新增至網路控制卡的 Mux VM 都可以處理傳入的要求。  因此，所有連接的總匯總輸送量只會受限於您已部署的 Mux Vm 數目。  

虛擬 IP （VIP）的個別連線一律會傳送至相同的 Mux，假設 mux 的數目維持不變，因此其輸送量會限制為單一 Mux VM 的輸送量。  Mux 只會處理目的地為 VIP 的輸入流量。  回應封包會直接從傳送回應的 VM 傳送至實體交換器，並將其轉送至用戶端。

在某些情況下，當要求的來源是從加入至管理 VIP 的相同網路控制站的 SDN 主機所產生時，也會執行要求之輸入路徑的進一步優化，讓大部分的封包直接傳送到用戶端到伺服器，完全略過 Mux 虛擬機器。  不需要進行其他設定，就可以進行此優化。

每個 SLB Mux VM 都必須根據[規劃軟體定義網路基礎結構](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md)主題中的 SDN 基礎結構虛擬機器角色需求一節中提供的指導方針來調整大小。

## <a name="single-root-io-virtualization-sr-iov"></a>單一根 IO 虛擬化（SR-IOV）

使用 40Gbit Ethernet 時，虛擬交換器用來處理 Mux VM 封包的功能會成為 Mux VM 輸送量的限制因素。  基於此原因，建議您在 SLB VM 的 VM 網路介面卡上啟用 SR-IOV，以確保虛擬交換器不是瓶頸。

若要啟用 SR-IOV，您必須在虛擬交換器建立時，在虛擬交換器上啟用它。  在此範例中，我們會建立具有交換器內嵌小組（SET）和 SR-IOV 的虛擬交換器：
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
然後，必須在處理資料流量的 SLB Mux VM 的虛擬網路介面卡上啟用此功能。  在此範例中，會在所有介面卡上啟用 SR-IOV：
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
