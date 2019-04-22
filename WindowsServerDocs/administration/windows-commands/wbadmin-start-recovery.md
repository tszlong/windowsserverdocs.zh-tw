---
title: wbadmin 開始復原
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f24c9dfeb0ce87474e58d3bd2bce8b68e31cb63
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823279"
---
# <a name="wbadmin-start-recovery"></a>wbadmin 開始復原



執行復原作業，根據您指定的參數。

若要執行此子命令的復原，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元，請按一下**開始**，以滑鼠右鍵按一下**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_Examples)。

## <a name="syntax"></a>語法

```
wbadmin start recovery
-version:<VersionIdentifier>
-items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>}
-itemtype:{Volume | App | File}
[-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}]
[-recursive]
[-overwrite:{Overwrite | CreateCopy | Skip}]
[-notRestoreAcl]
[-skipBadClusterCheck]
[-noRollForward]
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-version|指定要復原 MM/DD/yyyy 的備份的版本識別碼-hh: mm 格式。 如果您不知道的版本識別碼，鍵入**wbadmin 取得版本**。|
|-items|指定要復原的磁碟區、 應用程式、 檔案或資料夾的逗號分隔清單。</br>-如果 **-itemtype**是**磁碟區**，您可以指定單一磁碟區，藉由提供磁碟區的磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱。</br>-如果 **-itemtype**是**應用程式**，您可以指定單一應用程式。 若要復原，應用程式必須向 Windows Server Backup。 您也可以使用值**ADIFM**復原 Active Directory 的安裝。 如需詳細資訊，請參閱中的備註。</br>-如果 **-itemtype**是**檔案**您可以指定檔案或資料夾，但它們應該是相同的磁碟區的一部分，它們應該是相同的父資料夾下。|
|-itemtype|指定要復原項目的類型。 必須是**磁碟區**，**應用程式**，或**檔案**。|
|-backupTarget|指定包含您想要復原之備份的儲存位置。 這個參數很有用的位置不同於此電腦的備份通常的儲存位置。|
|-電腦|指定您想要復原的備份之電腦的名稱。 相同的位置已備份多部電腦時，這個參數是很有用。 它應該使用的時機 **-backupTarget**指定參數。|
|-recoveryTarget|指定要還原至的位置。 這個參數是很有用，如果此位置不同於先前備份的位置。 它也可以使用還原的磁碟區、 檔案或應用程式。 如果您要還原的磁碟區，您可以指定替代磁碟區的磁碟區磁碟機代號。 如果您要還原的檔案或應用程式，您可以指定其他復原位置。|
|-recursive|只有在復原檔案時，才有效。 復原資料夾中的檔案和所有檔案屬於指定的資料夾。 根據預設，會回復它在直接在指定的資料夾中檔案。|
|-overwrite|只有在復原檔案時，才有效。 指定已復原的檔案存在於相同的位置時所要採取的動作。</br>-   **略過**使 Windows Server Backup，以略過現有的檔案，並繼續進行下一步 的檔案復原。</br>-   **CreateCopy**會導致 Windows Server Backup 來建立一份現有的檔案，以便不會修改現有的檔案。</br>-   **覆寫**會導致 Windows Server Backup 從備份檔案以覆寫現有的檔案。|
|-notRestoreAcl|只有在復原檔案時，才有效。 指定未還原的安全性存取控制清單 (Acl) 從備份復原的檔案。 根據預設，會還原安全性 Acl (預設值是 **，則為 true)**。 如果使用此參數，還原檔案的 Acl 會繼承自要還原檔案的位置。|
|-skipBadClusterCheck|只有在復原磁碟區時，才有效。 略過檢查的磁碟，您要復原到不正確的叢集資訊。 如果您要復原到替代伺服器或硬體，我們建議您不要使用此參數。 您可以手動執行命令**chkdsk/b**隨時檢查它們不正確的叢集，並接著據以更新檔案系統資訊的這些磁碟上。</br>重要事項：直到您執行**chkdsk**如所述，錯誤報告在您復原的系統上的叢集可能不正確。|
|-noRollForward|復原應用程式時，才有效。 如果已選取 從備份的最新版本，可讓應用程式的上一個時間點復原。 針對其他版本的應用程式不是最新、 先前的時間點復原是作為預設值。|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="remarks"></a>備註

-   若要檢視適用於特定的備份版本從復原的項目清單，請使用**wbadmin 取得項目**。 如果磁碟區備份當時沒有掛接點或磁碟機代號，這個子命令會傳回應該用於復原磁碟區的 GUID 型磁碟區名稱。
-   當 **-itemtype**是**應用程式**，您可以使用的值**ADIFM**如 **-項目**若要執行的安裝媒體復原所有的作業所需的 Active Directory 網域服務的相關的資料。 **ADIFM**建立 Active Directory 資料庫、 登錄及 SYSVOL 狀態的複本，然後將此資訊儲存在所指定的位置 **-recoveryTarget**。 使用此參數時，才 **-recoveryTarget**指定。

>     [!NOTE]
>     Before using **wbadmin** to perform an install from media operation, you should consider using the **ntdsutil** command because **ntdsutil** only copies the minimum amount of data needed, and it uses a more secure data transport method.

## <a name="BKMK_Examples"></a>範例

若要執行備份的復原作業在 3 月 31，2013 中，在上午 9:00，磁碟區 d:，採取輸入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume -items:d:
```
若要備份的 d 磁碟機執行復原作業在 3 月 31，2013 中，採取上午 9:00，在登錄中，輸入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```
若要執行備份的復原作業在 3 月 31，2013 中，在上午 9:00，d:\folder 和資料夾屬於 d:\folder，採取輸入：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:File -items:d:\folder -recursive
```
若要執行備份的復原從 2013 年 3 月 31 日，上午 9:00，磁碟區採取\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\,類型：
```
wbadmin start recovery -version:03/31/2013-09:00 -itemType:Volume 
-items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
若要執行備份的復原從 2013 年 4 月 30 日，上午 9:00，共用資料夾的採取\\ \\servername\share 從 server01，型別：
```
wbadmin start recovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [開始 WBFileRecovery](https://technet.microsoft.com/library/jj902457.aspx) cmdlet
-   [開始 WBHyperVRecovery](https://technet.microsoft.com/library/jj902463.aspx) cmdlet
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet
-   [開始 WBVolumeRecovery](https://technet.microsoft.com/library/jj902470.aspx) cmdlet