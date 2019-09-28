---
title: 檢查清單：部署 DFS 命名空間
description: 本文說明如何設定和部署 DFS 命名空間。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ab38c41c32ec88285a69fb94e62abc1453ddc3d1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402271"
---
# <a name="checklist-deploy-dfs-namespaces"></a>檢查清單：部署 DFS 命名空間

> 適用於：Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

分散式檔案系統（DFS）命名空間和 DFS 複寫可用來將檔、軟體和企業營運資料發佈給整個組織的使用者。 雖然單獨 DFS 複寫只是用來散發資料，但您可以使用 DFS 命名空間來設定命名空間，讓資料夾由多部伺服器裝載，其中每一個都包含資料夾的更新複本。 這可增加資料可用性並且跨伺服器分散用戶端負載。

使用者在瀏覽命名空間中的資料夾時，並不知道資料夾裝載於多部伺服器。 當使用者開啟資料夾時，用戶端電腦會自動轉介至其網站上的伺服器。 如果沒有相同的網站伺服器可供使用，您可以設定命名空間，將用戶端參照至具有最低連線成本的伺服器（如 Active Directory 目錄服務（AD DS）中所定義）。

若要部署 DFS 命名空間，請執行下列工作：

-   檢視 DFS 命名空間的概念與需求。
[DFS 命名空間總覽](dfs-overview.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)
-   [建立 DFS 命名空間](create-a-dfs-namespace.md) 
-   將現有的網域型命名空間移轉到 Windows Server 2008 模式網域型命名空間。 [將網域型命名空間遷移至 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   將命名空間伺服器新增至網域型命名空間，提高可用性。 [新增命名空間伺服器至網域型 DFS 命名空間](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   新增資料夾到命名空間。 [在 DFS 命名空間中建立資料夾](create-a-folder-in-a-dfs-namespace.md)
-   新增資料夾目標到命名空間中的資料夾。 [新增資料夾目標](add-folder-targets.md)
-   使用 DFS 複寫在資料夾目標之間複寫內容 (選擇性)。 [使用 DFS 複寫複寫資料夾目標](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>另請參閱

-   [命名空間](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [檢查清單：調整 DFS 命名空間](checklist-tune-a-dfs-namespace.md)
-   [複寫](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


