---
title: wbadmin get 版本
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0b7ba0749c8ef347e27590bde4eed7bbcf25af7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362356"
---
# <a name="wbadmin-get-versions"></a>wbadmin get 版本



列出儲存在本機電腦或另一部電腦上的可用備份的詳細資料。 使用此子命令時，如果沒有參數，它會列出本機電腦的所有備份，即使這些備份無法使用也一樣。 針對備份所提供的詳細資料包括備份時間、備份儲存體位置、[ **wbadmin get items** ] 子命令所需的版本識別碼，以及執行復原的類型，以及您可以執行的復原類型。

若要使用此子命令取得可用備份的詳細資料，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元**命令提示**字元，請按一下 [以**系統管理員身分執行**]）。

如需如何使用此子命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|指定包含您想要其詳細資料之備份的儲存位置。 用於列出儲存于該目標位置的備份。 備份目標位置可以是本機連接的磁片磁碟機、磁片區、遠端共用資料夾、卸載式媒體（例如 DVD 光碟機或其他光學媒體）。 如果**wbadmin get 版本**是在建立備份的同一部電腦上執行，則不需要此參數。 不過，若要取得從另一部電腦建立之備份的相關資訊，則需要此參數。|
|-機器|指定您想要備份詳細資料的電腦。 當多部電腦的備份儲存在相同的位置時，請使用。 當指定 **-backupTarget**時，應該使用。|

## <a name="remarks"></a>備註

若要列出可從特定備份復原的專案，請使用**wbadmin get items**。

## <a name="BKMK_examples"></a>典型

若要查看磁片區 h 上儲存的可用備份清單，請輸入：
```
wbadmin get versions -backupTarget:h:
```
若要查看遠端共用資料夾中儲存的可用備份清單 \\ @ no__t-1servername\share 用於電腦 server01，請輸入：
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) Cmdlet