---
title: 軟體定義網路 (SDN)
description: 軟體定義的網路功能 (SDN) 提供方法來集中設定和管理實體和虛擬網路裝置，例如路由器、交換器及資料中心內的閘道。 您可以使用本主題來瞭解 Windows Server、System Center 和 Microsoft Azure 所提供的軟體定義網路 (SDN) 技術。
manager: grcusanz
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/09/2018
ms.openlocfilehash: aa5ff02e7bbb790b8d7af2b2dd7d7f478c2e3d0a
ms.sourcegitcommit: fb2ae5e6040cbe6dde3a87aee4a78b08f9a9ea7c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/23/2021
ms.locfileid: "98716954"
---
# <a name="sdn-in-windows-server-overview"></a>Windows Server 中的 SDN 概觀

>適用於：Windows Server 2019、Windows Server 2016


軟體定義的網路功能 (SDN) 提供方法來集中設定和管理實體和虛擬網路裝置，例如路由器、交換器及資料中心內的閘道。 您可以使用現有的 SDN 相容裝置，在虛擬網路與實體網路之間達成更深入的整合。 虛擬網路元素（例如 Hyper-v 虛擬交換器、Hyper-v 網路虛擬化和 RAS 閘道）設計為 SDN 基礎結構的整數元素。

>[!Note]
>Hyper-v 主機和虛擬機器 (Vm) 執行 SDN 基礎結構伺服器（例如網路控制站和軟體負載平衡節點）必須安裝 Windows Server 2019 或 2016 Datacenter edition。
>
>Hyper-v 主機只包含連線到 SDN 控制網路的租使用者工作負載 Vm，可以使用 Windows Server 2019 或 2016 Standard edition。

因為網路平面不再系結至網路裝置本身，所以可能會有 SDN。 但是，其他實體（例如 System Center 2016 之類的資料中心管理軟體則使用網路平面）。 SDN 可讓您以動態方式管理您的資料中心網路，提供自動化、集中式的方式，以符合您的應用程式和工作負載需求。

您可以使用 SDN 來：

- 以動態方式建立、保護和連線您的網路，以符合您的應用程式演進需求
- 以非干擾性的方式加快部署工作負載的速度
- 包含在網路中散佈的安全性弱點
- 定義及控制管理實體和虛擬網路的原則
- 以一致的方式大規模執行網路原則

SDN 可讓您完成所有的工作，同時也能降低整體基礎結構的成本。



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>聯絡資料中心和雲端網路產品小組

如果您想要與 Microsoft 或其他 SDN 客戶討論 SDN 技術，有各種不同的方法可以建立連絡人。

如需詳細資訊，請參閱 [與資料中心和雲端網路團隊聯絡](contact-sdn-team.md)。
