---
title: Wbadmin 還原類別目錄
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851709"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin 還原類別目錄



從您指定的儲存體位置中復原備份類別目錄的本機電腦。

若要復原備份類別目錄與此子命令，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|因為它之處建立備份後，請指定系統的備份類別目錄的位置。|
|-電腦|指定您想要復原的備份目錄之電腦的名稱。 當有多部電腦的備份已儲存在相同位置時使用。 應何時 **-backupTarget**指定。|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="remarks"></a>備註

如果您用來儲存備份的位置 （磁碟、 DVD 或遠端共用的資料夾） 已損毀或遺失，不能用來還原備份類別目錄，請使用**wbadmin 刪除目錄**刪除損毀的目錄。 在此情況下，您應該建立新的備份之後會刪除您的備份目錄, 中。

## <a name="BKMK_examples"></a>範例

若要從儲存在磁碟 d： 的備份中還原目錄，請輸入：
```
wbadmin restore catalog -backupTarget:d
```
若要從共用資料夾中儲存的備份還原類別目錄\\ \\servername\share 的 server01，型別：
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [還原 WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet