---
title: wbadmin start backup
description: Wbadmin [開始備份] 命令的參考文章，此命令會使用指定的參數建立備份。
ms.topic: reference
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a99d467aed8df57660f46b5efbcb4b8df5feb215
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524843"
---
# <a name="wbadmin-start-backup"></a>wbadmin start backup

使用指定的參數建立備份。 如果未指定任何參數，且您已建立排程的每日備份，此命令會使用排程備份的設定來建立備份。 如果指定了參數，則會建立磁碟區陰影複製服務 (VSS) 複本備份，且不會更新正在備份之檔案的歷程記錄。

若要使用此命令建立一次性備份，您必須是 **Backup Operators** 群組或 **Administrators** 群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元中，以滑鼠右鍵按一下 [**命令提示**字元]，然後選取 [以**系統管理員身分執行**]，以執行**wbadmin** 。

## <a name="syntax"></a>語法

```com
wbadmin start backup [-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}] [-include:<ItemsToInclude>] [-nonRecurseInclude:<ItemsToInclude>] [-exclude:<ItemsToExclude>] [-nonRecurseExclude:<ItemsToExclude>] [-allCritical] [-systemState] [-noVerify] [-user:<UserName>] [-password:<Password>] [-noInheritAcl] [-vssFull | -vssCopy] [-quiet]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
|--|--|
| -backupTarget | 指定此備份的存放位置。 需要硬碟磁碟機號 (f： ) 、格式為的磁片區 GUID 路徑 `\\?\Volume{GUID}` ，或在遠端共用資料夾 (UNC) 路徑的通用命名慣例 `(\\<servername>\<sharename>\)` 。 依預設，備份會儲存在： `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` 。 |
| -include | 指定要包含在備份中的專案清單（以逗號分隔）。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (`\`) 來終止。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 **-Include**參數只能搭配 **-backupTarget**參數使用。 |
| -排除 | 指定要從備份中排除的專案清單（以逗號分隔）。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (`\`) 來終止。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 **-Exclude**參數只能搭配 **-backupTarget**參數使用。 |
| -nonRecurseInclude | 指定要包含在備份中的非遞迴、以逗號分隔的專案清單。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (`\`) 來終止。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 **-NonRecurseInclude**參數只能搭配 **-backupTarget**參數使用。 |
| -nonRecurseExclude | 指定要從備份中排除的非遞迴、以逗號分隔的專案清單。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (`\`) 來終止。 `*`指定檔案的路徑時，您可以在檔案名中使用萬用字元 () 。 **-NonRecurseExclude**參數只能搭配 **-backupTarget**參數使用。 |
| -allCritical | 指定包含作業系統狀態的所有重要磁片區 (磁片區) 包含在備份中。 如果您要建立裸機復原的備份，此參數很有用。 只有在指定 **-backupTarget** 時才應使用，否則命令會失敗。 可以與 **-include** 選項搭配使用。<p>**秘訣：** 重要磁片區備份的目標磁片區可以是本機磁片磁碟機，但不能是備份中包含的任何磁片區。 |
| -systemState | 建立包含系統狀態的備份，以及您使用 **-include** 參數指定的任何其他專案。 系統狀態包含開機檔案 ( # A0、NDTLDR、NTDetect.com) 、Windows 登錄（包括 COM 設定、SYSVOL (群組原則和登入腳本) 、Active Directory 和 NTDS）。網域控制站上的 DIT，如果已安裝憑證服務，則為憑證存放區。 如果您的伺服器已安裝網頁伺服器角色，則會包含 IIS 中繼目錄。 如果伺服器是叢集的一部分，也會包含叢集服務資訊。 |
| -noVerify | 指定將備份儲存到卸載式媒體 (例如 DVD) 不會驗證是否有錯誤。 如果您未使用此參數，則會驗證儲存至卸載式媒體的備份是否有錯誤。 |
| -使用者 | 如果備份儲存到遠端共用資料夾，請指定具有該資料夾寫入權限的使用者名稱。 |
| -password | 指定參數 **使用者**所提供之使用者名稱的密碼。 |
| -noInheritAcl | 套用存取控制清單 (ACL) 許可權，這些許可權對應至 **-user** 和 **-password** 參數所提供的認證，以 `\\<servername>\<sharename>\WindowsImageBackup\<ComputerBackedUp>\` (包含備份) 的資料夾。 若要稍後存取備份，您必須使用這些認證，或是具有共用資料夾之電腦上的 Administrators 群組或 Backup Operators 群組的成員。 如果未使用 **-noInheritAcl** ，則預設會將遠端共用資料夾的 ACL 許可權套用到 `\<ComputerBackedUp>` 資料夾，讓具有遠端共用資料夾存取權的任何人都可以存取備份。 |
| -vssFull | 使用磁碟區陰影複製服務 (VSS) 來執行完整備份。 所有檔案都會進行備份，每個檔案的歷程記錄都會更新，以反映它已備份，且先前備份的記錄可能會被截斷。 如果未使用此參數，則 **wbadmin 開始備份** 會進行複本備份，但不會更新要備份的檔案歷程記錄。<p>**注意：** 如果您使用非 Windows Server Backup 的產品來備份目前備份所包含之磁片區上的應用程式，請勿使用此參數。 這樣做可能會中斷其他備份產品正在建立的增量、差異或其他類型的備份，因為它們所依賴的歷程記錄會判斷可能遺失的資料量，而且可能會不必要地執行完整備份。 |
| -vssCopy | 使用 VSS 執行複本備份。 所有檔案都會進行備份，但是要備份的檔案歷程記錄不會更新，因此您可以保留變更、刪除等檔案的所有資訊，以及任何應用程式記錄檔。 使用這種類型的備份並不會影響增量和差異備份的順序，此順序可能會與此複本備份無關。 這是預設值。<p>**警告：** 複本備份無法用於增量或差異備份或還原。 |
| -quiet | 在不提示使用者的情況下執行命令。 |

#### <a name="remarks"></a>備註

- 如果您將備份儲存到遠端共用資料夾，然後對相同的電腦和相同的遠端共用資料夾執行另一次備份，您將會覆寫先前的備份。

- 如果您的備份作業失敗，您最後可以不進行備份，因為較舊的備份會被覆寫，但較新的備份無法使用。 若要避免這種情況，建議您在遠端共用資料夾中建立子資料夾來組織您的備份。 不過，由於此組織的原因，您必須有兩倍的空間可作為父資料夾。

## <a name="examples"></a>範例

若要建立磁片區 *e：*、 *d： \\ 裝入*點和磁片區 `\\?\Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}\` *f：* 的備份，請輸入：

```
wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```

若要執行 *f： \\ folder1* 和 *h： \\ folder2* 到磁片區 *d：* 的一次性備份，以備份系統狀態，並進行複本備份以使一般排定的差異備份不受影響，請輸入：

```
wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
```

若要對網路位置執行 *d： \\ folder1* 的單次、非遞迴備份 `\\backupshare\backup1*` ，以及限制 **Administrators** 或 **backup Operators** 群組成員的存取權，請輸入：

```
wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [wbadmin 命令](wbadmin.md)
