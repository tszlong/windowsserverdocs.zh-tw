---
title: 聚合式網路介面卡（NIC）設定指引
description: 聚合式網路介面卡（NIC）可讓您透過主機分割區虛擬 NIC （vNIC）公開 RDMA，讓主機分割區服務可以在 Hyper-v 來賓用於 TCP/IP 流量的相同 Nic 上，存取遠端直接記憶體存取（RDMA）。
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: d791e0d51278d1f83f344250d38b1c7005c1a14a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355442"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>聚合式網路介面卡 \(NIC\) 設定指引

>適用於：Windows Server (半年通道)、Windows Server 2016

聚合式網路介面卡 \(NIC\) 可讓您透過主機\-磁碟分割虛擬 NIC \(vNIC\) 公開 RDMA，讓主機分割區服務可以在 Hyper-v 來賓用於 TCP/IP 流量的相同 Nic 上，存取遠端直接記憶體存取 \(RDMA\)。

在交集式 NIC 功能之前，需要使用 RDMA 的管理 \(主機磁碟分割\) 服務必須使用專用的 RDMA\-支援的 Nic，即使已系結至 Hyper-v 虛擬交換器的 Nic 上有可用的頻寬也一樣。

使用聚合式 NIC，\(RDMA 和來賓流量\) 的兩個工作負載，都可以共用相同的實體 Nic，讓您在伺服器中安裝較少的 Nic。

當您使用 Windows Server 2016 Hyper-v 主機和 Hyper-v 虛擬交換器部署交集 NIC 時，Hyper-v 主機中的 Vnic 會使用 RDMA 透過任何 Ethernet\-型 RDMA 技術，將 RDMA 服務公開至主機進程。

>[!NOTE]
>若要使用聚合式 NIC 技術，伺服器中認證的網路介面卡必須支援 RDMA。

本指南提供兩組指示，一個適用于您的伺服器已安裝單一網路介面卡的部署，這是聚合式 NIC 的基本部署;還有另一組指示，也就是您的伺服器已安裝兩張以上的網路介面卡，也就是透過交換器內嵌小組部署交集式 NIC \(設定\) 的 RDMA 小組\-支援的網路介面卡。


## <a name="prerequisites"></a>必要條件

以下是聚合式 NIC 的基本和資料中心部署的必要條件。

>[!NOTE]
>在提供的範例中，我們會使用 Mellanox ConnectX-3 Pro 40 Gbps Ethernet 介面卡，但您可以使用任何支援這項功能的 Windows Server 認證 RDMA\-支援的網路介面卡。 如需相容網路介面卡的詳細資訊，請參閱 Windows Server Catalog 主題[LAN 卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

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
- 您必須熟悉交換器內嵌小組 \(設定\)，這是在 Windows Server 2016 中包含 Hyper-v 和軟體定義網路（SDN）堆疊的環境中所使用的替代 NIC 小組解決方案。 將一些 NIC 小組功能整合到 Hyper-v 虛擬交換器。 如需詳細資訊，請參閱[遠端直接記憶體存取（RDMA）和交換器內嵌小組（SET）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="related-topics"></a>相關主題
- [具有單一網路介面卡的聚合式 NIC 設定](cnic-single.md)
- [聚合式 NIC 組合 NIC 設定](cnic-datacenter.md)
- [聚合式 NIC 的實體交換器設定](cnic-app-switch-config.md)
- [針對聚合式 NIC 設定進行疑難排解](cnic-app-troubleshoot.md)

---