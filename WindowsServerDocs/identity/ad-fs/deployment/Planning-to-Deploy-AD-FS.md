---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: "部署 AD FS 計劃"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>部署 AD FS 計劃

>適用於：Windows Server 2016、Windows Server 2012 R2、Windows Server 2012


您會收集關於您的環境資訊，您可以選擇在 Active Directory 同盟服務 \(AD FS\) 設計依照下列指導方針中的後[Windows Server 2012 中 AD FS 程式設計指南](https://technet.microsoft.com/library/dd807036.aspx)，就可以開始您的組織 AD FS 設計部署計劃。 已完成的設計和本主題中的資訊，您可以判斷部署 AD FS 您在組織中的執行的工作。  
  
## <a name="reviewing-your-ad-fs-design"></a>審查您的 AD FS 設計  
設計建構原始 AD FS 設計團隊組織不同於部署小組的實際會執行部署，請確定部署小組評論設計團隊的最終設計。 檢視關於設計下列重點：  
  
-   若要判斷在您的企業網路或周邊網路聯盟伺服器配置最佳實體拓撲設計團隊的策略。 本主題的文件可以參考部署小組所檢視的下列主題 AD FS 設計節目表中：  
  
    -   [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [規劃伺服器聯盟的位置](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [規劃聯盟 Proxy 伺服器的位置](https://technet.microsoft.com/library/dd807130.aspx)  
  
    很可能設計團隊，可能會使保留聯盟伺服器或聯盟伺服器 proxy 配置部署小組的主題。 部署小組負責然後文件並執行實體拓撲的伺服器。  
  
-   適用於您組織的指定為宣告提供者、信賴，或兩者記載 AD FS 設計的範圍中企業的原因。 確定原因為何要部署 AD FS 與其他公司或組織哪些部署小組的成員，了解聯盟合作關係與有關。 確保部署小組的成員也會了解其他公司或組織限制 \（有限的硬體，不外部環境，以及如此 forth\），可能會限制的一些方式設計範圍。 合作夥伴公司的相關詳細資訊，請查看[規劃您部署](https://technet.microsoft.com/library/dd807083.aspx)。  
  
之後設計團隊和部署團隊同意這些問題，就可以繼續 AD FS 設計的部署。 如需詳細資訊，請查看[實作您 AD FS 設計計劃](Implementing-Your-AD-FS-Design-Plan.md)。  
