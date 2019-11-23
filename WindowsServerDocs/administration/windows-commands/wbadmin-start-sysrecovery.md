---
title: wbadmin start sysrecovery
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9c653fa52a2a56267d6f0df169f8f9924f2aa94d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362284"
---
# <a name="wbadmin-start-sysrecovery"></a>wbadmin start sysrecovery



使用您指定的參數執行系統復原（裸機復原）。

> [!NOTE]
> 這個子命令只能從 Windows 修復環境執行，而且預設不會列在**Wbadmin**的使用方式文字中。 如需詳細資訊，請參閱[Windows 修復環境（WINDOWS RE）總覽](https://technet.microsoft.com/library/hh825173.aspx)。

若要使用這個子命令執行系統復原，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。

如需如何使用此子命令的範例，請參閱[範例](#BKMK_examples)。

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

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|-版本|以 MM/DD/YYYY-HH： MM 格式指定要復原之備份的版本識別碼。 如果您不知道版本識別碼，請輸入**wbadmin get 版本**。|
|-backupTarget|指定包含您要復原之備份或備份的儲存位置。 當儲存位置不同于通常儲存此電腦的備份時，此參數會很有用。|
|-機器|指定您要復原之電腦的名稱。 當多部電腦已備份到相同的位置時，這個參數很有用。 當指定 **-backupTarget**參數時，應該使用。|
|-restoreAllVolumes|從選取的備份復原所有磁片區。 如果未指定此參數，則只會復原重要的磁片區（包含系統狀態和作業系統元件的磁片區）。 當您需要在系統修復期間復原非重要磁片區時，這個參數很有用。|
|-recreateDisks|將磁片設定復原到建立備份時存在的狀態。</br>警告：此參數會刪除裝載作業系統元件的磁片區上的所有資料。 它也可能會刪除資料磁片區中的資料。|
|-excludeDisks|只有在使用 **-recreateDisks**參數指定時才有效，而且必須輸入為以逗號分隔的磁片識別碼清單（如**wbadmin get 磁片**的輸出中所列）。 排除的磁片不會進行分割或格式化。 此參數可協助保留您不想在復原操作期間修改的磁片資料。|
|-skipBadClusterCheck|略過檢查復原磁片是否有錯誤的叢集資訊。 如果您要還原至替代的伺服器或硬體，建議您不要使用此參數。 您可以隨時在復原磁片上手動執行**chkdsk/b** ，以檢查是否有錯誤的叢集，然後據此更新檔案系統資訊。</br>警告：在您依照所述執行**Chkdsk**之前，已復原的系統上報告的錯誤叢集可能不正確。|
|-quiet|執行命令，而不提示使用者。|

## <a name="BKMK_examples"></a>典型

若要開始復原在2013年3月31日（位於上午9:00，位於磁片磁碟機 d：）上執行的備份資訊，請輸入：
```
wbadmin start sysrecovery -version:03/31/2013-09:00 -backupTarget:d:
```
若要開始從2013年4月30日上午9:00 執行的備份復原資訊，位於共用資料夾 \\\\servername\shared：若是 server01，請輸入：
```
wbadmin start sysrecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
-   [WBBareMetalRecovery](https://technet.microsoft.com/library/jj902461.aspx) Cmdlet