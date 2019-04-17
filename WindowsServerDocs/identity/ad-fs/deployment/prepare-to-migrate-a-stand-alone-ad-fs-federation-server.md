---
title: "準備移轉獨立 AD FS 聯盟伺服器"
description: "在準備好獨立 AD FS 伺服器移轉到 Windows Server 2012 中提供的資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>準備獨立 AD FS 聯盟伺服器或單一節點 AD FS 發電廠移轉  
 
若要準備移轉（相同伺服器移轉）獨立 AD FS 2.0 聯盟伺服器或單一節點 AD FS 發電廠到 Windows Server 2012，您必須匯出並從這個伺服器備份 AD FS 設定資料。  
  
若要匯出 AD FS 設定資料，請執行下列工作：  
  
-   [步驟 1：匯出服務設定](#step-1-export-service-settings)  
  
-   [步驟 2：匯出宣告信任提供者](#step-2-export-claims-provider-trusts)  
  
-   [步驟 3：匯出信賴廠商信任](#step-3-export-relying-party-trusts)  
  
-   [步驟 4：備份自訂屬性存放區](#step-4-back-up-custom-attribute-stores)  
  
-   [步驟 5：備份網頁的自訂項目](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>步驟 1：匯出服務設定  
 若要匯出服務設定，請執行下列程序：  
  
### <a name="to-export-service-settings"></a>若要匯出服務設定  
  
1.  記錄憑證主體名稱與指紋的值同盟服務使用 SSL 憑證。 若要尋找 SSL 憑證，開放網際網路服務 (IIS) 管理主控台中，選取**預設網站**在左窗格中，按一下 [**繫結...** 在**動作**窗格中，尋找並選取 https 繫結，按一下 [**編輯**，然後按一下 [**檢視**。  
  
> [!NOTE]
>  或者，您也可以匯出同盟服務和.pfx 檔案其私密金鑰使用 SSL 憑證。 如需詳細資訊，請查看[匯出私人鍵部分伺服器驗證憑證的](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  因為這憑證會儲存在本機電腦個人化憑證存放區中且會保留在作業系統升級匯出 SSL 憑證是選擇性的。  
  
2.  記錄 AD FS 服務通訊的設定、權杖解密及權杖簽署的憑證。  若要檢視所有可用的憑證，開放 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以建立檔案中使用的所有的憑證清單 `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  或者，您也可以匯出任何權杖簽署、權杖加密或服務通訊的憑證和按鍵不內部專，除了所有自動簽署的憑證。 您可以檢視所有使用 Windows PowerShell 來使用您的伺服器上的憑證。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell`。 然後執行下列命令，以檢視所有的憑證會使用您的伺服器上的`PSH:>Get-ADFSCertificate`。 這個命令的輸出包括 StoreLocation 和 StoreName 值指定每個憑證存放區的位置。 您可以使用中的指導[匯出私人鍵部分伺服器驗證憑證的](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)，將每個憑證及私密金鑰匯出至.pfx 檔案。  
>   
>  因為在作業系統升級過程中保留所有外部憑證匯出這些憑證是選擇性的。  
  
3.  匯出 AD FS 2.0 聯盟服務屬性，例如同盟服務名稱、同盟服務顯示名稱，聯盟伺服器 id，檔案。  
  
若要匯出同盟服務屬性，開放 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出同盟服務屬性：`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`。  
  
輸出檔案將會包含重要設定下列值：  
  
    
|**聯盟服務屬性名稱 Get-ADFSProperties 報告**|**AD FS 管理主控台中同盟服務屬性名稱**|
|------|------|
|主機|聯盟服務名稱|  
|識別碼|聯盟服務識別碼|  
|顯示名稱|聯盟服務顯示名稱|  
  
4.  備份應用程式的設定檔。 在其他設定，此檔案包含原則資料庫連接字串。  
  
若要備份應用程式的設定檔，您必須手動複製`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`檔案備份伺服器在安全的位置。  
  
> [!NOTE]
>  請記下資料庫連接字串此檔案中位於後立即」policystore 連接字串 =」)。 如果連接字串指定 SQL Server 資料庫還原原始 AD FS 伺服器上的設定聯盟時需要值。  
>   
>  以下是 WID 連接字串的範例：`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`。 以下是 SQL Server 連接字串的範例：`"Data Source=databasehostname;Integrated Security=True"`。  
  
5.  記錄 AD FS 2.0 同盟服務 account 的身分，這 account 的密碼。  
  
若要尋找的身分值，請檢查**登入身分**欄的**AD FS 2.0 Windows 服務**中**服務**主機，並手動記錄這個值。  
  
> [!NOTE]
>  獨立同盟服務，可建帳號網路的服務。  若是如此，您不需要有密碼。  
  
6.  檔案匯出讓 AD FS 端點的清單。  
  
若要這樣做，請打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以讓 AD FS 端點清單匯出至檔案：`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`。  
  
7.  匯出檔案的任何自訂宣告描述。  
  
若要這樣做，請打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出檔案的任何自訂宣告描述：`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`。  
  
##  <a name="step-2-export-claims-provider-trusts"></a>步驟 2：匯出宣告信任提供者  
 若要匯出宣告提供者信任，執行下列程序：  
  
### <a name="to-export-claims-provider-trusts"></a>若要匯出宣告信任提供者  
  
1.  您可以使用 Windows PowerShell 來匯出所有宣告提供者信任。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令以匯出所有宣告提供者信任：`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`。  
  
## <a name="step-3-export-relying-party-trusts"></a>步驟 3：匯出信賴廠商信任  
 若要匯出信賴廠商信任，執行下列程序：  
  
### <a name="to-export-relying-party-trusts"></a>若要匯出信賴廠商信任  
  
1.  匯出所有信賴廠商信任、透過 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，將所有信賴廠商信任匯出：`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`。  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>步驟 4：備份自訂屬性存放區  
 您可以使用 Windows PowerShell 來使用 AD FS 找到自訂屬性存放區的相關資訊。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以尋找自訂屬性存放區的相關資訊：`PSH:>Get-ADFSAttributeStore`。 步驟升級，或者移轉自訂屬性存放區而有所不同。  
  
## <a name="step-5-back-up-webpage-customizations"></a>步驟 5：備份網頁的自訂項目  
 若要備份的任何網頁自訂項目，複製 AD FS 網頁和**web.config**檔案從 [對應至 virtual 路徑 directory **」日 adfs 日 ls]**在。 根據預設，這是在**%systemdrive%\inetpub\adfs\ls** directory。  

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)