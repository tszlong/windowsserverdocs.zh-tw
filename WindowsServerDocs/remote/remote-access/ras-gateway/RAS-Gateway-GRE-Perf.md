---
title: RAS 閘道 GRE 通道輸送量及效能
description: 本主題適用于 (IT) 專業人員提供的資訊技術，提供 RAS 閘道一般路由封裝 (GRE) 通道的輸送量效能資訊。
manager: brianlic
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: fceb3ff5421be65a9bb25ae74510257f70d0aaa6
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946774"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>RAS 閘道 GRE 通道輸送量及效能

>適用于： Windows Server \( 半年通道\)

您可以使用本主題來瞭解遠端存取服務器 \( RAS \) 閘道一般路由封裝 \( \) 非軟體定義網路 SDN 架構 \( 測試環境中 Windows Server 1709 版的 GRE 通道效能 \) 。

RAS 閘道是一種軟體路由器和閘道，您可以在單一租使用者模式或多租使用者模式中使用。 本主題討論單一租使用者模式、具有容錯移轉叢集的高可用性設定。 本主題所提供的 GRE 通道效能統計資料適用于 singele 租使用者和多租使用者模式中的 RAS 閘道。

>[!NOTE]
>「容錯移轉叢集」是一種 Windows Server 功能，可讓您將多部伺服器群組在一組容錯叢集中。 如需詳細資訊，請參閱[容錯移轉](../../../failover-clustering/failover-clustering-overview.md)叢集

單一租使用者模式可讓任何規模的組織將閘道部署為外部或網際網路 \- 面向邊緣虛擬私人網路的 \( VPN \) 伺服器。 在單一租使用者模式中，您可以在實體伺服器或虛擬機器 VM 上部署 RAS 閘道 \( \) 。 本主題說明在容錯移轉叢集中設定的兩部虛擬機器 vm 上的 RAS 閘道部署 \( \) 。

>[!IMPORTANT]
>因為 GRE 通道提供封裝但不加密，所以您不應該使用以 GRE 設定的 RAS 閘道作為網際網路邊緣閘道。 若要瞭解使用 GRE 通道的 RAS 閘道的最佳使用方式，請參閱 [Windows Server 中的 Gre 通道](gre-tunneling-windows-server.md)。

GRE 是輕量通道通訊協定，可在虛擬點內封裝各式各樣的網路層通訊協定， \- 以透過 \- 網際網路通訊協定網路連結點連結。 Microsoft GRE 實行會封裝 IPv4 和 IPv6。

如需詳細資訊，請參閱 [Ras 閘道](./ras-gateway.md#bkmk_deploy)主題中的 **Ras 閘道部署案例** 一節。

在此測試案例中（如下圖所示），測量的流量會從組織內部網路2移至組織內部網路1。 租使用者工作負載 Vm 會使用 RAS 閘道將網路流量從內部網路2傳送至內部網路1。

![RAS 閘道 GRE 通道測試案例總覽](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>測試環境設定

本節提供有關測試環境和 RAS 閘道設定的資訊。

在測試環境中，RAS 閘道 Vm 是部署在 \- 容錯移轉叢集中的 hyper-v 主機上，以提供高可用性。

### <a name="hyper-v-host-configuration"></a>Hyper-v \- 主機設定

有兩個 Hyper-v \- 主機設定為以下列方式支援測試案例。

- 兩部雙重 \- 主目錄電腦都是使用 Windows Server 1709 版設定
- 這兩部伺服器中的兩個實體網路介面卡會連接到不同的子網，兩者都代表組織內部網路的子網。 網路和支援硬體都有 10 GBps 的容量。
- 實體伺服器上的超執行緒已停用。 這會從實體 Nic 提供最大的輸送量。
- Hyper-v \- 伺服器角色會安裝在這兩部伺服器上，並設定兩個外部 Hyper-v \- 虛擬交換器（每個實體網路介面卡各一個）。
- 因為這兩部伺服器都連接到相同的內部網路，所以伺服器可以互相通訊。
- Hyper-v \- 主機是在容錯移轉叢集中透過內部網路網路進行設定。

>[!NOTE]
>如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](../../../virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch.md)。

### <a name="vm-configuration"></a>VM 組態

您可以透過下列方式，將兩部 Vm 設定為支援測試案例。

- 在每部伺服器上，已安裝執行 Windows Server 1709 版的 VM。 每個 VM 都設定有10個核心和 8 GB 的 RAM。
- 每個 VM 也會使用兩個虛擬網路介面卡來設定。 一個虛擬網路介面卡連線到內部網路1虛擬交換器，另一個虛擬網路介面卡則連接到內部網路2虛擬交換器。
- 每部 VM 都已安裝 RAS 閘道並設定為 GRE 型 \- VPN 伺服器。
- 閘道 Vm 是在容錯移轉叢集中設定。 當叢集化時，其中一個 VM 會處於作用中狀態，另一個則是被動 VM。

### <a name="workload-hyper-v-hosts-and-vms"></a>工作負載 Hyper-v \- 主機和 vm

針對這項測試，內部網路上會安裝兩個工作負載 Hyper-v \- 主機，且每部主機都會安裝一個 VM。 如果您要在自己的測試環境中複製這項測試，您可以視需要安裝最多的工作負載伺服器和 Vm。

- 工作負載 Hyper-v \- 主機會安裝一個連接到組織內部網路的實體網路介面卡。
- 在 Hyper-v \- 虛擬交換器中，會在每部主機上建立一個虛擬交換器。 交換器是外部的，且系結至連線至內部網路的一個網路介面卡。
- 工作負載 Vm 的設定具有 2 GB RAM 和2個核心。
- 工作負載 Vm 各有一個連接到內部網路虛擬交換器的虛擬網路介面卡。

### <a name="traffic-generator-tool"></a>流量產生器工具

此測試中使用的流量產生器工具是 ctsTraffic 工具。 這項工具的 Git 存放庫位於 [https://github.com/Microsoft/ctsTraffic](https://github.com/Microsoft/ctsTraffic) 。

## <a name="ras-gateway-performance"></a>RAS 閘道效能

本節中的圖例說明具有多個 TCP 連線的 GRE 通道輸送量工作管理員顯示。

在 \- 設定為 GRE RAS 閘道的多核心 vm 上，最多可達到 2.0 Gbps 的輸送量。

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>多個 TCP 會話的 GRE 通道效能

如果有多個 TCP 會話，CPU 使用率會達到100%，而且 GRE 通道上的最大輸送量是 2.0 Gbps。

下圖說明這兩個 RAS 閘道 Vm 上的 CPU 使用率。 使用中的 VM、RAS 閘道 VM #1、位於左側，而被動 VM 的 RAS 閘道 VM #2 則位於右邊。

![工作管理員中的閘道 VM CPU 使用率](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

下圖說明 RAS 閘道 Vm 上的乙太網路輸送量。 使用中的 VM、RAS 閘道 VM #1、位於左側，而被動 VM 的 RAS 閘道 VM #2 則位於右邊。

![工作管理員中的閘道 VM 乙太網路輸送量](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>具有一個 TCP 連接的 GRE 通道效能

當測試設定從多個 TCP 會話變更為單一 TCP 會話時，只有一個 CPU 核心會達到 RAS 閘道 Vm 上的最大容量。

GRE 通道上的最大輸送量介於 400-500 Mbps 之間。

下圖說明這兩個 RAS 閘道 Vm 上的 CPU 使用率。 使用中的 VM、RAS 閘道 VM #1、位於左側，而被動 VM 的 RAS 閘道 VM #2 則位於右邊。

![工作管理員中的閘道 VM CPU 使用率](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


下圖說明 RAS 閘道 Vm 上的乙太網路輸送量。 使用中的 VM、RAS 閘道 VM #1、位於左側，而被動 VM 的 RAS 閘道 VM #2 則位於右邊。

![工作管理員中的閘道 VM 乙太網路輸送量](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

如需 RAS 閘道效能的詳細資訊，請參閱 [軟體定義網路中的 HNV 閘道效能微調](../../../administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance.md)。
