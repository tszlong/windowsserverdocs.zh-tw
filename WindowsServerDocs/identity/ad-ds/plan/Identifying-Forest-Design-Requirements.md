---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: "檢測軍人森林設計需求"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 91d551e5d7b6daa6b1476570dad57e85d3bc163f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="identifying-forest-design-requirements"></a>檢測軍人森林設計需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要建立您的組織的樹系設計，您必須找出您 directory 結構必須符合企業需求。 這牽涉到判斷多少自主性公司的需要以管理他們網路資源，需要隔離群組其他網路上的資源，就不會每個群組中的群組。  
  
Active Directory Domain Services (AD DS) 可讓您設計可容納組織中的多個群組的唯一管理的需求 directory 基礎結構和達到結構和作業獨立之間所需的群組。  
  
您在組織中的群組可能會有一些需求下列類型：  
  
-   **組織結構需求**。 部分的組織可能參與共用的基礎結構儲存費用，但需要從組織的其餘部分獨立運作的能力。 例如研究群組中大型的組織可能需要維護所有自己研究的資料控制。  
  
-   **操作需求**。 組織的一部分可能會將唯一限制 directory 服務設定、 可用性或安全性，或使用應用程式放在 directory 唯一限制。 例如，業務單位組織中的可能部署 directory 支援的應用程式修改 directory 架構不是由其他公司單位部署。 森林中的所有網域之間共用 directory 架構，因為建立多個樹系是一個方案針對此類案例。 在下列組織和案例中找到其他範例：  
  
    -   廢棄組織  
  
    -   控管案例  
  
    -   組織維護 directory 可內部和外部 （例如這些公開存取網際網路上的使用者）  
  
-   **法律要求**。 某些組織必須遵守法律規定特定的方式運作，例如限制存取特定企業合約中所指定的資訊。 某些組織有安全性需求，隔離內部網路上運作。 符合下列需求失敗可能會導致遺失的合約可能法律動作。  
  
確認您的樹系設計需求的包括找出您在組織中的群組的可以信任潛在的樹系擁有和他們服務系統管理員等級，並找出您在組織中的每個群組自主和獨立需求。  
  
設計團隊，必須文件服務，資料管理每個群組中的組織會使用 AD DS 隔離和自主需求。 小組也必須注意限制，可能會影響到 AD DS 部署連接的任何部分。  
  
設計團隊，必須文件服務，資料管理每個群組中的組織會使用 AD DS 隔離和自主需求。 小組也必須注意限制，可能會影響到 AD DS 部署連接的任何部分。 協助您在擬您找出地區試算表，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip).  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [授權範圍服務系統管理員](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [與隔離自主性](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
  


