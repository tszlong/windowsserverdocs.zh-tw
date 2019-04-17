---
title: 聚合型的網路介面卡 (NIC) 設定指南
description: 本主題是匯集 NIC 設定節目表適用於 Windows Server 2016 的一部分。
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>聚合型的網路介面卡 \(NIC\) 設定指南

>適用於：Windows Server（以每年次管道）、Windows Server 2016

聚合型的網路介面卡 \(NIC\) 可讓您透過 host\ 磁碟分割 virtual NIC \(vNIC\) 公開 RDMA，以便主機的磁碟分割服務可以存取遠端直接記憶體存取 \(RDMA\) TCP/IP 流量使用 HYPER-V 來賓相同 Nic 上。

之前「匯集 NIC」功能，想要使用 RDMA 管理 \(host partition\) 服務已需要使用專用的 RDMA\ 能力 Nic，即使如果頻寬繫結至 HYPER-V Virtual 開關切換至 Nic 提供。

與匯集 NIC 的兩個工作負載 \（管理使用者的 RDMA 與來賓 traffic\）可以共用相同的實體 Nic，可讓您更少 Nic 安裝在您的伺服器。

當您匯集 NIC 部署 HYPER-V Virtual 參數與 Windows Server 2016 HYPER-V 主機時，在 HYPER-V 主機 vNICs 公開 RDMA 服務主機上的 RDMA 使用透過任何 Ethernet\ 型 RDMA 技術的處理程序。

>[!NOTE]
>若要使用匯集 NIC 技術，在您的伺服器認證的網路介面卡必須支援 RDMA。

本指南兩個設定的指示，一個用於您的伺服器上具有單一安裝網路介面卡，也就是匯集 NIC; 基本部署部署和其他的指示您的伺服器上具有兩個或更多網路介面卡，安裝，也就是匯集 NIC 部署到切換 Embedded 小組 \(SET\) 團隊的能力 RDMA\ 網路介面卡。

本指南包含下列主題。

- [使用單一網路介面卡的聚合型的 NIC 設定](cnic-single.md)
- [聚合型的 NIC 小組的 NIC 設定](cnic-datacenter.md)
- [聚合型 NIC 實體切換設定](cnic-app-switch-config.md)
- [疑難排解匯集 NIC 設定](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>必要條件

以下是匯集 NIC 基本和資料中心部署的必要條件

>[!NOTE]
>本指南範例，Mellanox ConnectX-3 Pro 40 Gbps 乙太網路卡使用，但是您可以使用任何的 Windows Server 認證 RDMA\ 能力網路介面卡的支援此功能。 如需相容的網路介面卡的相關資訊，查看 Windows Server Catalog 主題[區域網路卡](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)。

### <a name="basic-converged-nic-prerequisites"></a>基本匯集 NIC 必要條件

若要執行此基本匯集 NIC 設定節目表中的步驟，您必須符合下列。

- 執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 兩部伺服器。
- 一個 RDMA 處理能力，認證每個伺服器安裝網路介面卡。
- 必須在每個伺服器上安裝 HYPER-V 伺服器角色。

### <a name="datacenter-converged-nic-prerequisites"></a>Datacenter 匯集 NIC 必要條件

若要執行此 datacenter 匯集 NIC 設定節目表中的步驟，您必須符合下列。

- 執行 Windows Server 2016 Datacenter edition 或 Windows Server 2016 Standard edition 兩部伺服器。
- 有兩個 RDMA 處理能力，認證中每個伺服器安裝網路介面卡。
- 必須在每個伺服器上安裝 HYPER-V 伺服器角色。
- 您必須熟悉切換 Embedded 小組 \(SET\)，這是另一個方法 NIC 小組方案，您可以在 Windows Server 2016 中包含 HYPER-V 和軟體所定義網路 (SDN) 堆疊的環境中使用。 設定 HYPER-V Virtual 開關切換至整合 NIC 小組的某些功能。 如需詳細資訊，請查看[遠端直接記憶體存取 (RDMA) 和切換 Embedded 小組（設定）](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)。

