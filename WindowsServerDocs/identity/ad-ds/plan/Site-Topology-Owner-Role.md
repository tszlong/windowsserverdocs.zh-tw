---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: 站台拓撲擁有者角色
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: da0b34f8e2b14d03ef7f65417531aae674977969
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967695"
---
# <a name="site-topology-owner-role"></a>站台拓撲擁有者角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理網站拓撲的系統管理員稱為「網站拓撲擁有者」。 網站拓朴擁有者瞭解網站間網路的狀況，而且有權變更 Active Directory Domain Services (AD DS) 中的設定，以執行網站拓撲的變更。 網站拓朴的變更會影響複寫拓撲中的變更。 網站拓朴擁有者的責任包括：

-   控制網路連線能力變更時的網站拓撲變更。

-   從網路群組取得及維護網路連線和路由器的相關資訊。 網站拓撲擁有者必須維護子網位址、子網路遮罩以及每個所屬位置的清單。 網站拓撲擁有者也必須瞭解影響網站拓朴的網路速度和容量相關問題，以有效地設定站台連結的成本。

-   如果網域控制站的 IP 位址變更為不同網站中的不同子網，或子網本身已指派給不同的網站，請移動在網站之間代表網域控制站的 Active Directory 伺服器物件。 不論是哪一種情況，網站拓朴擁有者都必須手動將網域控制站的 Active Directory 伺服器物件移至新網站。



