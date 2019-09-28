---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: 計畫部署 AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed2082706975d58a1535aaeb61e6c5283d23306a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359496"
---
# <a name="planning-to-deploy-ad-fs"></a>計畫部署 AD FS


在您收集環境的相關資訊，並根據[Windows Server 2012 中的 AD FS 設計指南](https://technet.microsoft.com/library/dd807036.aspx)中的指導方針來決定 ACTIVE DIRECTORY 同盟服務 @no__t 0AD FS @ no__t-1 設計之後，就可以開始規劃下列部署：貴組織的 AD FS 設計。 透過本主題中的完整設計和資訊，您可以決定要執行哪些工作，以便在組織中部署 AD FS。  
  
## <a name="reviewing-your-ad-fs-design"></a>檢閱您的 AD FS 設計  
如果為您的組織建立原始 AD FS 設計的設計小組與實際實施部署的部署小組不同，請確定部署小組會向設計團隊審查最終的設計。 檢閱有關設計的下列各點：  
  
-   設計團隊決定在您的公司網路或周邊網路中放置同盟伺服器之最佳實體拓撲的策略。 部署小組可以藉由查看 AD FS 設計指南中的下列主題來參考此主題的相關檔：  
  
    -   [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [規劃同盟伺服器的位置](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [規劃同盟伺服器 Proxy 的位置](https://technet.microsoft.com/library/dd807130.aspx)  
  
    設計團隊可能將同盟伺服器或同盟伺服器 Proxy 放置位置的主題留給部署團隊決定。 接著，部署團隊必須負責記錄並實作伺服器的實體拓撲。  
  
-   將組織指定為宣告提供者、信賴憑證者或兩者的商業理由，屬於已記錄之 AD FS 設計的範圍。 確定部署團隊的成員瞭解部署 AD FS 的原因，以及與同盟合作關係相關的其他公司或組織。 請確定部署小組的成員也瞭解其他公司或組織 @no__t 0limited 硬體、沒有外部網路環境等的條件約束，以及 @ no__t-1，這可能會以某種方式限制設計的範圍。 如需有關夥伴組織的詳細資訊，請參閱[計畫您的部署](https://technet.microsoft.com/library/dd807083.aspx)。  
  
在設計小組和部署小組同意這些問題之後，他們就可以繼續部署 AD FS 的設計。 如需詳細資訊，請參閱 [實作您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。  
