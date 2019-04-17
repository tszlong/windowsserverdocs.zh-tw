---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: "規劃網域控制站位置"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c6d9ca064699f95390e5b95863a6871ea2dc9d6e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="planning-domain-controller-placement"></a>規劃網域控制站位置

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

您已收集的所有網路資訊會用來設計網站拓撲、計劃要放置網域控制站，包括森林根網域控制站、地區網域控制站之後操作主要角色與通用伺服器。  
  
Windows Server 2008，您也可以利用唯讀網域控制站 (Rodc)。 RODC 是一種全新的網域控制站裝載的 Active Directory 資料庫唯讀磁碟分割。 除了 account 的密碼，RODC 會保留所有的 Active Directory 物件和寫入網域控制站保留屬性。 不過，無法變更已儲存在 RODC 資料庫。 必須在 [寫入網域控制站和再複製到 RODC 變更。  
  
RODC 的設計目的是主要部署遠端或分支 office 環境，通常會有少數使用者、不良實體的安全性、相當不良網路頻寬中樞網站，以及瞭解有限資訊技術 (IT) 的人員。 以改善的安全性及網路資源更有效率存取部署 Rodc 結果。 如需 RODC 功能的詳細資訊，請查看 AD DS: Read-Only 網域控制站 ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616))。 如需有關如何部署 RODC 資訊，Step-by-Step 指南的 Read-Only 網域控制站 ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728))。  
  
> [!NOTE]  
> 本指南不解釋您如何判斷適當的數字的網域控制站在每個網站每個網域網域控制站的硬體需求。  
  
## <a name="in-this-section"></a>在本區段中  
  
-   [規劃森林根網域控制站的位置](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [規劃區域的網域控制站位置](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [規劃通用伺服器位置](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [規劃作業角色位置](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


