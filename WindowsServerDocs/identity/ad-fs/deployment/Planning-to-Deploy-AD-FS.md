---
description: 深入瞭解：規劃部署 AD FS
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: 計畫部署 AD FS
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.author: billmath
ms.openlocfilehash: efac16b60fa59fdbd30f1b9636629c97c2072677
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043237"
---
# <a name="planning-to-deploy-ad-fs"></a>計畫部署 AD FS


在您收集環境的相關資訊，並依照 \( \) [Windows Server 2012 的 AD FS 設計指南](../design/ad-fs-design-guide-in-windows-server-2012.md)中的指導方針決定 Active Directory 同盟服務 AD FS 設計之後，您就可以開始規劃組織的 AD FS 設計部署。 使用完成的設計和本主題中的資訊，您可以決定要在組織中部署 AD FS 所要執行的工作。

## <a name="reviewing-your-ad-fs-design"></a>檢閱您的 AD FS 設計
如果為您的組織建立原始 AD FS 設計的設計小組與實際執行部署的部署小組不同，請確定部署小組會使用設計小組來審核最終設計。 檢閱有關設計的下列各點：

-   設計團隊決定在您的公司網路或周邊網路中放置同盟伺服器之最佳實體拓撲的策略。 部署小組可以參閱 AD FS 設計指南中的下列主題，以參閱有關此主題的檔：

    -   [AD FS 設定資料庫的角色](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)

    -   [規劃同盟伺服器的位置](../design/planning-federation-server-placement.md)

    -   [規劃同盟伺服器 Proxy 的位置](../design/planning-federation-server-proxy-placement.md)

    設計團隊可能將同盟伺服器或同盟伺服器 Proxy 放置位置的主題留給部署團隊決定。 接著，部署團隊必須負責記錄並實作伺服器的實體拓撲。

-   將組織指定為宣告提供者、信賴憑證者或兩者的商業理由，屬於已記錄之 AD FS 設計的範圍。 確定部署團隊的成員瞭解部署 AD FS 的原因，以及同盟合作關係中牽涉到的其他公司或組織。 確定部署團隊的成員也瞭解其他公司或組織的 \( 有限硬體、無外部網路環境，以及 \) 可能會以某種方式限制設計領域的限制。 如需有關夥伴組織的詳細資訊，請參閱[計畫您的部署](../design/planning-your-deployment.md)。

設計小組和部署小組同意這些問題之後，就可以繼續部署 AD FS 的設計。 如需詳細資訊，請參閱 [實作您的 AD FS 設計計畫](Implementing-Your-AD-FS-Design-Plan.md)。
