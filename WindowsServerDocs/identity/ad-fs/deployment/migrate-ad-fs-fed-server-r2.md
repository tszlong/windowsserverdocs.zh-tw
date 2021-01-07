---
title: 將 AD FS 2.0 同盟伺服器遷移至 Windows Server 2012 R2
description: 瞭解如何匯出和備份 AD FS 設定資料、建立 Windows Server 2012 R2 同盟伺服器陣列，以及將原始設定資料匯入 Windows Server 2012 R2 AD FS 伺服器陣列中。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: 318437fbec8e214c3f21eae54dcf3c9da2b6c04f
ms.sourcegitcommit: 605a9b46b74b2c7a9116e631e902467ea02a6e70
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/07/2021
ms.locfileid: "97965670"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>將 AD FS 2.0 同盟伺服器遷移至 Windows Server 2012 R2 上的 AD FS

若要將屬於單一節點 AD FS 伺服器陣列、WIF 伺服器陣列或 SQL Server 服務器陣列的 AD FS 同盟伺服器遷移至 Windows Server 2012 R2，您必須執行下列工作：

1.  [匯出和備份 AD FS 設定資料](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)

2.  [建立 Windows Server 2012 R2 同盟伺服器陣列](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)

3.  [將原始設定資料匯入 Windows Server 2012 R2 AD FS 陣列](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)

##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>匯出和備份 AD FS 設定資料
 若要匯出 AD FS 設定，請執行下列程序：

###  <a name="to-export-service-settings"></a>匯出服務設定

1.  確定您可以存取 .pfx 檔案中的下列憑證和其私密金鑰：

    -   您要移轉的同盟伺服器陣列所使用的 SSL 憑證

    -   您要移轉的同盟伺服器陣列所使用的服務通訊憑證 (如果與 SSL 憑證不同)

    -   您要移轉的同盟伺服器陣列所使用的所有協力廠商權杖簽署或權杖加密/解密憑證

若要尋找 SSL 憑證，請開啟 Internet Information Services (IIS) 管理主控台，選取左窗格中的 [**預設的網站**]，按一下 [系結 **...** ] 在 [ **動作** ] 窗格中，尋找並選取 HTTPs 系結，按一下 [ **編輯**]，然後按一下 [ **View**]。

您必須將 Federation Service 所使用的 SSL 憑證以及其私密金鑰匯出至 .pfx 檔案。 如需詳細資訊，請參閱 [Export the Private Key Portion of a Server Authentication Certificate](export-the-private-key-portion-of-a-server-authentication-certificate.md)。

> [!NOTE]
>  如果您打算在 Windows Server 2012 R2 中執行 AD FS 的一部分部署裝置註冊服務，則必須取得新的 SSL 憑證。如需詳細資訊，請參閱 [註冊 AD FS 的 SSL 憑證](enroll-an-ssl-certificate-for-ad-fs.md) 和 [使用裝置註冊服務設定同盟伺服器](configure-a-federation-server-with-device-registration-service.md)。

若要檢視使用的權杖簽署、權杖解密和服務通訊憑證，請執行下列 Windows PowerShell 命令，建立檔案使用的所有憑證清單：

``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”
 ```

2. 將 AD FS Federation Service 屬性 (例如檔案的 Federation Service 名稱、Federation Service 顯示名稱和同盟伺服器識別碼) 匯出至檔案。

若要匯出 Federation Service 屬性，請開啟 Windows PowerShell，然後執行下列命令：

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.
```

輸出檔案將包含下列重要的設定值：

|**Get-ADFSProperties 報告的 Federation Service 屬性名稱**|**AD FS 管理主控台中的 Federation Service 屬性名稱**|
|-----|-----|
|HostName|Federation Service 名稱|
|識別碼|Federation Service 識別碼|
|DisplayName|Federation Service 顯示名稱|

3. 備份應用程式設定檔。 在其他設定中，這個檔案包含原則資料庫連接字串。

若要備份應用程式設定檔，必須手動將 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 檔案複製到備份伺服器上安全的位置。

> [!NOTE]
>  記下這個檔案中的資料庫連接字串，該字串緊接在 “policystore connectionstring=” 之後。 如果連接字串指定了某個 SQL Server 資料庫，還原同盟伺服器上的原始 AD FS 設定時就需要這個值。
>
>  以下是 WID 連接字串的範例： `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 連接字串的範例： `"Data Source=databasehostname;Integrated Security=True"`。

4. 記錄 AD FS Federation Service 帳戶的身分識別和此帳戶的密碼。

若要尋找識別值，請檢查 [服務]  主控台中 [AD FS 2.0 Windows 服務]  的 [登入身分]  欄位，然後手動記錄這個值。

> [!NOTE]
>  若為獨立 Federation Service，則會使用內建的網路服務帳戶。  在這種情況下，您不需要密碼。

5. 將啟用的 AD FS 端點清單匯出至檔案。

若要這樣做，請開啟 Windows PowerShell，然後執行下列命令：

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.
```

6. 將任何自訂宣告描述匯出至檔案。

若要這樣做，請開啟 Windows PowerShell，然後執行下列命令：

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.
 ```

7. 如果您在 web.config 檔案中設定自訂設定 (例如 useRelayStateForIdpInitiatedSignOn)，請記得備份 web.config 檔案以供參考。 您可以從對應到 IIS 中虛擬路徑 **“/adfs/ls”** 的目錄中複製檔案。 根據預設，該檔案位於 **%systemdrive%\inetpub\adfs\ls** 目錄。

###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>匯出宣告提供者信任和信賴憑證者信任

1.  若要匯出 AD FS 宣告提供者信任和信賴憑證者信任，您必須以系統管理員身分登入 (不過，您必須以系統管理員身分登入，而不是網域系統管理員) 到同盟伺服器上，然後執行位於 Windows Server 2012 R2 安裝光碟的 **media/server_support/adfs** 資料夾中的下列 Windows PowerShell 腳本： `export-federationconfiguration.ps1` 。

> [!IMPORTANT]
>  匯出指令碼採用下列參數：
>
> - Export-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-CertificatePassword <securestring \> ]
>   -   Export-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-CertificatePassword <securestring \> ] [-RelyingPartyTrustIdentifier <string [] >] [-ClaimsProviderTrustIdentifier <string [] >]
>   -   Export-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-CertificatePassword <securestring \> ] [-RelyingPartyTrustName <string [] >] [-ClaimsProviderTrustName <string [] >]
>
>   **-RelyingPartyTrustIdentifier <string[]>** - Cmdlet 只能針對字串陣列中指定的識別碼匯出信賴憑證者信任。 預設不會匯出信賴憑證者信任。 如果未指定 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName 和 ClaimsProviderTrustName，指令碼會匯出所有信賴憑證者信任和宣告提供者信任。
>
>   **-ClaimsProviderTrustIdentifier <string[]>** - Cmdlet 只能針對字串陣列中指定的識別碼匯出宣告提供者信任。 預設不會匯出宣告提供者信任。
>
>   **-RelyingPartyTrustName <string[]>** - Cmdlet 只能針對字串陣列中指定的名稱匯出信賴憑證者信任。 預設不會匯出信賴憑證者信任。
>
>   **-ClaimsProviderTrustName &lt;string[]&gt;** - Cmdlet 只能針對字串陣列中指定的名稱匯出宣告提供者信任。 預設不會匯出宣告提供者信任。
>
>   **-Path <字串 \>**-將包含匯出檔案的資料夾路徑。
>
>   **-ComputerName <字串 \>**-指定 STS 伺服器主機名稱。 預設是本機電腦。 如果您將 Windows Server 2012 的 AD FS 2.0 或 AD FS 移轉到 Windows Server 2012 R2 的 AD FS，此為舊版 AD FS 伺服器的主機名稱。
>
>   **-Credential <PSCredential \>**-指定具有執行此動作之許可權的使用者帳戶。 預設為目前使用者。
>
>   **-Force** – 指定不提示使用者確認。
>
>   **-CertificatePassword <SecureString \>**-指定用來匯出 AD FS 憑證私密金鑰的密碼。 如果未指定，若需要匯出具有私密金鑰的 AD FS 憑證，指令碼會提示輸入密碼。
>
>   **輸入**：無
>
>   **輸出**：字串 - 此 Cmdlet 會傳回匯出資料夾路徑。 您可以透過管道將傳回的物件傳送給 Import-FederationConfiguration。

###  <a name="to-back-up-custom-attribute-stores"></a>備份自訂屬性存放區

1.  您必須手動匯出要保留在 Windows Server 2012 R2 新 AD FS 伺服器陣列中的所有自訂屬性存放區。

> [!NOTE]
>  在 Windows Server 2012 R2 中，AD FS 需要以 .NET Framework 4.0 或更新版本為基礎的自訂屬性存放區。 請依照 [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) 中的指示安裝並設定 .Net Framework 4.5。

您可以執行下列 Windows PowerShell 命令，尋找 AD FS 使用的自訂屬性存放區的相關資訊：

``` powershell
Get-ADFSAttributeStore
```

升級或移轉自訂屬性存放區的步驟會有所不同。

2. 您也必須手動匯出要保留在 Windows Server 2012 R2 新 AD FS 伺服器陣列中的所有自訂屬性存放區的 .dll 檔案。 升級或移轉自訂屬性存放區之 .dll 檔案的步驟會有所不同。

##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>建立一個 Windows Server 2012 R2 同盟伺服器陣列

1.  在您要做為同盟伺服器的電腦上安裝 Windows Server 2012 R2 作業系統，然後新增 AD FS 伺服器角色。 如需詳細資訊，請參閱[安裝 AD FS 角色服務](install-the-ad-fs-role-service.md)。 接著透過 Active Directory Federation Service 設定精靈或 Windows PowerShell 設定新的 Federation Service。 如需詳細資訊，請參閱[設定同盟伺服器](configure-a-federation-server.md)中的＜設定新同盟伺服器陣列的第一個同盟伺服器＞。

完成此步驟時，您必須遵循下列指示：

-   您必須具有網域系統管理員權限才能設定 Federation Service。

-   您必須使用與 Windows Server 2012 的 AD FS 2.0 或 AD FS 相同的 Federation Service 名稱 (陣列名稱)。 如果您沒有使用相同的 federation service 名稱，則備份的憑證將無法在您嘗試設定的 Windows Server 2012 R2 federation service 中運作。

-   指定是否為 WID 或 SQL Server 同盟伺服器陣列。 如果是 SQL 陣列，請指定 SQL Server 資料庫位置和執行個體名稱。

-   準備進行 AD FS 移轉程序時，您必須提供 pfx 檔案，該檔案內含您所備份的 SSL 伺服器驗證憑證。

-   您必須指定與 Windows Server 2012 陣列的 AD FS 2.0 或 AD FS 相同的服務帳戶身分識別。

2. 設定完初始節點之後，就能在新的陣列新增其他節點。 如需詳細資訊，請參閱[設定同盟伺服器](configure-a-federation-server.md)中的＜將同盟伺服器新增至現有的同盟伺服器陣列＞。

##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>將原始設定資料匯入 Windows Server 2012 R2 AD FS 陣列
 現在您已有在 Windows Server 2012 R2 中執行的 AD FS 同盟伺服器陣列，您可以將原始的 AD FS 設定資料匯入其中。

1.  匯入和設定其他自訂 AD FS 憑證，包括外部註冊的權杖簽署和權杖解密/加密憑證，以及服務通訊憑證 (如果與 SSL 憑證不同)。

在 AD FS 管理主控台，選取 [憑證]。 檢查服務通訊、權杖加密/解密憑證和權杖簽署憑證，將每個值與移轉準備期間匯入 certificates.txt 檔案的值進行比對。

若要將權杖解密或權杖簽署憑證從預設的自我簽署憑證變更為外部憑證，您必須先停用預設啟用的自動憑證變換功能。 若要這樣做，您可使用下列 Windows PowerShell 命令：

``` powershell
Set-ADFSProperties –AutoCertificateRollover $false
```

2. 使用 Set-AdfsProperties Cmdlet 設定任何自訂 AD FS 服務設定，例如 AutoCertificateRollover 或 SSO 存留期。

3. 若要匯入 AD FS 的信賴憑證者信任和宣告提供者信任，您必須以系統管理員身分登入 (不過，您必須以系統管理員身分登入，而不是以網域系統管理員) 登入同盟伺服器，然後執行位於 Windows Server 2012 R2 安裝光碟的 \support\adfs 資料夾中的下列 Windows PowerShell 腳本：

    ``` powershell
    import-federationconfiguration.ps1
    ```

> [!IMPORTANT]
> 匯入指令碼採用下列參數：
>
>- Import-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-LogPath <string \> ] [-CertificatePassword <securestring \> ]
>- Import-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-LogPath <string \> ] [-CertificatePassword <securestring \> ] [-RelyingPartyTrustIdentifier <string [] >] [-ClaimsProviderTrustIdentifier <string [] >
>- Import-FederationConfiguration.ps1-Path <string \> [-ComputerName <string \> ] [-Credential <pscredential \> ] [-Force] [-LogPath <string \> ] [-CertificatePassword <securestring \> ] [-RelyingPartyTrustName <string [] >] [-ClaimsProviderTrustName <string [] >]
>
>**-RelyingPartyTrustIdentifier <string[]>** - Cmdlet 只能針對字串陣列中指定的識別碼匯入信賴憑證者信任。 預設不會匯入信賴憑證者信任。 如果未指定 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName 和 ClaimsProviderTrustName，指令碼會匯入所有信賴憑證者信任和宣告提供者信任。
>
>**-ClaimsProviderTrustIdentifier &lt;string[]&gt;** - Cmdlet 只能針對字串陣列中指定的識別碼匯入宣告提供者信任。 預設不會匯入宣告提供者信任。
>
>**-RelyingPartyTrustName &lt;string[]&gt;** - Cmdlet 只能針對字串陣列中指定的名稱匯入信賴憑證者信任。 預設不會匯入信賴憑證者信任。
>
>**-ClaimsProviderTrustName &lt;string[]&gt;** - Cmdlet 只能針對字串陣列中指定的名稱匯入宣告提供者信任。 預設不會匯入宣告提供者信任。
>
>**-Path <字串 \>**-包含要匯入之設定檔案的資料夾路徑。
>
>**-LogPath <字串 \>**-將包含匯入記錄檔的資料夾路徑。 此資料夾將會建立名為 “import.log” 的記錄檔。
>
>**-ComputerName <字串 \>**-指定 STS 伺服器的主機名稱。 預設是本機電腦。 如果您正在將 Windows Server 2012 的 AD FS 2.0 或 AD FS 移轉到 Windows Server 2012 R2 的 AD FS，此參數應設為舊版 AD FS 伺服器的主機名稱。
>
>**-Credential <PSCredential \>**-指定具有執行此動作之許可權的使用者帳戶。 預設為目前使用者。
>
>**-Force** – 指定不提示使用者確認。
>
>**-CertificatePassword <SecureString \>**-指定匯入 AD FS 憑證私密金鑰的密碼。 如果未指定，若需要匯入具有私密金鑰的 AD FS 憑證，指令碼會提示輸入密碼。
>
>**輸入：** 字串 - 此命令會以匯入資料夾路徑做為輸入。 您可以透過管道將 Export-FederationConfiguration 傳送給此命令。
>
>**輸出：** 無。

信賴憑證者信任的 WSFedEndpoint 屬性如有任何結尾空格，可能會造成匯入指令碼錯誤。 在這種情況下，請在匯入之前先手動移除檔案中的空格。 例如，下列項目會造成錯誤：

```
<URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>
```

```
<URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>
```

必須將它們編輯成：

```
<URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>
```

```
<URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>
```

> [!IMPORTANT]
> 如果來源系統的 Active Directory 宣告提供者信任中有任何自訂宣告規則 (AD FS 預設規則以外的規則)，指令碼不會移轉這些規則。 這是因為 Windows Server 2012 R2 有新的預設值。 任何自訂規則都必須藉由手動新增至新的 Windows Server 2012 R2 伺服器陣列中的 Active Directory 宣告提供者信任來合併。

1. 設定所有自訂 AD FS 端點設定。 在 AD FS 管理主控台，選取 [端點]。 將啟用的 AD FS 端點清單與 AD FS 移轉準備期間匯出至檔案的啟用 AD FS 端點清單進行比對。

    \- 還

    設定任何自訂宣告描述。 在 AD FS 管理主控台，選取 [宣告描述]。 將 AD FS 宣告描述清單與 AD FS 移轉準備期間匯出至檔案的宣告描述清單進行比對。 新增任何包含於您的檔案但未包含於 AD FS 中預設清單的自訂宣告描述。 請注意，管理主控台的宣告識別碼會對應至檔案的 ClaimType。

2. 安裝並設定所有備份自訂屬性存放區。 身為系統管理員，請確定所有自訂屬性存放區二進位檔都已升級為 .NET Framework 4.0 或更高版本，然後再更新 AD FS 設定以指向這些檔案。

3. 設定對應至舊版 web.config 檔案參數的服務屬性。

   -   如果 **useRelayStateForIdpInitiatedSignOn** 已新增至 windows server 2012 伺服器陣列中 AD FS 2.0 或 AD FS 的 **web.config** 檔案，則您必須在 windows server 2012 R2 伺服器陣列的 AD FS 中設定下列服務屬性：

       -   Windows Server 2012 R2 中的 AD FS 包含 **% systemroot% \ADFS\Microsoft.IdentityServer.Servicehost.exe.config** 檔案。 使用與 **web.config** 檔元素相同的語法來建立元素： `<useRelayStateForIdpInitiatedSignOn enabled="true" />` 。 將這個元素包含在 **Microsoft.IdentityServer.Servicehost.exe.config** 檔案 **<identityserver>** 區段中。

   -   如果 **<persistIdentityProviderInformation enabled = "true&#124;false" lifetimeInDays = "90" enablewhrPersistence = "true&#124;false"/ \>** 已新增至 windows server 2012 伺服器陣列中 AD FS 2.0 或 AD FS 的 **web.config** 檔案，則您必須在 windows server 2012 R2 伺服器陣列的 AD FS 中設定下列服務屬性：

       1.  在 Windows Server 2012 R2 的 AD FS 中，執行下列 Windows PowerShell 命令： `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime` 。

   -   如果 **<singleSignOn enabled = "true&#124;false"/ \>** 已新增至 windows server 2012 伺服器陣列 AD FS 2.0 或 AD FS 中的 **web.config** 檔案，您就不需要在 windows server 2012 R2 伺服器陣列中的 AD FS 設定任何其他服務屬性。 Windows Server 2012 R2 伺服器陣列中的 AD FS 預設會啟用單一登入。

   -   如果已將已將 localauthenticationtypes 設定新增至 Windows Server 2012 伺服器陣列中 AD FS 2.0 或 AD FS 的 **web.config** 檔案，則您必須在 windows Server 2012 R2 伺服器陣列的 AD FS 中設定下列服務屬性：

       -   整合式、表單、TlsClient、基本轉換清單到 Windows Server 2012 R2 中的對等 AD FS 具有全域驗證原則設定，可同時支援 federation service 和 proxy 驗證類型。 這些設定可以在 AD FS [驗證原則] 下的管理嵌入式管理單元中加以設定。

   匯入原始設定資料之後，您可以視需要自訂 AD FS 登入頁面。 如需詳細資訊，請參閱[自訂 AD FS 登入頁面](../operations/ad-fs-customization-in-windows-server.md)。

## <a name="next-steps"></a>後續步驟
 [將 Active Directory 同盟服務角色服務遷移至 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md) ，準備遷移[AD FS 同盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)的[AD FS 同盟伺服器](prepare-migrate-ad-fs-server-r2.md)，以[確認 AD FS 遷移至 Windows server 2012 R2](verify-ad-fs-migration.md)
