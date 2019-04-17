---
title: "AD FS 聯盟獨立伺服器或單一節點 AD FS 發電廠移轉"
description: "提供資訊移轉獨立或單一節點 AD FS 2.0 伺服器 Windows Server 2012"
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 10371349fe19be92fb997c9c28f19def0ecad7e9
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>AD FS 聯盟獨立伺服器或單一節點 AD FS 發電廠移轉  
本文件會提供 AD FS 2.0 獨立伺服器移轉到 Windows Server 2012 上的詳細的資訊。

## <a name="migrate-a-stand-alone-ad-fs-20-server"></a>移轉獨立 AD FS 2.0 伺服器

使用下列程序將 AD FS 2.0 移轉到 Windows Server 2012 的伺服器。
  
1.  檢視並執行中的程序[準備移轉獨立 AD FS 聯盟伺服器或單一節點 AD FS 發電廠](prepare-to-migrate-a-stand-alone-ad-fs-federation-server.md)。  
  
2.  伺服器從 Windows Server 2008 R2 或 Windows Server 2008 到 Windows Server 2012 上執行作業系統的就地升級。 如需詳細資訊，請查看[安裝 Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx)。  
  
> [!IMPORTANT]
>  作業系統升級的結果，在此伺服器上的 AD FS 設定將會遺失，並且移除 AD FS 2.0 伺服器角色。 Windows Server 2012 AD FS 伺服器角色已安裝改為，但未設定。 您必須手動建立原始設定，AD FS，並還原剩餘 AD FS 設定完成聯盟伺服器移轉。  
  
3.  建立原始設定，AD FS。 您可以建立原始設定，AD FS 使用其中一項下列方法：  
  
-   使用**AD FS 聯盟伺服器設定精靈**來建立新的聯盟伺服器。 如需詳細資訊，請查看[第一個聯盟伺服器建立聯盟伺服器陣列](Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md)。  
  
當您瀏覽精靈中，使用您準備，如下所示移轉 AD FS 聯盟伺服器時所收集的資訊：  
  
 |**聯盟伺服器設定精靈輸入的選項**|**使用下列值**| 
|-----|-----| 
|**SSL 憑證**的**指定同盟服務名稱**頁面|選取您記錄時準備 AD FS 聯盟伺服器移轉主體名稱與指紋的 SSL 憑證。|  
|**服務 account**與**密碼**在**指定服務帳號**頁面|輸入的服務 account 資訊錄製時準備 AD FS 聯盟伺服器移轉。 **注意：**如果您選擇聯盟獨立伺服器精靈中的第二個頁面上，網路服務會自動做服務 account。|  
  
> [!IMPORTANT] 
> 只有在您使用 Windows 內部資料庫 (WID) 來儲存您獨立聯盟伺服器或單一節點 AD FS 陣列 AD FS 設定資料庫，您可以使用此方法。  
>
>  如果您使用 SQL Server 來儲存您的單一節點 AD FS 陣列 AD FS 設定資料庫，您必須使用 Windows PowerShell 來建立原始設定，AD FS 聯盟伺服器上。  
  
-   使用 Windows PowerShell  
  
> [!IMPORTANT]
>  如果您正在使用 SQL Server 的獨立聯盟伺服器或單一節點 AD FS 發電廠儲存 AD FS 設定資料庫，您必須使用 Windows PowerShell。  
  
以下是如何在單一節點 SQL Server 發電廠聯盟伺服器上建立原始設定，AD FS 使用 Windows PowerShell 範例。  打開 Windows PowerShell 模組，並執行下列命令：`$fscredential = Get-Credential`。 輸入名稱及服務帳號錄製時您 SQL server 發電廠準備移轉的密碼。 然後執行下列命令：`C:\PS> Add-AdfsFarmNode -ServiceAccountCredential $fscredential -SQLConnectionString "Data Source=<Data Source>;Integrated Security=True"`在`Data Source`是原則市集連接字串值，在下列檔案中的資料來源值：`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`。  
  
4.  還原剩餘 AD FS 服務設定以及信任關係。 這是手動步驟期間，您可以使用您要匯出的檔案和您 AD FS 移轉準備時所收集的值。 詳細指示，會看到還原剩餘 AD FS 發電廠設定。  
  
> [!NOTE]
>  這個步驟只有需要如果您的移轉獨立聯盟伺服器或單一節點 WID 發電廠。  聯盟伺服器使用 SQL Server 資料庫設定存放區與，如果服務設定和信任關係的資料庫中保留。  
  
5.  更新您 AD FS 網頁。 這是手動的步驟。 如果您的移轉準備時備份您自訂 AD FS 網頁，使用您備份的資料覆寫預設 AD FS 網頁中的預設所建立的**%systemdrive%\inetpub\adfs\ls**目錄根據 AD FS 設定 Windows Server 2012 上。  
  
6.  還原任何剩餘 AD FS 的自訂項目，例如自訂屬性存放區。  
  
## <a name="restoring-the-remaining-ad-fs-farm-configuration"></a>還原剩餘 AD FS 發電廠設定  
  
-   下列 AD FS 服務設定還原至單一節點 WID 或獨立同盟服務，如下所示：  
  
    -   在 AD FS 管理主控台中，選取 [**服務**，按一下 [**編輯同盟服務...**. 檢查每個您要匯出至 properties.txt 檔案時準備移轉的值對值驗證同盟服務設定：  
  
    
|**聯盟服務屬性名稱 Get-ADFSProperties 報告**|**AD FS 管理主控台中同盟服務屬性名稱**|  
|-----|-----|
|顯示名稱|聯盟服務顯示名稱|  
|主機|聯盟服務名稱|  
|識別碼|聯盟服務識別碼|  
  
-   在 AD FS 管理主控台中，選取 [**的憑證**。 確認服務通訊，解密預付碼和權杖簽署的憑證檢查每個針對您要匯出至 certificates.txt 檔案時準備移轉的值。  
  
若要變更的預設自我簽署的憑證外部憑證權杖解密或預付碼簽署的憑證，您必須先停用功能的預設的自動憑證變換功能。  若要這樣做，您可以使用下列的 Windows PowerShell 命令：`PSH: Set-ADFSProperties –AutoCertificateRollover $false`。  
  
-   AD FS 管理主控台中，選取 [**的端點**。 檢查清單中時準備 AD FS 移轉匯出到檔案的讓 AD FS 端點針對讓的 AD FS 結束點。  
  
-   AD FS 管理主控台中，選取 [**宣告描述**。 檢查 AD FS 理賠要求描述清單的理賠要求描述您的檔案匯出準備 AD FS 移轉作業時的清單。 新增包含在您的檔案，但在 [預設清單中 AD FS 不包含任何自訂宣告描述。  請注意宣告識別碼管理主控台中的將檔案 ClaimType 對應。  適用於新增宣告描述的詳細資訊，請查看[需要新增描述取得](../operations/add-a-claim-description.md)。 如需詳細資訊，查看「步驟 1-匯出服務設定」中的區段準備要移轉 AD FS 2.0 聯盟伺服器。  
  
-   AD FS 管理主控台中，選取 [**宣告提供者信任**。 您必須使用以手動方式每個宣告提供者信任重新建立**新增宣告提供者信任精靈**。  使用您匯出和時準備 AD FS 移轉錄製宣告提供者信任的清單。 您可以忽略檔案的識別碼「AD 授權單位」宣告提供者信任因為這是「Active Directory「宣告提供者信任的 AD FS 設定預設的一部分。  不過，檢查您可能已新增至之前移轉的 Active Directory 信任任何自訂宣告規則。 針對上建立的詳細資訊宣告信任提供者，請查看[建立宣告提供者信任使用聯盟中繼資料](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-using-federation-metadata)或[宣告提供者信任手動建立](../operations/create-a-claims-provider-trust.md#to-create-a-claims-provider-trust-manually)。  
  
-   AD FS 管理主控台中，**選取可以廠商信任**。 您必須使用以手動方式每個信賴信任重新建立**新增可以廠商信任精靈**。 使用您匯出和時準備 AD FS 移轉錄製信賴廠商信任的清單。 適用於建立信賴廠商信任的詳細資訊，請查看[建立可以廠商信任使用聯盟中繼資料](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-using-federation-metadata)或[可以廠商信任手動建立](../operations/create-a-relying-party-trust.md#to-create-a-claims-aware-relying-party-trust-manually)。 

## <a name="next-steps"></a>後續步驟
 [準備移轉 AD FS 2.0 聯盟伺服器](prepare-to-migrate-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy 準備](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [移轉 AD FS 2.0 聯盟伺服器](migrate-the-ad-fs-fed-server.md)   
 [移轉 AD FS 2.0 聯盟伺服器 Proxy](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [移轉 AD FS 1.1 Web 代理程式](migrate-the-ad-fs-web-agent.md)




-   
-    