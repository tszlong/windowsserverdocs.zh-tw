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
ms.openlocfilehash: 7bf23c5af5620241574864babd07fd852115a450
ms.sourcegitcommit: 39ab8041d166e6817a95417d6aa30bc7abeeef54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/28/2019
ms.locfileid: "66260267"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知問題

>適用於：Windows Admin Center，Windows Admin Center 預覽

如果您發生本頁面未述及的問題，請[讓我們知道](http://aka.ms/WACfeedback)。

## <a name="lenovo-xclarity-integrator"></a>Lenovo XClarity 整合器
使用 Windows Admin Center 版本 1904.1 Lenovo XClarity 整合器擴充功能和 Windows Admin Center 版本 1904年先前揭露的不相容的問題已經解決。 我們強烈建議您更新至最新的 Windows Admin Center 支援版本。

- Lenovo XClarity 整合器延伸模組 1.1 版是與 Windows Admin Center 1904.1 完全相容。 我們強烈建議您更新至最新版的 Windows Admin Center 和 Lenovo 擴充功能。
- 因為任何原因，如果您要繼續使用 Windows Admin Center 1809.5 次，您可以使用 XClarity Integrator 1.0.4 也可在 「 Windows Admin Center 擴充功能摘要，直到 Windows Admin Center 1809.5 不再支援根據我們[的支援原則](../support/index.md)。

## <a name="installer"></a>安裝程式

- 使用您自己的憑證安裝 Windows Admin Center 時，要留意的是，如果您從認證管理員 MMC 工具複製憑證指紋，[它會在開頭包含無效的字元。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 此情況的因應措施是，先鍵入憑證指紋的第一個字元，再複製/貼上其餘部分。

- 不支援使用低於 1024年連接埠。 在服務模式中，您可以選擇性地設定連接埠重新導向至您指定的連接埠 80。

- 如果停止並停用 Windows Update service (wuauserv)，則安裝程式將會失敗。 [19100629]

### <a name="upgrade"></a>升級

- 如果您使用 msiexec 以無訊息模式，請在服務模式下，從先前的版本，升級 Windows Admin Center 時, 可能遇到的問題刪除 Windows Admin Center 連接埠的輸入的防火牆規則的位置。
  - 若要重新建立規則，請從提升權限的 PowerShell 主控台中，執行下列命令取代\<連接埠 > 搭配通訊埠設定的 Windows Admin Center （預設為 443。）

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>一般

- 如果您有安裝做為閘道上的 Windows Admin Center **Windows Server 2016**下方大量使用，服務可能會當機與包含事件記錄檔中的錯誤```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```。 這是因為 Windows Server 2019 已修正的 bug。 適用於 Windows Server 2016 修補程式已包含 2019 年 2 月累積更新時， [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)。

- 如果您有安裝做為閘道的 Windows Admin Center 連線清單可能已損毀，請執行下列步驟：

>[!WARNING]
>這會刪除所有的 Windows Admin Center 使用者，在閘道上的設定與連接清單。

  1. 解除安裝 Windows Admin Center
  2. 刪除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下的 **Server Management Experience** 資料夾。
  3. 重新安裝 Windows Admin Center

- 如果您將保留此工具開啟，而且閒置很長一段時間，您可能會收到數個**錯誤：這項作業的 runspace 狀態無效**錯誤。 如果發生這種情況，請重新整理瀏覽器。 如果您遇到這[傳送意見反應](http://aka.ms/WACfeedback)。

- 重新整理有很長 URL 的頁面時，可能會發生 **500 錯誤**。 [12443710]

- 在某些工具中，瀏覽器的拼字檢查工具可能會將特定欄位值標示為拼字錯誤。 [12425477]

- 在某些工具中，命令按鈕可能無法在按一下後立即反映狀態變更，而工具 UI 可能無法自動反映特定屬性的變更。 您可以按一下 **\[重新整理\]** 來擷取目標伺服器的最新狀態。 [11445790]

- 如果您選取 連線使用多重選取的核取方塊，然後篩選您的連線清單，依標記篩選在連線清單-標記、 原始選取項目持續發生，因此您選取任何動作將套用到先前選取的所有電腦。 [18099259]

- 可能有次要版本號碼，以執行 Windows Admin Center 模組，以及第 3 個合作對象軟體聲明中所列的作業系統之間的變異數。

### <a name="extension-manager"></a>擴充管理員

- 當您更新 Windows Admin Center 時，您必須重新安裝您的擴充功能。
- 如果您將無法存取的擴充功能摘要，沒有任何警告。 [14412861]

## <a name="browser-specific-issues"></a>瀏覽器特定問題

### <a name="microsoft-edge"></a>Microsoft Edge

- 在某些情況下，使用 Microsoft Edge 透過網際網路存取 Windows Admin Center 閘道時，可能會出現載入時間冗長的問題。 這可能會在其中 Windows Admin Center 閘道使用自我簽署憑證的 Azure VM 上發生。 [13819912]

- 當使用 Azure Active Directory 做為身分識別提供者，而 Windows Admin Center 是使用自我簽署憑證或未受信任憑證所設定時，您無法在 Microsoft Edge 中完成 AAD 驗證。  [15968377]

- 如果您已部署為服務的 Windows Admin Center，您使用 Microsoft Edge 瀏覽器作為您閘道連線至 Azure 之後繁衍新的瀏覽器視窗可能會失敗。 嘗試解決此問題，藉由新增 https://login.microsoftonline.com， https://login.live.com，並為您閘道的 URL 信任的網站，並允許您的用戶端端瀏覽器的快顯封鎖程式設定的網站。 如需詳細指引修正這[疑難排解指南](troubleshooting.md#azlogin)。 [17990376]

- 如果您有桌面模式中安裝的 Windows Admin Center，在 Microsoft Edge 瀏覽器索引標籤不會顯示 favicon。 [17665801]

### <a name="google-chrome"></a>Google Chrome

- 之前的版本 （發行年 10 月，2018 年底） 70 有 Chrome [bug](https://bugs.chromium.org/p/chromium/issues/detail?id=423609)有關 websockets 通訊協定與 NTLM 驗證。 這會影響下列工具：事件、 PowerShell、 遠端桌面。

- Chrome 可能會快顯多個認證提示，特別是在**工作群組** (非網域) 環境中加入連線體驗時。

- 如果您有部署為服務的 Windows Admin Center，閘道器 URL 的快顯畫面需要啟用，任何 Azure 的整合功能才能運作。 這些服務包括 Azure 網路介面卡、 Azure 更新管理和 Azure Site Recovery。

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center 未使用 Mozilla Firefox 來測試，但大部分功能應該都能運作。

- Windows 10 安裝：Mozilla Firefox 會有本身的憑證存放區，因此您必須匯入```Windows Admin Center Client```到使用 Windows 10 上的 Windows Admin Center Firefox 的憑證。

<a id="websockets"></a>

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>當使用 proxy 服務的 WebSocket 相容性

Windows Admin Center 中的遠端桌面、PowerShell 及事件模組會利用 WebSocket 通訊協定，而這通常在使用 Proxy 服務時不受支援。 Azure AD 應用程式 Proxy 相容性中的 Websocket 支援尚處於[預覽](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)階段，正在尋求有關相容性的意見反應。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>Windows Server 2016 (2012 r2、windows server 2012、windows 2008 R2) 之前的版本支援

> [!NOTE]
> Windows Admin Center 要求不包含在 Windows Server 2012 R2，2012年或 2008 R2 的 PowerShell 功能。 如果您將這些項目 Windows Admin Center 與管理 Windows Server，您必須在這些伺服器上安裝 WMF 5.1 或更新版本。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

<a id="rbacknownissues"></a>

## <a name="role-based-access-control-rbac"></a>角色型存取控制 (RBAC)

- RBAC 部署無法在設定為使用 Windows Defender 應用程式控制 (WDAC，先前稱為程式碼完整性) 的電腦上成功進行。[16568455]

- 若要在叢集中使用 RBAC，您必須將設定個別部署至每個成員節點。

- 部署 RBAC 時，您可能會收到不正確地歸因於 RBAC 設定的未經授權錯誤。 [16369238]

## <a name="server-manager-solution"></a>伺服器管理員解決方案

### <a name="server-settings"></a>伺服器設定

- 如果您修改設定，然後再嘗試瀏覽其他位置，而不儲存，頁面會警告您有未儲存的變更，但繼續瀏覽其他位置。 您可能會得到處於已選取 [設定] 索引標籤與頁面的內容不符。 [19905798] [19905787]

### <a name="certificates"></a>憑證

- 無法將 .PFX 加密憑證匯入至目前的使用者存放區。 [11818622]

### <a name="devices"></a>裝置

- 當巡覽具有鍵盤的資料表中，選取項目可能會跳到頂端的 [資料表] 群組。 [16646059]

### <a name="events"></a>事件

- 事件會受 [使用 Proxy 服務時的 WebSocket 相容性](#websockets)影響。

- 匯出大型記錄檔時，您可能會收到參考「封包大小」的錯誤。 [16630279]

  - 若要解決此問題，請在閘道機器上提高權限命令提示字元使用下列命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>檔案

- 尚未支援上傳或下載大型檔案。 (\~100 mb 的限制) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell 會受[使用 Proxy 服務時的 WebSocket 相容性](#websockets)影響

- 處於桌面 PowerShell 主控台時以單一按右鍵方式貼上，會沒有作用。 反而會出現瀏覽器的快顯功能表，您可在其中選取 [貼上]。 您也可以使用 Ctrl-V。

- Ctrl-C 複製沒有作用，始終都會傳送 Ctrl-C 中斷命令給主控台。 透過按右鍵操作功能表複製會奏效。

- 當您讓 Windows Admin Center 視窗變小時，終端機內容會自動重排，但是再讓該視窗變大時，內容可能無法回復到先前狀態。 如果內容變得雜亂時，您可以嘗試 Clear-Host，或使用終端機上方的按鈕，中斷連線然後重新連線。

### <a name="registry-editor"></a>登錄編輯程式

- 尚未實行搜尋功能。 [13820009]

### <a name="remote-desktop"></a>遠端桌面

- 遠端桌面工具可能無法連線時管理 Windows Server 2012。 [20258278]

- 當使用遠端桌面連接到未加入網域的機器，您必須輸入您的帳戶在```MACHINENAME\USERNAME```格式。

- 某些設定可以封鎖 Windows Admin Center 的遠端桌面用戶端與群組原則。 如果您遇到這種情況，啟用```Allow users to connect remotely by using Remote Desktop Services```下 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 遠端桌面都會受到[websocket 相容性。](#websockets)

- 遠端桌面工具目前不支援任何在本機桌面與遠端工作階段之間的文字、影像或檔案複製/貼上。

- 若要在遠端工作階段中執行任何複製/貼上，您可以按照正常方式複製 (按右鍵 + 複製或 Ctrl+C)，但貼上需要按右鍵 + 貼上 (Ctrl+V 無效)

- 您無法將下列按鍵命令傳送至遠端工作階段
  - Alt+Tab
  - 功能鍵
  - Windows 鍵
  - PrtScn

- 遠端應用程式 – 之後啟用遠端桌面設定的遠端應用程式工具工具可能不會出現在 [工具] 清單中管理伺服器含桌面體驗時。 [18906904]

### <a name="roles-and-features"></a>角色和功能

- 選取來源無法使用的角色或功能進行安裝時，會略過這些來源。 [12946914]

- 如果您選擇不要在安裝角色後自動重新開機，我們就不會再次詢問。 [13098852]

- 如果您選擇要自動重新開機，將會在狀態更新進度達 100% 之前發生重新開機。 [13098852]

### <a name="storage"></a>儲存體

- 擷取配額資訊可能會失敗，沒有錯誤通知 （仍有錯誤在瀏覽器的主控台） [18962274]

- 舊版：在舊版上的磁碟區不會出現 DVD/CD/軟碟機。

- 舊版：磁碟區和磁碟中的某些屬性沒有可用的舊版，因此無法辨識或在詳細資料窗格中的空白則會出現。

- 舊版：在建立新的磁碟區時，ReFS 只支援為 64k 配置單位大小 Windows 2012 和 2012 R2 電腦上。 如果 ReFS 是在下層目標使用較小的配置單位大小所建立，檔案系統格式化將會失敗。 新的磁碟區將無法使用。 解決方法是刪除磁碟區，並使用 64K 配置單位大小。

### <a name="updates"></a>更新

- 安裝更新之後，安裝狀態可能會快取，而且需要重新整理瀏覽器。

- 您可能會遇到的錯誤：「 索引鍵集不存在 」 時嘗試設定 Azure 更新管理。 在此情況下，請嘗試下列的補救步驟，在受管理的節點-
    1. 停止 「 密碼編譯服務 」 服務。
    2. （如有必要），以顯示變更資料夾選項會隱藏檔案。
    3. 移至"%allusersprofile%\microsoft\crypto\rsa\s-1-5-18"資料夾並刪除其所有內容。
    4. 重新啟動 「 密碼編譯服務 」 服務。
    5. 重複使用 Windows Admin Center 進行更新管理設定

### <a name="virtual-machines"></a>虛擬機器

- 在管理 Windows Server 2012 的主機上的虛擬機器時，瀏覽器中的 VM 連線工具將無法連接到 VM。 下載.rdp 檔案以連線至 VM 應該還是有效。 [20258278]

- Azure Site Recovery – ASR WAC，外部主機上的設定時您將無法保護從 VM 內 WAC [18972276]

- 目前不支援 Hyper-V 管理員提供的進階功能，例如虛擬 SAN 管理員、移動 VM、匯出 VM、VM 複寫。

### <a name="virtual-switches"></a>虛擬交換器

- 交換器內嵌小組 (SET):將 Nic 加入至小組中，它們必須位於相同子網路。

## <a name="computer-management-solution"></a>電腦管理解決方案

電腦管理解決方案包含伺服器管理員解決方案所提供工具的子集，因此適用同樣的已知問題，以及下列電腦管理解決方案特定問題：

- 如果您使用 Microsoft 帳戶 ([MSA](https://account.microsoft.com/account/))，或如果您使用 Azure Active Directory (AAD) 來登入您的 Windows 10 電腦時，您必須指定 「 管理-為"的認證可以管理您的本機電腦 [16568455]

- 當您嘗試管理 localhost 時，系統會提示您提高閘道程序的權限。 如果您在 \[使用者帳戶控制\] 快顯中按一下 **\[否\]** ，Windows Admin Center 將無法再次加以顯示。 在此情況下，以滑鼠右鍵按一下系統匣中的 Windows Admin Center 圖示並選擇 [結束] 以結束閘道程序，然後從 [開始] 功能表重新啟動 Windows Admin Center。

- Windows 10 預設不開啟 WinRM/PowerShell 遠端執行功能
  
  - 若要啟用 Windows 10 用戶端管理，您必須從提升權限的 PowerShell 命令提示字元發出命令 ```Enable-PSRemoting```。

  - 您可能還需要使用 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 更新防火牆，以允許來自外部本機子網路的連線。 如需更多嚴格網路案例，請參閱[此文件](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## <a name="failover-cluster-manager-solution"></a>容錯移轉叢集管理員解決方案

- 管理叢集時，(超融合式或傳統式？) 可能會發生**找不到命令介面**錯誤。 如果發生這種情況，請重新載入瀏覽器，或往外瀏覽至其他工具，然後再瀏覽器回來。 [13882442]

- 管理尚未設定完全的下層 (Windows Server 2012 或 2012 R2) 叢集時，可能會發生問題。 此問題的修正方式為，確保叢集的**每個成員節點**皆已安裝並啟用 Windows 功能 **RSAT-Clustering-PowerShell**。 若要使用 PowerShell 達成此目的，請在所有叢集節點上輸入下列命令 `Install-WindowsFeature -Name RSAT-Windows-PowerShell`。 [12524664]

- 叢集可能必須使用整個 FQDN 來新增，才能正確地探索到該叢集。

- 連線至使用安裝為閘道之 Windows Admin Center 的叢集時，並提供明確使用者名稱/密碼進行驗證時，您必須選取 **\[使用這些認證進行所有連線\]** ，讓認證可用於查詢成員節點。

## <a name="hyper-converged-cluster-manager-solution"></a>超融合式叢集管理員解決方案

- 有些命令 (例如 **Drives - Update firmware**、**Servers - Remove** 和 **Volumes - Open**) 已停用，而且目前不受支援。