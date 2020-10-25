---
title: wbadmin start sysrecovery
description: Wbadmin start sysrecovery 命令的參考文章，此命令會使用您指定的參數執行系統復原 (裸機復原) 。
ms.topic: reference
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 292c0d6fc42f4fe58dda6a6fbbb6a693b70a8756
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524743"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery

使用指定的參數，執行系統復原 (裸機復原) 。

若要使用此命令執行系統復原，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。

> [!IMPORTANT]
> **Wbadmin start sysrecovery**命令必須從 Windows 修復主控台執行，而且不會列在**wbadmin**工具的預設使用方式文字中。 如需詳細資訊，請參閱 [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)。

## <a name="syntax"></a>語法

```
wbadmin start sysrecovery -version:<VersionIdentifier> -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}  [-machine:<BackupMachineName>] [-restoreAllVolumes] [-recreateDisks] [-excludeDisks] [-skipBadClusterCheck] [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -version | 以 MM/DD/YYYY-HH： MM 格式指定要復原的備份版本識別碼。 如果您不知道版本識別碼，請執行 [ [wbadmin get 版本] 命令](wbadmin-get-versions.md)。 |
| -backupTarget | 指定包含您想要復原之備份 () 的儲存位置。 當儲存位置與此電腦的備份通常儲存位置不同時，此參數很有用。 |
| -電腦 | 指定您想要復原備份的電腦名稱稱。 當指定 **-backupTarget** 參數時，必須使用這個參數。 當多部電腦備份至相同的位置時， **-machine** 參數會很有用。 |
| -restoreAllVolumes | 復原所選備份中的所有磁片區。 如果未指定此參數，則只會復原包含系統狀態和作業系統元件) 的重要磁片區 (磁片區。 當您需要在系統復原期間復原非重要磁片區時，此參數很有用。 |
| -recreateDisks | 將磁片設定復原到建立備份時存在的狀態。<p>**警告：** 此參數會刪除裝載作業系統元件之磁片區上的所有資料。 它也可能會刪除資料磁片區中的資料。 |
| -excludeDisks | 只有在使用 **-recreateDisks** 參數指定時才有效，而且必須以逗號分隔的磁片識別碼清單來輸入， (如 [wbadmin get disk 命令](wbadmin-get-disks.md)) 的輸出中所列。 排除的磁片未分割或格式化。 此參數有助於保留您在復原作業期間不想修改之磁片上的資料。 |
| -skipBadClusterCheck | 只有在復原磁片區時才有效。 略過檢查您正在復原的磁片是否有錯誤的叢集資訊。 如果您要復原到替代伺服器或硬體，建議您不要使用此參數。 您可以隨時在這些磁片上手動執行命令 **chkdsk/b** 來檢查是否有錯誤的叢集，然後據此更新檔案系統資訊。<p>**重要事項：** 在您執行 **chkdsk/b**之前，復原系統上回報的錯誤叢集可能不正確。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要在2020年3月31日上午9:00 開始復原執行的備份資訊，請在磁片磁碟機 d：上輸入：

```
wbadmin start sysrecovery -version:03/31/2020-09:00 -backupTarget:d:
```

若要開始從2020年4月30日上午9:00 執行的備份復原資訊，請在 server01 的共用資料夾中 `\\servername\share` ，輸入：

```
wbadmin start sysrecovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [WBBareMetalRecovery](/powershell/module/windowserverbackup/Get-WBBareMetalRecovery)
