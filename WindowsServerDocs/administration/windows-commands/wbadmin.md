---
title: wbadmin
description: Wbadmin 命令的參考文章，可讓您從命令提示字元備份和還原作業系統、磁片區、檔案、資料夾和應用程式。
ms.topic: reference
ms.assetid: 4b0b3f32-d21f-4861-84bb-b2eadbf1e7b8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: df96e85d7cb4999088efe9bd6ede70087cd24cb7
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524663"
---
# <a name="wbadmin"></a>wbadmin

可讓您從命令提示字元備份和還原您的作業系統、磁片區、檔案、資料夾和應用程式。

若要使用此命令設定定期排程的備份，您必須是 **Administrators** 群組的成員。 若要使用此命令執行所有其他工作，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。

您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| [wbadmin delete catalog](wbadmin-delete-catalog.md) | 刪除本機電腦上的備份類別目錄。 只有當這部電腦上的備份目錄已損毀，而且您沒有將備份儲存在您可用來還原目錄的另一個位置時，才能使用此命令。 |
| [wbadmin delete systemstatebackup](wbadmin-delete-systemstatebackup.md) | 刪除一或多個系統狀態備份。 |
| [wbadmin disable backup](wbadmin-disable-backup.md) | 停用您的每日備份。 |
| [wbadmin enable backup](wbadmin-enable-backup.md) | 設定並啟用定期排程的備份。 |
| [wbadmin get disks](wbadmin-get-disks.md) | 列出目前上線的磁片。 |
| [wbadmin get items](wbadmin-get-items.md) | 列出備份中包含的專案。 |
| [wbadmin get status](wbadmin-get-status.md) | 顯示目前正在執行的備份或復原作業的狀態。 |
| [wbadmin get versions](wbadmin-get-versions.md) | 列出可從本機電腦復原的備份詳細資料，或從另一部電腦指定其他位置。 |
| [wbadmin restore catalog](wbadmin-restore-catalog.md) | 在本機電腦上的備份類別目錄已損毀的情況下，從指定的儲存位置復原備份類別目錄。 |
| [wbadmin start backup](wbadmin-start-backup.md) | 執行一次性備份。 如果使用時不含任何參數，則會使用每日備份排程的設定。 |
| [wbadmin start recovery](wbadmin-start-recovery.md) | 執行指定之磁片區、應用程式、檔案或資料夾的復原。 |
| [wbadmin start sysrecovery](wbadmin-start-sysrecovery.md) | 執行完整系統的復原 (至少包含作業系統狀態) 的所有磁片區。 只有當您使用 Windows 修復環境時，才能使用此命令。 |
| [wbadmin start systemstatebackup](wbadmin-start-systemstatebackup.md) | 執行系統狀態備份。 |
| [wbadmin start systemstaterecovery](wbadmin-start-systemstaterecovery.md) | 執行系統狀態復原。 |
| [wbadmin stop job](wbadmin-stop-job.md) | 停止目前執行中的備份或復原操作。 |

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows PowerShell 中的 Windows Server Backup Cmdlet](/powershell/module/windowsserverbackup)

- [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
