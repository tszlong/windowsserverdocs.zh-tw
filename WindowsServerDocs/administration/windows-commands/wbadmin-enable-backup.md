---
title: Wbadmin 啟用備份
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c0e57f8a-70fa-4c60-9754-e762e8ad8772
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fd0bea5da83ca9351d5ea1028c94392bdb40422
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845539"
---
# <a name="wbadmin-enable-backup"></a>Wbadmin 啟用備份



建立並可讓每日備份排程或修改現有的備份排程。 使用指定的任何參數，它會顯示目前排定的備份設定。

若要設定或修改每日備份排程，您必須是成員，或是**系統管理員**或是**Backup Operators**群組。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

適用於 Windows Server 2008 的語法：
```
wbadmin enable backup
[-addtarget:<BackupTargetDisk>]
[-removetarget:<BackupTargetDisk>]
[-schedule:<TimeToRunBackup>]
[-include:<VolumesToInclude>]
[-allCritical]
[-quiet]
```
適用於 Windows Server 2008 R2 的語法：
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
適用於 Windows Server 2012 和 Windows Server 2012 R2 的語法：
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

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-addtarget|對於 Windows Server 2008 中，指定備份的儲存位置。 需要您做為磁碟識別碼指定為備份目的地 （請參閱 < 備註 >）。 使用之前，格式化磁碟，其上的任何現有資料會永久清除。</br>適用於 Windows Server 2008 R2 和更新版本中，指定備份的儲存位置。 您必須要將位置指定為磁碟、 磁碟區或遠端共用資料夾的通用命名慣例 (UNC) 路徑 (\\\\\<伺服器名稱 >\<共用名稱 >\)。 根據預設，備份會儲存在： \\ \\ <servername>\<共用名稱 > \WindowsImageBackup\<ComputerBackedUp >\. 如果您指定的磁碟，磁碟將會格式化才能使用，和在其上任何現有的資料會永久清除。 如果您指定的共用的資料夾時，您無法新增更多位置。 您只能一次做為儲存體位置指定一個共用的資料夾。</br>重要事項：如果您將備份儲存到遠端共用資料夾時，如果您使用的相同資料夾再次備份相同的電腦將會覆寫該備份。 此外，如果備份作業失敗，您可能會得到沒有任何備份因為較舊的備份將會覆寫，但將無法使用較新的備份。 您可以在 遠端共用資料夾來組織您的備份中建立子資料夾來避免。 如果您這樣做時，子資料夾需要兩次之父資料夾的空間。</br>在單一命令，可以指定唯一一個位置。 再次執行命令，可以加入多個磁碟區和磁碟的備份儲存體位置。|
|-removetarget|指定您想要從現有的備份排程中移除的儲存位置。 您必須要做為磁碟識別碼指定的位置 （請參閱 < 備註 >）。|
|-schedule|指定一天建立備份時，才會格式化為 hh: mm 和逗號分隔。|
|-include|對於 Windows Server 2008 中，指定磁碟機代號、 磁碟區掛接點或要包含在備份中的 GUID 型磁碟區名稱的逗號分隔清單。</br>針對 Windows Server 2008 R2and 更新版本中，指定要包含在備份中的項目以逗號分隔清單。 您可以包含多個檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\)。 指定檔案的路徑時，您可以使用萬用字元 （*） 在 檔案名稱。|
|-nonRecurseInclude|適用於 Windows Server 2008 R2 和更新版本，請指定非遞迴、 以逗號分隔清單中要包含在備份中的項目。 您可以包含多個檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\)。 指定檔案的路徑時，您可以使用萬用字元 （*） 在 檔案名稱。 只有在使用-backupTarget 參數時，才應該使用。|
|-exclude|適用於 Windows Server 2008 R2 和更新版本中，指定要從備份中排除項目以逗號分隔的清單。 您可以排除檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\)。 指定檔案的路徑時，您可以使用萬用字元 （*） 在 檔案名稱。|
|-nonRecurseExclude|適用於 Windows Server 2008 R2 和更新版本，請指定非遞迴、 從備份中排除的項目以逗號分隔清單。 您可以排除檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\)。 指定檔案的路徑時，您可以使用萬用字元 （*） 在 檔案名稱。|
|-hyperv|指定要包含在備份中的元件。 以逗號分隔清單。 元件名稱或元件 GUID （不論有沒有大括號），可能是識別項。|
|-systemState|Windows 7 和 Windows Server 2008 R2 和更新版本中，會建立備份，其中包括系統狀態，除了您指定的任何其他項目 **-包含**參數。 系統狀態包含開機檔案 （Boot.ini、 NDTLDR、 NTDetect.com），包括 COM 設定 SYSVOL （群組原則和登入指令碼）、 Active Directory 和 NTDS Windows 登錄。網域控制站上的 DIT，如果已安裝憑證服務，將憑證存放區。 如果您的伺服器已安裝 Web 伺服器角色，IIS 中繼目錄將會包含。 如果伺服器是叢集的一部分，則也包含叢集服務資訊。|
|-allCritical|指定備份中包含所有重要磁碟區 （磁碟區含有作業系統的狀態）。 這個參數是很有用，如果您要建立完整的系統或系統狀態復原的備份。 指定-backupTarget 時，才應該使用它，否則命令將會失敗。 適用於 **-包含**選項。</br>秘訣：重要磁碟區備份的目標磁碟區可以是本機的磁碟機，但不是能是任何包含在備份磁碟區。|
|-vssFull|適用於 Windows Server 2008 R2 和更新版本，會執行完整備份使用磁碟區陰影複製服務 (VSS)。 所有的檔案備份，每個檔案的歷程記錄會更新以反映它已備份，，和先前的備份記錄檔可能會被截斷。 如果未使用此參數 wbadmin start backup 會複製備份，但要備份的檔案歷程記錄不會更新。</br>注意：如果您使用產品以外的 Windows Server Backup 來備份會包含在目前的備份磁碟區的應用程式，請勿使用此參數。 這麼做因此有可能會破壞的累加、 差異或另一種建立其他備份產品，因為它們會判斷要備份的資料量可能會遺漏依賴的歷程記錄，而且它們可能會執行完整的備份備份必要。|
|-vssCopy|適用於 Windows Server 2008 R2 和更新版本，會執行使用 VSS 複製備份 所有的檔案備份，但進行備份的檔案歷程記錄不會更新，讓您保留上的所有資訊的檔案，其中的變更、 刪除及等等，以及任何應用程式記錄檔。 使用此類型的備份不會影響的增量備份和差異備份可能會發生獨立於此複製備份的順序。 這是預設值。</br>警告：複製備份不能用於增量或差異備份或還原。|
|-使用者|適用於 Windows Server 2008 R2 和更新版本中，指定的使用者使用的備份儲存體目的地的 「 寫入 」 權限 （如果它是遠端共用的資料夾）。 使用者必須是 Administrators 群組或取得備份的電腦上的 Backup Operators 群組的成員。|
|-password|適用於 Windows Server 2008 R2 和更新版本中，指定參數所提供的使用者名稱的密碼-使用者。|
|-quiet|子命令會以不執行任何提示給使用者。|
|-allowDeleteOldBackups|會覆寫任何之前已升級之電腦的備份。|

## <a name="remarks"></a>備註

若要檢視您的磁碟的磁碟識別碼值，請輸入**wbadmin 取得磁碟**。

## <a name="BKMK_examples"></a>範例

下列範例會顯示如何**wbadmin 啟用備份**命令可用於不同的備份案例：

案例 #1
-   排程備份的硬碟磁碟機 e:、 d:\mountpoint，並\\ \\？ \Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
-   將檔案儲存到磁碟 DiskID
-   每日在上午 9:00 執行備份 和下午 6:00
```
wbadmin enable backup -addtarget:DiskID -schedule:09:00,18:00 -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
案例 #2
-   排程備份的網路位置資料夾 d:\documents \\ \\backupshare\backup1
-   備份的系統管理員 Aaren Ekelund (aekel)，身為成員的網域來驗證存取權的網路共用的 NETWORK2 使用網路認證。 Aaren 的密碼 *$3 hM 9 ^ 5lp*。
-   執行備份，在每天上午 12:00 和下午 7:00
```
wbadmin enable backup –addtarget:\\backupshare\backup1 –include: d:\documents –user:CONTOSOEAST\aekel –password:$3hM9^5lp –schedule:00:00,19:00
```
案例 #3
-   排程備份的磁碟區 t 和資料夾 d:\documents 磁碟 h:，但排除資料夾 d:\documents\~tmp
-   執行完整備份，使用磁碟區陰影複製服務。
-   每日在上午 1:00 執行備份
```
wbadmin enable backup –addtarget:H: –include T:,D:\documents –exclude D:\documents\~tmp –vssfull –schedule:01:00
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)