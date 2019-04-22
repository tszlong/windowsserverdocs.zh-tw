---
title: 角色、 角色服務和功能不在 Windows Server 的 Server Core
description: 深入了解角色和功能不包含在 Windows Server 的 Server Core 安裝選項。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825529"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>角色、 角色服務和功能不在 Windows Server 的 Server Core

> 適用於：Windows Server （半年通道） 和 Windows Server 2016

下列角色、 角色服務與功能已從 Windows Server 的 Server Core 安裝選項。 您可以使用此資訊來協助找出如果 Server Core 選項適用於您的環境。

> [!NOTE]
> 您也可以看到一份角色、 角色服務和功能[會包含在 Server Core](server-core-roles-and-services.md)。 它是非常大的清單，因此為了獲得最佳結果，清單中搜尋該特定角色或您感興趣的功能。

## <a name="roles-not-in-server-core"></a>不在 Server Core 中的角色

- 傳真
- MultiPointServerRole
- NPAS
- WDS

## <a name="role-services-not-in-server-core"></a>不在 Server Core 中的角色服務
請注意，某些遠端桌面角色服務包括在 Server Core （連線代理人、 授權、 虛擬化主機），但其他則否 （閘道，RD 工作階段主機，Web 存取）。

- 列印掃描伺服器
- 列印-網際網路
- RDS-Gateway
- RDS-RD-Server
- RDS-Web-Access
- Web-Mgmt-Console
- Web-Lgcy-Mgmt-Console
- WDS-Deployment
- WDS Transport *（之前 Windows Server 1803 版)*

## <a name="features-not-in-server-core"></a>不在 Server Core 中的功能

- BITS-IIS-Ext
- BitLocker-NetworkUnlock
- Direct Play
- Internet-Print-Client
- LPR-Port-Monitor
- MSMQ-Multicasting
- CMAK
- 遠端協助
- RSAT-SMTP
- RSAT-Feature-Tools-BitLocker-RemoteAdminTool
- RSAT-Bits-Server
- RSAT-NLB
- RSAT-SNMP
- RSAT-WINS
- Hyper-V-Tools
- RSAT-RDS-Tools
- RSAT-RDS-Gateway
- RSAT-RDS-Licensing-Diagnosis-UI
- RDS-Licensing-UI
- UpdateServices-UI
- RSAT-ADCS
- RSAT-ADCS-Mgmt
- RSAT-Online-Responder
- RSAT-ADRMS
- RSAT-Fax
- RSAT-File-Services
- RSAT-DFS-Mgmt-Con
- RSAT-FSRM-Mgmt
- RSAT-NFS-Admin
- RSAT-NPAS
- RSAT-Print-Services
- RSAT-VA-Tools
- WDS-AdminPack
- SMTP-Server
- TFTP-Client
- WebDAV-Redirector
- Biometric-Framework
- Windows-Defender-Gui
- Windows-Identity-Foundation
- PowerShell-ISE
- 搜尋服務
- Windows-TIFF-IFilter
- 無線網路
- XPS-Viewer

