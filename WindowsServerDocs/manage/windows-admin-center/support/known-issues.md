---
title: Windows Admin Center 已知問題
description: Windows Admin Center 已知問題 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 06/07/2019
ms.openlocfilehash: b222cd4b97beecd25c14b9f8f39627bf46cb7716
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869543"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知問題

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

如果您發生本頁面未述及的問題，請[讓我們知道](http://aka.ms/WACfeedback)。

## <a name="lenovo-xclarity-integrator"></a>聯想 XClarity 整合器

先前發現的「聯想 XClarity 整合器擴充功能」和「Windows 系統管理中心」1904版的不相容問題現在已透過 Windows 管理中心版本1904.1 解決。 我們強烈建議您更新至最新支援的 Windows 管理中心版本。

- 聯想 XClarity 整合器延伸模組版本1.1 與 Windows Admin Center 1904.1 完全相容。 我們強烈建議您更新至最新版本的 Windows 管理中心和聯想擴充功能。
- 基於任何原因，如果您當時需要繼續使用 Windows 系統管理中心1809.5，您可以使用 XClarity 整合器1.0.4，這也會在 Windows 管理中心延伸模組摘要中提供，直到不再支援 Windows Admin Center 1809.5，依據我們的[支援原則](../support/index.md)。

## <a name="installer"></a>安裝程式

- 使用您自己的憑證安裝 Windows Admin Center 時，要留意的是，如果您從認證管理員 MMC 工具複製憑證指紋，[它會在開頭包含無效的字元。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 此情況的因應措施是，先鍵入憑證指紋的第一個字元，再複製/貼上其餘部分。

- 不支援使用低於1024的埠。 在服務模式中，您可以選擇性地將埠80設定為重新導向至指定的埠。

- 如果 Windows Update 服務（wuauserv）已停止且停用，安裝程式將會失敗。 [19100629]

### <a name="upgrade"></a>升級

- 從舊版升級服務模式的 Windows 管理中心時，如果您在無訊息模式中使用 msiexec，您可能會遇到 Windows Admin Center 埠的輸入防火牆規則已刪除的問題。
  - 若要重新建立規則，請從提高許可權的 PowerShell 主控台執行下列命令\<，將埠 > 取代為為 Windows 系統管理中心設定的埠（預設為443）。

    ```powershell
    New-NetFirewallRule -DisplayName "SmeInboundOpenException" -Description "Windows Admin Center inbound port exception" -LocalPort <port> -RemoteAddress Any -Protocol TCP
    ```

## <a name="general"></a>一般

- 如果您將 windows 系統管理中心安裝為**windows Server 2016**上的閘道，請使用 [繁重]，此服務可能會在包含```Faulting application name: sme.exe```和```Faulting module name: WsmSvc.dll```的事件記錄檔中發生錯誤時損毀。 這是因為 Windows Server 2019 已修正的錯誤。 Windows Server 2016 的修補程式隨附于2月2019累積更新（ [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)）。

- 如果您已將 Windows 系統管理中心安裝為閘道，且您的連線清單似乎已損毀，請執行下列步驟：

   > [!WARNING]
   >這會刪除閘道上所有 Windows 系統管理中心使用者的連線清單和設定。

  1. 解除安裝 Windows Admin Center
  2. 刪除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下的 **Server Management Experience** 資料夾。
  3. 重新安裝 Windows Admin Center

- 如果您讓工具保持開啟且閒置很長一段時間，您可能會收到數**個錯誤：此**作業錯誤的運行時狀態無效。 如果發生這種情況，請重新整理瀏覽器。 如果您遇到這種情況，請[將意見反應傳送給我們](http://aka.ms/WACfeedback)。

- 重新整理有很長 URL 的頁面時，可能會發生 **500 錯誤**。 [12443710]

- 在某些工具中，瀏覽器的拼字檢查工具可能會將特定欄位值標示為拼字錯誤。 [12425477]

- 在某些工具中，命令按鈕可能無法在按一下後立即反映狀態變更，而工具 UI 可能無法自動反映特定屬性的變更。 您可以按一下 **\[重新整理\]** 來擷取目標伺服器的最新狀態。 [11445790]

- 連線清單上的標記篩選-如果您選取 [使用核取方塊的連線]，然後依標籤篩選您的連線清單，則會保留原始選取專案，因此您選取的任何動作都將套用至所有先前選取的電腦。 [18099259]

- 在 Windows 管理中心模組中執行的 OSS 版本號碼和協力廠商軟體注意事項中所列出的內容之間，可能會有些許差異。

### <a name="extension-manager"></a>延伸模組管理員

- 當您更新 Windows 管理中心時，必須重新安裝擴充功能。
- 如果您新增無法存取的延伸模組摘要，就不會出現警告。 [14412861]

## <a name="browser-specific-issues"></a>瀏覽器特定問題

### <a name="microsoft-edge"></a>Microsoft Edge

- 在某些情況下，使用 Microsoft Edge 透過網際網路存取 Windows Admin Center 閘道時，可能會出現載入時間冗長的問題。 這可能會在其中 Windows Admin Center 閘道使用自我簽署憑證的 Azure VM 上發生。 [13819912]

- 當使用 Azure Active Directory 做為身分識別提供者，而 Windows Admin Center 是使用自我簽署憑證或未受信任憑證所設定時，您無法在 Microsoft Edge 中完成 AAD 驗證。  [15968377]

- 如果您已將 Windows 系統管理中心部署為服務，而且您使用 Microsoft Edge 作為瀏覽器，則在建立新的瀏覽器視窗之後，將閘道連線至 Azure 可能會失敗。 嘗試解決此問題，藉由新增 [https://login.microsoftonline.com](https://login.microsoftonline.com )， [https://login.live.com](https://login.live.com )，並為您閘道的 URL 信任的網站，並允許您的用戶端端瀏覽器的快顯封鎖程式設定的網站。 如需修正此問題的詳細指引，請[參考疑難排解指南](troubleshooting.md#azure-features-dont-work-properly-in-edge)。 [17990376]

- 如果您以桌面圖案安裝 Windows 系統管理中心，Microsoft Edge 中的 [瀏覽器] 索引標籤不會顯示 favicon。 [17665801]

### <a name="google-chrome"></a>Google Chrome

- 在70版之前（2018年10月發行），Chrome 有關于 websocket 通訊協定和 NTLM 驗證的[錯誤](https://bugs.chromium.org/p/chromium/issues/detail?id=423609)。 這會影響下列工具：事件、PowerShell、遠端桌面。

- Chrome 可能會快顯多個認證提示，特別是在**工作群組** (非網域) 環境中加入連線體驗時。

- 如果您已將 Windows 系統管理中心部署為服務，則必須啟用閘道 URL 的快顯視窗，任何 Azure 整合功能才能運作。 這些服務包括 Azure 網路介面卡、Azure 更新管理和 Azure Site Recovery。

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center 未使用 Mozilla Firefox 來測試，但大部分功能應該都能運作。

- Windows 10 安裝：Mozilla Firefox 有自己的憑證存放區，因此您必須將```Windows Admin Center Client```憑證匯入 Firefox，才能在 windows 10 上使用 windows 系統管理中心。

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>使用 proxy 服務時的 WebSocket 相容性

Windows Admin Center 中的遠端桌面、PowerShell 及事件模組會利用 WebSocket 通訊協定，而這通常在使用 Proxy 服務時不受支援。 Azure AD 應用程式 Proxy 相容性中的 Websocket 支援尚處於[預覽](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)階段，正在尋求有關相容性的意見反應。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>2016之前的 Windows Server 版本支援（2012 R2、2012、2008 R2）

> [!NOTE]
> Windows 管理中心需要不包含在 Windows Server 2012 R2、2012或 2008 R2 中的 PowerShell 功能。 如果您要使用 Windows 系統管理中心來管理 Windows Server，您必須在這些伺服器上安裝 WMF 5.1 版或更新版本。

在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF 且版本是 5.1 或更高版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://www.microsoft.com/en-us/download/details.aspx?id=54616)。

## <a name="role-based-access-control-rbac"></a>以角色為基礎的存取控制（RBAC）

- RBAC 部署無法在設定為使用 Windows Defender 應用程式控制 (WDAC，先前稱為程式碼完整性) 的電腦上成功進行。[16568455]

- 若要在叢集中使用 RBAC，您必須將設定個別部署至每個成員節點。

- 部署 RBAC 時，您可能會收到不正確地歸因於 RBAC 設定的未經授權錯誤。 [16369238]

## <a name="server-manager-solution"></a>伺服器管理員解決方案

### <a name="server-settings"></a>伺服器設定

- 如果您修改設定，然後嘗試在不儲存的情況下流覽，頁面將會警告您有關未儲存的變更，但仍會繼續流覽。 最後，您可能會處於所選 [設定] 索引標籤與頁面內容不相符的狀態。 [19905798] [19905787]

### <a name="certificates"></a>憑證

- 無法將 .PFX 加密憑證匯入至目前的使用者存放區。 [11818622]

### <a name="devices"></a>裝置

- 當您使用鍵盤流覽資料表時，選取範圍可能會跳到資料表群組的頂端。 [16646059]

### <a name="events"></a>事件

- 事件會受 [使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響。

- 匯出大型記錄檔時，您可能會收到參考「封包大小」的錯誤。 [16630279]

  - 若要解決此問題，請在閘道電腦上以提高許可權的命令提示字元中使用下列命令：```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>檔案

- 尚未支援上傳或下載大型檔案。 （\~100mb 限制） [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell 會受[使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響

- 處於桌面 PowerShell 主控台時以單一按右鍵方式貼上，會沒有作用。 反而會出現瀏覽器的快顯功能表，您可在其中選取 [貼上]。 您也可以使用 Ctrl-V。

- Ctrl-C 複製沒有作用，始終都會傳送 Ctrl-C 中斷命令給主控台。 透過按右鍵操作功能表複製會奏效。

- 當您讓 Windows Admin Center 視窗變小時，終端機內容會自動重排，但是再讓該視窗變大時，內容可能無法回復到先前狀態。 如果內容變得雜亂時，您可以嘗試 Clear-Host，或使用終端機上方的按鈕，中斷連線然後重新連線。

### <a name="registry-editor"></a>登錄編輯程式

- 尚未實行搜尋功能。 [13820009]

### <a name="remote-desktop"></a>遠端桌面

- 管理 Windows Server 2012 時，遠端桌面工具可能無法連接。 [20258278]

- 使用遠端桌面連線到未加入網域的電腦時，您必須以格式輸入您的```MACHINENAME\USERNAME```帳戶。

- 有些設定可以封鎖 Windows Admin Center 的遠端桌面用戶端與群組原則。 如果您遇到這種情況```Allow users to connect remotely by using Remote Desktop Services``` ，請在下啟用```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

- 遠端桌面受到[websocket 相容性](#websocket-compatibility-when-using-a-proxy-service)的影響。

- 遠端桌面工具目前不支援任何在本機桌面與遠端工作階段之間的文字、影像或檔案複製/貼上。

- 若要在遠端工作階段中執行任何複製/貼上，您可以按照正常方式複製 (按右鍵 + 複製或 Ctrl+C)，但貼上需要按右鍵 + 貼上 (Ctrl+V 無效)

- 您無法將下列按鍵命令傳送至遠端工作階段
  - Alt+Tab
  - 功能鍵
  - Windows 鍵
  - PrtScn

- 遠端應用程式–從遠端桌面設定啟用遠端應用程式工具之後，在管理具有桌面體驗的伺服器時，工具可能不會出現在工具清單中。 [18906904]

### <a name="roles-and-features"></a>角色和功能

- 選取來源無法使用的角色或功能進行安裝時，會略過這些來源。 [12946914]

- 如果您選擇不要在安裝角色後自動重新開機，我們就不會再次詢問。 [13098852]

- 如果您選擇要自動重新開機，將會在狀態更新進度達 100% 之前發生重新開機。 [13098852]

### <a name="storage"></a>儲存體

- 在沒有錯誤通知的情況下，提取配額資訊可能會失敗（瀏覽器的主控台仍會發生錯誤） [18962274]

- 下層：DVD/CD/軟碟磁碟機不會顯示為下層的磁片區。

- 下層：磁片區和磁片中的某些屬性在下層無法使用，因此在詳細資料面板中會顯示為 [未知] 或 [空白]。

- 下層：在建立新的磁片區時，ReFS 僅支援在 Windows 2012 和 2012 R2 電腦上使用64K 的配置單位大小。 如果 ReFS 是在下層目標使用較小的配置單位大小所建立，檔案系統格式化將會失敗。 新的磁碟區將無法使用。 解決方法是刪除磁碟區，並使用 64K 配置單位大小。

### <a name="updates"></a>更新

- 安裝更新之後，可能會快取安裝狀態，而且需要重新整理瀏覽器。

- 您可能會遇到下列錯誤：嘗試設定 Azure 更新管理時，「金鑰集不存在」。 在此情況下，請在受管理的節點上嘗試下列補救步驟-
    1. 停止「密碼編譯服務」服務。
    2. 變更資料夾選項以顯示隱藏的檔案（如有必要）。
    3. 到達 "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" 資料夾，並刪除其所有內容。
    4. 重新開機「密碼編譯服務」服務。
    5. 使用 Windows 系統管理中心重複設定更新管理

### <a name="virtual-machines"></a>虛擬機器

- 管理 Windows Server 2012 主機上的虛擬機器時，瀏覽器內的 VM connect 工具將無法連線到 VM。 下載 .rdp 檔案以連線至 VM 應該仍然可以使用。 [20258278]

- Azure Site Recovery –如果 ASR 是在 WAC 外部的主機上設定，您將無法在 WAC 中保護 VM [18972276]

- 目前不支援 Hyper-V 管理員提供的進階功能，例如虛擬 SAN 管理員、移動 VM、匯出 VM、VM 複寫。

### <a name="virtual-switches"></a>虛擬交換器

- 交換器內嵌小組（SET）：將 Nic 新增至小組時，它們必須位於相同的子網上。

## <a name="computer-management-solution"></a>電腦管理解決方案

電腦管理解決方案包含伺服器管理員解決方案所提供工具的子集，因此適用同樣的已知問題，以及下列電腦管理解決方案特定問題：

- 如果您使用 Microsoft 帳戶（[MSA](https://account.microsoft.com/account/)），或使用 AZURE ACTIVE DIRECTORY （AAD）登入 Windows 10 電腦，您必須指定「管理身分」認證來管理本機電腦 [16568455]

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