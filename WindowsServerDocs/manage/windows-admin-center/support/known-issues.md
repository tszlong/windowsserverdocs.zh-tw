---
title: Windows Admin Center 已知問題
description: Windows Admin Center 已知問題 (Project Honolulu)
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 06/07/2019
ms.openlocfilehash: 4a91d09d6824795a21a9a7cdc7695c407aa70756
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79322920"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知問題

> 適用于： Windows 系統管理中心、Windows 系統管理中心預覽

如果您發生本頁面未述及的問題，請[讓我們知道](https://aka.ms/WACfeedback)。

## <a name="installer"></a>安裝程式

- 使用您自己的憑證安裝 Windows Admin Center 時，要留意的是，如果您從認證管理員 MMC 工具複製憑證指紋，[它會在開頭包含無效的字元。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 此情況的因應措施是，先鍵入憑證指紋的第一個字元，再複製/貼上其餘部分。

- 不支援使用低於1024的埠。 在服務模式中，您可以選擇性地將埠80設定為重新導向至指定的埠。

## <a name="general"></a>一般

- 如果您將 Windows 系統管理中心安裝為**Windows Server 2016**上的閘道，請使用 [繁重]，此服務可能會在包含 ```Faulting application name: sme.exe``` 和 ```Faulting module name: WsmSvc.dll```的事件記錄檔中發生錯誤時損毀。 這是因為 Windows Server 2019 已修正的錯誤。 Windows Server 2016 的修補程式隨附于2月2019累積更新（ [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)）。

- 如果您已將 Windows 系統管理中心安裝為閘道，且您的連線清單似乎已損毀，請執行下列步驟：

   > [!WARNING]
   >這會刪除閘道上所有 Windows 系統管理中心使用者的連線清單和設定。

  1. 解除安裝 Windows Admin Center
  2. 刪除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下的 **Server Management Experience** 資料夾。
  3. 重新安裝 Windows Admin Center

- 如果您讓工具保持開啟且閒置很長一段時間，您可能會收到數個**錯誤: Runspace 狀態不適用於此操作**錯誤。 如果發生這種情況，請重新整理瀏覽器。 如果您遇到這種情況，請[將意見反應傳送給我們](https://aka.ms/WACfeedback)。

- 在 Windows 管理中心模組中執行的 OSS 版本號碼和協力廠商軟體注意事項中所列出的內容之間，可能會有些許差異。

### <a name="extension-manager"></a>延伸模組管理員

- 當您更新 Windows 管理中心時，必須重新安裝擴充功能。
- 如果您新增無法存取的延伸模組摘要，就不會出現警告。 [14412861]

## <a name="browser-specific-issues"></a>瀏覽器特定問題

### <a name="microsoft-edge"></a>Microsoft Edge

- 如果您已將 Windows 系統管理中心部署為服務，而且您使用 Microsoft Edge 作為瀏覽器，則在建立新的瀏覽器視窗之後，將閘道連線至 Azure 可能會失敗。 請嘗試在用戶端瀏覽器上新增 https://login.microsoftonline.com、 https://login.live.com和閘道的 URL 作為 [信任的網站] 和 [允許的網站] 快顯封鎖程式設定，以解決此問題。 如需修正此問題的詳細指引，請[參考疑難排解指南](troubleshooting.md#azure-features-dont-work-properly-in-edge)。 [17990376]

### <a name="google-chrome"></a>Google Chrome

- 在70版之前（2018年10月發行），Chrome 有關于 websocket 通訊協定和 NTLM 驗證的[錯誤](https://bugs.chromium.org/p/chromium/issues/detail?id=423609)。 這會影響下列工具：事件、PowerShell、遠端桌面。

- Chrome 可能會快顯多個認證提示，特別是在**工作群組** (非網域) 環境中加入連線體驗時。

- 如果您已將 Windows 系統管理中心部署為服務，則必須啟用閘道 URL 的快顯視窗，任何 Azure 整合功能才能運作。

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center 未使用 Mozilla Firefox 來測試，但大部分功能應該都能運作。

- Windows 10 安裝： Mozilla Firefox 有自己的憑證存放區，因此您必須將 ```Windows Admin Center Client``` 憑證匯入 Firefox，才能在 Windows 10 上使用 Windows 系統管理中心。

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>使用 proxy 服務時的 WebSocket 相容性

Windows Admin Center 中的遠端桌面、PowerShell 及事件模組會利用 WebSocket 通訊協定，而這通常在使用 Proxy 服務時不受支援。 Azure AD 應用程式 Proxy 相容性中的 Websocket 支援尚處於[預覽](https://blogs.technet.microsoft.com/applicationproxyblog/2018/03/28/limited-websocket-support-now-in-public-preview/)階段，正在尋求有關相容性的意見反應。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>2016之前的 Windows Server 版本支援（2012 R2、2012、2008 R2）

> [!NOTE]
> Windows 管理中心需要不包含在 Windows Server 2012 R2、2012或 2008 R2 中的 PowerShell 功能。 如果您要使用 Windows 系統管理中心來管理 Windows Server，您必須在這些伺服器上安裝 WMF 5.1 版或更新版本。

請在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF，且版本是 5.1 或更新版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。

## <a name="role-based-access-control-rbac"></a>以角色為基礎的存取控制（RBAC）

- RBAC 部署無法在設定為使用 Windows Defender 應用程式控制 (WDAC，先前稱為程式碼完整性) 的電腦上成功進行。[16568455]

- 若要在叢集中使用 RBAC，您必須將設定個別部署至每個成員節點。

- 部署 RBAC 時，您可能會收到不正確地歸因於 RBAC 設定的未經授權錯誤。 [16369238]

## <a name="server-manager-solution"></a>伺服器管理員解決方案

### <a name="certificates"></a>憑證

- 無法將 .PFX 加密憑證匯入至目前的使用者存放區。 [11818622]

### <a name="events"></a>事件

- 事件會受 [使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響。

- 匯出大型記錄檔時，您可能會收到參考「封包大小」的錯誤。

  - 若要解決此問題，請在閘道電腦上以提高許可權的命令提示字元中使用下列命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>檔案

- 尚未支援上傳或下載大型檔案。 （\~100mb 限制）[12524234]

### <a name="powershell"></a>PowerShell

- PowerShell 會受[使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響

- 處於桌面 PowerShell 主控台時以單一按右鍵方式貼上，會沒有作用。 反而會出現瀏覽器的快顯功能表，您可在其中選取 [貼上]。 您也可以使用 Ctrl-V。

- Ctrl-C 複製沒有作用，始終都會傳送 Ctrl-C 中斷命令給主控台。 透過按右鍵操作功能表複製會奏效。

- 當您讓 Windows Admin Center 視窗變小時，終端機內容會自動重排，但是再讓該視窗變大時，內容可能無法回復到先前狀態。 如果內容變得雜亂時，您可以嘗試 Clear-Host，或使用終端機上方的按鈕，中斷連線然後重新連線。

### <a name="registry-editor"></a>登錄編輯程式

- 尚未實行搜尋功能。 [13820009]

### <a name="remote-desktop"></a>遠端桌面

- 當 Windows 系統管理中心部署為服務時，遠端桌面工具可能會在將 Windows 系統管理中心服務更新為新版本之後無法載入。 若要解決此問題，請清除瀏覽器快取。   [23824194]

- 管理 Windows Server 2012 時，遠端桌面工具可能無法連接。 [20258278]

- 使用遠端桌面連線到未加入網域的電腦時，您必須以 ```MACHINENAME\USERNAME``` 格式輸入您的帳戶。

- 有些設定可以封鎖 Windows Admin Center 的遠端桌面用戶端與群組原則。 如果您遇到這種情況，請在 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections``` 下啟用 ```Allow users to connect remotely by using Remote Desktop Services```

- 遠端桌面受到[websocket 相容性](#websocket-compatibility-when-using-a-proxy-service)的影響。

- 遠端桌面工具目前不支援任何在本機桌面與遠端工作階段之間的文字、影像或檔案複製/貼上。

- 若要在遠端工作階段中執行任何複製/貼上，您可以按照正常方式複製 (按右鍵 + 複製或 Ctrl+C)，但貼上需要按右鍵 + 貼上 (Ctrl+V 無效)

- 您無法將下列按鍵命令傳送至遠端工作階段
  - Alt+Tab
  - 功能鍵
  - Windows 鍵
  - PrtScn

### <a name="roles-and-features"></a>角色和功能

- 選取來源無法使用的角色或功能進行安裝時，會略過這些來源。 [12946914]

- 如果您選擇不要在安裝角色後自動重新開機，我們就不會再次詢問。 [13098852]

- 如果您選擇要自動重新開機，將會在狀態更新進度達 100% 之前發生重新開機。 [13098852]

### <a name="storage"></a>存放

- 下層：DVD/CD/軟碟機不會顯示為下層的磁碟區。

- 下層：[磁碟區和磁碟] 中的部分屬性無法在下層使用，因此這些屬性會在詳細資料面板中顯示為未知或空白。

- 下層：建立新磁碟區時，ReFS 在 Windows 2012 和 2012 R2 電腦上僅支援 64K 的配置單位大小。 如果 ReFS 是在下層目標使用較小的配置單位大小所建立，檔案系統格式化將會失敗。 新的磁碟區將無法使用。 解決方法是刪除磁碟區，並使用 64K 配置單位大小。

### <a name="updates"></a>更新

- 安裝更新之後，可能會快取安裝狀態，而且需要重新整理瀏覽器。

- 嘗試設定 Azure 更新管理時，您可能會遇到下列錯誤：「金鑰集不存在」。 在此情況下，請在受管理的節點上嘗試下列補救步驟-
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

- 交換器內嵌小組 (SET)：將 NIC 加入至小組時，這些 NIC 必須是在相同的子網路上。

## <a name="computer-management-solution"></a>電腦管理解決方案

電腦管理解決方案包含伺服器管理員解決方案所提供工具的子集，因此適用同樣的已知問題，以及下列電腦管理解決方案特定問題：

- 如果您使用 Microsoft 帳戶（[MSA](https://account.microsoft.com/account/)），或使用 AZURE ACTIVE DIRECTORY （AAD）登入 Windows 10 電腦，則必須使用 [管理-as] 來提供本機系統管理員帳戶的認證 [16568455]

- 當您嘗試管理 localhost 時，系統會提示您提高閘道程序的權限。 如果您在接下來的 [使用者帳戶控制] 快顯視窗中按一下 [**否**]，您必須取消連線嘗試並從頭開始。

- Windows 10 預設不會開啟 WinRM/PowerShell 遠端功能。
  
  - 若要啟用 Windows 10 用戶端管理，您必須從提升權限的 PowerShell 命令提示字元發出命令 ```Enable-PSRemoting```。

  - 您可能還需要使用 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 更新防火牆，以允許來自外部本機子網路的連線。 如需更多嚴格網路案例，請參閱[此文件](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## <a name="failover-cluster-manager-solution"></a>容錯移轉叢集管理員解決方案

- 管理叢集時，(超融合式或傳統式？) 可能會發生**找不到命令介面**錯誤。 如果發生這種情況，請重新載入瀏覽器，或往外瀏覽至其他工具，然後再瀏覽器回來。 [13882442]

- 管理尚未設定完全的下層 (Windows Server 2012 或 2012 R2) 叢集時，可能會發生問題。 此問題的修正方式為，確保叢集的**每個成員節點**皆已安裝並啟用 Windows 功能 **RSAT-Clustering-PowerShell**。 若要使用 PowerShell 達成此目的，請在所有叢集節點上輸入下列命令 `Install-WindowsFeature -Name RSAT-Clustering-PowerShell`。 [12524664]

- 叢集可能必須使用整個 FQDN 來新增，才能正確地探索到該叢集。

- 連線至使用安裝為閘道之 Windows Admin Center 的叢集時，並提供明確使用者名稱/密碼進行驗證時，您必須選取 **\[使用這些認證進行所有連線\]** ，讓認證可用於查詢成員節點。

## <a name="hyper-converged-cluster-manager-solution"></a>超融合式叢集管理員解決方案

- 有些命令 (例如 **Drives - Update firmware**、**Servers - Remove** 和 **Volumes - Open**) 已停用，而且目前不受支援。

## <a name="azure-services"></a>Azure 服務

### <a name="azure-file-sync-permissions"></a>Azure 檔案同步許可權

Azure 檔案同步需要 Azure 中 Windows 系統管理中心未提供1910版之前的許可權。 如果您使用早于 Windows 管理中心版本1910的版本向 Azure 註冊您的 Windows 管理中心閘道，則必須更新 Azure Active Directory 應用程式，以取得在最新版本中使用 Azure 檔案同步的正確許可權Windows 系統管理中心。 其他許可權可讓 Azure 檔案同步執行自動設定儲存體帳戶存取權，如這篇文章中所述：[確定 Azure 檔案同步有權存取儲存體帳戶](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal)。

若要更新您的 Azure Active Directory 應用程式，您可以執行兩個動作的其中一項
1. 移至 [**設定**] > **Azure** > **取消註冊**，然後再次向 azure 註冊 Windows 系統管理中心，並確定您選擇建立新的 Azure Active Directory 應用程式。 
2. 移至您的 Azure Active Directory 應用程式，並手動將您現有 Azure Active Directory 應用程式所需的許可權新增到已向 Windows 系統管理中心註冊。 若要這麼做，請移至**azure 中的**[**設定**] > **azure** > View。 從 Azure 的 [**應用程式註冊**] 分頁中，移至 [ **API 許可權**]，選取 [**新增許可權**]。 依序選取 [ **Azure Active Directory 圖形]** 、[**委派的許可權**] 和 [**目錄**]，然後選取 [ **AccessAsUser**]。 按一下 [**新增許可權**]，將更新儲存至應用程式。

### <a name="options-for-setting-up-azure-management-services"></a>設定 Azure 管理服務的選項

包括 Azure 監視器、Azure 更新管理和 Azure 資訊安全中心的 Azure 管理服務，會針對內部部署伺服器使用相同的代理程式： Microsoft Monitoring Agent。 Azure 更新管理有一組較有限的支援區域，而且需要將 Log Analytics 工作區連結至 Azure 自動化帳戶。 基於這項限制，如果您想要在 Windows 管理中心設定多個服務，您必須先設定 Azure 更新管理，然後 Azure 資訊安全中心或 Azure 監視器。 如果您已設定任何使用 Microsoft Monitoring Agent 的 Azure 管理服務，然後嘗試使用 Windows 管理中心來設定 Azure 更新管理，Windows 系統管理中心只會允許您設定 Azure 更新管理（如果現有的連結至 Microsoft Monitoring Agent 的資源支援 Azure 更新管理。 如果不是這種情況，您有兩個選項：

1. 移至 [控制台] > Microsoft Monitoring Agent，將[您的伺服器與現有的 Azure 管理解決方案](https://docs.microsoft.com/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics)（例如 Azure 監視器或 Azure 資訊安全中心）中斷連接。 然後在 Windows 系統管理中心設定 Azure 更新管理。 之後，您可以透過 Windows 系統管理中心返回設定其他 Azure 管理解決方案，而不會發生問題。
2. 您可以[手動設定 azure 更新管理所需的 azure 資源](https://docs.microsoft.com/azure/automation/automation-update-management)，然後[手動更新 Microsoft Monitoring Agent](https://docs.microsoft.com/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) （在 Windows 系統管理中心以外），以新增對應至您想要使用之更新管理解決方案的新工作區。
