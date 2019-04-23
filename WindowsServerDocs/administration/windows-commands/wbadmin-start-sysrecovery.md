---
title: Wbadmin start sysrecovery
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95b8232f-7c42-452b-838e-15b0cf6faebe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8e0ff114d09d70b9e50e8c4ea6af6330c74128c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847159"
---
# <a name="wbadmin-start-sysrecovery"></a>Wbadmin start sysrecovery



執行系統復原 （裸機復原），使用您指定的參數。

> [!NOTE]
> 此子命令可以執行只能從 Windows 修復環境中，而未列出預設的使用方式的文字**Wbadmin**。 如需詳細資訊，請參閱 < [Windows 修復環境 (Windows RE) 概觀](https://technet.microsoft.com/library/hh825173.aspx)。

若要執行系統復原與此子命令，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
wbadmin start sysrecovery
-version:<VersionIdentifier>
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-restoreAllVolumes]
[-recreateDisks]
[-excludeDisks]
[-skipBadClusterCheck]
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-version|指定要復原 MM/DD/yyyy 的備份的版本識別碼-hh: mm 格式。 如果您不知道的版本識別碼，鍵入**wbadmin 取得版本**。|
|-backupTarget|指定包含您想要復原的備份儲存位置。 不同於此電腦的備份通常的儲存位置的儲存位置時，這個參數是很有用。|
|-電腦|指定您想要復原的電腦名稱。 相同的位置已備份多部電腦時，這個參數是很有用。 應何時 **-backupTarget**指定參數。|
|-restoreAllVolumes|從選取的備份中復原所有磁碟區。 如果未指定此參數，則會復原僅重要磁碟區 （包含系統狀態與作業系統元件的磁碟區）。 您需要復原系統復原期間的非重要磁碟區時，則此參數會很有用。|
|-recreateDisks|復原的磁碟設定，以建立備份時已存在的狀態。</br>警告：此參數在主機作業系統元件時，都會刪除磁碟區上的所有資料。 從資料磁碟區，它可能也會刪除資料。|
|-excludeDisks|只有在指定時才有效 **-recreateDisks**參數，因此，必須輸入以逗號分隔的磁碟識別碼清單 (如下所示的輸出**wbadmin 取得磁碟**)。 已排除的磁碟未分割或格式化。 此參數可協助保存您不想在復原作業期間修改的磁碟上的資料。|
|-skipBadClusterCheck|檢查您的復原磁碟，如需不正確的叢集資訊時，略過。 如果您要復原到替代伺服器或硬體，我們建議您不要使用此參數。 您可以手動執行**chkdsk/b**隨時檢查它們不正確的叢集，並接著據以更新檔案系統資訊復原磁碟上。</br>警告：直到您執行**chkdsk**如所述，錯誤報告在您復原的系統上的叢集可能不正確。|
|-quiet|使用者不需提示執行命令。|

## <a name="BKMK_examples"></a>範例

若要開始從執行於 2013 年 3 月 31 日上午 9:00，備份復原資訊位於磁碟機 d:，型別：
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
若要開始復原從執行於 2013 年 4 月 30 日上午 9:00，備份的資訊位於 共用資料夾\\ \\servername\shared: for server01 中，輸入：
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) cmdlet