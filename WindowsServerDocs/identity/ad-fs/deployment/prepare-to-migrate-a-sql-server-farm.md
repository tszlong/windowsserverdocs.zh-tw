---
title: 準備遷移 AD FS SQL 伺服器陣列
description: 提供準備將 AD FS server SQL 伺服器陣列遷移至 Windows Server 2012 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: ef166adeb2285efbbcaa40cdce44cb53b4411dde
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940617"
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>準備移轉 SQL Server 伺服器陣列
 若要準備將屬於 SQL Server 服務器陣列的 AD FS 2.0 同盟伺服器遷移至 Windows Server 2012，您必須從這些伺服器匯出並備份 AD FS 設定資料。

 若要匯出 AD FS 設定資料，請執行下列工作：

-   [步驟1：匯出服務設定](#step-1-export-service-settings)

-   [步驟2：備份自訂屬性存放區](#step-2-back-up-custom-attribute-stores)

-   [步驟3：備份網頁自訂專案](#step-3-back-up-webpage-customizations)

## <a name="step-1-export-service-settings"></a>步驟1：匯出服務設定
 若要匯出服務設定，請執行下列程序：

### <a name="to-export-service-settings"></a>匯出服務設定

1.  記錄 Federation Service 所使用之 SSL 憑證的憑證主體名稱和憑證指紋值。 若要尋找 SSL 憑證，請開啟 Internet Information Services (IIS) 管理主控台，選取左窗格中的 [**預設的網站**]，然後按一下 [系結 **...** ] 在 [**動作**] 窗格中，尋找並選取 HTTPs 系結，按一下 [**編輯**]，然後按一下 [ **View**]。

> [!NOTE]
>  或者，您也可以將 SSL 憑證以及其私密金鑰匯出至 .pfx 檔案。 如需詳細資訊，請參閱 [Export the Private Key Portion of a Server Authentication Certificate](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)。
>
>  這個步驟是選擇性的，因為此憑證儲存在本機電腦個人憑證存放區中，作業系統升級時會予以保留。

2. 匯出不是在 AD FS 內部產生的權杖簽署、權杖加密或服務通訊憑證及金鑰。

您可以使用 Windows PowerShell 來檢視伺服器上 AD FS 使用的所有憑證。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令，以查看您的伺服器上使用的所有憑證 `PSH:>Get-ADFSCertificate` 。 這個命令的輸出包括指定每個憑證存放區位置的 StoreLocation 與 StoreName 值。

> [!NOTE]
>  或者，您可以使用[匯出伺服器驗證憑證的私用金鑰部分](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md)中的指導方針，將每個憑證及其私密金鑰匯出至 .pfx 檔案。 這個步驟是選擇性的，因為作業系統升級期間會保留所有外部憑證。

3. 備份應用程式設定檔。 在其他設定中，這個檔案包含原則資料庫連接字串。

若要備份應用程式設定檔，必須手動將 `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 檔案複製到備份伺服器上安全的位置。

> [!NOTE]
>  在下列檔案中，記錄 "policystore connectionstring =" 之後的 SQL Server 連接字串： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` 。 當您還原同盟伺服器上的原始 AD FS 設定時，會需要此字串。

4. 記錄 AD FS 2.0 Federation Service 帳戶的身分識別和此帳戶的密碼。

若要尋找身分識別值，請檢查 [服務]**** 主控台中 [AD FS 2.0 Windows 服務]**** 的 [登入身分]**** 欄位，然後手動記錄這個值。

## <a name="step-2-back-up-custom-attribute-stores"></a>步驟2：備份自訂屬性存放區
 您可以使用 Windows PowerShell 命令，尋找 AD FS 使用的自訂屬性存放區的相關資訊。 開啟 Windows PowerShell 並執行下列命令，將 AD FS Cmdlet 新增至 Windows PowerShell 工作階段： `PSH:>add-pssnapin “Microsoft.adfs.powershell”`。 然後執行下列命令來尋找自訂屬性存放區的相關資訊： `PSH:>Get-ADFSAttributeStore` 。 升級或移轉自訂屬性存放區的步驟會有所不同。

## <a name="step-3-back-up-webpage-customizations"></a>步驟3：備份網頁自訂專案
 若要備份任何網頁的自訂項目，請複製 AD FS 網頁和 **web.config** 檔案，它位於與 IIS 中虛擬路徑 **“/adfs/ls”** 對應的目錄中。 根據預設，該檔案位於 **%systemdrive%\inetpub\adfs\ls** 目錄。

## <a name="next-steps"></a>後續步驟
 [準備遷移 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)[準備遷移 AD FS 2.0 同盟伺服器 proxy](prepare-to-migrate-ad-fs-fed-proxy.md) [遷移 AD FS 2.0 同盟](migrate-the-ad-fs-fed-server.md)伺服器遷移[AD FS 2.0 同盟伺服器 proxy](migrate-the-ad-fs-2-fed-server-proxy.md) [遷移 AD FS 1.1 Web 代理](migrate-the-ad-fs-web-agent.md)程式