---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: 樹系設計模型
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aca59a31f75628b311a92bd842d93ee91408dc4c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819789"
---
# <a name="forest-design-models"></a>樹系設計模型

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您可以在 Active Directory 環境中套用下列三個樹系設計模型的其中一個：  
  
-   組織的樹系模型  
  
-   資源樹系模型  
  
-   限制的存取樹系模型  
  
很可能需要使用這些模型的組合，以符合您組織中的所有不同群組的需求。  
  
## <a name="organizational-forest-model"></a>組織的樹系模型  
在組織的樹系模型中，使用者帳戶和資源可包含樹系中，並分開管理。 組織的樹系可以用來提供服務自治 」、 服務隔離或資料隔離，如果樹系設定為該樹系之外的任何人都能防止存取。  
  
如果組織的樹系中的使用者需要存取其他樹系 （或相反） 中的資源，可以一個組織的樹系和其他樹系之間建立信任關係。 這可讓管理員在授與其他樹系中資源的存取權。 下圖顯示組織的樹系模型。  
  
![樹系設計模型](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
每個 Active Directory 設計包含至少一個組織的樹系。  
  
## <a name="resource-forest-model"></a>資源樹系模型  
在資源樹系模型中，另一個樹系用來管理資源。 資源樹系並不會包含所需的服務管理和所需提供替代的存取權的資源，在該樹系中，如果無法使用組織的樹系中的使用者帳戶以外的使用者帳戶。 建立樹系信任，讓來自其他樹系的使用者可以存取資源樹系中所包含的資源。 下圖顯示資源樹系模型。  
  
![樹系設計模型](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
資源樹系可提供用來保護需要維護狀態的高可用性的網路區域的服務隔離。 比方說，如果您的公司包含製造工廠需要能夠繼續運作上其餘的網路發生問題時，您可以建立個別的資源樹系製造群組。  
  
## <a name="restricted-access-forest-model"></a>限制的存取樹系模型  
在限制的存取樹系模型中，建立個別的樹系包含使用者帳戶和必須與組織的其餘部分隔離的資料。 限制的存取樹系可提供在某些情況下，洩露專案資料的結果是嚴重的資料隔離。 下圖顯示限制的存取樹系模型。  
  
![樹系設計模型](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
因為沒有信任關係存在，來自其他樹系的使用者無法獲得存取權受限制的資料。 在此模型中，使用者會在分類資料的存取權的限制的存取樹系中擁有一般組織資源的存取權的組織的樹系中的帳戶和不同的使用者帳戶。 這些使用者必須擁有兩個個別的工作站，一個連線到組織的樹系和其他已連線的限制的存取樹系。 這可防止從一個樹系中的服務系統管理員才能存取受限制的樹系中的工作站的可能性。  
  
在極端的情況下，可能會限制的存取樹系維護個別的實體網路上。 有時候分類的 government 專案工作的組織會維護以符合安全性需求的不同網路上的限制的存取樹系。  
  


