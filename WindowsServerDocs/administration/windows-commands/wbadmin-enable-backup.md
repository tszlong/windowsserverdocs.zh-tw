---
title: wbadmin enable backup
description: Wbadmin enable backup 的參考文章，可建立並啟用每日備份排程，或修改現有的備份排程。
ms.topic: reference
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6be32f6134bacf698d6e28998cbed76e8b50155f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89032066"
---
# <a name="wbadmin-enable-backup"></a>wbadmin enable backup



建立並啟用每日備份排程，或修改現有的備份排程。 如果未指定參數，則會顯示目前已排程的備份設定。

若要設定或修改每日備份排程，您必須是 **Administrators** 或 **backup Operators** 群組的成員。 此外，您必須從提升許可權的命令提示字元中執行 **wbadmin** 。  (開啟提升許可權的命令提示字元，以滑鼠右鍵按一下 **命令提示** 字元，然後按一下 [以 **系統管理員身分執行**]。 ) 

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
|-addtarget|針對 Windows Server 2008，指定備份的儲存位置。 需要您將備份的目的地指定為磁片識別碼 (請參閱備註) 。 磁片會在使用之前格式化，並永久清除該磁片上的任何現有資料。</br>針對 Windows Server 2008 R2 和更新版本，指定備份的儲存位置。 需要您將位置指定為磁片、磁片區或通用命名慣例， (UNC) 路徑指向遠端共用資料夾 (\\ \\ \<servername> \<sharename> \) 。 依預設，備份會儲存在： \\ \\ <servername> \<sharename> \WindowsImageBackup\<ComputerBackedUp>\. 如果您指定磁片，則會在使用之前將磁片格式化，並永久清除該磁片上的任何現有資料。 如果您指定共用資料夾，則無法新增更多位置。 您一次只能指定一個共用資料夾做為儲存位置。</br>重要事項：如果您將備份儲存到遠端共用資料夾，如果您使用相同的資料夾再次備份同一部電腦，將會覆寫該備份。 此外，如果備份作業失敗，因為舊備份被覆寫，但新備份無法使用，您最後可能沒有任何備份。 在遠端共用資料夾中建立子資料夾以組織備份，可以避免此狀況。 如果您這麼做，子資料夾將需要上層資料夾的兩倍的空間。</br>單一命令只能指定一個位置。 您可以再次執行命令，以新增多個磁片區和磁片備份儲存位置。|
|-removetarget|指定您想要從現有的備份排程中移除的儲存位置。 需要您將位置指定為磁片識別碼 (請參閱備註) 。|
|-schedule|指定要建立備份的當日時間，格式為 HH： MM 和逗點分隔。|
|-include|若為 Windows Server 2008，請指定要包含在備份中的磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱清單（以逗號分隔）。</br>若為 Windows Server 2008 R2and 之後，請指定要包含在備份中的專案清單（以逗號分隔）。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (結尾 \) 。 指定檔案的路徑時，您可以在檔案名中使用萬用字元 ( * ) 。|
|-nonRecurseInclude|若為 Windows Server 2008 R2 和更新版本，則會指定要包含在備份中的非遞迴、以逗號分隔的專案清單。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (結尾 \) 。 指定檔案的路徑時，您可以在檔案名中使用萬用字元 ( * ) 。 只有在使用-backupTarget 參數時，才應該使用。|
|-排除|若為 Windows Server 2008 R2 和更新版本，指定要從備份中排除的專案清單（以逗號分隔）。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (結尾 \) 。 指定檔案的路徑時，您可以在檔案名中使用萬用字元 ( * ) 。|
|-nonRecurseExclude|若為 Windows Server 2008 R2 和更新版本，指定要從備份中排除的非遞迴、以逗號分隔的專案清單。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線 (結尾 \) 。 指定檔案的路徑時，您可以在檔案名中使用萬用字元 ( * ) 。|
|-hyperv|指定要包含在備份中的元件清單（以逗號分隔）。 識別碼可以是元件名稱或元件 GUID (具有或不含大括弧) 。|
|-systemState|針對 Windows 7 和 Windows Server 2008 R2 和更新版本，除了您使用 **-include** 參數指定的任何其他專案之外，還會建立包含系統狀態的備份。 系統狀態包含開機檔案 ( # A0、NDTLDR、NTDetect.com) 、Windows 登錄（包括 COM 設定、SYSVOL (群組原則和登入腳本) 、Active Directory 和 NTDS）。網域控制站上的 DIT，如果已安裝憑證服務，則為憑證存放區。 如果您的伺服器已安裝網頁伺服器角色，則會包含 IIS 中繼目錄。 如果伺服器是叢集的一部分，也會包含叢集服務資訊。|
|-allCritical|指定包含作業系統狀態的所有重要磁片區 (磁片區) 包含在備份中。 如果您要建立完整系統或系統狀態復原的備份，此參數很有用。 只有在指定-backupTarget 時才應使用，否則命令會失敗。 可以與 **-include** 選項搭配使用。</br>秘訣：重要磁片區備份的目標磁片區可以是本機磁片磁碟機，但不能是備份中包含的任何磁片區。|
|-vssFull|若為 Windows Server 2008 R2 和更新版本，會使用磁碟區陰影複製服務 (VSS) 來執行完整備份。 所有檔案都會進行備份，每個檔案的歷程記錄都會更新，以反映它已備份，且先前備份的記錄可能會被截斷。 如果未使用此參數，wbadmin 開始備份會進行複本備份，但不會更新正在備份之檔案的歷程記錄。</br>注意：如果您使用非 Windows Server Backup 的產品來備份目前備份所包含之磁片區上的應用程式，請勿使用此參數。 這樣做可能會中斷其他備份產品正在建立的增量、差異或其他類型的備份，因為它們所依賴的歷程記錄會判斷可能遺失的資料量，而且可能會不必要地執行完整備份。|
|-vssCopy|若為 Windows Server 2008 R2 和更新版本，會使用 VSS 執行複本備份。 所有檔案都會進行備份，但是要備份的檔案歷程記錄不會更新，因此您可以保留變更、刪除等檔案的所有資訊，以及任何應用程式記錄檔。 使用這種類型的備份並不會影響增量和差異備份的順序，此順序可能會與此複本備份無關。 這是預設值。</br>警告：無法將複本備份用於增量或差異備份或還原。|
|-使用者|若為 Windows Server 2008 R2 和更新版本，指定具有備份儲存體目的地寫入權限的使用者 (如果是遠端共用資料夾) 。 使用者必須是正在進行備份之電腦上的 Administrators 群組或 Backup Operators 群組的成員。|
|-password|若為 Windows Server 2008 R2 和更新版本，則指定參數使用者所提供之使用者名稱的密碼。|
|-quiet|執行子命令，而不提示使用者。|
|-allowDeleteOldBackups|覆寫電腦升級之前所做的任何備份。|

## <a name="remarks"></a>備註

若要查看磁片的磁片識別碼值，請輸入 **wbadmin get 磁片**。

## <a name="examples"></a>範例

下列範例會示範如何在不同的備份案例中使用 **wbadmin enable backup** 命令：

案例 #1
- 排程硬碟的備份 e：、d:\mountpoint 和 \\ \\ ？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
- 將檔案儲存至磁片 DiskID
- 每天上午9:00 執行備份 和 06:00:00 執行報表，
  ```
  wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  案例 #2
- 將 d:\documents 資料夾的備份排程至網路位置 \\ \\ backupshare\backup1
- 使用 backup administrator Aaren Ekelund (aekel) 的網路認證，也就是網域 CONTOSOEAST 的成員，用來驗證網路共用的存取權。 Aaren 的密碼為 *$ 3hM9 ^ 5lp*。
- 每天上午12:00 執行備份 和 7:00 P.M。
  ```
  wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
  ```
  案例 #3
- 將磁片區 t：和資料夾 d:\documents 的備份排程到磁片磁碟機 h：，但排除 d:\documents \~ tmp 資料夾
- 使用磁碟區陰影複製服務執行完整備份。
- 每天上午1:00 執行備份
  ```
  wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
  ```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
-   [Backup](wbadmin.md)