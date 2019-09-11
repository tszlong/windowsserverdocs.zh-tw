---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: AD FS 快速還原工具
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/02/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fc924f5e5bdd7dabecac4fdd6805ad261a0fc634
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866173"
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速還原工具

## <a name="overview"></a>總覽
現在 AD FS 藉由設定 AD FS 伺服器陣列, 使其成為高可用性。 有些組織想要有一種方式來 AD FS 部署單一伺服器, 而不需要多部 AD FS 伺服器和網路負載平衡基礎結構, 同時仍有一些保證可以在發生問題時快速還原服務。
新的 AD FS 快速還原工具可讓您還原 AD FS 資料, 而不需要完整備份和還原作業系統或系統狀態。 您可以使用新工具, 將 AD FS 設定匯出至 Azure 或內部部署位置。  接著, 您可以將匯出的資料套用至全新的 AD FS 安裝, 重新建立或複製 AD FS 環境。 

## <a name="scenarios"></a>案例
在下列案例中, 可以使用 AD FS 快速還原工具:

1. 在問題發生後快速還原 AD FS 功能
    - 使用此工具建立 AD FS 的冷待命安裝, 可以快速部署來取代線上 AD FS 伺服器
2. 部署相同的測試和生產環境
    - 使用工具, 在測試環境中快速建立生產 AD FS 的精確複本, 或將已驗證的測試設定快速部署到生產環境

>[!NOTE] 
>如果您使用 SQL 合併式複寫或 Always on 可用性群組, 則不支援快速還原工具。 我們建議使用以 SQL 為基礎的備份, 以及 SSL 憑證的備份做為替代方案。

## <a name="what-is-backed-up"></a>備份的內容
此工具會備份下列 AD FS 設定
    
- AD FS 設定資料庫 (SQL 或 WID)
- 設定檔 (位於 AD FS 資料夾)
- 自動產生的權杖簽署和解密憑證和私密金鑰 (從 Active Directory 的 DKM 容器)
- SSL 憑證和任何外部註冊的憑證 (權杖簽署、權杖解密和服務通訊) 及對應的私密金鑰 (注意: 私密金鑰必須可匯出, 而且執行腳本的使用者必須具有存取這些金鑰的許可權)
- 已安裝的自訂驗證提供者、屬性存放區和本機宣告提供者信任的清單。

## <a name="how-to-use-the-tool"></a>如何使用工具
首先, 將 MSI[下載](https://go.microsoft.com/fwlink/?LinkId=825646)並安裝到您的 AD FS 伺服器。  

從 PowerShell 提示字元執行下列命令:

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果您使用的是 Windows 整合式資料庫 (WID), 則此工具必須在主要 AD FS 伺服器上執行。  您可以使用`Get-AdfsSyncProperties` PowerShell Cmdlet 來判斷您所在的伺服器是否為主伺服器。

### <a name="system-requirements"></a>系統需求

- 此工具適用于 Windows Server 2012 R2 和更新版本中的 AD FS。 
- 所需的 .NET framework 至少為4.0。 
- 還原必須在與備份相同版本的 AD FS 伺服器上執行, 而且與 AD FS 服務帳戶使用相同的 Active Directory 帳戶。

## <a name="create-a-backup"></a>建立備份
若要建立備份, 請使用 Backup-ADFS Cmdlet。 此 Cmdlet 會備份 AD FS 設定、資料庫、SSL 憑證等。 

使用者至少必須是本機系統管理員, 才能執行此 Cmdlet。 若要備份 Active Directory DKM 容器 (預設 AD FS 設定中的必要項), 使用者必須是網域系統管理員、必須傳入 AD FS 服務帳戶認證, 或具有 DKM 容器的存取權。  如果您使用 gMSA 帳戶, 則使用者必須是網域系統管理員或具有容器的許可權;您無法提供 gMSA 認證。 

備份將根據 "adfsBackup_ID_Date Time" 模式命名。 它會包含備份執行的版本號碼、日期和時間。
此 Cmdlet 會採用下列參數:
    
參數集

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>詳細描述

- **BackupDKM** -備份 Active Directory DKM 容器, 其中包含預設設定中的 AD FS 金鑰 (自動產生的權杖簽署和解密憑證)。 這會使用 AD 工具 ' ldifde ' 來匯出 AD 容器和其所有的子樹。

- -**StorageType&lt;字串&gt;** -使用者想要使用的儲存體類型。 "FileSystem" 表示使用者想要將它儲存在本機資料夾或網路 "Azure" 中, 表示使用者想要在使用者執行備份時, 將它儲存在 Azure 儲存體容器中, 他們會選取備份位置, 也就是檔案系統或形成. 若要使用 Azure, 應將 Azure 儲存體認證傳遞至 Cmdlet。 儲存體認證包含帳戶名稱和金鑰。 此外, 也必須傳入容器名稱。 如果容器不存在，則會在備份期間建立。 對於要使用的檔案系統, 必須指定儲存路徑。 在該目錄中, 將會為每個備份建立一個新的目錄。 所建立的每個目錄都會包含備份檔案。 

- **EncryptionPassword&lt;字串&gt;** -將用來加密所有備份檔案的密碼, 然後再儲存

- **AzureConnectionCredentials&lt; pscredential&gt;** -Azure 儲存體帳戶的帳戶名稱和金鑰

- **New-azurestoragecontainer&lt;字串&gt;** -備份將儲存在 Azure 中的儲存體容器

- **StoragePath&lt;字串&gt;** -將儲存備份的位置

- **ServiceAccountCredential&lt; pscredential&gt;** -指定目前正在執行的 AD FS 服務所使用的服務帳戶。 只有當使用者想要備份 DKM, 而不是網域系統管理員或無法存取容器的內容時, 才需要此參數。 

- **BackupComment&lt;string []&gt;** -有關將在還原期間顯示之備份的資訊字串，類似于 hyper-v 檢查點命名的概念。 預設值為空字串

 
## <a name="backup-examples"></a>備份範例
以下是使用 AD FS 快速還原工具的備份範例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>將 AD FS 設定 (具有 DKM) 備份至檔案系統, 並可存取 DKM 容器內容 (網域管理員或委派)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>將具有 DKM 的 AD FS 設定備份至具有服務帳戶認證的檔案系統, 以本機系統管理員身分執行

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>將不具 DKM 的 AD FS 設定備份至 Azure 儲存體容器。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>將不具 DKM 的 AD FS 設定備份至檔案系統

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>從備份還原
若要將使用備份-ADFS 建立的設定套用至新的 AD FS 安裝, 請使用 Restore-ADFS Cmdlet。

此 Cmdlet 會使用 Cmdlet `Install-AdfsFarm`建立新的 AD FS 伺服器陣列, 並還原 AD FS 設定、資料庫、憑證等。如果尚未在伺服器上安裝 AD FS 角色, 此 Cmdlet 將會安裝它。  指令程式會檢查現有備份的還原位置, 並提示使用者根據所取得的日期/時間, 以及使用者可能附加至備份的任何備份批註, 來選擇適當的備份。 如果有多個 AD FS 設定具有不同的同盟服務名稱, 則系統會提示使用者先選擇適當的 AD FS 設定。
使用者必須同時是本機和網域系統管理員, 才能執行此 Cmdlet。


>[!NOTE] 
>使用 AD FS 快速復原工具之前, 請先確定伺服器已加入網域, 然後再還原備份。 

此 Cmdlet 會採用下列參數: 

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>詳細描述

- **StorageType&lt;字串&gt;** -使用者想要使用的儲存體類型。
 "FileSystem" 表示使用者想要將它儲存在本機資料夾或網路 "Azure" 中, 表示使用者想要將它儲存在 Azure 儲存體容器中

- **DecryptionPassword&lt;字串&gt;** -用來加密所有備份檔案的密碼 

- **AzureConnectionCredentials&lt; pscredential&gt;** -Azure 儲存體帳戶的帳戶名稱和金鑰

- **New-azurestoragecontainer&lt;字串&gt;** -備份將儲存在 Azure 中的儲存體容器

- **StoragePath&lt;字串&gt;** -將儲存備份的位置

- **ADFSName&lt; string&gt;** -已備份且即將還原之同盟的名稱。 如果未提供此項, 而且只有一個 federation service 名稱, 則會使用。 如果有一個以上的同盟服務備份至該位置, 則會提示使用者選擇其中一個已備份的同盟服務。

- **ServiceAccountCredential&lt; pscredential&gt;** -指定將用於要還原之新 AD FS 服務的服務帳戶 

- **GroupServiceAccountIdentifier&lt;字串&gt;** -使用者想要用於還原之新 AD FS 服務的 GMSA。 根據預設, 如果未提供任何值, 則會使用已備份的帳戶名稱 (如果已 GMSA), 否則系統會提示使用者將其放入服務帳戶

- **DBConnectionString&lt;字串&gt;** -如果使用者想要使用不同的 DB 進行還原, 他們應該在 wid 中傳遞 SQL 連接字串或類型。

- **強制&lt;使用bool&gt;**  -略過工具在選擇備份之後可能會有的提示

- **RestoreDKM&lt; bool&gt;** -將 DKM 容器還原至 AD, 應設定為如果要新增至新的 ad, 並在一開始備份 dkm。

## <a name="restore-examples"></a>還原範例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>還原 Azure 儲存體容器中沒有 DKM 的 AD FS 設定

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>從檔案系統還原不具 DKM 的 AD FS 設定
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>將具有 DKM 的 AD FS 設定還原至檔案系統 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>將 AD FS 設定還原至 WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>將 AD FS 設定還原至 SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>使用指定的 GMSA 還原 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>使用指定的服務帳戶認證還原 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密資訊
所有備份資料會先加密, 然後才推送至雲端或儲存在檔案系統中。  

在備份過程中建立的每份檔都會使用 AES-256 進行加密。 傳入此工具的密碼是用來做為使用 Rfc2898DeriveBytes 類別來產生新密碼的片語。 

RngCryptoServiceProvider 是用來產生 AES 和 Rfc2898DeriveBytes 類別所使用的 salt。 

## <a name="log-files"></a>記錄檔
每次執行備份或還原時, 就會建立記錄檔。 您可以在下列位置找到這些內容:

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 執行還原時，可能會建立 PostRestore_Instructions 檔案，其中包含其他驗證提供者的總覽、屬性存放區和本機宣告提供者信任，以便在啟動 AD FS 服務之前手動安裝。

## <a name="version-release-history"></a>版本發行歷程記錄

### <a name="version-10820"></a>版本1.0.82。0
版本2019年7月

**已修正的問題:**
- 修正包含 LDAP 逸出字元 AD FS 服務帳戶名稱的 Bug


### <a name="version-10810"></a>版本：1.0.81.0
版本2019 年 4 月

**已修正的問題:**


- 憑證備份和還原的錯誤修正
- 記錄檔的其他追蹤資訊


### <a name="version-10750"></a>版本：1.0.75.0
版本2018 年 8 月

**已修正的問題:**
* 更新備份-使用-BackupDKM 參數時的 ADFS。  此工具會判斷目前的內容是否有可存取 DKM 容器的許可權。  若是如此, 則不需要網域系統管理員許可權或服務帳戶認證。  這可讓自動備份進行, 而不需明確提供認證或以網域系統管理員帳戶的身分執行。

### <a name="version-10730"></a>版本：1.0.73.0
版本2018 年 8 月

**已修正的問題:**
* 更新加密演算法, 讓應用程式符合 FIPS 規範
    
    >[!NOTE]
    > 舊的備份將無法使用新版本, 因為加密演算法的變更是根據 FIPS 合規性
    
* 新增使用合併式複寫之 SQL 叢集的支援

### <a name="version-10720"></a>版本：1.0.72.0
版本2018 年 7 月

**已修正的問題:**

   - Bug 修正:已修正。支援就地升級的 MSI 安裝程式 

### <a name="10180"></a>1.0.18.0
版本2018 年 7 月

**已修正的問題:**

   - Bug 修正：處理在其中有特殊字元的服務帳戶密碼（即 ' & '）
   - Bug 修正: 還原失敗, 因為另一個進程正在使用 IdentityServer. .exe .config


### <a name="1000"></a>1.0.0.0
達到2016 年 10 月

AD FS 快速還原工具的初始版本
