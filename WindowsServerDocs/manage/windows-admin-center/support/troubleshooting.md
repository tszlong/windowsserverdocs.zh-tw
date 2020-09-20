---
title: Windows Admin Center 一般疑難排解步驟
description: Windows Admin Center 一般疑難排解步驟 (Project Honolulu)
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: 1775149495871353ef250eff3cb8f6f8cc5c22d6
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766201"
---
# <a name="troubleshooting-windows-admin-center"></a>Windows Admin Center 疑難排解

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Important]
> 此指南將協助您診斷並解決讓您無法使用 Windows Admin Center 的問題。 如果您的特定工具發生問題，請檢查您是否遇到了[已知問題。](./known-issues.md)

## <a name="installer-fails-with-message-_the-module-microsoftpowershelllocalaccounts-could-not-be-loaded_"></a>安裝程式失敗並出現訊息： **_無法載入模組 ' LocalAccounts '。_**

如果您的預設 PowerShell 模組路徑已經修改或移除，就會發生這種情況。 若要解決此問題，請確定這 ```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules``` 是您 PSModulePath 環境變數中的 **第一個** 專案。 您可以使用下列這一行 PowerShell 來達成此目的：

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>我在網頁瀏覽器中收到**無法連線到此網站/頁面**錯誤

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>如果您已將 Windows Admin Center 安裝為 **Windows 10 上的應用程式**

* 請檢查以確定 Windows Admin Center 正在執行中。 尋找系統匣中的 Windows Admin Center 圖示 ![ Windows Admin Center 圖示， ](../media/trayIcon.PNG) 或工作管理員中的 **Windows Admin Center 桌面/SmeDesktop.exe** 。 如果找不到，請從 [開始] 功能表啟動 **\[Windows Admin Center\]**。

> [!NOTE]
> 重新啟動之後，您必須從 [開始] 功能表啟動 Windows Admin Center。

* [檢查 Windows 版本](#check-the-windows-version)

* 確定您用來做為網頁瀏覽器的是 Microsoft Edge 或 Google Chrome。

* [第一次啟動](../use/get-started.md#selecting-a-client-certificate)時，您是否選取了正確的憑證？

  * 嘗試在私人工作階段中開啟瀏覽器，如果可以運作，您需要清除快取。

* 您最近將 Windows 10 升級為新的組建或版本嗎？

  * 這可能已清除您信任的主機設定。 [遵循這些指示來更新您的受信任主機設定。](#configure-trustedhosts)

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>如果您已將 Windows Admin Center 安裝為 **Windows Server 上的閘道**

* 請為用戶端和伺服器[檢查 Windows 版本](#check-the-windows-version)。

* 確定您用來做為網頁瀏覽器的是 Microsoft Edge 或 Google Chrome。

* 在伺服器上，開啟工作管理員 > Services，並確定 **ServerManagementGateway/Windows Admin Center** 正在執行。

    ![工作管理員-服務] 索引標籤](../media/Service-TaskMan.PNG)

* 測試與閘道的網路連線 (取代為 \<values> 您的部署中的資訊) 

    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>如果您已在 Azure Windows Server VM 中安裝 Windows Admin Center

* [檢查 Windows 版本](#check-the-windows-version)
* 您是否已新增 HTTPS 的輸入連接埠規則？
* [深入了解如何在 Azure VM 中安裝 Windows Admin Center](../azure/azure-integration.md)

### <a name="check-the-windows-version"></a>檢查 Windows 版本

* 開啟 [執行] 對話方塊 (Windows 鍵 + R) 並啟動 ```winver```。

* 如果您使用的是 Windows 10 版本 1703 或較早的版本，您的 Microsoft Edge 版本不支援 Windows Admin Center。 請升級至最新版本的 Windows 10，或使用 Chrome。

* 如果您使用的是 Windows 10 或伺服器的 insider preview 版本，其組建版本介於17134和17637之間，則 Windows 會造成 Windows Admin Center 失敗的 bug。 請使用目前支援的 Windows 版本。

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>確定 Windows 遠端管理 (WinRM) 服務正在閘道電腦和受控節點上執行

* 使用 WindowsKey + R 開啟 [執行] 對話方塊
* 輸入 ```services.msc``` ，然後按 enter 鍵
* 在開啟的視窗中，尋找 Windows 遠端管理 (WinRM) ，確定它正在執行中，並設定為自動啟動

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>您是否已將伺服器從2016升級為2019？

* 這可能已清除您信任的主機設定。 [遵循這些指示來更新您的受信任主機設定。](#configure-trustedhosts)

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>我收到下列訊息：「無法安全地連接到這個頁面。 這可能是因為網站使用過期或不安全的 TLS 安全性設定。

您的電腦受限於 HTTP/2 連線。 Windows Admin Center 使用不受 HTTP/2 支援的整合式 Windows 驗證。 在執行 ```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters``` **瀏覽器的電腦** 上，于機碼底下新增下列兩個登錄值，以移除 HTTP/2 限制：

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>我在使用遠端桌面、事件和 PowerShell 工具時遇到問題。

這三個工具需要 websocket 通訊協定，此通訊協定通常會由 proxy 伺服器和防火牆封鎖。 如果您使用 Google Chrome，則會有 websocket 和 NTLM 驗證的 [已知問題](known-issues.md#google-chrome) 。

## <a name="i-can-connect-to-some-servers-but-not-others"></a>我可以連線到一些伺服器，但連線至其他則不行

* 在本機登入閘道電腦並嘗試 ```Enter-PSSession <machine name>``` 在 PowerShell 中，以 \<machine name> 您嘗試在 Windows Admin Center 中管理的機器名稱取代。

* 如果您的環境使用工作群組而不使用網域，請參閱[在工作群組中使用 Windows Admin Center](#using-windows-admin-center-in-a-workgroup)。

* **使用本機系統管理員帳戶：** 如果您正在使用的本機使用者帳戶不是內建的 Administrator 帳戶，則必須以目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以啟用目標電腦上的原則：

    ```
    REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
    ```

## <a name="using-windows-admin-center-in-a-workgroup"></a>在工作群組中使用 Windows Admin Center

### <a name="what-account-are-you-using"></a>您正在使用哪個帳戶？
請確定您使用的認證是目標伺服器本機 Administrator 群組的成員。 在某些情況下，WinRM 也必須有 Remote Management Users 群組的成員資格。 如果您使用的本機使用者帳戶不是**內建的 Administrator 帳戶**，您必須以目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以啟用目標電腦上的原則：

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>您是否正在連接不同子網路上的工作群組機器？

若要連線至並非與閘道位於相同子網路的工作群組電腦，請確定 WinRM 的防火牆連接埠 (TCP 5985) 允許目標電腦上的輸入流量。 您可以使用目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以建立防火牆規則：

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### <a name="configure-trustedhosts"></a>設定 TrustedHosts

安裝 Windows Admin Center 時，您可以選擇讓 Windows Admin Center 管理閘道的 TrustedHosts 設定。 在工作群組環境中，或在網域中使用本機系統管理員認證時，這是必要的。 如果您選擇省略這項設定，就必須手動設定 TrustedHosts。

**若要使用 PowerShell 命令修改 TrustedHosts：**

1. 開啟系統管理員 PowerShell 工作階段。
2. 檢視目前的 TrustedHosts 設定：

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

   > [!WARNING]
   > 如果 TrustedHosts 目前的設定不是空的，以下命令將會覆寫您的設定。 建議您使用下列命令將目前的設定儲存到文字檔案，以便在需要時可加以還原：
   >
   > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. 將 TrustedHosts 設定為您想要管理的電腦的 NetBIOS、IP 或 FQDN：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

   > [!TIP]
   > 如需一次就能設定所有 TrustedHosts 的簡便方法，您可以使用萬用字元。
   >
   > ```powershell
   > Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'
   > ```

4. 當您完成測試時，可以從提升權限的 PowerShell 工作階段發出下列命令來清除 TrustedHosts 設定：

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. 如果先前已匯出您的設定，請開啟檔案、複製值，並使用此命令：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>我之前已安裝 Windows Admin Center，但現在沒有任何其他專案可以使用相同的 TCP/IP 埠

在提高許可權的命令提示字元中，手動執行這兩個命令：

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

## <a name="azure-features-dont-work-properly-in-edge"></a>Azure 功能在 Edge 中無法正常運作

Edge 有與安全性區域相關的 [已知問題](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge) ，這些問題會影響 Windows Admin Center 中的 Azure 登入。 如果您在使用邊緣時遇到問題，請嘗試將 https://login.microsoftonline.com https://login.live.com 閘道的 URL 新增為信任的網站，並新增至用戶端瀏覽器上允許的邊緣快顯封鎖程式設定的網站。

若要這樣做：
1. 在 Windows [開始] 功能表中搜尋**網際網路選項**
2. 移至 [ **安全性** ] 索引標籤
3. 在 [信任的網站] 選項底下，按一下 [網站] 按鈕，然後在開啟的對話方塊中新增 URL。 您將需要新增您的閘道 URL，以及 https://login.microsoftonline.com 和 https://login.live.com 。
4. 移至 [ **隱私權** ] 索引標籤
5. 在 [快顯封鎖 **程式** ] 區段下，按一下 [ **設定** ] 按鈕，然後在開啟的對話方塊中新增 url。 您將需要新增您的閘道 URL，以及 https://login.microsoftonline.com 和 https://login.live.com 。

## <a name="having-an-issue-with-an-azure-related-feature"></a>有 Azure 相關功能的問題嗎？

請傳送電子郵件給我們， wacFeedbackAzure@microsoft.com 並提供下列資訊：
* [下列問題](#providing-feedback-on-issues)的一般問題資訊。
* 描述您的問題，以及重現問題所採取的步驟。
* 您先前是否使用 New-AadApp.ps1 可下載的腳本向 Azure 註冊您的閘道，然後升級至1807版？ 或者，您是否使用 UI，從 Azure 的閘道 > 設定將您的閘道註冊至 Azure？
* 您的 Azure 帳戶是否與多個目錄/租使用者相關聯？
    * 如果是：向 Windows Admin Center 註冊 Azure AD 應用程式時，您在 Azure 中使用的是您的預設目錄嗎？
* 您的 Azure 帳戶是否有權存取多個訂用帳戶？
* 您使用的訂用帳戶是否已附加帳單？
* 您是否已在遇到問題時登入多個 Azure 帳戶？
* 您的 Azure 帳戶是否需要多重要素驗證？
* 您正在嘗試管理 Azure VM 的電腦嗎？
* Windows Admin Center 是否已安裝在 Azure VM 上？

## <a name="providing-feedback-on-issues"></a>提供問題的意見反應

移至 [事件檢視器] > [應用程式及服務] > [Microsoft-ServerManagementExperience]，並尋找任何錯誤或警告。

在描述您所遇問題的 [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) 上，向我們提報錯誤。

請包含任何您在事件記錄檔中找到的錯誤或警告，以及下列資訊：

* **安裝** Windows Admin Center 所在的平台 (Windows 10 或 Windows Server)：
    * 如果安裝在伺服器上，執行**瀏覽器**的 Windows[版本](#check-the-windows-version)的電腦可存取 Windows Admin Center：
    * 您是否使用安裝程式所建立的自我簽署憑證？
    * 如果您使用您自己的憑證，主體名稱是否符合電腦？
    * 如果您使用您自己的憑證，此憑證是否指定替代主體名稱？
* 您是否使用了預設連接埠設定來安裝？
    * 如果不是，您指定了哪個連接埠？
* **安裝** Windows Admin Center 的電腦是否已加入網域？
* **安裝** Windows Admin Center 所在的 Windows [版本](#check-the-windows-version)：
* 您要**嘗試管理**的電腦是否已加入網域？
* 您要**嘗試管理**之電腦的 Windows [版本](#check-the-windows-version)：
* 您使用何種瀏覽器？
    * 如果您使用 Google Chrome，版本為何？ ([說明] > [關於 Google Chrome])