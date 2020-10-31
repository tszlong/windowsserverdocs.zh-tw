---
title: AD 樹系復原 - 程序
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.openlocfilehash: 3a4edd21cb538c2aa8fd1f2d1eb4f3832c3fac56
ms.sourcegitcommit: b115e5edc545571b6ff4f42082cc3ed965815ea4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93067790"
---
# <a name="ad-forest-recovery---procedures"></a>AD 樹系復原 - 程序

>適用于： Windows Server 2016、Windows Server 2012 和 2012 R2、Windows Server 2008 和 2008 R2

本章節包含與樹系修復程式相關的程式。 這些程式適用于 Windows Server 2016、2012 R2、2012，而且也適用于 Windows Server 2008 R2 和2008（有一些次要例外狀況）。

包含 windows Server 2003 各項步驟的程式，可在 [具有 Windows server 2003 網域控制站的樹](AD-Forest-Recovery-Windows-Server-2003.md)系復原中找到。

以下是用來備份和還原網域控制站和 Active Directory 的程式清單。

- [備份完整伺服器](AD-Forest-Recovery-Backing-up-a-Full-Server.md)
- [備份系統狀態資料](AD-Forest-Recovery-Backing-up-System-State.md)
- [執行完整伺服器復原](AD-Forest-Recovery-Perform-a-Full-Recovery.md)
- [執行 DFSR 複寫 SYSVOL 的授權同步處理](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [執行 Active Directory Domain Services 的非系統授權還原](AD-Forest-Recovery-Nonauthoritative-Restore.md)

這些步驟說明如何同時執行 SYSVOL 的授權還原。

- [設定 DNS 伺服器服務](AD-Forest-Recovery-Configure-DNS.md)
- [移除通用類別目錄](AD-Forest-Recovery-Remove-GC.md)
- [提高可用 RID 集區的值](AD-Forest-Recovery-Raise-RID-Pool.md)
- [使目前的 RID 集區失效](AD-Forest-Recovery-Invaildate-RID-Pool.md)
- [佔用操作主機角色](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)
- [在還原之後清除](AD-Forest-Recovery-Cleanup.md)
- [清除已移除可寫入網域控制站的中繼資料](AD-Forest-Recovery-Cleaning-Metadata.md)
- [重設網域控制站的電腦帳戶密碼](AD-Forest-Recovery-Reset-Computer-Account-DC.md)
- [重設 krbtgt 密碼](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)
- [在信任的一端重設信任密碼](AD-Forest-Recovery-Reset-Trust.md)
- [新增通用類別目錄](AD-Forest-Recovery-Add-GC.md)
- [確認複寫運作的資源](AD-Forest-Recovery-Verify-Replication.md)

## <a name="next-steps"></a>後續步驟

- [AD 樹系復原 - 先決條件](AD-Forest-Recovery-Prerequisties.md)
- [AD 樹系復原-設計自訂樹系復原方案](AD-Forest-Recovery-Devising-a-Plan.md)
- [AD 樹系修復-找出問題](AD-Forest-Recovery-Identify-the-Problem.md)
- [AD 樹系復原-決定復原方式](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [AD 樹系復原-執行初始復原](AD-Forest-Recovery-Perform-initial-recovery.md)
- [AD 樹系復原 - 程序](AD-Forest-Recovery-Procedures.md)
- [AD 樹系復原-常見問題](AD-Forest-Recovery-FAQ.md)
- [AD 樹系復原-復原多網域樹系中的單一網域](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [AD 樹系復原-含 Windows Server 2003 網域控制站的樹系復原](AD-Forest-Recovery-Windows-Server-2003.md)
