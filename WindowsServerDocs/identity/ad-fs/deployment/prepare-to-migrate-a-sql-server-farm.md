---
title: 準備移轉 AD FS SQL 伺服器陣列
description: 準備移轉到 Windows Server 2012 的 AD FS 伺服器的 SQL 伺服器陣列提供相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 284e02174b4a8c06f114640223d289dc63ea3a26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890389"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>準備移轉 SQL Server 伺服器陣列  
 若要準備移轉屬於 SQL Server 伺服器陣列到 Windows Server 2012 的 AD FS 2.0 同盟伺服器，您必須匯出，並從這些伺服器備份的 AD FS 組態資料。  
  
 若要匯出 AD FS 設定資料，請執行下列工作：  
  
-   [步驟 1：匯出服務設定](#step-1-export-service-settings)  
  
-   [步驟 2：備份自訂屬性存放區](#step-2-back-up-custom-attribute-stores)  
  
-   [步驟 3：備份網頁自訂項目](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>步驟 1：匯出服務設定  
 若要匯出服務設定，請執行下列程序：  
  
### <a name="to-export-service-settings"></a>匯出服務設定  
  
1.  記錄 Federation Service 所使用之 SSL 憑證的憑證主體名稱和憑證指紋值。 若要尋找 SSL 憑證，請開啟 Internet Information Services (IIS) 管理主控台中，選取**Default Web Site**的左窗格中，按一下 **繫結...** 在 **動作**窗格中，尋找並選取 https 繫結中，按一下**編輯**，然後按一下**檢視**。  
  
> [!NOTE]
>  或者，您也可以將 SSL 憑證以及其私密金鑰匯出至 .pfx 檔案。 如需詳細資訊，請參閱＜ [匯出伺服器驗證憑證的私密金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)＞。  
>   
>  這個步驟是選擇性的，因為此憑證儲存在本機電腦個人憑證存放區中，作業系統升級時會予以保留。  
  
2.  匯出不是在 AD FS 內部產生的權杖簽署、權杖加密或服務通訊憑證及金鑰。  
  
您可以使用 Windows PowerShell 來檢視伺服器上 AD FS 使用的所有憑證。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令來檢視您的伺服器上使用中的所有憑證`PSH:>Get-ADFSCertificate`。 這個命令的輸出包括指定每個憑證存放區位置的 StoreLocation 與 StoreName 值。  
  
> [!NOTE]
>  或者，您可以使用[匯出伺服器驗證憑證的私用金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)中的指導方針，將每個憑證及其私密金鑰匯出至 .pfx 檔案。 這個步驟是選擇性的，因為作業系統升級期間會保留所有外部憑證。  
  
3.  備份應用程式設定檔。 在其他設定中，這個檔案包含原則資料庫連接字串。  
  
若要備份應用程式設定檔，必須手動將 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 檔案複製到備份伺服器上安全的位置。  
  
> [!NOTE]
>  記錄 SQL Server 連接字串之後"policystore connectionstring ="在下列檔案： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。 當您還原原始的 AD FS 設定同盟伺服器上時，您會需要此字串。  
  
4.  記錄 AD FS 2.0 同盟服務帳戶的身分識別和此帳戶的密碼。  
  
若要尋找識別值，請檢查**登入身分**資料行**AD FS 2.0 Windows 服務**中**Services**主控台，然後手動記錄這個值。  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>步驟 2：備份自訂屬性存放區  
 您可以使用 Windows PowerShell 命令，尋找 AD FS 使用的自訂屬性存放區的相關資訊。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以尋找自訂屬性存放區的相關資訊： `PSH:>Get-ADFSAttributeStore`。 升級或移轉自訂屬性存放區的步驟會有所不同。  
  
## <a name="step-3-back-up-webpage-customizations"></a>步驟 3：備份網頁的自訂項目  
 若要備份任何網頁自訂項目，複製 AD FS 網頁和**web.config**對應到虛擬路徑的目錄中的檔案 **"/ adfs/ls"** 在 IIS 中。 根據預設，它在 **%systemdrive%\inetpub\adfs\ls** 目錄中。  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)