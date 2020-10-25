---
title: wbadmin start recovery
description: Wbadmin start recovery 命令的參考文章，它會根據您指定的參數執行復原作業。
ms.topic: reference
ms.assetid: 52381316-a0fa-459f-b6a6-01e31fb21612
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 408ccadd830e50e9b51447cc19f7243d349cb3ef
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524753"
---
# <a name="wbadmin-start-recovery"></a>wbadmin start recovery

根據您指定的參數執行復原作業。

若要使用此命令執行復原，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="syntax"></a>語法

```
wbadmin start recovery -version:<VersionIdentifier> -items:{<VolumesToRecover> | <AppsToRecover> | <FilesOrFoldersToRecover>} -itemtype:{Volume | App | File} [-backupTarget:{<VolumeHostingBackup> | <NetworkShareHostingBackup>}] [-machine:<BackupMachineName>] [-recoveryTarget:{<TargetVolumeForRecovery> | <TargetPathForRecovery>}] [-recursive] [-overwrite:{Overwrite | CreateCopy | Skip}] [-notRestoreAcl] [-skipBadClusterCheck] [-noRollForward] [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -version | 以 MM/DD/YYYY-HH： MM 格式指定要復原的備份版本識別碼。 如果您不知道版本識別碼，請執行 [ [wbadmin get 版本] 命令](wbadmin-get-versions.md)。 |
| -items | 指定要復原之磁片區、應用程式、檔案或資料夾的逗號分隔清單。 您必須使用此參數搭配 **-itemtype** 參數。 |
| -itemtype | 指定要復原的專案類型。 必須是**磁片**區、**應用程式****或檔案**。 如果 **-itemtype** 是 *磁片*區，您可以藉由提供磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定單一磁片區。 如果 **-itemtype** 是 *App*，您可以只指定單一應用程式，也可以使用值 **ADIFM** 來復原 Active Directory 的安裝。 若要復原，應用程式必須已向 Windows Server Backup 註冊。 如果 **-itemtype** 是 *File*，則您可以指定檔案或資料夾，但是它們應該是相同磁片區的一部分，而且應該位於相同的父資料夾。 |
| -backupTarget | 指定包含您想要復原之備份的儲存位置。 當位置與此電腦的備份通常儲存位置不同時，此參數很有用。 |
| -電腦 | 指定您想要復原備份的電腦名稱稱。 當指定 **-backupTarget** 參數時，必須使用這個參數。 當多部電腦備份至相同的位置時， **-machine** 參數會很有用。 |
| -recoveryTarget | 指定要還原的位置。 如果這個位置與先前備份的位置不同，這個參數就很有用。 它也可以用於復原磁碟區、檔案或應用程式。 如果您要復原磁碟區，您可以指定替代磁片區的磁片區磁碟機號。 如果您要還原檔案或應用程式，您可以指定替代的復原位置。 |
| -遞迴 | 只有在復原檔案時才有效。 復原資料夾中的檔案，以及從屬於指定資料夾的所有檔案。 依預設，只會復原位於指定資料夾中的檔案。 |
|-覆寫 | 只有在復原檔案時才有效。 指定當正在復原的檔案已經存在於相同位置時要採取的動作。 有效的選項為：<ul><li>**Skip** -導致 Windows Server Backup 略過現有的檔案，並繼續進行下一個檔案的復原。</li><li>**CreateCopy** -導致 Windows Server Backup 建立現有檔案的複本，如此就不會修改現有的檔案。</li><li>**覆寫** -導致 Windows Server Backup 使用備份中的檔案來覆寫現有的檔案。 |
| -notRestoreAcl | 只有在復原檔案時才有效。 指定不還原安全性存取控制清單 (Acl) 從備份復原的檔案。 預設會還原安全性 Acl (預設值為 **true**) 。 如果使用此參數，則會從還原檔案的位置繼承還原檔案的 Acl。 |
| -skipBadClusterCheck | 只有在復原磁片區時才有效。 略過檢查您正在復原的磁片是否有錯誤的叢集資訊。 如果您要復原到替代伺服器或硬體，建議您不要使用此參數。 您可以隨時在這些磁片上手動執行命令 **chkdsk/b** 來檢查是否有錯誤的叢集，然後據此更新檔案系統資訊。<p>**重要事項：** 在您執行 **chkdsk/b**之前，復原系統上回報的錯誤叢集可能不正確。 |
| -noRollForward | 只有在復原應用程式時才有效。 如果您從備份選取最新版本，則可允許應用程式先前的時間點復原。 先前的時間點復原是做為所有其他非最新版本應用程式的預設值。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

#### <a name="remarks"></a>備註

- 若要查看可從特定備份版本復原的專案清單，請執行 [ [wbadmin get items] 命令](wbadmin-get-items.md)。 如果磁片區在備份時沒有掛接點或磁碟機號，則此命令會傳回以 GUID 為基礎的磁片區名稱，以供復原磁片區使用。

- 如果您使用 **ADIFM** 的值來執行「從媒體安裝」作業來復原 Active Directory Domain Services 所需的相關資料， **ADIFM** 會建立 Active Directory 資料庫、登錄和 SYSVOL 狀態的複本，然後將此資訊儲存在 **-recoveryTarget**所指定的位置。 只有在指定 **-recoveryTarget** 時，才使用此參數。

## <a name="examples"></a>範例

若要在2020年3月31日（上午9:00）執行備份復原，請在磁片區 d：中輸入：

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:d:
```

若要從2020年3月31日（從9:00 年3月31日）開始復原至磁片磁碟機 d，請輸入：

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:App -items:Registry -recoverytarget:d:\
```

若要從2020年3月31日（從年3月31日）9:00 開始復原備份，請在 d:\folder 的 d:\folder 和資料夾中，輸入：

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:File -items:d:\folder -recursive
```

若要從2020年3月31日（上午 9:00 A.M.）執行備份復原，請 `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` 輸入：

```
wbadmin start recovery -version:03/31/2020-09:00 -itemType:Volume -items:\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

若要從 server01 的共用資料夾在2020年4月30日上午9:00 執行備份復原， `\\servername\share` 請輸入：

```
wbadmin start recovery -version:04/30/2020-09:00 -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [開始-WBFileRecovery](/powershell/module/windowserverbackup/Start-WBFileRecovery)

- [開始-WBHyperVRecovery](/powershell/module/windowserverbackup/Start-WBHyperVRecovery)

- [開始-WBSystemStateRecovery](/powershell/module/windowserverbackup/Start-WBSystemStateRecovery)

- [開始-WBVolumeRecovery](/powershell/module/windowserverbackup/Start-WBVolumeRecovery)
