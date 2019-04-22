---
title: Wbadmin delete systemstatebackup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813149"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin delete systemstatebackup



刪除您所指定的系統狀態備份。 如果指定的磁碟區包含以外的本機伺服器的系統狀態備份的備份，不會刪除這些備份。

> [!NOTE]
> Windows Server Backup 不會備份或復原系統狀態備份或系統狀態復原的一部分的登錄使用者 hive 控制檔 (HKEY_CURRENT_USER)。

若要刪除與此子命令的系統狀態備份，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> 必須指定其中一個這些參數： **-keepVersions**， **-版本**，或 **-deleteOldest**。

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-keepVersions|指定要保留最新的系統狀態備份的數目。 值必須是正整數。 參數值 **-keepVersions: 0**會刪除所有系統狀態備份。|
|-version|指定備份的版本識別碼，以 MM/DD/YYYY-hh: mm 格式。 如果您不知道的版本識別碼，鍵入**wbadmin 取得版本**。</br>您可以使用此命令刪除專用的系統狀態備份版本。 使用**wbadmin 取得項目**若要檢視的版本類型。|
|-deleteOldest|刪除最舊的系統狀態備份。|
|-backupTarget|指定您想要刪除之備份的儲存位置。 磁碟備份的儲存位置可以是磁碟機代號、 掛接點或以 GUID 為基礎的磁碟區路徑。 此值只需要指定尋找不是本機電腦的備份。 本機電腦的備份的相關資訊可在本機電腦上的備份目錄。|
|-電腦|指定您想要刪除的系統狀態備份的電腦。 多部電腦備份到相同的位置時很有用。 應何時 **-backupTarget**指定參數。|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="BKMK_examples"></a>範例

若要刪除系統狀態備份，建立於 2013 年 3 月 31 日上午 10:00，請輸入：
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
若要刪除所有的系統狀態備份，除了最新的三個輸入：
```
wbadmin delete systemstatebackup -keepVersions:3
```
若要刪除儲存在磁碟 f 上最舊系統狀態備份，請輸入：
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)