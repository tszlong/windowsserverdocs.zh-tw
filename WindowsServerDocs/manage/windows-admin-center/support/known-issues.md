---
title: Windows Admin Center 已知問題
description: Windows Admin Center 已知問題 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: b0159d88251c7f4f6422dffd8f1e1414e1f30f33
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296943"
---
# Windows Admin Center 已知問題

>適用於：Windows Admin Center、Windows Admin Center 預覽版

如果您發生本頁面未述及的問題，請[讓我們知道](http://aka.ms/WACfeedback)。

## Lenovo XClarity 整合商
Lenovo XClarity 整合商的特定版本組合和 Windows Admin Center 之間，存在不相容。 如果您目前使用或計劃要使用 Windows Admin Center 中的 Lenovo XClarity 整合商擴充功能，以下是您需要知道：

- Lenovo XClarity 整合商延伸版本 1.0.4 是與 Windows Admin Center 1809.5 完全相容。
- Lenovo XClarity 整合商延伸版本 1.0.4 已公開上 Windows Admin Center 1904 相容性問題。 Microsoft 和 Lenovo 工程師積極研究在一起，以及我們希望能夠儘速提供解決方案。 將會以下張貼的任何更新，Windows Admin Center 文件在網站上，以及您也可以參考[Lenovo 的支援頁面](https://support.lenovo.com/solutions/ht507549)來供參考。
- 如果您是經常 Lenovo 的 XClarity 功能的使用者，您可以繼續使用 XClarity 整合商 1.0.4，Windows Admin Center 1809.5 上的任一種掌握或者您可以升級到 Windows Admin Center 1904，並另外使用獨立 XClarity 系統管理員解決方法是目前的軟體。


## 安裝程式

- 使用您自己的憑證安裝 Windows Admin Center 時，要留意的是，如果您從認證管理員 MMC 工具複製憑證指紋，[它會在開頭包含無效的字元。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 此情況的因應措施是，先鍵入憑證指紋的第一個字元，再複製/貼上其餘部分。

- 不支援使用連接埠 1024年下方。 在服務模式中，您可能會選擇設定連接埠 80 重新導向到您指定的連接埠。

- 如果已停止 Windows Update 服務 (wuauserv)，且停用，安裝程式將會失敗。 [19100629]

### 升級

- 在升級 Windows Admin Center 從先前的版本，服務模式中，如果您使用 msiexec 以無訊息模式，您可能會遇到的問題刪除 Windows Admin Center 連接埠的對內的防火牆規則的位置。
  - 若要重新建立規則，執行下列命令從提升權限的 PowerShell 主控台，\<port> 取代 Windows Admin center （預設值 443。） 設定的連接埠

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## 一般

- 如果您有 Windows Admin Center 安裝為閘道底下大量使用**Windows Server 2016**上，服務可能會損毀，並包含在事件記錄檔中的錯誤```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```。 這是因為已經修正 Windows Server 2019 中的錯誤。 Windows Server 2016 的更新已包含 2 月 2019 累積更新， [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)。

- 如果您有 Windows Admin Center 安裝為閘道，且您的連線清單可能已損毀，執行下列步驟：

>[!WARNING]
>這將會刪除的連線清單和閘道上的所有 Windows Admin Center 使用者的設定。

  1. 解除安裝 Windows Admin Center
  2. 刪除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下的 **Server Management Experience** 資料夾。
  3. 重新安裝 Windows Admin Center

- 如果您讓工具保持開啟且閒置很長一段時間，您可能會收到數個**錯誤: Runspace 狀態不適用於此操作**錯誤。 如果發生這種情況，請重新整理瀏覽器。 如果您遇到這[傳送意見反應給我們](http://aka.ms/WACfeedback)。

- 重新整理有很長 URL 的頁面時，可能會發生 **500 錯誤**。 [12443710]

- 在某些工具中，瀏覽器的拼字檢查工具可能會將特定欄位值標示為拼字錯誤。 [12425477]

- 在某些工具中，命令按鈕可能無法在按一下後立即反映狀態變更，而工具 UI 可能無法自動反映特定屬性的變更。 您可以按一下 **\[重新整理\]** 來擷取目標伺服器的最新狀態。 [11445790]

- 如果您選取 \ [連線使用的多重選取核取方塊，然後連線根據篩選清單篩選連線清單-上標記的標記，原始選取項目會持續存在，讓您選取任何動作會套用至先前所選取的所有電腦。 [18099259]

- 可能的 Windows Admin Center 模組和所列第 3 方軟體通知內容中執行的 OS 版本號碼之間的有些微差異。

### 擴充管理員]

- 當您更新 Windows Admin Center 時，您必須重新安裝您的延伸模組。
- 如果您將無法存取的擴充功能摘要，沒有任何警告。 [14412861]

## 瀏覽器特定問題

### Microsoft Edge

- 在某些情況下，使用 Microsoft Edge 透過網際網路存取 Windows Admin Center 閘道時，可能會出現載入時間冗長的問題。 這可能會在其中 Windows Admin Center 閘道使用自我簽署憑證的 Azure VM 上發生。 [13819912]

- 當使用 Azure Active Directory 做為身分識別提供者，而 Windows Admin Center 是使用自我簽署憑證或未受信任憑證所設定時，您無法在 Microsoft Edge 中完成 AAD 驗證。  [15968377]

- 如果您有 Windows Admin Center 部署為一種服務，您使用 Microsoft Edge 做為您的瀏覽器連線至 Azure 閘道之後繁衍新的瀏覽器視窗可能會失敗。 若要解決此問題，藉由新增嘗試https://login.microsoftonline.com， https://login.live.com，和 URL 做為您閘道的信任的網站，並允許網站使用的用戶端的側邊瀏覽器上的快顯封鎖程式設定。 如需於修正這個問題的[疑難排解指南](troubleshooting.md#azlogin)中的指引。 [17990376]

- 如果您有 Windows Admin Center 安裝在桌面模式中，瀏覽器索引標籤中的，在 Microsoft Edge 中將不會顯示 favicon。 [17665801]

### Google Chrome

- 在版本 70 （放開晚期，2018 年 10 月） 之前 Chrome 有關於 websocket 通訊協定和 NTLM 驗證[錯誤](https://bugs.chromium.org/p/chromium/issues/detail?id=423609)。 這會影響下列工具：事件、PowerShell、遠端桌面。

- Chrome 可能會快顯多個認證提示，特別是在**工作群組** (非網域) 環境中加入連線體驗時。

- 如果您有 Windows Admin Center 部署為服務時，快顯視窗，從閘道 URL 需要啟用任何 Azure 整合功能，才能運作。 這些服務包括 Azure 網路介面卡、 Azure 更新管理和 Azure Site Recovery。

### Mozilla Firefox

Windows Admin Center 未使用 Mozilla Firefox 來測試，但大部分功能應該都能運作。

- Windows 10 安裝：Mozilla Firefox 有其本身的憑證存放區，因此您必須將 ```Windows Admin Center Client``` 憑證匯入至 Firefox，才能使用 Windows 10 上的 Windows Admin Center。

<a id="websockets"></a>

## 使用 Proxy 服務時的 WebSocket 相容性

Windows Admin Center 中的遠端桌面、PowerShell 及事件模組會利用 WebSocket 通訊協定，而這通常在使用 Proxy 服務時不受支援。 Azure AD 應用程式 Proxy 相容性中的 Websocket 支援尚處於[預覽](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)階段，正在尋求有關相容性的意見反應。

## 支援適用於 Windows Server 2016 (2012 R2、 2012、 2008 R2) 之前的版本

> [!NOTE]
> Windows Admin Center 需要 Windows Server 2012 R2、 2012年或 2008 R2 中未包含的 PowerShell 功能。 如果您將這些使用 Windows Admin Center 管理 Windows Server，您將需要在這些伺服器上安裝 WMF 5.1 版或更高版本。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更新版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

<a id="rbacknownissues"></a>

## 角色型存取控制 (RBAC)

- RBAC 部署無法在設定為使用 Windows Defender 應用程式控制 (WDAC，先前稱為程式碼完整性) 的電腦上成功進行。[16568455]

- 若要在叢集中使用 RBAC，您必須將設定個別部署至每個成員節點。

- 部署 RBAC 時，您可能會收到不正確地歸因於 RBAC 設定的未經授權錯誤。 [16369238]

## 伺服器管理員解決方案

### 伺服器設定

- 如果您修改設定，然後再嘗試瀏覽離開不儲存，頁面將會警告您未儲存的變更，但繼續瀏覽離開。 您可能會在結束的狀態，已選取 [設定] 索引標籤不相符的頁面內容。 [19905798] [19905787]

### 憑證

- 無法將 .PFX 加密憑證匯入至目前的使用者存放區。 [11818622]

### 裝置

- 瀏覽表格使用鍵盤時, 選取項目可能會跳到表格群組頂端。 [16646059]

### 事件

- 事件會受 [使用 Proxy 服務時的 WebSocket 相容性](#websockets)影響。

- 匯出大型記錄檔時，您可能會收到參考「封包大小」的錯誤。 [16630279]

  - 若要解決此問題，請在閘道電腦上提升權限的命令提示字元中使用下列命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### 檔案

- 尚未支援上傳或下載大型檔案。 (\~100mb 限制) [12524234]

### PowerShell

- PowerShell 會受[使用 Proxy 服務時的 WebSocket 相容性](#websockets)影響

- 處於桌面 PowerShell 主控台時以單一按右鍵方式貼上，會沒有作用。 反而會出現瀏覽器的快顯功能表，您可在其中選取 [貼上]。 您也可以使用 Ctrl-V。

- Ctrl-C 複製沒有作用，始終都會傳送 Ctrl-C 中斷命令給主控台。 透過按右鍵操作功能表複製會奏效。

- 當您讓 Windows Admin Center 視窗變小時，終端機內容會自動重排，但是再讓該視窗變大時，內容可能無法回復到先前狀態。 如果內容變得雜亂時，您可以嘗試 Clear-Host，或使用終端機上方的按鈕，中斷連線然後重新連線。

### 登錄編輯程式

- 尚未實行搜尋功能。 [13820009]

### 遠端桌面

- 遠端桌面工具可能會失敗，來管理 Windows Server 2012 時的連線。 [20258278]

- 當使用遠端桌面連線至未加入網域的電腦，您必須輸入您的帳戶```MACHINENAME\USERNAME```格式。

- 有些設定可以封鎖 Windows Admin Center 的遠端桌面用戶端使用群組原則。 如果您遇到此問題時，啟用```Allow users to connect remotely by using Remote Desktop Services```底下 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 遠端桌面會受到[的 websocket 相容性。](#websockets)

- 遠端桌面工具目前不支援任何在本機桌面與遠端工作階段之間的文字、影像或檔案複製/貼上。

- 若要在遠端工作階段中執行任何複製/貼上，您可以按照正常方式複製 (按右鍵 + 複製或 Ctrl+C)，但貼上需要按右鍵 + 貼上 (Ctrl+V 無效)

- 您無法將下列按鍵命令傳送至遠端工作階段
  - Alt+Tab
  - 功能鍵
  - Windows 鍵
  - PrtScn

- 遠端應用程式 – 啟用遠端桌面設定，從遠端應用程式工具之後工具可能不會顯示工具清單中管理含有桌面體驗的伺服器] 時。 [18906904]

### 角色和功能

- 選取來源無法使用的角色或功能進行安裝時，會略過這些來源。 [12946914]

- 如果您選擇不要在安裝角色後自動重新開機，我們就不會再次詢問。 [13098852]

- 如果您選擇要自動重新開機，將會在狀態更新進度達 100% 之前發生重新開機。 [13098852]

### 儲存空間

- 擷取配額資訊可能會失敗，而不需要錯誤通知 （雖然仍然有錯誤的瀏覽器主控台中） [18962274]

- 下層：DVD/CD/軟碟機不會顯示為下層的磁碟區。

- 下層：[磁碟區和磁碟] 中的部分屬性無法在下層使用，因此這些屬性會在詳細資料面板中顯示為未知或空白。

- 下層：建立新磁碟區時，ReFS 在 Windows 2012 和 2012 R2 電腦上僅支援 64K 的配置單位大小。 如果 ReFS 是在下層目標使用較小的配置單位大小所建立，檔案系統格式化將會失敗。 新的磁碟區將無法使用。 解決方法是刪除磁碟區，並使用 64K 配置單位大小。

### 更新

- 安裝更新之後，安裝狀態可能保留快取，而且需要重新整理瀏覽器。

- 您可能會遇到錯誤: 「 索引鍵不存在 」 時嘗試設定 Azure 更新管理。 在此情況下，請嘗試下列補救步驟受管理的節點上
    1. 停止 「 密碼編譯服務 」 服務。
    2. 變更資料夾選項來顯示隱藏的檔案 （如有必要）。
    3. 了解到 「 %allusersprofile%\microsoft\crypto\rsa\s-1-5-18 」 資料夾和刪除其所有內容。
    4. 重新啟動 「 密碼編譯服務 」 服務。
    5. 重複使用 Windows Admin Center 的更新管理設定

### 虛擬機器

- 當管理虛擬機器，Windows Server 2012 主機上，瀏覽器中 VM 連線來連線到 VM 將會失敗的工具。 下載.rdp 檔案來連線到 VM 仍可正常運作。 [20258278]

- Azure Site Recovery – 如果 ASR 以外 WAC，主機上的安裝您將無法以保護從 VM 內 WAC [18972276]

- 目前不支援 Hyper-V 管理員提供的進階功能，例如虛擬 SAN 管理員、移動 VM、匯出 VM、VM 複寫。

### 虛擬交換器

- 交換器內嵌小組 (SET)：將 NIC 加入至小組時，這些 NIC 必須是在相同的子網路上。

## 電腦管理解決方案

電腦管理解決方案包含伺服器管理員解決方案所提供工具的子集，因此適用同樣的已知問題，以及下列電腦管理解決方案特定問題：

- 如果您使用 Microsoft 帳戶 ([MSA](https://account.microsoft.com/account/))，或如果您使用 Azure Active Directory (AAD) 來登入您的 Windows 10 電腦時，您必須指定 「 管理-為 「 認證來管理您的本機電腦 [16568455]

- 當您嘗試管理 localhost 時，系統會提示您提高閘道程序的權限。 如果您在 \[使用者帳戶控制\] 快顯中按一下 **\[否\]**，Windows Admin Center 將無法再次加以顯示。 在此情況下，以滑鼠右鍵按一下系統匣中的 Windows Admin Center 圖示並選擇 [結束] 以結束閘道程序，然後從 [開始] 功能表重新啟動 Windows Admin Center。

- Windows 10 預設不開啟 WinRM/PowerShell 遠端執行功能
  
  - 若要啟用 Windows 10 用戶端管理，您必須從提升權限的 PowerShell 命令提示字元發出命令 ```Enable-PSRemoting```。

  - 您可能還需要使用 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 更新防火牆，以允許來自外部本機子網路的連線。 如需更多嚴格網路案例，請參閱[此文件](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## 容錯移轉叢集管理員解決方案

- 管理叢集時，(超融合式或傳統式？) 可能會發生**找不到命令介面**錯誤。 如果發生這種情況，請重新載入瀏覽器，或往外瀏覽至其他工具，然後再瀏覽器回來。 [13882442]

- 管理尚未設定完全的下層 (Windows Server 2012 或 2012 R2) 叢集時，可能會發生問題。 此問題的修正方式為，確保叢集的**每個成員節點**皆已安裝並啟用 Windows 功能 **RSAT-Clustering-PowerShell**。 若要使用 PowerShell 達成此目的，請在所有叢集節點上輸入下列命令 `Install-WindowsFeature -Name RSAT-Windows-PowerShell`。 [12524664]

- 叢集可能必須使用整個 FQDN 來新增，才能正確地探索到該叢集。

- 連線至使用安裝為閘道之 Windows Admin Center 的叢集時，並提供明確使用者名稱/密碼進行驗證時，您必須選取 **\[使用這些認證進行所有連線\]**，讓認證可用於查詢成員節點。

## 超融合式叢集管理員解決方案

- 有些命令 (例如 **Drives - Update firmware**、**Servers - Remove** 和 **Volumes - Open**) 已停用，而且目前不受支援。