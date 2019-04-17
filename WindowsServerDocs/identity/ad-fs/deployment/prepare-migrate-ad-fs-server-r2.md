---
title: "AD FS 2.0 聯盟伺服器移轉到 Windows Server 2012 R2 上 AD FS 進行準備"
description: "準備好 AD FS 伺服器移轉到 Windows Server 2012 R2 提供相關資訊。"
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4d0ff53b9118db1dd6ba5af94b3e627bf1597e0c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>AD FS 2.0 聯盟伺服器移轉到 Windows Server 2012 R2 上 AD FS 進行準備

本文件告訴您如何將 AD FS 2.0 或 Windows Server 2012 聯盟伺服器陣列移轉到 Windows Server 2012 R2 AD FS 陣列。  步驟可以搭配 AD FS 農場為基礎資料庫中使用 WID 或 SQL Server 中。  
  
-   [移轉處理程序外框](prepare-migrate-ad-fs-server-r2.md#migrate-process-outline)  
  
-   [Windows Server 2012 R2 的新 AD FS 功能](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)  
  
-   [在 Windows Server 2012 R2 AD FS 需求](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)  
  
-   [增加您的 Windows PowerShell 限制](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)  
  
-   [其他移轉工作及注意事項](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)  
  
##  <a name="migration-process-outline"></a>移轉處理程序外框  
 若要完成移轉到 Windows Server 2012 R2 AD FS 聯盟伺服器陣列，您必須完成以下工作：  
  
1.  匯出記錄，並備份您現有的 AD FS 發電廠中的下列設定資料。 詳細指示完成這些工作，請查看[移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
在 Windows Server 2012 R2 安裝光碟 \support\adfs 資料夾中的指令碼的移轉下列設定：  
  
-   宣告信任提供者，但在 Active Directory 宣告提供者信任自訂宣告規則除外。 如需詳細資訊，請查看[移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
-   可以廠商信任。  
  
-   AD FS 內部產生、自我權杖登入及權杖解密憑證。  
  
您必須手動移轉自訂的下列設定：  
  
 -   服務設定：  
  
     -   非預設權杖登入，以及權杖解密企業版或公用憑證授權單位是發行憑證。  
  
     -   AD FS 使用的 SSL 伺服器驗證憑證。  
  
     -   AD FS 使用的服務通訊憑證（根據預設，這是相同的憑證以 SSL 憑證。  
  
      -   適用於任何同盟服務屬性，例如 AutoCertificateRollover 或 SSO 期間非預設值。  
  
      -   非預設 AD FS 端點設定和宣告描述。  
  
-   自訂取得上的 Active Directory 宣告提供者信任規則。  
  
    -   AD FS 登入頁面自訂項目  
  
如需詳細資訊，請查看[移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)。  
  
2.  建立 Windows Server 2012 R2 聯盟伺服器陣列。  
  
3.  這個新的 Windows Server 2012 R2 AD FS 發電廠匯入的原始設定資料。  
  
4.  設定及自訂 AD FS 登入頁面。  
  
##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Windows Server 2012 R2 的新 AD FS 功能  
 AD FS 下列功能在 Windows Server 2012 R2 影響移轉從變更 AD FS 2.0 或 Windows Server 2012 中的 AD FS:  
  
**IIS 相依性**  
   - 在 Windows Server 2012 R2 AD FS 自我裝載並不需要 IIS 安裝。 請確定您注意下列由於這項變更：  
   -   現在必須透過 Windows PowerShell 執行 SSL 憑證管理聯盟伺服器和您 AD FS 發電廠 proxy 電腦。  
  
**變更 AD FS 登入頁面的設定和自訂項目**  
-   在 Windows Server 2012 R2 AD FS，有幾個變更要改善的系統管理員與使用者登入的體驗。 立即移除 IIS 裝載網頁有在舊版的 AD FS。 AD FS 登入之網站的外觀及操作 AD FS 中自我裝載，現在您可以自訂來量身訂做的使用者體驗。 變更包括：  
    -   自訂 AD FS 登入體驗，包括自訂的公司名稱、商標、圖，以及登入的描述。  
    -   自訂的錯誤訊息。  
    -   自訂 ADFS Home 領域探索體驗，其中包括下列項目：  
        -   設定您的身分使用某些電子郵件尾碼的提供者。  
        -   設定可以依據身分提供者清單派對。  
        -   略過 Home 領域探索內部網路。  
        -   建立自訂 web 主題。  
  
設定 AD FS 登入頁面的外觀與感覺詳細指示，請查看[[自訂頁面 AD FS 登入](../operations/AD-FS-Customization-in-Windows-Server-2016.md)。  
  
如果您有您想要移轉到 Windows Server 2012 R2 的現有 AD FS 陣列網頁自訂項目，您可以重新建立他們使用的新的自訂項目功能在 Windows Server 2012 R2 移轉程序的一部分。  
  
-   **其他的變更**  
  
    -   在 Windows Server 2012 R2 AD FS 是以在 Windows 的身分基礎 (WIF) 3.5，不 WIF 4.5 為基礎。 因此，在 Windows Server 2012 R2 AD FS 不支援 WIF 4.5（例如，Kerberos 宣告和動態存取控制）的某些特定功能。  
  
    -   連接埠 443; 上裝置登記服務 (DRS) 在 Windows Server 2012 R2 的運作方式連接埠 49443 ClientTLS 使用者憑證驗證的運作方式  
  
        -   作用中的非瀏覽器用憑證傳輸模式驗證的專門為固定指向連接埠 443，程式碼變更，才能繼續使用使用者憑證驗證 49443 連接埠。  
  
        -   適用於被動式應用程式需要不會變更，因為 AD FS 重新導向至 amc 到正確的使用者憑證驗證連接埠。  
  
        -   Client 與 proxy 之間防火牆連接埠必須讓使用者憑證驗證通過連接埠 49443 流量。  
  
##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 AD FS 需求  
 為了已成功將您 AD FS 發電廠移轉到 Windows Server 2012 R2，您必須符合下列需求：  
  
 AD fs 運作，每個您想要聯盟的電腦必須加入網域。  
  
 才能執行 Windows Server 2012 R2 上 AD fs，您 Active Directory domain 必須執行下列其中一項：  
  
-   Windows Server 2012 R2  
  
-   Windows Server 2012  
  
-   Windows Server 2008 R2  
  
-   Windows Server 2008  
  
 如果您打算使用 AD FS 為服務 account 群組管理服務 Account (gMSA)，您必須至少網域控制站在 Windows Server 2012 或 Windows Server 2012 R2 的作業系統上執行的環境中。  
  
 如果想要部署的工作地點 AD Join 裝置登記服務 (DRS) AD FS 部署的一部分，AD DS 架構需要更新到 Windows Server 2012 R2 層級。 有三種方式，若要更新：  
  
1.  在現有的 Active Directory 樹系執行 adprep /forestprep 從在執行 Windows Server 2008，或更新版本任何 64 位元伺服器上的 Windows Server 2012 R2 作業系統系統 DVD \support\adprep 資料夾。 在這種情形下，任何其他網域控制站需要安裝，且不現有的網域控制站需要升級。  
  
若要執行 adprep 日 forestprep，您必須架構管理員群組、企業系統管理員群組和的成員，主控架構主機的網域系統管理員」群組。  
  
2.  在現有的 Active Directory 樹系安裝執行 Windows Server 2012 R2 網域控制站。 在本案例中，adprep /forestprep 會自動執行為網域控制站安裝的一部分。  
  
網域控制站在安裝期間，您可能需要額外的認證指定以執行 adprep /forestprep。  
  
3.  藉由在執行 Windows Server 2012 R2 的伺服器上安裝 AD DS 建立新的 Active Directory 森林。 在本案例中，adprep /forestprep 不需要執行，因為架構將一開始建立的所有所需的容器和支援 DRS 物件。  
  
### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>在 Windows Server 2012 R2 AD FS SQL Server 支援  
 如果您想要建立 AD FS 發電廠 SQL Server 來儲存您設定的資料的使用，您可以使用 SQL Server 2008 和較新版本，包括 SQL Server 2012。  
  
##  <a name="increasing-your-windows-powershell-limits"></a>增加您的 Windows PowerShell 限制  
 如果您有多個 1000 年宣告提供者信任並信賴信任 AD FS 陣列、中或如果您在嘗試執行 AD FS 移轉/匯出工具時看到以下錯誤，您必須提高您的 Windows PowerShell 限制：  
  
```  
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...  
```  
  
 因為 Windows PowerShell 工作階段預設記憶體限制太低狀況這個錯誤。 Windows PowerShell 2.0，工作階段預設記憶體是 150 MB。 Windows PowerShell 3.0 中的工作階段預設記憶體是 1024 MB。 您可以檢查 Windows PowerShell 工作階段遠端記憶體限制，使用下列命令：`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`。 您可以增加限制，請執行下列命令：`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`。  
  
## <a name="other-migration-tasks-and-considerations"></a>其他移轉工作及注意事項  
 已成功將您 AD FS 發電廠移轉到 Windows Server 2012 R2，以確認您會注意到以下：  
  
-   在 Windows Server 2012 R2 安裝光碟 \support\adfs 資料夾中的移轉指令碼需要您保留相同聯盟伺服器發電廠名稱和時，使用您的舊版 AD FS 陣列在您移轉到 Windows Server 2012 R2 的服務 account 身分名稱。  
  
-   如果您想要移轉 SQL Server AD FS 發電廠，請注意移轉程序，包括建立新 SQL 資料庫執行個體您要必須匯入的原始設定資料。  
  
## <a name="next-steps"></a>後續步驟
 [Active Directory 同盟服務角色服務移轉到 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [移轉 AD FS 聯盟伺服器](migrate-ad-fs-fed-server-r2.md)   
 [移轉 AD FS 聯盟伺服器 Proxy](migrate-fed-server-proxy-r2.md)   
 [檢查 AD FS 移轉到 Windows Server 2012 R2](verify-ad-fs-migration.md)