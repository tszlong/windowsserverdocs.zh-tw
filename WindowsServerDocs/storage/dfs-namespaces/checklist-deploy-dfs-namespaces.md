---
title: 檢查清單：部署 DFS 命名空間
description: 本文說明如何設定和部署 DFS 命名空間。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 7f7cca6b67ff6fa8d81e88323381866315f07f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842119"
---
# <a name="checklist-deploy-dfs-namespaces"></a>檢查清單：部署 DFS 命名空間

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

分散式的檔案系統 (DFS) 命名空間和 DFS 複寫，都可以用來將文件、 軟體和特定的業務資料發佈給整個組織的使用者。 雖然單靠 DFS 複寫不足，無法將資料散發，您可以使用 DFS 命名空間，來設定命名空間，以便資料夾由多部伺服器，每個都持有之資料夾的更新的複本。 這可增加資料可用性並且跨伺服器分散用戶端負載。

使用者在瀏覽命名空間中的資料夾時，並不知道資料夾裝載於多部伺服器。 當使用者開啟資料夾時，用戶端電腦會自動轉介至其網站上的伺服器。 如果沒有相同站台伺服器可供使用，您可以設定要將用戶端轉介到伺服器之最低連線成本 Active Directory 目錄服務 (AD DS) 中所定義的命名空間。

若要部署 DFS 命名空間，請執行下列工作：

-   檢視 DFS 命名空間的概念與需求。
[DFS 命名空間的概觀](dfs-overview.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)
-   [建立 DFS 命名空間](create-a-dfs-namespace.md) 
-   將現有的網域型命名空間移轉到 Windows Server 2008 模式網域型命名空間。 [將網域型命名空間移轉至 Windows Server 2008 模式](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md) 
-   將命名空間伺服器新增至網域型命名空間，提高可用性。 [將命名空間伺服器新增到網域型 DFS 命名空間](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   新增資料夾到命名空間。 [DFS 命名空間中建立資料夾](create-a-folder-in-a-dfs-namespace.md)
-   新增資料夾目標到命名空間中的資料夾。 [新增資料夾目標](add-folder-targets.md)
-   使用 DFS 複寫在資料夾目標之間複寫內容 (選擇性)。 [複寫使用 DFS 複寫的資料夾目標](replicate-folder-targets-using-dfs-replication.md)


## <a name="see-also"></a>另請參閱

-   [命名空間](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [檢查清單：調整 DFS 命名空間](checklist-tune-a-dfs-namespace.md)
-   [複寫](https://technet.microsoft.com/library/cc770278(v=ws.11).aspx)


