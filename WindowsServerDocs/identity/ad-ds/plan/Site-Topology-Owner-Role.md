---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: "網站拓撲擁有者角色"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>網站拓撲擁有者角色

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

管理網站拓撲的系統管理員稱為拓撲擁有者的網站。 網站拓撲擁有者瞭解之網站間網路的條件，並具有權限在 Active Directory Domain Services (AD DS) 實作變更網站拓撲中變更設定。 網站拓撲變更會影響複寫技術的變更。 網站拓撲擁有者的責任包括：  
  
-   如果網路連接變更，控制網站拓撲變更。  
  
-   取得與維護網路連接以及從網路群組路由器的相關資訊。 擁有者的網站拓撲必須維持一份子網路位址，子網路遮罩，與每個所屬的位置。 網站拓撲擁有者也必須了解有關網路速度和容量影響的網站拓撲有效設定的網站連結費用的任何問題。  
  
-   移動 Active Directory 伺服器物件代表網域控制站台如果網域控制站的 IP 位址變更為不同的網站，以不同的子網路，或自行子網路已指派給其他網站。 無論何者，拓撲擁有者的網站必須手動伺服器的 Active Directory 物件的網域控制站移動到新的網站。  
  


