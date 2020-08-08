---
title: 準備將 AD FS 2.0 同盟伺服器遷移至 Windows Server 2012 R2 上的 AD FS
description: 提供準備將 AD FS 伺服器遷移至 Windows Server 2012 R2 的相關資訊。
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: c515389ae941a8ade88ebbde37a31c97272c0d25
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940639"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>準備將 AD FS 2.0 同盟伺服器遷移至 Windows Server 2012 R2 上的 AD FS

本檔說明如何將 AD FS 2.0 或 Windows Server 2012 同盟伺服器陣列遷移至 Windows Server 2012 R2 AD FS 伺服器陣列。  這些步驟可搭配使用 WID 或 SQL Server 作為基礎資料庫的 AD FS 伺服器陣列使用。

-   [移轉程序概述](prepare-migrate-ad-fs-server-r2.md#migration-process-outline)

-   [Windows Server 2012 R2 中 AD FS 的新功能](prepare-migrate-ad-fs-server-r2.md#new-ad-fs-functionality-in-windows-server-2012-r2)

-   [Windows Server 2012 R2 中的 AD FS 需求](prepare-migrate-ad-fs-server-r2.md#ad-fs-requirements-in-windows-server-2012-r2)

-   [增加您的 Windows PowerShell 限制](prepare-migrate-ad-fs-server-r2.md#increasing-your-windows-powershell-limits)

-   [其他移轉工作和考量](prepare-migrate-ad-fs-server-r2.md#other-migration-tasks-and-considerations)

##  <a name="migration-process-outline"></a>移轉程序概述

 若要完成從 AD FS 同盟伺服器陣列至 Windows Server 2012 R2 的移轉，您必須完成下列工作：

1.  匯出、記錄和備份現有 AD FS 陣列中的下列設定資料。 如需如何完成這些工作的詳細指示，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。

下列設定會與位於 Windows Server 2012 R2 安裝光碟上 \support\adfs 資料夾中的腳本一起遷移：

-   宣告提供者信任，但 Active Directory 宣告提供者信任的自訂宣告規則除外。 如需詳細資訊，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。

-   信賴憑證者信任。

-   AD FS 會內部產生、自我簽署權杖簽署和權杖解密憑證。

下列任何自訂設定必須手動移轉：

- 服務設定：

  - 由企業或公開憑證授權單位所發出的非預設權杖簽署和權杖解密憑證。

  - AD FS 使用的 SSL 伺服器驗證憑證。

  - AD FS 使用的服務通訊憑證 (根據預設，這和 SSL 憑證的憑證相同)。

    -   任何 Federation Service 屬性的非預設值，例如 AutoCertificateRollover 或 SSO 存留期。

    -   非預設 AD FS 端點設定和宣告描述。

-   Active Directory 宣告提供者信任的自訂宣告規則。

    -   AD FS 登入頁面自訂

如需詳細資訊，請參閱[移轉 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)。

2. 建立一個 Windows Server 2012 R2 同盟伺服器陣列。

3. 將原始設定資料匯入到這個新的 Windows Server 2012 R2 AD FS 陣列。

4. 設定和自訂 AD FS 登入頁面。

##  <a name="new-ad-fs-functionality-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中 AD FS 的新功能
 Windows Server 2012 R2 中的下列 AD FS 功能變更會影響 Windows Server 2012 中 AD FS 2.0 或 AD FS 的遷移：

**IIS 相依性**
   - Windows Server 2012 R2 中的 AD FS 是自我裝載的，不需安裝 IIS。 因為此變更的緣故，請務必注意下列事項：
   -   現在必須透過 Windows PowerShell 執行 AD FS 陣列中同盟伺服器和 Proxy 電腦的 SSL 憑證管理。

**AD FS 登入頁面的設定和自訂變更**
-   在 Windows Server 2012 R2 的 AD FS 中，有幾項變更是為了改善系統管理員和使用者的登入體驗。 存在於舊版 AD FS 中由 IIS 裝載的網頁現在已移除。 AD FS 登入網頁的外觀與操作方式是自我裝載於 AD FS 中，而且現在可加以自訂來量身打造使用者經驗。 這些變更包括：
    -   自訂 AD FS 登入經驗，包括自訂公司名稱、標誌、圖例和登入描述。
    -   自訂錯誤訊息。
    -   自訂 ADFS 主領域探索經驗，其中包括下列項目：
        -   設定您的身分識別提供者以使用特定電子郵件尾碼。
        -   設定每個信賴憑證者的身分識別提供者清單。
        -   略過內部網路的主領域探索。
        -   建立自訂 Web 佈景主題。

如需設定 AD FS 登入頁面外觀與風格的詳細指示，請參閱[自訂 AD FS 登入頁面](../operations/ad-fs-customization-in-windows-server.md)。

如果您在現有 AD FS 伺服器陣列中有要遷移到 Windows Server 2012 R2 的網頁自訂，您可以使用 Windows Server 2012 R2 中的新自訂功能，在遷移程式中重新建立它們。

-   **其他變更**

    -   Windows Server 2012 R2 中的 AD FS 是以 Windows Identity Foundation 為基礎， (WIF) 3.5，而不是 WIF 4.5。 因此，Windows Server 2012 R2 中的 AD FS 不支援 WIF 4.5 (的某些特定功能，例如，Kerberos 宣告和動態存取控制) 。

    -   Windows Server 2012 R2 中的裝置註冊服務 (DRS) 會在埠443上運作;使用者憑證驗證的 Clienttls 是會在埠49443上運作

        -   若為使用憑證傳輸模式驗證的主動、非瀏覽器用戶端，會特別進行硬式編碼以指向連接埠 443，程式碼的變更必須在連接埠 49443 上繼續使用使用者憑證驗證。

        -   若為被動應用程式，則不需要做任何變更，因為 AD FS 會導向至使用者憑證驗證的正確連接埠。

        -   用戶端和 Proxy 之間的防火牆連接埠必須讓使用者憑證驗證的連接埠 49443 流量通過。

##  <a name="ad-fs-requirements-in-windows-server-2012-r2"></a>Windows Server 2012 R2 中的 AD FS 需求
 若要成功地將您的 AD FS 伺服器陣列遷移至 Windows Server 2012 R2，您必須符合下列需求：

 若要讓 AD FS 運作，您要成為同盟的每部電腦都必須加入網域。

 若要讓在 Windows Server 2012 R2 上執行的 AD FS 能夠正常運作，您的 Active Directory 網域必須執行下列其中一項：

- Windows Server 2012 R2

- Windows Server 2012

- Windows Server 2008 R2

- Windows Server 2008

  如果您打算使用群組受管理的服務帳戶 (gMSA) 做為 AD FS 的服務帳戶，則在 Windows Server 2012 或 Windows Server 2012 R2 作業系統上執行的環境中至少必須有一個網域控制站。

  如果您計畫將裝置註冊服務部署 (DRS Workplace Join) 作為 AD FS 部署的一部分，則必須將 AD DS 架構更新為 Windows Server 2012 R2 層級。 更新結構描述有三種方法：

1.  在現有的 Active Directory 樹系中，從執行 Windows Server 2008 或更新版本的任何64位伺服器上的 Windows Server 2012 R2 作業系統 DVD 的 \support\adprep 資料夾執行 adprep/forestprep。 在這種情況下，不需要安裝其他網域控制站，而且也不需要升級現有的網域控制站。

您必須是裝載架構主機之網域中的 Schema Admins 群組、Enterprise Admins 群組及 Domain Admins 群組的成員，才可以執行 adprep/forestprep。

2. 在現有的 Active Directory 樹系中，安裝執行 Windows Server 2012 R2 的網域控制站。 在這種情況下，adprep /forestprep 會在安裝網域控制站時自動執行。

在安裝網域控制站期間，您可能需要指定其他認證才能執行 adprep /forestprep。

3. 在執行 Windows Server 2012 R2 的伺服器上安裝 AD DS，以建立新的 Active Directory 樹系。 在這種情況下，不需要執行 adprep /forestprep，因為一開始會使用支援 DRS 的所有必要容器和物件建立結構描述。

### <a name="sql-server-support-for-ad-fs-in-windows-server-2012-r2"></a>SQL Server 支援 Windows Server 2012 R2 的 AD FS
 如果您要建立一個 AD FS 陣列並使用 SQL Server 儲存您的設定資料，可以使用 SQL Server 2008 和更新版本，包括 SQL Server 2012。

##  <a name="increasing-your-windows-powershell-limits"></a>增加您的 Windows PowerShell 限制
 如果您的 AD FS 陣列中有超過 1000 個以上的宣告提供者信任和信賴憑證者信任，或是如果您嘗試執行 AD FS 移轉匯出/匯入工具時看到下列錯誤，則必須增加您的 Windows PowerShell 限制：

```
'Exception of type 'System.OutOfMemoryException' was thrown. At E:\dev\ds\security\ADFSv2\Product\Migration\Export-FederationConfiguration.ps1:176 char:21 + $configData = Invoke-Command -ScriptBlock $GetConfig -Argume ...
```

 因為 Windows PowerShell 工作階段預設記憶體限制太低，所以發生此錯誤。 Windows PowerShell 2.0 的工作階段預設記憶體大小為 150MB。 在 Windows PowerShell 3.0 中，會話預設記憶體為1024MB。 您可以使用下列命令來驗證 Windows PowerShell 遠端工作階段記憶體限制：`Get-Item wsman:localhost\Shell\MaxMemoryPerShellMB`。 您可以透過執行下列命令來增加限制：`Set-Item wsman:localhost\Shell\MaxMemoryPerShellMB 512`。

## <a name="other-migration-tasks-and-considerations"></a>其他移轉工作和考量
 為了將 AD FS 陣列成功移轉到 Windows Server 2012 R2，務必注意下列事項：

-   位於 Windows Server 2012 R2 安裝光碟上 \support\adfs 資料夾中的遷移腳本，會要求您保留相同的同盟伺服器陣列名稱和服務帳戶身分識別名稱，以供您將它遷移至 Windows Server 2012 R2 時在舊版 AD FS 伺服器陣列中使用。

-   如果您要移轉 SQL Server AD FS 陣列，請注意，移轉程序包含建立一個新的 SQL 資料庫執行個體，您必須將原始設定資料匯入其中。

## <a name="next-steps"></a>後續步驟
 [將 Active Directory 同盟服務角色服務遷移至 Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md) [遷移 AD FS 同盟伺服器](migrate-ad-fs-fed-server-r2.md)[遷移 AD FS 同盟伺服器 Proxy](migrate-fed-server-proxy-r2.md) [驗證 AD FS 遷移至 windows server 2012 R2](verify-ad-fs-migration.md)
