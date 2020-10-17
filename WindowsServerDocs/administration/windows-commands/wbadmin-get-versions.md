---
title: wbadmin get versions
description: Wbadmin get 版本命令的參考文章，其中列出儲存在本機電腦或另一部電腦上之可用備份的詳細資料。
ms.topic: reference
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6825b515f028046702283a2374de26100fd3015f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155925"
---
# <a name="wbadmin-get-versions"></a>wbadmin get versions

列出儲存在本機電腦或另一部電腦上之可用備份的詳細資料。 針對備份所提供的詳細資料包括備份時間、備份儲存位置、版本識別碼，以及您可以執行的復原類型。

若要使用此命令取得可用備份的詳細資料，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

如果使用此命令時沒有參數，它會列出本機電腦的所有備份，即使這些備份無法使用也是一樣。

## <a name="syntax"></a>語法

```
wbadmin get versions [-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}] [-machine:BackupMachineName]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -backupTarget | 指定包含您想要其詳細資料之備份的儲存位置。 用於列出儲存在該目標位置的備份。 備份目標位置可以是本機連接的磁片磁碟機、磁片區、遠端共用資料夾、卸除式媒體，例如 DVD 光碟機或其他光學媒體。 如果此命令在建立備份的相同電腦上執行，則不需要此參數。 不過，若要取得從另一部電腦建立之備份的相關資訊，則需要此參數。 |
| -電腦 | 指定您想要備份詳細資料的電腦。 當多部電腦的備份儲存在相同位置時使用。 當指定 **-backupTarget** 時，應該使用。 |

## <a name="examples"></a>範例

若要查看磁片區 H：上儲存的可用備份清單，請輸入：

```
wbadmin get versions -backupTarget:H:
```

若要查看針對電腦 server01 儲存在遠端共用資料夾中的可用備份清單 `\\<servername>\<share>` ，請輸入：

```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin get items 命令](wbadmin-get-items.md)

- [WBBackupTarget](/powershell/module/windowserverbackup/Get-WBBackupTarget)
