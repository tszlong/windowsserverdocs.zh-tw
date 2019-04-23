---
title: Wbadmin get 項目
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842879"
---
# <a name="wbadmin-get-items"></a>Wbadmin get 項目



列出特定的備份中包含的項目。

若要使用此子命令，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-version|指定的備份版本 MM/DD/yyyy-hh: mm 格式。 如果您不知道版本資訊，請輸入**wbadmin 取得版本**。|
|-backupTarget|指定包含您想要的詳細資料的備份儲存位置。 列出儲存於該目標位置的備份中使用。 備份的目標位置可以是本機連接的磁碟機或遠端共用的資料夾。 如果**wbadmin 取得項目**執行的相同電腦上建立備份時，不需要此參數。 不過，這被必要參數以取得從另一部電腦建立備份的相關資訊。|
|-電腦|指定您想要的備份詳細資料的電腦名稱。 相同的位置已備份多部電腦時很有用。 應何時 **-backupTarget**指定。|

## <a name="BKMK_examples"></a>範例

若要從執行於 2013 年 3 月 31 日上午 9:00，類型的備份清單項目：
```
wbadmin get items -version:03/31/2013-09:00
```
從備份的執行於 2013 年 4 月 30 日，上午 9:00 的 server01 的清單項目 並儲存在\\ \\servername\share，型別：
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet