---
title: Wbadmin start systemstatebackup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 998366c1-0a64-45e6-9ed3-4c3f5b8406f0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d98ba295b2a76baf98e85a01a02677d57922877d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440259"
---
# <a name="wbadmin-start-systemstatebackup"></a>Wbadmin start systemstatebackup



建立本機電腦的系統狀態備份，並將其儲存在指定的位置。

> [!NOTE]
> Windows Server Backup 不會備份或復原系統狀態備份或系統狀態復原的一部分的登錄使用者 hive 控制檔 (HKEY_CURRENT_USER)。

若要執行與此子命令的系統狀態備份，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin start systemstatebackup
-backupTarget:<VolumeName>
[-quiet]
```

## <a name="parameters"></a>參數

|   參數   |                                                                                                                                                                                                                      描述                                                                                                                                                                                                                      |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| -backupTarget | 指定您想要用來儲存備份的位置。 儲存體位置需要磁碟機代號或 GUID 型磁碟區的格式： \\ \\？ \Volume {*GUID*}。</br>執行 Windows Server 2008 的電腦上不支援系統狀態備份到共用的網路資料夾。 如果您的伺服器執行 Windows Server 2008 R2 或更新版本，您可以使用命令 **-backuptarget:\\\\servername\sharedFolder\\** 儲存系統狀態備份。 |
|    -quiet     |                                                                                                                                                                                                   子命令會以不執行任何提示給使用者。                                                                                                                                                                                                    |

## <a name="remarks"></a>備註

儲存的相關資訊的系統狀態備份到磁碟區，接著，會包含系統狀態檔案，請參閱 944530 在 Microsoft 知識庫文件 ([https://go.microsoft.com/fwlink/?LinkId=110439](https://go.microsoft.com/fwlink/?LinkId=110439))。

## <a name="BKMK_examples"></a>範例

若要建立系統狀態備份，並將它儲存在磁碟區 f 上，輸入：
```
wbadmin start systemstatebackup -backupTarget:f:
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-wbbackup](https://technet.microsoft.com/library/jj902459.aspx) cmdlet