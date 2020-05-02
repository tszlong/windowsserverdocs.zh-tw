---
title: wbadmin
description: Wbadmin 的參考主題，可讓您從命令提示字元備份和還原作業系統、磁片區、檔案、資料夾和應用程式。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94f07d17d46dad4e5301ba3ea6be94b10f26a3af
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725829"
---
# <a name="wbadmin"></a>wbadmin



可讓您從命令提示字元備份和還原作業系統、磁片區、檔案、資料夾和應用程式。

若要設定定期排定的備份，您必須是**Administrators**群組的成員。 若要使用此命令執行所有其他工作，您必須是**Backup Operators**或**Administrators**群組的成員，或者必須已被委派適當的許可權。

您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下 [**命令提示**字元]，然後按一下 [以**系統管理員身分執行**]。

## <a name="subcommands"></a>子

|子命令|描述|
|----------|-----------|
|[Wbadmin enable backup](wbadmin-enable-backup.md)|設定並啟用定期排程的備份。|
|[Wbadmin disable backup](wbadmin-disable-backup.md)|停用您的每日備份。|
|[Wbadmin start backup](wbadmin-start-backup.md)|執行一次性備份。 如果與沒有參數搭配使用，則會使用每日備份排程的設定。|
|[Wbadmin stop job](wbadmin-stop-job.md)|停止目前正在執行的備份或復原作業。|
|[Wbadmin get versions](wbadmin-get-versions.md)|列出可從本機電腦復原的備份詳細資料，或從另一部電腦指定另一個位置。|
|[Wbadmin get items](wbadmin-get-items.md)|列出備份中所包含的專案。|
|[Wbadmin start recovery](wbadmin-start-recovery.md)|執行指定的磁片區、應用程式、檔案或資料夾的復原。|
|[Wbadmin get status](wbadmin-get-status.md)|顯示目前正在執行之備份或復原作業的狀態。|
|[Wbadmin get disks](wbadmin-get-disks.md)|列出目前上線的磁片。|
|[Wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|執行系統狀態復原。|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|執行系統狀態備份。|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|刪除一或多個系統狀態備份。|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|執行完整系統的復原（至少包含作業系統狀態的所有磁片區）。 只有當您使用 Windows 修復環境時，才能使用此子命令。|
|[Wbadmin restore catalog](wbadmin-restore-catalog.md)|當本機電腦上的備份類別目錄已損毀時，從指定的儲存位置復原備份類別目錄。|
|[Wbadmin delete catalog](wbadmin-delete-catalog.md)|刪除本機電腦上的備份類別目錄。 只有當這部電腦上的備份類別目錄已損毀，而且您沒有將備份儲存在另一個可用來還原類別目錄的位置時，才使用此子命令。|

## <a name="additional-references"></a>其他參考

-   [備份和復原](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [Windows PowerShell 中的 Windows Server Backup Cmdlet](https://technet.microsoft.com/library/jj902428.aspx)