---
title: wbadmin 啟用備份
description: Wbadmin 「啟用備份」的參考主題，它會建立並啟用每日備份排程，或修改現有的備份排程。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a44cfca936e5349e1757d66a4b7b6a8195b44228
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720184"
---
# <a name="wbadmin-enable-backup"></a>wbadmin 啟用備份



建立並啟用每日備份排程，或修改現有的備份排程。 若未指定任何參數，則會顯示目前排定的備份設定。

若要設定或修改每日備份排程，您必須是**Administrators**或**backup Operators**群組的成員。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）

## <a name="syntax"></a>語法

Windows Server 2008 的語法：
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
Windows Server 2008 R2 的語法：
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-allCritical]
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet]
```
Windows Server 2012 和 Windows Server 2012 R2 的語法：
```
wbadmin enable backup
[-addtarget:<BackupTarget>]
[-removetarget:<BackupTarget>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>][-systemState]
[-hyperv:<HyperVComponentsToExclude>]
[-allCritical]
[-systemState] 
[-vssFull | -vssCopy]
[-user:<UserName>]
[-password:<Password>]
[-quiet] 
[-allowDeleteOldBackups]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-addtarget|針對 Windows Server 2008，指定備份的儲存位置。 需要您指定備份的目的地做為磁片識別碼（請參閱備註）。 磁片在使用前會先格式化，並會永久清除其上的任何現有資料。</br>針對 Windows Server 2008 R2 和更新版本，指定備份的儲存位置。 需要您將位置指定為遠端共用資料夾的磁片、磁片區或通用命名慣例（UNC）路徑（\\\\\<servername>\<共用名稱>。 \) 根據預設，備份會儲存在： \\ \\ <servername> \<共用名稱> \windowsimagebackup\<ComputerBackedUp>\. 如果您指定磁片，則會在使用之前先格式化磁片，而且會永久清除其上的任何現有資料。 如果您指定共用資料夾，就無法加入更多位置。 您一次只能指定一個共用資料夾做為儲存位置。</br>重要事項：如果您將備份儲存到遠端共用資料夾，當您再次使用相同的資料夾來備份相同的電腦時，將會覆寫該備份。 此外，如果備份作業失敗，因為舊備份被覆寫，但新備份無法使用，您最後可能沒有任何備份。 在遠端共用資料夾中建立子資料夾以組織備份，可以避免此狀況。 如果您這樣做，子資料夾將需要上層資料夾空間的兩倍。</br>只能在單一命令中指定一個位置。 您可以再次執行命令來新增多個磁片區和磁片備份儲存位置。|
|-removetarget|指定您想要從現有的備份排程中移除的儲存位置。 需要您將位置指定為磁片識別碼（請參閱備註）。|
|-schedule|指定建立備份的當日時間，格式為 HH： MM 並以逗號分隔。|
|-include|針對 Windows Server 2008，指定要包含在備份中的磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱清單（以逗號分隔）。</br>針對 Windows Server 2008 R2and 之後，指定要包含在備份中的專案清單（以逗號分隔）。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\)來結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（*）。|
|-nonRecurseInclude|若為 Windows Server 2008 R2 和更新版本，請指定要包含在備份中的非遞迴、以逗號分隔的專案清單。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\)來結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（*）。 只有在使用-backupTarget 參數時，才應該使用。|
|-排除|針對 Windows Server 2008 R2 和更新版本，指定要從備份中排除的專案清單（以逗號分隔）。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\)來結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（*）。|
|-nonRecurseExclude|針對 Windows Server 2008 R2 和更新版本，指定要從備份中排除的非遞迴、以逗號分隔的專案清單。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\)來結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（*）。|
|-hyperv|指定要包含在備份中的元件清單（以逗號分隔）。 此識別碼可以是元件名稱或元件 GUID （包含或不含大括弧）。|
|-systemState|對於 Windows 7 和 Windows Server 2008 R2 和更新版本，會建立包含系統狀態的備份，以及您使用 **-include**參數指定的任何其他專案。 系統狀態包含開機檔案（Boot.ini、NDTLDR、NTDetect.com）、Windows 登錄，包括 COM 設定、SYSVOL （群組原則和登入腳本）、Active Directory 和 NTDS。網域控制站上的 DIT，如果已安裝憑證服務，則為憑證存放區。 如果您的伺服器已安裝 Web 服務器角色，則會包含 IIS 中繼目錄。 如果伺服器是叢集的一部分，則也會包含叢集服務資訊。|
|-allCritical|指定備份中包含所有重要磁片區（包含作業系統狀態的磁片區）。 如果您要建立完整系統或系統狀態復原的備份，此參數會很有用。 只有在指定-backupTarget 時才應使用，否則命令會失敗。 可以搭配 **-include**選項使用。</br>提示：重要磁片區備份的目標磁片區可以是本機磁片磁碟機，但不能是備份中包含的任何磁片區。|
|-vssFull|針對 Windows Server 2008 R2 和更新版本，會使用磁碟區陰影複製服務（VSS）來執行完整備份。 所有檔案都會進行備份，每個檔案的歷程記錄都會更新以反映它已備份，而先前備份的記錄可能會被截斷。 如果未使用此參數，則 wbadmin start backup 會進行複本備份，但不會更新所備份之檔案的歷程記錄。</br>注意：如果您使用 Windows Server Backup 以外的產品來備份目前備份所包含之磁片區上的應用程式，請勿使用此參數。 這麼做可能會中斷其他備份產品所建立的增量、差異或其他類型的備份，因為它們所依賴的歷程記錄會決定要備份的資料量可能會遺失，而且可能會不必要地執行完整備份。|
|-vssCopy|若為 Windows Server 2008 R2 和更新版本，會使用 VSS 執行複本備份。 所有檔案都會進行備份，但是要備份的檔案歷程記錄並不會更新，因此您可以保留變更、刪除等檔案的所有資訊，以及任何應用程式記錄檔。 使用這種類型的備份並不會影響與此複本備份無關的增量和差異備份順序。 這是預設值。</br>警告：複本備份無法用於增量或差異備份或還原。|
|-使用者|針對 Windows Server 2008 R2 和更新版本，指定具有備份儲存體目的地之寫入權限的使用者（如果它是遠端共用資料夾）。 使用者必須是正在進行備份之電腦上的 Administrators 群組或 Backup Operators 群組的成員。|
|-password|針對 Windows Server 2008 R2 和更新版本，指定參數使用者所提供之使用者名稱的密碼。|
|-quiet|執行子命令，而不提示使用者。|
|-allowDeleteOldBackups|覆寫電腦升級前所做的任何備份。|

## <a name="remarks"></a>備註

若要查看磁片的磁片識別碼值，請輸入**wbadmin 取得磁片**。

## <a name="examples"></a>範例

下列範例會示範如何在不同的備份案例中使用**wbadmin enable backup**命令：

案例 #1
- 排程硬碟磁片磁碟機 e：、d:\mountpoint 和\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\ 的備份
- 將檔案儲存至磁片 DiskID
- 每天上午9:00 執行備份 和 06:00:00 執行報表，
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  案例 #2
- 將資料夾 d:\documents 的備份排程到網路位置\\ \\backupshare\backup1
- 使用備份管理員 Aaren Ekelund （aekel）的網路認證，其為網域 CONTOSOEAST 的成員，以驗證網路共用的存取權。 Aaren 的密碼為 *$ 3hM9 ^ 5lp*。
- 每天上午12:00 執行備份 和下午7:00
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  案例 #3
- 將磁片區 t：和資料夾 d:\documents 的備份排程到磁片磁碟機 h：，但排除資料夾\~d:\documents tmp
- 使用磁碟區陰影複製服務執行完整備份。
- 每天上午1:00 執行備份
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

## <a name="additional-references"></a>其他參考

-   - [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)