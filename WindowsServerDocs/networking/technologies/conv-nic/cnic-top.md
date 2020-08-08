---
title: 聚合式網路介面卡 (NIC) 設定指引
description: 聚合式網路介面卡 (NIC) 可讓您透過主機分割區虛擬 NIC 公開 RDMA (vNIC) ，讓主機磁碟分割服務可以在 Hyper-v 來賓用於 TCP/IP 流量的相同 Nic 上，存取遠端直接記憶體存取 (RDMA) 。
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 2b077b911d3721907e70b198c62970aafe25e58d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944752"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>聚合式網路介面卡 \( NIC 設定 \) 指引

>適用於：Windows Server (半年度管道)、Windows Server 2016

聚合式網路介面卡 \( NIC 可 \) 讓您透過主機 \- 磁碟分割虛擬 NIC vNIC 公開 RDMA， \( \) 讓主機分割區服務可以 \( \) 在 Hyper-v 來賓用於 tcp/ip 流量的相同 Nic 上，存取遠端直接記憶體存取 rdma。

在聚合式 NIC 功能之前， \( 需要使用 rdma 的管理主機分割 \) 服務必須使用具備專用 rdma \- 功能的 nic，即使已系結至 hyper-v 虛擬交換器的 nic 上有可用的頻寬也一樣。

透過聚合式 NIC，RDMA 和來賓流量的兩個工作負載 \( 管理使用者 \) 可以共用相同的實體 nic，讓您在伺服器中安裝較少的 nic。

當您使用 Windows Server 2016 Hyper-v 主機和 Hyper-v 虛擬交換器部署交集 NIC 時，Hyper-v 主機中的 Vnic 會透過任何以乙太網路為 \- 基礎的 rdma 技術，使用 rdma 將 rdma 服務公開至主機進程。

>[!NOTE]
>若要使用聚合式 NIC 技術，伺服器中認證的網路介面卡必須支援 RDMA。

本指南提供兩組指示，一個適用于您的伺服器已安裝單一網路介面卡的部署，這是聚合式 NIC 的基本部署;還有另一組指示，也就是您的伺服器已安裝兩個或更多個網路介面卡，這是在 \( \) 支援 RDMA 的網路介面卡上，透過交換器內嵌小組集團隊部署的交集 NIC \- 。


## <a name="prerequisites"></a>必要條件

以下是聚合式 NIC 的基本和資料中心部署的必要條件。

>[!NOTE]
>在提供的範例中，我們使用 Mellanox ConnectX-3 Pro 40 Gbps Ethernet 介面卡，但您可以使用任何 \- 支援這項功能且具備 Windows Server 認證 RDMA 功能的網路介面卡。 如需相容網路介面卡的詳細資訊，請參閱 Windows Server Catalog 主題[LAN 卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本交集 NIC 必要條件

若要針對基本交集 NIC 設定執行本指南中的步驟，您必須具備下列各項。

- 兩部執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的伺服器。
- 每部伺服器上安裝一個具備 RDMA 功能、認證的網路介面卡。
- 每部伺服器上安裝的 hyper-v 伺服器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>資料中心交集 NIC 必要條件

若要針對 datacenter 交集 NIC 設定執行本指南中的步驟，您必須具備下列各項。

- 兩部執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的伺服器。
- 每部伺服器上安裝了兩個具備 RDMA 功能的認證網路介面卡。
- 每部伺服器上安裝的 hyper-v 伺服器角色。
- 您必須熟悉交換器內嵌小組 \( 集合 \) ，這是在包含 hyper-v 的環境中使用的替代 NIC 小組解決方案，以及在 Windows Server 2016 中 (SDN) 堆疊的軟體定義網路功能。 將一些 NIC 小組功能整合到 Hyper-v 虛擬交換器。 如需詳細資訊，請參閱[ (RDMA 的遠端直接記憶體存取) 和切換內嵌小組 (設定) ](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="related-topics"></a>相關主題
- [具有單一網路介面卡的聚合式 NIC 設定](cnic-single.md)
- [聚合式 NIC 組合 NIC 設定](cnic-datacenter.md)
- [聚合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
- [針對聚合式 NIC 設定進行疑難排解](cnic-app-troubleshoot.md)

---