---
ms.assetid: 692bd2af-deee-44cf-9af9-f364677e267f
title: 規劃設置網域控制站
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 68406d4973dd585bf0a98562c987c1b1512c095c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883359"
---
# <a name="planning-domain-controller-placement"></a>規劃設置網域控制站

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

您已收集到的所有網路資訊會用來設計您的站台拓撲之後，規劃您要將網域控制站，包括樹系根網域控制站、 地區網域控制站、 操作主機角色持有者，以及通用類別目錄伺服器。  
  
在 Windows Server 2008 中，您也可以利用唯讀網域控制站 (Rodc)。 RODC 是新型的網域控制站，裝載 Active Directory 資料庫的唯讀分割。 除了帳戶密碼，RODC 會保存所有的 Active Directory 物件和可寫入網域控制站保存的屬性。 不過，無法變更資料庫儲存在 RODC 上。 必須在可寫入網域控制站上所做的變更，並將其然後複寫回 RODC。  
  
RODC 的主要設計目的部署於遠端或分公司環境，通常具有相對較少的使用者、 較不嚴謹的實體安全性、 相對較差的網路頻寬，中樞站台，並瞭解有限的資訊的人員技術 (IT)。 部署 Rodc 結果中改進的安全性及更有效率地存取網路資源。 如需 RODC 功能的詳細資訊，請參閱 < AD DS:唯讀網域控制站 ([https://go.microsoft.com/fwlink/?LinkID=106616](https://go.microsoft.com/fwlink/?LinkID=106616))。 如何部署 RODC 的相關資訊，請參閱逐步指南，唯讀網域控制站 ([https://go.microsoft.com/fwlink/?LinkID=92728](https://go.microsoft.com/fwlink/?LinkID=92728))。  
  
> [!NOTE]  
> 本指南並未說明如何決定適當的網域控制站和網域控制站硬體需求，每個站台中的每個網域的數目。  
  
## <a name="in-this-section"></a>本節內容  
  
-   [規劃放置樹系根網域控制站](../../ad-ds/plan/Planning-Forest-Root-Domain-Controller-Placement.md)  
  
-   [規劃放置區域網域控制站](../../ad-ds/plan/Planning-Regional-Domain-Controller-Placement.md)  
  
-   [規劃通用類別目錄伺服器的位置](../../ad-ds/plan/Planning-Global-Catalog-Server-Placement.md)  
  
-   [規劃操作主機角色定位](../../ad-ds/plan/Planning-Operations-Master-Role-Placement.md)  
  


