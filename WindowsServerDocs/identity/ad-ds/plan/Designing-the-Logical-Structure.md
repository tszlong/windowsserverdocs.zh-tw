---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: 設計邏輯結構
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 8d72d7ed9617d18b42f1be10daeafbac994dad88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402636"
---
# <a name="designing-the-logical-structure"></a>設計邏輯結構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services （AD DS）可讓組織為使用者與資源管理建立可擴充、安全且可管理的基礎結構。 它也可讓他們支援啟用目錄的應用程式。  
  
設計良好的 Active Directory 邏輯結構提供下列優點：  
  
-   簡化包含大量物件的 Microsoft Windows 網路管理  
  
-   合併的網域結構和降低的系統管理成本  
  
-   能夠適當地委派資源的系統管理控制權  
  
-   降低對網路頻寬的影響  
  
-   簡化的資源分享  
  
-   最佳搜尋效能  
  
-   低擁有權總成本  
  
設計良好的 Active Directory 邏輯結構，有助於有效率地將這類功能整合到群組原則;桌面鎖定;軟體發佈;以及使用者、群組、工作站和伺服器系統管理。 此外，仔細設計的邏輯結構可協助整合 Microsoft 和非 Microsoft 應用程式和服務，例如 Microsoft Exchange Server、公開金鑰基礎結構（PKI）和網域型分散式檔案系統（DFS）。  
  
當您在部署 AD DS 之前，設計 Active Directory 邏輯結構時，您可以將部署程式優化，以充分利用 Active Directory 的功能。 若要設計 Active Directory 邏輯結構，您的設計小組會先識別組織的需求，並根據此資訊決定樹系和網域界限的放置位置。 然後，設計小組會決定如何設定網域名稱系統（DNS）環境，以符合樹系的需求。 最後，設計小組會識別委派組織中資源管理所需的組織單位（OU）結構。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [瞭解 Active Directory 的邏輯模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [識別部署專案參與者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [建立樹系設計](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [建立網域設計](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [建立組織單位設計](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [附錄 A：DNS 清查](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


