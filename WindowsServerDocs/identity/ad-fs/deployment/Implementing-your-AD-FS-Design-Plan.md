---
ms.assetid: d04dd17e-a843-46fd-8711-0039918f92d9
title: "實作您 AD FS 設計計畫"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 31c2e048c3c125d0ea60610b049501151d7aa823
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="implementing-your-ad-fs-design-plan"></a>實作您 AD FS 設計計畫

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

下列環境條件和需求的實作 Active Directory 同盟服務 \(AD FS\) 設計計畫的重點：  
  
-   **支援的合作夥伴：**您通常是使用 AD FS 使用合作夥伴。 若要建立的身分聯盟，判斷您想要形成的合作關係與的組織。 基礎 AD FS 部署位於位置之後，使用協力廠商操作包括新增合作夥伴、 移除合作夥伴與更新合作夥伴資訊。 變更合作關係可能的原因。 例如，如果對方發生重大變更其商務，您的組織變得較大的公司或組織聯盟的一部分或由其他公司取得您的組織 AD FS 部署可能需要合作關係更新。 在任何案例中您聯合身分的多個網域中，您必須知道您要支援的網域 \(partners\) 和，表示可能合作夥伴所有其他網域。  
  
-   **支援的應用程式與服務類型︰**某些應用程式和服務需要存取作業系統資源，有些則是 「 宣告注意。 」 請務必以了解類型的應用程式和服務，AD FS 支援，讓您可以制訂管理的需求。  
  
-   **邏輯和實體架構圖片或部署拓撲：**您會需要了解：  
  
    -   是否聯盟伺服器會在一組陣列來說伺服器或單一伺服器上的功能。  
  
    -   在您的網路部署防火牆與 proxy。  
  
    -   資源與使用者是否存取資源組織，或兩個之外，在組織中的位置。  
  
## <a name="how-to-implement-your-ad-fs-design-using-this-guide"></a>如何將您 AD FS 使用實作設計本指南  
若要判斷必須執行每個部署工作順序是實作設計的下一步。 本指南使用檢查清單可協助您逐步不同伺服器和應用程式部署工作所需實作設計計劃。 家長以及子女的檢查清單可視代表必須處理設計在特定 AD FS 的工作順序。  
  
在本區段中的節目表使用下列家長檢查清單熟悉部署工作實作慣用的 AD FS 設計您的組織：  
  
-   [檢查清單︰ 實作 Web SSO 設計](Checklist--Implementing-a-Web-SSO-Design.md)  
  
-   [檢查清單︰ 實作的聯盟的網路 SSO 設計](Checklist--Implementing-a-Federated-Web-SSO-Design.md)  
