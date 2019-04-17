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
ms.openlocfilehash: a91a8dcf6f05ef0ef66dee603851150b2145d559
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296959"
---
# Windows Admin Center 疑難排解

>適用於：Windows Admin Center、Windows Admin Center 預覽版

> [!Important]
> 此指南將協助您診斷並解決讓您無法使用 Windows Admin Center 的問題。 如果您的特定工具發生問題，請檢查您是否遇到了[已知問題。](http://aka.ms/wacknownissues)

<a id="toc"></a>

## 快速連結

* [安裝程式失敗，並訊息：**_無法載入模組 'Microsoft.PowerShell.LocalAccounts'。_**](#psmodulepath)

* 我在網頁瀏覽器中收到 **無法連線到此網站/頁面** 錯誤 (選取您的部署類型)
    * [我已在 Windows 10 將 Windows Admin Center 應用程式安裝為應用程式](#whitescreenw10)
    * [我已將 Windows Admin Center 安裝為 Windows Server 上的閘道](#whitescreenws)
    * [我已將 Windows Admin Center 安裝為 Azure VM 上的閘道](#whitescreenazvm)

* [Windows Admin Center 首頁會載入，但我已困難上新增連線] 窗格中，或我無法連線到任何電腦。](#winvercompat)

* [我收到訊息：「Error while loading the module. Rpc: Expired retries 'Ping'.」(載入模組時發生錯誤。Rpc: 重試 'Ping' 過期。)](#winvercompat)

* [我收到訊息: 「 無法安全地連線到此頁面。 這可能是因為的網站使用的過時或不安全的 TLS 安全性設定。 」](#tls)

* [我發生問題，使用遠端桌面、 事件及 PowerShell 的工具。](#websockets)

* [我有問題在 Edge 中使用 Azure 的功能](#azlogin)

* [我可以連線到一些伺服器，但連線至其他則不行](#connectionissues)

* [我是在**工作群組**中使用 Windows Admin Center](#workgroup)

* [先前 Windows Admin Center 安裝，並現在任何其他項目可以使用相同的 TCP/IP 連接埠](#urlacl)

* [這裡沒有列出我的問題，或此頁面的步驟無法解決我的問題。](#filebug)

<a id="psmodulepath"></a>

## 安裝程式失敗，並訊息：**_無法載入模組 'Microsoft.PowerShell.LocalAccounts'。_**

如果已修改或移除您預設 PowerShell 模組的路徑，這可能會發生。 若要解決這個問題，請確定```%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules```是您 PSModulePath 環境變數中的**第一個**項目。 您可以使用下列這行程式 PowerShell 來達到此目的：

```powershell
[Environment]::SetEnvironmentVariable("PSModulePath","%SystemRoot%\system32\WindowsPowerShell\v1.0\Modules;" + ([Environment]::GetEnvironmentVariable("PSModulePath","User")),"User")
```

## 我在網頁瀏覽器中收到**無法連線到此網站/頁面**錯誤

<a id="whitescreenw10"></a>

### 如果您已將 Windows Admin Center 安裝為 **Windows 10 上的應用程式**

* 請檢查以確定 Windows Admin Center 正在執行中。 尋找 Windows Admin Center 圖示![](../media/trayIcon.PNG)系統匣中或**Windows Admin Center 桌面 / SmeDesktop.exe**工作管理員] 中。 如果找不到，請從 [開始] 功能表啟動 **\[Windows Admin Center\]**。

> [!NOTE] 
> 重新啟動之後，您必須從 [開始] 功能表啟動 Windows Admin Center。  

* [檢查 Windows 版本](#winvercompat)

* 確定您用來做為網頁瀏覽器的是 Microsoft Edge 或 Google Chrome。

* [第一次啟動](../use/get-started.md#selecting-a-client-certificate)時，您是否選取了正確的憑證？

  * 嘗試在私人工作階段中開啟瀏覽器，如果可以運作，您需要清除快取。

* 沒有您最近升級 Windows 10 到新的組建或版本？

  * 這可能會清除您的信任的主機設定。 [請依照下列指示來更新您的信任的主機設定。](#configure-trustedhosts) 

[[返回頁首]](#toc)

<a id="whitescreenws"></a>

### 如果您已將 Windows Admin Center 安裝為 **Windows Server 上的閘道**

* 您從先前版本的 Windows Admin Center 升級嗎？ 請檢查以確定因為[這個已知的問題](known-issues.md#upgrade)未刪除的防火牆規則。 您可以使用下列 PowerShell 命令來判斷規則是否存在。 如果沒有，請依照[下列指示](known-issues.md#upgrade)來重新建立它。
    ```powershell
    Get-NetFirewallRule -DisplayName "SmeInboundOpenException"
    ```

* 請為用戶端和伺服器[檢查 Windows 版本](#winvercompat)。

* 確定您用來做為網頁瀏覽器的 Microsoft Edge 或 Google Chrome。

* 在伺服器上，開啟 [工作管理員 > 服務，並確定**ServerManagementGateway / Windows Admin Center**正在執行。
![](../media/Service-TaskMan.PNG)

* 測試與閘道的網路連線 (將 \<values> 取代為您部署中的資訊)
    ```powershell
    Test-NetConnection -Port <port> -ComputerName <gateway> -InformationLevel Detailed
    ```

[[返回頁首]](#toc)

<a id="whitescreenazvm"></a>  

### 如果您已在 Azure Windows Server VM 中安裝 Windows Admin Center

* [檢查 Windows 版本](#winvercompat)
* 您是否已新增 HTTPS 的輸入連接埠規則？ 
* [深入了解如何在 Azure VM 中安裝 Windows Admin Center](https://docs.microsoft.com/windows-server/manage/windows-admin-center/configure/azure-integration#use-a-windows-admin-center-gateway-deployed-in-azure)

[[返回頁首]](#toc)

<a id="winvercompat"></a>

### 檢查 Windows 版本

* 開啟 [執行] 對話方塊 (Windows 鍵 + R) 並啟動 ```winver```。

* 如果您使用的是 Windows 10 版本 1703 或較早的版本，您的 Microsoft Edge 版本不支援 Windows Admin Center。 請升級至最新版本的 Windows 10，或使用 Chrome。

* 如果您使用的 Windows 10 或 Server 的 insider preview 版本組建 17134 與 17637 之間版本，Windows 會有一個錯誤導致 Windows Admin Center 失敗。 請使用目前支援的 Windows 版本。

### 請確定 Windows 遠端管理 (WinRM) 服務在閘道電腦和受管理的節點上執行

* WindowsKey + R 以開啟 [執行] 對話方塊
* 類型```services.msc```並按 enter
* 在開啟的視窗中，尋找 Windows 遠端管理 (WinRM)，請確定它正在執行，並設定為自動啟動

### 沒有您升級您的伺服器從 2016年到 2019年？

* 這可能會清除您的信任的主機設定。 [請依照下列指示來更新您的信任的主機設定。](#configure-trustedhosts) 

[[返回頁首]](#toc)

<a id="tls"></a>

## 我收到訊息: 「 無法安全地連線到此頁面。 這可能是因為網站使用的過時或不安全的 TLS 安全性設定。

<!--REF: https://docs.microsoft.com/iis/get-started/whats-new-in-iis-10/http2-on-iis#when-is-http2-not-supported -->
您的電腦是限制為 HTTP/2 的連線。 Windows Admin Center 會使用整合式的 Windows 驗證，不支援 HTTP/2。 新增下列兩個登錄值在```HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Http\Parameters```若要移除的 HTTP/2 限制**執行瀏覽器的電腦**上的金鑰：

```
EnableHttp2Cleartext=dword:00000000
EnableHttp2Tls=dword:00000000
```

[[返回頁首]](#toc)

<a id="websockets"></a> 

## 我發生問題，使用遠端桌面、 事件及 PowerShell 的工具。

這些三個工具需要 websocket 通訊協定，通常由 proxy 伺服器和防火牆封鎖。 如果您使用 Google Chrome，沒有[已知問題](known-issues.md#google-chrome)與 websocket 和 NTLM 驗證。

[[返回頁首]](#toc)


<a id="connectionissues"></a> 

## 我可以連線到一些伺服器，但連線至其他則不行
* 在本機登入閘道電腦，並嘗試在 PowerShell 中執行 ```Enter-PSSession <machine name>```，將 \<machine name> 以您嘗試要在 Windows Admin Center 中管理的電腦名稱來取代。 

* 如果您的環境使用工作群組而不使用網域，請參閱[在工作群組中使用 Windows Admin Center](#workgroup)。

* **使用本機系統管理員帳戶：** 如果您正在使用的本機使用者帳戶不是內建的 Administrator 帳戶，則必須以目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以啟用目標電腦上的原則：

        REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1


[[返回頁首]](#toc)

<a id="workgroup"></a>

## 在工作群組中使用 Windows Admin Center 

### 您正在使用哪個帳戶？
請確定您使用的認證是目標伺服器本機 Administrator 群組的成員。 在某些情況下，WinRM 也必須有 Remote Management Users 群組的成員資格。 如果您使用的本機使用者帳戶不是**內建的 Administrator 帳戶**，您必須以目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以啟用目標電腦上的原則：

```cmd
REG ADD HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1
```
### 您要連線至不同子網路上的工作群組電腦嗎？

若要連線至並非與閘道位於相同子網路的工作群組電腦，請確定 WinRM 的防火牆連接埠 (TCP 5985) 允許目標電腦上的輸入流量。 您可以使用目標電腦的系統管理員身分在 PowerShell 或命令提示字元中執行下列命令，以建立防火牆規則：

- **Windows Server**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any
    ```

- **Windows 10**

    ```powershell
    Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any
    ```

### 設定 TrustedHosts

安裝 Windows Admin Center 時，您可以選擇讓 Windows Admin Center 管理閘道的 TrustedHosts 設定。 在工作群組環境中，或在網域中使用本機系統管理員認證時，這是必要的。 如果您選擇省略這項設定，就必須手動設定 TrustedHosts。

**若要使用 PowerShell 命令修改 TrustedHosts：**

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

[[返回頁首]](#toc)

<a id="urlacl"></a>

## 先前 Windows Admin Center 安裝，並現在任何其他項目可以使用相同的 TCP/IP 連接埠

手動執行這兩個命令在提升權限的命令提示字元：

```cmd
netsh http delete sslcert ipport=0.0.0.0:443
netsh http delete urlacl url=https://+:443/
```

[[返回頁首]](#toc)

<a id="azlogin"></a>

## 我有問題在 Edge 中使用 Azure 的功能

Edge 有[已知問題](https://github.com/AzureAD/azure-activedirectory-library-for-js/wiki/Known-issues-on-Edge)會影響 Windows Admin Center 中的 Azure 登入的安全性區域與相關。 如果您無法使用 Edge 時使用 Azure 的功能，請嘗試新增https://login.microsoftonline.com，https://login.live.com做為您閘道的 URL 信任的網站，並以允許使用 Edge 快顯封鎖程式設定您的用戶端端瀏覽器上的網站。 

做法如下：
1. 搜尋 Windows [開始] 功能表中的**網際網路選項**
2. 請移至 [**安全性**] 索引標籤
3. 選項之下的**信任的網站**，請按一下**網站**] 按鈕，然後將 Url 新增 [開啟] 對話方塊中。 您將需要新增您的閘道 URL 以及https://login.microsoftonline.com和https://login.live.com。
4. 請移至 [**隱私權**] 索引標籤
5. **快顯封鎖程式**區段底下，按一下 [**設定**] 按鈕，並將 Url 新增 [開啟] 對話方塊中。 您將需要新增您的閘道 URL 以及https://login.microsoftonline.com和https://login.live.com。


[[返回頁首]](#toc)

<a id="azissue"></a>

## 有 Azure 相關的功能有問題嗎？

請將傳送電子郵件在 wacFeedbackAzure@microsoft.com 具有下列資訊：
* 從[問題如下所列](#filebug)的一般問題資訊。 
* 說明您的問題，以及您用重現問題的步驟。 
* 您沒有先前註冊您的閘道至 Azure 使用新 AadApp.ps1 可下載指令碼並再升級至版本 1807年？ 或您是否已註冊您的閘道至 Azure 使用從閘道設定 > Azure UI？
* 是您多個目錄/租用戶相關聯的 Azure 帳戶？
    * 如果是： 當註冊 Azure AD 應用程式至 Windows Admin Center，是您在 Azure 中使用您的預設目錄的目錄嗎？ 
* Azure 帳戶有多個訂閱的存取權嗎？
* 您使用的訂閱是否有附加的計費？
* 您登入到多個 Azure 帳戶時遇到問題嗎？
* Azure 帳戶需要多重要素驗證嗎？
* 是您嘗試管理 Azure VM 的電腦嗎？
* Azure VM 上安裝的 Windows Admin Center？

[[返回頁首]](#toc)

<a id="filebug"></a>

## 仍然無法運作，或這裡並末您的問題？ [疑難排解常見問題]

移至 [事件檢視器] > [應用程式及服務] > [Microsoft-ServerManagementExperience]，並尋找任何錯誤或警告。

在描述您所遇問題的 [UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5BBug%5D) 上，向我們提報錯誤。

請包含任何您在事件記錄檔中找到的錯誤或警告，以及下列資訊： 

* **安裝** Windows Admin Center 所在的平台 (Windows 10 或 Windows Server)：
    * 如果伺服器上安裝，什麼是**執行瀏覽器的機器**能夠存取 Windows Admin Center 的 Windows[版本](#winvercompat)： 
    * 您正在使用安裝程式所建立的自我簽署的憑證？
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

[[返回頁首]](#toc)