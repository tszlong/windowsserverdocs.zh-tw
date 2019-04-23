---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: 設計邏輯結構
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 82184fe678c05b1d7458584de8eecd0c07ece02f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832329"
---
# <a name="designing-the-logical-structure"></a>設計邏輯結構

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

Active Directory 網域服務 (AD DS) 可讓組織建立可調整、 安全且可管理的基礎結構，為使用者與資源管理。 它也可讓它們支援目錄功能的應用程式。  
  
設計良好的 Active Directory 邏輯結構提供下列優點：  
  
-   Microsoft Windows 架構的網路包含大量物件的簡化的管理  
  
-   合併的網域結構和降低的管理成本  
  
-   委派資源，因為適當的系統管理控制的能力  
  
-   減少的網路頻寬的影響  
  
-   簡化的資源共用  
  
-   最佳搜尋效能  
  
-   低擁有權總成本  
  
設計良好的 Active Directory 邏輯結構有助於為群組原則; 這類功能的有效整合桌面鎖定;軟體發佈;和您系統的使用者、 群組、 工作站和伺服器管理作業。 此外，審慎設計的邏輯結構有助於整合 Microsoft 和非 Microsoft 應用程式和服務，例如 Microsoft Exchange Server、 公開金鑰基礎結構 (PKI)，並以網域為基礎的分散式檔案系統 (DFS)。  
  
當您設計 Active Directory 邏輯結構，才能部署 AD DS 時，您可以最佳化您的部署程序，才能善加利用 Active Directory 功能。 若要設計的 Active Directory 邏輯結構，您的設計團隊第一次識別貴組織的需求，，然後根據這項資訊會決定如何放置樹系和網域界限。 然後，設計團隊決定如何設定以符合需求的樹系的網域名稱系統 (DNS) 環境。 最後，設計團隊會識別所需的組織中資源的管理委派組織單位 (OU) 結構。  
  
## <a name="in-this-guide"></a>本指南內容  
  
-   [了解 Active Directory 邏輯模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [識別部署專案參與者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [建立樹系設計](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [建立網域設計](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [建立組織單位設計](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [附錄 a:DNS 清查](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


