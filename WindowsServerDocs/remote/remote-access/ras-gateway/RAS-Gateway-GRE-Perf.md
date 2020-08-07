---
title: RAS 閘道 GRE 通道輸送量及效能
description: 本主題適用于) 專業人員 (的資訊技術，提供 RAS 閘道一般路由封裝 (GRE) 通道的輸送量效能資訊。
manager: brianlic
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 3cf4b74a6aa6d8f64b917842cc0806cd463716e8
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953664"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>RAS 閘道 GRE 通道輸送量及效能

>適用于： Windows Server \( 半年通道\)

您可以使用本主題來瞭解遠端存取服務器 \( RAS \) 閘道一般路由如何 \( \) 在 Windows Server 1709 版的非軟體定義網路 \( SDN 測試環境中封裝 GRE 通道效能 \) 。

RAS 閘道是一種軟體路由器和閘道，可讓您在單一租使用者模式或多租使用者模式中使用。 本主題討論單一租使用者模式、具有容錯移轉叢集的高可用性設定。 本主題中所提供的 GRE 通道效能統計資料，對於 singele 租使用者和多租使用者模式中的 RAS 閘道而言都是有效的。

>[!NOTE]
>「容錯移轉叢集」是一種 Windows Server 功能，可讓您將多部伺服器組成一個容錯叢集。 如需詳細資訊，請參閱[容錯移轉](../../../failover-clustering/failover-clustering-overview.md)叢集

單一租使用者模式可讓任何規模的組織將閘道部署為外部或網際網路 \- 面向邊緣虛擬私人網路 \( VPN \) 伺服器。 在單一租使用者模式中，您可以在實體伺服器或虛擬機器 VM 上部署 RAS 閘道 \( \) 。 本主題說明在 \( 容錯移轉叢集中設定的兩個虛擬機器 vm 上的 RAS 閘道部署 \) 。

>[!IMPORTANT]
>由於 GRE 通道提供封裝，但不會加密，因此您不應該使用以 GRE 設定的 RAS 閘道做為網際網路邊緣閘道。 若要瞭解具有 GRE 通道的 RAS 閘道最佳用法，請參閱[Windows Server 中的 GRE 通道](gre-tunneling-windows-server.md)。

GRE 是輕量通道通訊協定，可在虛擬點內封裝各種不同的網路層通訊協定， \- 以 \- 指向網際網路通訊協定網路上的連結。 Microsoft GRE 實作為封裝 IPv4 和 IPv6。

如需詳細資訊，請參閱[Ras 閘道](./ras-gateway.md#bkmk_deploy)主題中的**Ras 閘道部署案例**一節。

在下圖所示的此測試案例中，測量的流量會從組織內部網路2移到組織內部網路1。 租使用者工作負載 Vm 會使用 RAS 閘道，將網路流量從內部網路2傳送到內部網路1。

![RAS 閘道 GRE 通道測試案例總覽](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>測試環境設定

本節提供測試環境和 RAS 閘道設定的相關資訊。

在測試環境中，RAS 閘道 Vm 會部署在 \- 容錯移轉叢集中的 hyper-v 主機上，以提供高可用性。

### <a name="hyper-v-host-configuration"></a>Hyper-v \- 主機設定

兩部 Hyper-v \- 主機已設定為以下列方式支援測試案例。

- 兩部雙重 \- 主機實體電腦設定為使用 Windows Server，版本1709
- 兩部伺服器中的兩個實體網路介面卡都連接到不同的子網，這兩者都代表組織內部網路的子網。 網路和支援的硬體都有 10 GBps 的容量。
- 實體伺服器上的超執行緒已停用。 這會提供來自實體 Nic 的最大輸送量。
- Hyper-v \- 伺服器角色會安裝在兩部伺服器上，並以兩個外部 Hyper-v \- 虛擬交換器設定，每個實體網路介面卡各一個。
- 因為這兩部伺服器都連接到相同的內部網路，所以伺服器可以彼此通訊。
- Hyper-v \- 主機是在容錯移轉叢集中，透過內部網路進行設定。

>[!NOTE]
>如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../../../virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch.md)。

### <a name="vm-configuration"></a>VM 組態

兩個 Vm 會設定為以下列方式支援測試案例。

- 在每部伺服器上，已安裝執行 Windows Server （版本1709）的 VM。 每個 VM 都設定了10個核心和 8 GB 的 RAM。
- 每個 VM 也設定了兩張虛擬網路介面卡。 一個虛擬網路介面卡連線到內部網路1虛擬交換器，另一個虛擬網路介面卡連線到內部網路2虛擬交換器。
- 每部 VM 都已安裝 RAS 閘道，並將其設定為 GRE 型 \- VPN 伺服器。
- 閘道 Vm 會在容錯移轉叢集中設定。 叢集時，有一個 VM 作用中，而另一個 vm 是被動的。

### <a name="workload-hyper-v-hosts-and-vms"></a>工作負載 Hyper-v \- 主機和 vm

在這項測試中，兩個工作負載 \- 的 hyper-v 主機會安裝在內部網路上，而且每部主機都會安裝一部 VM。 如果您要在自己的測試環境中複製這項測試，您可以安裝的工作負載伺服器和 Vm 數目，適用于您的目的。

- 工作負載 Hyper-v \- 主機已安裝一個連線到組織內部網路的實體網路介面卡。
- 在 Hyper-v \- 虛擬交換器中，會在每部主機上建立一個虛擬交換器。 交換器是外部的，並且系結至連線到內部網路的一張網路介面卡。
- 工作負載 Vm 設定了 2 GB RAM 和2個核心。
- 工作負載 Vm 各有一個連線到內部網路虛擬交換器的虛擬網路介面卡。

### <a name="traffic-generator-tool"></a>流量產生器工具

這項測試中使用的流量產生器工具是 ctsTraffic 工具。 此工具的 Git 存放庫位於 [https://github.com/Microsoft/ctsTraffic](https://github.com/Microsoft/ctsTraffic) 。

## <a name="ras-gateway-performance"></a>RAS 閘道效能

本節中的圖例描述 [工作管理員] 顯示具有多個 TCP 連線的 GRE 通道輸送量。

在 \- 設定為 GRE RAS 閘道的多核心 vm 上，最多可達到 2.0 Gbps 的輸送量。

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>具有多個 TCP 會話的 GRE 通道效能

在多個 TCP 會話中，CPU 使用率達到100%，而 GRE 通道的最大輸送量為 2.0 Gbps。

下圖描述這兩個 RAS 閘道 Vm 的 CPU 使用率。 [使用中的 VM]、[RAS 閘道 VM #1] 位於左側，而 [被動 VM，RAS 閘道 VM] #2 則位於右側。

![工作管理員中的閘道 VM CPU 使用率](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

下圖描述 RAS 閘道 Vm 上的乙太網路輸送量。 [使用中的 VM]、[RAS 閘道 VM #1] 位於左側，而 [被動 VM，RAS 閘道 VM] #2 則位於右側。

![工作管理員中的閘道 VM 乙太網路輸送量](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>具有一個 TCP 連線的 GRE 通道效能

當測試設定從多個 TCP 會話變更為單一 TCP 會話時，只有一個 CPU 核心會達到 RAS 閘道 Vm 上的最大容量。

GRE 通道的最大輸送量介於 400-500 Mbps 之間。

下圖描述這兩個 RAS 閘道 Vm 的 CPU 使用率。 [使用中的 VM]、[RAS 閘道 VM #1] 位於左側，而 [被動 VM，RAS 閘道 VM] #2 則位於右側。

![工作管理員中的閘道 VM CPU 使用率](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


下圖描述 RAS 閘道 Vm 上的乙太網路輸送量。 [使用中的 VM]、[RAS 閘道 VM #1] 位於左側，而 [被動 VM，RAS 閘道 VM] #2 則位於右側。

![工作管理員中的閘道 VM 乙太網路輸送量](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

如需 RAS 閘道效能的詳細資訊，請參閱[在軟體定義網路中 HNV 閘道效能微調](../../../administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance.md)。
