---
title: 移轉獨立的 AD FS 同盟伺服器或單一節點 AD FS 伺服器陣列
description: 提供有關移轉到 Windows Server 2012 的獨立單獨或單一節點 AD FS 2.0 伺服器
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851929"
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>移轉獨立的 AD FS 同盟伺服器或單一節點 AD FS 伺服器陣列  
本文件提供有關移轉到 Windows Server 2012 的 AD FS 2.0 的獨立伺服器的詳細的資訊。

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>移轉獨立的 AD FS 2.0 伺服器

使用下列程序將 AD FS 2.0 伺服器移轉到 Windows Server 2012。
  
1.  檢閱並執行中的程序[準備移轉獨立的 AD FS 同盟伺服器或單一節點 AD FS 伺服器陣列](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md)。  
  
2.  您的伺服器從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上執行作業系統就地的升級。 如需相關資訊，請參閱[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定會遺失且 AD FS 2.0 伺服器角色會被移除。 相反地，安裝 Windows Server 2012 的 AD FS 伺服器角色，但尚未進行設定。 您必須手動建立原始 AD FS 設定，並還原剩餘的 AD FS 設定，來完成同盟伺服器移轉。  
  
3.  建立原始 AD FS 設定。 您可以使用其中一種下列方法建立原始 AD FS 設定：  
  
-   使用**AD FS 同盟伺服器設定精靈**來建立新的同盟伺服器。 如需詳細資訊，請參閱[在同盟伺服器陣列中建立第一個同盟伺服器](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
當您執行精靈時，使用您準備要移轉 AD FS 同盟伺服器時所收集的資訊，如下所示：  
  
 |**同盟伺服器設定精靈輸入選項**|**使用下列值**| 
|-----|-----| 
|[指定 Federation Service 名稱] 頁面上的 [SSL 憑證]|選取您準備 AD FS 同盟伺服器移轉時所記錄之主體名稱和憑證指紋的 SSL 憑證。|  
|[指定服務帳戶] 頁面上的 [服務帳戶] 和 [密碼]|輸入您準備 AD FS 同盟伺服器移轉時所記錄的服務帳戶資訊。 **注意：** 如果您在精靈的第二頁上選取獨立的同盟伺服器時，會自動為服務帳戶使用網路服務。|  
  
> [!IMPORTANT] 
> 只有當您使用 Windows 內部資料庫 (WID) 來儲存獨立的同盟伺服器或單一節點 AD FS 伺服器陣列的 AD FS 設定資料庫，您可以使用此方法。  
>
>  如果您使用 SQL Server 來儲存您的單一節點 AD FS 伺服器陣列的 AD FS 設定資料庫，您必須使用 Windows PowerShell 來建立原始 AD FS 設定同盟伺服器上。  
  
-   使用 Windows PowerShell  
  
> [!IMPORTANT]
>  如果您使用 SQL Server 來儲存獨立的同盟伺服器或單一節點 AD FS 伺服器陣列的 AD FS 設定資料庫，您必須使用 Windows PowerShell。  
  
以下是如何在單一節點 SQL Server 伺服器陣列的同盟伺服器上建立原始 AD FS 設定使用 Windows PowerShell 的範例。  開啟 Windows PowerShell 模組，然後執行下列命令： `$fscredential = Get-Credential`。 輸入準備移轉 SQL Server 伺服器陣列時所記錄之服務帳戶的名稱和密碼。 然後執行下列命令：`C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`何處`Data Source`是在原則存放區連接字串值，在下列檔案中的資料來源值： `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
4.  還原剩餘的 AD FS 服務設定及信任關係。 這是手動步驟，在這期間您可以使用準備 AD FS 移轉時所匯出的檔案及收集的值。 如需詳細指示，請參閱「還原剩餘的 AD FS 伺服器陣列設定」。  
  
> [!NOTE]
>  只有在移轉獨立的同盟伺服器或單一節點 WID 伺服器陣列時，才需要執行此步驟。  如果同盟伺服器使用 SQL Server 資料庫做為設定存放區，則服務設定和信任關係會保留在資料庫中。  
  
5.  更新您的 AD FS 網頁。 這是手動步驟。 如果您準備移轉時備份您自訂 AD FS 網頁，請使用備份資料來覆寫預設會在所建立的 AD FS 網頁 **%systemdrive%\inetpub\adfs\ls**的目錄在 Windows Server 2012 AD FS 設定。  
  
6.  還原任何剩餘的 AD FS 自訂項目，例如自訂屬性存放區。  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>還原剩餘的 AD FS 伺服器陣列組態  
  
-   將下列 AD FS 服務設定還原為單一節點 WID 陣列或獨立的 Federation Service，如下所示：  
  
    -   在 AD FS 管理主控台中，選取 [服務]，然後按一下 [編輯 Federation Service]。 將每個值與移轉準備期間匯出到 properties.txt 檔案的值進行比對，來檢查 Federation Service 設定：  
  
    
|**Get-adfsproperties 報告的 federation Service 屬性名稱**|**在 AD FS 管理主控台中的 federation Service 屬性名稱**|  
|-----|-----|
|DisplayName|Federation Service 顯示名稱|  
|HostName|Federation Service 名稱|  
|識別碼|Federation Service 識別碼|  
  
-   在 AD FS 管理主控台，選取 [憑證]。 將每個值與移轉準備期間匯出到 certificates.txt 檔案中的值進行比對，來檢查服務通訊、權杖解密及權杖簽署憑證。  
  
若要將權杖解密或權杖簽署憑證從預設的自我簽署憑證變更為外部憑證，您必須先停用預設啟用的自動憑證變換功能。  若要這樣做，您可以使用下列 Windows PowerShell 命令： `PSH: Set-ADFSProperties –AutoCertificateRollover $false`。  
  
-   在 AD FS 管理主控台，選取 [端點]。 將啟用的 AD FS 端點清單與 AD FS 移轉準備期間匯出至檔案的啟用 AD FS 端點清單進行比對。  
  
-   在 AD FS 管理主控台，選取 [宣告描述] 。 將 AD FS 宣告描述清單與 AD FS 移轉準備期間匯出至檔案的宣告描述清單進行比對。 新增任何包含於您的檔案但未包含於 AD FS 中預設清單的自訂宣告描述。  請注意，管理主控台的宣告識別碼會對應至檔案的 ClaimType。  如需新增宣告描述的詳細資訊，請參閱[新增宣告描述](../operations/add-a-claim-description.md)。 如需詳細資訊，請參閱＜準備移轉 AD FS 2.0 同盟伺服器＞的「步驟 1 - 匯出服務設定」一節。  
  
-   在 AD FS 管理主控台，選取 [宣告提供者信任]。 您必須使用 [新增宣告提供者信任精靈]，手動重新建立每個宣告提供者信任。  使用 AD FS 準備移轉期間所匯出和記錄的宣告提供者信任的清單。 您可以忽略檔案中識別碼 “AD AUTHORITY” 的宣告提供者信任，因為這是屬於預設 AD FS 設定中一部分的 “Active Directory” 宣告提供者信任。  不過，請檢查移轉前已新增至 Active Directory 信任的任何自訂宣告規則。 如需建立宣告提供者信任的詳細資訊，請參閱[使用同盟中繼資料建立宣告提供者信任](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata)或[手動建立宣告提供者信任](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually)。  
  
-   在 AD FS 管理主控台，選取 [信賴憑證者信任]。 您必須使用 [新增信賴憑證者信任精靈]，手動重新建立每個信賴憑證者信任。 使用 AD FS 準備移轉期間所匯出和記錄的信賴憑證者信任的清單。 如需建立宣告提供者信任的詳細資訊，請參閱[使用同盟中繼資料建立信賴憑證者信任](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)或[手動建立信賴憑證者信任](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)。 

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 同盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [準備移轉 AD FS 2.0 同盟伺服器 Proxy](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 同盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 同盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 網路代理程式](migrate-the-ad-fs-web-agent.md)




-   
-    