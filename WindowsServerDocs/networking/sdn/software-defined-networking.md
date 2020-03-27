---
title: 軟體定義網路 (SDN)
description: 軟體定義網路功能 (SDN) 提供的方法可讓您集中設定及管理實體與虛擬網路裝置，例如您資料中心內的路由器、交換器與閘道。 使用本主題來瞭解 Windows Server、System Center 和 Microsoft Azure 中提供的軟體定義網路（SDN）技術。
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9a1ea73c-20cd-42c5-95ad-b003b9cc6d64
ms.author: lizross
author: eross-msft
ms.date: 08/09/2018
ms.openlocfilehash: a1283c6afcebe7b6abc12f9847865d6305bd3f2c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317271"
---
# <a name="sdn-in-windows-server-overview"></a>Windows Server 中的 SDN 概觀

>適用於：Windows Server (半年通道)、Windows Server 2016


軟體定義網路功能 (SDN) 提供的方法可讓您集中設定及管理實體與虛擬網路裝置，例如您資料中心內的路由器、交換器與閘道。 您可以使用現有的 SDN 相容裝置，在虛擬網路和實體網路之間取得更深入的整合。 Hyper-v 虛擬交換器、Hyper-v 網路虛擬化和 RAS 閘道等虛擬網路元素，都是設計成 SDN 基礎結構的整數元素。 

>[!Note]
>執行 SDN 基礎結構伺服器的 hyper-v 主機和虛擬機器（Vm）（例如網路控制卡和軟體負載平衡節點）必須安裝 Windows Server 2016 Datacenter edition。 
>
>僅包含連線到 SDN 控制網路之租使用者工作負載 Vm 的 hyper-v 主機可以使用 Windows Server 2016 Standard edition。

SDN 是可行的，因為網路平面已不再系結至網路裝置本身。 不過，其他實體（例如 System Center 2016 之類的資料中心管理軟體）會使用網路平面。 SDN 可讓您以動態方式管理您的資料中心網路，提供自動化、集中式的方法來符合應用程式和工作負載的需求。 

您可以使用 SDN 來執行下列動作：

- 以動態方式建立、保護及連接您的網路，以符合您應用程式不斷演進的需求
- 以非干擾性的方式加速部署工作負載
- 包含在網路上散佈的安全性弱點
- 定義和控制負責管理實體和虛擬網路的原則 
- 大規模地以一致的方式執行網路原則

SDN 可讓您完成所有這項工作，同時也能降低整體的基礎結構成本。



## <a name="contact-the-datacenter-and-cloud-networking-product-team"></a>聯絡資料中心和雲端網路產品小組

如果您想要與 Microsoft 或其他 SDN 客戶討論 SDN 技術，有各種不同的方法可建立連絡人。

如需詳細資訊，請參閱[聯絡資料中心和雲端網路小組](contact-sdn-team.md)。
