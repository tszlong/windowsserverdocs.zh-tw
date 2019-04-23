---
title: RAS 閘道 GRE 通道輸送量及效能
description: 本主題中，這是適用於資訊技術 (IT) 專業人員，提供有關 RAS 閘道 Generic Routing Encapsulation (GRE) 通道的輸送量效能資訊。
manager: brianlic
ms.prod: windows-server-threshold
ms.date: ''
ms.technology: networking-ras
ms.topic: article
ms.assetid: c051b2ec-de0f-49d1-82b9-5742b259cd7c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 73ae4e573d926f4a77b076c880c1d74ed69f032d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880759"
---
# <a name="ras-gateway-gre-tunnel-throughput-and-performance"></a>RAS 閘道 GRE 通道輸送量及效能

>適用於：Windows Server\(半年通道\)

您可以使用本主題來深入了解遠端存取伺服器\(RAS\)閘道 Generic Routing Encapsulation \(GRE\)通道上 Windows Server 1709 版，在非-軟體定義網路效能\(SDN\)測試環境。

RAS 閘道是軟體路由器，以及您可以使用多租用戶的模式或單一租用戶模式中的閘道。 本主題討論單一租用戶模式中，使用容錯移轉叢集的高可用性組態。 會在本主題中的 GRE 通道效能統計資料的有效值 RAS 閘道 singele 租用戶和多租用戶模式中。

>[!NOTE]
>容錯移轉叢集是 Windows Server 功能，可讓您將一起分組到容錯移轉叢集的多部伺服器。 如需詳細資訊，請參閱[容錯移轉叢集](../../../failover-clustering/failover-clustering-overview.md)

單一租用戶模式可讓部署閘道做為外部或網際網路的任何規模的組織\-面向虛擬私人網路時，edge \(VPN\)伺服器。 在單一租用戶模式中，您可以在實體伺服器或虛擬機器上部署 RAS 閘道\(VM\)。 本主題描述 RAS 閘道部署兩部虛擬機器\(Vm\)容錯移轉叢集中設定。

>[!IMPORTANT]
>因為 GRE 通道會提供封裝但不是加密，所以您不應該使用 RAS 閘道設定為最新的網際網路閘道的 GRE。 若要深入了解的最佳使用 RAS 閘道的 GRE 通道使用，請參閱[Windows Server 中的 GRE 通道](gre-tunneling-windows-server.md)。

GRE 是通道可封裝各種不同的網路層通訊協定中虛擬點的通訊協定的輕量型\-至\-點透過網際網路通訊協定網路的連結。 Microsoft GRE 實作封裝 IPv4 和 IPv6。

如需詳細資訊，請參閱**RAS 閘道部署案例**主題中的 < [RAS 閘道](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway#bkmk_deploy)。 

在此測試案例中，會在下圖中所述，測量的流量流程移出組織內部網路 2 到組織內部網路 1。 租用戶工作負載 Vm 從內部網路 2，為內部網路 1 使用 RAS 閘道，傳送網路流量。

![RAS 閘道的 GRE 通道測試案例概觀](../../media/GRE-Tunnel-Perf/Gre-Infrastructure.jpg)

## <a name="test-environment-configuration"></a>測試環境組態

本節中的測試環境和 RAS 閘道設定的相關資訊。

在測試環境中，RAS 閘道 Vm 部署於 Hyper-v\-V 主機在容錯移轉叢集的高可用性。

### <a name="hyper-v-host-configuration"></a>超\-V 主應用程式組態

兩個 Hyper-v\-Hyperv 主機都設定為以下列方式支援測試案例。 

- 兩個雙\-主伺服器的實體電腦設定 Windows Server 1709 版
- 在兩部伺服器的每個兩個實體網路介面卡會連接到不同的子網路-這兩者都代表組織內部網路的子網路。 網路和支援的硬體有容量為 10 GBps。
- 在實體伺服器上的超執行緒已停用。 此實體 Nic 從提供的最大輸送量。
- Hyper\-V 伺服器角色是兩部伺服器上安裝和設定兩個外部超\-V 虛擬交換器，一個用於每個實體網路介面卡。
- 因為這兩部伺服器連線到相同的內部網路時，伺服器可以互相通訊。
- Hyper-v\-V 主機透過內部網路時，已容錯移轉叢集中。 

>[!NOTE]
>如需詳細資訊，請參閱 [Hyper-V 虛擬交換器](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/hyper-v-virtual-switch)。

### <a name="vm-configuration"></a>VM 組態

兩個 Vm 設定為以下列方式支援測試案例。

- 在每一部伺服器，也就是安裝在 VM 上執行 Windows Server 1709 版。 每個 VM 已使用 10 個核心，8 GB 的 RAM。
- 每個 VM 也會設定兩個虛擬網路介面卡。 一個虛擬網路介面卡連線至內部網路 1 的虛擬交換器，以及其他虛擬網路介面卡連線到內部網路 2 虛擬交換器。
- 每個 VM 都 RAS 閘道安裝並設定為 GRE\-VPN 伺服器。
- 閘道 Vm 會在容錯移轉叢集設定。 叢集化時，有一個 VM 仍作用中和其他 VM 是被動。

### <a name="workload-hyper-v-hosts-and-vms"></a>工作負載的超\-HYPER-V 主機和 Vm

此測試的兩個工作負載超\-內部網路上安裝 Hyperv 主機，以及每個主機有一個已安裝的 VM。 如果您在自己的測試環境中要複製這項測試，您可以安裝任意數目的工作負載的伺服器和 Vm，視您的目的。

- 工作負載 Hyper-v\-Hyperv 主機有一個也就是安裝的實體網路介面卡連線到組織內部網路。
- 在 Hyper-v 中\-V 虛擬交換器，一個虛擬交換器會建立每個主機上。 參數是外部，並繫結至一個網路介面卡連線到內部網路。
- 工作負載 Vm 都會設定有 2 GB RAM 和 2 個核心。
- 工作負載 Vm 各有一個已連線到內部網路的虛擬交換器的虛擬網路介面卡。

### <a name="traffic-generator-tool"></a>流量產生器工具

這項測試中使用的流量產生器工具是 ctsTraffic 工具。 這項工具的 Git 儲存機制位於[ https://github.com/Microsoft/ctsTraffic ](https://github.com/Microsoft/ctsTraffic)。

## <a name="ras-gateway-performance"></a>RAS 閘道效能

本章節中的圖例說明工作管理員顯示的多重 TCP 連線的 GRE 通道輸送量。

您可以達到多上的最多 2.0 Gbps 輸送量\-核心設定為 GRE RAS 閘道的 Vm。

### <a name="gre-tunnel-performance-with-multiple-tcp-sessions"></a>使用多個 TCP 工作階段的 GRE 通道效能

有多個 TCP 工作階段的 CPU 使用率達到 100%，以及 GRE 通道的最大輸送量是 2.0 Gbps。

下圖說明這兩個 RAS 閘道 Vm 上的 CPU 使用率。 作用中的 VM，RAS 閘道 VM #1 時，在左側，被動的 VM，RAS 閘道 VM #2 中，位於右邊。

![閘道 VM CPU 使用率在 工作管理員](../../media/GRE-Tunnel-Perf/Gre-Tunnel-01.jpg)

下圖說明 RAS 閘道 Vm 上的乙太網路輸送量。 作用中的 VM，RAS 閘道 VM #1 時，在左側，被動的 VM，RAS 閘道 VM #2 中，位於右邊。

![閘道 VM 乙太網路輸送量在 工作管理員](../../media/GRE-Tunnel-Perf/Gre-Tunnel-02.jpg)


### <a name="gre-tunnel-performance-with-one-tcp-connection"></a>與單一 TCP 連線的 GRE 通道效能

使用測試設定，其從多個 TCP 工作階段變更為單一 TCP 工作階段時，只有一個 CPU 核心會達到 RAS 閘道 Vm 上的最大容量。

GRE 通道的最大輸送量為 400-500 Mbps 之間。

下圖說明這兩個 RAS 閘道 Vm 上的 CPU 使用率。 作用中的 VM，RAS 閘道 VM #1 時，在左側，被動的 VM，RAS 閘道 VM #2 中，位於右邊。

![閘道 VM CPU 使用率在 工作管理員](../../media/GRE-Tunnel-Perf/Gre-Tunnel-03.jpg)


下圖說明 RAS 閘道 Vm 上的乙太網路輸送量。 作用中的 VM，RAS 閘道 VM #1 時，在左側，被動的 VM，RAS 閘道 VM #2 中，位於右邊。

![閘道 VM 乙太網路輸送量在 工作管理員](../../media/GRE-Tunnel-Perf/Gre-Tunnel-04.jpg)

如需 RAS 閘道效能的詳細資訊，請參閱[HNV 閘道中的效能微調軟體定義網路](https://docs.microsoft.com/windows-server/administration/performance-tuning/subsystem/software-defined-networking/hnv-gateway-performance)。