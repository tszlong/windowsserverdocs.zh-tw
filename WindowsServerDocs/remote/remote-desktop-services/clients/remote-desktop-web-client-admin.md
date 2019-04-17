---
title: 為您的使用者設定遠端桌面 Web 用戶端
description: 說明如何以系統管理員可以設定遠端桌面 web 用戶端。
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 11/2/2018
ms.topic: article
author: Heidilohr
ms.localizationpriority: medium
ms.openlocfilehash: 2cb819a7f91646c61b84c3ee70550af6033ba340
ms.sourcegitcommit: 491c94b25501732c4a4abda533cc62b8bd278ed2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/22/2019
ms.locfileid: "9099181"
---
# 為您的使用者設定遠端桌面 Web 用戶端

遠端桌面 web 用戶端可讓使用者透過相容的網頁瀏覽器存取您組織的遠端桌面基礎結構。 他們將可與遠端應用程式或像它們不論他們的本機電腦與桌面互動。 一旦您設定好您的遠端桌面 web 用戶端，若要開始，需要您的使用者是 URL 它們可以存取用戶端、 他們的認證，以及支援的網頁瀏覽器。

>[!IMPORTANT]
>Web 用戶端目前不支援使用 Azure 應用程式 Proxy，並不完全支援 Web 應用程式 Proxy。 如需詳細資訊，請參閱[應用程式 proxy 服務搭配使用的 RDS](../rds-supported-config.md#using-remote-desktop-services-with-application-proxy-services) 。

## 您將需要設定 web 用戶端

之前開始使用，請記住下列各項：

* 請確定您的[遠端桌面部署](../rds-deploy-infrastructure.md)RD 閘道、 RD 連線代理人和 Windows Server 2016 或 2019年上執行的 RD Web 存取。
* 請確定您的部署已設定的[每一使用者用戶端存取使用權](../rds-client-access-license.md)(Cal)，而不是每個裝置，否則會取用所有授權。
* [更新 Windows 10 KB4025334](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)上安裝 RD 閘道。 更新版本的累積更新可能已經包含此 KB。
* 請確定為 RD 閘道和 RD Web 存取角色設定公開金鑰信任的憑證。
* 請確定您的使用者將會連接到任何電腦正在執行下列作業系統版本的其中一個：
  * Windows 10
  * Windows Server 2008R2 或更新版本

較佳的效能連線至 Windows Server 2016 （或更新版本） 以及 Windows 10 （版本 1611年或更新版本），將會看到您的使用者。

>[!IMPORTANT]
>如果您在預覽期間使用 web 用戶端，且安裝在 1.0.0 之前的版本，您必須第一次解除舊的用戶端，然後再移到新的版本。 如果您收到錯誤，指出 「 web 用戶端安裝時是使用較舊版本的 RDWebClientManagement 及部署新版本之前必須先移除 」 時，請依照下列步驟：
>
>1. 開啟提升權限的 PowerShell 提示字元。
>2. 執行**解除安裝模組 RDWebClientManagement**解除安裝新的模組。
>3. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>4. 執行**安裝模組 RDWebClientManagement-RequiredVersion \<old version> 安裝舊的模組。**
>5. 執行**解除安裝 RDWebClient**解除安裝舊的 web 用戶端。
>6. 執行**解除安裝模組 RDWebClientManagement**解除安裝舊的模組。
>7. 關閉並重新開啟提升權限的 PowerShell 命令提示字元。
>8. 繼續進行一般的安裝步驟，如下所示。

## 如何發佈遠端桌面 web 用戶端

若要安裝 web 用戶端第一次，請依照下列步驟：

1. 在 RD 連線代理人伺服器上，取得針對遠端桌面連線用的憑證，並將它匯出為.cer 檔案。 RD 連線代理人.cer 檔案複製到執行 RD Web 角色的伺服器。
2. 在 RD Web 存取伺服器上，開啟提升權限的 PowerShell 命令提示字元。
3. Windows Server 2016 上，請更新 PowerShellGet 模組，由於收件匣版本不支援安裝 web 用戶端管理模組。 若要更新 PowerShellGet，執行下列 cmdlet:
    ```PowerShell
    Install-Module -Name PowerShellGet -Force
    ```

    >[!IMPORTANT]
    >您將需要重新啟動 PowerShell，才更新才會的生效，否則可能無法運作的模組。

4. 安裝遠端桌面 web 用戶端管理 PowerShell 模組，從 PowerShell 資源庫，使用下列 cmdlet:
    ```PowerShell
    Install-Module -Name RDWebClientManagement
    ```

5. 在那之後，執行下列 cmdlet 以下載最新版的遠端桌面 web 用戶端：
    ```PowerShell
    Install-RDWebClientPackage
    ```

6. 接下來，使用括號值取代為您複製的 RD 代理人之.cer 檔案的路徑中執行此 cmdlet︰
    ```PowerShell
    Import-RDWebClientBrokerCert <.cer file path>
    ```

7. 最後，執行這個 cmdlet 來發佈遠端桌面 web 用戶端：
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```
    請確定您可以在 web 用戶端 URL web 用戶端存取您的伺服器名稱，格式為<https://server_FQDN/RDWeb/webclient/index.html>。 請務必使用伺服器名稱符合 RD Web 存取公開憑證的 url （通常是伺服器 FQDN）。

    >[!NOTE]
    >執行時**發佈 RDWebClientPackage** cmdlet，您可能會看到警告，指出每個裝置 Cal 不支援，即使您的部署針對每一使用者 Cal 設定。 如果您的部署使用的每一使用者 Cal，您可以忽略這個警告。 我們會以確定您已設定限制注意顯示。
8. 當您準備好讓使用者存取 web 用戶端時，只是將它們傳送您建立的 web 用戶端 URL。

>[!NOTE]
>若要查看所有支援的 cmdlet RDWebClientManagement 模組的清單，請在 PowerShell 中執行下列 cmdlet:
>```PowerShell
>Get-Command -Module RDWebClientManagement
>```

## 如何更新遠端桌面 web 用戶端

遠端桌面 web 用戶端的新版本可用時，請依照下列步驟來使用新的用戶端更新部署：

1. 開啟已提升權限的 PowerShell 命令提示字元 RD Web 存取伺服器上，並執行下列 cmdlet 以下載最新的 web 用戶端的可用版本：
    ```PowerShell
    Install-RDWebClientPackage
    ```

2. （選擇性） 您可以發佈用戶端進行正式發行前測試執行此 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Test -Latest
    ```

    用戶端應該會出現在測試 URL 對應至您的 web 用戶端 URL (例如， <https://server_FQDN/RDWeb/webclient-test/index.html>)。
3. 將用戶端以使用者發佈藉由執行下列 cmdlet:
    ```PowerShell
    Publish-RDWebClientPackage -Type Production -Latest
    ```

    當它們重新啟動網頁時，這將會取代為所有使用者的用戶端。

## 如何解除安裝 [遠端桌面 web 用戶端

若要移除所有追蹤 web 用戶端，請依照下列步驟：

1. 在 RD Web 存取伺服器上，開啟提升權限的 PowerShell 命令提示字元。
2. 取消發佈的測試與生產用戶端、 解除安裝所有本機封裝及移除 web 用戶端設定：

   ```PowerShell
   Uninstall-RDWebClient
   ```

3. 解除安裝 [遠端桌面 web 用戶端管理 PowerShell 模組：

   ```PowerShell
   Uninstall-Module -Name RDWebClientManagement
   ```

## 如何安裝遠端桌面 web 用戶端沒有網際網路連線

請依照下列步驟來將 web 用戶端部署到 RD Web 存取的伺服器，不會有網際網路連線。

> [!NOTE]
> 安裝沒有網際網路連線是可用和以上版本中版本 1.0.1 RDWebClientManagement PowerShell 模組。

> [!NOTE]
> 您仍然需要具有存取網際網路，才能下載必要檔案傳輸到伺服器離線之前的系統管理員的電腦。

> [!NOTE]
> 終端使用者電腦現在需要網際網路連線。 這將會在未來版本的用戶端提供完整的離線案例解決。

### 從網際網路存取的裝置

1. 開啟 PowerShell 命令提示字元。

2. 從 PowerShell 資源庫中匯入遠端桌面 web 用戶端管理 PowerShell 模組：
    ```PowerShell
    Import-Module -Name RDWebClientManagement
    ```

3. 下載最新版的遠端桌面 web 用戶端，針對不同的裝置上安裝：
    ```PowerShell
    Save-RDWebClientPackage "C:\WebClient\"
    ```

4. 下載最新版的 RDWebClientManagement PowerShell 模組：
    ```PowerShell
    Find-Module -Name "RDWebClientManagement" -Repository "PSGallery" | Save-Module -Path "C:\WebClient\"
    ```

5. 「 C:\WebClient\ 」 的內容複製到 RD Web 存取伺服器。

### 從 RD Web 存取伺服器

請依照下[如何發佈遠端桌面 web 用戶端](remote-desktop-web-client-admin.md#how-to-publish-the-remote-desktop-web-client)，取代下列步驟 4 和 5 的指示進行。

4. 從本機資料夾中匯入遠端桌面 web 用戶端管理 PowerShell 模組：
    ```PowerShell
    Import-Module -Name "C:\WebClient\"
    ```

5. 部署遠端桌面 web 用戶端，從本機資料夾 （取代為適當的 zip 檔案） 的最新版本：
    ```PowerShell
    Install-RDWebClientPackage -Source "C:\WebClient\rdwebclient-1.0.1.zip"
    ```

## 連線到 RD 代理程式，而不在 Windows Server 2019 的 RD 閘道
本章節描述了如何啟用 RD 代理程式，而不在 Windows Server 2019 RD 閘道 web 用戶端連線。

### 設定 RD 代理人伺服器

#### 請依照下列步驟，如果沒有繫結到 RD 代理人伺服器憑證

1. 開啟**伺服器管理員**\] > **遠端桌面服務**。

2. 在**部署概觀**區段中，選取**工作**下拉式功能表。

3. 選取 \ [**編輯部署內容**，新視窗標題為**部署屬性**將會開啟。

4. 在**部署屬性**視窗中，選取左側功能表中的**憑證**。

5. 在憑證層級清單中，選取 [ **RD 連線代理人-啟用單一登入**。 有兩個選項: （1） 建立新的憑證或 (2) 現有的憑證。

#### 請依照下列步驟，如果有先前繫結到 RD 代理人伺服器憑證

1. 開啟憑證繫結到代理程式和複製**憑證指紋**值。

2. 若要將這個憑證繫結到安全的連接埠 3392，開啟提升權限的 PowerShell 視窗並執行下列命令，以從上一個步驟複製的值取代 **「 < 指紋 > 」** :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" certstorename="Remote Desktop" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要檢查憑證結合是否正確，請執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在清單中的 SSL 憑證繫結，請確定了正確的憑證繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit)，並以 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```並找出**WebSocketURI**的索引鍵。 值必須設定為**https://+:3392/rdp/**。

### 設定 RD 工作階段主機
如果 RD 工作階段主機伺服器此地址不同於 RD 代理人伺服器，請依照下列步驟：

1. 建立 RD 工作階段主機電腦的憑證、 開啟它並複製**憑證指紋**值。

2. 若要將這個憑證繫結到安全的連接埠 3392，開啟提升權限的 PowerShell 視窗並執行下列命令，以從上一個步驟複製的值取代 **「 < 指紋 > 」** :

    ```PowerShell
    netsh http add sslcert ipport=0.0.0.0:3392 certhash="<thumbprint>" appid="{00000000-0000-0000-0000-000000000000}"
    ```

    > [!NOTE]
    > 若要檢查憑證結合是否正確，請執行下列命令：
    >
    > ```PowerShell
    > netsh http show sslcert
    > ```
    >
    > 在清單中的 SSL 憑證繫結，請確定了正確的憑證繫結至連接埠 3392。

3. 開啟 Windows 登錄 (regedit)，並以 nagivate```HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp```並找出**WebSocketURI**的索引鍵。 值必須設定為**https://+:3392/rdp/**。

### 一般的觀察值

* 請確定 RD 工作階段主機和 RD 代理人伺服器執行 Windows Server 2019。

* 請確定該公開信任憑證已設定 RD 工作階段主機和 RD 代理人伺服器。
    > [!NOTE]
    > 如果在 RD 工作階段主機和 RD 代理人伺服器共用相同的電腦，設定只 RD 代理人伺服器憑證。 如果 RD 工作階段主機和 RD 代理程式伺服器使用不同的電腦，兩者都必須具有唯一的憑證設定。

* 針對每個憑證的**主體替代名稱 (SAN)** 必須設為電腦的**完整完整網域名稱 (FQDN)**。 **一般名稱 (CN)** 必須符合每個憑證 SAN。

## 疑難排解

如果第一次開啟 web 用戶端時，使用者會報告任何下列問題，下列各節會告訴您要執行的動作來修正這些問題。

### 如果使用者的瀏覽器顯示安全性警告，當使用者嘗試以存取 web 用戶端怎麼辦？

RD Web 存取角色可能不會使用受信任的憑證。 請確定 RD Web 存取角色使用公開信任憑證設定。

如果無法解決問題，您的伺服器名稱，在 web 用戶端 URL 可能不符合 RD Web 憑證所提供的名稱。 請確定您的 URL 使用裝載的 RD Web 角色的伺服器的 FQDN。

### 如果使用者無法連線到資源使用 web 用戶端即使他們可以看到所有資源下方的項目怎麼辦？

如果使用者報告時，它們無法聯繫 web 用戶端即使他們可以看到列出的資源，請檢查下列各項：

* RD 閘道角色正確設定為使用受信任的公開憑證？
* RD 閘道伺服器之前，是否已安裝的必要的更新？ 請確定您的伺服器有[KB4025334 更新](https://support.microsoft.com/en-us/help/4025334/windows-10-update-kb4025334)安裝。

如果使用者取得 「 未預期的伺服器驗證憑證已收到 」 錯誤訊息，當使用者嘗試以連線，接著就會顯示訊息的憑證指紋。 搜尋 RD 代理人伺服器的憑證管理員 」 中使用該憑證指紋找出正確的憑證。 確認憑證已設定要用於 RD 代理人角色的遠端桌面部署的屬性頁面中。 確定憑證尚未到期之後，將.cer 檔案格式的憑證複製到 RD Web 存取伺服器和 RD Web 存取伺服器上執行下列命令，以括號由憑證的檔案路徑取代的值：

```PowerShell
Import-RDWebClientBrokerCert <certificate file path>
```

### 診斷與主控台記錄檔的問題

如果您無法解決問題，根據本文中的疑難排解的指示，您可以嘗試自行診斷問題的來源，竊聽主控台登入瀏覽器。 Web 用戶端提供一個方法來使用 web 用戶端，來協助診斷問題時錄製的瀏覽器主控台記錄的活動。

* 選取右上角的省略符號，並瀏覽到**相關**頁面，在下拉式功能表中。
* 在**擷取的支援資訊**] 下選取**開始錄製**] 按鈕。
* 在產生您想要診斷的問題 「 web 用戶端中執行的作業。
* 瀏覽至 [**關於**] 頁面，然後選取 [**停止錄製**。
* 您的瀏覽器會自動下載標題為**RD 主控台 Logs.txt**.txt 檔案。 此檔案將包含完整的主機記錄活動重現目標問題時產生。

在主控台也可能會直接透過您的瀏覽器存取。 在主控台通常位於開發人員工具。 例如，您可以存取 Microsoft Edge 中的記錄檔按**F12**鍵，或透過選取省略符號，然後瀏覽到**更多工具** > **開發人員工具**。

## 取得協助 web 用戶端

如果您已遇到這篇文章中的資訊無法解決問題，您可以報告它的 [[傳送電子郵件給我們](mailto:rdwbclnt@microsoft.com)。 您也可以要求或投票新功能，我們[建議方塊](https://aka.ms/rdwebfbk)。