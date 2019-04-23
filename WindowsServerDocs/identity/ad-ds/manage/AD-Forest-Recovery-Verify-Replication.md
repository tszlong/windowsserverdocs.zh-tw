---
title: AD 樹系復原-驗證複寫
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.technology: identity-adds
ms.openlocfilehash: fb05586f281460dc2c7a1afea4c0423493e3fc46
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878309"
---
# <a name="resources-to-verify-replication-is-working"></a>若要確認複寫運作正常的資源 

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

在還原或重新安裝所有的網域控制站之後，您可以確認 AD DS 與 SYSVOL 是復原與複寫正確地使用**repadmin /replsum**，以在任何版本的 Windows Server 上執行。  
  
> [!TIP]
> 您也可以下載並執行[Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005)(ADReplStatus)，免費的工具，監視網域控制站和報告錯誤的複寫狀態。 ADReplStatus 需要.NET Framework 4，將會安裝，如果它尚未存在。  

請檢查事件檢視器的事件識別碼 4602 （或檔案複寫服務事件識別碼 13516），表示已初始化 SYSVOL 的 DFS 複寫記錄檔。  

如果第一個復原 DC 會記錄事件識別碼 4614 （「 網域控制站正在等候執行初始複寫。 複寫的資料夾會保留在首次同步處理狀態直到與其協力電腦複寫"） 中的 DFS 複寫記錄檔，則不會不出現事件識別碼 4602 和您需要執行下列手動步驟復原所複寫的 SYSVOLDFSR:  

1. 當 DFSR 事件 4612 出現在第一個還原的網域控制站上執行手動的系統授權還原中所述[2218556:如何針對 DFSR 複製的 SYSVOL （例如"D4/D2"frs) 強制執行權威和非權威同步處理](https://support.microsoft.com/kb/2218556)(https://support.microsoft.com/kb/2218556)。  
2. 設定**SysvolReady 旗標**為 1 以手動方式，如中所述[947022 的 NETLOGON 共用不存在之後您新的完整或唯讀模式的 Windows Server 2008 網域控制站上安裝 Active Directory 網域服務,](https://support.microsoft.com/kb/947022).  

您也可以建立 DFS 複寫診斷報告。 如需詳細資訊，請參閱 <<c0> [ 建立 DFS 複寫診斷報告](https://technet.microsoft.com/library/cc754227.aspx)並[DFS 逐步指南適用於 Windows Server 2008](https://technet.microsoft.com/library/cc732863\(WS.10\).aspx)。 如果伺服器執行 Windows Server 2008 R2，您可以使用[dfsrdiag.exe ReplicationState 命令列參數](http://blogs.technet.com/b/filecab/archive/2009/05/28/dfsrdiag-exe-replicationstate-what-s-dfsr-up-to.aspx)。  

您也可以執行使用 dcdiag.exe 檢查複寫錯誤的複寫測試。 如需詳細資訊，請參閱知識庫[文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)
