---
title: Windows Admin Center 已知問題
description: Windows Admin Center 已知問題 (Project Honolulu)
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: b4d7d039c775b85321d168f8de7415de6b92e784
ms.sourcegitcommit: 97a65d8f52514848963e8917021bd9a1f6ee3b19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/02/2020
ms.locfileid: "89287820"
---
# <a name="windows-admin-center-known-issues"></a>Windows Admin Center 已知問題

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

如果您發生本頁面未述及的問題，請[讓我們知道](https://aka.ms/WACfeedback)。

## <a name="installer"></a>Installer

- 使用您自己的憑證安裝 Windows Admin Center 時，要留意的是，如果您從認證管理員 MMC 工具複製憑證指紋，[它會在開頭包含無效的字元。](https://support.microsoft.com/help/2023835/certificate-thumbprint-displayed-in-mmc-certificate-snap-in-has-extra) 此情況的因應措施是，先鍵入憑證指紋的第一個字元，再複製/貼上其餘部分。

- 不支援使用低於1024的埠。 在服務模式中，您可以選擇性地設定埠80，以重新導向至指定的埠。

## <a name="general"></a>一般

- 在1910.2 版的 Windows Admin Center 中，您可能無法連接到特定硬體上的 Hyper-v 伺服器。 如果您已封鎖此問題， [請下載我們先前的組建](https://aka.ms/wacprevious)。

- 如果您在繁重使用的情況下，將 Windows Admin Center 安裝為 **Windows Server 2016** 上的閘道，則服務可能會損毀，並在包含與的事件記錄檔中發生錯誤 ```Faulting application name: sme.exe``` ```Faulting module name: WsmSvc.dll``` 。 這是因為 Windows Server 2019 中已修正的錯誤。 Windows Server 2016 的修補套裝程式含2月2019累積更新 [KB4480977](https://www.catalog.update.microsoft.com/Search.aspx?q=4480977)。

- 如果您已 Windows Admin Center 安裝為閘道，而您的連線清單似乎已損毀，請執行下列步驟：

   > [!WARNING]
   >這會刪除閘道上所有 Windows Admin Center 使用者的連接清單和設定。

  1. 解除安裝 Windows Admin Center
  2. 刪除 **C:\Windows\ServiceProfiles\NetworkService\AppData\Roaming\Microsoft** 下的 **Server Management Experience** 資料夾。
  3. 重新安裝 Windows Admin Center

- 如果您讓工具保持開啟且閒置很長一段時間，您可能會收到數個**錯誤: Runspace 狀態不適用於此操作**錯誤。 如果發生這種情況，請重新整理瀏覽器。 如果您遇到這種情況，請 [傳送意見](https://aka.ms/WACfeedback)反應給我們。

- 在 Windows Admin Center 模組中執行的 OSS 版本號碼之間可能會有微小的差異，以及協力廠商軟體聲明中所列的內容。

### <a name="extension-manager"></a>擴充管理員

- 當您更新 Windows Admin Center 時，您必須重新安裝您的擴充功能。
- 如果您加入無法存取的延伸模組摘要，就不會出現任何警告。 [14412861]

## <a name="browser-specific-issues"></a>瀏覽器特定問題

### <a name="microsoft-edge"></a>Microsoft Edge

- 如果您已將 Windows Admin Center 部署為服務，且您使用 Microsoft Edge 作為瀏覽器，則在產生新的瀏覽器視窗之後，將您的閘道連接至 Azure 可能會失敗。 若要解決此問題，請 https://login.microsoftonline.com https://login.live.com 在用戶端瀏覽器上新增、和閘道的 URL 作為受信任的網站，以及允許的快顯封鎖程式設定網站。 如需有關在 [疑難排解指南](troubleshooting.md#azure-features-dont-work-properly-in-edge)中修正此問題的詳細指引。 [17990376]

### <a name="google-chrome"></a>Google Chrome

- 在70版之前的版 (發行之前，2018) Chrome 有關于 websocket 通訊協定和 NTLM 驗證的 [錯誤](https://bugs.chromium.org/p/chromium/issues/detail?id=423609) 。 這會影響下列工具：事件、PowerShell、遠端桌面。

- Chrome 可能會快顯多個認證提示，特別是在**工作群組** (非網域) 環境中加入連線體驗時。

- 如果您已將 Windows Admin Center 部署為服務，則需要啟用來自閘道 URL 的快顯視窗，任何 Azure 整合功能才能運作。

### <a name="mozilla-firefox"></a>Mozilla Firefox

Windows Admin Center 未使用 Mozilla Firefox 來測試，但大部分功能應該都能運作。

- Windows 10 安裝： Mozilla Firefox 有自己的憑證存放區，因此您必須將憑證匯入 ```Windows Admin Center Client``` Firefox，才能在 Windows 10 上使用 Windows Admin Center。

## <a name="websocket-compatibility-when-using-a-proxy-service"></a>使用 Proxy 服務時的 WebSocket 相容性

Windows Admin Center 中的遠端桌面、PowerShell 及事件模組會利用 WebSocket 通訊協定，而這通常在使用 Proxy 服務時不受支援。

## <a name="support-for-windows-server-versions-before-2016-2012-r2-2012-2008-r2"></a>2016 (2012 R2、2012、2008 R2) 之前的 Windows Server 版本支援

> [!NOTE]
> Windows Admin Center 需要 Windows Server 2012 R2、2012或 2008 R2 中未包含的 PowerShell 功能。 如果您要使用 Windows Admin Center 管理 Windows Server，您必須在這些伺服器上安裝 WMF 5.1 版或更高版本。

請在 PowerShell 中輸入 `$PSVersiontable`，確認已安裝 WMF，且版本是 5.1 或更新版本。

如果未安裝，您可以[下載並安裝 WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616)。

## <a name="role-based-access-control-rbac"></a>角色型存取控制 (RBAC)

- RBAC 部署無法在設定為使用 Windows Defender 應用程式控制 (WDAC，先前稱為程式碼完整性) 的電腦上成功進行。[16568455]

- 若要在叢集中使用 RBAC，您必須將設定個別部署至每個成員節點。

- 部署 RBAC 時，您可能會收到不正確地歸因於 RBAC 設定的未經授權錯誤。 [16369238]

## <a name="server-manager-solution"></a>伺服器管理員解決方案

### <a name="certificates"></a>憑證

- 無法將 .PFX 加密憑證匯入至目前的使用者存放區。 [11818622]

### <a name="events"></a>事件

- 事件會受 [使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響。

- 匯出大型記錄檔時，您可能會收到參考「封包大小」的錯誤。

  - 若要解決此問題，請在閘道電腦上提高許可權的命令提示字元中使用下列命令： ```winrm set winrm/config @{MaxEnvelopeSizekb="8192"}```

### <a name="files"></a>檔案儲存體

- 尚未支援上傳或下載大型檔案。  (\~ 100mb 限制) [12524234]

### <a name="powershell"></a>PowerShell

- PowerShell 會受[使用 Proxy 服務時的 WebSocket 相容性](#websocket-compatibility-when-using-a-proxy-service)影響

- 處於桌面 PowerShell 主控台時以單一按右鍵方式貼上，會沒有作用。 反而會出現瀏覽器的快顯功能表，您可在其中選取 [貼上]。 您也可以使用 Ctrl-V。

- Ctrl-C 複製沒有作用，始終都會傳送 Ctrl-C 中斷命令給主控台。 透過按右鍵操作功能表複製會奏效。

- 當您讓 Windows Admin Center 視窗變小時，終端機內容會自動重排，但是再讓該視窗變大時，內容可能無法回復到先前狀態。 如果內容變得雜亂時，您可以嘗試 Clear-Host，或使用終端機上方的按鈕，中斷連線然後重新連線。

### <a name="registry-editor"></a>登錄編輯器

- 尚未實行搜尋功能。 [13820009]

### <a name="remote-desktop"></a>遠端桌面

- 當 Windows Admin Center 部署為服務時，在將 Windows Admin Center 服務更新至新版本之後，遠端桌面工具可能無法載入。 若要解決此問題，請清除瀏覽器快取。   [23824194]

- 管理 Windows Server 2012 時，遠端桌面工具可能無法連線。 [20258278]

- 使用遠端桌面連線到未加入網域的電腦時，您必須以格式輸入您的帳戶 ```MACHINENAME\USERNAME``` 。

- 某些設定可以封鎖 Windows Admin Center 具有群組原則的遠端桌面用戶端。 如果您遇到這種情況， ```Allow users to connect remotely by using Remote Desktop Services``` 請啟用 ```Computer Configuration/Policies/Administrative Templates/Windows Components/Remote Desktop Services/Remote Desktop Session Host/Connections```

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

### <a name="storage"></a>儲存體

- 下層：DVD/CD/軟碟機不會顯示為下層的磁碟區。

- 下層：[磁碟區和磁碟] 中的部分屬性無法在下層使用，因此這些屬性會在詳細資料面板中顯示為未知或空白。

- 下層：建立新磁碟區時，ReFS 在 Windows 2012 和 2012 R2 電腦上僅支援 64K 的配置單位大小。 如果 ReFS 是在下層目標使用較小的配置單位大小所建立，檔案系統格式化將會失敗。 新的磁碟區將無法使用。 解決方法是刪除磁碟區，並使用 64K 配置單位大小。

### <a name="updates"></a>更新

- 安裝更新之後，安裝狀態可能會快取，且需要重新整理瀏覽器。

- 嘗試設定 Azure 更新管理時，您可能會遇到錯誤：「索引鍵集不存在」。 在此情況下，請嘗試在受管理的節點上執行下列補救步驟-
    1. 停止「密碼編譯服務」服務。
    2. 視需要變更資料夾選項以顯示隱藏的檔案 () 。
    3. 到達 "%allusersprofile%\Microsoft\Crypto\RSA\S-1-5-18" 資料夾並刪除其所有內容。
    4. 重新開機「密碼編譯服務」服務。
    5. 使用 Windows Admin Center 重複設定更新管理

### <a name="virtual-machines"></a>虛擬機器

- 管理 Windows Server 2012 主機上的虛擬機器時，瀏覽器內的 VM connect 工具將無法連線至 VM。 下載 .rdp 檔案以連線至 VM 仍應可運作。 [20258278]

- Azure Site Recovery –如果在 WAC 以外的主機上設定 ASR，您將無法從 WAC 內保護 VM [18972276]

- 目前不支援 Hyper-V 管理員提供的進階功能，例如虛擬 SAN 管理員、移動 VM、匯出 VM、VM 複寫。

### <a name="virtual-switches"></a>虛擬交換器

- 交換器內嵌小組 (SET)：將 NIC 加入至小組時，這些 NIC 必須是在相同的子網路上。

## <a name="computer-management-solution"></a>電腦管理解決方案

電腦管理解決方案包含伺服器管理員解決方案所提供工具的子集，因此適用同樣的已知問題，以及下列電腦管理解決方案特定問題：

- 如果您使用 Microsoft 帳戶 ([MSA](https://account.microsoft.com/account/)) 或使用 AZURE ACTIVE DIRECTORY (AAD) 登入您 Windows 10 電腦，則必須使用 [管理身分] 來提供本機系統管理員帳戶的認證 [16568455]

- 當您嘗試管理 localhost 時，系統會提示您提高閘道程序的權限。 如果您在接下來的 [使用者帳戶控制] 快顯視窗中按一下 [ **否** ]，則必須取消連接嘗試並重新開始。

- Windows 10 預設不會啟用 WinRM/PowerShell 遠端功能。

  - 若要啟用 Windows 10 用戶端管理，您必須從提升權限的 PowerShell 命令提示字元發出命令 ```Enable-PSRemoting```。

  - 您可能還需要使用 ```Set-NetFirewallRule -Name WINRM-HTTP-In-TCP -RemoteAddress Any``` 更新防火牆，以允許來自外部本機子網路的連線。 如需更多嚴格網路案例，請參閱[此文件](/powershell/module/microsoft.powershell.core/enable-psremoting?view=powershell-5.1)。

## <a name="cluster-deployment"></a>叢集部署

### <a name="step-12"></a>步驟1。2
新增伺服器時，目前不支援混合的工作組電腦。 用於群集的所有機器都必須屬於相同的工作組。 如果沒有，則會停用 [下一步] 按鈕，並會出現下列錯誤：「無法在不同 Active Directory 網域中的伺服器建立叢集。 請確認伺服器名稱是否正確。 請將所有伺服器移至相同的網域，然後再試一次。」

### <a name="step-14"></a>步驟1。4
Hyper-v 必須安裝在執行 Azure Stack HCI OS 的虛擬機器上。 嘗試啟用這些虛擬機器的 Hyper-v 功能將會失敗，並出現下列錯誤：

![Hyper-v 啟用錯誤的螢幕擷取畫面](../media/cluster-create-install-hyperv.png)

若要在執行 Azure Stack HCI OS 的虛擬機器上安裝 Hyper-v，請執行下列命令：

```PowerShell
Enable-WindowsOptionalFeature -Online -FeatureName 'Microsoft-Hyper-V'
```

### <a name="step-17"></a>步驟1。7
在安裝更新之後，伺服器有時會花費比預期更長的時間來重新開機。 Windows Admin Center 叢集部署嚮導會定期檢查伺服器重新開機狀態，以得知伺服器是否已成功重新開機。 但是，如果使用者以手動方式在 wizard 之外重新開機伺服器，則 wizard 無法以適當的方式來捕捉伺服器狀態。

如果您想要手動重新開機伺服器，請結束目前的 wizard 會話。 重新開機伺服器之後，您可以重新啟動精靈。

### <a name="stretch-cluster-creation"></a>Stretch cluster 建立
建議您在建立延展叢集時使用已加入網域的伺服器。 由於 WinRM 的限制，嘗試使用工作組電腦進行延展叢集部署時，發生網路分割問題。

### <a name="undo-and-start-over"></a>復原並重新開始
在叢集部署中重複使用相同的電腦時，清除先前的叢集實體對於在同一組電腦中取得成功的叢集部署而言很重要。 請參閱 [部署超交集基礎結構](../use/deploy-hyperconverged-infrastructure.md#undo-and-start-over) 的頁面，以取得如何清除叢集的指示。

### <a name="credssp"></a>CredSSP
Windows Admin Center cluster deployment wizard 在數個地方使用 CredSSP。 您會在嚮導期間遇到這個錯誤訊息 (這會在 [驗證叢集] 步驟) 中最常發生：

![叢集建立 CredSSP 錯誤的螢幕擷取畫面](../media/cluster-create-credssp-error.jpg)

您可以使用下列步驟進行疑難排解：

1. 停用所有節點和 Windows Admin Center 閘道電腦上的 CredSSP 設定。 在您的閘道電腦上執行第一個命令，並在您的叢集中的所有節點上執行第二個命令：

```PowerShell
Disable-WsmanCredSSP -Role Client
```
```PowerShell
Disable-WsmanCredSSP -Role Server
```
2. 修復所有節點上的信任。 在所有節點上執行下列命令：
```PowerShell
Test-ComputerSecureChannel -Verbose -Repair -Credential <account name>
```

3. 使用命令重設群組原則傳播資料
```Command Line
gpupdate /force
```

4. 重新開機節點。 重新開機之後，請使用下列命令，測試閘道電腦與目標節點之間的連線，以及節點之間的連線能力：
```PowerShell
Enter-PSSession -computername <node fqdn>
```

### <a name="nested-virtualization"></a>巢狀虛擬化
在虛擬機器上驗證 Azure Stack HCI OS 叢集部署時，必須先開啟嵌套虛擬化，才能使用下列 powershell 命令啟用角色/功能：

```PowerShell
Set-VMProcessor -VMName <VMName> -ExposeVirtualizationExtensions $true
```

  > [!Note]
  > 若要讓虛擬交換器小組在虛擬機器環境中成功，在建立虛擬機器之後，必須在主機上的 PowerShell 中執行下列命令： Get vm |% {set-VMNetworkAdapter-VMName $ _。Name-MacAddressSpoofing On-AllowTeaming on}

如果您是使用 Azure Stack HCI 作業系統來部署叢集，則會有其他需求。 VM 開機虛擬硬碟必須預先安裝 Hyper-v 功能。 若要這樣做，請在建立虛擬機器之前，先執行下列命令：

```PowerShell
Install-WindowsFeature –VHD <Path to the VHD> -Name Hyper-V, RSAT-Hyper-V-Tools, Hyper-V-PowerShell
```

### <a name="support-for-rdma"></a>支援 RDMA
Windows Admin Center 2007 版中的叢集部署嚮導不提供 RDMA 設定的支援。

## <a name="failover-cluster-manager-solution"></a>容錯移轉叢集管理員解決方案

- 管理叢集時，(超融合式或傳統式？) 可能會發生**找不到命令介面**錯誤。 如果發生這種情況，請重新載入瀏覽器，或往外瀏覽至其他工具，然後再瀏覽器回來。 [13882442]

- 管理尚未設定完全的下層 (Windows Server 2012 或 2012 R2) 叢集時，可能會發生問題。 此問題的修正方式為，確保叢集的**每個成員節點**皆已安裝並啟用 Windows 功能 **RSAT-Clustering-PowerShell**。 若要使用 PowerShell 達成此目的，請在所有叢集節點上輸入下列命令 `Install-WindowsFeature -Name RSAT-Clustering-PowerShell`。 [12524664]

- 叢集可能必須使用整個 FQDN 來新增，才能正確地探索到該叢集。

- 連線至使用安裝為閘道之 Windows Admin Center 的叢集時，並提供明確使用者名稱/密碼進行驗證時，您必須選取 **\[使用這些認證進行所有連線\]**，讓認證可用於查詢成員節點。

## <a name="hyper-converged-cluster-manager-solution"></a>超融合式叢集管理員解決方案

- 有些命令 (例如 **Drives - Update firmware**、**Servers - Remove** 和 **Volumes - Open**) 已停用，而且目前不受支援。

## <a name="azure-services"></a>Azure 服務

### <a name="azure-file-sync-permissions"></a>Azure 檔案同步許可權

Azure 檔案同步需要 Azure 中 Windows Admin Center 未在1910版之前提供的許可權。 如果您使用 Windows Admin Center 1910 版之前的版本向 Azure 註冊 Windows Admin Center 閘道，您將需要更新 Azure Active Directory 應用程式，以取得正確的許可權，以在最新版本的 Windows Admin Center 中使用 Azure 檔案同步。 額外的許可權可讓 Azure 檔案同步依照本文所述，執行儲存體帳戶存取的自動設定： [確定 Azure 檔案同步具有儲存體帳戶的存取](/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2cazure-portal#tabpanel_CeZOj-G++Q-5_azure-portal)權。

若要更新您的 Azure Active Directory 應用程式，您可以進行下列兩項作業之一：
1. 移至 [**設定**  >  **azure**  >  **取消註冊**]，然後再次向 azure 註冊 Windows Admin Center，確定您選擇建立新的 Azure Active Directory 應用程式。
2. 移至您的 Azure Active Directory 應用程式，並手動將現有 Azure Active Directory 應用程式所需的許可權新增至 Windows Admin Center 註冊的應用程式。 若要這樣做，請**Settings**移至  >  **Azure**  >  **azure 中的**[設定] azure View。 從 Azure 中的 **應用程式註冊** 分頁，移至 [ **API 許可權**]，然後選取 [ **新增許可權**]。 向下展開以選取 **Azure Active Directory Graph**，選取 [ **委派的許可權**]，展開 [ **目錄**]，然後選取 [ **AccessAsUser**]。 按一下 [ **新增許可權** ]，將更新儲存至應用程式。

### <a name="options-for-setting-up-azure-management-services"></a>設定 Azure 管理服務的選項

包含 Azure 監視器、Azure 更新管理和 Azure 資訊安全中心的 Azure 管理服務，對內部部署伺服器使用相同的代理程式： Microsoft Monitoring Agent。 Azure 更新管理有一組較受支援的區域，而且需要將 Log Analytics 工作區連結至 Azure 自動化帳戶。 由於這項限制，如果您想要在 Windows Admin Center 中設定多個服務，您必須先設定 Azure 更新管理，然後 Azure 資訊安全中心或 Azure 監視器。 如果您已設定任何使用 Microsoft Monitoring Agent 的 Azure 管理服務，然後嘗試使用 Windows Admin Center 來設定 Azure 更新管理，則只有當連結到更新管理的現有資源支援 Azure Microsoft Monitoring Agent 時，Windows Admin Center 才可讓您設定 Azure 更新管理。 如果不是這種情況，您有兩個選項：

1. 移至主控台 > Microsoft Monitoring Agent，將 [您的伺服器與現有的 Azure 管理解決方案中斷](/azure/azure-monitor/platform/log-faq#q-how-do-i-stop-an-agent-from-communicating-with-log-analytics) 連線 (例如 Azure 監視器或 Azure 資訊安全中心) 。 然後設定 Windows Admin Center 中的 Azure 更新管理。 之後，您可以透過 Windows Admin Center 來設定其他 Azure 管理解決方案，而不會有任何問題。
2. 您可以 [手動設定 azure 更新管理所需的 azure 資源](/azure/automation/automation-update-management) ，然後 [手動更新](/azure/azure-monitor/platform/agent-manage#adding-or-removing-a-workspace) Windows Admin Center) 以外的 Microsoft Monitoring Agent (，以新增對應到您想要使用之更新管理解決方案的新工作區。
