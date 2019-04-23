---
title: 升級您的遠端桌面服務部署至 Windows Server 2016
description: 本文說明如何升級現有的遠端桌面服務部署至 Windows Server 2016。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 03/20/2018
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7b1f1f6-57c8-40ab-a235-e36240dcc1f8
author: spatnaik
manager: scottman
notes: https://social.technet.microsoft.com/wiki/contents/articles/22069.remote-desktop-services-upgrade-guidelines-for-windows-server-2012-r2.aspx
ms.openlocfilehash: f683a7d9346494e7f1fb6faf716ca9c90cfef8d3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875749"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>升級您的遠端桌面服務部署至 Windows Server 2016

>適用於：Windows Server （半年通道），Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色支援 OS 升級
只能從 Windows Server 2012 R2 和 Windows Server 2016 支援至 Windows Server 2016 的升級。

## <a name="flow-for-deployment-upgrades"></a>部署升級的流程
為了讓停機時間保持在最小值，最好是遵循下列步驟：

1. **RD 連線代理人伺服器**應該要升級的第一個。 如果沒有主動/主動設定，在部署中，從部署移除所有保留一個伺服器，而且執行就地升級。 執行升級其餘 RD 連線代理人伺服器離線，再重新新增至部署。 部署 RD 連線代理人伺服器升級期間將無法使用。

   > [!NOTE] 
   > 它，才能將 RD 連線代理人伺服器升級。 我們在混合式部署中與 Windows Server 2016 的伺服器不支援 Windows Server 2012 R2 RD 連線代理人伺服器。 一旦 RD 連線代理人伺服器都在執行 Windows Server 2016 部署會正常運作，即使在部署伺服器的其餘部分仍在執行 Windows Server 2012 R2。

2. **RD 授權伺服器**應進行升級，然後再升級您的 RD 工作階段主機伺服器。
   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 授權伺服器會使用 Windows Server 2016 的部署，但它們只能處理從 Windows Server 2012 R2 和較舊的 Cal。 它們不能使用 Windows Server 2016 Cal。 請參閱[與用戶端存取使用權 (Cal) 授權您的 RDS 部署](rds-client-access-license.md)如需有關 RD 授權伺服器。

3. **RD 工作階段主機伺服器**可以接著升級。 若要避免停機時間，在升級期間系統管理員可以分割如下所述的 2 個步驟中要升級的伺服器。 所有功能之後會升級。 若要升級，請使用 中所述的步驟[到 Windows Server 2016 的升級遠端桌面工作階段主機伺服器](upgrade-to-rdsh.md)。

4. **RD 虛擬主機伺服器**可以接著升級。 若要升級，請使用 中所述的步驟[到 Windows Server 2016 的升級遠端桌面虛擬化主機伺服器](upgrade-to-rdvh.md)。

5. **RD Web 存取伺服器**可以隨時升級。
   > [!NOTE]
   > 升級 RD Web，可能會重設 IIS 屬性 （例如任何組態檔）。 若要不會遺失您的變更，請附註或為了在 IIS 中的 RD Web 站台的自訂項目副本。

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD Web 存取伺服器會使用 Windows Server 2016 部署。

6. **RD 閘道伺服器**可以隨時升級。
   > [!NOTE]
   > Windows Server 2016 不包含網路存取保護 (NAP) 原則-這些使用者必須先移除。 若要移除的正確原則的最簡單方式是執行升級精靈。 您必須先刪除任何 NAP 原則時，升級就會封鎖，並建立含有特定的原則在桌面上的文字檔。 若要管理 NAP 原則，開啟 網路原則伺服器工具。 在刪除它們之後, 按一下**重新整理**安裝工具，以繼續進行升級程序中。 

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 閘道伺服器會使用 Windows Server 2016 部署。

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>VDI 部署 – 支援的客體 OS 升級
系統管理員將擁有下列選項來升級的 VM 集合：

### <a name="upgrade-managed-shared-vm-collections"></a>升級管理共用的 VM 集合 
系統管理員將必須使用所需的 OS 版本建立 VM 範本，並用來修補程式集區中的所有 Vm。 

我們支援下列的修復案例：
- Windows 7 SP1 修補 Windows 8 或 Windows 8.1
- Windows 8 可以修補至 Windows 8.1
- Windows 10 已修補 Windows 8.1

### <a name="upgrade-unmanaged-shared-vm-collections"></a>升級共用的 VM 未受管理的集合 
使用者無法升級他們的個人桌面。 系統管理員應該執行升級。 確切步驟仍是判斷。