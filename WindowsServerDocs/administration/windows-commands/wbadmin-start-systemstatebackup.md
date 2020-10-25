---
title: wbadmin start systemstatebackup
description: Wbadmin start systemstatebackup 命令的參考文章，此命令會建立本機電腦的系統狀態備份，並將它儲存在指定的位置。
ms.topic: reference
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: afdfb3f4a52ae0f5897517f8d59069bea820bc4a
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524723"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup

建立本機電腦的系統狀態備份，並將它儲存在指定的位置。

若要使用此命令執行系統狀態備份，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

> [!NOTE]
> Windows Server Backup 不會在系統狀態備份或系統狀態復原時，備份或復原登錄使用者 hive (HKEY_CURRENT_USER) 。

## <a name="syntax"></a>語法

```
wbadmin start systemstatebackup -backupTarget:<VolumeName> [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -backupTarget | 指定您要儲存備份的位置。 儲存位置需要磁碟機號或以 GUID 為基礎的格式磁片區： `\\?\Volume{*GUID*}` 。 使用命令 `-backuptarget:\\servername\sharedfolder\` 來存放系統狀態備份。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要建立系統狀態備份並將其儲存在磁片區 f 上，請輸入：

```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [開始-WBBackup](/powershell/module/windowserverbackup/Start-WBBackup)
