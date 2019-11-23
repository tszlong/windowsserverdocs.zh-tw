---
title: wbadmin 開始備份
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c8eb017e8bf49191c33cd2d9f0cf4a62b08ebb07
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362336"
---
# <a name="wbadmin-start-backup"></a>wbadmin 開始備份



使用指定的參數建立備份。 如果未指定任何參數，而且您已建立排程的每日備份，則此子命令會使用排程備份的設定來建立備份。 如果指定了參數，它會建立磁碟區陰影複製服務（VSS）複本備份，而且不會更新正在備份之檔案的歷程記錄。

若要使用這個子命令建立一次性備份，您必須是**Backup Operators**群組或**Administrators**群組的成員，或者必須已被委派適當的許可權。 此外，您必須從提升許可權的命令提示字元執行**wbadmin** 。 （若要開啟提升許可權的命令提示字元，以滑鼠右鍵按一下**命令提示**字元，然後按一下 [以**系統管理員身分執行**]）

如需如何使用此子命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

Windows Vista 和 Windows Server 2008 的語法：
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
Windows 7 和 Windows Server 2008 R2 和更新版本的語法：
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

## <a name="parameters"></a>Parameters

|參數|描述|
|---------|-----------|
|-backupTarget|指定此備份的儲存位置。 需要硬碟磁碟機號（f：）、磁片區 GUID 路徑，格式為 \\\\？\\磁片區 {GUID}，或遠端共用資料夾的通用命名慣例（UNC）路徑（\\\\\<servername >\\\<共用名稱 >\\）。 根據預設，備份將會儲存在： \\\\\<servername >\\\<共用名稱 >\\**WindowsImageBackup**\\\<ComputerBackedUp >\\。</br>重要事項：如果您將備份儲存到遠端共用資料夾，當您再次使用相同的資料夾來備份相同的電腦時，將會覆寫該備份。 此外，如果備份作業失敗，最後可能不會有備份，因為將會覆寫較舊的備份，但較新的備份將無法使用。 您可以在遠端共用資料夾中建立子資料夾來組織您的備份，以避免發生這種情況。 如果您這樣做，子資料夾將需要兩倍的空間做為上層資料夾。|
|-include|針對 Windows Vista 和 Windows Server 2008，指定要包含在備份中的磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱清單（以逗號分隔）。 只有在使用 **-backupTarget**參數時，才應該使用這個參數。</br>若是 Windows 7 和 Windows Server 2008 R2 和更新版本，請指定要包含在備份中的專案清單（以逗號分隔）。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\\）結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（\*）。 只有在使用 **-backupTarget**參數時，才應該使用。|
|-排除|針對 Windows 7 和 Windows Server 2008 R2 和更新版本，指定要從備份中排除的專案清單（以逗號分隔）。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\\）結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（\*）。 只有在使用 **-backupTarget**參數時，才應該使用。|
|-nonRecurseInclude|針對 Windows 7 和 Windows Server 2008 R2 和更新版本，指定要包含在備份中的非遞迴、以逗號分隔的專案清單。 您可以包含多個檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\\）結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（\*）。 只有在使用 **-backupTarget**參數時，才應該使用。|
|-nonRecurseExclude|針對 Windows 7 和 Windows Server 2008 R2 和更新版本，指定要從備份中排除的非遞迴、以逗號分隔的專案清單。 您可以排除檔案、資料夾或磁片區。 您可以使用磁片區磁碟機號、磁片區掛接點或以 GUID 為基礎的磁片區名稱來指定磁片區路徑。 如果您使用以 GUID 為基礎的磁片區名稱，則應該以反斜線（\\）結束。 指定檔案的路徑時，您可以在檔案名中使用萬用字元（\*）。 只有在使用 **-backupTarget**參數時，才應該使用。|
|-allCritical|指定備份中包含所有重要磁片區（包含作業系統狀態的磁片區）。 如果您要建立裸機復原的備份，這個參數很有用。 只有在指定 **-backupTarget**時才應使用，否則命令會失敗。 可以搭配 **-include**選項使用。</br>提示：重要磁片區備份的目標磁片區可以是本機磁片磁碟機，但不能是備份中包含的任何磁片區。|
|-systemState|對於 Windows 7 和 Windows Server 2008 R2 和更新版本，會建立包含系統狀態的備份，以及您使用 **-include**參數指定的任何其他專案。 系統狀態包含開機檔案（Boot.ini、NDTLDR、NTDetect.com）、Windows 登錄，包括 COM 設定、SYSVOL （群組原則和登入腳本）、Active Directory 和 NTDS。網域控制站上的 DIT，如果已安裝憑證服務，則為憑證存放區。 如果您的伺服器已安裝 Web 服務器角色，則會包含 IIS 中繼目錄。 如果伺服器是叢集的一部分，則也會包含叢集服務資訊。|
|-noVerify|指定儲存到卸載式媒體（例如 DVD）的備份不會驗證是否有錯誤。 如果您未使用此參數，則會驗證儲存到卸載式媒體的備份是否有錯誤。|
|-使用者|如果將備份儲存到遠端共用資料夾，請指定具有資料夾寫入權限的使用者名稱。|
|-password|指定參數**使用者**所提供之使用者名稱的密碼。|
|-noInheritAcl|將對應至 **-user**和 **-password**參數提供之認證的存取控制清單（ACL）許可權套用至 \\\\\<servername >\\\<共用名稱 >\\WindowsImageBackup\\\<ComputerBackedUp >\\ （包含備份的資料夾）。 若要稍後存取備份，您必須使用這些認證，或在具有共用資料夾的電腦上，是 Administrators 群組或 Backup Operators 群組的成員。 如果未使用 **-noInheritAcl** ，則遠端共用資料夾的 ACL 許可權會依預設套用至 \\\<ComputerBackedUp > 資料夾，讓具有遠端共用資料夾存取權的任何人都可以存取備份。|
|-vssFull|使用磁碟區陰影複製服務（VSS）執行完整備份。 所有檔案都會進行備份，每個檔案的歷程記錄都會更新以反映它已備份，而先前備份的記錄可能會被截斷。 如果未使用此參數，則**wbadmin start backup**會進行複本備份，但不會更新所備份之檔案的歷程記錄。</br>注意：如果您使用 Windows Server Backup 以外的產品來備份目前備份所包含之磁片區上的應用程式，請勿使用此參數。 這麼做可能會中斷其他備份產品所建立的增量、差異或其他類型的備份，因為它們所依賴的歷程記錄會決定要備份的資料量可能會遺失，而且可能會執行完整備份。無謂.|
|-vssCopy|對於 Windows 7 和 Windows Server 2008 R2 和更新版本，會使用 VSS 執行複本備份。 所有檔案都會進行備份，但是要備份的檔案歷程記錄並不會更新，因此您可以保留變更、刪除等檔案的所有資訊，以及任何應用程式記錄檔。 使用這種類型的備份並不會影響與此複本備份無關的增量和差異備份順序。 這是預設值。</br>警告：複本備份無法用於增量或差異備份或還原。|
|-quiet|執行子命令，而不提示使用者。|

## <a name="BKMK_examples"></a>典型

下列範例會示範如何在不同的備份案例中使用**wbadmin start backup**命令：

案例 #1
- 建立磁片區 e：、d：\\掛接點和 \\\\的備份？\\磁片區 {cc566d14-4410-11d9-9d93-806e6f6e6963}
- 將備份儲存至磁片區 f：
  ```
  wbadmin start backup -backupTarget:f: -include:e:,d:\mountpoint,\\?\Volume{cc566d14-44a0-11d9-9d93-806e6f6e6963}\
  ```
  案例 #2
- 執行*f：\\folder1*和*h：\\folder2*到磁片區*d：* 的一次性備份。
- 備份系統狀態
- 製作複本備份，如此一來，一般排定的差異備份就不會受到影響。
  ```
  wbadmin start backup –backupTarget:d: -include:g\folder1,h:\folder2 –systemstate -vsscopy
  ```
  案例 #3
- 執行應該以非遞迴方式備份的*d：\\folder1*的一次性備份。
- 將資料夾備份至網路位置 *\\\\backupshare\\備份 1*
- 將備份的存取許可權制為**Administrators**或**backup Operators**群組的成員。
  ```
  wbadmin start backup –backupTarget: \\backupshare\backup1 -noinheritacl -nonrecurseinclude:d:\folder1
  ```

#### <a name="additional-references"></a>其他參考資料

-   [命令列語法關鍵](command-line-syntax-key.md)
-   [Restore](wbadmin.md)
