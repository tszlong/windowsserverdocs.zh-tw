---
title: Windows Admin Center 一般疑難排解步驟
description: Windows Admin Center 一般疑難排解步驟 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 02/12/2019
ms.openlocfilehash: 53c943ee3eddbe8f67bec125961eb3d36ead17a3
ms.sourcegitcommit: 21165734a0f37c4cd702c275e85c9e7c42d6b3cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2019
ms.locfileid: "65034476"
---
# <a name="troubleshooting-windows-admin-center"></a>Windows Admin Center 疑難排解

>適用於：Windows Admin Center，Windows Admin Center 預覽

> [!Important]
> 此指南將協助您診斷並解決讓您無法使用 Windows Admin Center 的問題。 如果您的特定工具發生問題，請檢查您是否遇到了[已知問題。](http://aka.ms/wacknownissues)

<a id="toc"></a>

## <a name="quick-links"></a>快速連結

* [安裝程式失敗訊息： **_無法載入模組 'Microsoft.PowerShell.LocalAccounts'。_**  ](#psmodulepath)

* 我在網頁瀏覽器中收到 **無法連線到此網站/頁面** 錯誤 (選取您的部署類型)
    * [我已安裝為 Windows 10 上的應用程式的 Windows Admin Center](#whitescreenw10)
    * [我有作為閘道在 Windows Server 上安裝的 Windows Admin Center](#whitescreenws)
    * [我有作為閘道的 Azure VM 上安裝的 Windows Admin Center](#if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm)

* [Windows Admin Center 首頁上的負載，但我卡在 [新增連接] 窗格中，或我無法連線到任何電腦。](#winvercompat)

* [我會收到訊息：「 載入模組時發生錯誤。Rpc:過期的重試次數 'Ping'。 」](#winvercompat)

* [我會收到訊息：「 無法安全地連線到此頁面。這可能是因為站台使用過期或不安全的 TLS 安全性設定。 」](#tls)

* [我無法使用遠端桌面、 事件和 PowerShell 工具。](#websockets)

* [我有使用 Edge 中的 Azure 功能的問題](#azlogin)

* [我可以連線到某些伺服器，但不是會針對其他](#connectionissues)

* [我在使用中的 Windows Admin Center**工作群組**](#workgroup)

* [先前安裝的 Windows Admin Center 和現在沒有其他項目可以使用相同的 TCP/IP 連接埠](#urlacl)

* [我的問題並未列出，或在本頁的步驟沒有解決我的問題。](#filebug)

<a id="psmodulepath"></a>

## <a name="installer-fails-with-message-the-module-microsoftpowershelllocalaccounts-could-not-be-loaded"></a>安裝程式會失敗，訊息： **_無法載入模組 'Microsoft.PowerShell.LocalAccounts'。_** 

如果已修改或移除預設 PowerShell 模組路徑，會發生這項目。 若要解決此問題，請確定```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules```已**第一次**您 PSModulePath 環境變數中的項目。 您可以使用下列 PowerShell 程式來達到此目的：

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## <a name="i-get-a-this-sitepage-cant-be-reached-error-in-my-web-browser"></a>我在網頁瀏覽器中收到**無法連線到此網站/頁面**錯誤

<a id="whitescreenw10"></a>

### <a name="if-youve-installed-windows-admin-center-as-an-app-on-windows-10"></a>如果您已將 Windows Admin Center 安裝為 **Windows 10 上的應用程式**

* 請檢查以確定 Windows Admin Center 正在執行中。 尋找 Windows Admin Center 圖示![](../media/trayIcon.PNG)系統匣中或**Windows Admin Center 桌面 / SmeDesktop.exe**工作管理員 中。 如果找不到，請從 [開始] 功能表啟動 **\[Windows Admin Center\]** 。

> [!NOTE] 
> 重新啟動之後，您必須從 [開始] 功能表啟動 Windows Admin Center。  

* [檢查 Windows 版本](#winvercompat)

* 確定您用來做為網頁瀏覽器的是 Microsoft Edge 或 Google Chrome。

* [第一次啟動](../use/get-started.md#selecting-a-client-certificate)時，您是否選取了正確的憑證？

  * 嘗試在私人工作階段中開啟瀏覽器，如果可以運作，您需要清除快取。

* 沒有您最近將 Windows 10 升級至新的組建或版本？

  * 這可能會清除您信任的主控件的設定。 [請遵循這些指示來更新您的受信任的主控件設定。](#configure-trustedhosts) 

[[回到頁首]](#toc)

<a id="whitescreenws"></a>

### <a name="if-youve-installed-windows-admin-center-as-a-gateway-on-windows-server"></a>如果您已將 Windows Admin Center 安裝為 **Windows Server 上的閘道**

* 您未升級從舊版的 Windows Admin Center 嗎？ 請檢查以確定原因未刪除防火牆規則[已知問題這](known-issues.md#upgrade)。 您可以使用下列 PowerShell 命令來判斷規則是否存在。 如果沒有，請遵循[這些指示](known-issues.md#upgrade)來重新建立它。
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* 請為用戶端和伺服器[檢查 Windows 版本](#winvercompat)。

* 確定您用來做為網頁瀏覽器的是 Microsoft Edge 或 Google Chrome。

* 在伺服器上，開啟 工作管理員 > 服務，並確定**ServerManagementGateway / Windows Admin Center**正在執行。
![](../media/Service-TaskMan.PNG)

* 測試網路連線至閘道 (取代\<值 > 您的部署中的資訊)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[回到頁首]](#toc)

### <a name="if-you-have-installed-windows-admin-center-in-an-azure-windows-server-vm"></a>如果您已安裝 Windows Admin Center，在 Azure Windows Server VM

* [檢查 Windows 版本](#winvercompat)
* 您是否已新增 HTTPS 的輸入連接埠規則？ 
* [深入了解 Azure VM 中安裝 Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[回到頁首]](#toc)

<a id="winvercompat"></a>

### <a name="check-the-windows-version"></a>檢查 Windows 版本

* 開啟 [執行] 對話方塊 (Windows 鍵 + R) 並啟動 ```winver```。

* 如果您使用的是 Windows 10 版本 1703 或較早的版本，您的 Microsoft Edge 版本不支援 Windows Admin Center。 請升級至最新版本的 Windows 10，或使用 Chrome。

* 如果您使用 Windows 10 或 Server insider preview 版本 17134 和 17637 之間的組建版本，Windows 就會有錯誤導致失敗的 Windows Admin Center。 請使用目前支援的 Windows 版本。

### <a name="make-sure-the-windows-remote-management-winrm-service-is-running-on-both-the-gateway-machine-and-managed-node"></a>請確定在閘道電腦和受管理的節點上執行的 Windows 遠端管理 (WinRM) 服務

* 開啟 [執行] 對話方塊 WindowsKey + R
* 型別```services.msc```然後按 enter 鍵
* 在開啟的視窗中，尋找 Windows 遠端管理 (WinRM)，請確定它正在執行且設定為自動啟動

### <a name="did-you-upgrade-your-server-from-2016-to-2019"></a>是否已升級您的伺服器從 2016年到 2019年？

* 這可能會清除您信任的主控件的設定。 [請遵循這些指示來更新您的受信任的主控件設定。](#configure-trustedhosts) 

[[回到頁首]](#toc)

<a id="tls"></a>

## <a name="i-get-the-message-cant-connect-securely-to-this-page-this-might-be-because-the-site-uses-outdated-or-unsafe-tls-security-settings"></a>我會收到訊息：「 無法安全地連線到此頁面。 這可能是因為站台使用過期或不安全的 TLS 安全性設定。

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
只有 HTTP/2 連線到您的電腦。 Windows Admin Center 會使用整合式的 Windows 驗證，不支援 HTTP/2。 新增下列兩個登錄值之下```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters```上的索引鍵**執行瀏覽器的電腦**移除 HTTP/2 限制：

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[回到頁首]](#toc)

<a id="websockets"></a> 

## <a name="im-having-trouble-with-the-remote-desktop-events-and-powershell-tools"></a>我無法使用遠端桌面、 事件和 PowerShell 工具。

這些工具需要的 websocket 通訊協定，通常是由 proxy 伺服器和防火牆封鎖。 如果您使用 Google Chrome，則[已知問題](known-issues.md#google-chrome)websockets 和 NTLM 驗證。

[[回到頁首]](#toc)


<a id="connectionissues"></a> 

## <a name="i-can-connect-to-some-servers-but-not-others"></a>我可以連線到一些伺服器，但連線至其他則不行
* 登入閘道機器本機，並嘗試```Enter-PSSession <machine name>```在 PowerShell 中，取代\<電腦名稱 > 您嘗試在 Windows Admin Center 中管理的電腦的名稱。 

* 如果您的環境使用工作群組而不使用網域，請參閱[在工作群組中使用 Windows Admin Center](#workgroup)。

* **使用本機系統管理員帳戶：** 如果您使用的不是內建的 administrator 帳戶的本機使用者帳戶，您必須藉由執行下列命令在 PowerShell 中或在命令提示字元以系統管理員身分在目標電腦上啟用目標電腦上的原則：

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[回到頁首]](#toc)

<a id="workgroup"></a>

## <a name="using-windows-admin-center-in-a-workgroup"></a>在工作群組中使用 Windows Admin Center 

### <a name="what-account-are-you-using"></a>您正在使用哪個帳戶？
請確定您使用的認證是目標伺服器本機 Administrator 群組的成員。 在某些情況下，WinRM 也必須有 Remote Management Users 群組的成員資格。 如果您使用的本機使用者帳戶不是**內建的 Administrator 帳戶**，您必須以目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以啟用目標電腦上的原則：

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### <a name="are-you-connecting-to-a-workgroup-machine-on-a-different-subnet"></a>您要連線至不同子網路上的工作群組電腦嗎？

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

**若要修改 TrustedHosts 使用 PowerShell 命令：**

1. 開啟系統管理員 PowerShell 工作階段。
2. 檢視目前的 TrustedHosts 設定：

    ```powershell
    Get-Item WSMan:\localhost\Client\TrustedHosts
    ```

    > [!WARNING]
    > 如果 TrustedHosts 目前的設定不是空的，以下命令將會覆寫您的設定。 建議您使用下列命令將目前的設定儲存到文字檔案，以便在需要時可加以還原：

    > `Get-Item WSMan:localhost\Client\TrustedHosts | Out-File C:\OldTrustedHosts.txt`

3. 將 TrustedHosts 設定為您想要管理的電腦的 NetBIOS、IP 或 FQDN：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '192.168.1.1,server01.contoso.com,server02'
    ```

    > [!TIP] 
    >如需一次就能設定所有 TrustedHosts 的簡便方法，您可以使用萬用字元。

    >     Set-Item WSMan:\localhost\Client\TrustedHosts -Value '*'

4. 當您完成測試時，可以從提升權限的 PowerShell 工作階段發出下列命令來清除 TrustedHosts 設定：

    ```powershell
    Clear-Item WSMan:localhost\Client\TrustedHosts
    ```

5. 如果先前已匯出您的設定，請開啟檔案、複製值，並使用此命令：

    ```powershell
    Set-Item WSMan:localhost\Client\TrustedHosts -Value '<paste values from text file>'
    ```

[[回到頁首]](#toc)

<a id="urlacl"></a>

## <a name="i-previously-had-windows-admin-center-installed-and-now-nothing-else-can-use-the-same-tcpip-port"></a>先前安裝的 Windows Admin Center 和現在沒有其他項目可以使用相同的 TCP/IP 連接埠

在提升權限的命令提示字元中手動執行這兩個命令：

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[回到頁首]](#toc)

<a id="azlogin"></a>

## <a name="im-having-issues-using-azure-features-in-edge"></a>我有使用 Edge 中的 Azure 功能的問題

邊[已知問題](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge)相關影響 Azure 登入 Windows Admin Center 中的安全性區域。 如果您無法使用 Azure 功能，當使用 Edge 時，再次嘗試新增 https://login.microsoftonline.com， https://login.live.com和信任的網站做為您閘道的 URL，並以允許站台的 Edge 瀏覽器用戶端端上的快顯封鎖程式設定。 

請這樣做：
1. 搜尋**網際網路選項**在 Windows 中開始功能表
2. 移至**安全性** 索引標籤
3. 底下**信任的網站**選項中，按一下 **站台**按鈕，然後在對話方塊中，開啟 新增 Url。 您必須新增您的閘道 URL，以及 https://login.microsoftonline.com和 https://login.live.com。
4. 移至**隱私權** 索引標籤
5. 底下**快顯封鎖程式**區段中，按一下**設定**按鈕，然後在對話方塊中，開啟 新增 Url。 您必須新增您的閘道 URL，以及 https://login.microsoftonline.com和 https://login.live.com。


[[回到頁首]](#toc)

<a id="azissue"></a>

## <a name="having-an-issue-with-an-azure-related-feature"></a>問題與 Azure 相關的功能？

請傳送電子郵件到wacFeedbackAzure@microsoft.com具有下列資訊：
* 從一般的問題資訊[以下列出的問題](#filebug)。 
* 描述您的問題，以及您用來重現問題的步驟。 
* 沒有您先前註冊您的閘道，以使用新 AadApp.ps1 可下載指令碼的 Azure，並接著升級到版本 1807年嗎？ 您是否已註冊您的閘道，使用從閘道設定 UI 在 azure 或者 > Azure 嗎？
* 是您與多個目錄/租用戶相關聯的 Azure 帳戶嗎？
    * 如果是：註冊時的 Azure AD 應用程式，以 Windows Admin Center，已在 Azure 中使用您的預設目錄的目錄？ 
* 您的 Azure 帳戶有存取多個訂用帳戶嗎？
* 您所使用的訂用帳戶是否有附加的帳單？
* 是您登入多個 Azure 帳戶時遇到問題嗎？
* 您的 Azure 帳戶之前，是否需要多重要素驗證？
* 是您嘗試管理 Azure VM 的機器？
* 在 Azure VM 上安裝的 Windows Admin Center？

[[回到頁首]](#toc)

<a id="filebug"></a>

## <a name="still-not-working-or-is-your-issue-not-captured-here-troubleshooting-common-questions"></a>仍不使用時，或您不會擷取以下的問題嗎？ [疑難排解常見的問題]

移至 [事件檢視器] > [應用程式及服務] > [Microsoft-ServerManagementExperience]，並尋找任何錯誤或警告。

在描述您所遇問題的 [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) 上，向我們提報錯誤。

請包含任何您在事件記錄檔中找到的錯誤或警告，以及下列資訊： 

* **安裝** Windows Admin Center 所在的平台 (Windows 10 或 Windows Server)：
    * 如果伺服器上安裝，什麼是 Windows[版本](#winvercompat)的**執行瀏覽器的機器**存取 Windows Admin Center: 
    * 您使用安裝程式所建立的自我簽署的憑證？
    * 如果您使用您自己的憑證，主體名稱是否符合電腦？
    * 如果您使用您自己的憑證，此憑證是否指定替代主體名稱？
* 您是否使用了預設連接埠設定來安裝？
    * 如果不是，您指定了哪個連接埠？
* **安裝** Windows Admin Center 的電腦是否已加入網域？
* **安裝** Windows Admin Center 所在的 Windows [版本](#winvercompat)：
* 您要**嘗試管理**的電腦是否已加入網域？
* 您要**嘗試管理**之電腦的 Windows [版本](#winvercompat)：
* 您使用的是哪個瀏覽器？
    * 如果您使用 Google Chrome，版本為何？ ([說明] > [關於 Google Chrome])

[[回到頁首]](#toc)