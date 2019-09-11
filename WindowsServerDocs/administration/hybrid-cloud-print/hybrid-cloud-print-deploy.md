---
title: 部署 Windows Server 混合式雲端列印
description: 如何設定 Microsoft 混合式雲端列印
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
ms.openlocfilehash: 552695626c98ee0fc01148536b50d4466d1b96e4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866808"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>使用預先驗證來部署 Windows Server 混合式雲端列印

>適用於：Windows Server 2016

本主題適用于 IT 系統管理員，說明 Microsoft 混合式雲端列印解決方案的端對端部署。 此解決方案會在現有的 Windows Server （以列印伺服器執行）上分層，並啟用 Azure Active Directory 聯結和 MDM 管理的裝置，以探索及列印到組織管理的印表機。

## <a name="pre-requisites"></a>先決條件

在開始此安裝之前，您必須先取得一些訂用帳戶、服務和電腦。 分別為：

-   Azure AD premium 訂用帳戶
    
    請參閱[開始使用 azure 訂用](https://azure.microsoft.com/trial/get-started-active-directory/)帳戶，以取得 azure 試用版訂用帳戶。 

-   MDM 服務，例如 Intune
    
    如需 Intune 的試用版訂閱，請參閱[Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune)。

-   執行為 Active Directory 的 Windows Server

    請[參閱逐步解說：設定 Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/)中的 Active Directory，以取得設定 Active Directory 的協助。

-   已加入網域的 Windows Server 2016 做為列印伺服器
    
    如需如何在 Windows Server 上安裝角色和角色服務的相關資訊，請參閱[使用新增角色及功能嚮導來安裝角色、角色服務和功能](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)。

-   Azure AD Connect
    
    如需設定 Azure AD Connect 的說明，請參閱[Azure AD Connect 的自訂安裝](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)。

-   在個別加入網域的 Windows Server 電腦上 Azure 應用程式 Proxy 連接器
    
    如需設定 Azure 應用程式 Proxy 連接器的協助，請參閱[開始使用應用程式 Proxy 和安裝連接器](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)。

-   公開的功能變數名稱
    
    您可以使用 Azure 為您建立的功能變數名稱，或購買您自己的功能變數名稱。

## <a name="deployment-steps"></a>部署步驟

本指南概述五（5）個安裝步驟：

- 步驟 1：安裝 Azure AD Connect 以在 Azure AD 與內部部署 AD 之間同步
- 步驟 2：在列印伺服器上安裝混合式雲端列印套件
- 步驟 3：使用 Kerberos 限制委派（KCD）安裝 Azure 應用程式 Proxy （AAP-SCHEME）
- 步驟 4：設定必要的 MDM 原則
- 步驟 5：發佈共用印表機

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>步驟 1-安裝 Azure AD 與內部部署 AD 之間同步的 Azure AD Connect
1. 在 Windows Server Active Directory 機上，下載 Azure AD Connect 軟體
2. 啟動 Azure AD Connect 安裝套件，然後選取 [**使用快速設定**]
3. 輸入安裝程式中要求的資訊，然後按一下 [**安裝**]

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>步驟 2-在列印伺服器上安裝混合式雲端列印套件

1. 安裝混合式雲端列印 PowerShell 模組
   - 從提高許可權的 PowerShell 命令提示字元執行下列命令
      - `find-module -Name "PublishCloudPrinter"`確認電腦可以觸達 PowerShell 資源庫（PSGallery）
      - `install-module -Name "PublishCloudPrinter"`

     > 注意：您可能會看到訊息，指出「PSGallery」是不受信任的存放庫。  輸入 ' y ' 以繼續安裝。

2. 安裝混合式雲端列印解決方案
    - 在同一個已提升許可權的 PowerShell 命令提示字元中，將目錄變更為`C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - 執行 <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. 設定2個 IIS 端點以支援 SSL
   -   SSL 憑證可以是自我簽署的憑證，或從某些信任的憑證授權單位單位（CA）發行的憑證
   -  如果使用自我簽署憑證，請確定憑證已匯入至用戶端電腦
4. 安裝 SQLite 套件
   - 開啟提升許可權的 PowerShell 命令提示字元
   - 執行下列命令以下載 System.object nuget 套件 <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - 執行下列命令來安裝套件<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > 注意：建議您保留 "-requiredversion" 選項，以下載並安裝最新版本。

5. 將 SQLite dll 複製到\<MopriaCloudService Webapp bin\>資料夾（**C：\\inetpub\\ \\wwwroot\\MopriaCloudService bin**）： <br>
   - SQLite 二進位檔\\應位於 "Program Files\\PackageManagement\\NuGet\\封裝"

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

   > 注意： x.x 是上面安裝的 SQLite 版本。

6. 在下列\<運行時間assemblyBinding\>區段中，更新檔案以包含 SQLite 版本 x.x. x：\> / `c:\inetpub\wwwroot\MopriaCloudService\web.config` \<

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

7. 建立 SQLite 資料庫：
    -  從下載並安裝 SQLite 工具二進位檔<https://www.sqlite.org/>
    -  移至**c：\\inetpub\\wwwroot\\MopriaCloudService\\資料庫**目錄
    -  執行下列命令，在此目錄中建立資料庫：`sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  從 [檔案瀏覽器] 開啟 [MopriaDeviceDb] 檔案內容，以新增允許發佈至 [安全性] 索引標籤中 Mopria 資料庫的使用者/群組
        - 建議僅新增必要的管理使用者群組。
8. 向 Azure AD 註冊2個 web 應用程式以支援 OAuth2 authentication
   - 以全域管理員身分登入 Azure AD 租使用者管理入口網站
     1. 移至 [應用程式註冊] 索引標籤以新增列印端點
        - 加入應用程式，選取 [新增應用程式註冊]
        - 提供適當的名稱，然後選取 [Web 應用程式/API]
        - 登入 URL = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. 針對探索端點重複執行
        - 登入 URL = "<http://MopriaDiscoveryService/CloudPrint>"
     3. 針對 native client 應用程式重複執行
        -   提供應用程式名稱時，請務必選取 [原生用戶端應用程式] 作為 [應用程式類型]
        -   [重新導向 URI]\<= "HTTPs://services-\>機器-端點/RedirectUrl"
     4. 進入原生用戶端應用程式的 [設定]
        -   複製 [應用程式識別碼] 值，以用於稍後的安裝步驟
        -   選取 [必要許可權]
            1.  按一下 [新增]
            2.  按一下 [選取 API]
            3.  依據您在建立應用程式端點時所定義的名稱，搜尋列印端點和探索端點
            4.  新增2個端點
            5.  請確定每個應用程式端點的 [委派的許可權] 選項已啟用
            6.  請確定您按一下底部的 [完成] 按鈕
            7.  一旦新增這兩個端點，請按一下 [授與許可權]。  當系統提示您核准要求時，請選取 [是]。
        -   移至 [重新導向 URI]，並將下列重新導向 Uri 新增至清單：`ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>步驟 3-使用 Kerberos 限制委派（KCD）安裝 Azure 應用程式 Proxy （AAP-SCHEME）
1. 登入您的 Azure AD （AAD）租使用者管理入口網站
    - 在 AAD 功能表清單中，選取 [應用程式 proxy]
    - 按一下畫面頂端的 [啟用應用程式 proxy]
    - 將「應用程式 Proxy 連接器」下載至加入網域的 Windows Server 電腦，以作為 Web 應用程式 Proxy （WAP）。
2. 在 WAP 機器上，以系統管理員身分登入，並安裝「應用程式 Proxy 連接器」
    - 在安裝期間，為應用程式 proxy 連接器提供您想要啟用 AAP-SCHEME 的 Azure 原則認證
    - 請確定 WAP 機器已加入內部部署的網域 Active Directory
3. 在 Active Directory 機上，移至 [**工具]-[> 使用者和電腦**]
    - 選取正在執行應用程式 Proxy 連接器的電腦
    - 以滑鼠右鍵按一下並選取 [**屬性-> 委派**] 索引標籤
    - 選取 [**信任這台電腦，但只委派指定的服務]。**
    - 選取 [**使用任何驗證通訊協定]。**
    - 在**此帳戶可以顯示委派認證的 [服務**] 底下
        - 新增執行服務之電腦的 SPN 身分識別值（MopriaDiscoveryService 和 MicrosoftEnterpriseCloudPrint 服務）
            - 針對 [SPN]，輸入電腦本身的 SPN，亦即「主機/\<MachineName\>。\<網域\>」<br>
                `HOST/appServer.Contoso.com`
4. 回到 AAD 租使用者管理入口網站，並新增應用程式 proxy
   - 移至 [**企業應用程式**] 索引標籤
   - 按一下 [**新增應用程式**]
   - 選取 [內部**部署應用程式**] 並填寫欄位
       - 名稱：任何您想要的名稱
       - 內部 URL：這是您 WAP 電腦可以存取的 Mopria 探索雲端服務的內部 URL
       - 外部 URL：設定適合您的組織
       - 預先驗證方法：Azure Active Directory

     >   注意:如果您找不到上述所有設定，請新增具有可用設定的 proxy，然後選取您剛才建立的應用程式 proxy，並移至 [**應用程式 proxy** ] 索引標籤，並新增上述所有資訊。

   - 建立之後，返回 [**企業應用程式** -> ] [**所有應用程式**]，選取您剛建立的新應用程式
   - 移至 [**單一登入**]，確認 [單一登入模式] 設定為 [整合式 Windows 驗證]
   - 將「內部應用程式 SPN」設定為您在上述步驟3.3 中指定的 SPN
   - 請確定「委派的登入身分識別」已設定為「使用者主體名稱」

5. 針對企業雲端列印服務重複上述4，並提供您的企業雲端列印服務的內部 URL
6. 回到 Azure AD 租使用者管理入口網站，移至**應用程式註冊**並選取 [Native Client] 應用程式-> [設定]
    - 選取**必要許可權**
        - 新增您剛建立的2個新的 proxy 應用程式
        - 授與這兩個應用程式的委派許可權
        - 新增這兩個 proxy 應用程式之後，請按一下 [授與許可權]。  當系統提示您核准要求時，請選取 [是]。

7. 在 IIS 中為 Mopria 雲端服務和企業雲端列印服務電腦啟用 Windows 驗證
    - 請確定已安裝 Windows 驗證功能：
        1. 開啟 [伺服器管理員]
        2. 按一下 [**管理**]
        3. 按一下 [**新增角色及功能**]
        4. 選取以**角色為基礎或以功能為基礎的安裝**
        5. 選取伺服器
        6. 在 [網頁伺服器（IIS）-> 網頁伺服器-> 安全性] 底下，選取 [ **Windows 驗證**]
        7. 按 [下一步] 直到安裝完成
    - 在 IIS 中啟用 Windows 驗證：
        1. 開啟 Internet Information Services （IIS）管理員
        2. 針對每個服務/網站：
            1.  選取服務/網站
            2.  按兩下 [**驗證**]
            3.  按一下 [ **Windows 驗證**]，然後按一下 [在**動作**底下**啟用**]

### <a name="step-4---configure-the-required-mdm-policies"></a>步驟 4-設定所需的 MDM 原則
- 登入您的 MDM 提供者
- 尋找「企業雲端列印原則」群組，並遵循下列指導方針來設定原則：
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD 目錄識別碼\>
  - CloudPrintOAuthClientId = 您在 Azure AD 管理入口網站中註冊的原生 Web 應用程式的「應用程式識別碼」值
  - CloudPrinterDiscoveryEndPoint = 步驟3.3 中所建立 Azure 應用程式 Proxy 的 Mopria 探索服務的外部 URL （必須完全相同，但不含尾端/）
  - MopriaDiscoveryResourceId = 步驟3.4 中所建立 Azure 應用程式 Proxy 的 Mopria 探索服務的外部 URL （必須完全相同，包括尾端/）
  - CloudPrintResourceId = 在步驟3.5 中建立之企業雲端列印服務 Azure 應用程式 Proxy 的外部 URL （必須完全相同，包括尾端/）
  - DiscoveryMaxPrinterLimit = \<正整數\>

>   注意:如果您使用 Microsoft Intune 服務，您可以在 [雲端印表機] 類別下找到這些設定。

|Intune 顯示名稱                     |原則                         |
|----------------------------------------|-------------------------------|
|印表機探索 URL                   |CloudPrinterDiscoveryEndpoint  |
|印表機存取授權 URL            |CloudPrintOAuthAuthority       |
|Azure native client 應用程式 GUID            |CloudPrintOAuthClientId        |
|列印服務資源 URI              |CloudPrintResourceId           |
|要查詢的印表機上限（僅限行動裝置）  |DiscoveryMaxPrinterLimit       |
|印表機探索服務資源 URI  |MopriaDiscoveryResourceId      |

>   注意:如果 [雲端列印原則] 群組無法使用，但 MDM 提供者支援 OMA-URI 設定，則您可以設定相同的原則。  如需其他資訊，請參閱這<a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">篇文章</a>。

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - 值 = `https://login.microsoftonline.com/` \<Azure AD 目錄識別碼\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - 值 = \<Azure AD 原生應用程式的應用程式識別碼\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - 值 = Mopria 探索服務的外部 URL，Azure 應用程式在步驟3.3 中建立的 Proxy （必須完全相同，但不含尾端/）
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - 值 = Mopria 探索服務的外部 URL，Azure 應用程式在步驟3.4 中建立的 Proxy （必須完全相同，包括尾端/）
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - 值 = 在步驟3.5 中建立之企業雲端列印服務 Azure 應用程式 Proxy 的外部 URL （必須完全相同，包括尾端/）
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - 值 = \<正整數\>

### <a name="step-5---publish-desired-shared-printers"></a>步驟 5-發行所需的共用印表機
1. 在列印伺服器上安裝所需的印表機
2. 透過印表機屬性 UI 共用印表機
3. 選取所需的使用者集合以授與存取權
4. 儲存變更並關閉 [印表機內容] 視窗
5. 從 Windows 10 秋季建立者更新電腦，開啟提升許可權的 Windows PowerShell 命令提示字元
   1. 執行下列命令
      - `find-module -Name "PublishCloudPrinter"`確認電腦可以觸達 PowerShell 資源庫（PSGallery）
      - `install-module -Name "PublishCloudPrinter"`

        >   注意：您可能會看到訊息，指出「PSGallery」是不受信任的存放庫。  輸入 ' y ' 以繼續安裝。

      - 發行-CloudPrinter
        - 印表機 = 已定義的共用印表機名稱
        - 製造商 = 印表機製造商
        - Model = 印表機型號
        - OrgLocation = 指定印表機位置的 JSON 字串，例如：`{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - Sddl = 代表印表機權限的 SDDL 字串。 這可以藉由適當地修改 [印表機內容] 安全性索引標籤，然後在命令提示字元中執行下列命令來取得：`(Get-Printer PrinterName -full).PermissionSDDL`
            比如.「G:DUD：（A; OICI; FA;;;WD）」

          > 注意：將值設定為 SDDL **`O:BA`** 設定之前，您必須先從上述命令提示字元命令的結果中新增 as 前置詞。  範例：SDDL =`O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = 步驟3.4 中所建立 Azure 應用程式 Proxy 的 Mopria 探索服務的外部 URL
        - PrintServerEndpoint = 在步驟3.5 中建立之企業雲端列印服務 Azure 應用程式 Proxy 的外部 URL
        - AzureClientId = 上述已註冊原生 Web 應用程式值的應用程式識別碼
        - AzureTenantGuid = Azure AD 租使用者的目錄識別碼
        - DiscoveryResourceId = [選擇性] 代理 Mopria 探索雲端服務的應用程式識別碼

        > 注意：您也可以在命令列中輸入所有必要的參數值。<br>
        **發行-CloudPrinter**PowerShell 命令語法： <br>
        發行-CloudPrinter-印表機\<字串\> -製造商\<字串\> -模型\<字串\> -OrgLocation \<字串\> -Sddl \<字串\> DiscoveryEndpoint字串-\> PrintServerEndpoint\>字串- AzureClientId\<string-\> AzureTenantGuid \< \< \<字串[\> - DiscoveryResourceId\<string]\> <br>
        範例命令：`publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>驗證部署
在已設定 MDM 原則的 Azure AD 聯結裝置上：
- 開啟網頁瀏覽器，並移至 [&lt;HTTPs://服務]-[&gt;機器-端點/mcs/services] （探索端點的外部 URL）
- 您應該會看到描述此端點功能集的 JSON 文字
- 移至 [OS 設定-\>裝置-\>印表機 & 掃描器]
    - 您應該會看到「搜尋雲端印表機」連結
    - 按一下連結
    - 按一下 [請選取搜尋位置] 連結
        - 您應該會看到裝置位置階層
    - 挑選位置，按一下 **[確定]** ，然後按一下 [**搜尋**] 按鈕以尋找印表機
    - 選取印表機，然後按一下 [**新增裝置**] 按鈕
    - 成功安裝印表機之後，從您最愛的應用程式列印到印表機

>   注意:如果使用 "EcpPrintTest" 印表機，您可以在 "C：\\ECPTestOutput\\EcpTestPrint. xps" 位置下的列印伺服器電腦中找到輸出檔案。
