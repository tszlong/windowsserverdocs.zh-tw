---
title: wbadmin delete systemstatebackup
description: Wbadmin delete systemstatebackup 命令的參考文章，此命令會刪除您指定的系統狀態備份。
ms.topic: reference
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05ce1e2deb8a2466df70b56a43c4532871d491b9
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156075"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup

刪除您指定的系統狀態備份。 如果指定的磁片區包含本機伺服器的系統狀態備份以外的備份，將不會刪除這些備份。

若要使用此命令刪除系統狀態備份，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

> [!NOTE]
> Windows Server Backup 不會在系統狀態備份或系統狀態復原時，備份或復原登錄使用者 hive (HKEY_CURRENT_USER) 。

## <a name="syntax"></a>Syntax

```
wbadmin delete systemstatebackup {-keepVersions:<numberofcopies> | -version:<versionidentifier> | -deleteoldest} [-backupTarget:<volumename>] [-machine:<backupmachinename>] [-quiet]
```

> [!IMPORTANT]
> 您必須指定下列其中一個參數： **-keepVersions**、 **-version**或 **-deleteOldest**。

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -keepVersions | 指定要保留的最新系統狀態備份數目。 其值必須為正整數。 參數值 **-keepversions： 0** 會刪除所有的系統狀態備份。 |
| -version | 以 MM/DD/YYYY-HH： MM 格式指定備份的版本識別碼。 如果您不知道版本識別碼，請執行 [ [wbadmin get 版本](wbadmin-get-versions.md) ] 命令。<p>您可以使用此命令來刪除由獨佔系統狀態備份所組成的版本。 執行 [ [wbadmin get items](wbadmin-get-items.md) ] 命令以查看版本類型。 |
| -deleteOldest | 刪除最舊的系統狀態備份。 |
| -backupTarget | 指定您想要刪除之備份的儲存位置。 磁片備份的儲存位置可以是磁碟機號、掛接點或以 GUID 為基礎的磁片區路徑。 只需要指定這個值，即可尋找不在本機電腦上的備份。 本機電腦的備份類別目錄中有提供本機電腦備份的相關資訊。 |
| -電腦 | 指定您想要刪除其系統狀態備份的電腦。 當多部電腦備份至相同位置時很有用。 當指定 **-backupTarget** 參數時，應該使用。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要刪除在2013年3月31日上午10:00 建立的系統狀態備份，請輸入：

```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```

若要刪除所有的系統狀態備份，除了三個最新的以外，請輸入：

```
wbadmin delete systemstatebackup -keepVersions:3
```

若要刪除儲存在磁片 f：上最舊的系統狀態備份，請輸入：

```
wbadmin delete systemstatebackup -backupTarget:f:\ -deleteOldest
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin get 版本命令](wbadmin-get-versions.md)

- [wbadmin get items 命令](wbadmin-get-items.md)
