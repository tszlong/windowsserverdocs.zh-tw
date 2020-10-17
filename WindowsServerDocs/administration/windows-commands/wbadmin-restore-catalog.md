---
title: wbadmin restore catalog
description: Wbadmin restore catalog 命令的參考文章，它會從您指定的儲存位置復原本機電腦的備份類別目錄。
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 046c4b4162c9512d5893c3c06e83e7260386c990
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155861"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

從您指定的儲存位置復原本機電腦的備份類別目錄。

若要使用此命令復原特定備份中包含的備份類別目錄，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

> [!NOTE]
> 如果您儲存備份的 (磁片、DVD 或遠端共用資料夾) 位置已損毀或遺失，且無法用來還原備份類別目錄，請執行 [ [wbadmin delete catalog](wbadmin-delete-catalog.md) ] 命令以刪除損毀的目錄。 在此情況下，我們建議您在刪除備份類別目錄之後建立新的備份。

## <a name="syntax"></a>語法

```
wbadmin restore catalog -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>} [-machine:<BackupMachineName>] [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -backupTarget | 指定在建立備份之後，系統備份類別目錄的位置。 |
| -電腦 | 指定您想要復原備份類別目錄的電腦名稱稱。 當多部電腦的備份儲存在相同位置時使用。 當指定 **-backupTarget** 時，應該使用。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要從磁片 D：上儲存的備份還原目錄，請輸入：

```
wbadmin restore catalog -backupTarget:D
```

若要從儲存在 server01 共用資料夾中的備份還原類別目錄 `\\<servername>\<share>` ，請輸入：

```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin delete catalog 命令](wbadmin-delete-catalog.md)

- [還原-WBCatalog](/powershell/module/windowserverbackup/Restore-WBCatalog)
