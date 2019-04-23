---
title: 新增命名空間伺服器至網域型 DFS 命名空間
description: 本文說明如何使用 DFS 管理，指定其他命名空間伺服器來裝載命名空間。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: bb3b98e1ea687b68bbb87d0da413f9624d336370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853329"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>新增命名空間伺服器至網域型 DFS 命名空間

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

您可以藉由指定其他命名空間伺服器來裝載命名空間，提高網域型命名空間的可用性。

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>若要新增命名空間伺服器至網域型命名空間

若要使用 DFS 管理來新增命名空間伺服器至網域型命名空間，請使用下列程序：

1.  按一下 [開始]，指向 [系統管理工具]，然後按一下 [DFS 管理]。

2.  在主控台樹狀目錄的 **\[命名空間\]** 節點下，於網域型命名空間上按一下滑鼠右鍵，再按一下 **\[新增命名空間伺服器\]**。

3.  輸入另一部伺服器的路徑，或按一下 **\[瀏覽\]** 尋找伺服器。

> [!NOTE]
> 此程序不適用於獨立命名空間，因為它們僅支援單一命名空間伺服器。 若要提高獨立命名空間的可用性，請在新增命名空間精靈中將容錯移轉叢集指定為命名空間伺服器。


> [!TIP]
> 若要使用 Windows PowerShell 新增命名空間伺服器，請使用 [New-DfsnRootTarget Cmdlet](https://docs.microsoft.com/powershell/module/dfsn/set-dfsnroottarget)。 DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [檢閱 DFS 命名空間伺服器需求](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [建立 DFS 命名空間](create-a-dfs-namespace.md)
-   [委派管理 DFS 命名空間的權限](delegate-management-permissions-for-dfs-namespaces.md)

