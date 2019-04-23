---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: 計畫部署 AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831689"
---
# <a name="planning-to-deploy-ad-fs"></a>計畫部署 AD FS

>適用於：Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012


您收集有關您的環境的資訊，並在您決定在 Active Directory Federation Services 之後\(AD FS\)依照中的指導方針的設計[Windows Server 2012 中 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)，您可以開始規劃組織的 AD FS 設計的部署。 完整的設計與本主題中的資訊，您就可以決定要執行部署組織中的 AD FS。  
  
## <a name="reviewing-your-ad-fs-design"></a>檢閱您的 AD FS 設計  
建構的原始 AD FS 設計團隊設計您的組織不同於部署小組，可以實際實作部署，請確定部署團隊檢閱與設計小組的最終設計。 檢閱有關設計的下列各點：  
  
-   設計團隊決定在您的公司網路或周邊網路中放置同盟伺服器之最佳實體拓撲的策略。 本主題的詳細文件可以參照部署小組檢閱 AD FS 設計指南中的下列主題：  
  
    -   [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [規劃同盟伺服器的位置](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [規劃同盟伺服器 Proxy 的位置](https://technet.microsoft.com/library/dd807130.aspx)  
  
    設計團隊可能將同盟伺服器或同盟伺服器 Proxy 放置位置的主題留給部署團隊決定。 接著，部署團隊必須負責記錄並實作伺服器的實體拓撲。  
  
-   將組織指定為宣告提供者、信賴憑證者或兩者的商業理由，屬於已記錄之 AD FS 設計的範圍。 請確定部署團隊的成員了解為什麼 AD FS 部署的原因，以及其他公司或組織為何牽涉到同盟合作關係。 確定部署團隊的成員也了解其他公司或組織的條件約束\(有限的硬體、 無外部網路環境中，等\)，可能會限制以某種方式設計的範圍。 如需有關夥伴組織的詳細資訊，請參閱[計畫您的部署](https://technet.microsoft.com/library/dd807083.aspx)。  
  
之後的設計團隊與部署團隊同意這些問題，就可以繼續使用 AD FS 設計的部署。 如需詳細資訊，請參閱 [實作您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。  
