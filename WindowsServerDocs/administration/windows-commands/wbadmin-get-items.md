---
title: wbadmin 取得專案
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6c3cc532381321655bbd3d5549b3c9b1896b9280
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362409"
---
# <a name="wbadmin-get-items"></a>wbadmin 取得專案



列出特定備份中包含的專案。

若要使用這個子命令，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）

如需如何使用此子命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|-版本|以 MM/DD/YYYY-HH： MM 格式指定備份的版本。 如果您不知道版本資訊，請輸入**wbadmin get 版本**。|
|-backupTarget|指定包含您想要其詳細資料之備份的儲存位置。 用於列出儲存于該目標位置的備份。 備份目標位置可以是本機連接的磁片磁碟機或遠端共用資料夾。 如果在建立備份的同一部電腦上執行**wbadmin get items**，則不需要此參數。 不過，若要取得從另一部電腦建立之備份的相關資訊，則需要此參數。|
|-機器|指定您想要備份詳細資料的電腦名稱稱。 當多部電腦已備份到相同位置時很有用。 當指定 **-backupTarget**時，應該使用。|

## <a name="BKMK_examples"></a>典型

若要列出在2013年3月31日上午9:00 點執行的備份專案，請輸入：
```
wbadmin get items -version:03/31/2013-09:00
```
列出從2013年4月30日上午9:00 點執行的 server01 備份中的專案 並儲存在 \\\\servername\share 上，輸入：
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) Cmdlet