---
title: AD 樹系復原-確認複寫
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 302e522a-fb40-43bc-bc63-83dcc87ebde5
ms.openlocfilehash: 6f08871e3ffa27f3bfc063c5962437ae65f8f667
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93070780"
---
# <a name="resources-to-verify-replication-is-working"></a>確認複寫運作的資源

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

在還原或重新安裝所有的 Dc 之後，您可以使用 **repadmin/replsum** （可在任何版本的 Windows Server 上執行）來確認 AD DS 和 SYSVOL 已正確地復原和複寫。

> [!TIP]
> 您也可以下載並執行 [Active Directory 複寫狀態工具](https://www.microsoft.com/download/details.aspx?id=30005) (ADReplStatus) ，這是一項免費的工具，可監視 dc 的複寫狀態並報告錯誤。 ADReplStatus 需要 .NET Framework 4，如果不存在，則會加以安裝。

檢查事件識別碼 4602 (或檔案複寫服務事件識別碼 13516) （指出 SYSVOL 已初始化）的 DFS 複寫登入事件檢視器。

如果第一個復原的 DC 記錄事件識別碼 4614 (」網域控制站正在等候執行初始複寫。 複寫的資料夾將保持在初始同步處理狀態，直到它已與 DFS 複寫記錄檔中的「) 」進行複寫為止，然後不會出現事件識別碼4602，而且您必須執行下列手動步驟來復原 SYSVOL （如果 DFSR 複寫）：

1. 當第一次還原的 DC 上出現 DFSR 事件4612時，請執行手動授權還原，如2218556中所述 [：如何強制執行 DFSR 複寫 SYSVOL 的授權和非權威同步處理 (例如「D4/D2」（適用于 FRS) ](https://support.microsoft.com/kb/2218556) (） https://support.microsoft.com/kb/2218556) 。
2. 以手動方式將 **SysvolReady 旗** 標設定為1，如947022中所述， [當您在新的完整或唯讀 Windows Server 2008 型網域控制站上安裝 Active Directory Domain Services 之後，就不會出現 NETLOGON 共用](https://support.microsoft.com/kb/947022)。

您也可以 DFS 複寫建立診斷報告。 如需詳細資訊，請參閱建立適用于[Windows Server 2008 之 DFS 複寫和 DFS 逐步指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11))的[診斷報告](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11))。 如果伺服器正在執行 Windows Server 2008 R2，您可以使用 [dfsrdiag.exe ReplicationState 命令列參數](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754227(v=ws.11))。

您也可以使用 dcdiag.exe 來執行複製測試，以檢查複寫錯誤。 如需詳細資訊，請參閱知識庫 [文章 249256](https://support.microsoft.com/kb/249256)。

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原指南](AD-Forest-Recovery-Guide.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
