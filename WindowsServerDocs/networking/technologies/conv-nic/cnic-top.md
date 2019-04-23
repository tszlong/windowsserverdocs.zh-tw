---
title: 聚合式的網路介面卡 (NIC) 設定指導方針
description: 聚合式的網路介面卡 (NIC) 可讓您公開，讓主機分割服務可以存取遠端直接記憶體存取 (RDMA) 相同 HYPER-V 來賓使用的 TCP/IP 流量的 Nic 上的磁碟分割的主機虛擬 NIC (vNIC) 透過 RDMA。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838839"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>聚合式網路介面卡\(NIC\)設定指導方針

>適用於：Windows Server （半年通道），Windows Server 2016

聚合式的網路介面卡\(NIC\)可讓您透過主機公開 RDMA\-分割虛擬 NIC \(vNIC\) ，讓主機分割服務可以存取遠端直接記憶體存取\(RDMA\)相同 HYPER-V 來賓使用的 TCP/IP 流量的 Nic 上。

交集的 NIC 功能，管理之前\(裝載的磁碟分割\)想要使用 RDMA 的服務必須使用專用的 RDMA\-功能的 Nic，即使已繫結至 HYPER-V 的 Nic 上的頻寬無法虛擬交換器。

以及聚合的 NIC，兩個工作負載\(管理使用者的 RDMA 和客體流量\)可以共用相同的實體 Nic，讓您可以在您伺服器中安裝較少的 Nic。

當您部署交集的 NIC 使用 Windows Server 2016 HYPER-V 主機和 HYPER-V 虛擬交換器時，在 HYPER-V 主機 Vnic RDMA 對公開服務主機處理序使用任何乙太網路的 RDMA\-基礎 RDMA 技術。

>[!NOTE]
>若要使用交集的 NIC 技術，在您的伺服器的認證的網路介面卡必須支援 RDMA。

本指南提供兩組指示，一個用於您的伺服器上具有單一網路介面卡安裝，也就是交集的 NIC; 的基本部署的部署和另一組指示您的伺服器上具有兩個或多個網路介面卡，安裝，也就是透過交換器內嵌小組的交集的 NIC 的部署\(設定\)小組的 RDMA\-支援網路介面卡。


## <a name="prerequisites"></a>先決條件

以下是交集的 NIC 的基本和資料中心部署的必要條件

>[!NOTE]
>提供的範例，我們使用 Mellanox connectx-3 Pro 40 Gbps 乙太網路介面卡，但您可以使用任何 Windows Server 認證 RDMA\-支援這項功能的網路介面卡。 如需有關相容的網路介面卡的詳細資訊，請參閱 Windows Server Catalog [LAN 卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本的交集的 NIC 必要條件

若要執行基本的交集的 NIC 設定本指南中的步驟，您必須擁有下列項目。

- 執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的兩部伺服器。
- 一個具備 RDMA 功能，認證的每部伺服器上安裝的網路介面卡。
- 每部伺服器上安裝 HYPER-V 伺服器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>資料中心交集的 NIC 的必要條件

若要執行的資料中心交集的 NIC 設定本指南中的步驟，您必須擁有下列項目。

- 執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 的兩部伺服器。
- 兩個具備 RDMA 功能，認證的每部伺服器上安裝的網路介面卡。
- 每部伺服器上安裝 HYPER-V 伺服器角色。
- 您必須先熟悉 Switch Embedded Teaming\(設定\)、 替代的 NIC 小組解決方案包含 Windows Server 2016 中的 HYPER-V 和軟體定義網路 (SDN) 堆疊的環境中使用。 設定會將部分 NIC 小組功能整合到 HYPER-V 虛擬交換器。 如需詳細資訊，請參閱 <<c0> [ 遠端直接記憶體存取 (RDMA) 和 Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

## <a name="related-topics"></a>相關主題
- [具有單一網路介面卡的交集的 NIC 設定](cnic-single.md)
- [交集的 NIC 組合的 NIC 設定](cnic-datacenter.md)
- [交集的 NIC 的實體交換器組態](cnic-app-switch-config.md)
- [疑難排解交集 NIC 組態](cnic-app-troubleshoot.md)

---