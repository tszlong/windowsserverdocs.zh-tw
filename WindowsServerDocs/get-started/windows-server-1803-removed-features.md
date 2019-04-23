---
title: Windows Server 版本 1803 - 已移除的功能
description: 了解將會在 Windows Server 版本 1803 或未來版本中移除或取代的功能
ms.prod: windows-server-threshold
ms.mktglfcycl: plan
ms.localizationpriority: medium
ms.sitesec: library
author: lizap
ms.author: elizapo
ms.date: 05/10/2018
ms.openlocfilehash: c80738fe7ceda43a1a73adb0a8b1061bbb24319f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886199"
---
# <a name="features-removed-or-planned-for-replacement-starting-with-windows-server-version-1803"></a>從 Windows Server 版本 1803 開始移除或計劃取代的功能

> 適用於：Windows Server 版本 1803

每個版本的 Windows Server 都會加入新特色與功能；我們有時也會移除特性與功能，這通常是因為我們新增了更好的選項。 以下是有關我們在 Windows Server 版本 1803 中已移除之特色與功能的詳細資訊。   

> [!TIP]
> - 您可以加入 [Windows 測試人員計畫](https://insider.windows.com)來優先存取 Windows Server 組建，這是測試功能變更的絕佳方式。
> - 對其他版本有任何問題嗎？ 查看 [Windows Server 2016](deprecated-features.md) 和 [Windows Server 版本 1709](removed-features-1709.md) 的相關資訊。

**清單會有所變更，且可能不會包含每個受影響的功能。** 

## <a name="features-we-removed-in-this-release"></a>我們在此版本中移除的功能

我們從 Windows Server 版本 1803 已安裝的產品映像中移除了下列特性與功能。 如果您沒有使用替代方法，依存於這些功能的應用程式或程式碼將無法在此版本中運作。   

|功能    |您可以改用...|
|-----------|--------------------|
|[檔案複寫服務](https://support.microsoft.com/en-us/help/4025991/windows-server-version-1709-no-longer-supports-frs)|最早出現在 Windows Server 2003 R2 中的檔案複寫服務 (FRS) 已由 DFS 複寫取代。 您需要[透過 SYSVOL 將任何使用 FRS 的網域控制站都移轉至 DFS 複寫](https://blogs.technet.microsoft.com/filecab/2014/06/25/streamlined-migration-of-frs-to-dfsr-sysvol/)。|
|Hyper-V 網路虛擬化 (HNV)|[網路虛擬化](../networking/sdn/technologies/hyper-v-network-virtualization/whats-new-hyperv-network-virtualization-windows-server.md)現已包含在 Windows Server 中做為[軟體定義網路](../networking/sdn/software-defined-networking.md) (SDN) 解決方案的一部分，這也包含網路控制卡、軟體負載平衡、使用者定義路由和存取控制清單。|

## <a name="features-were-no-longer-developing"></a>我們不再開發的功能

我們不再主動開發這些功能，而且可能在未來更新中加以移除。 有些功能已取代為其他特色或功能，有些則暫時從不同的來源提供。 

>[!NOTE]
> 請注意，以下所述的一些功能和角色未包含在 Windows Server 版本 1803 所提供的 Server Core 安裝選項中。 這些功能和角色*實際是*在 [含有桌面體驗的伺服器] 安裝選項中提供，上次已隨同 Windows Server 2016 發行，而在 Windows Server 2019 中，也將再次發行。

如果您有建議取代任何這些功能的意見反應要分享，可以使用[意見反應中樞 App](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

|功能或角色    |您可以改用...|
|-----------|---------------------|
|企業掃描，也稱為分散式掃描管理 (DSM)|[掃描管理功能](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759124\(v%3dws.11\))最初是在 Windows Server 2008 R2 中導入，可在企業中啟用掃描器的安全掃描及管理。 我們不再投資開發這項功能，也沒有支援的裝置可用。|
|IPv4/6 轉換技術 (6to4、ISATAP 和直接通道)|6to4 從 Windows 10 版本 1607 (年度更新版) 之後預設停用；ISATAP 從 Windows 10 版本 1703 (Creators Update) 之後預設停用；而直接通道 (Direct Tunnels) 一直都是預設停用。 請改用原生 IPv6 支援。|
|[MultiPoint 服務](../remote/multipoint-services/multipoint-services.md)|我們不再開發做為 Windows Server 一部分的 MultiPoint 服務角色。 Windows Server 和 Windows 10 都可以透過[隨選功能](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)使用 MultiPoint 連接器服務。 您可以使用[遠端桌面服務](../remote/remote-desktop-services/welcome-to-rds.md) (特別是遠端桌面服務工作階段主機) 來提供 RDP 連線。 |
|[離線符號套件](https://docs.microsoft.com/windows-hardware/drivers/debugger/debugger-download-symbols) (偵錯符號 MSI)|我們不再以可下載 MSI 的形式提供符號套件。 而是 [Microsoft 符號伺服器即將移轉成 Azure 型符號存放區](https://blogs.msdn.microsoft.com/windbg/2017/10/18/update-on-microsofts-symbol-server/)。 如果您需要 Windows 符號，請連線至 Microsoft 符號伺服器以在本機快取您的符號，或是將資訊清單檔案搭配可存取網際網路之電腦上的 SymChk.exe 使用。|
|Server Core 安裝中的[遠端桌面連線代理人和遠端桌面虛擬化主機](../remote/remote-desktop-services/desktop-hosting-service.md)|大部分遠端桌面服務部署都有這些與遠端桌面工作階段主機 (RDSH) 共置的角色，這需要含有桌面體驗的伺服器；為了與 RDSH 一致，我們正在將這些角色變更為同樣需要含有桌面體驗的伺服器。 我們不再開發這些用於 [Server Core 安裝](../administration/server-core/what-is-server-core.md)的 RDS 角色。 如果您需要[將這些角色部署成遠端桌面基礎結構的一部分](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，您可以[在含有桌面體驗的 Windows Server 2016 上安裝這些角色](getting-started-with-server-with-desktop-experience.md)。 <br/><br/>這些角色也已包含在 Windows Server 2019 的桌面體驗安裝選項中。 您可以在 [Windows Server 2019 的 Windows 測試人員組建](https://docs.microsoft.com/windows-insider/at-work/)中進行測試，只是務必要選擇 LTSC 映像。 |
|[RemoteFX vGPU](../remote/remote-desktop-services/rds-remotefx-vgpu.md)|我們正在針對虛擬化環境開發新的圖形加速選項。 您也可以使用[不同的裝置指派 (DDA)](../virtualization/hyper-v/plan/plan-for-deploying-devices-using-discrete-device-assignment.md) 做為替代方法。|
|群組原則中的[軟體限制原則](../identity/software-restriction-policies/software-restriction-policies.md)|不要透過群組原則使用軟體限制原則，您可以使用 [AppLocker](https://docs.microsoft.com/windows/security/threat-protection/applocker/applocker-overview) 或 [Windows Defender 裝置控制](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control)來控制哪些應用程式使用者可以存取，以及什麼程式碼可以在核心中執行。|
|使用 SAS 結構之共用設定的儲存空間|部署[儲存空間直接存取](../storage/storage-spaces/storage-spaces-direct-overview.md)。 「儲存空格直接存取」支援使用 HLK 認證 SAS 機箱，但是要在非共用設定下使用，如[儲存空格直接存取硬體需求](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md)中所述的。|
|Windows Server Essentials 體驗|我們不再針對 Windows Server Standard 或 Windows Server Datacenter SKU 開發 Essentials 體驗角色。 如果您需要適用於中小型企業的簡單易用伺服器解決方案，請查看我們新的 [Microsoft 365 商務版](https://www.microsoft.com/microsoft-365/business)解決方案，或使用 [Windows Server 2016 Essentials](https://docs.microsoft.com/windows-server-essentials/get-started/get-started)。|

