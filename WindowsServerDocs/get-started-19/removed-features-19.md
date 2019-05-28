---
title: 已移除，或在 Windows Server 2019 中移除功能
description: 深入了解的特色和功能已移除或計劃移除開頭為 Windows Server 2019。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 05/21/2019
ms.localizationpriority: medium
ms.openlocfilehash: 820dfed8a0a58d3ccc64023325c373b761461ba8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/27/2019
ms.locfileid: "65976516"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>移除或取代啟動 Windows Server 2019 計劃的功能

>適用於：Windows Server 2019

每個版本的 Windows Server 都會加入新特色與功能；我們有時也會移除特性與功能，這通常是因為我們新增了更好的選項。 以下是有關的特色和功能，我們已移除在 Windows Server 2019 的詳細資料。

> [!TIP]
> - 您可以加入 [Windows 測試人員計畫](https://insider.windows.com)來優先存取 Windows Server 組建，這是測試功能變更的絕佳方式。
> - 對其他版本有任何問題嗎？ 簽出的資訊[Windows Server 中最新消息](../get-started/whats-new-in-windows-server.md)。

**清單會有所變更，且可能不會包含每個受影響的功能。** 

## <a name="features-we-removed-in-this-release"></a>我們在此版本中移除的功能

我們要從已安裝的產品映像，在 Windows Server 2019 移除下列特性與功能。 如果您沒有使用替代方法，依存於這些功能的應用程式或程式碼將無法在此版本中運作。

|功能    |您可以改用...|
|-----------|--------------------
|企業掃描，也稱為分散式掃描管理 (DSM)|我們要移除此安全掃描和掃描器的管理功能-沒有任何支援這項功能的裝置。|
|列印元件現在是選用的元件，用於 Server Core 安裝|在舊版的 Windows Server 中，已列印的元件*停用*預設會在 Server Core 安裝選項。 我們會變更，在 Windows Server 2016 中，依預設啟用它們。 在 Windows Server 2019，這些列印元件同樣預設為停用適用於 Server Core。 如果您需要啟用列印的元件，則可以藉由執行**Install-windowsfeature 列印伺服器**cmdlet。|
|Server Core 安裝中的[遠端桌面連線代理人和遠端桌面虛擬化主機](../remote/remote-desktop-services/desktop-hosting-service.md)|大部分遠端桌面服務部署都有這些與遠端桌面工作階段主機 (RDSH) 共置的角色，這需要含有桌面體驗的伺服器；為了與 RDSH 一致，我們正在將這些角色變更為同樣需要含有桌面體驗的伺服器。 這些 RDS 角色已不再適用於[Server Core 安裝](../administration/server-core/what-is-server-core.md)。 如果您需要[部署這些角色做為您的遠端桌面基礎結構的一部分](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，您可以[含有桌面體驗的 Windows 伺服器上加以安裝](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>這些角色也已包含在 Windows Server 2019 的桌面體驗安裝選項中。 |

## <a name="features-were-no-longer-developing"></a>我們不再開發的功能

我們不再主動開發這些功能，並可能移除它們未來的更新。 有些功能已取代為其他特色或功能，有些則暫時從不同的來源提供。 

如果您有建議取代任何這些功能的意見反應要分享，可以使用[意見反應中樞 App](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

| 功能   | 您可以改用... |
|-----------|---------------------|
| 在 HYPER-V 中的金鑰儲存體磁碟機|我們不會再正努力在 HYPER-V 中的金鑰儲存區的磁碟機功能。 如果您使用第 1 代 Vm，請參閱[層代 1 VM 虛擬化安全性](https://docs.microsoft.com/windows-server/virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v)如接下來的選項的相關資訊。 如果您要建立新的 Vm 會使用更安全的解決方案的 TPM 裝置的第 2 代虛擬機器。 |
| 受信任的平台模組 (TPM) 管理主控台|先前在 TPM 管理主控台中提供的資訊現在已提供下載[**裝置的安全性**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)頁面[Windows Defender 資訊安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)。 |
| 主機守護者服務的 Active Directory 證明模式|我們不會再開發主機守護者服務的 Active Directory 證明模式-相反地，我們已新增新的證明模式中，[裝載金鑰證明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，這是最簡單且同樣為相容，則為基礎的 Active Directory證明。  這個新模式提供的安裝體驗、 更簡易的管理與 Active Directory 證明比較少的基礎結構相依性的對等功能。 主機金鑰證明有超過所需，哪些 Active Directory 證明沒有其他硬體需求，因此所有現有的系統仍與新的模式相容。 請參閱[部署受防護主機](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)的證明選項的相關資訊。 |
| OneSync 服務|OneSync 服務同步處理的郵件、 行事曆和人員的應用程式的資料。 我們已加入 Outlook 應用程式提供相同的同步處理的同步處理引擎。 |
| 遠端差異壓縮 API 支援|遠端差異壓縮 API 支援啟用同步處理的資料與遠端來源，使用壓縮技術，透過網路傳送的資料量降到最低。 這項支援目前沒有使用任何 Microsoft 產品。 |
| WFP 輕量型篩選器的交換器擴充功能|WFP 輕量型篩選器的交換器擴充功能可讓開發人員建置[簡單的網路封包篩選適用於 HYPER-V 虛擬交換器擴充功能](https://docs.microsoft.com/en-us/windows-hardware/drivers/network/using-virtual-switch-filtering)。 您可以建立完整的篩選擴充功能來達到相同的功能。 因此，我們將移除此擴充功能在未來。 |