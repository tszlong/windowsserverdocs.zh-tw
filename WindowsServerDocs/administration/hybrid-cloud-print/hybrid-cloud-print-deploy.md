---
title: 部署 Windows Server 混合式雲端列印
description: 如何設定 Microsoft 混合式雲端列印
ms.prod: windows-server
ms.technology: windows server 2016
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: c06aafb015b065f307eca02abc7a6adaa8ba763c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852111"
---
# <a name="deploy-windows-server-hybrid-cloud-print"></a>部署 Windows Server 混合式雲端列印

>適用於︰Windows Server 2016

本主題適用于 IT 系統管理員，說明 Microsoft 混合式雲端列印（HCP）解決方案的端對端部署。 此解決方案會在現有的 Windows Server （以列印伺服器執行）之上進行分層，並啟用 Azure Active Directory （Azure AD）聯結和 MDM 管理的裝置，以探索並列印到組織管理的印表機。

## <a name="pre-requisites"></a>必要條件

在開始此安裝之前，您必須先取得一些訂用帳戶、服務和電腦。 分別為：

- Azure AD premium 訂用帳戶。

  請參閱[開始使用 azure 訂用](https://azure.microsoft.com/trial/get-started-active-directory/)帳戶以取得 azure 的試用版訂用帳戶。

- MDM 服務，例如 Intune。

  如需 Intune 的試用版訂閱，請參閱[Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) 。

- 執行 Active Directory 的 Windows Server 2016 或更新版本電腦。

  如需設定 Active Directory 的說明，請參閱逐步解說[：在 Windows Server 2016 中設定 Active Directory](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/) 。

- 以「列印伺服器」執行的專用、已加入網域的 Windows Server 2016 或更新版本電腦。

- 執行為連接器伺服器的專用、已加入網域的 Windows Server 2016 或更新版本電腦。

  如需詳細資訊，請參閱[瞭解 Azure AD 應用程式 Proxy 連接器](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors)。

- Windows 10 秋季建立者更新或更新版本的電腦，用於發佈印表機。

- 公開的功能變數名稱。

  您可以使用 Azure （*domainname*. onmicrosoft.com）為您建立的功能變數名稱，或購買您自己的功能變數名稱。 請參閱[使用 Azure Active Directory 入口網站新增自訂功能變數名稱](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)。

## <a name="deployment-steps"></a>部署步驟

下列步驟適用于一般的混合式雲端列印部署。

### <a name="step-1---install-azure-ad-connect"></a>步驟 1-安裝 Azure AD Connect

1. Azure AD connect 會同步處理 Azure AD 至內部部署 AD。 在具有 Active Directory 的 Windows Server 電腦上，下載並安裝具有快速設定的 Azure AD Connect 軟體。 請參閱[使用快速設定開始使用 Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)。

### <a name="step-2---install-application-proxy"></a>步驟 2-安裝應用程式 Proxy

1. 應用程式 proxy 可讓您組織中的使用者從雲端存取內部部署應用程式。 在連接器伺服器上安裝應用程式 Proxy。
    - 如需安裝指示，請參閱[教學課程：在 Azure Active Directory 中新增透過應用程式 Proxy 進行遠端存取的內部部署應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)。
    - 如果組織具有複雜的網路拓撲，則建議使用專用的連接器群組。 請參閱[使用連接器群組在個別的網路和位置上發佈應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connector-groups)。

### <a name="step-3---register-and-configure-applications"></a>步驟 3-註冊及設定應用程式

若要啟用與 HCP 服務的驗證通訊，我們需要建立3個應用程式：2個 Web 應用程式來表示兩個 HCP 服務，以及1個原生應用程式來與這些服務通訊。

1. 登入 Azure 入口網站以註冊 web 應用程式。
    - 在 Azure Active Directory 底下，移至**應用程式註冊** > **新增註冊**。

    ![AAD 應用程式註冊1](../media/hybrid-cloud-print/AAD-AppRegistration.png)

    - 輸入 Mopria 探索服務的應用程式名稱。 按一下 [**註冊**] 完成。

    ![AAD 應用程式註冊2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria.png)

    - 針對「企業雲端列印服務」重複此動作。
    - 針對原生應用程式重複執行。
    - 這三個應用程式應該會顯示在 [**應用程式註冊**] 之下。

    ![AAD 應用程式註冊3](../media/hybrid-cloud-print/AAD-AppRegistration-AllApps.png)

2. 公開2個 Web 應用程式的 API。
    - 仍在 [**應用程式註冊**] 分頁中，按一下 [Mopria 探索服務] 應用程式，選取 [**公開 API**]，然後按一下 [應用程式識別碼 URI] 旁的 [**設定**]。

    ![AAD 公開 API 1](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI.png)

    - 按一下 [**儲存**] 而不變更 [應用程式識別碼 URI] 的預設值。 這個值只需要立即設定，稍後將會變更。

    ![AAD 公開 API 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-Save.png)

    - 按一下 [**新增領域**]。

    ![AAD 公開 API 3](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-AddScope.png)

    - 提供 [領域名稱]、[允許系統管理員和使用者同意]、輸入同意描述，然後按一下 [**新增領域**] 以完成。

    ![AAD 公開 API 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-ScopeName.png)

    - 針對「企業雲端列印服務」重複此動作。 請使用不同的範圍名稱和同意描述。

    ![AAD 公開 API 5](../media/hybrid-cloud-print/AAD-AppRegistration-ECP-ExposeAPI-ScopeName.png)

3. 新增 API 許可權
    - 返回應用程式註冊 分頁。 按一下 [原生應用程式]，然後選取 [API 許可權]。 按一下 [**新增許可權**]。

    ![AAD API 許可權1](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission.png)

    - 切換至 [**我的組織使用的 api**]，然後使用 [搜尋] 方塊尋找先前新增的 Mopria 探索服務。 按一下搜尋結果中的服務。

    ![AAD API 許可權2](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria.png)

    - 選取 [**委派的許可權**]。 勾選 [API 範圍] 旁的方塊。 按一下 [**新增許可權**]。

    ![AAD API 許可權3](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria-Add.png)

    - 重複此步驟，將許可權新增至企業雲端列印服務。

    ![AAD API 許可權4](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-ECP-Add.png)

    - 一旦返回 [API 許可權] 分頁，請等待10秒，再按一下 [系統**管理員同意**]。

    ![AAD API 許可權5](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent.png)

    - 出現提示時，按一下 **[是]** 。

    ![AAD API 許可權6](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent-Yes.png)

    - 確認 [API] 許可權的 [狀態] 欄顯示綠色核取記號。

    ![AAD API 許可權7](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Verify.png)

4. 設定 Web 應用程式的應用程式 Proxy
    - 請移至**Azure Active Directory** > **企業應用**程式 > **所有應用程式**。 搜尋 Mopria 探索服務，並按一下它。

    ![AAD 應用程式 Proxy 1](../media/hybrid-cloud-print/AAD-EnterpriseApp-AllApps.png)

    - 按一下 [**應用程式 proxy**]。 使用 `https://<fully qualified domain name of the Print Server>/mcs/`格式輸入內部 Url。 按一下 [**儲存**] 完成。

    ![AAD 應用程式 Proxy 2](../media/hybrid-cloud-print/AAD-EnterpriseApp-Mopria-AppProxy.png)

    - 針對「企業雲端列印服務」重複此動作。 請注意，內部 URL 為 `https://<fully qualified domain name of the Print Server>/ecp/`。

    ![AAD 應用程式 Proxy 3](../media/hybrid-cloud-print/AAD-EnterpriseApp-ECP-AppProxy.png)

    - 移至**Azure Active Directory** > **應用程式註冊**。 按一下 [Mopria 探索服務]。 請注意，在 **[總覽**] 底下，應用程式識別碼 URI 已從預設值變更為 [**應用程式 proxy**] 底下的 [外部 URL]。 在列印伺服器安裝期間、在用戶端 MDM 原則中，以及用於發佈印表機時，將會使用此 URI。

    ![AAD 應用程式 Proxy 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-Overview.png)

5. 將使用者指派給應用程式
    - 請移至**Azure Active Directory** > **企業應用**程式 > **所有應用程式**。 搜尋 Mopria 探索服務並按一下它
    - 按一下 [**使用者和群組**] 並指派使用者，或按一下 [內容] **，然後變更**需要的**使用者指派？** 設為 [**否**]
    - 針對「企業雲端列印服務」重複此動作。

6. 設定原生應用程式中的重新導向 URI
    - 移至**Azure Active Directory** > **應用程式註冊**。 按一下 [原生應用程式]。 移至 **[總覽**] 並複製**應用程式（用戶端）識別碼**。

    ![AAD 重新導向 URI 1](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Overview.png)

    - 移至 [**驗證**]。 將 [**類型**] 下拉式清單方塊變更為 [`Public...`]，然後使用下列格式輸入兩個重新導向 uri，其中 `<NativeClientAppID>` 是來自上一個步驟：

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/<NativeClientAppID>`

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

    ![AAD 重新導向 URI 2](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Authentication.png)

    - 按一下 [**儲存**] 完成。

### <a name="step-4---setup-the-print-server"></a>步驟 4-設定列印伺服器

1. 請確定列印伺服器已安裝所有可用的 Windows Update。 注意：伺服器2019必須修補為組建17763.165 或更新版本。
    - 安裝下列伺服器角色：
        - 列印伺服器角色
        - 網際網路資訊服務（IIS）
    - 如需如何安裝伺服器角色的詳細資訊，請參閱[使用新增角色及功能嚮導來安裝角色、角色服務和功能](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw)。

    ![列印伺服器角色](../media/hybrid-cloud-print/PrintServer-Roles.png)

2. 安裝混合式雲端列印 PowerShell 模組。
    - 從提高許可權的 PowerShell 命令提示字元執行下列命令：

        `find-module -Name PublishCloudPrinter` 確認機器可以觸達 PowerShell 資源庫（PSGallery）

        `install-module -Name PublishCloudPrinter`

    > 注意：您可能會看到訊息，指出 ' PSGallery ' 是不受信任的存放庫。  輸入 ' y ' 以繼續安裝。

    ![列印伺服器發佈雲端印表機](../media/hybrid-cloud-print/PrintServer-PublishCloudPrinter.png)

3. 安裝混合式雲端列印解決方案。
    - 在同一個已提升許可權的 PowerShell 命令提示字元中，將目錄變更為下列內容（所需的引號）：

        `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`

    - 執行

        `.\CloudPrintDeploy.ps1 -AzureTenant <Azure Active Directory domain name> -AzureTenantGuid <Azure Active Directory ID>`

    - 請參閱下面的螢幕擷取畫面，以尋找 Azure Active Directory 的功能變數名稱。

    ![列印伺服器如何取得 AAD 功能變數名稱](../media/hybrid-cloud-print/PrintServer-GetAADDomainName.png)

    - 請參閱下列螢幕擷取畫面，以尋找 Azure Active Directory 識別碼。

    ![列印伺服器雲端列印部署](../media/hybrid-cloud-print/PrintServer-GetAADId.png)

    - CloudPrintDeploy 腳本的輸出看起來像這樣：

    ![列印伺服器雲端列印部署](../media/hybrid-cloud-print/PrintServer-CloudPrintDeploy.png)

    - 檢查記錄檔以查看是否有任何錯誤： `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0\CloudPrintDeploy.log`

4. 在提高許可權的命令提示字元中執行**RegitEdit** 。 前往 Computer \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\EnterpriseCloudPrintService。
    - 請確定 [AzureAudience] 設定為 [企業雲端列印應用程式] 的 [應用程式識別碼 URI]。
    - 請確定 [AzureTenant] 設定為 [Azure AD 功能變數名稱]。

    ![列印伺服器 ECP 登錄機碼](../media/hybrid-cloud-print/PrintServer-RegEdit-ECP.png)

5. 前往 Computer \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\MopriaDiscoveryService。
    - 請確定 AzureAudience 是 Mopria 探索服務應用程式的應用程式識別碼 URI。
    - 請確定 AzureTenant 是 Azure AD 功能變數名稱。
    - 請確定 URL 是 Mopria 探索服務應用程式的應用程式識別碼 URI。

    ![列印伺服器 Mopria 登錄機碼](../media/hybrid-cloud-print/PrintServer-RegEdit-Mopria.png)

6. 在提升許可權的 Powershell 命令提示字元中執行**iisreset** 。 這可確保在上一個步驟中所做的任何登錄變更都會生效。

7. 設定 IIS 端點以支援 SSL。
    - SSL 憑證可以是自我簽署的憑證，或從信任的憑證授權單位單位（CA）發出的憑證。
    - 如果使用自我簽署憑證，**請確定憑證已匯入至用戶端電腦**。
    - 如果您向協力廠商提供者註冊您的網域，您將需要使用 SSL 憑證來設定 IIS 端點。 如需詳細資訊，請參閱本[指南](https://www.sslsupportdesk.com/microsoft-server-2016-iis-10-10-5-ssl-installation/)。

8. 安裝 SQLite 套件。
   - 開啟已提升許可權的 PowerShell 命令提示字元。
   - 執行下列命令以下載 system.string nuget 套件。

        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`

   - 執行下列命令來安裝封裝。

        `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > 注意：建議您保留-requiredversion 選項來下載並安裝最新版本。

    ![列印伺服器 Mopria 登錄機碼](../media/hybrid-cloud-print/PrintServer-InstallSQLite.png)

9. 將 SQLite dll 複製到 MopriaCloudService Webapp bin 資料夾（C:\inetpub\wwwroot\MopriaCloudService\bin）。
    - 建立包含下列 PowerShell 腳本的 ps1 檔案。
    - 將 $version 變數變更為上一個步驟中所安裝的 SQLite 版本。
    - 在提高許可權的 PowerShell 命令提示字元中執行 ps1 檔案。

    ```powershell
    $source = \Program Files\PackageManagement\NuGet\Packages
    $core = System.Data.SQLite.Core
    $linq = System.Data.SQLite.Linq
    $ef6 = System.Data.SQLite.EF6
    $version = x.x.x.x
    $target = C:\inetpub\wwwroot\MopriaCloudService\bin

    xcopy /y $source\$core.$version\lib\net46\System.Data.SQLite.dll $target\
    xcopy /y $source\$core.$version\build\net46\x86\SQLite.Interop.dll $target\x86\
    xcopy /y $source\$core.$version\build\net46\x64\SQLite.Interop.dll $target\x64\
    xcopy /y $source\$linq.$version\lib\net46\System.Data.SQLite.Linq.dll $target\
    xcopy /y $source\$ef6.$version\lib\net46\System.Data.SQLite.EF6.dll $target\
    ```

10. 更新 c:\inetpub\wwwroot\MopriaCloudService\web.config 檔案，以在下列 `<runtime>/<assemblyBinding>` 區段中包含 SQLite 版本 x.x. x。 這是上一個步驟中使用的相同版本。

    ```xml
    ...
    <dependentAssembly>
    assemblyIdentity name=System.Data.SQLite culture=neutral publicKeyToken=db937bc2d44ff139 /
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Core culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.EF6 culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Linq culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    ...
    ```

11. 建立 SQLite 資料庫。
    - 從 `https://www.sqlite.org/`下載並安裝 SQLite 工具二進位檔。
    - 移至 `c:\inetpub\wwwroot\MopriaCloudService\Database` 目錄。
    - 執行下列命令，在此目錄中建立資料庫：

        `sqlite3.exe MopriaDeviceDb.db .read MopriaSQLiteDb.sql`

    - 在 [檔案管理器] 中，開啟 [MopriaDeviceDb] 檔案內容，以新增允許在 [安全性] 索引標籤中發佈至 Mopria 資料庫的使用者或群組。使用者或群組必須存在於內部部署 Active Directory，並與 Azure AD 同步處理。
    - 如果解決方案部署至不可路由傳送的網域（例如*mydomain*），則必須將 Azure AD 網域（例如 onmicrosoft.com，或從協力廠商購買的一個）新增為內部部署 ACTIVE DIRECTORY 的 UPN*尾碼。* 如此一來，就可以在資料庫檔案的安全性設定中加入與發行印表機相同的使用者（例如 admin@*domainname*. onmicrosoft.com）。 如需目錄同步作業，請參閱[準備無法路由](https://docs.microsoft.com/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization)傳送的網域。

    ![列印伺服器 Mopria 登錄機碼](../media/hybrid-cloud-print/PrintServer-SQLiteDB.png)

### <a name="step-5-optional---configure-pre-authentication-with-azure-ad"></a>步驟 5 \[選擇性\]-使用 Azure AD 設定預先驗證

1. 請參閱檔[Kerberos 限制委派，以使用應用程式 Proxy 單一登入您的應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd)。

2. 設定內部部署 Active Directory。
    - 在 Active Directory 機上，開啟伺服器管理員並移至 [ **工具**] > **Active Directory [使用者和電腦**]。
    - 流覽至 [**電腦**] 節點，然後選取連接器伺服器。
    - 以滑鼠右鍵按一下，然後選取  **屬性** -> **委派**  索引標籤。
    - 選取 [ **信任這台電腦，但只委派指定的服務**]。
    - 選取 [ **使用任何驗證通訊協定**]。
    - 在 [ **此帳戶可以顯示委派認證的服務**] 下。
        - 新增列印伺服器電腦的服務主體名稱（SPN）。
        - 針對 [服務類型] 選取 [主機]。
    ![Active Directory 委派](../media/hybrid-cloud-print/AD-Delegation.png)

3. 確認 IIS 中已啟用 [Windows 驗證]。
    - 在列印伺服器上，開啟 [伺服器管理員 > 工具] > [網際網路資訊服務（IIS）管理員]。
    - 流覽至網站。
    - 按兩下 [**驗證**]。
    - 按一下 [ **Windows 驗證**]，然後按一下 [**動作**] 底下的 [**啟用**]。
    ![列印伺服器 IIS 驗證](../media/hybrid-cloud-print/PrintServer-IIS-Authentication.png)

4. 設定單一登入。
    - 在 Azure 入口網站上，移至**Azure Active Directory** > 的**企業應用程式** > **所有應用程式**。
    - 選取 [MopriaDiscoveryService 應用程式]。
    - 移至 [**應用程式 proxy**]。 將 [預先驗證方法] 變更為**Azure Active Directory**。
    - 移至 [**單一登入**]。 選取 [整合式 Windows 驗證] 做為單一登入方法。
    - 將 [**內部應用程式 spn** ] 設定為列印伺服器電腦的 spn。
    - 將 **[委派的登**入身分識別] 設定為 [使用者主體名稱]。
    - 針對 EntperiseCloudPrint 應用程式重複此步驟。
    ![AAD 單一登入 IWA](../media/hybrid-cloud-print/AAD-SingleSignOn-IWA.png)

### <a name="step-6---configure-the-required-mdm-policies"></a>步驟 6-設定所需的 MDM 原則

1. 登入您的 MDM 提供者。
2. 尋找「企業雲端列印原則」群組，並遵循下列指導方針來設定原則：
    - CloudPrintOAuthAuthority = `https://login.microsoftonline.com/<Azure AD Directory ID>`。 您可以在 Azure Active Directory > 屬性 下找到目錄識別碼。
    - CloudPrintOAuthClientId = 應用程式 \(原生應用程式的用戶端\) 識別碼值。 您可以在 Azure Active Directory > 應用程式註冊中找到此，> 選取原生應用程式 > 總覽。
    - CloudPrinterDiscoveryEndPoint = Mopria 探索服務應用程式的外部 URL。 您可以在 Azure Active Directory > 企業應用程式 下找到此 > 選取 Mopria 探索服務應用程式 > 應用程式 proxy。 **它必須完全相同，但不含尾端/** 。
    - MopriaDiscoveryResourceId = Mopria 探索服務應用程式的應用程式識別碼 URI。 您可以在 Azure Active Directory > 應用程式註冊中找到此，> 選取 [Mopria 探索服務] 應用程式 > [總覽]。 **它必須與尾端/完全相同**。
    - CloudPrintResourceId = 企業雲端列印應用程式的應用程式識別碼 URI。 您可以在 Azure Active Directory > 應用程式註冊中找到此 > 選取 [企業雲端列印應用程式] > 總覽。 **它必須與尾端/完全相同**。
    - DiscoveryMaxPrinterLimit = \<正整數\>。

> 注意：如果您使用 Microsoft Intune 服務，您可以在 [雲端印表機] 類別下找到這些設定。

|Intune 顯示名稱                     |Policy(Windows Intune 說明：原則)                         |
|----------------------------------------|-------------------------------|
|印表機探索 URL                   |CloudPrinterDiscoveryEndpoint  |
|印表機存取授權 URL            |CloudPrintOAuthAuthority       |
|Azure Native client 應用程式 GUID            |CloudPrintOAuthClientId        |
|列印服務資源 URI              |CloudPrintResourceId           |
|要查詢的印表機上限（僅限行動裝置）  |DiscoveryMaxPrinterLimit       |
|印表機探索服務資源 URI  |MopriaDiscoveryResourceId      |

> 注意：如果 [雲端列印原則] 群組無法使用，但 MDM 提供者支援 OMA-URI 設定，則您可以設定相同的原則。  如需其他資訊，請參閱[這裡](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority)。

    - OMA-URI 的值
        - CloudPrintOAuthAuthority =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority
            - 值 = https://login.microsoftonline.com/<Azure AD Directory ID>
        - CloudPrintOAuthClientId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId
            - 值 = < Azure AD 原生應用程式的應用程式識別碼 >
        - CloudPrinterDiscoveryEndPoint =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint
            - 值 = Mopria 探索服務應用程式的外部 URL （必須完全相同，但不含尾端/）
        - MopriaDiscoveryResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId
            - 值 = Mopria 探索服務應用程式的應用程式識別碼 URI
        - CloudPrintResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId
            - 值 = 企業雲端列印應用程式的應用程式識別碼 URI
        - DiscoveryMaxPrinterLimit =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit
            - 值 = 正整數
    
### <a name="step-7---publish-the-shared-printer"></a>步驟 7-發佈共用印表機

1. 在列印伺服器上安裝所需的印表機。
2. 透過 [印表機內容] UI 共用印表機。
3. 選取所需的使用者集合以授與存取權。
4. 儲存變更並關閉 [印表機內容] 視窗。
5. 準備 Windows 10 秋季建立者更新或更新版本的電腦。 將電腦加入 Azure AD，並以與內部部署 Active Directory 同步處理的使用者身分登入，並已獲得 MopriaDeviceDb 檔案的適當許可權。
6. 從 Windows 10 電腦，開啟提升許可權的 Windows PowerShell 命令提示字元。
    - 執行下列命令。
        - `find-module -Name PublishCloudPrinter` 確認機器可以觸達 PowerShell 資源庫（PSGallery）
        - `install-module -Name PublishCloudPrinter`

            > 注意：您可能會看到訊息，指出 ' PSGallery ' 是不受信任的存放庫。  輸入 ' y ' 以繼續安裝。

        - `Publish-CloudPrinter`
        - 印表機 = 共用的印表機名稱。 此名稱必須完全符合印表機內容 [**共用**] 索引標籤中所顯示的共用名稱，並在列印伺服器上開啟。

        ![列印伺服器印表機屬性](../media/hybrid-cloud-print/PrintServer-PrinterProperties.png)

        - 製造商 = 印表機製造商。
        - Model = 印表機型號。
        - OrgLocation = 指定印表機位置的 JSON 字串，例如

            `{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:Microsoft, depth:1}, {category:site, vs:Redmond, WA, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_number, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}`

        - Sddl = 代表印表機權限的 SDDL 字串。
            - 以系統管理員身分登入列印伺服器，然後針對您要發佈的印表機執行下列 PowerShell 命令： `(Get-Printer PrinterName -full).PermissionSDDL`。
            - 在上述命令的結果中，將**O:BA**新增為前置詞。 例如 如果上一個命令所傳回的字串是 G:DUD：（A; OICI; FA;;;WD），然後 SDDL = O:BAG： DUD：（A; OICI; FA;;;WD）。
        - DiscoveryEndpoint = 登入 Azure 入口網站，然後從企業應用程式 > Mopria 探索服務應用程式 > 應用程式 proxy > 外部 URL 取得字串。 省略尾端/。
        - PrintServerEndpoint = 登入 Azure 入口網站，然後從企業應用程式 > 企業雲端列印應用程式 > 應用程式 proxy > 外部 URL 取得字串。 省略尾端/。
        - AzureClientId = 已註冊原生應用程式的應用程式識別碼。
        - AzureTenantGuid = Azure AD 租使用者的目錄識別碼。
        - DiscoveryResourceId = Mopria 探索服務應用程式的應用程式識別碼 URI。

    - 您也可以在命令列中輸入所有必要的參數值。 語法為：

        `Publish-CloudPrinter -Printer <string> -Manufacturer <string> -Model <string> -OrgLocation <string> -Sddl <string> -DiscoveryEndpoint <string> -PrintServerEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        範例命令：

        `Publish-CloudPrinter -Printer HcpTestPrinter -Manufacturer Manufacturer1 -Model Model1 -OrgLocation '{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:MyCompany, depth:1}, {category:site, vs:MyCity, State, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_name, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}' -Sddl O:BAG:DUD:(A;OICI;FA;;;WD) -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -PrintServerEndpoint https://enterprisecloudprint-contoso.msappproxy.net/ecp -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

    - 使用下列命令來確認已發佈印表機。

        `Publish-CloudPrinter -Query -DiscoveryEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        範例命令：

        `Publish-CloudPrinter -Query -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

## <a name="verify-the-deployment"></a>檢查部署

在已設定 MDM 原則的 Azure AD 聯結裝置上：
- 開啟網頁瀏覽器，並移至 https://mopriadiscoveryservice-*的*msappproxy.net/mcs/services。
- 您應該會看到描述此端點功能集的 JSON 文字。
- 移至 [**設定**] [ > **印表機 & 掃描器**] > [**裝置**]。
    - 按一下 [**新增印表機] 或 [掃描器**]。
    - 您應該會看到 [搜尋雲端印表機] （或在較新的 Windows 10 電腦上搜尋我組織中的印表機）連結。
    - 按一下連結。
    - 按一下 [請選取搜尋位置] 連結。
        - 您應該會看到裝置位置階層。
    - 挑選位置，按一下 **[確定]** ，然後按一下 [**搜尋**] 按鈕以尋找印表機。
    - 選取 [印表機]，然後按一下 [**新增裝置**] 按鈕。
    - 成功安裝印表機之後，請從您最愛的應用程式列印到印表機。

> 注意：如果使用 EcpPrintTest 印表機，您可以在列印伺服器電腦的 C：\\ECPTestOutput\\EcpTestPrint. xps location 底下找到輸出檔案。

## <a name="troubleshooting"></a>疑難排解

以下是 HCP 部署期間的常見問題

|錯誤 |建議的步驟 |
|------|------|
|CloudPrintDeploy PowerShell 腳本失敗 | <ul><li>確定 Windows Server 有最新的更新。</li><li>如果使用 Windows Server Update Services （WSUS），請參閱[如何在使用 WSUS/SCCM 時，讓功能隨選和語言套件可用](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs)。</li></ul> |
|SQLite 安裝失敗，訊息：偵測到封裝 ' System.object ' 的相依性迴圈 | Install-Package system.object-providername nuget-SkipDependencies<br>Install-Package EF6-providername nuget-SkipDependencies<br>Install-Package system.string-providername nuget-SkipDependencies<br><br>成功下載套件之後，請確定它們都是相同的版本。 如果不是，請將-requiredversion 參數新增至上述命令，並將它們設定為相同的版本。 |
|發行印表機失敗 | <ul><li>針對 [傳遞預先驗證]，請確定發行印表機的使用者已獲得發行資料庫的適當許可權。</li><li>針對 Azure AD 預先驗證，請確定已在 IIS 中啟用 Windows 驗證。 請參閱步驟5.3。 此外，請先嘗試傳遞預先驗證。 如果通過預先驗證運作，問題可能與應用程式 proxy 相關。 請參閱針對[應用程式 Proxy 問題和錯誤訊息進行疑難排解](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot)。 請注意，切換到 [通過] 會重設單一登入設定;再次流覽步驟5以重新設定 Azure AD 預先驗證。</li></ul> |
|列印工作會保持傳送至印表機狀態 | <ul><li>請確定連接器伺服器上已啟用 TLS 1.2。 請參閱步驟2.1 中的連結文章。</li><li>請確定連接器伺服器上的 HTTP2 已停用。 請參閱步驟2.1 中的連結文章。</li></ul> |

以下是可協助疑難排解的記錄檔位置

|Component |記錄檔位置 |
|------|------|
|Windows 10 用戶端 | <ul><li>使用事件檢視器查看 Azure AD 作業的記錄。 按一下 [**開始**]，然後輸入事件檢視器。 流覽至 [應用程式及服務記錄檔] > Microsoft > Windows > AAD > 作業。</li><li>使用意見反應中樞來收集記錄。 請參閱[使用意見反應中樞應用程式將意見反應傳送給 Microsoft](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)</li></ul> |
|連接器伺服器 | 使用事件檢視器查看應用程式 Proxy 的記錄檔。 按一下 [**開始**]，然後輸入事件檢視器。 流覽至 [應用程式及服務記錄檔] > Microsoft > AadApplicationProxy > 連接器 > 系統管理員。 |
|列印伺服器 | 您可以在 C:\inetpub\logs\LogFiles\W3SVC1. 找到 Mopria 探索服務應用程式和企業雲端列印應用程式的記錄檔。 |
