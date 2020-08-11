---
title: 移轉角色和功能
description: 有關於如何將角色和功能移轉至較新版 Windows Server 的資訊。
ms.date: 08/28/2019
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 029f3c2b2857dba88af0e5c7b584e257c579d063
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959576"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>在 Windows Server 中移轉角色和功能

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

此頁面包含資訊和工具的連結，有助於引導您完成將角色和功能移轉至較新版 Windows Server 的程序。 您可以使用[儲存體移轉服務](../storage/storage-migration-service/overview.md)來移轉檔案伺服器和儲存體，而許多其他角色和功能則可使用 Windows Server 移轉工具來移轉；此工具是 Windows Server 2008 R2 中導入的一組 PowerShell Cmdlet，可用來移轉角色和功能。

移轉指南支援將指定的角色和功能從一部伺服器移轉至另一部 (而不是就地升級)。 除非指引中另有說明，否則所支援移轉乃介於實體與虛擬電腦之間，和在 Windows Server 完整安裝選項與執行 Server Core 安裝選項的伺服器之間。

## <a name="before-you-begin"></a>在您開始前

在您開始移轉角色和功能之前，請確認來源和目的地伺服器是否執行適用於其作業系統的最新 Service Pack。

> [!NOTE]
> 每當您移轉或升級到任何 Windows Server 版本時，都應該檢閱和了解該版本的[支援週期原則](https://support.microsoft.com/lifecycle)與時間範圍，並進行適當規劃。 您可以針對感興趣的特定 Windows Server 版本[搜尋週期資訊](https://support.microsoft.com/lifecycle)。

## <a name="windows-server-2019"></a>Windows Server 2019

若要將檔案伺服器和儲存體移轉至 Windows Server 2019 或 Windows Server 2016，建議使用[儲存體移轉服務](../storage/storage-migration-service/overview.md)。 若要移轉其他角色，請參閱 Windows Server 2016 和 Windows Server 2012 R2 的指引。

## <a name="windows-server-2016"></a>Windows Server 2016

以下是 Windows Server 2016 的移轉指南。 請注意，在許多情況下，您也可以使用 Windows Server 2012 R2 移轉指南。

- [遠端桌面服務](../remote/remote-desktop-services/migrate-rds-role-services.md)
- [網頁伺服器 (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [MultiPoint 服務](../remote/multipoint-services/multipoint-services-migrate.md)

若要將檔案伺服器移轉至 Windows Server 2019 或 Windows Server 2016，建議使用[儲存體移轉服務](../storage/storage-migration-service/overview.md)。

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

遵循下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2、Windows Server 2012 或 Windows Server 2012 R2 的伺服器移轉到 Windows Server 2012 R2。 Windows Server 2012 R2 的 Windows Server 移轉工具支援跨子網路移轉。

- [安裝、使用以及移除 Windows Server 移轉工具](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [Windows Server 2012 R2 的 Active Directory 憑證服務移轉指南](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486797(v=ws.11))
- [將 Active Directory Federation Services 角色服務移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486815(v=ws.11))
- [Active Directory Rights Management Services 移轉和升級指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754277(v=ws.10))
- [將檔案和存放服務移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479292(v=ws.11))
- [將 Hyper-V 從 Windows Server 2012 移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486799(v=ws.11))
- [將網路原則伺服器移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [將遠端桌面服務移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn479239(v=ws.11))
- [將 Windows Server Update Services 移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [將叢集角色移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn530779(v=ws.11))
- [將 DHCP 伺服器移轉到 Windows Server 2012 R2](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn495425(v=ws.11))

目前已有 Windows Server 2012 R2 與 Windows Server 2012 移轉指南電子書可供閱讀。 如需詳細資訊並下載電子書，請參閱 [Microsoft 技術電子書庫](https://download.microsoft.com/download/8/D/3/8D33661A-7E21-4FEE-9AAA-C17C3004B5AA/Windows-Migration-and-Upgrade-Guide.pdf)。

## <a name="windows-server-2012"></a>Windows Server 2012

遵循下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008、Windows Server 2008 R2 或 Windows Server 2012 的伺服器移轉到 Windows Server 2012。 Windows Server 2012 的 Windows Server 移轉工具支援跨子網路移轉。

- [安裝、使用以及移除 Windows Server 移轉工具](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134202(v=ws.11))
- [將 Active Directory 同盟服務角色服務移轉到 Windows Server 2012](../identity/ad-fs/deployment/migrate-ad-fs-role-services-to-windows-server-2012.md)
- [將健康情況登錄授權單位移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831513(v=ws.11))
- [將 Hyper-V 從 Windows Server 2008 R2 移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574113(v=ws.11))
- [將 IP 設定移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574133(v=ws.11))
- [將網路原則伺服器移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831652(v=ws.11))
- [將列印和文件服務移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134150(v=ws.11))
- [將遠端存取移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831423(v=ws.11))
- [將 Windows Server Update Services 移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh852339(v=ws.11))
- [將 Active Directory 網域控制站升級到 Windows Server 2012](../identity/ad-ds/deploy/upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012.md)
- [將叢集服務和應用程式移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486790(v=ws.11))


如需其他移轉資源，請瀏覽[將角色和功能移轉到 Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134039(v=ws.11))。

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

遵循下列指南中的步驟，將角色和功能從執行 Windows Server 2003、Windows Server 2008 或 Windows Server 2008 R2 的伺服器移轉到 Windows Server 2008 R2。 Windows Server 2008 R2 的 Windows Server 移轉工具不支援跨子網路移轉。

- [Windows Server 移轉工具安裝、存取和移除](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379545(v=ws.10))
- [Active Directory 憑證服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee126170(v=ws.10))
- [Active Directory Domain Services 和網域名稱系統 (DNS) 伺服器移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379558(v=ws.10))
- [BranchCache 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd548365(v=ws.10))
- [動態主機設定通訊協定 (DHCP) 伺服器移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379535(v=ws.10))
- [檔案服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379487(v=ws.10))
- [HRA 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791829(v=ws.10))
- [Hyper-V 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee849855(v=ws.10))
- [IP 設定移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379537(v=ws.10))
- [本機使用者和群組移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379531(v=ws.10))
- [NPS 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee791849(v=ws.10))
- [列印服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd379488(v=ws.10))
- [遠端桌面服務移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff849223(v=ws.10))
- [RRAS 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822825(v=ws.10))
- [Windows Server 移轉一般工作和資訊](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff400258(v=ws.10))
- [Windows Server Update Services 3.0 SP2 移轉指南](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee822826(v=ws.10))

如需其他移轉資源，請瀏覽 [Migrate Roles and Features to Windows Server 2008 R2](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd365353(v=ws.10)) (將角色和功能移轉到 Windows Server 2008 R2)。
