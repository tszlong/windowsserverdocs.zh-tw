---
title: "準備移轉 AD FS SQL 陣列"
description: "在準備好 AD FS 伺服器 SQL 陣列移轉到 Windows Server 2012 中提供的資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 284e02174b4a8c06f114640223d289dc63ea3a26
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>準備移轉 SQL Server 陣列  
 若要準備移轉到 Windows Server 2012 SQL Server 發電廠屬於 AD FS 2.0 聯盟伺服器，您必須匯出，並從這些伺服器備份 AD FS 設定資料。  
  
 若要匯出 AD FS 設定資料，請執行下列工作：  
  
-   [步驟 1：匯出服務設定](#step-1-export-service-settings)  
  
-   [步驟 2：備份自訂屬性存放區](#step-2-back-up-custom-attribute-stores)  
  
-   [步驟 3：備份網頁的自訂項目](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>步驟 1：匯出服務設定  
 若要匯出服務設定，請執行下列程序：  
  
### <a name="to-export-service-settings"></a>若要匯出服務設定  
  
1.  記錄憑證主體名稱與指紋的值同盟服務使用 SSL 憑證。 若要尋找 SSL 憑證，開放網際網路服務 (IIS) 管理主控台中，選取**網站預設**在左窗格中，按一下 [**繫結...** 在**動作**窗格中，尋找並選取 https 繫結，按一下 [**編輯**，然後按一下 [**檢視**。  
  
> [!NOTE]
>  或者，您也可以匯出 SSL）憑證及其私密金鑰.pfx 檔案。 如需詳細資訊，請查看[匯出私人鍵部分伺服器驗證憑證的](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。  
>   
>  這個步驟是選擇性的因為此憑證會儲存在本機電腦個人化憑證存放區中，並將會保留在升級作業系統。  
  
2.  匯出權杖簽署、權杖加密或服務通訊的憑證，都不內部由 AD FS 按鍵。  
  
您可以檢視所有使用 Windows PowerShell 中的伺服器上 AD FS 使用的憑證。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以檢視所有的憑證會使用您的伺服器上的`PSH:>Get-ADFSCertificate`。 這個命令的輸出包括 StoreLocation 和 StoreName 值指定每個憑證存放區的位置。  
  
> [!NOTE]
>  （選擇性）您可以使用中的指導[匯出私人鍵部分伺服器驗證憑證的](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)，將每個憑證及私密金鑰匯出至.pfx 檔案。 因為在作業系統升級過程中保留所有外部憑證，是選擇性的此步驟。  
  
3.  備份應用程式的設定檔。 在其他設定，此檔案包含原則資料庫連接字串。  
  
若要備份應用程式的設定檔，您必須手動複製`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`檔案備份伺服器在安全的位置。  
  
> [!NOTE]
>  錄製 SQL Server 連接之後字串」policystore 連接字串 =」下列檔案：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。 還原原始 AD FS 伺服器上的設定聯盟時，您需要這個字串。  
  
4.  記錄 AD FS 2.0 同盟服務 account 的身分，這 account 的密碼。  
  
若要尋找的身分值，請檢查**登入以**欄的**AD FS 2.0 Windows 服務**中**服務**主機和記錄值，以手動方式。  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>步驟 2：備份自訂屬性存放區  
 您可以使用 Windows PowerShell 來使用 AD FS 找到自訂屬性存放區的相關資訊。 打開 Windows PowerShell 並執行下列命令新增至您的 Windows PowerShell 工作階段的 AD FS cmdlet: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以尋找自訂屬性存放區的相關資訊：`PSH:>Get-ADFSAttributeStore`。 步驟升級，或者移轉自訂屬性存放區而有所不同。  
  
## <a name="step-3-back-up-webpage-customizations"></a>步驟 3：備份網頁的自訂項目  
 若要備份的任何網頁自訂項目，複製 AD FS 網頁和**web.config**檔案從 [對應至 virtual 路徑 directory **」日 adfs 日 ls]**在。 根據預設，這是在**%systemdrive%\inetpub\adfs\ls** directory。  
  
## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)