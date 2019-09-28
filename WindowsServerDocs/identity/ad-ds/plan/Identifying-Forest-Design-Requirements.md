---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: 識別樹系設計需求
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 41caeca82819eaea3d86d5f1eb4883ab8bbf53cc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408794"
---
# <a name="identifying-forest-design-requirements"></a>識別樹系設計需求

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要為您的組織建立樹系設計，您必須識別目錄結構需要配合的商務需求。 這牽涉到決定組織中的群組需要多少獨立性來管理其網路資源，以及每個群組是否需要在網路上將其資源與其他群組隔離。  
  
Active Directory Domain Services （AD DS）可讓您設計一個目錄基礎結構，以容納組織內具有獨特管理需求的多個群組，以及達成群組之間的結構化和操作獨立性視需要。  
  
您組織中的群組可能會有下列部分類型的需求：  
  
-   **組織結構需求**。 組織的各部分可能會參與共用的基礎結構，以節省成本，但需要能夠獨立操作組織的其他部分。 例如，大型組織內的研究群組可能需要維護所有自己研究資料的控制權。  
  
-   **操作需求**。 組織的其中一個部分可能會在目錄服務設定、可用性或安全性上放置獨特的條件約束，或使用在目錄上放置唯一條件約束的應用程式。 例如，組織內的個別業務單位可能會部署已啟用目錄的應用程式，以修改不是由其他業務單位部署的目錄架構。 因為樹系中的所有網域之間共用目錄架構，所以建立多個樹系是適用于這類案例的一個解決方案。 其他範例可在下列組織和案例中找到：  
  
    -   軍事組織  
  
    -   裝載案例  
  
    -   維護一個可供內部和外部使用之目錄的組織（例如，可由網際網路上的使用者公開存取）  
  
-   **法律需求**。 有些組織會以特定方式來操作法律需求，例如，限制存取商務合約中指定的特定資訊。 有些組織具有在隔離的內部網路上操作的安全性需求。 若不符合這些需求，可能會導致合約遺失，並可能會造成合法的行動。  
  
識別樹系設計需求的一部分，包括識別組織中的群組可以信任的可能樹系擁有者及其服務系統管理員，以及識別各自的獨立性和隔離需求組織中的群組。  
  
設計小組必須記錄組織中想要使用 AD DS 之每個群組的服務和資料管理的隔離和獨立性需求。 小組也必須注意可能會影響 AD DS 部署的任何受限連線區域。  
  
設計小組必須記錄組織中想要使用 AD DS 之每個群組的服務和資料管理的隔離和獨立性需求。 小組也必須注意可能會影響 AD DS 部署的任何受限連線區域。 如需協助您記載所識別區域的工作表，請從[適用于 Windows Server 2003 部署套件的作業輔助](https://go.microsoft.com/fwlink/?LinkID=102558)下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services，並開啟「樹系設計需求」（DSSLOGI_2）。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [授權單位的服務管理員範圍](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)  
  
-   [自治與隔離](../../ad-ds/plan/Autonomy-vs.-Isolation.md)  
