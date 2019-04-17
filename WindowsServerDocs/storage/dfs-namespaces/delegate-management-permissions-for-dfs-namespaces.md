---
title: "委派 DFS 命名空間的管理權限"
description: "本文描述如何委派 DFS 命名空間的管理權限，以及哪些群組預設可以執行命名空間工作"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e584b49639a83e4ab1da142a999741ae4ac7ff84
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="delegate-management-permissions-for-dfs-namespaces"></a>委派 DFS 命名空間的管理權限

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2、Windows Server 2008

下表描述預設可以執行基本命名空間工作的群組，以及委派執行這些工作之能力的方法：

|工作 | 預設可以執行此工作的群組 | 委派方法 |
|---|---|---|
|建立網域型命名空間|設定命名空間之網域中的 Domain Admins 群組|在主控台樹狀目錄的 **\[命名空間\]** 節點上按一下滑鼠右鍵，然後按一下 **\[委派管理權限\]**。 或使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell Cmdlet (於 Windows Server 2012 中引進)。 您也必須新增使用者至命名空間伺服器上的本機系統管理員群組。|
|新增命名空間伺服器至網域型命名空間|設定命名空間之網域中的 Domain Admins 群組| 在主控台樹狀目錄的網域型命名空間上按一下滑鼠右鍵，然後按一下 **\[委派管理權限\]**。 或使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell Cmdlet (於 Windows Server 2012 中引進)。 您也必須新增使用者至要新增的命名空間伺服器上的本機系統管理員群組。|
|管理網域型命名空間|每個命名空間伺服器上的本機系統管理員群組| 在主控台樹狀目錄的網域型命名空間上按一下滑鼠右鍵，然後按一下 **\[委派管理權限\]**。 |
|建立獨立命名空間|命名空間伺服器上的本機系統管理員群組| 新增使用者至命名空間伺服器上的本機系統管理員群組。 |
|管理獨立命名空間*|命名空間伺服器上的本機系統管理員群組| 在主控台樹狀目錄的獨立命名空間上按一下滑鼠右鍵，然後按一下 **\[委派管理權限\]**。 或使用 [Set-DfsnRoot GrantAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot) 和 [Set-DfsnRoot RevokeAdminAccounts](https://technet.microsoft.com/itpro/powershell/windows/dfsn/set-dfsnroot)。 Windows PowerShell Cmdlet (於 Windows Server 2012 中引進)。|
|建立複寫群組或在資料夾上啟用 DFS 複寫|設定命名空間之網域中的 Domain Admins 群組| 在主控台樹狀目錄的 \[複寫\] 節點上按一下滑鼠右鍵，然後按一下 **\[委派管理權限\]**。 |

<br />

\*管理獨立命名空間的委派管理權限無法使用 **\[委派\]** 索引標籤來授與使用者檢視和管理安全性的能力，除非使用者是命名空間伺服器上本機系統管理員群組的成員。 這個問題是因為 DFS 管理嵌入式管理單元無法從登錄擷取獨立命名空間的判別存取控制清單 (DACL)。 若要讓嵌入式管理單元顯示委派資訊，您必須遵循 Microsoft<sup>®</sup> 知識庫文章：[KB314837：如何管理遠端登錄存取權](http://go.microsoft.com/fwlink?linkid=46803)中的步驟執行