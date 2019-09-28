---
title: wbadmin delete systemstatebackup
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1f324cba3fcdae8639009395c4df734a2db6b814
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362518"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup



刪除您指定的系統狀態備份。 如果指定的磁片區包含的備份不是本機伺服器的系統狀態備份，就不會刪除這些備份。

> [!NOTE]
> Windows Server Backup 不會備份或復原登錄使用者 hive （HKEY_CURRENT_USER）做為系統狀態備份或系統狀態修復的一部分。

若要使用這個子命令刪除系統狀態備份，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，請以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）。

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
> 至少必須指定下列其中一個參數： **-keepVersions**、 **-version**或 **-deleteOldest**。

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-keepVersions|指定要保留的最新系統狀態備份數目。 此值必須是正整數。 參數值 **-keepVersions： 0**會刪除所有系統狀態備份。|
|-版本|以 MM/DD/YYYY-HH： MM 格式指定備份的版本識別碼。 如果您不知道版本識別碼，請輸入**wbadmin get 版本**。</br>您可以使用此命令來刪除獨佔系統狀態備份的版本。 使用**wbadmin get items**來查看版本類型。|
|-deleteOldest|刪除最舊的系統狀態備份。|
|-backupTarget|指定您想要刪除之備份的儲存位置。 磁片備份的儲存位置可以是磁碟機號、掛接點或以 GUID 為基礎的磁片區路徑。 此值只需要指定，以尋找不在本機電腦上的備份。 本機電腦上的備份類別目錄中，將會提供本機電腦備份的相關資訊。|
|-機器|指定您要刪除其系統狀態備份的電腦。 當多部電腦備份到相同位置時很有用。 當指定 **-backupTarget**參數時，應該使用。|
|-quiet|執行子命令，而不提示使用者。|

## <a name="BKMK_examples"></a>典型

若要刪除在2013年3月31日上午10:00 點建立的系統狀態備份，請輸入：
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
若要刪除所有的系統狀態備份，但最新三個除外，請輸入：
```
wbadmin delete systemstatebackup -keepVersions:3
```
若要刪除儲存在磁片 f 上最舊的系統狀態備份，請輸入：
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)