---
title: 已移除或計劃移除 Windows Server 2019 中的功能
description: 深入了解之特色與功能已移除或計劃從 Windows Server 2019 開始移除。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: jasgro
ms.date: 03/29/2019
ms.localizationpriority: medium
ms.openlocfilehash: 470857616a9b36d238de031b4ccf80a68eff1e61
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279129"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>已移除或計劃要從 Windows Server 2019 開始取代的功能

>適用於︰Windows Server 2019

每個版本的 Windows Server 都會加入新特色與功能；我們有時也會移除特性與功能，這通常是因為我們新增了更好的選項。 以下是特色和功能，我們已移除 Windows Server 2019 中的相關詳細資料。   

> [!TIP]
> - 您可以加入 [Windows 測試人員計畫](https://insider.windows.com)來優先存取 Windows Server 組建，這是測試功能變更的絕佳方式。
> - 對其他版本有任何問題嗎？ 請查看適用於[Windows Server 版本 1803年](../get-started/windows-server-1803-removed-features.md) [Windows Server （版本 1709）](../get-started/removed-features-1709.md)，及[Windows Server 2016](../get-started/deprecated-features.md)的資訊。

**清單隨時會變更，可能無法完全列出各項受影響的特色與功能。** 

## <a name="features-we-removed-in-this-release"></a>我們在此版本中移除的功能

我們正在從 Windows Server 2019 中已安裝的產品映像移除下列特色及功能。 如果您沒有使用替代方法，依存於這些功能的應用程式或程式碼將無法在此版本中運作。   

|功能    |您可以改用...|
|-----------|--------------------
|企業掃描，也稱為分散式掃描管理 (DSM)|我們正在移除此安全掃描及掃描器管理功能-沒有支援此功能的裝置。|
|列印元件-現在選用元件的 Server Core 安裝|列印元件先前版本的 Windows Server 中，還是在 Server Core 安裝選項中，預設的 [*停用*。 我們會變更，在 Windows Server 2016 的預設啟用它們。 在 Windows Server 2019 中，這些列印元件再次預設都會停用適用於 Server Core。 如果您需要啟用列印的元件，您可以執行**安裝 Add-windowsfeature 列印伺服器**cmdlet 來執行此動作。|
|Server Core 安裝中的[遠端桌面連線代理人和遠端桌面虛擬化主機](../remote/remote-desktop-services/desktop-hosting-service.md)|大部分遠端桌面服務部署都有這些與遠端桌面工作階段主機 (RDSH) 共置的角色，這需要含有桌面體驗的伺服器；為了與 RDSH 一致，我們正在將這些角色變更為同樣需要含有桌面體驗的伺服器。 這些 RDS 角色已不再是[Server Core 安裝](../administration/server-core/what-is-server-core.md)中可供使用。 如果您要[部署這些角色做為您的遠端桌面基礎結構的一部分](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，您可以[在 [含有桌面體驗的 Windows Server 安裝它們](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>這些角色也已包含在 Windows Server 2019 的桌面體驗安裝選項中。 |



## <a name="features-were-no-longer-developing"></a>我們不再開發的功能

我們正在不再主動開發這些功能，並可能從未來的更新移除。 有些功能已取代為其他特色或功能，有些則暫時從不同的來源提供。 

如果您有建議取代任何這些功能的意見反應要分享，可以使用[意見反應中樞 App](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

|功能    |您可以改用...|
|-----------|---------------------|
|HYPER-V 中的金鑰儲存磁碟機|我們不再正在進行的 HYPER-V 中的金鑰存放區的磁碟機功能。 如果您使用第 1 代 Vm，請查看[世代 1 VM 虛擬化安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)選項往後的相關資訊。 如果您正在建立新的虛擬機器會使用第 2 代虛擬機器搭配 TPM 的裝置，以更安全的解決方案。 |
|信賴平台模組 (TPM) 管理主控台|先前在 TPM 管理主控台中可用的資訊現在已可在[Windows Defender 資訊安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)中的 [[**裝置安全性**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)] 頁面的。|
|主機守護者服務 Active Directory 證明模式|我們不再開發主機守護者服務 Active Directory 證明模式-改為我們已經新增新的證明模式，[主機的金鑰證明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，最簡單且平均相容為 Active Directory 型證明。  這個新的模式提供安裝體驗、 較簡單的管理與 Active Directory 證明比較少的基礎結構相依性的對等功能。 主機金鑰證明有必要，哪些 Active Directory 證明超出沒有其他硬體需求，因此所有現有的系統將會維持與新的模式相容。 如需您證明選項的詳細資訊，請參閱[部署受防護主機](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)。|
|OneSync 服務|OneSync 服務會同步處理電子郵件、 行事曆和連絡人的應用程式的資料。 我們已經新增同步引擎提供相同的同步處理的 Outlook app。|
|遠端差異壓縮 API 支援|遠端差異壓縮 API 支援啟用遠端來源使用壓縮技術，最小化的跨網路傳送的資料量資料同步處理。 這項支援目前未使用的任何 Microsoft 產品中。|
|WFP 輕量型篩選交換器延伸模組|WFP 輕量型篩選交換器延伸模組可讓開發人員建立[HYPER-V 虛擬的簡單網路封包篩選擴充功能切換](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering)。 您可以藉由建立完整篩選擴充功能來達到相同的功能。 因此，我們將移除此延伸模組在未來。|

