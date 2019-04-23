---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: 站台拓撲擁有者角色
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832109"
---
# <a name="site-topology-owner-role"></a>站台拓撲擁有者角色

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

管理站台拓撲之系統管理員就是所謂的站台拓撲擁有者。 站台拓撲擁有者了解站台間網路狀況，並有權在 Active Directory 網域服務 (AD DS) 來實作變更站台拓樸中變更設定。 站台拓撲的變更會影響複寫拓撲中的變更。 站台拓撲擁有者的責任包括：  
  
-   控制站台拓撲的變更，如果網路連線能力變更。  
  
-   取得並維護網路連線和路由器，從網路群組的相關資訊。 站台拓撲擁有者必須維護子網路位址、 子網路遮罩和每個所屬的位置的清單。 站台拓撲擁有者也必須了解關於網路速度和容量會影響站台拓樸，有效地設定的站台連結成本的任何問題。  
  
-   移動 Active Directory 伺服器物件，表示如果網域控制站的 IP 位址變更為不同的子網路位於不同的網站，或如果子網路本身指派給不同的站台的站台之間的網域控制站。 任一情況下，站台拓撲擁有者必須手動在網域控制站的 Active Directory 伺服器物件移動到新的站台。  
  


