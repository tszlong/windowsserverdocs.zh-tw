---
title: 不在 Windows Server Core 中的角色、角色服務和功能
description: 瞭解 Windows Server 的 Server Core 安裝選項中未包含的角色和功能。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: ce5107e8e0ab573df7588428db65c8b223cf1f13
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476519"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>不在 Windows Server Core 中的角色、角色服務和功能

> 適用於：Windows Server 2019、Windows Server 2016 和 Windows Server (半年通道)

已從 Windows Server 的 Server Core 安裝選項中移除下列角色、角色服務和功能。 使用這項資訊有助於找出伺服器核心選項是否適用于您的環境。

> [!NOTE]
> 您也可以查看[Server Core 中包含](server-core-roles-and-services.md)的角色、角色服務和功能的清單。 這是非常大的清單, 因此為了得到最佳結果, 請在清單中搜尋您感興趣的特定角色或功能。

## <a name="roles-not-in-server-core"></a>不在 Server Core 中的角色

- 傳真
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>不在 Server Core 中的角色服務
請注意, 某些遠端桌面角色服務已包含在 Server Core (連線代理人、授權、虛擬化主機) 中, 但其他則不是 (閘道、RD 工作階段主機、Web 存取)。

- 列印-掃描-伺服器
- 列印-網際網路
- RDS-閘道
- RDS-RD-伺服器
- RDS-Web 存取
- Web 管理-主控台
- Web-Lgcy-管理-主控台
- WDS-部署
- WDS-傳輸 *(在 Windows Server 1803 版之前)*

## <a name="features-not-in-server-core"></a>不在 Server Core 中的功能

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- 直接播放
- 網際網路-列印-用戶端
- LPR-埠-監視
- MSMQ-多播
- CMAK
- 遠端協助
- RSAT-SMTP
- RSAT-功能工具-BitLocker-RemoteAdminTool
- RSAT-Bits-伺服器
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-v-工具
- RSAT-RDS-工具
- RSAT-RDS-閘道
- RSAT-RDS-授權-診斷-UI
- RDS-授權-UI
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-管理
- RSAT-線上-回應者
- RSAT-ADRMS
- RSAT-Fax
- RSAT-檔案-服務
- RSAT-DFS 管理-Con
- RSAT-FSRM-管理
- RSAT-NFS-系統管理員
- RSAT-NPAS
- RSAT-列印-服務
- RSAT-VA-工具
- WDS-AdminPack
- SMTP-伺服器
- TFTP-用戶端
- WebDAV-重新導向程式
- 生物識別架構
- Windows-Defender-Gui
- Windows-身分識別-基礎
- PowerShell-ISE
- 搜尋-服務
- Windows-TIFF-IFilter
- 無線網路
- XPS-檢視器

