---
title: "廣告樹系修復-驗證複寫"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adfs
ms.openlocfilehash: d85336a10e808755677a99777af8ecf5c07b05ab
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="resources-to-verify-replication-is-working"></a>資源驗證複寫運作 

>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2
 
 還原或重新安裝所有網域控制站之後，您可以驗證，AD DS 和 SYSVOL 會復原及複製正確的方式使用**repadmin /replsum**，在 Windows Server 的任何版本上執行。  
  
> [!TIP]
>  您也可以下載並執行[Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)(ADReplStatus)、 免費監視複寫狀態的網域控制站與報告錯誤的工具。 ADReplStatus 需要它已經不會安裝.NET Framework 4。  
  
 請在事件檢視器的事件編號 4602 （或檔案複寫服務 263 13516），這表示 SYSVOL 初始化 DFS 複寫登入。  
  
 如果第一個復原俠登事件 ID 4614 （」 的網域控制站正在等待執行初始複寫。 複寫的資料夾維持在初始同步狀態複寫其合作夥伴使用 「） 中 DFS 複寫登入，然後 263 4602 未顯示，就需要複製 DFSR 來復原 SYSVOL 手動下列步驟執行：  
  
1.  在第一次還原網域控制站 DFSR 事件 4612 出現執行手動授權還原中所述[2218556：如何強迫授權和未經授權的同步 DFSR 複寫的 SYSVOL（例如「D4 /d2」的 FRS) 的](https://support.microsoft.com/kb/2218556)（https://support.microsoft.com/kb/2218556)。  
  
2.  設定**SysvolReady 旗標**1 手動中, 所述[947022 NETLOGON 分享未顯示在您安裝新完整或唯讀為基礎 Windows Server 2008 的網域控制站的 Active Directory Domain Services 之後](https://support.microsoft.com/kb/947022)。  
  
 您也可以建立 DFS 複寫診斷報告。 如需詳細資訊，請查看[診斷報告建立的 DFS 複製](https://technet.microsoft.com/library/cc754227.aspx)和[DFS Step-by-Step 指南 Windows Server 2008 的](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)。 如果執行 Windows Server 2008 R2 的伺服器，您可以使用[dfsrdiag.exe ReplicationState 命令列參數](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  
  
 您也可以執行複寫測試使用 dcdiag.exe 檢查複寫錯誤。 如需詳細資訊，請查看知識庫[文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>後續步驟

- [廣告樹系復原指南](AD-Forest-Recovery-Guide.md)
- [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)
