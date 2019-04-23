---
title: 準備移轉獨立的 AD FS 同盟伺服器
description: 提供已準備好移轉到 Windows Server 2012 的獨立的 AD FS 伺服器的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834489"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>準備移轉獨立的 AD FS 同盟伺服器或單一節點 AD FS 伺服器陣列  
 
若要準備移轉 （相同伺服器移轉） 獨立的 AD FS 2.0 同盟伺服器或單一節點 AD FS 伺服器陣列到 Windows Server 2012，您必須匯出並從這個伺服器備份的 AD FS 組態資料。  
  
若要匯出 AD FS 設定資料，請執行下列工作：  
  
-   [步驟 1：匯出服務設定](#step-1-export-service-settings)  
  
-   [步驟 2：匯出宣告提供者信任](#step-2-export-claims-provider-trusts)  
  
-   [步驟 3：匯出信賴憑證者信任](#step-3-export-relying-party-trusts)  
  
-   [步驟 4：備份自訂屬性存放區](#step-4-back-up-custom-attribute-stores)  
  
-   [步驟 5：備份網頁自訂項目](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>步驟 1：匯出服務設定  
 若要匯出服務設定，請執行下列程序：  
  
### <a name="to-export-service-settings"></a>匯出服務設定  
  
1.  記錄 Federation Service 所使用之 SSL 憑證的憑證主體名稱和憑證指紋值。 若要尋找 SSL 憑證，請開啟 Internet Information Services (IIS) 管理主控台中，選取**Default Web Site**的左窗格中，按一下 **繫結...** 在 **動作**窗格中，尋找並選取 https 繫結中，按一下**編輯**，然後按一下**檢視**。  
  
> [!NOTE]
>  或者，您也可以將 Federation Service 所使用的 SSL 憑證以及其私密金鑰匯出至 .pfx 檔案。 如需詳細資訊，請參閱＜ [匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)＞。  
>   
>  匯出 SSL 憑證是選擇性的，因為此憑證儲存在本機電腦個人憑證存放區中，作業系統升級時會予以保留。  
  
2.  記錄 AD FS 服務通訊、權杖解密與權杖簽署憑證的設定。  若要檢視所使用的所有憑證，請開啟 Windows PowerShell 並執行下列命令以將 AD FS cmdlet 新增至您的 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令來建立檔案中的使用中的所有憑證的清單 `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  或者，除了所有自我簽署憑證以外，您也可以匯出不是內部產生的任何權杖簽署、權杖加密或服務通訊憑證以及金鑰。 您可以使用 Windows PowerShell 來檢視伺服器上使用中的所有憑證。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell`。 然後執行下列命令來檢視您的伺服器上使用中的所有憑證`PSH:>Get-ADFSCertificate`。 這個命令的輸出包括指定每個憑證存放區位置的 StoreLocation 與 StoreName 值。 然後，您可以使用[匯出伺服器驗證憑證的私用金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)中的指導方針，將每個憑證及其私密金鑰匯出至 .pfx 檔案。  
>   
>  匯出這些憑證是選擇性的，因為作業系統升級期間會保留所有外部憑證。  
  
3.  匯出 AD FS 2.0 federation service 屬性，例如 federation service 名稱、 federation service 顯示名稱和同盟伺服器識別碼的檔案。  
  
若要匯出 federation service 屬性，開啟 Windows PowerShell 並執行下列命令以將 AD FS cmdlet 新增至您的 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出 federation service 屬性： `PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`。  
  
輸出檔案將包含下列重要的設定值：  
  
    
|**Get-adfsproperties 報告的 federation Service 屬性名稱**|**在 AD FS 管理主控台中的 federation Service 屬性名稱**|
|------|------|
|HostName|Federation Service 名稱|  
|識別碼|Federation Service 識別碼|  
|DisplayName|Federation Service 顯示名稱|  
  
4.  備份應用程式設定檔。 在其他設定中，這個檔案包含原則資料庫連接字串。  
  
若要備份應用程式設定檔，必須手動將 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 檔案複製到備份伺服器上安全的位置。  
  
> [!NOTE]
>  記下這個檔案中的資料庫連接字串，該字串緊接在 “policystore connectionstring=” 之後。 如果連接字串指定了某個 SQL Server 資料庫，還原同盟伺服器上的原始 AD FS 設定時就需要這個值。  
>   
>  以下是 WID 連接字串的範例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 連接字串的範例： `"Data Source=databasehostname;Integrated Security=True"`。  
  
5.  記錄 AD FS 2.0 同盟服務帳戶的身分識別和此帳戶的密碼。  
  
若要尋找識別值，請檢查 [服務]  主控台中 [AD FS 2.0 Windows 服務]  的 [登入身分]  欄位，然後手動記錄這個值。  
  
> [!NOTE]
>  若為獨立 Federation Service，則會使用內建的網路服務帳戶。  在這種情況下，您不需要密碼。  
  
6.  將啟用的 AD FS 端點清單匯出至檔案。  
  
若要這樣做，請開啟 Windows PowerShell 並執行下列命令以將 AD FS cmdlet 新增至您的 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以啟用的 AD FS 端點清單匯出至檔案： `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`。  
  
7.  將任何自訂宣告描述匯出至檔案。  
  
若要這樣做，請開啟 Windows PowerShell 並執行下列命令以將 AD FS cmdlet 新增至您的 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將任何自訂宣告描述匯出至檔案： `Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`。  
  
##  <a name="step-2-export-claims-provider-trusts"></a>步驟 2：匯出宣告提供者信任  
 若要匯出宣告提供者信任，請執行下列程序：  
  
### <a name="to-export-claims-provider-trusts"></a>匯出宣告提供者信任  
  
1.  您可以使用 Windows PowerShell 來匯出所有宣告提供者信任。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出所有宣告提供者信任： `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`。  
  
## <a name="step-3-export-relying-party-trusts"></a>步驟 3：匯出信賴憑證者信任  
 若要匯出信賴憑證者信任，請執行下列程序：  
  
### <a name="to-export-relying-party-trusts"></a>匯出信賴憑證者信任  
  
1.  若要匯出所有信賴憑證者信任，開啟 Windows PowerShell 並執行下列命令以將 AD FS cmdlet 新增至您的 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出所有信賴憑證者信任：`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`。  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>步驟 4：備份自訂屬性存放區  
 您可以使用 Windows PowerShell 命令，尋找 AD FS 使用的自訂屬性存放區的相關資訊。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以尋找自訂屬性存放區的相關資訊： `PSH:>Get-ADFSAttributeStore`。 升級或移轉自訂屬性存放區的步驟會有所不同。  
  
## <a name="step-5-back-up-webpage-customizations"></a>步驟 5：備份網頁的自訂項目  
 若要備份任何網頁自訂項目，複製 AD FS 網頁和**web.config**對應到虛擬路徑的目錄中的檔案 **"/ adfs/ls"** 在 IIS 中。 根據預設，它在 **%systemdrive%\inetpub\adfs\ls** 目錄中。  

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)