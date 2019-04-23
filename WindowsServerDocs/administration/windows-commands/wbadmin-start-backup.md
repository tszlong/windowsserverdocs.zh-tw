---
title: Wbadmin start backup
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 56f3e752-d99a-4c3d-8e97-10303c37dd78
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09b2ffabcea414dd4717a2ffa1f6e860a17f3653
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871699"
---
# <a name="wbadmin-start-backup"></a>Wbadmin start backup



建立備份，使用指定的參數。 如果未指定任何參數，而且您已建立排定的每日備份，這個子命令會使用設定排定的備份來建立備份。 如果未指定參數，它會建立磁碟區陰影複製服務 (VSS) 複製備份，並且將不會更新正在備份的檔案歷程記錄。

若要建立此子命令的一次性備份，您必須隸屬**Backup Operators**群組或有**系統管理員**群組，或者您必須已經被委派適當的權限。 此外，您必須執行**wbadmin**從提升權限的命令提示字元。 (若要開啟提升權限的命令提示字元上按一下滑鼠右鍵**命令提示字元**，然後按一下**系統管理員身分執行**。)

如需如何使用此子命令的範例，請參閱 <<c0> [ 範例](#BKMK_examples)。

## <a name="syntax"></a>語法

適用於 Windows ° Vista 和 Windows Server 2008 的語法：
```
wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<VolumesToInclude>]
[-allCritical]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noinheritAcl]
[-vssFull]
[-quiet]
```
Windows 7 和 Windows Server 2008 R2 的語法和更新版本：
```
Wbadmin start backup
[-backupTarget:{<BackupTargetLocation> | <TargetNetworkShare>}]
[-include:<ItemsToInclude>]
[-nonRecurseInclude:<ItemsToInclude>]
[-exclude:<ItemsToExclude>]
[-nonRecurseExclude:<ItemsToExclude>]
[-allCritical]
[-systemState]
[-noVerify]
[-user:<UserName>]
[-password:<Password>]
[-noInheritAcl]
[-vssFull | -vssCopy] 
[-quiet]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|-backupTarget|指定此備份的儲存位置。 需要的硬碟磁碟機代號 (f): 以磁碟區 GUID 型路徑中的格式\\ \\？ \Volume{GUID}，或到遠端共用資料夾的通用命名慣例 (UNC) 路徑 (\\\\\<伺服器名稱 >\<sharename >\)。 根據預設，備份會儲存在： \\ \\ <servername>\<共用名稱 >\** WindowsImageBackup * *\\<ComputerBackedUp>\.</br>重要事項：如果您將備份儲存到遠端共用資料夾時，如果您使用的相同資料夾再次備份相同的電腦將會覆寫該備份。 此外，如果備份作業失敗，您可能會得到沒有任何備份因為較舊的備份將會覆寫，但將無法使用較新的備份。 您可以在 遠端共用資料夾來組織您的備份中建立子資料夾來避免。 如果您這樣做，子資料夾需要兩倍的空間作為父資料夾。|
|-include|針對 Windows ° Vista 和 Windows Server 2008，指定磁碟機代號、 磁碟區掛接點或要包含在備份中的 GUID 型磁碟區名稱的逗號分隔清單。 應該使用這個參數時，才 **-backupTarget**參數使用。</br>Windows 7 和 Windows Server 2008 R2 和更新版本中，指定要包含在備份中的項目以逗號分隔清單。 您可以包含多個檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\\)。 您可以使用萬用字元 (\*) 中的檔案名稱時指定檔案的路徑。 應該使用時，才 **-backupTarget**參數使用。|
|-exclude|Windows 7 和 Windows Server 2008 R2 和更新版本中，指定以逗號分隔的清單項目，以從備份中排除。 您可以排除檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\\)。 您可以使用萬用字元 (\*) 中的檔案名稱時指定檔案的路徑。 應該使用時，才 **-backupTarget**參數使用。|
|-nonRecurseInclude|Windows 7 和 Windows Server 2008 R2 和更新版本中，指定非遞迴、 以逗號分隔清單中要包含在備份中的項目。 您可以包含多個檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\\)。 您可以使用萬用字元 (\*) 中的檔案名稱時指定檔案的路徑。 應該使用時，才 **-backupTarget**參數使用。|
|-nonRecurseExclude|Windows 7 和 Windows Server 2008 R2 和更新版本中，指定非遞迴、 從備份中排除的項目以逗號分隔清單。 您可以排除檔案、 資料夾或磁碟區。 磁碟區路徑，可以使用磁碟機代號、 磁碟區掛接點或以 GUID 為基礎的磁碟區名稱來指定。 如果您使用 GUID 型磁碟區的名稱，它應該終止加上反斜線 (\\)。 您可以使用萬用字元 (\*) 中的檔案名稱時指定檔案的路徑。 應該使用時，才 **-backupTarget**參數使用。|
|-allCritical|指定備份中包含所有重要磁碟區 （磁碟區含有作業系統的狀態）。 這個參數是很有用，如果您要建立的裸機復原備份。 應該使用它時，才 **-backupTarget**已指定，否則命令將會失敗。 適用於 **-包含**選項。</br>秘訣：重要磁碟區備份的目標磁碟區可以是本機的磁碟機，但不是能是任何包含在備份磁碟區。|
|-systemState|Windows 7 和 Windows Server 2008 R2 和更新版本中，會建立備份，其中包括系統狀態，除了您指定的任何其他項目 **-包含**參數。 系統狀態包含開機檔案 （Boot.ini、 NDTLDR、 NTDetect.com），包括 COM 設定 SYSVOL （群組原則和登入指令碼）、 Active Directory 和 NTDS Windows 登錄。網域控制站上的 DIT，如果已安裝憑證服務，將憑證存放區。 如果您的伺服器已安裝 Web 伺服器角色，IIS 中繼目錄將會包含。 如果伺服器是叢集的一部分，也會包含叢集服務的資訊。|
|-noVerify|指定備份儲存到卸除式媒體 （例如 DVD) 不會驗證有錯誤。 如果您不使用這個參數，就會儲存到卸除式媒體的備份驗證錯誤。|
|-使用者|如果將備份儲存到遠端共用資料夾，請使用資料夾的寫入權限指定的使用者名稱。|
|-password|指定使用者名稱參數所提供的密碼 **-使用者**。|
|-noInheritAcl|適用於所提供的認證對應的存取控制清單 (ACL) 權限 **-使用者**並 **-密碼**參數\\ \\ \<伺服器名稱 >\<sharename > \WindowsImageBackup\<ComputerBackedUp > \ （包含備份的資料夾）。 若要存取備份更新版本中，您必須使用這些認證或是 Administrators 群組或具有共用資料夾的電腦上的 「 備份操作員 」 群組的成員。 如果 **-noInheritAcl**是未使用，從遠端共用資料夾的 ACL 權限會套用至\<ComputerBackedUp > 根據預設，讓任何人使用遠端共用資料夾存取權可存取備份的資料夾。|
|-vssFull|執行使用磁碟區陰影複製服務 (VSS) 的完整備份。 所有的檔案備份，每個檔案的歷程記錄會更新以反映它已備份，，和先前的備份記錄檔可能會被截斷。 如果未使用此參數**wbadmin start backup**使複製備份，但不是更新要備份的檔案的歷程記錄。</br>注意：如果您使用產品以外的 Windows Server Backup 來備份會包含在目前的備份磁碟區的應用程式，請勿使用此參數。 這麼做因此有可能會破壞的累加、 差異或另一種建立其他備份產品，因為它們會判斷要備份的資料量可能會遺漏依賴的歷程記錄，而且它們可能會執行完整的備份備份必要。|
|-vssCopy|Windows 7 和 Windows Server 2008 R2 和更新版本中，會執行使用 VSS 複製備份 所有的檔案備份，但進行備份的檔案歷程記錄不會更新，讓您保留上的所有資訊的檔案，其中的變更、 刪除及等等，以及任何應用程式記錄檔。 使用此類型的備份不會影響的增量備份和差異備份可能會發生獨立於此複製備份的順序。 這是預設值。</br>警告：複製備份不能用於增量或差異備份或還原。|
|-quiet|子命令會以不執行任何提示給使用者。|

## <a name="BKMK_examples"></a>範例

下列範例會顯示如何**wbadmin start backup**命令可用於不同的備份案例：

案例 #1
-   建立備份的磁碟區 e:、 d:\mountpoint，並\\ \\？ \Volume{cc566d14-4410-11d9-9d93-806e6f6e6963}
-   將備份儲存到磁碟區 f:
```
wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
```
案例 #2
-   執行一次性的備份*f:\folder1*並*h:\folder2*到磁碟區*d:*。
-   備份系統狀態
-   請複製備份，以便正常排程差異備份不會受到影響。
```
wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
```
案例 #3
-   執行一次性的備份*d:\folder1* ，應該備份非遞迴的方式。
-   備份至網路位置的資料夾 *\\ \\backupshare\backup1*
-   限制存取權的成員備份**系統管理員**或是**Backup Operators**群組。
```
wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
