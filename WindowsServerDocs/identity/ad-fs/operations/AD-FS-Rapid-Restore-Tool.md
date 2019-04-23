---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: AD FS 快速還原工具
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/19/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6fb023529ac8857f7c2eb35586be497f0c809a51
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59874169"
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速還原工具

>適用於：Windows Server 2016, Windows Server 2012 R2

## <a name="overview"></a>總覽
現在 AD FS 會變成高可用性藉由設定 AD FS 伺服器陣列。 有些組織想要在單一伺服器 AD FS 部署的方法，而不需要多個 AD FS 伺服器和網路負載平衡的基礎結構，同時仍保有一些保證服務很快就可還原是否有問題。
新的 AD FS 快速還原工具可用來還原 AD FS 資料，而不需要完整備份和還原作業系統或系統狀態。 您可以使用新的工具，將 AD FS 組態匯出至 Azure 或內部部署位置。  然後您可以匯出的資料套用至最新的 AD FS 安裝，，重新建立或複製 AD FS 環境。 

## <a name="scenarios"></a>案例
AD FS 快速還原工具可以用於下列案例：

1. 快速還原發生問題後的 AD FS 功能
    - 使用工具來建立可快速部署來取代在線上的 AD FS 伺服器的 AD FS 的冷待命安裝
2. 部署完全相同的測試和生產環境
    - 使用此工具，快速地在測試環境中，建立生產 AD FS 的正確副本或快速部署到生產環境的 已驗證的測試組態

## <a name="what-is-backed-up"></a>備份的內容
此工具會備份下列的 AD FS 設定
    
- AD FS 設定資料庫 （SQL 或 WID）
- 組態檔 （位於 AD FS 資料夾）
- 自動產生權杖的簽章和解密憑證和私密金鑰 （從 Active Directory DKM 容器）
- SSL 憑證與任何外部註冊的憑證 （權杖簽署、 權杖解密和服務通訊） 和對應的私密金鑰 (注意： 私密金鑰必須可以匯出和執行指令碼的使用者必須具有存取權限)
- 一份自訂驗證提供者、 屬性存放區，以及本機宣告提供者信任的安裝。

## <a name="how-to-use-the-tool"></a>如何使用工具
首先，[下載](https://go.microsoft.com/fwlink/?LinkId=825646)和安裝 MSI，您的 AD FS 伺服器。  

從 PowerShell 提示字元執行下列命令：

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

>[!NOTE] 
>如果您使用 Windows 整合式資料庫 (WID)，此工具必須在主要 AD FS 伺服器上執行。  您可以使用`Get-AdfsSyncProperties`PowerShell cmdlet 來判斷是否在伺服器您位於主要伺服器。

### <a name="system-requirements"></a>系統需求

- 此工具適用於 AD FS，在 Windows Server 2012 R2 和更新版本。 
- 必要的.NET framework 為至少 4.0。 
- 還原必須以相同的版本做為備份的 AD FS 伺服器上，並使用相同的 Active Directory 帳戶做為 AD FS 服務帳戶。

## <a name="create-a-backup"></a>建立備份
若要建立備份時，使用備份 ADFS cmdlet。 此 cmdlet 會將備份的 AD FS 設定、 資料庫、 SSL 憑證，依此類推。 

使用者必須是至少一個要執行這個指令程式的本機系統管理員。 若要備份 Active Directory DKM 容器 （在預設的 AD FS 組態中的必要項），使用者必須是網域系統管理員，必須將 AD FS 服務帳戶認證，或具有 DKM 容器的存取權。  如果您使用 gMSA 帳戶，使用者必須是網域系統管理員，或具有容器; 的權限您無法提供 gMSA 認證。 

備份將會命名為根據的模式為 「 adfsBackup_ID_Date-時間 」。 它會包含的版本號碼、 日期和時間，備份已完成。
此 cmdlet 會使用下列參數：
    
參數集

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>詳細的描述

- **BackupDKM** -備份包含 AD FS 中的索引鍵的預設組態 （自動產生的權杖簽署及解密憑證） 的 Active Directory DKM 容器。 這會使用 AD 工具 'ldifde' 匯出的 AD 容器和其所有子目錄。

- -**StorageType&lt;字串&gt;** -使用者想要使用的儲存體類型。 "FileSystem"表示使用者想要將它儲存在資料夾中，在本機或網路的 「 Azure 」 表示使用者想要將它儲存在 Azure 儲存體容器中，當使用者執行備份，在選取備份位置，也就是檔案系統中或在雲端。 若要使用 azure，Azure 儲存體認證應該傳遞至 cmdlet。 儲存體認證包含帳戶名稱和金鑰。 此外，容器名稱必須傳遞中。 如果容器不存在，它會建立在備份期間。 若要使用檔案系統，必須指定儲存體路徑。 在該目錄中，您會建立新目錄，每個備份。 建立每個目錄將包含備份的檔案。 

- **EncryptionPassword&lt;字串&gt;** -要用來加密備份的所有檔案，再將其儲存的密碼

- **AzureConnectionCredentials &lt;pscredential&gt;**  -帳戶名稱和 Azure 儲存體帳戶金鑰

- **AzureStorageContainer&lt;字串&gt;** -備份儲存在 Azure 中的儲存體容器

- **StoragePath&lt;字串&gt;** -位置的備份會儲存在

- **ServiceAccountCredential &lt;pscredential&gt;**  -指定要用於目前執行的 AD FS 服務的服務帳戶。 此參數只有當使用者想要備份的 DKM，才會需要和不是網域系統管理員或沒有存取容器的內容。 

- **BackupComment &lt;string []&gt;**  -備份將會顯示在還原期間，類似的 HYPER-V 的檢查點命名概念的相關資訊的字串。 預設值為空字串

 
## <a name="backup-examples"></a>備份的範例
以下是使用 AD FS 快速還原工具備份範例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>備份 AD FS 組態中的，使用 DKM，到檔案系統中，且具有 DKM 容器內容的存取權 (可能是網域系統管理員或委派)

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```
 
### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>備份 AD FS 組態，DKM，以本機系統管理員身分執行的服務帳戶認證的檔案系統

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>備份至 Azure 儲存體容器 DKM AD FS 組態。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>備份至檔案系統 DKM AD FS 組態

```powershell   
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>從備份還原
若要套用到新的 AD FS 安裝使用備份 ADFS 所建立的組態，請使用還原 ADFS cmdlet。

此 cmdlet 會建立新的 AD FS 伺服器陣列，使用 cmdlet`Install-AdfsFarm`並還原的 AD FS 設定、 資料庫、 憑證等等。如果尚未在伺服器上安裝 AD FS 角色，此 cmdlet 便會進行安裝。  此 cmdlet 會檢查現有的備份的還原位置，並會提示使用者選擇適當的日期/時間和使用者可能會有附加至備份的任何備份註解為基礎的備份。 如果有不同的同盟服務名稱的多個 AD FS 組態，請先選擇適當的 AD FS 設定提示使用者。
使用者必須是本機和網域管理員，才能執行這個指令程式。


>[!NOTE] 
>使用 AD FS 快速的復原工具之前，請確定伺服器已加入網域，再還原備份。 

此 cmdlet 會使用下列參數： 

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>詳細的描述

- **StorageType&lt;字串&gt;** -使用者想要使用的儲存體類型。
 "FileSystem"表示使用者想要將它儲存在本機資料夾或網路中 [Azure] 表示使用者想要將它儲存在 Azure 儲存體容器

- **DecryptionPassword&lt;字串&gt;** -用來加密備份的所有檔案的密碼 

- **AzureConnectionCredentials &lt;pscredential&gt;**  -帳戶名稱和 Azure 儲存體帳戶金鑰

- **AzureStorageContainer&lt;字串&gt;** -備份儲存在 Azure 中的儲存體容器

- **StoragePath&lt;字串&gt;** -位置的備份會儲存在

- **ADFSName&lt;字串&gt;**  -已備份，而且要還原之同盟的名稱。 如果未提供此，而且沒有只有一部同盟服務名稱，然後將用於。 如果有一個以上的同盟服務備份到位置，然後系統會提示使用者選擇其中一個備份 Federation Services。

- **ServiceAccountCredential &lt; pscredential &gt;**  -指定將用於新的 AD FS 服務正在還原的服務帳戶 

- **GroupServiceAccountIdentifier&lt;字串&gt;** -使用者想要用於新的 AD FS 服務正在還原的 GMSA。 根據預設，如果都不提供然後備份上使用帳戶名稱是如果是的 GMSA，否則系統會提示使用者將放在服務帳戶

- **DBConnectionString&lt;字串&gt;** -如果使用者想要使用不同的資料庫還原，則他們應該類型的 SQL 連接字串中傳遞 WID WID

- **強制&lt;bool&gt;**  -略過選擇備份之後，可能會有工具提示

- **RestoreDKM &lt;bool&gt;**  -DKM 容器還原到 AD，如果將新的 AD 應該設定而 DKM 一開始備份。

## <a name="restore-examples"></a>Restore 範例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>還原的 AD FS 設定，而不需要 DKM，從 Azure 儲存體容器

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>從檔案系統還原的 AD FS 設定，而不需要 DKM
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>還原至檔案系統的 DKM 的 AD FS 組態 
 
```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>還原至 WID 的 AD FS 組態

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
``` 

### <a name="restore-the-ad-fs-configuration-to-sql"></a>將 AD FS 組態還原到 SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>還原 AD FS 設定與指定的 GMSA

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>還原 AD FS 設定與指定的服務帳戶認證

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\uSERS\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密資訊
所有的備份資料會加密之前將它推送至雲端，或將其儲存在檔案系統。  

使用 AES-256 加密的備份過程中建立每個文件。 傳遞至工具的密碼為複雜密碼來產生使用 Rfc2898DeriveBytes 類別的新密碼。 

RngCryptoServiceProvider 用來產生 AES 和 Rfc2898DeriveBytes 類別所使用的 salt。 

## <a name="log-files"></a>記錄檔
每次執行備份或還原時，會建立記錄檔。 這些可以在下列位置找到：

- **%localappdata%\ADFSRapidRecreationTool**

>[!NOTE]
> 當執行的還原，PostRestore_Instructions 檔案可能會建立包含額外的驗證提供者的概觀，為屬性存放區和本機宣告提供者信任來手動安裝，再啟動 AD FS 服務。

## <a name="version-release-history"></a>版本發行歷程記錄

### <a name="version-10750"></a>版本：1.0.75.0
版本：2018 年 8 月

**已修正的問題：**
* 使用-BackupDKM 交換器時，請更新備份 ADFS。  此工具會判斷目前的內容是否具有 DKM 容器的存取權。  如果是的話，它不需要網域系統管理員權限或服務帳戶認證。  這可讓進行而不需要明確提供認證，或以網域系統管理員帳戶身分執行的自動的備份。

### <a name="version-10730"></a>版本：1.0.73.0
版本：2018 年 8 月

**已修正的問題：**
* 更新的加密演算法，使應用程式符合 FIPS 規範
    
    >[!NOTE]
    > 舊的備份無法搭配新的版本，因為變更加密演算法，根據 FIPS 合規性
    
* 新增使用合併式複寫的 SQL 叢集的支援

### <a name="version-10720"></a>版本：1.0.72.0
版本：2018 年 7 月

**已修正的問題：**

   - Bug 修正：已修正。MSI 安裝程式，以支援就地升級 

### <a name="10180"></a>1.0.18.0
版本：2018 年 7 月

**已修正的問題：**

   - Bug 修正： 處理服務帳戶密碼的方式，有特殊字元 (亦即，'&')
   - Bug 修正： 還原會失敗，因為 Microsoft.IdentityServer.Servicehost.exe.config 正由另一個處理序


### <a name="1000"></a>1.0.0.0
發行日期：2016 年 10 月

初始版本的 AD FS 快速還原工具
