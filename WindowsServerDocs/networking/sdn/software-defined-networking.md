---
title: 軟體定義網路 (SDN)
description: 軟體定義網路功能 (SDN) 提供的方法可讓您集中設定及管理實體與虛擬網路裝置，例如您資料中心內的路由器、交換器與閘道。 您可以使用本主題來深入了解 Windows Server、 System Center 和 Microsoft Azure 中提供的軟體定義網路 (SDN) 技術。
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: pashort
author: shortpatti
ms.date: 08/09/2018
ms.openlocfilehash: a6c4db97b1c55d5114eba09251685ef0f2b840bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855229"
---
# <a name="sdn-in-windows-server-overview"></a>Windows Server 中的 SDN 概觀

>適用於：Windows Server （半年通道），Windows Server 2016


軟體定義網路功能 (SDN) 提供的方法可讓您集中設定及管理實體與虛擬網路裝置，例如您資料中心內的路由器、交換器與閘道。 若要更深入的虛擬網路與實體網路之間的整合，您可以使用您現有的 SDN 相容裝置。 虛擬網路元素，例如 HYPER-V 虛擬交換器、 HYPER-V 網路虛擬化，以及 RAS 閘道被設計來 SDN 基礎結構的整合元素。 

>[!Note]
>HYPER-V 主機和執行 SDN 基礎結構伺服器，例如網路控制站和軟體負載平衡節點的虛擬機器 (Vm) 必須安裝 Windows Server 2016 Datacenter edition。 
>
>包含唯一租用戶工作負載 Vm 連線至 SDN 控制網路的 HYPER-V 主機可以使用 Windows Server 2016 Standard edition。

SDN 可能是因為網路飛機不再繫結至網路裝置本身。 不過，其他實體，例如資料中心管理軟體，例如 System Center 2016 會使用網路飛機。 SDN 可讓您管理您的資料中心網路，以動態方式提供自動化、 集中式的方式以符合您的應用程式和工作負載的需求。 

您可以使用 SDN 來：

- 以動態方式建立、 保護和您的網路，以滿足不斷成長的需求，您的應用程式的連線
- 加速您的工作負載的部署，以非干擾性的方式
- 包含您的網路上散佈的安全性弱點
- 定義及控制管理實體和虛擬網路的原則 
- 以一致的方式大規模實作網路原則

SDN 可讓您完成所有這些動作同時降低您整體基礎結構成本。



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>請連絡資料中心和雲端網路功能的產品小組

如果您想要討論向 Microsoft 或其他 SDN 客戶的 SDN 技術，有各種方法來進行連絡。

如需詳細資訊，請參閱 <<c0> [ 連絡資料中心和雲端網路團隊](contact-sdn-team.md)。
