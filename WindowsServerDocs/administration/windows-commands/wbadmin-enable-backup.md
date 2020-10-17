---
title: wbadmin enable backup
description: Wbadmin enable backup 命令的參考文章，它會建立並啟用每日備份排程，或修改現有的備份排程。
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3135df39135a1d085509bdbdabdfa7f88817c3be
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156063"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup

建立並啟用每日備份排程，或修改現有的備份排程。 如果未指定參數，則會顯示目前已排程的備份設定。

若要使用此命令來設定或修改每日備份排程，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

若要查看磁片的磁片識別碼值，請執行 [ [wbadmin get](wbadmin-get-disks.md) disk] 命令。

## <a name="syntax"></a>語法

```
wbadmin enable backup [-addtarget:<BackupTarget>] [-removetarget:<BackupTarget>] [-schedule:<TimeToRunBackup>] [-include:<VolumesToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>][-systemState] [-hyperv:<HyperVComponentsToExclude>] [-allCritical] [-systemState] [-vssFull | -vssCopy] [-user:<UserName>] [-password:<Password>] [-allowDeleteOldBackups]  [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| -addtarget | 指定備份的儲存位置。 需要您將位置指定為磁片、磁片區或通用命名慣例， (UNC) 路徑指向遠端共用資料夾 (`\\<servername>\<sharename>`) 。 依預設，備份會儲存在： `\\<servername>\<sharename> WindowsImageBackup <ComputerBackedUp>` 。 如果您指定磁片，則會在使用之前將磁片格式化，並永久清除該磁片上的任何現有資料。 如果您指定共用資料夾，則無法新增更多位置。 您一次只能指定一個共用資料夾做為儲存位置。<p>**重要事項：** 如果您將備份儲存到遠端共用資料夾，如果您使用相同的資料夾再次備份同一部電腦，則會覆寫該備份。 此外，如果備份作業失敗，最後可能不會有備份，因為較舊的備份將會遭到覆寫，但較新的備份將無法使用。 您可以在遠端共用資料夾中建立子資料夾來組織您的備份，以避免發生此情況。 如果您這樣做，子資料夾需要上層資料夾的兩倍空間。<p>單一命令只能指定一個位置。 您可以再次執行命令，以新增多個磁片區和磁片備份儲存位置。 |
| -removetarget | 指定您想要從現有的備份排程中移除的儲存位置。 需要您將位置指定為磁片識別碼。 |
| -schedule | 指定要建立備份的當日時間，格式為 HH： MM 和逗點分隔。 |
| -include | 指定要包含在備份中的專案清單（以逗號分隔）。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則其結尾應為反斜線 (`\`) 。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 |
| -nonRecurseInclude | 指定要包含在備份中的非遞迴、以逗號分隔的專案清單。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則其結尾應為反斜線 (`\`) 。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 只有在使用 **-backupTarget** 參數時，才應該使用。 |
| -排除 | 指定要從備份中排除的專案清單（以逗號分隔）。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則其結尾應為反斜線 (`\`) 。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 |
| -nonRecurseExclude | 指定要從備份中排除的非遞迴、以逗號分隔的專案清單。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則其結尾應為反斜線 (`\`) 。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 |
| -hyperv | 指定要包含在備份中的元件清單（以逗號分隔）。 識別碼可以是元件名稱或元件 GUID (具有或不含大括弧) 。 |
| -systemState | 建立包含系統狀態的備份，以及您使用 **-include** 參數指定的任何其他專案。 系統狀態包含開機檔案 ( # A0、NDTLDR、NTDetect.com) 、Windows 登錄（包括 COM 設定、SYSVOL (群組原則和登入腳本) 、Active Directory 和 NTDS）。網域控制站上的 DIT，如果已安裝憑證服務，則為憑證存放區。 如果您的伺服器已安裝網頁伺服器角色，則會包含 IIS 中繼目錄。 如果伺服器是叢集的一部分，也會包含叢集服務資訊。 |
| -allCritical | 指定包含作業系統狀態的所有重要磁片區 (磁片區) 包含在備份中。 如果您要建立完整系統或系統狀態復原的備份，此參數很有用。 只有在指定 **-backupTarget** 時，才應使用此值;否則，命令會失敗。 可以與 **-include** 選項搭配使用。<p>**秘訣：** 重要磁片區備份的目標磁片區可以是本機磁片磁碟機，但不能是備份中包含的任何磁片區。 |
| -vssFull | 使用磁碟區陰影複製服務 (VSS) 來執行完整備份。 所有檔案都會進行備份，每個檔案的歷程記錄都會更新，以反映它已備份，且先前備份的記錄可能會被截斷。 如果未使用此參數，則 [wbadmin start backup](wbadmin-start-backup.md) 命令會進行複本備份，但不會更新要備份的檔案歷程記錄。<p>**注意：** 如果您使用非 Windows Server Backup 的產品來備份目前備份所包含之磁片區上的應用程式，請勿使用此參數。 這樣做可能會中斷其他備份產品正在建立的增量、差異或其他類型的備份，因為它們所依賴的歷程記錄會判斷可能遺失的資料量，而且可能會不必要地執行完整備份。 |
| -vssCopy | 使用 VSS 執行複本備份。 所有檔案都會進行備份，但是要備份的檔案歷程記錄不會更新，因此您可以保留變更、刪除等檔案的所有資訊，以及任何應用程式記錄檔。 使用這種類型的備份並不會影響增量和差異備份的順序，此順序可能會與此複本備份無關。 這是預設值。<p>**警告：** 備份複本無法用於增量或差異備份或還原。 |
| -使用者 | 指定具有備份儲存體目的地之寫入權限的使用者 (如果是遠端共用資料夾) 。 使用者必須是電腦上 [ **Administrators** ] 或 [ **Backup Operators** ] 群組的成員，才能進行備份。 |
| -password | 指定參數 **使用者**所提供之使用者名稱的密碼。 |
| -allowDeleteOldBackups | 覆寫電腦升級之前所做的任何備份。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

## <a name="examples"></a>範例

若要在 9:00 AM 和 6:00 PM 為硬碟設定硬碟的每日備份 E：、D:\mountpoint 和 `\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\` ，並將檔案儲存到名為 DiskID 的磁片，請輸入：

```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:E:,D:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

若要將 D:\documents 資料夾的每日備份排程在上午12:00 點和 7:00 PM 至網路位置 `\\backupshare\backup1` ，請使用 **備份操作員**的網路認證 Aaren Ekelund (aekel) ，其密碼是 *$ 3hM9 ^ 5lp* ，而身為 domain CONTOSOEAST 的成員（用來驗證網路共用的存取權），請輸入：

```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: D:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```

若要排程磁片區 T：的每日備份，以及上午1:00 的 D:\documents 資料夾到磁片磁碟機 H：、排除資料夾 `d:\documents\~tmp` ，以及使用磁碟區陰影複製服務執行完整備份，請輸入：

```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)

- [wbadmin enable backup 命令](wbadmin-enable-backup.md)

- [wbadmin 開始備份命令](wbadmin-start-backup.md)

- [wbadmin get 磁片命令](wbadmin-get-disks.md)
