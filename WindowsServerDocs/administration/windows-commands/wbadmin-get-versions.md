---
title: wbadmin get 版本
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4ebbd0d78de0ffbff1ee8c658d6d9811b87df1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813529"
---
# <a name="wbadmin-get-versions"></a>wbadmin get 版本



列出儲存在本機電腦或另一部電腦的可用備份的詳細資料。 使用此子命令時，不含參數，它會列出所有備份的本機電腦，即使這些備份不提供。 提供備份的詳細資料包含備份的時間，備份儲存體位置的版本識別碼 (所需**wbadmin 取得項目**子命令，並執行復原)，以及您可以執行的復原類型。

若要取得有關使用此子命令的可用備份的詳細資料，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|指定包含您想要的詳細資料的備份儲存位置。 列出儲存於該目標位置的備份中使用。 備份的目標位置可以是本機連接的磁碟機、 磁碟區、 遠端共用的資料夾，例如 DVD 光碟機的卸除式媒體或其他光學媒體。 如果**wbadmin 取得版本**執行的相同電腦上建立備份時，不需要此參數。 不過，這被必要參數以取得從另一部電腦建立備份的相關資訊。|
|-電腦|指定您想要備份的詳細資料的電腦。 當多部電腦的備份會儲存在相同的位置時使用。 應何時 **-backupTarget**指定。|

## <a name="remarks"></a>備註

清單項目可用於從特定的備份復原，使用**wbadmin 取得項目**。

## <a name="BKMK_examples"></a>範例

若要查看可用的備份儲存在磁碟區 h 上的清單，請輸入：
```
wbadmin get versions -backupTarget:h:
```
若要查看可用的備份儲存在遠端共用資料夾中的一份\\ \\servername\share 電腦 server01 類型為：
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) cmdlet