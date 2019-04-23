---
title: AD 樹系復原 - 程序
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827719"
---
# <a name="ad-forest-recovery---procedures"></a>AD 樹系復原 - 程序

>適用於：Windows Server 2016 中，Windows Server 2012 和 2012 R2 中，Windows Server 2008 和 2008 R2

本章節包含樹系修復程序的相關程序。 程序是適用於 Windows Server 2016、 2012 R2，2012年，另外還有適用於 Windows Server 2008 R2 和 2008，但有一些次要的例外狀況。

Windows Server 2003 而有所不同，其中包含的程序中找到[與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)。  

以下是備份和還原網域控制站和 Active Directory 中使用的程序的清單。

- [備份完整伺服器](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [備份系統狀態資料](AD-Forest-Recovery-Backing-up-System-State.md)  
- [執行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [執行 DFSR 複寫 SYSVOL 的權威的同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [執行非系統授權還原的 Active Directory 網域服務](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

下列步驟說明如何執行 SYSVOL 的授權還原，在相同的時間。  

- [設定 DNS 伺服器服務](AD-Forest-Recovery-Configure-DNS.md)  
- [移除通用類別目錄](AD-Forest-Recovery-Remove-GC.md)  
- [引發可用 RID 集區的值](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [讓目前的 RID 集區](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [拿取操作主機角色](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [還原後進行清除](AD-Forest-Recovery-Cleanup.md)
- [清除已移除的可寫入網域控制站的中繼資料](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [重設的網域控制站的電腦帳戶密碼](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [重設一方的信任的信任密碼](AD-Forest-Recovery-Reset-Trust.md)  
- [新增通用類別目錄](AD-Forest-Recovery-Add-GC.md)  
- [若要確認複寫運作正常的資源](AD-Forest-Recovery-Verify-Replication.md)  

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原-必要條件](AD-Forest-Recovery-Prerequisties.md)  
- [AD 樹系復原-設計自訂的樹系復原計劃](AD-Forest-Recovery-Devising-a-Plan.md)  
- [AD 樹系復原-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-判斷如何復原](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始的復原](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [AD 樹系修復程序](AD-Forest-Recovery-Procedures.md)  
- [AD 樹系復原-常見問題集](AD-Forest-Recovery-FAQ.md)  
- [AD 樹系復原-復原 Multidomain 樹系內的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [AD 樹系復原與 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md) 
