---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: "AD FS 快速還原工具"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/20/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cd1cc8dab07288ba73c507bc551f089bb79502bc
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2018
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速還原工具

>適用於：Windows Server 2016、Windows Server 2012 R2

## <a name="overview"></a>概觀
今天 AD FS 可高度使用 AD FS 農場上的設定。 某些組織想要的方式有單一伺服器 AD FS 部署，不需要為多個 AD FS 伺服器與網路負載平衡基礎結構，時仍有一些保證服務的可還原快速是否有問題。
AD FS 快速還原的新工具提供還原 AD FS 資料，而不需完整備份與還原作業系統或系統狀態的方式。 若要匯出 AD FS 設定 Azure 或場所上的位置，您可以使用新的工具。  然後您可以 AD FS 全新安裝，以套用匯出的資料重新建立或複製 AD FS 環境。 

## <a name="scenarios"></a>案例
AD FS 快速還原工具可用於下列案例：

1. 快速之後問題還原 AD FS 功能
    - 使用工具來建立冰凍的 AD FS 可快速部署來取代 online AD FS 伺服器待命安裝
2. 部署相同測試和實際環境
    - 使用工具快速建立 production AD FS 正確複本，在測試環境中，或快速正式部署驗證的測試設定

## <a name="what-is-backed-up"></a>備份是項目
此工具備份 AD FS 下列設定
    
- AD FS 設定資料庫 （SQL 或 WID）
- 設定檔 （位於 AD FS 資料夾）
- 自動產生權杖登入和解密憑證和私密金鑰 （從 Active Directory DKM 容器）
- SSL 憑證和任何外部退出憑證 （權杖登入，權杖解密和服務通訊） 和對應私密金鑰 (請注意： 私密金鑰必須匯出和執行指令碼的使用者必須其存取權限)
- 自訂驗證提供者、 屬性商店與本機宣告提供者的清單信任的安裝。

## <a name="how-to-use-the-tool"></a>如何使用的工具
第一次，[下載](https://go.microsoft.com/fwlink/?LinkId=825646)並 MSI 安裝程式 AD FS 伺服器。  

>[!NOTE]
>AD FS 快速還原工具不會 FIPS 相容。

從 PowerShell 命令提示字元中執行下列命令：

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果您使用 Windows 整合資料庫 (WID)，則需要主要 AD FS 伺服器上執行此工具。  您可以使用`Get-SyncProperties`PowerShell cmdlet 來判斷是否伺服器上的主要伺服器。

### <a name="system-requirements"></a>系統需求

- 這個工具適用於在 Windows Server 2012 R2 及更新版本 AD FS。 
- 所需的.NET framework 為至少 4.0。 
- 還原必須完成 AD FS 相同版本做為備份的伺服器上為 AD FS 服務 account 使用相同的 Active Directory 帳號，

## <a name="create-a-backup"></a>建立備份
若要建立備份，請使用備份-ADFS cmdlet。 這個 cmdlet 備份 AD FS 設定資料庫、 SSL 憑證、 等等。 

使用者必須執行這個 cmdlet 至少本機系統管理員。 若要備份 Active Directory DKM 容器 （預設 AD FS 設定必要），使用者會網域系統管理員也已或需要傳遞 AD FS 服務 account 認證中。

將模式 」 adfsBackup_ID_Date 時間 「 根據命名為備份。 它將會包含的版本號碼、 日期和時間，已完成備份。
下列參數 cmdlet:
    
參數集

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>詳細的描述

- **BackupDKM** -備份 Active Directory DKM 容器包含 AD FS 按鍵 （登入和解密憑證自動預付碼） 預設設定。 這會使用 AD 工具 'ldifde' 匯出 AD 容器和所有其子樹。

- -**StorageType&lt;字串&gt;** -使用者想要使用的儲存空間的類型。 「 檔案系統 」 表示使用者想要將它儲存在本機的資料夾或網路 」 Azure 」 表示使用者想要儲存在 Azure 儲存容器使用者執行備份時，它們選取備份的檔案系統位置，或在雲端中。 使用 azure，Azure 儲存認證應傳遞給 cmdlet。 儲存認證包含帳號和金鑰。 除了，容器名稱必須傳遞中。 如果容器不存在，它會建立備份時。 使用檔案系統，必須授與的儲存空間路徑。 在該 directory，將會建立新的 directory 每個備份。 建立的每個 directory 會包含備份的檔案。 

- **EncryptionPassword&lt;字串&gt;** -將用於備份的所有檔案都加密之前將它儲存的密碼

- **AzureConnectionCredentials &lt;pscredential&gt; ** -account 名稱及 Azure 儲存 account 鍵

- **AzureStorageContainer&lt;字串&gt;**的備份儲存在 Azure 儲存容器

- **StoragePath&lt;字串&gt;** -位置的備份將會儲存在

- **ServiceAccountCredential &lt;pscredential&gt; ** -指定目前執行的 AD FS 服務所用服務 account。 若使用者想要備份 DKM 只需要此參數，並不網域系統管理員類型。

- **BackupComment&lt;字串 []&gt; ** -的備份還原，類似於 HYPER-V 檢查點命名的概念期間會顯示的相關資訊字串。 預設值是空字串

 
## <a name="backup-examples"></a>備份範例
備份使用 AD FS 快速還原工具的範例如下。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-while-running-as-the-domain-admin"></a>AD FS 設定，DKM，檔案系統，以網域系統管理員身分執行時使用的備份

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>DKM，使用本機系統管理員身分執行的服務 account 認證系統檔案的備份 AD FS 設定

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>備份不 Azure 儲存容器 DKM AD FS 設定。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>備份不到檔案系統 DKM AD FS 設定

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>從備份還原
若要套用的設定建立備份-ADFS 使用 AD FS 全新安裝，請使用還原-ADFS cmdlet。

這個 cmdlet 建立新的 AD FS 發電廠使用 cmdlet `Install-AdfsFarm` ，並還原 AD FS 設定資料庫、 憑證、 等等。如果尚未在伺服器上安裝 AD FS 的角色，cmdlet 將會安裝它。  Cmdlet 檢查現有的備份還原的位置，並提示使用者選擇根據拍攝的日期/時間和使用者可能會有連接到備份任何備份意見適當的備份。 如果有多個 AD FS 設定的不同同盟服務的名稱，使用者會提示第一次選擇的適當 AD FS 設定。
使用者必須執行這個 cmdlet 網域和本機系統管理員。


>[!NOTE] 
>之前，請使用 AD FS 快速修復工具，請先確認伺服器所加入的網域之前還原備份。 

下列參數 cmdlet: 

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>詳細的描述

- **StorageType&lt;字串&gt;** -使用者想要使用的儲存空間的類型。
 「 檔案系統 」 表示使用者想要將它儲存在本機的資料夾，或在網路 」 Azure 」 表示使用者想要將它儲存在 Azure 儲存容器

- **DecryptionPassword&lt;字串&gt;**的密碼，用於備份的所有檔案都加密 

- **AzureConnectionCredentials &lt;pscredential&gt; ** -account 名稱及 Azure 儲存 account 鍵

- **AzureStorageContainer&lt;字串&gt;**的備份儲存在 Azure 儲存容器

- **StoragePath&lt;字串&gt;** -位置的備份將會儲存在

- **ADFSName&lt;字串&gt; ** -聯盟備份並前往還原的名稱。 如果這不提供，而且有只有一個同盟服務的名稱，然後將會使用。 如果有多個同盟服務備份到該位置，然後選擇備份同盟服務的其中一個提示使用者。

- **ServiceAccountCredential &lt; pscredential &gt; ** -指定服務帳號，用於新 AD FS 服務進行還原 

- **GroupServiceAccountIdentifier&lt;字串&gt;** -GMSA 使用者想要還原新 AD FS 服務使用。 根據預設，如果都不提供然後備份上 account 名稱使用是否 GMSA，其他使用者會提示要放在 [服務 account

- **DBConnectionString&lt;字串&gt;**若使用者想要使用不同的 DB 還原，然後他們應該 SQL 連接字串或類型 WID 為傳入 WID.-

- **推動&lt;bool&gt; ** -略過在選擇備份時，可能會有工具提示

- **RestoreDKM &lt;bool&gt; ** -還原 DKM 容器 ad、 移到新的廣告應該設定和 DKM 備份一開始。

## <a name="restore-examples"></a>還原範例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>從 Azure 儲存容器還原 AD FS 不 DKM 設定

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>從系統檔案還原 AD FS 不 DKM 設定
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>系統檔案還原 DKM 使用 AD FS 設定 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>AD FS 設定還原至 WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>還原 sql AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>還原指定 GMSA 使用 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>還原與指定的服務 account 認證 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密資訊
推送至雲端，或將它儲存檔案系統中之前加密備份的所有資料。  

每份文件所建立的備份一部分使用好一段-256 加密。 傳送到此工具的密碼 pass 句子用於產生 Rfc2898DeriveBytes 課程的新密碼。 

RngCryptoServiceProvider 用於產生鹽好一段和 Rfc2898DeriveBytes 課程使用。 

## <a name="log-files"></a>登入的檔案
每次執行時的備份或還原登入會建立檔案。 您可以在下列位置找到這些：

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 當執行 PostRestore_Instructions 檔案可能包含的額外的驗證者概觀建立的還原，屬性儲存和本機宣告提供者信任開始 AD FS 服務之前手動安裝。
