---
title: wbadmin
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2ce8109ef6a0885abd02ef1dee9f11d21b7d7e9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858879"
---
# <a name="wbadmin"></a>wbadmin



可讓您備份及還原您的作業系統、 磁碟區、 檔案、 資料夾和應用程式，從命令提示字元。

若要設定定期排定的備份，您必須隸屬**系統管理員**群組。 若要執行此命令使用的所有其他工作，您必須隸屬**Backup Operators**或**系統管理員**群組，或者您必須已經被委派適當的權限。

您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。)

## <a name="subcommands"></a>子命令

|子命令|描述|
|----------|-----------|
|[Wbadmin 啟用備份](wbadmin-enable-backup.md)|設定，並可讓您有排定來定期備份。|
|[Wbadmin 停用備份](wbadmin-disable-backup.md)|停用您的每日備份。|
|[Wbadmin start backup](wbadmin-start-backup.md)|執行一次性的備份。 如果使用不含任何參數，會使用從每日備份排程設定。|
|[Wbadmin 停止作業](wbadmin-stop-job.md)|停止目前執行的備份或復原作業。|
|[wbadmin get 版本](wbadmin-get-versions.md)|列出可從本機電腦復原備份的詳細資料，或如果指定另一個位置，從另一部電腦。|
|[Wbadmin get 項目](wbadmin-get-items.md)|列出備份中包含的項目。|
|[wbadmin 開始復原](wbadmin-start-recovery.md)|執行復原的磁碟區、 應用程式、 檔案或指定的資料夾。|
|[Wbadmin get 狀態](wbadmin-get-status.md)|顯示目前執行的備份或復原作業的狀態。|
|[Wbadmin get 磁碟](wbadmin-get-disks.md)|列出目前上線的磁碟。|
|[wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md)|執行系統狀態復原。|
|[Wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md)|執行系統狀態備份。|
|[Wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md)|刪除一或多個系統狀態備份。|
|[Wbadmin start sysrecovery](wbadmin-start-sysrecovery.md)|執行完整的系統 （最少的所有磁碟區含有作業系統的狀態） 的復原。 此子命令才可使用，如果您使用 Windows 修復環境。|
|[Wbadmin 還原類別目錄](wbadmin-restore-catalog.md)|從指定的儲存體位置，在本機電腦上備份的目錄發生損毀的情況下復原備份類別目錄。|
|[Wbadmin delete 類別目錄](wbadmin-delete-catalog.md)|刪除本機電腦上的備份目錄。 只有當這部電腦上的備份資料目錄已損毀，而且沒有備份儲存在另一個位置可供您還原類別目錄，請使用此子命令。|

#### <a name="additional-references"></a>其他參考資料

-   [備份和復原](https://go.microsoft.com/fwlink/?LinkID=195054)
-   [在 Windows PowerShell 中 Windows Server Backup Cmdlet](https://technet.microsoft.com/library/jj902428.aspx)