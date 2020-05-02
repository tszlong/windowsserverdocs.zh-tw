---
title: wbadmin restore catalog
description: Wbadmin restore catalog 的參考主題，它會從您指定的儲存位置復原本機電腦的備份類別目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de9ce6b64f996e50fb85a8c612104bc6851ebdfd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720145"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

從您指定的儲存位置復原本機電腦的備份類別目錄。

若要使用這個子命令復原備份類別目錄，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

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
|-機器|指定您想要復原備份類別目錄之電腦的名稱。 當多部電腦的備份儲存在相同的位置時，請使用。 當指定 **-backupTarget**時，應該使用。|
|-quiet|執行子命令，而不提示使用者。|

## <a name="remarks"></a>備註

如果您儲存備份的位置（磁片、DVD 或遠端共用資料夾）損毀或遺失，而且無法用來還原備份類別目錄，請使用**wbadmin delete catalog**來刪除損毀的類別目錄。 在此情況下，您應該在刪除備份類別目錄後建立新的備份。

## <a name="examples"></a>範例

若要從磁片 d：上儲存的備份還原目錄，請輸入：
```
wbadmin restore catalog -backupTarget:d
```
若要從 server01 的共用資料夾\\ \\servername\share 中儲存的備份還原目錄，請輸入：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) Cmdlet