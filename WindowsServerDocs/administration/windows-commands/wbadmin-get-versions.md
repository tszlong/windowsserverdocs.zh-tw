---
title: wbadmin get versions
description: Wbadmin get 版本的參考文章，其中列出本機電腦或另一部電腦上儲存的可用備份的詳細資料。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d122176e37d7c49f9193477e348e44e039e9ac9
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954610"
---
# <a name="wbadmin-get-versions"></a>wbadmin get versions



列出儲存在本機電腦或另一部電腦上的可用備份的詳細資料。 使用此子命令時，如果沒有參數，它會列出本機電腦的所有備份，即使這些備份無法使用也一樣。 針對備份所提供的詳細資料包括備份時間、備份儲存體位置、[ **wbadmin get items** ] 子命令所需的版本識別碼，以及執行復原的類型，以及您可以執行的復原類型。

若要使用此子命令取得可用備份的詳細資料，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元**命令提示**字元，請按一下 [以**系統管理員身分執行**]）。

## <a name="syntax"></a>語法

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|指定包含您想要其詳細資料之備份的儲存位置。 用於列出儲存于該目標位置的備份。 備份目標位置可以是本機連接的磁片磁碟機、磁片區、遠端共用資料夾、卸載式媒體（例如 DVD 光碟機或其他光學媒體）。 如果**wbadmin get 版本**是在建立備份的同一部電腦上執行，則不需要此參數。 不過，若要取得從另一部電腦建立之備份的相關資訊，則需要此參數。|
|-機器|指定您想要備份詳細資料的電腦。 當多部電腦的備份儲存在相同的位置時，請使用。 當指定 **-backupTarget**時，應該使用。|

## <a name="remarks"></a>備註

若要列出可從特定備份復原的專案，請使用**wbadmin get items**。

## <a name="examples"></a>範例

若要查看磁片區 h 上儲存的可用備份清單，請輸入：
```
wbadmin get versions -backupTarget:h:
```
若要查看電腦 server01 的 [遠端共用資料夾] servername\share 中儲存的可用備份清單 \\ \\ ，請輸入：
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBBackupTarget](/powershell/module/windowserverbackup/?view=winserver2012r2-ps) Cmdlet
