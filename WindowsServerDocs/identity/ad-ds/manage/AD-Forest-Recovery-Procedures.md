---
title: "廣告樹系修復程序"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adfs
ms.openlocfilehash: 91e3954c05fe3cd12d35b5db91afd29fb3a31e00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/13/2017
---
# <a name="ad-forest-recovery---procedures"></a>廣告樹系修復程序


>適用於： Windows Server 2016、 Windows Server 2012 和 2012 R2、 Windows Server 2008 和 2008 R2

這一節包含與相關的樹系的修復程序的程序。 程序適用於 Windows Server 2016 的 2012 R2、2012 年及也適用於 Windows Server 2008 R2 和某些次要例外 2008 年。 

程序，包括 Windows Server 2003 不同步驟位於[和 Windows Server 2003 網域控制站的樹系修復](AD-Forest-Recovery-Windows-Server-2003.md)。  

以下是一份備份及還原網域控制站和 Active Directory 中所使用的程序。
  
-   [完整的伺服器備份](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
-   [備份資料](AD-Forest-Recovery-Backing-up-System-State.md)  
-   [執行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
-   [執行 DFSR 複寫 SYSVOL 授權的同步](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
-   [執行 Active Directory Domain Services 未授權還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)  
  
     這些步驟如何執行一次 SYSVOL 授權還原。  
-   [設定 DNS 伺服器服務](AD-Forest-Recovery-Configure-DNS.md)  
-   [移除通用](AD-Forest-Recovery-Remove-GC.md)  
-   [提高提供 RID 集區的值](AD-Forest-Recovery-Raise-RID-Pool.md)  
-   [停用目前 RID 集區](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
-   [抓取作業主角](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
-   [完成後還原進行清理](AD-Forest-Recovery-Cleanup.md)
-   [移除寫入網域控制站清潔中繼資料](AD-Forest-Recovery-Cleaning-Metadata.md)  
-   [重設電腦的網域控制站 account 密碼](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
-   [重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
-   [將其中一邊的信任信任密碼重設](AD-Forest-Recovery-Reset-Trust.md)  
-   [新增通用](AD-Forest-Recovery-Add-GC.md)  
-   [資源驗證複寫運作](AD-Forest-Recovery-Verify-Replication.md)  
  
  
## <a name="next-steps"></a>後續步驟
-   [廣告樹系修復必要條件](AD-Forest-Recovery-Prerequisties.md)  
-   [廣告樹系修復-設計自訂樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [廣告樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
-   [廣告樹系修復-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [廣告樹系修復-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [廣告樹系修復程序](AD-Forest-Recovery-Procedures.md)  
-   [廣告樹系修復-常見問題集](AD-Forest-Recovery-FAQ.md)  
-   [廣告樹系修復-復原 Multidomain 樹系單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [廣告樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
