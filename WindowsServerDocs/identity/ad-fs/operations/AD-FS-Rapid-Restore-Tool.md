---
ms.assetid: 4deff06a-d0ef-4e5a-9701-5911ba667201
title: AD FS 快速還原工具
author: billmath
ms.author: billmath
manager: femila
ms.date: 04/24/2019
ms.topic: article
ms.openlocfilehash: cb95a3eb85132ee5d5f3b40536364ebd0f58b40a
ms.sourcegitcommit: 3181fcb69a368f38e0d66002e8bc6fd9628b1acc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96330460"
---
# <a name="ad-fs-rapid-restore-tool"></a>AD FS 快速還原工具

## <a name="overview"></a>概觀

現今 AD FS 是藉由設定 AD FS 伺服器陣列來提供高可用性。 某些組織希望有一種方式來 AD FS 部署單一伺服器，而不需要多部 AD FS 伺服器和網路負載平衡基礎結構，同時仍有一些保證可在發生問題時快速還原服務。
全新的 AD FS 快速還原工具提供了一種方法，可讓您在不需要完整備份和還原作業系統或系統狀態的情況下，還原 AD FS 的資料。 您可以使用新的工具將 AD FS 設定匯出至 Azure 或內部部署位置。  然後，您可以將匯出的資料套用至全新的 AD FS 安裝，然後重新建立或複製 AD FS 環境。

## <a name="scenarios"></a>案例

您可以在下列案例中使用 AD FS 快速還原工具：

1. 在問題發生之後快速還原 AD FS 功能
    - 您可以使用此工具來建立 AD FS 的冷待命安裝，以快速部署以取代線上 AD FS 伺服器
2. 部署相同的測試和生產環境
    - 您可以使用此工具，在測試環境中快速建立生產 AD FS 的精確複本，或快速地將經過驗證的測試設定部署到生產環境
3. 從以 SQL 為基礎的設定遷移至 WID，反之亦然
    - 使用工具從 SQL 型伺服器陣列設定移至 WID，反之亦然。


> [!NOTE]
> 如果您使用 SQL 合併式複寫或 Always on 可用性群組，則不支援「快速還原」工具。 建議您以 SQL 為基礎的備份和 SSL 憑證的備份作為替代方案。

## <a name="what-is-backed-up"></a>備份的內容

此工具會備份下列 AD FS 設定

- AD FS 設定資料庫 (SQL 或 WID) 
- 設定檔 (位於 AD FS 資料夾) 
- 自動產生的權杖簽署和解密憑證和私密金鑰 (從 Active Directory DKM 容器) 
- SSL 憑證和任何外部註冊的憑證 (權杖簽署、權杖解密和服務通訊) 和對應的私密金鑰 (注意：私密金鑰必須是可匯出的，且執行腳本的使用者必須擁有存取這些憑證的許可權) 
- 已安裝的自訂驗證提供者、屬性存放區和本機宣告提供者信任清單。

## <a name="how-to-use-the-tool"></a>如何使用工具

首先，將 MSI [下載](https://go.microsoft.com/fwlink/?LinkId=825646) 並安裝到您的 AD FS 伺服器。

從 PowerShell 提示字元執行下列命令：

```powershell
import-module 'C:\Program Files (x86)\ADFS Rapid Recreation Tool\ADFSRapidRecreationTool.dll'
```

> [!NOTE]
> 如果您使用 Windows 整合式資料庫 (WID) ，則必須在主要 AD FS 伺服器上執行此工具。  您可以使用 `Get-AdfsSyncProperties` PowerShell Cmdlet 來判斷您所在的伺服器是否為主伺服器。

### <a name="system-requirements"></a>系統需求

- 這項工具適用于 Windows Server 2012 R2 和更新版本中的 AD FS。
- 必要的 .NET framework 至少為4.0。
- 還原必須在與備份相同版本的 AD FS 伺服器上完成，且使用與 AD FS 服務帳戶相同的 Active Directory 帳戶。

## <a name="create-a-backup"></a>建立備份

若要建立備份，請使用備份-ADFS Cmdlet。 此 Cmdlet 會備份 AD FS 設定、資料庫、SSL 憑證等。

使用者至少必須是本機系統管理員，才能執行此 Cmdlet。
若要備份預設 AD FS 設定) 所需的 Active Directory DKM 容器 (，使用者必須是網域系統管理員，必須傳入 AD FS 服務帳戶認證，或具有 DKM 容器的存取權。  如果您使用 gMSA 帳戶，則使用者必須是網域系統管理員，或擁有容器的許可權;您無法提供 gMSA 認證。

備份會根據 "adfsBackup_ID_Date Time" 模式命名。 它會包含備份完成的版本號碼、日期和時間。
此 Cmdlet 會使用下列參數：

參數集

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter1.png)

### <a name="detailed-description"></a>詳細描述

- **BackupDKM** -備份 Active Directory DKM 容器，其中包含預設設定中的 AD FS 金鑰 (自動產生的權杖簽署和解密憑證) 。 這會使用 AD 工具「ldifde」來匯出 AD 容器及其所有子樹。

- -**StorageType &lt; 字串 &gt;** -使用者想要使用的儲存體類型。
「檔案系統」表示使用者想要將它儲存在本機的資料夾中，或在「Azure」網路中，表示使用者想要在使用者執行備份時，將它儲存在 Azure 儲存體容器中，他們會選取備份位置，也就是檔案系統或雲端。
針對要使用的 Azure，Azure 儲存體認證應傳遞給 Cmdlet。 儲存體認證包含帳戶名稱和金鑰。 此外，也必須傳入容器名稱。 如果容器不存在，則會在備份期間建立。
針對要使用的檔案系統，必須提供儲存路徑。 在該目錄中，將會針對每個備份建立新的目錄。 所建立的每個目錄都會包含已備份的檔案。

- **EncryptionPassword &lt; 字串 &gt;** -儲存之前用來加密所有已備份檔案的密碼

- **AzureConnectionCredentials &lt; Pscredential &gt;** -Azure 儲存體帳戶的帳戶名稱和金鑰

- **>new-azurestoragecontainer &lt; 字串 &gt;** -備份將儲存在 Azure 中的儲存體容器

- **StoragePath &lt; 字串 &gt;** -將儲存備份的位置

- **ServiceAccountCredential &lt; pscredential &gt;** -指定目前正在執行之 AD FS 服務所使用的服務帳戶。 只有當使用者想要備份 DKM 且不是網域系統管理員，或無法存取容器的內容時，才需要此參數。

- **BackupComment &lt; string [] &gt;** -將在還原期間顯示之備份的相關資訊字串，類似于 hyper-v 檢查點命名的概念。 預設值為空字串


## <a name="backup-examples"></a>備份範例

以下是使用 AD FS 快速還原工具的備份範例。

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-and-has-access-to-the-dkm-container-contents-either-domain-admin-or-delegated"></a>使用 DKM 將 AD FS 設定備份到檔案系統，並可存取可 (網域系統管理員或委派的 DKM 容器內容) 

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM
```

### <a name="backup-the-ad-fs-configuration-with-the-dkm-to-the-file-system-with-the-service-account-credential-running-as-local-admin"></a>將具有 DKM 的 AD FS 設定備份到具有服務帳戶認證的檔案系統（以本機系統管理員身分執行）

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)" -BackupDKM -ServiceAccountCredential $cred
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-azure-storage-container"></a>將不具 DKM 的 AD FS 設定備份至 Azure 儲存體容器。

```powershell
Backup-ADFS -StorageType "Azure" -AzureConnectionCredentials $cred -AzureStorageContainer "adfsbackups"  -EncryptionPassword "password" -BackupComment "Clean Install of ADFS"
```

### <a name="backup-the-ad-fs-configuration-without-the-dkm-to-the-file-system"></a>備份不具 DKM 的 AD FS 設定至檔案系統

```powershell
Backup-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -EncryptionPassword "password" -BackupComment "Clean Install of ADFS (FS)"
```

## <a name="restore-from-backup"></a>從備份還原

若要將使用備份建立的設定套用至新的 AD FS 安裝，請使用 Restore-ADFS Cmdlet。

此 Cmdlet 會使用 Cmdlet 建立新的 AD FS 伺服器陣列， `Install-AdfsFarm` 並還原 AD FS 的設定、資料庫、憑證等。 如果伺服器上尚未安裝 AD FS 的角色，Cmdlet 將會安裝該角色。  指令程式會檢查現有備份的還原位置，並提示使用者根據所取得的日期/時間，以及使用者可能已附加至備份的任何備份批註，來選擇適當的備份。 如果有多個 AD FS 設定具有不同的 federation service 名稱，則系統會提示使用者先選擇適當的 AD FS 設定。
使用者必須是本機和網域系統管理員，才能執行此 Cmdlet。


> [!NOTE]
> 使用 AD FS 快速修復工具之前，請先確定伺服器已加入網域，再還原備份。

此 Cmdlet 會使用下列參數：

![AD FS 快速還原工具](media/AD-FS-Rapid-Restore-Tool/parameter2.png)

### <a name="detailed-description"></a>詳細描述

- **StorageType &lt; 字串 &gt;** -使用者想要使用的儲存體類型。
 「檔案系統」表示使用者想要將它儲存在本機的資料夾中，或儲存在網路 "Azure" 中，表示使用者想要將它儲存在 Azure 儲存體容器中

- **DecryptionPassword &lt; 字串 &gt;** -用來加密所有已備份檔案的密碼

- **AzureConnectionCredentials &lt; Pscredential &gt;** -Azure 儲存體帳戶的帳戶名稱和金鑰

- **>new-azurestoragecontainer &lt; 字串 &gt;** -備份將儲存在 Azure 中的儲存體容器

- **StoragePath &lt; 字串 &gt;** -將儲存備份的位置

- **ADFSName &lt; 字串 &gt;** -已備份且即將還原的同盟名稱。 如果未提供此項，而且只有一個 federation service 名稱，則會使用該名稱。 如果有多個同盟服務備份到該位置，則會提示使用者選擇其中一個已備份的同盟服務。

- **ServiceAccountCredential &lt; pscredential &gt;** -指定將用於要還原之新 AD FS 服務的服務帳戶

- **GroupServiceAccountIdentifier &lt; 字串 &gt;** -使用者想要用於要還原之新 AD FS 服務的 GMSA。 依預設，如果未提供，則會使用備份的帳戶名稱（如果已 GMSA），否則系統會提示使用者輸入服務帳戶

- **DBConnectionString &lt; 字串 &gt;** -如果使用者想要使用不同的資料庫進行還原，則應該針對 wid 在 Wid 中傳遞 SQL 連接字串或類型。

- **強制 &lt; bool &gt;** -略過工具在選擇備份之後可能會有的提示

- **RestoreDKM &lt; bool &gt;** -將 DKM 容器還原至 AD，則應設定為如果要進行新的 ad，並一開始就備份 dkm。

## <a name="restore-examples"></a>還原範例

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-azure-storage-container"></a>從 Azure 儲存體容器還原不具 DKM 的 AD FS 設定

```powershell
Restore-ADFS -StorageType "Azure" -AzureConnectionCredential $cred -DecryptionPassword "password" -AzureStorageContainer "adfsbackups"
```

### <a name="restore-the-ad-fs-configuration-without-the-dkm-from-the-file-system"></a>從檔案系統還原不具 DKM 的 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password"
```

### <a name="restore-the-ad-fs-configuration-with-the-dkm-to-the-file-system"></a>將具有 DKM 的 AD FS 設定還原至檔案系統

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -RestoreDKM
```

### <a name="restore-the-ad-fs-configuration-to-wid"></a>將 AD FS 設定還原至 WID

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "WID"
```

### <a name="restore-the-ad-fs-configuration-to-sql"></a>將 AD FS 設定還原至 SQL

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -DBConnectionString "Data Source=TESTMACHINE\SQLEXPRESS; Integrated Security=True"
```

### <a name="restores-the-ad-fs-configuration-with-the-specified-gmsa"></a>使用指定的 GMSA 還原 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -GroupServiceAccountIdentifier "mangupd1\adfsgmsa$"
```
### <a name="restore-the-ad-fs-configuration-with-the-specified-service-account-creds"></a>使用指定的服務帳戶認證來還原 AD FS 設定

```powershell
Restore-ADFS -StorageType "FileSystem" -StoragePath "C:\Users\administrator\testExport\" -DecryptionPassword "password" -ServiceAccountCredential $cred
```

## <a name="encryption-information"></a>加密資訊

所有備份資料都會先經過加密，再將它推送至雲端，或將它儲存在檔案系統中。

在備份過程中建立的每份檔都會使用 AES-256 進行加密。 傳遞至工具的密碼可作為使用 Rfc2898DeriveBytes 類別來產生新密碼的傳遞片語。

RngCryptoServiceProvider 用來產生 AES 和 Rfc2898DeriveBytes 類別所使用的 salt。

## <a name="log-files"></a>記錄檔

每次執行備份或還原時，就會建立記錄檔。 您可以在下列位置找到這些內容：

- **%LOCALAPPDATA%\ADFSRapidRecreationTool**

> [!NOTE]
> 執行 PostRestore_Instructions 還原時，可能會建立一個包含其他驗證提供者的總覽、屬性存放區，以及在啟動 AD FS 服務之前手動安裝的本機宣告提供者信任。

## <a name="version-release-history"></a>版本發行歷程記錄

### <a name="version-10823"></a>版本1.0.82。3

版本：2020年4月

**已修正的問題：**

- 已新增以 CNG 為基礎的憑證支援


### <a name="version-10820"></a>版本1.0.82。0

發行：2019年7月

**已修正的問題：**

- 修正包含 LDAP escape 字元之 AD FS 服務帳戶名稱的 Bug


### <a name="version-10810"></a>版本：1.0.81。0

版本：2019年4月

**已修正的問題：**

- 憑證備份與還原的錯誤修正
- 記錄檔的其他追蹤資訊


### <a name="version-10750"></a>版本：1.0.75。0

發行：2018年8月

**已修正的問題：**

* 更新備份-使用-BackupDKM 參數時的 ADFS。  此工具將會判斷目前的內容是否具有 DKM 容器的存取權。  如果是，則不需要網域系統管理員許可權或服務帳戶認證。  這可讓您在未明確提供認證或以網域系統管理員帳戶身分執行的情況下，進行自動備份。

### <a name="version-10730"></a>版本：1.0.73。0

發行：2018年8月

**已修正的問題：**

* 更新加密演算法，讓應用程式符合 FIPS 規範

    > [!NOTE]
    > 舊的備份將無法使用新的版本，因為加密演算法會根據 FIPS 合規性進行變更

* 新增使用合併式複寫之 SQL 叢集的支援

### <a name="version-10720"></a>版本：1.0.72。0

發行：2018年7月

**已修正的問題：**

- Bug 修正：已修正。支援就地升級的 MSI 安裝程式

### <a name="10180"></a>1.0.18.0

發行：2018年7月

**已修正的問題：**

- Bug 修正：處理有特殊字元的服務帳戶密碼 (ie，' & ' ) 
- Bug 修正：還原失敗，因為另一個進程正在使用 Microsoft.IdentityServer.Servicehost.exe.config


### <a name="1000"></a>1.0.0.0

發行日期：2016年10月

AD FS 快速還原工具的初始版本
