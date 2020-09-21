---
title: Windows Admin Center 常見問題集
description: 取得有關 Windows Admin Center (Project Honolulu) 的問題解答
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 12/02/2019
ms.openlocfilehash: f33707050f6686ad285fc8ecb786456f0157fef7
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2020
ms.locfileid: "90767011"
---
# <a name="windows-admin-center-frequently-asked-questions"></a>Windows Admin Center 常見問題集

> 適用於：Windows Admin Center、Windows Admin Center 預覽版

以下是有關 Windows Admin Center 最常見問題的解答。

## <a name="what-is-windows-admin-center"></a>什麼是 Windows Admin Center？

Windows Admin Center 是輕量的瀏覽器型 GUI 平台與工具組，供 IT 系統管理員用來管理 Windows Server 和 Windows 10。 這是熟悉的 [伺服器管理員] 和 Microsoft Management Console (MMC) 等系統隨附管理工具逐漸發展成現代簡化整合式資訊安全體驗的演進。

## <a name="can-i-use-windows-admin-center-in-production-environments"></a>我可以在生產環境中使用 Windows Admin Center 嗎？

是。 Windows Admin Center 已正式運作，並且已準備就緒可供廣泛使用和生產部署。 目前的平台功能及核心工具符合 Microsoft 的標準發行準則，以及我們對可用性、可靠性、效能、協助工具、安全性和採用率的品質規範。

[!INCLUDE [support-policy](../includes/support-policy.md)]

## <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>使用 Windows Admin Center 需要多少費用？

Windows Admin Center 除了 Windows 本身以外，不需另付費用。 您可以在 Windows Server 或 Windows 10 的有效授權下免費使用 Windows Admin Center (可供個別下載)，這已依據 Windows 增補 EULA 取得授權。

## <a name="what-versions-of-windows-server-can-i-manage-with-windows-admin-center"></a>哪些版本的 Windows Server 可以使用 Windows Admin Center 來管理？

Windows Admin Center 經過最佳化，最適用於 Windows Server 2019，並且會在即將發行的 Windows Server 2019 版本中啟用重要主題：尤其是混合式雲端案例和超融合式基礎結構管理。 雖然 Windows Admin Center 最適合與 Windows Server 2019 搭配使用，但也支援管理各種客戶已經使用的版本：完全支援 Windows Server 2012 和更新版本。 另外還有用於管理 Windows Server 2008 R2 的有限功能。

## <a name="is-windows-admin-center-a-complete-replacement-for-all-traditional-in-box-and-rsat-tools"></a>Windows Admin Center 是否會完全取代所有傳統的隨附工具和 RSAT 工具？

不可以。 Windows Admin Center 可以管理許多常見的案例，但不會完全取代所有傳統 Microsoft Management Console (MMC) 工具。 如需詳細查看 Windows Admin Center 中隨附哪些工具，請閱讀我們文件中更多有關[管理伺服器](../use/manage-servers.md)的資訊。 Windows Admin Center 在其伺服器管理員解決方案中有下列重要功能：

* 顯示資源和資源使用率
* 憑證管理
* 管理裝置
* 事件檢視器
* 檔案總管
* 防火牆管理
* 管理已安裝的應用程式
* 設定本機使用者和群組
* 網路設定
* 檢視/結束處理序，以及建立處理序傾印
* 登錄編輯
* 管理排定的工作
* 管理 Windows 服務
* 啟用/停用角色和功能
* 管理 Hyper-V VM 和虛擬交換器
* 管理儲存空間
* 管理儲存體複本
* 管理 Windows 更新
* PowerShell 主控台
* 遠端桌面連線

Windows Admin Center 還提供下列解決方案：

* 電腦管理 – 提供管理 Windows 10 用戶端電腦所需的伺服器管理員功能子集
* 容錯移轉叢集管理員 – 提供持續管理容錯移轉叢集和叢集資源的支援
* 超融合式叢集管理員 – 提供專為儲存空間直接存取及 Hyper-V 量身訂作的全新體驗。 它具有儀表板，並強調監控所用的圖表和警示。

Windows Admin Center 與 RSAT (遠端伺服器管理工具) 相輔相成，但不會加以取代，因為 Active Directory、DHCP、DNS、IIS 等角色還沒有 Windows Admin Center 中所提供的對等管理功能。

## <a name="can-windows-admin-center-be-used-to-manage-the-free-microsoft-hyper-v-server"></a>是否可以使用 Windows Admin Center 來管理免費的 Microsoft Hyper-V Server？

是。 Windows Admin Center 可以用來管理 Microsoft Hyper-V Server 2016 和 Microsoft Hyper-V Server 2012 R2。

## <a name="can-i-deploy-windows-admin-center-on-a-windows-10-computer"></a>是否可以在 Windows 10 電腦上部署 Windows Admin Center？

是的，可以將 Windows Admin Center 安裝在執行於桌面模式中的 Windows 10 (版本 1709 或更新版本)。  也可以將 Windows Admin Center 安裝在以閘道模式執行 Windows Server 2016 或更新版本的伺服器上，然後透過 Windows 10 電腦中的網頁瀏覽器進行存取。 [深入了解安裝選項](../plan/installation-options.md)。

## <a name="ive-heard-that-windows-admin-center-uses-powershell-under-the-hood-can-i-see-the-actual-scripts-that-it-uses"></a>我聽過 Windows Admin Center 會在幕後使用 PowerShell，請問我能看到其使用的實際指令碼嗎？

是的！ [Showscript 功能](../use/get-started.md#view-powershell-scripts-used-in-windows-admin-center)已新增至 Windows Admin Center 預覽版本 1806，而且現在已包含在 GA 通道中。

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-windows-server-2008-r2-or-earlier"></a>Windows Admin Center 是否有任何方案可以管理 Windows Server 2008 R2 或更早版本？

Windows Admin Center **不再支援**管理 Windows Server 2008 R2 的功能。 Windows Admin Center 需依賴 Windows Server 2008 R2 中所沒有的 PowerShell 功能和平台技術，使完整支援變得可行。 如果您尚未具有該功能，Microsoft 建議[移至 Azure 或升級至 Windows Server 最新版](https://www.microsoft.com/cloud-platform/windows-server-2008)。

## <a name="are-there-any-plans-for-windows-admin-center-to-manage-linux-connections"></a>是否有任何要讓 Windows Admin Center 管理 Linux 連線的計劃？

我們因為客戶要求而正在調查，但目前還沒有鎖定計畫要提供，而支援可能只包含透過 SSH 的主控台連線。

## <a name="which-web-browsers-are-supported-by-windows-admin-center"></a>Windows Admin Center 支援哪些網頁瀏覽器？

最新版本的 Microsoft Edge (Windows 10 版本 1709 或更新版本)、Google Chrome 和 [Microsoft Edge Insider](https://microsoftedgeinsider.com) 已在 Windows 10 上測試過並且可受到支援。 [檢視瀏覽器特有的已知問題](../support/known-issues.md#browser-specific-issues)。 其他新式網頁瀏覽器或其他平台目前未納入我們的測試矩陣，因此沒有「正式」  支援。

## <a name="how-does-windows-admin-center-handle-security"></a>Windows Admin Center 如何處理安全性？

從瀏覽器至 Windows Admin Center 閘道的流量使用 HTTPS。 從閘道至受管理伺服器的流量是透過 WinRM 的標準 PowerShell 和 WMI。 我們支援 LAPS (區域系統管理員密碼解決方案)、資源型限制委派、閘道存取控制 (使用 AD 或 Azure AD) 以及角色型存取控制，以進行目標伺服器管理。

## <a name="does-windows-admin-center-use-credssp"></a>Windows Admin Center 會使用 CredSSP 嗎？

是的，在少數情況下，Windows Admin Center 會需要 CredSSP。 除了傳送認證至您鎖定的特定伺服器來進行管理外，您還必須傳送認證至機器以用於驗證。 例如，如果您要管理**伺服器 B** 上的虛擬機器，但想要將這些虛擬機器的 vhdx 檔案儲存在由**伺服器 C** 所託管的檔案共用上，Windows Admin Center 就必須使用 CredSSP 來向**伺服器 C** 存取檔案共用。

Windows Admin Center 會在提示您進行同意後，自動處理 CredSSP 的設定。 設定 CredSSP 之前，Windows Admin Center 會檢查並確定系統具有最新的 CredSSP [更新](https://support.microsoft.com/help/4093492/credssp-updates-for-cve-2018-0886-march-13-2018)。

CredSSP 目前會在下列區域中使用：

- 在虛擬機器工具使用分離式 SMB 存放區 (上述範例)。
- 在更容錯移轉或超融合式叢集管理解決方案中使用更新工具，以執行[叢集感知更新](../../../failover-clustering/cluster-aware-updating.md)

## <a name="are-there-any-cloud-dependencies"></a>是否有任何雲端相依性？

Windows Admin Center 不需要網際網路存取，也不需要 Microsoft Azure。 Windows Admin Center 會在任何位置管理 Windows Server 和 Windows 執行個體：在實體系統、在任何 Hypervisor 中的任何虛擬機器，或在任何雲端執行的。 雖然與各種 Azure 服務的整合將隨著時間陸續新增，但這些會是選用的加值功能，並不是使用 Windows Admin Center 的必要條件。

## <a name="are-there-any-other-dependencies-or-prerequisites"></a>是否有任何其他相依性或必要條件？

可以將 Windows Admin Center 安裝在 Windows 10 秋季年度更新版 (1709) 或更新版本，或是 Windows Server 2016 或更新版本。 若要管理 Windows Server 2008 R2、2012 或 2012 R2，這些伺服器上必須有 Windows Management Framework 5.1 安裝。 沒有任何其他相依性。 不需要 IIS、不需要代理程式，也不需要 SQL Server。

## <a name="what-about-extensibility-and-3rd-party-support"></a>擴充性和第三方支援呢？

Windows Admin Center 有可用的 SDK，因此讓任何人都可以撰寫自己的擴充功能。 做為一個平台，擴展生態系統和啟用協力廠商擴充性，自開始以來一直都是我們重要的優先考量。 [深入了解 Windows Admin Center SDK](../extend/extensibility-overview.md)。

## <a name="can-i-manage-hyper-converged-infrastructure-with-windows-admin-center"></a>是否可以使用 Windows Admin Center 來管理超融合式基礎結構？

是。 Windows Admin Center 支援管理執行 Windows Server 2016 或 Windows Server 2019 組建的超融合式叢集。 Windows Admin Center 中的超融合式叢集管理員解決方案先前是以預覽版提供，但現在已「正式推出」  (有些新功能仍在預覽狀態)。 如需詳細資訊，請[閱讀更多有關管理超融合式基礎結構的資訊](../use/manage-hyper-converged.md)。

## <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center 是否需要 System Center？

不可以。 Windows Admin Center 可與 System Center 互補，但 System Center 並非必要的。 [深入了解 Windows Admin Center 和 System Center](related-management.md#system-center)。

## <a name="can-windows-admin-center-replace-system-center-virtual-machine-manager-scvmm"></a>Windows Admin Center 可以取代 System Center Virtual Machine Manager (SCVMM) 嗎？

Windows Admin Center 與 SCVMM 相輔相成；Windows Admin Center 主要用來取代傳統 Microsoft Management Console (MMC) 嵌入式管理單元和伺服器管理體驗。  Windows Admin Center 無意取代 SCVMM 監視層面的功能。 [深入了解 Windows Admin Center 和 System Center](related-management.md#system-center)。

## <a name="what-is-windows-admin-center-preview-which-version-is-right-for-me"></a>什麼是 Windows Admin Center 預覽版？哪個版本適合我用？

有兩個 Windows Admin Center 版本可供下載：

### <a name="windows-admin-center"></a>Windows Admin Center

* 如果您是 IT 系統管理員，但無法經常進行更新，或是需要較長時間驗證用於生產的版本時，此版本正合您需要。 我們目前已正式推出 (GA) 的版本是 Windows Admin Center 1910。
* [!INCLUDE [support-policy](../includes/support-policy.md)]
* 若要取得最新的版本，請[從這裡下載](https://aka.ms/WACDownload)。

### <a name="windows-admin-center-preview"></a>Windows Admin Center 預覽版

* 如果您是 IT 系統管理員，想要依一般發行頻率取得最新的絕佳功能時，此版本正合您需要。 我們的目的是每個月左右提供後續的更新版本。 核心平台仍繼續為生產就緒產品，而授權會提供生產使用權。 但請注意，您將會看到清楚標示為 PREVIEW 且適合評估及測試的新工具和功能陸續推出。
* 若要取得最新的 Insider Preview 版本，已註冊的測試人員可以直接從 [Windows Server Insider Preview 下載頁面](https://microsoft.com/en-us/software-download/windowsinsiderpreviewserver)的 [其他下載] 下拉式清單中下載 Windows Admin Center 預覽版。 如果您還沒有註冊為測試人員，請參閱商務用 Windows 測試人員入口網站上的[開始使用 Windows Server](https://insider.windows.com/en-us/for-business-getting-started-server/)。

## <a name="why-was-windows-admin-center-chosen-as-the-final-name-for-project-honolulu"></a>為什麼選擇「Windows Admin Center」做為「Project Honolulu」的最終名稱？

Windows Admin Center 是「Project Honolulu」的正式產品名稱，加強了我們在核心系統管理與管理案例廣泛範圍內對 IT 系統管理員整合式體驗的願景。 同時也昭示我們以客戶為導向、對 IT 系統管理員使用者需求的關注，視之為我們投資方式與提供項目的核心主軸。

## <a name="where-can-i-learn-more-about-windows-admin-center-or-get-more-details-on-the-topics-above"></a>何處可深入了解 Windows Admin Center，或取得更多有關上述主題的詳細資料？

我們的[啟動頁面](../overview.md)是最適合的起點，其中有連結連到我們新近分類的文件內容、下載位置、意見反應提供方式、參考資訊等資源。

## <a name="what-is-the-version-history-of-windows-admin-center"></a>Windows Admin Center 的版本記錄為何？

[在此檢視版本記錄。](../support/release-history.md)

## <a name="im-having-an-issue-with-windows-admin-center-where-can-i-get-help"></a>我的 Windows Admin Center 發生問題，我可以在哪裡取得協助？

請參閱我們的[疑難排解指南](../support/troubleshooting.md)和[已知問題](../support/known-issues.md)清單。