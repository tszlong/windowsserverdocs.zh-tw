---
title: wbadmin start systemstaterecovery
description: Wbadmin start systemstaterecovery 的參考主題，它會執行系統狀態復原至某個位置，以及您指定的備份。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: edbd6acefe2ef921b9325de4808753d5929efd1e
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821378"
---
# <a name="wbadmin-start-systemstaterecovery"></a>wbadmin start systemstaterecovery



執行系統狀態復原至某個位置，以及您指定的備份。

> [!NOTE]
> Windows Server Backup 不會在系統狀態備份或系統狀態復原時，備份或復原登錄使用者 hive （HKEY_CURRENT_USER）。

若要使用這個子命令來執行系統狀態復原，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。



## <a name="syntax"></a>語法

Windows Server 2008 的語法：
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
Windows Server 2008 R2 或更新版本的語法：
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

### <a name="parameters"></a>參數

|參數|說明|
|---------|-----------|
|-version|以 MM/DD/YYYY-HH： MM 格式指定要復原之備份的版本識別碼。 如果您不知道版本識別碼，請輸入**wbadmin get 版本**。|
|-showsummary|報告上一次系統狀態復原的摘要（完成作業所需的重新開機之後）。 此參數不可搭配任何其他參數使用。|
|-backupTarget|指定包含您要復原之備份或備份的儲存位置。 當儲存位置與通常儲存此電腦的備份位置不同時，這個參數很有用。|
|-機器|指定您要復原之電腦的名稱。 當多部電腦已備份到相同的位置時，這個參數很有用。 當指定 **-backupTarget**參數時，應該使用。|
|-recoveryTarget|指定還原目的地的目錄。 如果備份還原至替代位置，此參數會很有用。|
|-authsysvol|如果使用，會執行 SYSVOL 的授權還原（系統磁碟區共用目錄）。|
|-autoReboot|指定在系統狀態復原操作結束時重新開機系統。 此參數僅適用于復原到原始位置。 如果您需要在復原操作之後執行步驟，則不建議使用此參數。|
|-quiet|執行子命令，而不提示使用者。|

## <a name="examples"></a>範例

- 若要在上午9:00 從03/31/2013 執行備份的系統狀態復原，請輸入：
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```
- 若要在上午9:00 從04/30/2013 執行備份的系統狀態復原 這會儲存在 server01 的共用資源 \\ \\ servername\share 上，請輸入：
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) Cmdlet