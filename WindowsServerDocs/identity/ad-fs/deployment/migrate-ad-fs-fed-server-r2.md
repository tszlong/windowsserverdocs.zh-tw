---
title: "移轉 AD FS 2.0 聯盟伺服器"
description: "AD FS 伺服器移轉到 Windows Server 2012 R2 提供相關資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>AD FS 2.0 聯盟伺服器移轉到 Windows Server 2012 R2 上 AD FS

若要移轉屬於單一節點 AD FS 發電廠、WIF 陣列或 Windows Server 2012 R2 SQL Server 陣列 AD FS 聯盟伺服器，您必須執行下列工作：  
  
1.  [匯出和備份 AD FS 設定資料](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [建立 Windows Server 2012 R2 聯盟伺服器陣列](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [匯入 Windows Server 2012 R2 AD FS 發電廠原始設定資料](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>匯出和備份 AD FS 設定資料  
 若要匯出 AD FS 設定，請執行下列程序：  
  
###  <a name="to-export-service-settings"></a>若要匯出服務設定  
  
1.  請確定您擁有的存取權下列憑證及私密金鑰.pfx 檔案中：  
  
    -   由您想要移轉聯盟伺服器陣列 SSL 憑證  
  
    -   （如果有不同的 SSL 憑證）服務通訊憑證，可供您想要移轉聯盟伺服器陣列  
  
    -   所有協力廠商權杖簽署或預付碼-加密解密憑證可供您想要移轉聯盟伺服器陣列  
  
若要尋找 SSL 憑證，開放網際網路服務 (IIS) 管理主控台中，選取**預設網站**在左窗格中，按一下 [**繫結...** 在**動作**窗格中，尋找並選取 https 繫結，按一下 [**編輯**，然後按一下 [**檢視**。  
  
您必須匯出同盟服務和.pfx 檔案其私密金鑰使用 SSL 憑證。 如需詳細資訊，請查看[匯出私人鍵部分伺服器驗證憑證的](export-the-private-key-portion-of-a-server-authentication-certificate.md)。  
  
> [!NOTE]
>  如果您要部署的裝置登記服務 AD FS 您執行 Windows Server 2012 R2 的一部分，您必須取得新的 SSL 憑證。如需詳細資訊，請查看[註冊 AD FS SSL 憑證](enroll-an-ssl-certificate-for-ad-fs.md)和[設定聯盟伺服器裝置登記服務與](configure-a-federation-server-with-device-registration-service.md)。  
  
若要檢視權杖登入，權杖解密及所使用的服務通訊憑證，請執行下列 Windows PowerShell 命令來建立檔案中使用的所有的憑證清單：  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  AD FS 同盟服務屬性，例如同盟服務名稱、同盟服務顯示名稱，並聯盟伺服器識別碼匯出檔案。  
  
若要匯出同盟服務屬性，開放 Windows PowerShell 並執行下列命令： 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

輸出檔案將會包含重要設定下列值：  
 
|**聯盟服務屬性名稱 Get-ADFSProperties 報告**|**AD FS 管理主控台中同盟服務屬性名稱**|
|-----|-----|  
|主機|聯盟服務名稱|  
|識別碼|聯盟服務識別碼|  
|顯示名稱|聯盟服務顯示名稱|  
  
3.  備份應用程式的設定檔。 在其他設定，此檔案包含原則資料庫連接字串。  
  
若要備份應用程式的設定檔，您必須手動複製`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`檔案備份伺服器在安全的位置。  
  
> [!NOTE]
>  請記下資料庫連接字串此檔案中位於後立即」policystore 連接字串 =」。 如果連接字串指定 SQL Server 資料庫還原原始 AD FS 伺服器上的設定聯盟時需要值。  
>   
>  以下是 WID 連接字串的範例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 連接字串的範例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
4.  記錄 AD FS 同盟服務 account 的身分，這 account 的密碼。  
  
若要尋找的身分值，請檢查**登入身分**欄的**AD FS 2.0 Windows 服務**中**服務**主機，並手動記錄這個值。  
  
> [!NOTE]
>  獨立同盟服務，可建帳號網路的服務。  若是如此，您不需要有密碼。  
  
5.  檔案匯出讓 AD FS 端點的清單。  
  
若要這樣做，請打開 Windows PowerShell 並執行下列命令： 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  匯出檔案的任何自訂宣告描述。  
  
若要這樣做，請打開 Windows PowerShell 並執行下列命令： 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  如果您有自訂設定，例如 useRelayStateForIdpInitiatedSignOn web.config 檔案中的設定，請確定您備份的參考 web.config。 您可以從 [對應至 virtual 路徑 directory 複製檔案**「日 adfs 日 ls]**在。 根據預設，這是在**%systemdrive%\inetpub\adfs\ls** directory。  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>若要匯出宣告提供者信任且信賴信任  
  
1.  若要匯出 AD FS 宣告信任提供者和廠商信任做為基礎，您必須登入以系統管理員身分 (但不為網域系統管理員) 到您聯盟伺服器，並執行下列 Windows PowerShell 指令碼是位於**媒體日 server_support 日 adfs**資料夾的 Windows Server 2012 R2 安裝光碟：`export-federationconfiguration.ps1`。  
  
> [!IMPORTANT]
>  匯出指令碼的參數下列動作：  
>   
>  -   Export-FederationConfiguration.ps1-Path < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-CertificatePassword < securestring\ >]  
> -   Export-FederationConfiguration.ps1-路徑 < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < 字串 [>] [-ClaimsProviderTrustIdentifier < 字串 [] >]  
> -   Export-FederationConfiguration.ps1-路徑 < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < 字串 [>] [-ClaimsProviderTrustName < 字串 [] >]  
>   
>  **-RelyingPartyTrustIdentifier < 字串 [>** -cmdlet 只有匯出可以其識別碼詳列於字串陣列廠商信任。 預設值是匯出皆信賴的派對信任。 如果未 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName，以及 ClaimsProviderTrustName 指定，指令碼將匯出所有信賴信任並宣告信任提供者。  
>   
>  **-ClaimsProviderTrustIdentifier < 字串 [>** -cmdlet 只會匯出宣告其識別碼字串陣列中指定的提供者信任。 匯出宣告提供者信任皆為預設值。  
>   
>  **-RelyingPartyTrustName < 字串 [>** -cmdlet 只有匯出可以廠商信任字串陣列中指定的名稱。 預設值是匯出皆信賴的派對信任。  
>   
>  **-ClaimsProviderTrustName < 字串 [>** -cmdlet 只會匯出宣告其名稱的字串陣列中所指定的提供者信任。 匯出宣告提供者信任皆為預設值。  
>   
>  **-Path < string\ >** -資料夾會包含匯出之的檔案的路徑。  
>   
>  **-電腦名稱 < string\ >** -指定 STS 伺服器主機名稱。 預設值是本機電腦。 如果您要在 Windows Server 2012 R2 AD FS 移轉 AD FS 2.0 或 Windows Server 2012 中的 AD FS，這是主機舊版 AD FS 伺服器的名稱。  
>   
>  **-Credential < PSCredential\ >** -指定帳號具有權限來執行此動作。 預設值是目前的使用者。  
>   
>  **-強制**– 指定不使用者確認的提示。  
>   
>  **-CertificatePassword < SecureString\ >** -指定匯出 AD FS 憑證私密金鑰的密碼。 如果您不指定，如果需要匯出私密金鑰 AD FS 憑證指令碼將會提示輸入密碼。  
>   
>  **輸入**：無  
>   
>  **輸出**：字串-這個 cmdlet 傳回匯出資料夾路徑。 您可以管道 Import-FederationConfiguration 傳回的物件。  
  
###  <a name="to-back-up-custom-attribute-stores"></a>若要備份自訂屬性存放區  
  
1.  您必須手動匯出您想要讓您在 Windows Server 2012 R2 的新 AD FS 發電廠中的所有自訂屬性存放區。  
  
> [!NOTE]
>  在 Windows Server 2012 R2，AD FS 需要為基礎的.NET Framework 4.0 或上述自訂屬性存放區。 請依照[的 Microsoft.NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)來安裝，安裝.Net Framework 4.5。  
  
您可以執行下列 Windows PowerShell 命令 AD FS 使用找到自訂屬性存放區的相關資訊： 

``` powershell
Get-ADFSAttributeStore
```  

步驟升級，或者移轉自訂屬性存放區而有所不同。  
  
2.  您必須手動匯出您想要讓您在 Windows Server 2012 R2 的新 AD FS 發電廠中的自訂屬性商店所有.dll 檔案。 步驟升級或移轉自訂屬性存放區的.dll 檔案，而有所不同。  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>建立 Windows Server 2012 R2 聯盟伺服器陣列  
  
1.  Windows Server 2012 R2 作業系統的電腦上安裝您想要作為聯盟伺服器，然後新增 AD FS 伺服器角色。 如需詳細資訊，請查看[安裝 AD FS 角色服務](install-the-ad-fs-role-service.md)。 然後將您新同盟服務 Active Directory 同盟服務設定精靈透過或透過 Windows PowerShell 設定。 如需詳細資訊，看到」新聯盟伺服器陣列中設定的第一個聯盟伺服器][設定聯盟伺服器](configure-a-federation-server.md)。  

在完成此步驟，時，您必須遵循這些指示執行：  
  
-   您必須以設定您的同盟服務網域系統管理員權限。  
  
-   已使用 AD FS 2.0 或 Windows Server 2012 中的 AD FS 中，您必須使用相同的同盟服務名稱（發電廠名稱）。 如果您不使用的相同同盟服務名稱，在您嘗試設定的 Windows Server 2012 R2 同盟服務無法運作，您備份的憑證。  
  
-   指定是否 WID 或 SQL Server 聯盟伺服器發電廠。 如果是 SQL 發電廠，指定 SQL Server 資料庫位置和執行個體的名稱。  
  
-   您必須提供 pfx 包含 SSL 伺服器驗證憑證的準備 AD FS 移轉處理程序的一部分為您備份檔案。  
  
-   您必須指定中 AD FS 2.0 或 Windows Server 2012 發電廠中的 AD FS 使用的相同服務 account 身分。  
  
2.  初始節點設定之後, 您可以新增額外的節點您新發電廠。 如需詳細資訊，查看 [現有聯盟伺服器陣列新增聯盟伺服器][設定聯盟伺服器](configure-a-federation-server.md)。  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>匯入 Windows Server 2012 R2 AD FS 發電廠原始設定資料  
 既然您已經執行 Windows Server 2012 R2 AD FS 聯盟伺服器發電廠，您可以匯入它的原始 AD FS 設定資料。  
  
1.  匯入和其他自訂 AD FS 憑證的設定，包括外部退出權杖簽署及加密解密權杖日憑證，並服務通訊憑證是否不同 SSL 憑證。  
  
在 AD FS 管理主控台中，選取 [**的憑證**。 檢查服務通訊，權杖-加密日解密和權杖簽署的憑證檢查每個針對您要匯出至 certificates.txt 檔案時準備移轉的值。  
  
若要變更的預設自我簽署的憑證外部憑證權杖解密或預付碼簽署的憑證，您必須先停用功能的預設的自動憑證變換功能。 若要這樣做，您可以使用下列的 Windows PowerShell 命令：  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  在任何設定自訂 AD FS 服務 AutoCertificateRollover 或 SSO 期間使用 Set-AdfsProperties cmdlet 例如。  
  
3.  若要匯入 AD FS 信賴的派對信任和宣告提供者信任，您必須登入以系統管理員身分 (不過，不做為網域系統管理員) 到您聯盟伺服器，並執行下列 Windows PowerShell 指令碼是位於 \support\adfs 資料夾中的 Windows Server 2012 R2 安裝光碟：  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  匯入指令碼的參數下列動作：  
>   
>  -  Import-FederationConfiguration.ps1-Path < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   Import-FederationConfiguration.ps1-路徑 < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < 字串 [>] [-ClaimsProviderTrustIdentifier < 字串 [] >  
> -   Import-FederationConfiguration.ps1-路徑 < string\ > [-< string\ > 電腦名稱] [-認證 < pscredential\ >] [-推動] [-LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < 字串 [>] [-ClaimsProviderTrustName < 字串 [] >]  
>   
>  **-RelyingPartyTrustIdentifier < 字串 [>** -可以其識別碼詳列於字串陣列廠商信任的 cmdlet 只匯入。 預設值是信賴的派對信任皆匯入。 如果未 RelyingPartyTrustIdentifier、ClaimsProviderTrustIdentifier、RelyingPartyTrustName，以及 ClaimsProviderTrustName 指定，指令碼將會匯入所有信賴信任並宣告信任提供者。  
>   
>  **-ClaimsProviderTrustIdentifier < 字串 [>** -cmdlet 只能匯入宣告其識別碼字串陣列中指定的提供者信任。 預設值不是宣告提供者信任匯入。  
>   
>  **-RelyingPartyTrustName < 字串 [>** -cmdlet 只匯入可以廠商信任字串陣列中指定的名稱。 預設值是信賴的派對信任皆匯入。  
>   
>  **-ClaimsProviderTrustName < 字串 [>** -cmdlet 只能匯入宣告其名稱的字串陣列中所指定的提供者信任。 預設值不是宣告提供者信任匯入。  
>   
>  **-Path < string\ >**的資料夾，其中包含要匯入的設定檔的路徑。  
>   
>  **-LogPath < string\ >** -資料夾會包含登入匯入檔案的路徑。 在此資料夾會建立名為「import.log「登入檔案。  
>   
>  **-電腦名稱 < string\ >** -指定主機 STS 伺服器名稱。 預設值是本機電腦。 如果您要在 Windows Server 2012 R2 AD FS 移轉 AD FS 2.0 或 Windows Server 2012 中的 AD FS，此參數應設主機舊版 AD FS 伺服器的名稱。  
>   
>  **-Credential < PSCredential\ >**-指定帳號具有權限來執行此動作。 預設值是目前的使用者。  
>   
>  **-強制**– 指定不使用者確認的提示。  
>   
>  **-CertificatePassword < SecureString\ >** -指定匯入 AD FS 憑證私密金鑰的密碼。 如果您不指定，如果需要匯入 AD FS 憑證，以私密金鑰指令碼將會提示輸入密碼。  
>   
>  **輸入：**字串-這個命令會匯入資料夾路徑當做輸入。 您可以管道 Export-FederationConfiguration 此命令。  
>   
>  **輸出：**無。  
  
任何空格信賴的派對信任的 WSFedEndpoint 屬性中可能會造成錯誤匯入指令碼。 此時，請手動移除空間檔案之前，若要匯入。 例如，這些項目會造成錯誤：  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  如果您有任何自訂宣告規則（規則 AD FS 預設規則以外）上的 Active Directory 宣告提供者信任的來源系統中，這些將不會移轉的指令碼。 這是因為 Windows Server 2012 R2 有新的預設值。 必須手動新增到新的 Windows Server 2012 R2 發電廠 Active Directory 宣告提供者信任合併任何自訂規則。  
  
4.  設定所有自訂 AD FS 結束點。 AD FS 管理主控台中，選取 [**的端點**。 檢查清單中時準備 AD FS 移轉匯出到檔案的讓 AD FS 端點針對讓的 AD FS 結束點。  
  
     \-以及-  
  
     設定任何自訂宣告描述。 AD FS 管理主控台中，選取 [**宣告描述**。 檢查 AD FS 理賠要求描述清單的理賠要求描述您的檔案匯出準備 AD FS 移轉作業時的清單。 新增包含在您的檔案，但在 [預設清單中 AD FS 不包含任何自訂宣告描述。 請注意宣告識別碼管理主控台中的將檔案 ClaimType 對應。  
  
5.  安裝和所有備份的自訂的設定存放區的屬性。 系統管理員的身分，以確保任何自訂屬性市集二進位檔.NET Framework 4.0 或更高版本升級之前更新 AD FS 指向這些設定。  
  
6.  設定 web.config 舊版檔案參數地圖服務屬性。  
  
    -   如果**useRelayStateForIdpInitiatedSignOn**已加入到**web.config**中 AD FS 2.0 或中 Windows 伺服器 2012 年發電廠，AD FS 檔案，您必須在 Windows Server 2012 R2 發電廠您 AD FS 中設定下列服務屬性：  
  
        -   在 Windows Server 2012 R2 AD FS 包含**%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config**檔案。 使用與相同的語法建立的項目**web.config**元素檔案：`<useRelayStateForIdpInitiatedSignOn enabled="true" />`。 將此項目的一部分**< Microsoft.identityserver.web >**區段**Microsoft.IdentityServer.Servicehost.exe.config**檔案。  
  
    -   如果**< persistIdentityProviderInformation 支援 =」true 和 #124; false」lifetimeInDays =」90」enablewhrPersistence =」true 和 #124; false」日 \ >**已加入到**web.config**中 AD FS 2.0 或中 Windows 伺服器 2012 年發電廠，AD FS 檔案，則必須設定中的 Windows Server 2012 R2 發電廠您 AD FS 下列服務屬性：  
  
        1.  在 Windows Server 2012 R2 AD FS，執行下列 Windows PowerShell 命令：`Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`。  
  
    -   如果**< singleSignOn 支援 = 」 true 和 #124; false 」 日 \ >**已加入到**web.config**中您 AD FS 2.0 檔案或 AD FS 發電廠 Windows 伺服器 2012 」 中的，您不需要任何其他服務屬性在 AD FS 您設定 Windows Server 2012 R2 發電廠。 單一登入預設的 Windows Server 2012 R2 發電廠 AD FS 支援。  
  
    -   如果 localAuthenticationTypes 設定已新增至**web.config**您必須在 Windows Server 2012 R2 發電廠您 AD FS 中設定下列服務屬性檔案 AD FS 2.0 或中 Windows 伺服器 2012年發電廠，AD FS 中：  
  
        -   整合，表單、 TlsClient，基本轉換清單相當於在 Windows Server 2012 R2 的 AD FS 有全球驗證原則設定支援兩種同盟服務和 proxy 驗證類型。 在 [管理] 嵌入式管理單元在 AD FS 這些設定可以設定**驗證原則**。  
  
 匯入的原始設定資料之後，您可以視需要自訂 AD FS 登入頁面。 如需詳細資訊，請查看[[自訂頁面 AD FS 登入](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
## <a name="next-steps"></a>後續步驟
 [Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [正在準備移轉 AD FS 聯盟伺服器](prepare-migrate-ad-fs-server-r2.md)   
 [移轉 AD FS 聯盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
 [檢查 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)