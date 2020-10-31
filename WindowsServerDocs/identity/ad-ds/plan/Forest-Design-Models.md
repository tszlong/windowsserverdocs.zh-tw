---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: 樹系設計模型
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7382f70d292fdfdc08c3e4e1a24bacb04a7a7642
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93068460"
---
# <a name="forest-design-models"></a>樹系設計模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以在 Active Directory 環境中套用下列三個樹系設計模型的其中一種：

-   組織樹系模型

-   資源樹系模型

-   限制存取樹系模型

您很可能需要使用這些模型的組合，以符合組織中所有不同群組的需求。

## <a name="organizational-forest-model"></a>組織樹系模型
在組織樹系模型中，使用者帳戶和資源會包含在樹系中，並獨立管理。 如果設定樹系以防止存取樹系以外的任何人，則可以使用組織樹系來提供服務自主性、服務隔離或資料隔離。

如果組織樹系中的使用者需要存取其他樹系中的資源 (或反向) ，則可以在一個組織樹系與其他樹系之間建立信任關係。 如此一來，系統管理員就可以將存取權授與其他樹系中的資源。 下圖顯示組織樹系模型。

![樹系設計模型](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)

每個 Active Directory 設計都包含至少一個組織樹系。

## <a name="resource-forest-model"></a>資源樹系模型
在資源樹系模型中，會使用不同的樹系來管理資源。 如果組織樹系中的使用者帳戶變得無法使用，則資源樹系不會包含服務管理所需的使用者帳戶，以及為該樹系中的資源提供替代存取權所需的使用者帳戶。 建立樹系信任，讓其他樹系的使用者可以存取資源樹系中包含的資源。 下圖顯示資源樹系模型。

![樹系設計模型](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)

資源樹系提供服務隔離，用來保護必須維持高可用性狀態的網路區域。 例如，如果您的公司所包含的製造設備需要在網路的其餘部分發生問題時繼續運作，您可以為製造群組建立個別的資源樹系。

## <a name="restricted-access-forest-model"></a>限制存取樹系模型
在限制存取樹系模型中，會建立個別的樹系，以包含必須與組織其餘部分隔離的使用者帳戶與資料。 受限制的存取樹系可在影響專案資料的後果很嚴重的情況下，提供資料隔離。 下圖顯示受限制的存取樹系模型。

![樹系設計模型](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)

因為沒有信任關係，所以無法授與來自其他樹系的使用者存取受限制的資料。 在此模型中，使用者在組織樹系中有一個帳戶可存取一般組織資源，並在限制存取樹系中使用個別的使用者帳戶來存取分類資料。 這些使用者必須有兩個不同的工作站，一個是連接到組織樹系，另一個連線到受限制的存取樹系。 這可防止某個樹系的服務系統管理員存取受限制樹系中的工作站。

在極端情況下，可能會在個別的實體網路上維護限制存取樹系。 在分類的政府專案上工作的組織有時會在不同的網路上維護受限制的存取樹系，以符合安全性需求。



