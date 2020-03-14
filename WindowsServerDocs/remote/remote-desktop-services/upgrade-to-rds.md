---
title: 將遠端桌面服務部署升級至 Windows Server 2016
description: 本文說明如何將現有的遠端桌面服務部署升級至 Windows Server 2016。
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 29648db89b61a9d22aad6d5aa814cfe7f425a970
ms.sourcegitcommit: 0a0a45bec6583162ba5e4b17979f0b5a0c179ab2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/13/2020
ms.locfileid: "79323360"
---
# <a name="upgrading-your-remote-desktop-services-deployments-to-windows-server-2016"></a>將遠端桌面服務部署升級至 Windows Server 2016

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016

## <a name="supported-os-upgrades-with-rds-role-installed"></a>已安裝 RDS 角色且受支援的作業系統升級
目前僅支援從 Windows Server 2012 R2 和 Windows Server 2016 升級至 Windows Server 2016。

## <a name="flow-for-deployment-upgrades"></a>部署升級的流程
為了盡可能縮短停機時間，最好遵循下列步驟：

1. 應該第一個升級 **RD 連線代理人伺服器**。 如果部署中有主動/主動設定，請從部署中移除所有但保留一部伺服器，並執行就地升級。 在其餘的 RD 連線代理人伺服器上離線執行升級，然後重新將它們新增至部署。 在 RD 連線代理人伺服器升級期間將無法使用部署。

   > [!NOTE] 
   > 必須升級 RD 連線代理人伺服器。 在含有 Windows Server 2016 伺服器的混合式部署中，不支援 Windows Server 2012 R2 RD 連線代理人伺服器。 一旦 RD 連線代理人伺服器正在執行 Windows Server 2016 之後，部署將會運作，即使部署中其餘的伺服器仍在執行 Windows Server 2012 R2 也一樣。

2. 應先升級 **RD 授權伺服器**，然後再升級您的 RD 工作階段主機伺服器。
   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 授權伺服器將會使用 Windows Server 2016 部署，但它們只能處理來自 Windows Server 2012 R2 和較舊版本的 CAL。 它們無法使用 Windows Server 2016 CAL。 如需 RD 授權伺服器的詳細資訊，請參閱[使用用戶端存取使用權 (CAL) 授權您的 RDS 部署](rds-client-access-license.md)。

3. 接著升級 **RD 工作階段主機伺服器**。 若要避免在升級期間產生停機時間，系統管理員可將伺服器分割成 2 個步驟來升級，詳情如下。 一切都將在升級之後正常運作。 若要升級，請使用[將遠端桌面工作階段主機伺服器升級到 Windows Server 2016](upgrade-to-rdsh.md) 中所述的步驟。

4. 接著升級 **RD 虛擬主機伺服器**。 若要升級，請使用[將遠端桌面虛擬主機伺服器升級到 Windows Server 2016](upgrade-to-rdvh.md) 中所述的步驟。

5. 隨時都可升級 **RD Web 存取伺服器**。
   > [!NOTE]
   > 升級 RD Web 可能會重設 IIS 屬性 (例如任何設定檔)。 如果不想遺失您的變更，請在 IIS 中的 RD Web 網站上完成自訂的附註或複本。

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD Web 存取伺服器將會使用 Windows Server 2016 部署。

6. 隨時都可升級 **RD 閘道伺服器**。
   > [!NOTE]
   > Windows Server 2016 不會包含網路存取保護 (NAP) 原則 - 必須先移除它們。 移除正確原則的最簡單方式是執行升級精靈。 如果有任何您必須刪除的 NAP 原則，升級將會在桌面上封鎖並建立含有特定原則的文字檔。 若要管理 NAP 原則，請開啟網路原則伺服器工具。 刪除它們之後，按一下安裝工具中的 [重新整理]  ，以繼續進行升級程序。 

   > [!NOTE] 
   > Windows Server 2012 和 2012 R2 RD 閘道伺服器將會使用 Windows Server 2016 部署。

## <a name="vdi-deployment--supported-guest-os-upgrade"></a>VDI 部署：支援的客體 OS 升級
系統管理員將使用下列選項來升級 VM 集合：

### <a name="upgrade-managed-shared-vm-collections"></a>升級受控的共用 VM 集合 
系統管理員將必須使用所需的 OS 版本來建立 VM 範本，並使用它來修補集區中的所有 VM。 

我們支援下列修補案例：
- Windows 7 SP1 可以修補成 Windows 8 或 Windows 8.1
- Windows 8 可以修補成 Windows 8.1
- Windows 8.1 可以修補成 Windows 10

### <a name="upgrade-unmanaged-shared-vm-collections"></a>升級受控的共用 VM 集合 
終端使用者無法升級他們的個人桌面。 系統管理員應該執行升級。 確切的步驟仍有待商榷。