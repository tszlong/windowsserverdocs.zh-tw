---
title: 為您的使用者設定遠端桌面 Web 用戶端
description: 說明系統管理員如何設定遠端桌面 Web 用戶端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: d167fb5dfdfbb2a302c2b0fca9286dc034b730e3
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546339"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>為您的使用者設定遠端桌面 Web 用戶端

遠端桌面 Web 用戶端可讓使用者透過相容的網頁瀏覽器存取您的組織的遠端桌面基礎結構。 他們無論身在何處，都將能夠像在本機電腦上一樣與遠端應用程式或桌面互動。 在您設定遠端桌面 Web 用戶端後，您的使用者只需取得可用來存取用戶端、其認證和支援的網頁瀏覽器的 URL，即可展開作業。

>[!IMPORTANT]
>Web 用戶端目前不支援使用 Azure 應用程式 Proxy，也完全不支援 Web 應用程式 Proxy。 如需詳細資訊，請參閱[搭配使用 RDS 與應用程式 Proxy 服務](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services)。

## <a name="what-youll-need-to-set-up-the-web-client"></a>設定 Web 用戶端時所需的項目

開始之前，請留意下列事項：

* 確定您的[遠端桌面部署](../rds-deploy-infrastructure.md)具有執行於 Windows Server 2016 或 2019 的 RD 閘道、RD 連線代理人和 RD Web 存取。
* 確定您的部署已設定了[每一使用者的用戶端存取使用權](../rds-client-access-license.md) (CAL) (而非每一裝置)，否則將會使用所有授權。
* 在 RD 閘道上安裝 [Windows 10 KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)。 更新版本的累積更新可能已包含此 KB。
* 確定已為「RD 閘道」和「RD Web 存取」角色設定公開信任憑證。
* 確定您的使用者將連接的任何電腦執行的是下列其中一個 OS 版本：
  * Windows 10
  * Windows Server 2008R2 或更新版本

連線至 Windows Server 2016 (或更新版本) 和 Windows 10 (1611 版或更新版本) 時，您的使用者將會發現效能有所提升。

>[!IMPORTANT]
>如果您在預覽期間使用 Web 用戶端，且安裝了 1.0.0 之前的版本，則必須先將舊版的用戶端解除安裝，再移轉至新版本。 如果您出現錯誤指出「Web 用戶端是使用較舊版本的 RDWebClientManagement 安裝的，必須先移除才能部署新版本」，請遵循下列步驟：
>
>1. 開啟提升權限的 PowerShell 命令提示字元。
>2. 執行 **Uninstall-Module RDWebClientManagement**，將新模組解除安裝。
>3. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>4. 執行 **Install-Module RDWebClientManagement -RequiredVersion \<舊版本>，以安裝舊模組。**
>5. 執行 **Uninstall-RDWebClient**，將舊版 Web 用戶端解除安裝。
>6. 執行 **Uninstall-Module RDWebClientManagement**，將舊模組解除安裝。
>7. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>8. 繼續執行一般安裝步驟，如下所示。

## <a name="how-to-publish-the-remote-desktop-web-client"></a>如何發佈遠端桌面 Web 用戶端

在第一次安裝 Web 用戶端時，請遵循下列步驟：

1. 在 RD 連線代理人伺服器上，取得用於遠端桌面連線的憑證，並將其匯出為 .cer 檔案。 將 RD 連線代理人的 .cer 檔案複製到執行 RD Web 角色的伺服器。
2. 在 RD Web 存取伺服器上，開啟提升權限的 PowerShell 命令提示字元。
3. 在 Windows Server 2016 上更新 PowerShellGet 模組，因為內建版本不支援安裝 Web 用戶端管理模組。 若要更新 PowerShellGet，請執行下列 Cmdlet：
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >必須重新啟動 PowerShell，更新才能生效，否則模組可能無法運作。

4. 使用下列 Cmdlet，從 PowerShell 資源庫安裝遠端桌面 Web 用戶端管理 PowerShell 模組：
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 然後，執行下列 Cmdlet 以下載最新版的遠端桌面 Web 用戶端：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接著，執行下列 Cmdlet，並將括號內的值取代為您從 RD 代理人複製的 .cer 檔案所在的路徑：
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最後，執行下列 Cmdlet 以發佈遠端桌面 Web 用戶端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    請確定您可以從含有您伺服器名稱的 Web 用戶端 URL (格式為 <https://server_FQDN/RDWeb/webclient/index.html>) 存取 Web 用戶端。 在此 URL 中請務必使用符合 RD Web 存取公開憑證的伺服器名稱 (通常是伺服器 FQDN)。

    >[!NOTE]
    >執行 **Publish-RDWebClientPackage** Cmdlet 時，可能會出現警告指出不支援每一裝置 CAL，即使您的部署已設定每一使用者 CAL，仍是如此。 如果您的部署使用每一使用者 CAL，您可以忽略此警告。 顯示此警告的用意是要確定您了解設定的限制。
8. 當您準備好要讓使用者存取 Web 用戶端時，只需將您建立的 Web 用戶端 URL 傳送給他們即可。

>[!NOTE]
>若要查看 RDWebClientManagement 模組支援的所有 Cmdlet，請在 PowerShell 中執行下列 Cmdlet：
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>如何更新遠端桌面 Web 用戶端

有新版的遠端桌面 Web 用戶端可供使用時，請依照下列步驟將部署更新為新的用戶端：

1. 在 RD Web 存取伺服器上開啟提升權限的 PowerShell 命令提示字元，然後執行下列 Cmdlet 以下載最新提供的 Web 用戶端版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. (選擇性) 您可以執行下列 Cmdlet，以在正式發行之前發佈用戶端進行測試：
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    用戶端應該會出現在與您的 Web 用戶端 URL 相對應的測試 URL 上 (例如 <https://server_FQDN/RDWeb/webclient-test/index.html>)。
3. 執行下列 Cmdlet，為使用者發佈用戶端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    這將會在使用者重新啟動網頁時，取代所有使用者的用戶端。

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>如何解除安裝遠端桌面 Web 用戶端

若要移除 Web 用戶端的所有追蹤，請遵循下列步驟：

1. 在 RD Web 存取伺服器上，開啟提升權限的 PowerShell 命令提示字元。
2. 取消發佈測試和生產用戶端、解除安裝所有本機套件，並移除 Web 用戶端設定：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 解除安裝遠端桌面 Web 用戶端管理 PowerShell 模組：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>如何在沒有網際網路連線的情況下安裝遠端桌面 Web 用戶端

依照下列步驟，將 Web 用戶端部署至沒有網際網路連線的 RD Web 存取伺服器。

> [!NOTE]
> 在 1.0.1 版及更新版本的 RDWebClientManagement PowerShell 模組中，可在沒有網際網路連線的情況下進行安裝。

> [!NOTE]
> 您仍然需要可存取網際網路的系統管理員電腦以下載必要的檔案，再將其傳送至離線的伺服器。

> [!NOTE]
> 使用者電腦目前需要網際網路連線。 此問題將在未來的用戶端版本中解決，以提供完整的離線案例。

### <a name="from-a-device-with-internet-access"></a>從連接網際網路的裝置

1. 開啟 PowerShell 命令提示字元。

2. 從 PowerShell 資源庫匯入遠端桌面 Web 用戶端管理 PowerShell 模組：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 在不同的裝置下載最新版的遠端桌面 Web 用戶端，以進行安裝：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下載最新版的 RDWebClientManagement PowerShell 模組：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. 將 C:\WebClient\" 的內容複製到 RD Web 存取伺服器。

### <a name="from-the-rd-web-access-server"></a>從 RD Web 存取伺服器

依照[如何發佈遠端桌面 Web 用戶端](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)下的指示操作 (請將步驟 4 和 5 取代為下列步驟)。

4. 從本機資料夾匯入遠端桌面 Web 用戶端管理 PowerShell 模組：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 從本機資料夾部署最新版的遠端桌面 Web 用戶端 (請取代為適當的 zip 檔案)：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>連線至 Windows Server 2019 中不含 RD 閘道的 RD 代理人
本節說明如何在 Windows Server 2019 中對 RD 代理人啟用不含 RD 閘道的 Web 用戶端連線。

### <a name="setting-up-the-rd-broker-server"></a>設定 RD 代理人伺服器

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>如果沒有繫結至 RD 代理人伺服器的憑證，請遵循下列步驟

1. 開啟 [伺服器管理員]   > [遠端桌面服務]  。

2. 在 [部署概觀]  區段中，選取 [工作]  下拉式功能表。

3. 選取 [編輯部署內容]  ，標題為「部署內容」  的新視窗隨即開啟。

4. 在 [部署內容]  視窗中，選取左側功能表中的 [憑證]  。

5. 在 [憑證層級] 清單中，選取 [RD 連線代理人 - 啟用單一登入]  。 您有兩個選項：(1) 建立新的憑證或 (2) 使用現有憑證。

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>如果有已繫結至 RD 代理人伺服器的憑證，請遵循下列步驟

1. 開啟繫結至代理人的憑證，並複製 [指紋]  值。

2. 若要將此憑證繫結至安全的連接埠 3392，請開啟提升權限的 PowerShell 視窗，並執行下列命令 (請將 **"< thumbprint >"** 取代為從先前的步驟複製的值)：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要確認憑證是否已正確繫結，請執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在 SSL 憑證繫結的清單中，請確定已將正確的憑證繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit)，然後瀏覽至 ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` 並找出機碼 **WebSocketURI**。 此值必須設為 <strong>https://+:3392/rdp/</strong>。

### <a name="setting-up-the-rd-session-host"></a>設定 RD 工作階段主機
如果 RD 工作階段主機伺服器與 RD 代理人伺服器不相同，請遵循下列步驟：

1. 建立 RD 工作階段主機電腦的憑證，並加以開啟，然後複製 [指紋]  值。

2. 若要將此憑證繫結至安全的連接埠 3392，請開啟提升權限的 PowerShell 視窗，並執行下列命令 (請將 **"< thumbprint >"** 取代為從先前的步驟複製的值)：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要確認憑證是否已正確繫結，請執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在 SSL 憑證繫結的清單中，請確定已將正確的憑證繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit)，然後瀏覽至 ```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp``` 並找出機碼 **WebSocketURI**。 此值必須設為 <https://+:3392/rdp/>。

### <a name="general-observations"></a>一般觀察

* 確定 RD 工作階段主機和 RD 代理人伺服器皆執行 Windows Server 2019。

* 確定已為 RD 工作階段主機和 RD 代理人伺服器設定公開信任憑證。
    > [!NOTE]
    > 如果 RD 工作階段主機和 RD 代理人伺服器共用相同的電腦，請設定 RD 代理人伺服器憑證即可。 如果 RD 工作階段主機和 RD 代理人伺服器使用不同的電腦，則兩者都必須設定唯一的憑證。

* 每個憑證的**主體別名 (SAN)** 都必須設為電腦的**完整網域名稱 (FQDN)** 。 **一般名稱 (CN)** 必須符合每個憑證的 SAN。

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>如何預先設定遠端桌面 Web 用戶端使用者的設定
本節將說明如何使用 PowerShell 來設定您遠端桌面 Web 用戶端部署的設定。 這些 PowerShell Cmdlet 可控制使用者根據您的組織的安全性考量或預定工作流程來變更設定的能力。 下列設定全都位於 Web 用戶端的 [設定]  側邊面板中。

### <a name="suppress-telemetry"></a>抑制遙測
根據預設，使用者可以選擇啟用或停用收集遙測資料以傳送至 Microsoft 的功能。 如需 Microsoft 收集遙測資料的相關資訊，請透過 [關於]  側邊面板中的連結參閱我們的隱私權聲明。

身為系統管理員，您可以選擇使用下列 PowerShell Cmdlet，抑制部署的遙測資料收集：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "SuppressTelemetry" $true
   ```

根據預設，使用者可以選擇啟用或停用遙測。 布林值 **$false** 會比對出預設用戶端行為。 布林值 **$true** 會停用遙測，並限制使用者對遙測的啟用。

### <a name="remote-resource-launch-method"></a>遠端資源啟動方法
根據預設，使用者可以選擇下列遠端資源啟動方式：(1) 在瀏覽器中啟動，或 (2) 下載 .rdp 檔案，並以安裝在電腦上的另一個用戶端進行處理。 身為系統管理員，您可以選擇使用下列 Powershell 命令，限制您的部署必須使用遠端資源啟動方法：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser" ($true|$false)
   ```
 根據預設，使用者可以選取任一啟動方法。 布林值 **$true** 會強制使用者在瀏覽器中啟動資源。 布林值 **$false** 會強制使用者下載 .rdp 檔案並以本機安裝的 RDP 用戶端進行處理，以啟動資源。

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>將 RDWebClientDeploymentSetting 設定重設為預設值

若要將部署層級的 Web 用戶端設定重設為預設組態，請執行下列 PowerShell Cmdlet 並使用 -name 參數指定要重設的設定：

   ```PowerShell
    Reset-RDWebClientDeploymentSetting -Name "LaunchResourceInBrowser"
    Reset-RDWebClientDeploymentSetting -Name "SuppressTelemetry"
   ```

## <a name="troubleshooting"></a>疑難排解

如果使用者在第一次開啟 Web 用戶端時回報了下列任何問題，可以參考以下各節提供指示予以修正。

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>瀏覽器在使用者嘗試存取 Web 用戶端時顯示了安全性警告，該如何處理

RD Web 存取角色可能未使用信任憑證。 請確定已為「RD Web 存取」角色設定了公開信任憑證。

若無法解決問題，表示您在 Web 用戶端 URL 中的伺服器名稱可能不符合 RD Web 憑證所提供的名稱。 請確定您的 URL 使用的是託管 RD Web 角色之伺服器的 FQDN。

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>使用者即使可看到 [所有資源] 下的項目，仍無法透過 Web 用戶端連線至資源時，該如何處理

如果使用者回報他們即使可看到列出的資源，仍無法透過 Web 用戶端加以連線，請檢查下列事項：

* RD 閘道角色是否正確設定為使用信任的公開憑證？
* RD 閘道伺服器是否已安裝必要的更新？ 請確定您的伺服器已安裝 [KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)。

如果在使用者嘗試連線時出現「收到非預期的伺服器驗證憑證」錯誤訊息，訊息將會顯示憑證的指紋。 請使用該指紋搜尋 RD 代理人伺服器的憑證管理員，以找出正確的憑證。 在遠端桌面部署的屬性頁面中，確認該憑證已設定為要用於 RD 代理人角色。 確定憑證尚未過期後，請以 .cer 檔案的格式將憑證複製到 RD Web 存取伺服器，並在 RD Web 存取伺服器上執行下列命令 (請將括號內的值取代為憑證的檔案路徑)：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>使用主控台記錄來診斷問題

如果您無法依據本文中的疑難排解指示解決問題，您可以嘗試自行在瀏覽器中查看主控台記錄，以診斷問題的來源。 利用 Web 用戶端來診斷問題時，可以使用 Web 用戶端所提供的方法來記錄瀏覽器主控台記錄活動。

* 選取右上角的省略符號，並瀏覽至下拉式清單中的 [關於]  頁面。
* 在 [擷取支援資訊]  下方，選取 [開始記錄]  按鈕。
* 在 Web 用戶端中執行先前產生問題使您嘗試診斷的作業。
* 瀏覽至 [關於]  頁面，然後選取 [停止記錄]  。
* 您的瀏覽器將會自動下載標題為「RD 主控台 Logs.txt」  的 .txt 檔案。 此檔案會包含在重現目標問題時產生的完整主控台記錄活動。

您也可以透過瀏覽器直接存取主控台。 主控台通常位於開發人員工具下方。 例如，若要存取 Microsoft Edge 中的記錄，只要按 **F12** 鍵，或選取省略符號並瀏覽至 [更多工具]   > [開發人員工具]  即可。

## <a name="get-help-with-the-web-client"></a>取得網頁用戶端的協助

如果您遇到本文資訊無法解決的問題，您可以在 [Tech Community](https://aka.ms/wvdtc)上回報問題。 您也可以在我們的[建議方塊](https://remotedesktop.uservoice.com/forums/911494-remote-desktop-web-client)要求或票選新功能。
