---
title: 建立 DFS 命名空間
description: 本文說明如何建立 DFS 命名空間。
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 90a6c0f72fc1a11d9070fa7866c0b64044131a07
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469713"
---
# <a name="create-a-dfs-namespace"></a>建立 DFS 命名空間

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

若要建立新的命名空間，您可以在安裝 DFS 命名空間角色服務時，使用伺服器管理員來建立命名空間。 您也可以在 Windows PowerShell 工作階段中使用 [New-DfsnRoot Cmdlet](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot)。

DFSN Windows PowerShell 模組於 Windows Server 2012 中引進。

或者，您也可以在安裝角色服務之後，使用下列程序來建立命名空間。

## <a name="to-create-a-namespace"></a>若要建立命名空間

1.  按一下 [**開始**]，然後指向 [**系統管理工具**]，再按一下 [**DFS 管理**]。

2.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[命名空間\]** 節點，然後按一下 **\[新增命名空間\]**。

3.  依照 **\[新增命名空間精靈\]** 中的指示執行。

    若要在容錯移轉叢集上建立獨立命名空間，請在 **\[新增命名空間精靈\]** 的 **\[命名空間伺服器\]** 頁面上指定叢集檔案伺服器執行個體的名稱。

> [!IMPORTANT]
> 除非樹系功能等級是 Windows Server 2003 或更高版本，否則請勿嘗試使用 Windows Server 2008 模式來建立網域型命名空間。 這麼做會造成無法刪除 DFS 資料夾的命名空間，並產生下列錯誤訊息：「無法刪除資料夾。 無法完成此功能。」

## <a name="additional-references"></a>其他參考

-   [部署 DFS 命名空間](deploying-dfs-namespaces.md)
-   [選擇命名空間類型](choose-a-namespace-type.md)
-   [新增命名空間伺服器至網域型 DFS 命名空間](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [委派 DFS 命名空間的管理許可權](delegate-management-permissions-for-dfs-namespaces.md)。


