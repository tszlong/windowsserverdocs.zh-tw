---
title: wbadmin start systemstaterecovery
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c99a934987e320baaec0e56c69f36eda5a32819
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852679"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery



執行系統狀態復原的位置，並從您指定的備份。

> [!NOTE]
> Windows Server Backup 不會備份或復原系統狀態備份或系統狀態復原的一部分的登錄使用者 hive 控制檔 (HKEY_CURRENT_USER)。

若要執行此子命令的系統狀態復原，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

適用於 Windows Server 2008 的語法：
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
適用於 Windows Server 2008 R2 或更新版本的語法：
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-version|指定要復原 MM/DD/yyyy 的備份的版本識別碼-hh: mm 格式。 如果您不知道的版本識別碼，鍵入**wbadmin 取得版本**。|
|-showsummary|報告最後一個系統狀態復原的摘要 （之後重新啟動，才能完成此作業）。 這個參數無法搭配任何其他參數。|
|-backupTarget|指定包含您想要復原的備份儲存位置。 不同於此電腦的備份通常的儲存位置的儲存位置時，這個參數是很有用。|
|-電腦|指定您想要復原的電腦名稱。 相同的位置已備份多部電腦時，這個參數是很有用。 應何時 **-backupTarget**指定參數。|
|-recoveryTarget|指定要還原至目錄。 這個參數是很有用，如果在備份還原到替代位置。|
|-authsysvol|如果使用，執行系統授權還原的 SYSVOL （系統磁碟區共用目錄）。|
|-autoReboot|指定要重新啟動系統的系統狀態復原作業結尾處。 這個參數是僅適用於復原到原始位置。 我們不建議您使用這個參數，如果您需要執行復原作業之後的步驟。|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="BKMK_examples"></a>範例

-   若要執行的備份系統狀態復原，從 03/31/2013年上午 9:00，輸入：  
    ```
    wbadmin start systemstaterecovery -version:03/31/2013-09:00
    ```  
-   若要執行系統狀態復原的備份從 2013 年 04 月 30 日，上午 9:00 儲存在共用資源上\\ \\servername\share for server01 中，類型：  
    ```
    wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
    ```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet