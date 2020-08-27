---
ms.assetid: 7d957ebb-3476-49d8-b00b-6e93b4a94778
title: 識別樹系設計需求
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/07/2018
ms.topic: article
ms.openlocfilehash: 9c5278fa01d34b5ed0bf77153dce1575ee6ac509
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939078"
---
# <a name="identifying-forest-design-requirements"></a>識別樹系設計需求

> 適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要為您的組織建立樹系設計，您必須識別您的目錄結構需要配合的商務需求。 這包括決定您組織中的群組需要多少自主性來管理其網路資源，以及每個群組是否需要在網路上隔離其資源與其他群組。

Active Directory Domain Services (AD DS) 可讓您設計一個目錄基礎結構，以容納組織內有獨特管理需求的多個群組，並視需要達成群組之間的結構化和操作獨立性。

您組織中的群組可能會有下列幾種類型的需求：

- **組織結構需求**。 組織的各部分可能會參與共用基礎結構來節省成本，但需要能夠與組織的其他部分分開運作。 例如，大型組織內的研究小組可能需要保有所有自己研究資料的控制權。

- **營運需求**。 組織的一個部分可能會在目錄服務設定、可用性或安全性上放置唯一的條件約束，或使用在目錄上放置唯一限制的應用程式。 例如，組織內的個別業務單位可能會部署啟用目錄的應用程式，而這些應用程式會修改其他業務單位未部署的目錄架構。 因為樹系中所有網域之間共用了目錄架構，所以建立多個樹系是適用于這類案例的解決方案之一。 其他範例可在下列組織和案例中找到：

    - 軍事組織

    - 裝載案例

    - 組織會維護可在內部和 (外部使用的目錄，例如可從網際網路上的使用者公開存取的目錄) 

- **法律需求**。 某些組織有法律需求，以特定方式運作，例如，限制存取商務合約中指定的特定資訊。 某些組織有安全性需求，可在隔離的內部網路上運作。 若無法符合這些需求，可能會導致合約遺失，並可能會導致法律的動作。

識別您的樹系設計需求的一部分，就是識別您組織中的群組可以信任潛在樹系擁有者及其服務系統管理員，以及識別您組織中每個群組的自主性和隔離需求的程度。

設計小組必須針對組織中想要使用 AD DS 的每個群組，記錄服務和資料管理的隔離和自主性需求。 小組也必須注意任何可能會影響 AD DS 部署的有限連線區域。

設計小組必須針對組織中想要使用 AD DS 的每個群組，記錄服務和資料管理的隔離和自主性需求。 小組也必須注意任何可能會影響 AD DS 部署的有限連線區域。 如需協助您記錄所識別區域的工作表，請從 [Windows Server 2003 部署套件的工作輔助](https://microsoft.com/download/details.aspx?id=9608) 下載 Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip，並開啟「樹系設計需求」 ( # A1) 。

## <a name="in-this-section"></a>本節內容

- [授權單位的服務管理員範圍](../../ad-ds/plan/Service-Administrator-Scope-of-Authority.md)

- [自治與隔離](../../ad-ds/plan/Autonomy-vs.-Isolation.md)
