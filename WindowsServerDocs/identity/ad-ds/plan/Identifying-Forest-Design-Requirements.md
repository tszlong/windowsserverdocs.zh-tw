---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: 識別樹系設計需求
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bf8d9d164bf07151572785cda906be911f97b53e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835329"
---
# <a name="identifying-forest-design-requirements"></a>識別樹系設計需求

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

若要建立您的組織的樹系設計，您必須識別您的目錄結構必須滿足的商務需求。 這牽涉到決定多少自我管理組織的需求來管理他們的網路資源，而且需要找出其資源的其他群組在網路上每個群組中的群組。  
  
Active Directory 網域服務 (AD DS) 可讓您設計目錄基礎結構能夠容納組織內具有特殊管理需求的多個群組，並確保結構化且可運作的獨立群組之間視需要。  
  
在您的組織中的群組可能會有下列類型的需求：  
  
-   **組織結構的需求**。 組織的部分可能會參與共用的基礎結構，以節省成本，但需要能夠獨立作業，從組織的其餘部分。 例如，大型組織中的參考資料群組可能需要維護自己的研究資料的所有控制項。  
  
-   **操作需求**。 組織的其中一部分可能會放在目錄服務設定、 可用性或安全性，unique 條件約束，或使用唯一的條件約束，在目錄的應用程式。 比方說，組織內的個別業務單位可能會部署啟用目錄的應用程式修改的目錄結構描述不會部署其他業務單位。 因為樹系中的所有網域之間共用的目錄結構描述，建立多個樹系是全方位的解決方案，這種情況。 其他範例位於下列組織和案例：  
  
    -   軍事組織  
  
    -   裝載案例  
  
    -   組織所維護的目錄，均可在內部和外部 （例如那些可公開存取的網際網路上的使用者）  
  
-   **法律需求**。 某些組織必須遵守法律規定運作以特定的方式，例如，限制存取特定的商務合約中所指定的資訊。 某些組織有在隔離的內部網路上運作的安全性需求。 為了符合這些需求可能導致遺失之合約和可能合法的動作。  
  
識別您的樹系設計需求的部分牽涉到識別的潛在的樹系擁有者和其的服務系統管理員，可以信任您組織中的群組的程度，並識別每個自治與隔離需求在您的組織中的群組。  
  
設計團隊必須文件的組織想要使用 AD DS 中的每個群組的服務和資料管理的隔離與自我管理需求。 小組必須也請注意任何可能會影響 AD ds 部署連線能力有限的區域。  
  
設計團隊必須文件的組織想要使用 AD DS 中的每個群組的服務和資料管理的隔離與自我管理需求。 小組必須也請注意任何可能會影響 AD ds 部署連線能力有限的區域。 為工作表可協助您記錄您所識別的區域中，下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip 從[工作輔助工具的 Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558)並開啟 樹系設計需求 > (DSSLOGI_2.doc)。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [服務系統管理員領域的授權單位](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [自主性與。隔離](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
