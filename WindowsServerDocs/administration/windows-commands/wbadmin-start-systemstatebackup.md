---
title: wbadmin start systemstatebackup
description: Wbadmin start systemstatebackup 的參考文章，它會建立本機電腦的系統狀態備份，並將它儲存在指定的位置。
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386f5053dd547c4b5285a2b9a09cea76238dcf5b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879655"
---
# <a name="wbadmin-start-systemstatebackup"></a>wbadmin start systemstatebackup



建立本機電腦的系統狀態備份，並將它儲存在指定的位置。

> [!NOTE]
> Windows Server Backup 不會在系統狀態備份或系統狀態復原時，備份或復原登錄使用者 hive (HKEY_CURRENT_USER) 。

若要使用這個子命令執行系統狀態備份，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。  (開啟已提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]。 ) 

## <a name="syntax"></a>語法

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

### <a name="parameters"></a>參數

|   參數   |                                                                                                                                                                                                                      描述                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | 指定您要儲存備份的位置。 儲存位置需要磁碟機號或以 GUID 為基礎的磁片區，格式為： \\ \\ ？ \Volume{*guid*}。</br>執行 Windows Server 2008 的電腦不支援系統狀態備份到共用網路資料夾。 如果您的伺服器執行的是 Windows Server 2008 R2 或更新版本，您可以使用命令**backuptarget： \\ \\ servername\sharedFolder \\ **來儲存系統狀態備份。 |
|    -quiet     |                                                                                                                                                                                                   執行子命令，而不提示使用者。                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

如需將系統狀態備份儲存至磁片區的相關資訊，其中包含系統狀態檔案，請參閱 Microsoft 知識庫中的文章 944530 ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439)) 。

## <a name="examples"></a>範例

若要建立系統狀態備份，並將它儲存在磁片區 f 上，請輸入：
```
wbadmin start systemstatebackup -backupTarget:f:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBBackup](/previous-versions/windows/it-pro/windows-8.1-and-8/hh825173(v=win.10)) Cmdlet
