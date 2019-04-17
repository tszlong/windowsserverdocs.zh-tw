---
ms.assetid: c7f49a65-c3eb-4383-99d3-756aa8c79fc0
title: "森林設計模型"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b24eba7173a4878102ac7b7a84f7322425df8a52
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="forest-design-models"></a>森林設計模型

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您可以將其中一項下列三種樹系設計機型套用 Active Directory 環境中：  
  
-   組織樹系模型  
  
-   資源森林模型  
  
-   限制的存取森林模型  
  
很有可能，您將需要使用這些型號的組合，您在組織中所有不同群組的需求。  
  
## <a name="organizational-forest-model"></a>組織樹系模型  
在 [組織樹系型號，帳號資源森林中所包含和獨立管理。 組織的樹系可用於提供服務自主、 服務隔離或資料隔離，如果樹系設定以防止外面樹系的人的存取。  
  
若組織森林中的使用者存取其他森林 （反之亦然） 中的資源，可以建立信任關係組織樹和其他的樹系。 這可讓系統管理員權限授與其他森林中的資源。 下圖顯示組織的樹系模型。  
  
![森林設計模型](media/Forest-Design-Models/b1ddb47e-78a5-49c7-bb21-d7421b7b84b8.gif)  
  
每個 Active Directory 設計包含至少組織的樹系。  
  
## <a name="resource-forest-model"></a>資源森林模型  
資源森林型號，在不同的樹系用來管理資源。 資源森林不包含帳號以外所需的服務管理，以及需要提供其他資源的存取權的樹系，如果組織森林中的使用者帳號無法使用。 信任的樹系所建立，讓使用者從其他森林可以存取資源森林中所包含的資源。 下圖顯示資源森林模型。  
  
![森林設計模型](media/Forest-Design-Models/c0b348a6-958c-4fc5-9035-e2d2a54d5573.gif)  
  
資源森林提供服務獨立用來保護的網路，必須維持可用性狀態的區域。 例如，如果您的公司包含製造設備需要繼續運作，在網路上的其他問題時，，您可以建立不同的資源樹系製造群組。  
  
## <a name="restricted-access-forest-model"></a>限制的存取森林模型  
限制的存取權的樹系型號，在不同的樹系建立包含帳號及必須隔離的其餘的組織的資料。 限制的存取森林提供獨立情形何處嚴重危害專案資料的結果中的資料。 下圖顯示限制的存取森林模型。  
  
![森林設計模型](media/Forest-Design-Models/e49cfc8c-a58a-4386-93bd-d4a6ee00f89c.gif)  
  
從其他樹系使用者無法授與限制資料的存取權因為信任不存在。 在此模式，使用者會有一般組織資源存取組織森林中的 account 和另外帳號限制的存取樹系用於機密資料的存取權。 這些使用者必須要有兩個不同的工作站、 連接到組織的樹系和其他連接到限制的存取權的樹系。 這會防止的樹系服務系統管理員可以存取工作站限制森林中的可能性。  
  
極端萬一，可能會不同實體網路上限制的存取樹系維護。 有時候分類的政府專案運作的組織維持在不同的網路，以符合安全性需求上限制的存取森林。  
  


