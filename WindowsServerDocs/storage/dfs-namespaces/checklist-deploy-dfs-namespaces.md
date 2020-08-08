---
title: 檢查清單：部署 DFS 命名空間
description: 本文說明如何設定和部署 DFS 命名空間。
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 43cb085eb8af627609371f37f61eab22be91d606
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87957716"
---
# <a name="checklist-deploy-dfs-namespaces"></a>檢查清單：部署 DFS 命名空間

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

分散式檔案系統 (DFS) 命名空間與 DFS 複寫可用來發佈文件、軟體和特定業務資料給整個組織的使用者。 雖然單靠 DFS 複寫來分送資料即已足夠，但是您可以使用 DFS 命名空間來設定命名空間，讓其中的同一資料夾由多部伺服器裝載，而每部伺服器各保留一份該資料夾的更新複本。 這可增加資料可用性並且跨伺服器分散用戶端負載。

使用者在瀏覽命名空間中的資料夾時，並不知道資料夾裝載於多部伺服器。 當使用者開啟資料夾時，用戶端電腦會自動轉介至其網站上的伺服器。 如果沒有可用的相同網站伺服器，您可以設定命名空間為轉介用戶端至 Active Directory Directory Services (AD DS) 中所定義具有最低連線成本的伺服器。

若要部署 DFS 命名空間，請執行下列工作：

-   檢視 DFS 命名空間的概念與需求。
[DFS 命名空間的概觀](dfs-overview.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)
-   [建立 DFS 命名空間](create-a-dfs-namespace.md)
-   將現有的網域型命名空間移轉到 Windows Server 2008 模式網域型命名空間。 [將網域型命名空間遷移至 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)
-   將命名空間伺服器新增至網域型命名空間，提高可用性。 [新增命名空間伺服器至網域型 DFS 命名空間](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   新增資料夾到命名空間。 [在 DFS 命名空間中建立資料夾](create-a-folder-in-a-dfs-namespace.md)
-   新增資料夾目標到命名空間中的資料夾。 [新增資料夾目標](add-folder-targets.md)
-   使用 DFS 複寫在資料夾目標之間複寫內容 (選擇性)。 [使用 DFS 複寫複寫資料夾目標](replicate-folder-targets-using-dfs-replication.md)


## <a name="additional-references"></a>其他參考資料

-   [命名空間](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771914(v=ws.11))
-   [檢查清單：調整 DFS 命名空間](checklist-tune-a-dfs-namespace.md)
-   [複寫](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770278(v=ws.11))
