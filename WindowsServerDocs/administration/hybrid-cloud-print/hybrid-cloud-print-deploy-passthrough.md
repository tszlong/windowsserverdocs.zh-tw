---
title: 部署 Windows Server 混合式雲端列印-傳遞驗證
description: 如何設定 Microsoft 混合式雲端列印使用傳遞驗證
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: 95cced8dc06cc4ee3768addf65e17cf379e20454
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435757"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>部署 Windows Server 混合式雲端列印使用傳遞驗證

>適用於：Windows Server 2016

本主題中，為 IT 系統管理員，說明 Microsoft 混合式雲端列印解決方案的端對端部署。 在執行列印伺服器，以及啟用 Azure Active Directory 已加入，並受 MDM 管理的現有 Windows 伺服器上此方案層級，探索並列印到組織的裝置管理印表機。

## <a name="pre-requisites"></a>必要條件

有幾個的訂用帳戶和服務，您必須取得開始這個安裝之前的電腦。 分別為：

-   Azure AD premium 訂用帳戶
    
    請參閱[開始使用 Azure 訂用帳戶](https://azure.microsoft.com/trial/get-started-active-directory/)，azure 試用版訂用帳戶。 

-   MDM 服務，例如 Intune
    
    請參閱[Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)，intune 試用版訂用帳戶。

-   Windows Server Active Directory 以執行

    請參閱[逐步：設定 Windows Server 2016 中的 Active Directory](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/)，如需設定 Active Directory 的說明。

-   已加入網域的 Windows Server 2016 執行列印伺服器
    
    請參閱[使用新增角色及功能精靈安裝角色、 角色服務與功能](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)，如需如何在 Windows Server 上安裝角色和角色服務。

-   Azure AD Connect
    
    請參閱[的 Azure AD Connect 的自訂安裝](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)，如需設定 Azure AD Connect 的說明。

-   在不同網域的 azure 應用程式 Proxy 連接器已加入 Windows Server 機器
    
    請參閱[開始使用應用程式 Proxy 並安裝連接器](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)，如需設定 Azure 應用程式 Proxy 連接器的說明。

-   公開的網域名稱
    
    您可以使用 Azure，為您建立的網域名稱，或購買您自己的網域名稱。

## <a name="deployment-steps"></a>部署步驟

本指南概述五 （5） 安裝步驟：

- 步驟 1：安裝 Azure AD 之間進行同步處理的 Azure AD Connect 和內部部署 AD
- 步驟 2：在列印伺服器上安裝混合式雲端列印套件
- 步驟 3：安裝 Azure 應用程式 Proxy (AAP) 與傳遞驗證
- 步驟 4：設定必要的 MDM 原則
- 步驟 5：發佈共用的印表機

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>步驟 1-Azure AD 之間進行同步處理安裝 Azure AD Connect 和內部部署 AD
1. Windows Server Active Directory 電腦上，下載 Azure AD Connect 軟體
2. 啟動 Azure AD Connect 安裝套件，然後選取**使用快速設定**
3. 輸入安裝程序中所要求的資訊，然後按一下**安裝**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>步驟 2-列印伺服器上安裝混合式雲端列印封裝

1. 安裝混合式雲端列印 PowerShell 模組
   - 從提升權限的 PowerShell 命令提示字元執行下列命令
      - `find-module -Name "PublishCloudPrinter"` 若要確認電腦可以連線到 PowerShell 資源庫 (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > 注意：您可能會看到訊息指出 'PSGallery' 不受信任的存放庫。  輸入 'y' 以繼續進行安裝。

2. 安裝混合式雲端列印方案
    - 在同一個提高權限的 PowerShell 命令提示，將目錄變更 `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - 執行 <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. 設定 2 個 IIS 端點以支援 SSL
   -   SSL 憑證可以是自我簽署的憑證或是發出一些受信任憑證授權單位 (CA) 從
   -  如果使用自我簽署的憑證，請確定憑證已匯入到用戶端電腦
4. 安裝 SQLite 套件
   - 開啟提升權限的 PowerShell 命令提示字元
   - 執行下列命令來下載 System.Data.SQLite nuget 套件 <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - 執行下列命令以安裝套件<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > 注意：建議您下載並安裝最新版本，藉由忽略"-requiredversion"選項。

5. 將 SQLite dll 複製到 MopriaCloudService Webapp\<筒\>資料夾 (**c:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
   - SQLite 二進位檔應該會在 「\\Program Files\\PackageManagement\\NuGet\\套件 」

           \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
           --\> \<bin\>\\System.Data.SQLite.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
           --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
           --\> \<bin\>\\**x64**\\SQLite.Interop.dll
           \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
           --\> \<bin\>\\System.Data.SQLite.Linq.dll  
           \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
           --\> \<bin\>\\System.Data.SQLite.EF6.dll

   > 注意： x.x.x.x 是先前安裝的 SQLite 版本。

6. 更新`c:\inetpub\wwwroot\MopriaCloudService\web.config`檔案，以下列包含 SQLite 版本 x.x.x.x\<執行階段\>/\<assemblyBinding\>各節：

       <dependentAssembly>
       assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>

7. 建立之 SQLite 資料庫：
    -  下載並安裝的 SQLite 工具二進位檔 <https://www.sqlite.org/>
    -  移至**c:\\inetpub\\wwwroot\\MopriaCloudService\\資料庫**目錄
    -  執行下列命令來建立此目錄中的資料庫：  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  從檔案總管 中，開啟 MopriaDeviceDb.db 檔案屬性，以新增使用者/群組，允許發佈到 安全性 索引標籤中的 Mopria 資料庫
        - 建議您只新增必要的系統管理員使用者群組。
8. 向 Azure AD 以支援 OAuth2 驗證的 2 個 web 應用程式
   - Azure AD 租用戶管理入口網站的全域管理員身分登入
     1. 移 「 應用程式註冊 索引標籤，以新增列印端點
        - 新增應用程式中，選取 「 新的應用程式註冊 」
        - 提供適當的名稱，然後選取 「 Web 應用程式 / API 」
        - 登入 URL ="<http://MicrosoftEnterpriseCloudPrint/CloudPrint>」
     2. 重複的探索端點
        - 登入 URL ="<http://MopriaDiscoveryService/CloudPrint>」
     3. 重複的原生用戶端應用程式
        -   提供應用程式名稱時，請確定您選取 「 原生用戶端應用程式 」 與 「 應用程式類型 」
        -   Redirect URI = "https://\<services-machine-endpoint\>/RedirectUrl"
     4. 移至原生用戶端應用程式 [設定]
        -   複製 「 應用程式識別碼 」 值，用於後續安裝步驟
        -   選取 [必要權限]
            1.  按一下 [新增]
            2.  按一下 [選取 API]
            3.  列印端點和探索端點，建立應用程式端點時，您定義的名稱搜尋
            4.  新增 2 個端點
            5.  請確定每個應用程式端點的 [委派權限] 選項已啟用
            6.  請務必按一下底部的 「 已完成 」 按鈕
            7.  加入這兩個端點之後，請按一下 [授與權限]。  選取 [是] 當系統提示您核准要求。
        -   請移至 [重新導向 URI]，並將清單中的下列重新導向 Uri: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>步驟 3-安裝與傳遞驗證的 Azure 應用程式 Proxy (AAP)
1. 登入您 Azure AD (AAD) 租用戶的管理入口網站
    - 在 AAD 功能表清單中，選取 [應用程式 proxy]
    - 按一下畫面頂端的 [啟用應用程式 proxy]
    - 下載到已加入網域的 Windows Server 機器會做為 Web 應用程式 Proxy (WAP) 的 「 應用程式 Proxy 連接器 」。
2. 在 WAP 電腦]、 [系統管理員身分安裝 「 應用程式 Proxy 連接器 」 的登入
    - 在安裝期間，提供應用程式 proxy 連接器認證給您想要在啟用 AAP 您 Azure 租用戶
    - 請確定 WAP 電腦已加入至您的內部部署 Active Directory 網域
3. 返回至 AAD 租用戶管理入口網站，並新增應用程式 proxy
   - 移至**企業應用程式** 索引標籤
   - 按一下 **新的應用程式**
   - 選取 **在內部部署應用程式**並填寫各欄位
       - 名稱：任何您想要的名稱
       - 內部 URL:這是 WAP 電腦可以存取 Mopria 探索雲端服務的內部 URL
       - 外部 URL:針對您的組織適當地設定
       - 預先驗證方法：通過

     >   注意:如果找不到上述的所有設定，將 proxy 可用的設定與然後選取您剛才建立的應用程式 proxy 並移至**應用程式 proxy**索引標籤並加入上述的所有資訊。

4. 重複上述的 3，企業雲端列印服務和內部的 URL 提供給您的企業雲端列印服務

    >   注意:Https://&lt;services 機器端點 &gt; /mcs 下面所述的 URL 應該是在您的安裝程式 Mopria 雲端服務和/或企業雲端列印服務的外部 URL。


### <a name="step-4---configure-the-required-mdm-policies"></a>步驟 4-設定必要的 MDM 原則
- 登入您的 MDM 提供者
- 找到企業雲端列印原則的群組，並設定以下的指導方針的原則：
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD Directory ID\>
  - CloudPrintOAuthClientId ="Application ID"值，您在 Azure AD 管理入口網站中註冊的原生 Web 應用程式
  - CloudPrinterDiscoveryEndPoint = Mopria 探索服務 Azure 應用程式 Proxy 的步驟 3.3 中建立的外部 URL (必須是完全相同，但是不含尾端 /)
  - MopriaDiscoveryResourceId = 「 應用程式識別碼 URI 」 Web 應用程式註冊步驟 2.8 中的探索端點的 API。  您可以找到此設定下-> 應用程式的屬性
  - CloudPrintResourceId = 「 應用程式識別碼 URI 」 Web 應用程式註冊步驟 2.8 中的列印端點的 API。 您可以找到此設定下-> 應用程式的屬性
  - DiscoveryMaxPrinterLimit = \<a positive integer\>

>   注意:如果您使用 Microsoft Intune 服務，您可以找到這些設定 「 雲端印表機 」 類別下。

|Intune 的顯示名稱                     |原則                         |
|----------------------------------------|-------------------------------|
|印表機探索 URL                   |CloudPrinterDiscoveryEndpoint  |
|印表機存取授權 URL            |CloudPrintOAuthAuthority       |
|Azure 的原生用戶端應用程式 GUID            |CloudPrintOAuthClientId        |
|列印服務資源 URI              |CloudPrintResourceId           |
|要查詢 （僅限行動裝置） 的最大印表機  |DiscoveryMaxPrinterLimit       |
|印表機探索服務資源 URI  |MopriaDiscoveryResourceId      |


>   注意:如果雲端列印原則群組可用，但 MDM 提供者支援的 OMA URI 設定，則您可以將相同的原則。  請參閱此<a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">文章</a>的其他資訊。

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - 值 = `https://login.microsoftonline.com/` \<Azure AD 目錄識別碼\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - 值 = \<Azure AD 原生應用程式的應用程式識別碼\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - 值 = Mopria 探索服務 Azure 應用程式 Proxy 的步驟 3.3 中建立的外部 URL (必須是完全相同，但是不含尾端 /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - 值 = 「 應用程式識別碼 URI 」 Web 應用程式註冊步驟 2.8 中的探索端點的 API。  您可以找到此設定下-> 應用程式的屬性
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - 值 = 「 應用程式識別碼 URI 」 Web 應用程式註冊步驟 2.8 中的列印端點的 API。 您可以找到此設定下-> 應用程式的屬性
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - 值 =\<正整數\>
  
### <a name="step-5---publish-desired-shared-printers"></a>步驟 5-發行所需的共用的印表機
1. 安裝所需的印表機上列印伺服器
2. 共用印表機的 [屬性] UI 透過印表機
3. 選取所需的一組使用者來授與存取權
4. 儲存變更並關閉印表機的 [屬性] 視窗
5. Windows 10 Fall Creator Update 機器上，開啟提升權限的 Windows PowerShell 命令提示字元
   1. 執行下列命令
      - `find-module -Name "PublishCloudPrinter"` 若要確認電腦可以連線到 PowerShell 資源庫 (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   注意：您可能會看到訊息指出 'PSGallery' 不受信任的存放庫。  輸入 'y' 以繼續進行安裝。

      - Publish-CloudPrinter
        - 印表機 = 已定義的共用的印表機名稱
        - 製造商 = 印表機製造商
        - 模型 = 印表機型號
        - OrgLocation = JSON 字串，例如指定印表機的位置：   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - Sddl = SDDL 字串，表示印表機的權限。 這可取得適當地修改 [印表機內容-安全性] 索引標籤，然後在命令提示字元中執行下列命令： `(Get-Printer PrinterName -full).PermissionSDDL`
            例如："G:DUD:(A;OICI;FA;;;WD)"

          > 注意：您必須新增 **`O:BA`** 做為前置詞結果與上述命令提示字元命令之前將值設定為 SDDL 設定。  範例：SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = https://&lt;services-machine-endpoint&gt;/mcs
        - PrintServerEndpoint = https://&lt;services-machine-endpoint&gt;/ecp
        - AzureClientId = 已註冊的原生的 Web 應用程式值，從上方的應用程式識別碼
        - AzureTenantGuid = 的 Azure AD 租用戶目錄識別碼
        - DiscoveryResourceId = 代理 Mopria 探索雲端服務的 [選擇性] 應用程式識別碼

        > 注意：您可以在命令列中輸入所有必要的參數值。<br>
        **發行 CloudPrinter** PowerShell 命令語法： <br>
        發行 CloudPrinter-印表機\<字串\>-製造商\<字串\>-模型\<字串\>-OrgLocation\<字串\>-Sddl \<字串\>-DiscoveryEndpoint\<字串\>-PrintServerEndpoint\<字串\>-AzureClientId\<字串\>-AzureTenantGuid \<字串\>[-DiscoveryResourceId\<字串\>] <br>
        範例命令： `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>正在驗證部署
在 Azure AD 已加入已設定的 MDM 原則的裝置：
- 開啟網頁瀏覽器並移到 https://&lt;services 機器端點 &gt; /mcs/服務
- 您應該會看到 JSON 文字，描述這個端點的功能集
- 前往 」 OS 設定-\>裝置-\>印表機和掃描器 」
    - 您應該會看到 「 搜尋雲端印表機 」 連結
    - 按一下連結
    - 按一下 [請選取搜尋位置] 連結
        - 您應該會看到裝置位置階層
    - 挑選的位置，然後按一下 **[確定]** ，然後按一下**搜尋**按鈕來尋找印表機
    - 選取印表機，然後按一下**新增裝置**按鈕
    - 安裝完成後成功的印表機，列印至印表機從您最愛的應用程式

>   注意:如果使用 「 EcpPrintTest 」 印表機，您就可以在底下的列印伺服器電腦中找到的輸出檔"c:\\ECPTestOutput\\EcpTestPrint.xps 」 位置。