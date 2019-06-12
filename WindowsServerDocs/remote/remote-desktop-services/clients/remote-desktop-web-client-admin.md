---
title: 為您的使用者設定遠端桌面 Web 用戶端
description: 描述如何為系統管理員可以設定遠端桌面 web 用戶端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 45164e9eca0873c82148aa3b7baa179a3f626dd7
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804970"
---
# <a name="set-up-the-remote-desktop-web-client-for-your-users"></a>為您的使用者設定遠端桌面 Web 用戶端

遠端桌面 web 用戶端可讓使用者透過相容的網頁瀏覽器存取貴組織的遠端桌面基礎結構。 他們將能夠與遠端應用程式或桌面，無論是本機電腦一樣進行互動。 一旦您設定您的遠端桌面 web 用戶端時，您的使用者只需要開始會是 URL，他們可以在其中存取用戶端、 他們的認證，以及支援的網頁瀏覽器集。

>[!IMPORTANT]
>Web 用戶端目前不支援使用 Azure 應用程式 Proxy，並不完全支援 Web 應用程式 Proxy。 請參閱[使用應用程式 proxy 服務的使用 RDS](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services)如需詳細資訊。

## <a name="what-youll-need-to-set-up-the-web-client"></a>您必須設定 web 用戶端

開始之前，請記住下列事項：

* 請確定您[遠端桌面部署](../rds-deploy-infrastructure.md)具有 RD 閘道、 RD 連線代理人和 RD Web 存取 Windows Server 2016 或 2019年上執行。
* 請確定您的部署已針對[每個使用者用戶端存取使用權](../rds-client-access-license.md)(Cal) 而不是每個裝置，否則所有授權將會都耗用。
* 安裝[Windows 10 KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)RD 閘道上。 更新版本的累積更新可能已經包含此知識庫文章。
* 請確定公開受信任的憑證已針對 「 RD 閘道和 RD Web 存取 」 角色。
* 請確定您的使用者會連線到任何電腦正在執行下列作業系統版本的其中一個：
  * Windows 10
  * Windows Server 2008 r2 或更新版本

您的使用者會看到較佳的效能會連接到 Windows Server 2016 （或更新版本） 和 Windows 10 （版本 1611年或更新版本）。

>[!IMPORTANT]
>如果您在預覽期間使用的 web 用戶端，並已安裝版本 1.0.0 之前，您必須先解除舊的用戶端，再移至新的版本。 如果您收到錯誤指出 「 web 用戶端使用 RDWebClientManagement 較舊版本所安裝和部署新版本之前必須先移除 」，請遵循下列步驟：
>
>1. 開啟提升權限的 PowerShell 提示字元。
>2. 執行**Uninstall-module RDWebClientManagement**解除安裝新的模組。
>3. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>4. 執行**Install-module RDWebClientManagement-RequiredVersion\<舊版本 > 安裝的舊模組。**
>5. 執行**解除安裝 RDWebClient**解除安裝舊的 web 用戶端。
>6. 執行**Uninstall-module RDWebClientManagement**解除安裝舊模組。
>7. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>8. 繼續執行一般的安裝步驟，如下所示。

## <a name="how-to-publish-the-remote-desktop-web-client"></a>如何發佈遠端桌面 web 用戶端

若要安裝 web 用戶端第一次，請遵循下列步驟：

1. 在 RD 連線代理人伺服器上，取得用於遠端桌面連線的憑證，並將它匯出為.cer 檔案。 RD 連線代理人的.cer 檔案複製到執行 RD Web 角色的伺服器。
2. RD Web 存取伺服器上，開啟提升權限的 PowerShell 提示字元。
3. Windows Server 2016，請更新 PowerShellGet 模組，因為收件匣版本不支援安裝 web 用戶端管理模組。 若要更新 PowerShellGet，請執行下列 cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >您必須更新才能生效，否則為模組可能無法運作，請重新啟動 PowerShell。

4. 安裝遠端桌面 web 用戶端管理的 PowerShell 模組，從 PowerShell 資源庫，搭配這個指令程式：
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 之後，執行下列 cmdlet 來下載遠端桌面 web 用戶端的最新版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接下來，執行這個指令程式使用括號的值取代您複製從 RD Broker 的.cer 檔案的路徑：
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最後，執行這個指令程式可發佈遠端桌面 web 用戶端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    請確定您可以使用您的伺服器名稱，格式為存取 web 用戶端在 web 用戶端 URL <https://server_FQDN/RDWeb/webclient/index.html>。 請務必使用符合 URL （通常是伺服器 FQDN） 中的 RD Web 存取公用憑證的伺服器名稱。

    >[!NOTE]
    >執行時**發佈 RDWebClientPackage** cmdlet，您可能會看到不支援的警告，指出每一裝置 Cal，即使您的部署已針對每個使用者 Cal。 如果您的部署會使用每個使用者 Cal，您可以忽略此警告。 我們會顯示以確定您了解組態限制。
8. 當您準備好供使用者存取 web 用戶端時時，請只對他們傳送您所建立的 web 用戶端 URL。

>[!NOTE]
>若要查看所有支援的 cmdlet RDWebClientManagement 模組清單，請在 PowerShell 中執行下列 cmdlet:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## <a name="how-to-update-the-remote-desktop-web-client"></a>如何更新遠端桌面 web 用戶端

遠端桌面 web 用戶端新版本可用時，請遵循下列步驟以使用新的用戶端更新的部署：

1. 開啟提升權限的 PowerShell 提示字元上的 RD Web 存取伺服器，然後執行下列 cmdlet 來下載 web 用戶端的最新可用版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. （選擇性） 您也可以執行下列 cmdlet 來測試在正式發行之前發行用戶端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    用戶端應該會出現在 測試 URL 對應至您的 web 用戶端 URL (例如<https://server_FQDN/RDWeb/webclient-test/index.html>)。
3. 發行使用者的用戶端藉由執行下列 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    當它們重新啟動 web 網頁時，這會取代所有使用者的用戶端。

## <a name="how-to-uninstall-the-remote-desktop-web-client"></a>如何解除安裝遠端桌面 web 用戶端

若要移除的 web 用戶端的所有追蹤，請遵循下列步驟：

1. RD Web 存取伺服器上，開啟提升權限的 PowerShell 提示字元。
2. 取消發行的測試和實際執行用戶端、 所有的本機套件解除安裝和移除 web 用戶端設定：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 解除安裝遠端桌面 web 用戶端管理的 PowerShell 模組：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## <a name="how-to-install-the-remote-desktop-web-client-without-an-internet-connection"></a>如何安裝遠端桌面 web 用戶端，沒有網際網路連線

請遵循下列步驟來部署到 RD Web 存取伺服器沒有網際網路連線的 web 用戶端。

> [!NOTE]
> 安裝沒有網際網路連線可在 1.0.1 版及更新版本的 RDWebClientManagement PowerShell 模組。

> [!NOTE]
> 您仍然需要下載必要的檔案，再將它們傳送到離線的伺服器存取網際網路的系統管理員電腦。

> [!NOTE]
> 終端使用者電腦現在需要網際網路連線。 這將在未來版本的用戶端提供完整的離線案例中解決。

### <a name="from-a-device-with-internet-access"></a>從具有網際網路存取的裝置

1. 開啟 PowerShell 提示字元。

2. 匯入遠端桌面 web 用戶端管理的 PowerShell 模組，從 PowerShell 資源庫：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 下載最新版的安裝在不同裝置上的遠端桌面 web 用戶端：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下載最新版的 RDWebClientManagement PowerShell 模組：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. 複製的內容 」 C:\WebClient\"到 RD Web 存取伺服器。

### <a name="from-the-rd-web-access-server"></a>從 RD Web 存取伺服器

請依照下列指示[如何發佈遠端桌面 web 用戶端](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)，取代下列的步驟 4 和 5。

4. 從本機資料夾，匯入遠端桌面 web 用戶端管理的 PowerShell 模組：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 部署遠端桌面 web 用戶端，從本機資料夾 （取代為適當的 zip 檔案） 的最新版本：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## <a name="connecting-to-rd-broker-without-rd-gateway-in-windows-server-2019"></a>連線到 RD 代理人，不需要在 Windows Server 2019 中的 RD 閘道
本節說明如何啟用 web 用戶端連線到 RD Broker 不需要在 Windows Server 2019 RD 閘道。

### <a name="setting-up-the-rd-broker-server"></a>設定 RD 代理人伺服器

#### <a name="follow-these-steps-if-there-is-no-certificate-bound-to-the-rd-broker-server"></a>如果沒有繫結至 RD 代理人伺服器的憑證，請遵循下列步驟

1. 開啟**伺服器管理員** > **遠端桌面服務**。

2. 在 **部署概觀**區段中，選取**工作**下拉式功能表。

3. 選取 **編輯部署內容**，新的視窗，標題為**部署屬性**隨即開啟。

4. 在 **部署內容**視窗中，選取**憑證**左側功能表中。

5. 在憑證的層級清單中，選取**RD 連線代理人-啟用單一登入**。 您有兩個選項：（1） 建立新的憑證或 (2) 的現有憑證。

#### <a name="follow-these-steps-if-there-is-a-certificate-previously-bound-to-the-rd-broker-server"></a>如果沒有之前繫結至 RD 代理人伺服器的憑證，請遵循下列步驟

1. 開啟憑證繫結至訊息代理程式和複製**指紋**值。

2. 此憑證繫結至安全的連接埠 3392，開啟提升權限的 PowerShell 視窗並執行下列命令，將 **"< 憑證指紋 >"** 使用上一個步驟複製的值：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要確認憑證是否已正確限制，執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在清單中的 SSL 憑證繫結，請確定正確的憑證，繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit) 並以 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```並找出索引鍵**WebSocketURI**。 值必須設定為<strong>https://+:3392/rdp/</strong>。

### <a name="setting-up-the-rd-session-host"></a>設定 RD 工作階段主機
如果 RD 工作階段主機伺服器不是從 RD 代理人伺服器，請遵循下列步驟：

1. 建立 RD 工作階段主機電腦的憑證、 開啟和複製**指紋**值。

2. 此憑證繫結至安全的連接埠 3392，開啟提升權限的 PowerShell 視窗並執行下列命令，將 **"< 憑證指紋 >"** 使用上一個步驟複製的值：

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要確認憑證是否已正確限制，執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在清單中的 SSL 憑證繫結，請確定正確的憑證，繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit) 並以 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```並找出索引鍵**WebSocketURI**。 值必須設定為<https://+:3392/rdp/>。

### <a name="general-observations"></a>一般的觀察值

* 確定在 RD 工作階段主機 」 和 「 RD 代理人伺服器正在執行 Windows Server 2019。

* 請確定該公用信任憑證設定的 RD 工作階段主機 」 和 「 RD 代理人的伺服器。
    > [!NOTE]
    > 如果 RD 工作階段主機及 RD 代理人伺服器共用相同的電腦，設定只 RD 代理人伺服器憑證。 如果 RD 工作階段主機及 RD 代理人伺服器使用不同的電腦，兩者都必須使用唯一的憑證設定。

* **主體別名 (SAN)** 每個憑證必須設定為電腦的**完全完整網域名稱 (FQDN)** 。 **一般名稱 (CN)** 必須符合每個憑證的 SAN。

## <a name="how-to-pre-configure-settings-for-remote-desktop-web-client-users"></a>如何預先設定的遠端桌面 web 用戶端使用者設定
本節會告訴您如何使用 PowerShell 來設定遠端桌面 web 用戶端部署的設定。 這些 PowerShell cmdlet 可控制使用者能夠變更設定根據貴組織的安全性考量，或供工作流程。 下列設定都位於**設定**web 用戶端的側邊面板。 

### <a name="suppress-telemetry"></a>隱藏遙測
根據預設，使用者可以選擇啟用或停用傳送給 Microsoft 的遙測資料的集合。 如需 Microsoft 收集遙測資料的資訊，請參閱我們的隱私權聲明中的連結透過**關於**側邊面板。

身為管理員，您可以選擇隱藏遙測收集您使用下列 PowerShell cmdlet 的部署：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -SuppressTelemetry $true
   ```

根據預設，使用者可能選取以啟用或停用遙測。 布林值 **$false**會比對預設用戶端行為。 布林值 **$true**停用遙測，並會限制使用者啟用遙測。

### <a name="remote-resource-launch-method"></a>遠端資源啟動方法
根據預設，使用者可以選擇啟動 （1） 瀏覽器或 （2） 藉由下載.rdp 檔案以處理另一個在其電腦上已安裝的用戶端的遠端資源。 身為管理員，您可以選擇限制遠端資源啟動方法，為您的部署，使用下列 Powershell 命令：

   ```PowerShell
    Set-RDWebClientDeploymentSetting -LaunchResourceInBrowser ($true|$false)
   ```
 根據預設，使用者可以選取這兩種啟動方法。 布林值 **$true**會強制使用者啟動瀏覽器中的資源。 布林值 **$false**會強制使用者下載.rdp 檔案来處理的本機安裝的 RDP 用戶端啟動的資源。

### <a name="reset-rdwebclientdeploymentsetting-configurations-to-default"></a>重設為預設值的 RDWebClientDeploymentSetting 組態
若要重設所有的部署層級的 web 用戶端設定為預設設定，執行下列 PowerShell cmdlet:

   ```PowerShell
    Reset-RDWebClientDeploymentSetting 
   ```

## <a name="troubleshooting"></a>疑難排解

如果第一次開啟 web 用戶端時，使用者會回報任何下列的問題，下列各節會告訴您該如何加以修正。

### <a name="what-to-do-if-the-users-browser-shows-a-security-warning-when-they-try-to-access-the-web-client"></a>如果使用者的瀏覽器會顯示安全性警告，在嘗試存取 web 用戶端時該怎麼辦

RD Web 存取角色可能不會使用受信任的憑證。 確定 RD Web 存取角色已公開的受信任的憑證。

如果不行，您的伺服器名稱，在 web 用戶端 URL 可能不符合 RD Web 憑證所提供的名稱。 請確定您的 URL 會使用裝載 RD Web 角色的伺服器 FQDN。

### <a name="what-to-do-if-the-user-cant-connect-to-a-resource-with-the-web-client-even-though-they-can-see-the-items-under-all-resources"></a>如果使用者無法連線到資源與 web 用戶端即使他們可以看到所有資源 底下的項目，該怎麼辦

如果使用者回報雖然他們可以看到所列出的資源無法連線它們與 web 用戶端，請檢查下列事項：

* RD 閘道角色正確設定為使用受信任公用憑證？
* RD 閘道伺服器是否已安裝必要的更新？ 請確定您的伺服器有[KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)安裝。

如果使用者可以獲得 「 已收到未預期的伺服器驗證憑證 」 錯誤訊息，當使用者試著連接，則訊息會顯示憑證的指紋。 搜尋 「 RD 代理人 」 伺服器的憑證管理員，請使用該憑證指紋來尋找正確的憑證。 確認憑證已設定要用於 [遠端桌面部署] 屬性頁面中的 RD 代理人角色。 確定憑證尚未過期之後，將憑證的.cer 檔案格式複製到 RD Web 存取伺服器和 RD Web 存取伺服器上執行下列命令，使用憑證的檔案路徑來取代括號的值：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### <a name="diagnose-issues-with-the-console-log"></a>診斷主控台記錄檔的問題

如果您無法解決問題疑難排解的指示，在這篇文章為基礎，您可以嘗試自行診斷問題的來源，觀看主控台登入瀏覽器。 Web 用戶端提供用於使用 web 用戶端，以協助診斷問題時記錄瀏覽器主控台記錄檔 」 活動的方法。

* 在右上角選取省略符號，並瀏覽至**關於**下拉式清單中的頁面。
* 底下**擷取支援資訊**選取**開始錄製** 按鈕。
* 產生您嘗試診斷問題的 web 用戶端中執行的作業。
* 瀏覽至**關於**頁面，然後選取**停止錄製**。
* 您的瀏覽器將會自動下載標題為.txt 檔案**RD 主控台 Logs.txt**。 這個檔案會包含可重現目標問題時產生完整的主控台記錄活動。

也會透過您的瀏覽器直接存取主控台。 在主控台通常位於開發人員工具。 比方說，您可以存取 Microsoft Edge 中的記錄檔，藉由按下**F12**鍵，或藉由選取省略符號，然後巡覽至**更多工具** > **開發人員工具**.

## <a name="get-help-with-the-web-client"></a>取得 web 用戶端的協助

如果您遇到無法在這篇文章中的資訊來解決問題，您可以[電子郵件傳送到](mailto:rdwbclnt@microsoft.com)地回報問題。 您也可以要求或票選新功能，在我們[建議方塊](https://aka.ms/rdwebfbk)。
