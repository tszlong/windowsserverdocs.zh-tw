---
description: 深入瞭解：網站拓撲擁有者角色
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: 站台拓撲擁有者角色
author: iainfoulds
ms.author: daveba
manager: daveba
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4cac0905993b1e1b750cdbc9e0a64d17c2637cd6
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049256"
---
# <a name="site-topology-owner-role"></a>站台拓撲擁有者角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理網站拓撲的系統管理員稱為「網站拓撲擁有者」。 網站拓朴擁有者瞭解網站之間網路的條件，而且有權在 Active Directory Domain Services (AD DS) 中變更設定，以執行網站拓撲的變更。 對網站拓撲進行的變更會影響複寫拓撲中的變更。 網站拓朴擁有者的職責包括：

-   在網路連線變更時控制網站拓撲的變更。

-   從網路群組取得和維護網路連接和路由器的相關資訊。 網站拓朴擁有者必須維護子網位址、子網路遮罩及每個所屬位置的清單。 網站拓撲擁有者也必須瞭解任何有關網路速度和容量的問題，這些問題會影響網站拓撲，以有效地設定站台連結的成本。

-   如果網域控制站的 IP 位址變更為不同網站中的不同子網，或子網本身已指派給不同的網站，則移動 Active Directory server 物件，代表網站間的網域控制站。 無論是哪一種情況，網站拓朴擁有者都必須手動將網域控制站的 Active Directory server 物件移至新的網站。



