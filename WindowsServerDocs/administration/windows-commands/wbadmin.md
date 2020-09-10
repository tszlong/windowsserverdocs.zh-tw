---
title: wbadmin
description: Wbadmin 的參考文章，可讓您從命令提示字元備份和還原作業系統、磁片區、檔案、資料夾和應用程式。
ms.topic: reference
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8041497fb3ac9b1ae1d564948afc749870a8e12d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640685"
---
# <a name="wbadmin"></a>wbadmin



可讓您從命令提示字元備份和還原您的作業系統、磁片區、檔案、資料夾和應用程式。

若要設定定期排程的備份，您必須是 **Administrators** 群組的成員。 若要使用此命令執行所有其他工作，您必須是 **Backup Operators** 或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。

您必須從提升許可權的命令提示字元執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 [ **命令提示**字元]，然後按一下 [以 **系統管理員身分執行**]。 ) 

## <a name="subcommands"></a>子

|子命令|描述|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|設定並啟用定期排程的備份。|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|停用您的每日備份。|
|[Wbadmin start backup](wbadmin-start-backup.md)|執行一次性備份。 如果使用時不含任何參數，則會使用每日備份排程的設定。|
|[Wbadmin stop job](wbadmin-stop-job.md)|停止目前執行中的備份或復原操作。|
|[Wbadmin get versions](wbadmin-get-versions.md)|列出可從本機電腦復原的備份詳細資料，或從另一部電腦指定其他位置。|
|[Wbadmin get items](wbadmin-get-items.md)|列出備份中包含的專案。|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|執行指定之磁片區、應用程式、檔案或資料夾的復原。|
|[Wbadmin get status](wbadmin-get-status.md)|顯示目前正在執行的備份或復原作業的狀態。|
|[Wbadmin get disks](wbadmin-get-disks.md)|列出目前上線的磁片。|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|執行系統狀態復原。|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|執行系統狀態備份。|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|刪除一或多個系統狀態備份。|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|執行完整系統的復原 (至少包含作業系統狀態) 的所有磁片區。 只有當您使用 Windows 修復環境時，才能使用這個子命令。|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|在本機電腦上的備份類別目錄已損毀的情況下，從指定的儲存位置復原備份類別目錄。|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|刪除本機電腦上的備份類別目錄。 只有當這部電腦上的備份目錄已損毀，而且您沒有將備份儲存在您可用來還原類別目錄的另一個位置時，才使用這個子命令。|

## <a name="additional-references"></a>其他參考資料

-   [備份和復原](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Windows PowerShell 中的 Windows Server Backup Cmdlet](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
