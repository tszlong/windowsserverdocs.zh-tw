---
title: Windows Server 2019 中已移除或已計劃移除的功能
description: 深入了解從 Windows Server 2019 起開始移除或計畫移除的特色與功能。
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: jasongerend
ms.author: jgerend
manager: jasgro
ms.date: 08/22/2019
ms.localizationpriority: medium
ms.openlocfilehash: 261023223b56e235320b7ad414fa91206420b1ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360845"
---
# <a name="features-removed-or-planned-for-replacement-starting-windows-server-2019"></a>從 Windows Server 2019 開始移除或計劃取代的功能

>適用於：Windows Server Standard 2012 R2

每個版本的 Windows Server 都會加入新特色與功能；我們有時也會移除特性與功能，這通常是因為我們新增了更好的選項。 以下是有關我們在 Windows Server 2019 中已移除的特色與功能詳細資訊。

> [!TIP]
> - 您可以加入 [Windows 測試人員計畫](https://insider.windows.com)來優先存取 Windows Server 組建，這是測試功能變更的絕佳方式。
> - 對其他版本有任何問題嗎？ 查看 [Windows Server 中已移除或計劃取代的功能](removed-features.md)。

**清單隨時會變更，可能無法完全列出各項受影響的特色與功能。** 

## <a name="features-we-removed-in-this-release"></a>我們在此版本中移除的功能

我們會從 Windows Server 2019 已安裝的產品映像中移除下列特性與功能。 如果您沒有使用替代方法，依存於這些功能的應用程式或程式碼將無法在此版本中運作。

| 功能   | 您可以改用... |
| --------- | -------------------- |
| 企業掃描，也稱為分散式掃描管理 (DSM)|我們會移除此安全掃描和掃描器的管理功能 - 沒有任何支援此功能的裝置。 |
| 列印元件 - 現在是核心安裝的選用元件|在舊版 Windows Server 中，列印元件在核心安裝選項中預設為「停用」  。 我們在 Windows Server 2016 中變更了此設定，改成預設為啟用。 在 Windows Server 2019 中，這些列印元件再次針對 Server Core 預設為停用。 如果您需要啟用列印元件，可以藉由執行 **Install-WindowsFeature Print-Server** Cmdlet 來完成。 |
| 核心安裝中的[遠端桌面連線代理人和遠端桌面虛擬化主機](../remote/remote-desktop-services/desktop-hosting-service.md)|大部分遠端桌面服務部署都有這些與遠端桌面工作階段主機 (RDSH) 共置的角色，這需要含有桌面體驗的伺服器；為了與 RDSH 一致，我們正在將這些角色變更為同樣需要含有桌面體驗的伺服器。 [核心安裝](../administration/server-core/what-is-server-core.md)中將不再提供 RDS 角色。 如果您需要[將這些角色部署成遠端桌面基礎結構的一部分](../remote/remote-desktop-services/rds-deploy-infrastructure.md)，您可以[在含有桌面體驗的 Windows Server 上安裝這些角色](../get-started/getting-started-with-server-with-desktop-experience.md)。 <br/><br/>這些角色也已包含在 Windows Server 2019 的桌面體驗安裝選項中。 |

## <a name="features-were-no-longer-developing"></a>我們不再開發的功能

我們不再主動開發這些功能，而且可能在未來更新中加以移除。 有些功能已取代為其他特色或功能，有些則暫時從不同的來源提供。 

如果您有建議取代任何這些功能的意見反應要分享，可以使用[意見反應中樞 App](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)。 

| 功能     | 您可以改用... |
| ----------- | --------------------- |
| Hyper-V 中的金鑰存放磁碟機|Hyper-V 中將不再使用金鑰存放磁碟機功能。 如果您使用第 1 代 VM，請參閱[第 1 代 VM 虛擬化安全性](../virtualization/hyper-v/learn-more/generation-1-virtual-machine-security-settings-for-hyper-v.md)，以了解之後的相關資訊。 如果您要建立新的 VM，請使用搭配 TPM 裝置的第 2 代虛擬機器，以取得更安全的解決方案。 |
| 信賴平台模組 (TPM) 管理主控台|先前在 TPM 管理主控台中提供的資訊，現在也可以在 [Windows Defender 資訊安全中心](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/windows-defender-security-center)的[**裝置安全性**](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-security-center/wdsc-device-security)頁面取得。 |
| 主機守護者服務的 Active Directory 證明模式|我們已不再開發主機守護者服務的 Active Directory 證明模式 - 相反地，我們已新增新的證明模式：[主機金鑰證明](../security/guarded-fabric-shielded-vm/guarded-fabric-create-host-key.md)，這是最簡單且同樣與 Active Directory 相容的證明。  此新模式提供具有安裝體驗的同等功能，比起 Active Directory 證明，其管理方式更簡單，基礎結構相依性也較少。 主機金鑰證明的硬體需求皆沒有超過 Active Directory 證明所要求的內容，因此所有現有系統仍可與新的模式相容。 請參閱[部署受防護主機](../security/guarded-fabric-shielded-vm/guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)，取得有關證明選項的相關資訊。 |
| OneSync 服務 | OneSync 服務可同步郵件、行事曆和人員應用程式的資料。 我們已將同步引擎新增至 Outlook 應用程式以提供相同的同步功能。 |
| 遠端差異壓縮 API 支援 | 遠端差異壓縮 API 支援可使用壓縮技術 (將網路間傳送的資料最小化) 來同步資料與遠端來源。 |
| WFP 輕量型篩選器的交換器擴充功能 | WFP 輕量型篩選器的交換器擴充功能可讓開發人員[針對 Hyper-V 虛擬交換器建置簡單的網路封包篩選擴充功能](https://docs.microsoft.com/windows-hardware/drivers/network/using-virtual-switch-filtering)。 您可以建立完整的篩選擴充功能來達到相同功能性。 因此，我們將在未來移除此擴充功能。 |
