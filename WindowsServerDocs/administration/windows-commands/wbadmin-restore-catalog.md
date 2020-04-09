---
title: wbadmin restore catalog
description: Wbadmin restore catalog 的 Windows 命令主題，它會從您指定的儲存位置復原本機電腦的備份類別目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0ce9182e4e405b1538277db25f06b6a49d7240f9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829701"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog



從您指定的儲存位置復原本機電腦的備份類別目錄。

若要使用這個子命令復原備份類別目錄，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

如需如何使用此子命令的範例，請參閱[範例](#BKMK_examples)。

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

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要從磁片 d：上儲存的備份還原目錄，請輸入：
```
wbadmin restore catalog -backupTarget:d
```
若要從儲存在共用資料夾中的備份還原目錄 \\\\servername\share 的 server01，請輸入：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考資料

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) Cmdlet