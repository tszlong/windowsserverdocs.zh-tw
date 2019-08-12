---
title: Windows Server 容器的效能調整
description: Windows Server 16 上容器的效能調整建議
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: landing-page
ms.author: DavSo; Ericam; YaShi
author: akino
ms.date: 10/16/2017
ms.openlocfilehash: 072d71a7825907ada7d4bc02eb5390722692e81d
ms.sourcegitcommit: 02f1e11ba37a83e12d8ffa3372e3b64b20d90d00
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68863416"
---
# <a name="performance-tuning-windows-server-containers"></a>Windows Server 容器的效能調整

## <a name="introduction"></a>簡介
Windows Server 2016 是第一個對 OS 提供內建容器技術支援的 Windows 版本。 在 Server 2016 中，有兩種類型的容器可使用：Windows Server 容器和 Hyper-V 容器。 每個容器類型都可支援 Windows Server 2016 的 Server Core 或 Nano Server SKU。 

這些設定都有不同的效能影響，我們將在下方詳細說明，以協助您了解何者最適合您的案例。 此外，我們會詳細說明影響效能的設定，並說明這每個選項的權衡取捨。

### <a name="windows-server-container-and-hyper-v-containers"></a>Windows Server 容器和 Hyper-V 容器

Windows Server 容器和 Hyper-V 容器有許多相同的可攜性和一致性優點，但在隔離保證和效能特性方面則有所不同。

**Windows Server 容器**可透過程序和命名空間隔離技術，提供應用程式隔離功能。 Windows Server 容器可與容器主機和所有執行於主機上的容器共用核心。

**Hyper-V 容器**可藉由在高度最佳化的虛擬機器中執行每個容器，擴充 Windows Server 容器所提供的隔離能力。 在此設定中，容器主機的核心不會與 Hyper-V 容器共用。

Hyper-V 容器所提供的額外隔離主要是藉助隔離容器與容器主機的 Hypervisor 層。 有別於 Windows Server 容器，這會影響容器密度，使系統檔案的共用減少並可能產生二進位檔，進而導致整體較高的儲存體和記憶體使用量。 此外，還會有一些網路、儲存體 IO 及 CPU 路徑的額外負荷。

### <a name="nano-server-and-server-core"></a>Nano Server 與 Server Core

Windows Server 容器和 Hyper-V 容器都可支援 Server Core 和 Windows Server 2016 中可用的新安裝選項：[Nano Server](https://technet.microsoft.com/windows-server-docs/compute/nano-server/getting-started-with-nano-server)。 

Nano Server 是一個遠端管理的伺服器作業系統，已針對私人雲端和資料中心最佳化。 它類似於 Server Core 模式的 Windows Server，但明顯較小、沒有本機登入功能，而且只支援 64 位元應用程式、工具和代理程式。 其佔用的磁碟空間相當少，而且啟動速度更快。

## <a name="container-start-up-time"></a>容器啟動時間
在許多案例中，容器啟動時間是容器是否能提供最大效益的關鍵衡量標準。 因此，了解如何最佳化容器的啟動時間相當重要。 若要改善啟動時間，您必須了解以下一些調整的利弊得失。

### <a name="first-logon"></a>第一次登入

Microsoft 會提供 Nano Server 和 Server Core 的基底映像。 專為 Server Core 提供的基底映像，已藉由移除第一次登入 (OOBE) 時相關的啟動時間額外負荷來進行最佳化。 但 Nano Server 基底映像卻不是如此。 不過，我們可以藉由將至少一個層級認可至容器，來從 Nano Server 基底映像中移除此成本。 後續的容器會從沒有第一次登入成本的映像啟動。
### <a name="scratch-space-location"></a>臨時空間位置

根據預設，在執行中容器的存留期間，容器會在容器主機的系統磁碟機媒體上使用暫存的臨時空間作為儲存體。 這可作為容器的系統磁碟機，因此，在容器作業中完成的許多寫入和讀取都會遵循此路徑。 若主機系統的系統磁碟機存在於轉盤式磁性媒體 (HDD) 上，但其具有更快速的儲存媒體 (更快速的 HDD 或 SSD) 可用，則您可以將容器臨時空間移至不同的磁碟機。 這可利用 dockerd –g 命令來達成。 此命令屬於全域性，因此會影響所有在系統上執行的容器。

### <a name="nested-hyper-v-containers"></a>巢狀式 Hyper-V 容器
適用於 Windows Server 2016 的 Hyper-V 引進了巢狀式 Hypervisor 支援。 也就是說，現在已能夠在虛擬機器內執行虛擬機器。 這開創了許多實用的案例，但也擴大了一些 Hypervisor 產生的效能影響，因為有兩層 Hypervisor 在實體主機上執行。

對於容器，這會在虛擬機器內執行 Hyper-V 容器時產生影響。 由於 Hyper-V 容器會透過其本身和容器主機之間的 Hypervisor 層來達到隔離效果，因此，當容器主機是 Hyper-V 架構的虛擬機器時，就會有啟動時間、儲存體 IO、網路 IO 和輸送量及 CPU 方面的效能額外負荷。

## <a name="storage"></a>儲存體
### <a name="mounted-data-volumes"></a>掛接的資料磁碟區

容器可讓您使用容器主機系統磁碟機作為容器的臨時空間。 不過，容器臨時空間的存留期範圍與容器相同。 也就是說，當容器停止時，臨時空間和所有相關聯的資料都會消失。

不過，有許多案例是希望資料能保持獨立，不受容器存留期影響。 在這些情況下，我們支援將資料磁碟區從容器主機掛接到容器中。 針對 Windows Server 容器，掛接的資料磁碟區只會產生細微的 IO 路徑負擔 (接近原生效能)。 不過，將資料磁碟區掛接到 Hyper-V 容器時，該路徑中會有 IO 效能降低的情況。 此外，當您在虛擬機器內執行 Hyper-V 容器時，此影響會擴大。

### <a name="scratch-space"></a>臨時空間

根據預設，Windows Server 容器和 Hyper-V 容器都會提供 20GB 的動態 VHD 作為容器的臨時空間。 針對這兩種容器類型，容器 OS 都會佔用該空間的一部份，而這適用於每個啟動的容器。 因此，請務必記得，每個啟動的容器都會多少影響到儲存體，而且根據不同的工作負載，您可以寫入最多 20GB 的備份儲存媒體。 設計伺服器儲存體組態時應將此情形列入考量。
(我們可以設定暫時的大小)

## <a name="networking"></a>網路功能
Windows Server 容器和 Hyper-V 容器會提供各種不同的網路模式，以符合不同的網路組態需求。 這每個選項都會呈現其本身的效能特性。

### <a name="windows-network-address-translation-winnat"></a>Windows 網路位址轉譯 (WinNAT)

每個容器將會從內部、私用 IP 首碼 (例如 172.16.0.0/12) 收到 IP 位址。 支援從容器主機到容器端點的連接埠轉送/對應。 Docker 依預設會在 dockerd 第一次執行時建立 NAT 網路。

在這三種模式中，NAT 組態是成本最高的網路 IO 路徑，但需要最少的設定。 

Windows Server 容器使用主機 vNIC 連結到虛擬交換器。 Hyper-V 容器使用綜合 VM NIC (不向公用程式 VM 公開) 連結到虛擬交換器。 當容器與外部網路通訊時，封包會透過 WinNAT 及套用的位址轉譯來進行路由，而這會產生一些額外負荷。

### <a name="transparent"></a>透明

每個容器端點直接連接至實體網路。 實體網路的 IP 位址可以是靜態指派，或是從外部 DHCP 伺服器動態指派。

透明模式是成本最低的網路 IO 路徑，外部封包會直接傳遞至容器虛擬 NIC (提供直接存取給外部網路)。

### <a name="l2-bridge"></a>L2 橋接器
每個容器端點會在與容器主機相同的 IP 子網路中。 必須以靜態方式從與容器主機相同的首碼指派 IP 位址。 因為第 2 層位址轉譯的關係，主機上的所有容器端點會具有相同的 MAC 位址。

L2 橋接模式的效能比 WinNAT 模式好，因為其對外部網路提供直接存取，但效能比透明模式低，因為其也引入了 MAC 位址轉譯。




