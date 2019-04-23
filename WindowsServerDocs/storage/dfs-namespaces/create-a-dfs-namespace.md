---
title: 建立 DFS 命名空間
description: 本文說明如何建立 DFS 命名空間。
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4256e124e75be72f94cbd35c182edfe38e92bc90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847499"
---
# <a name="create-a-dfs-namespace"></a>建立 DFS 命名空間

> 適用於：Windows Server 2019，Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2、 Windows Server 2008

若要建立新的命名空間，您可以在安裝 DFS 命名空間角色服務時，使用伺服器管理員來建立命名空間。 您也可以在 Windows PowerShell 工作階段中使用 [New-DfsnRoot Cmdlet](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot)。 

DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。 

或者，您也可以在安裝角色服務之後，使用下列程序來建立命名空間。

## <a name="to-create-a-namespace"></a>若要建立命名空間

1.  按一下 [開始]，指向 [系統管理工具]，然後按一下 [DFS 管理]。

2.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[命名空間\]** 節點，然後按一下 **\[新增命名空間\]**。

3.  依照 **\[新增命名空間精靈\]** 中的指示執行。

    若要在容錯移轉叢集上建立獨立命名空間，請在 **\[新增命名空間精靈\]** 的 **\[命名空間伺服器\]** 頁面上指定叢集檔案伺服器執行個體的名稱。

> [!IMPORTANT]
> 請勿嘗試建立使用 Windows Server 2008 模式，除非樹系功能等級是 Windows Server 2003 或更高版本的網域型命名空間。 如此一來，可能會導致命名空間，您無法刪除 DFS 資料夾，進而產生下列錯誤訊息：「 無法刪除資料夾。 無法完成此功能。」

## <a name="see-also"></a>另請參閱

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)
-   [將命名空間伺服器新增到網域型 DFS 命名空間](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [委派 DFS 命名空間的管理權限](delegate-management-permissions-for-dfs-namespaces.md)。


