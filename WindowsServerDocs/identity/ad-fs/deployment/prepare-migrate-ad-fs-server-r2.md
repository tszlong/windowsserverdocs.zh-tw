---
title: 準備移轉 AD FS 2.0 同盟伺服器至 Windows Server 2012 R2 上的 AD FS
description: 提供已準備好移轉到 Windows Server 2012 R2 的 AD FS 伺服器的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cb301d0d68f00625ccea8c11d315b9defffe40f3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444531"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>準備移轉 AD FS 2.0 同盟伺服器至 Windows Server 2012 R2 上的 AD FS

本文件說明如何將 AD FS 2.0 或 Windows Server 2012 的同盟伺服器陣列移轉至 Windows Server 2012 R2 AD FS 伺服器陣列。  步驟可以搭配 AD FS 伺服器陣列使用 WID 或 SQL Server 為基礎的資料庫。  
  
-   [移轉程序概述](prepare-migrate-ad-fs-server-r2.md#migration-process-outline)  
  
-   [Windows Server 2012 R2 中的新 AD FS 功能](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [Windows Server 2012 R2 中 AD FS 需求](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [增加您的 Windows PowerShell 限制](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [其他移轉工作和考量](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>移轉程序概述

 若要完成從 AD FS 同盟伺服器陣列至 Windows Server 2012 R2 的移轉，您必須完成下列工作：  
  
1.  匯出、記錄和備份現有 AD FS 陣列中的下列設定資料。 如需如何完成這些工作的詳細指示，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
下列設定的移轉與中 Windows Server 2012 R2 安裝 cd \support\adfs 資料夾的指令碼：  
  
-   宣告提供者信任，但 Active Directory 宣告提供者信任的自訂宣告規則除外。 如需詳細資訊，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
-   信賴憑證者信任。  
  
-   AD FS 會內部產生、自我簽署權杖簽署和權杖解密憑證。  
  
下列任何自訂設定必須手動移轉：  
  
- 服務設定：  
  
  - 由企業或公開憑證授權單位所發出的非預設權杖簽署和權杖解密憑證。  
  
  - AD FS 使用的 SSL 伺服器驗證憑證。  
  
  - AD FS 使用的服務通訊憑證 (根據預設，這和 SSL 憑證的憑證相同)。  
  
    -   任何 Federation Service 屬性的非預設值，例如 AutoCertificateRollover 或 SSO 存留期。  
  
    -   非預設的 AD FS 端點設定和宣告描述。  
  
-   Active Directory 宣告提供者信任的自訂宣告規則。  
  
    -   AD FS 登入頁面自訂  
  
如需詳細資訊，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
2. 建立一個 Windows Server 2012 R2 同盟伺服器陣列。  
  
3. 將原始設定資料匯入到這個新的 Windows Server 2012 R2 AD FS 陣列。  
  
4. 設定和自訂 AD FS 登入頁面。  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 的新功能  
 變更影響 Windows Server 2012 R2 中的下列 AD FS 功能從 AD FS 2.0 或 Windows Server 2012 中的 AD FS 移轉：  
  
**IIS 相依性**  
   - Windows Server 2012 R2 中的 AD FS 是自我裝載的，不需安裝 IIS。 因為此變更的緣故，請務必注意下列事項：  
   -   現在必須透過 Windows PowerShell 執行 AD FS 陣列中同盟伺服器和 Proxy 電腦的 SSL 憑證管理。  
  
**變更 AD FS 登入頁面的設定和自訂**  
-   在 Windows Server 2012 R2 中 AD FS 中，有幾項變更，目的是要改進系統管理員和使用者的登入體驗。 存在於舊版 AD FS 中由 IIS 裝載的網頁現在已移除。 AD FS 登入網頁的外觀與操作方式是自我裝載於 AD FS 中，而且現在可加以自訂來量身打造使用者經驗。 這些變更包括：  
    -   自訂 AD FS 登入經驗，包括自訂公司名稱、標誌、圖例和登入描述。  
    -   自訂錯誤訊息。  
    -   自訂 ADFS 主領域探索經驗，其中包括下列項目：  
        -   設定您的身分識別提供者以使用特定電子郵件尾碼。  
        -   設定每個信賴憑證者的身分識別提供者清單。  
        -   略過內部網路的主領域探索。  
        -   建立自訂 Web 佈景主題。  
  
如需設定 AD FS 登入頁面的外觀及操作的詳細指示，請參閱[Customizing the AD FS sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
如果您有 web 頁面自訂您想要移轉到 Windows Server 2012 R2 的現有 AD FS 伺服器陣列中時，您可以重新建立它們做為 Windows Server 2012 R2 中使用新的自訂功能的移轉程序的一部分。  
  
-   **其他變更**  
  
    -   Windows Server 2012 R2 中的 AD FS 以在 Windows Identity Foundation (WIF) 3.5 中，不是 WIF 4.5。 因此，在 Windows Server 2012 R2 中的 AD FS 不支援 WIF 4.5 （例如，Kerberos 宣告和動態存取控制） 的一些特定功能。  
  
    -   連接埠 443 上的裝置註冊服務 (DRS) 設定 Windows Server 2012 R2 中運作連接埠 49443 上的使用者憑證驗證的 ClientTLS 運作  
  
        -   若為使用憑證傳輸模式驗證的主動、非瀏覽器用戶端，會特別進行硬式編碼以指向連接埠 443，程式碼的變更必須在連接埠 49443 上繼續使用使用者憑證驗證。  
  
        -   若為被動應用程式，則不需要做任何變更，因為 AD FS 會導向至使用者憑證驗證的正確連接埠。  
  
        -   用戶端和 Proxy 之間的防火牆連接埠必須讓使用者憑證驗證的連接埠 49443 流量通過。  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中的 AD FS 需求  
 若要將 AD FS 陣列成功移轉到 Windows Server 2012 R2 中，您必須符合下列需求：  
  
 適用於 AD FS 函式，您想要同盟的每一部電腦必須加入網域。  
  
 適用於 Windows Server 2012 R2 上執行函式的 AD FS，您的 Active Directory 網域必須執行下列其中一項：  
  
- Windows Server 2012 R2  
  
- Windows Server 2012  
  
- Windows Server 2008 R2  
  
- Windows Server 2008  
  
  如果您打算使用群組受控服務帳戶 (gMSA) 做為服務帳戶，適用於 AD FS，您必須至少一個網域控制站執行 Windows Server 2012 或 Windows Server 2012 R2 作業系統的環境中。  
  
  如果您打算部署 AD Workplace Join 的裝置註冊服務 (DRS)，AD FS 部署的一部分時，AD DS 架構必須更新為 Windows Server 2012 R2 等級。 更新結構描述有三種方法：  
  
1.  在現有的 Active Directory 樹系中，從 Windows Server 2012 R2 作業系統 DVD 的 \support\adprep 資料夾在任何執行 Windows Server 2008 或更新版本的 64 位元伺服器上執行 adprep /forestprep。 在這種情況下，不需要安裝其他網域控制站，而且也不需要升級現有的網域控制站。  
  
您必須是裝載架構主機之網域中的 Schema Admins 群組、Enterprise Admins 群組及 Domain Admins 群組的成員，才可以執行 adprep/forestprep。  
  
2. 在現有的 Active Directory 樹系中安裝執行 Windows Server 2012 R2 的網域控制站。 在這種情況下，adprep /forestprep 會在安裝網域控制站時自動執行。  
  
在安裝網域控制站期間，您可能需要指定其他認證才能執行 adprep /forestprep。  
  
3. 在執行 Windows Server 2012 R2 的伺服器上安裝 AD DS 來建立新的 Active Directory 樹系。 在此情況下，adprep /forestprep 不必執行，因為具有所有必要的容器和物件以支援 DRS 會一開始建立結構描述。  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>SQL Server 支援 Windows Server 2012 R2 的 AD FS  
 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。  
  
##  <a name="increasing-your-windows-powershell-limits"></a>增加您的 Windows PowerShell 限制  
 如果您的 AD FS 陣列中有超過 1000 個以上的宣告提供者信任和信賴憑證者信任，或是如果您嘗試執行 AD FS 移轉匯出/匯入工具時看到下列錯誤，則必須增加您的 Windows PowerShell 限制：  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 因為 Windows PowerShell 工作階段預設記憶體限制太低，所以發生此錯誤。 Windows PowerShell 2.0 的工作階段預設記憶體大小為 150MB。 在 Windows PowerShell 3.0 中，工作階段預設記憶體是 1024 MB。 您可以使用下列命令來驗證 Windows PowerShell 遠端工作階段記憶體限制：`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`。 您可以透過執行下列命令來增加限制：`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`。  
  
## <a name="other-migration-tasks-and-considerations"></a>其他移轉工作和考量  
 為了將 AD FS 陣列成功移轉到 Windows Server 2012 R2，務必注意下列事項：  
  
-   位於 Windows Server 2012 R2 安裝 cd 的 \support\adfs 資料夾移轉指令碼會要求您保留相同的同盟伺服器陣列名稱，當您將它移轉到 Windows，您使用舊版 AD FS 伺服器陣列中的服務帳戶身分識別名稱Server 2012 R2。  
  
-   如果您要移轉 SQL Server AD FS 陣列，請注意，移轉程序包含建立一個新的 SQL 資料庫執行個體，您必須將原始設定資料匯入其中。  
  
## <a name="next-steps"></a>後續步驟
 [將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [移轉 AD FS 同盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
 [驗證 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)