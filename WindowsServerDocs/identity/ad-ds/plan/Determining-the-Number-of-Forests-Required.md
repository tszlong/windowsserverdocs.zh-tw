---
description: 深入瞭解：判斷所需的樹係數目
ms.assetid: 173b72c1-ac83-4f42-abab-cf58f43769f0
title: 決定所需的樹系數目
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 3bc6de521db406258e213082c3ebc78056419e98
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97046296"
---
# <a name="determining-the-number-of-forests-required"></a>決定所需的樹系數目

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

若要判斷您必須部署的樹係數目，您需要仔細找出並評估組織中每個群組的隔離和自主性需求，然後將這些需求對應至適當的樹系設計模型。

當您決定要為組織部署的樹係數目時，請考慮下列事項：

-   隔離需求會限制您的設計選擇。 因此，如果您找出隔離需求，請確定群組確實需要資料隔離，而且資料自主性不足以滿足其需求。 確定您組織中的各種群組清楚瞭解隔離和自主性的概念。

-   協調設計可能是冗長的流程。 群組可能很難提供擁有權的合約和可用資源的使用。 請確定您有足夠的時間，讓組織中的群組進行適當的研究以找出他們的需求。 設定設計決策的公司期限，並在建立的期限之後取得所有合作物件的共識。

-   判斷要部署的樹係數目牽涉到平衡成本與效益。 單一樹系模型是最符合成本效益的選項，而且需要最少的系統管理負荷。 雖然組織中的群組可能會偏好自發的服務作業，但對於組織來說，從集中且受信任的資訊技術（ (IT) 群組）訂閱服務提供，可能會更符合成本效益。 這可讓群組擁有資料管理，而不需要建立額外的服務管理成本。 將成本與權益進行平衡可能需要來自 executive 贊助者的輸入。

    單一樹系是最容易管理的設定。 它允許在環境內進行最大的合作，因為：

    -   單一樹系中的所有物件都會列在通用類別目錄中。 因此，不需要跨樹系進行同步處理。

    -   不需要管理重複的基礎結構。

-   我們不建議將單一樹系的共同擁有權由兩個不同且自發的 IT 組織。 未來，這兩個 IT 群組的目標可能會變更，因此無法再接受共用控制。

-   我們不建議將服務管理外包給一個以上的外部夥伴。 在不同國家或地區有群組的跨國組織，可能會選擇將服務管理外包給每個國家或地區的不同外部合作夥伴。 因為多個外部夥伴無法彼此隔離，所以一個夥伴的動作可能會影響另一個夥伴的服務，因此很難讓夥伴擁有其服務等級協定的責任。

-   任何時候都只能有一個 Active Directory 網域的實例存在。 Microsoft 不支援在嘗試建立相同網域的第二個實例時，從一個網域複製、分割或複製網域控制站。 如需這項限制的詳細資訊，請參閱下一節。

## <a name="restructuring-limitations"></a>重建限制
當公司取得另一個公司、營業單位或產品線時，採購公司可能也會想要從賣方取得對應的 IT 資產。 具體而言，買方可能會想要取得部分或所有的網域控制站，這些網域控制站會裝載使用者帳戶、電腦帳戶，以及對應至要取得之商務資產的安全性群組。 買方取得儲存在賣方 Active Directory 樹系之 IT 資產的唯一支援方法如下：

1.  取得樹系的唯一實例，包括賣方整個樹系中的所有網域控制站和目錄資料。

2.  將所需的目錄資料從賣方的樹系或網域遷移至一或多個買方網域。 這類遷移的目標可能是全新的樹系，或已部署在買方樹系中的一或多個現有網域。

這項支援限制存在的原因是：

-   在建立樹系期間，會將唯一的身分識別指派給 Active Directory 樹系中的每個網域。 將網域控制站從原始網域複製到複製的網域，可危及網域和樹系的安全性。 對原始網域和複製的網域的威脅包括下列各項：

    -   共用可用於存取資源的密碼

    -   有關特殊許可權使用者帳戶和群組的深入解析

    -   IP 位址與電腦名稱稱的對應

    -   如果已複製網域中的網域控制站與原始網域的網域控制站建立網路連線，則會新增、刪除和修改目錄資訊

-   複製的網域共用一般的安全性身分識別;因此，即使其中一個或兩個網域都已重新命名，也無法在兩者之間建立信任關係。

## <a name="in-this-section"></a>本節內容

-   [樹系設計模型](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770439(v=ws.10))

-   [將設計需求對應至樹系設計模型](Forest-Design-Models.md)

-   [使用組織網域樹系模型](../../ad-ds/plan/Using-the-Organizational-Domain-Forest-Model.md)

