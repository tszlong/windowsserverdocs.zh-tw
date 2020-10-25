---
title: wbadmin start systemstaterecovery
description: Wbadmin start systemstaterecovery 命令的參考文章，此命令會執行系統狀態復原至位置，以及從您指定的備份執行系統狀態復原。
ms.topic: reference
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b52b4fedf14eb2d4caab4e9d521767ff9e89365a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524713"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery

執行系統狀態復原至位置以及您指定的備份。

若要使用此命令執行系統狀態復原，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

> [!NOTE]
> Windows Server Backup 不會在系統狀態備份或系統狀態復原時，備份或復原登錄使用者 hive (HKEY_CURRENT_USER) 。

## <a name="syntax"></a>語法

```
wbadmin start systemstaterecovery -version:<VersionIdentifier> -showsummary [-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>] [-recoveryTarget:<TargetPathForRecovery>] [-authsysvol] [-autoReboot] [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -version | 以 MM/DD/YYYY-HH： MM 格式指定要復原的備份版本識別碼。 如果您不知道版本識別碼，請執行 [ [wbadmin get 版本] 命令](wbadmin-get-versions.md)。 |
| -showsummary | 回報完成作業) 所需的重新開機後，上次系統狀態復原 (的摘要。 此參數不能與任何其他參數一起使用。 |
| -backupTarget | 指定您想要復原的備份 () 的儲存位置。 當儲存位置與通常儲存備份的位置不同時，此參數很有用。 |
| -電腦 | 指定要復原備份的電腦名稱稱。 當指定 **-backupTarget** 參數時，必須使用這個參數。 當多部電腦備份至相同的位置時， **-machine** 參數會很有用。 |
| -recoveryTarget | 指定要還原的目標目錄。 如果備份還原到替代位置，這個參數就很有用。 |
| -authsysvol | 執行系統磁碟區 (sysvol) 共用目錄的授權還原。 |
| -autoReboot | 指定在系統狀態復原操作結束時重新開機系統。 此參數僅適用于復原到原始位置。 如果您需要在復原操作之後執行步驟，我們不建議使用此參數。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要從03/31/2020 年上午9:00 開始備份的系統狀態復原，請輸入：

```
wbadmin start systemstaterecovery -version:03/31/2020-09:00
```

從04/30/2020 上午9:00 開始備份的系統狀態復原 這會儲存在 server01 的共用資源上 `\\servername\share` ，請輸入：

```
wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [開始-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)
