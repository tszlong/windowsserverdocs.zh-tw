---
title: 新增命名空間伺服器至網域型 DFS 命名空間
description: 本文說明如何使用 DFS 管理，指定其他命名空間伺服器來裝載命名空間。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f00a6419bae1951a7c1597212d3c37676a4db90e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961250"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>新增命名空間伺服器至網域型 DFS 命名空間

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

您可以藉由指定其他命名空間伺服器來裝載命名空間，提高網域型命名空間的可用性。

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>若要新增命名空間伺服器至網域型命名空間

若要使用 DFS 管理來新增命名空間伺服器至網域型命名空間，請使用下列程序：

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於網域型命名空間上按一下滑鼠右鍵，再按一下 **\[新增命名空間伺服器\]**。

3.  輸入另一部伺服器的路徑，或按一下 **\[瀏覽\]** 尋找伺服器。

> [!NOTE]
> 此程序不適用於獨立命名空間，因為它們僅支援單一命名空間伺服器。 若要提高獨立命名空間的可用性，請在新增命名空間精靈中將容錯移轉叢集指定為命名空間伺服器。


> [!TIP]
> 若要使用 Windows PowerShell 新增命名空間伺服器，請使用 [New-DfsnRootTarget Cmdlet](/powershell/module/dfsn/new-dfsnroottarget)。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

## <a name="additional-references"></a>其他參考

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [檢視 DFS 命名空間伺服器需求](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753448(v=ws.11))
-   [建立 DFS 命名空間](create-a-dfs-namespace.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)
