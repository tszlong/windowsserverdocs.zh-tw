---
title: wbadmin restore catalog
description: Wbadmin restore catalog 的參考文章，它會從您指定的儲存位置復原本機電腦的備份類別目錄。
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c551aa77ff98a66d3faacc3901806be8cc67b8f2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637603"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

從您指定的儲存位置復原本機電腦的備份類別目錄。

若要使用此子命令復原備份類別目錄，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 **命令提示**字元，然後按一下 [以 **系統管理員身分執行**]。 ) 

## <a name="syntax"></a>語法

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|指定在建立備份之後，系統備份類別目錄的位置。|
|-電腦|指定您想要復原備份類別目錄的電腦名稱稱。 當多部電腦的備份儲存在相同位置時使用。 當指定 **-backupTarget** 時，應該使用。|
|-quiet|執行子命令，而不提示使用者。|

## <a name="remarks"></a>備註

如果您儲存備份的 (磁片、DVD 或遠端共用資料夾) 位置已損毀或遺失，且無法用來還原備份類別目錄，請使用 **wbadmin delete catalog** 來刪除損毀的目錄。 在此情況下，您應該在刪除備份類別目錄之後建立新的備份。

## <a name="examples"></a>範例

若要從磁片 d：上儲存的備份還原目錄，請輸入：
```
wbadmin restore catalog -backupTarget:d
```
若要從 server01 的共用資料夾 servername\share 中儲存的備份還原類別目錄 \\ \\ ，請輸入：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)
-   [Restore-WBCatalog](/powershell/module/windowserverbackup/?view=winserver2012r2-ps) Cmdlet
