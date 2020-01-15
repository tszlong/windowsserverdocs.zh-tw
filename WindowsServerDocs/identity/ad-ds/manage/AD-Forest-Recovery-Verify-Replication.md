---
title: AD 樹系復原-驗證複寫
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: f6bee5164849d6643c1744ce121b9ce91b5e7f7f
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949044"
---
# <a name="resources-to-verify-replication-is-working"></a>確認複寫正在運作的資源 

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

還原或重新安裝所有 Dc 之後，您可以使用**repadmin/replsum**（可在任何版本的 Windows Server 上執行），確認是否已正確復原和複寫 AD DS 和 SYSVOL。  
  
> [!TIP]
> 您也可以下載並執行[Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)（ADReplStatus），這是一個免費的工具，可監視 dc 和報告錯誤的複寫狀態。 ADReplStatus 需要 .NET Framework 4，如果尚未存在，就會加以安裝。  

檢查事件識別碼4602（或檔案複寫服務事件識別碼13516）事件檢視器中的 DFS 複寫記錄，這表示 SYSVOL 已初始化。  

如果第一個復原的 DC 記錄事件識別碼4614（「網域控制站正在等候執行初始複寫。 在 DFS 複寫記錄檔中，複寫的資料夾會維持在初始同步處理狀態，直到其與合作夥伴複寫為止」），然後才會出現事件識別碼4602，而且您必須執行下列手動步驟來復原 SYSVOL （如果它是由DFSR  

1. 當第一個還原的 DC 上出現 DFSR 事件4612時，請執行手動授權還原，如[2218556：如何強制對 DFSR 複寫的 SYSVOL 進行授權和非權威同步處理（如 FRS 的 "D4/D2"）](https://support.microsoft.com/kb/2218556) （ https://support.microsoft.com/kb/2218556) 。  
2. 以手動方式將**SysvolReady 旗**標設定為1，如947022中所述，[當您在新的完整或唯讀 Windows Server 2008 架構的網域控制站上安裝 ACTIVE DIRECTORY DOMAIN SERVICES 之後，NETLOGON 共用不存在](https://support.microsoft.com/kb/947022)。  

您也可以建立 DFS 複寫的診斷報告。 如需詳細資訊，請參閱[建立適用于 DFS 複寫的診斷報告](https://technet.microsoft.com/library/cc754227.aspx)和[Windows Server 2008 的 DFS 逐步指南](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)。 如果伺服器正在執行 Windows Server 2008 R2，您可以使用[Dfsrdiag ReplicationState 命令列參數](https://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  

您也可以使用 dcdiag.exe 執行複寫測試，以檢查複寫錯誤。 如需詳細資訊，請參閱知識庫[文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
