---
ms.assetid: 9ad81367-f3fe-4b2e-bd7c-5900b2b9f77f
title: "設計的邏輯結構"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8d1b9c27f05faef49f7fd4228f4ebe689b75d30f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="designing-the-logical-structure"></a>設計的邏輯結構

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

Active Directory Domain Services (AD DS) 可讓您組織建立使用者和資源管理延展性、安全且更容易管理基礎結構。 它也會讓它們支援 directory 功能的應用程式。  
  
個精心設計的 Active Directory 邏輯結構提供下列優點：  
  
-   Microsoft windows 的網路包含大量物件簡化的管理  
  
-   整合的網域結構和管理降低的成本  
  
-   委派系統資源，視控制  
  
-   減少的頻寬影響  
  
-   簡化的資源分享  
  
-   搜尋最佳效能  
  
-   低取得成本  
  
個精心設計的 Active Directory 邏輯結構可協助有效整合的群組原則; 此類功能桌面鎖定。軟體 distribution;和使用者、群組、工作站和伺服器管理到您的系統。 此外，仔細設計邏輯結構有助於 integration 的 Microsoft 非 Microsoft 應用程式與服務，例如 Microsoft Exchange Server 公用基礎結構 (PKI)，而且網域型散發檔案系統 (DFS)。  
  
當您設計邏輯結構 Active Directory 部署 AD DS 之前時，您可以充分利用功能 Active Directory 部署程序最佳化。 若要設計的 Active Directory 邏輯結構，您的設計團隊第一次辨識出您的組織的需求，根據這項資訊，決定造成樹系和網域限制的位置。 然後、設計團隊決定如何設定的網域名稱系統」(DNS) 環境樹系的需求。 最後，設計團隊辨識所需委派資源您在組織中管理結構單位（組織單位）。  
  
## <a name="in-this-guide"></a>本指南  
  
-   [了解 Active Directory 邏輯模型](../../ad-ds/plan/Understanding-the-Active-Directory-Logical-Model.md)  
  
-   [檢測軍人部署專案參與者](../../ad-ds/plan/Identifying-the-Deployment-Project-Participants.md)  
  
-   [建立森林設計](../../ad-ds/plan/Creating-a-Forest-Design.md)  
  
-   [建立網域設計](../../ad-ds/plan/Creating-a-Domain-Design.md)  
  
-   [建立 DNS 基礎結構設計](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md)  
  
-   [建立組織單位設計](../../ad-ds/plan/Creating-an-Organizational-Unit-Design.md)  
  
-   [答附錄 DNS 清單](../../ad-ds/plan/Appendix-A--DNS-Inventory.md)  
  


