---
title: Windows 10 支援遠端桌面服務 VDI 的安全性設定
description: 提供在 Windows Server 2016 中支援使用 RDS 進行哪些 Windows 10 VDI 設定的相關資訊。
ms.author: elizapo
ms.date: 10/27/2016
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: 7fd8de56d02dfe83add67b740405265a232747d9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87946341"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>Windows 10 支援遠端桌面服務 VDI 的安全性設定

> 適用於：Windows Server 2016

Windows 10 和 Windows Server 2016 具有內建於作業系統的新式保護層級，可進一步對抗安全性漏洞、協助封鎖惡意攻擊，並增強虛擬機器、應用程式與資料的安全性。

> [!NOTE]
> 請務必檢閱[支援遠端桌面服務的設定資訊](rds-supported-config.md)。

下表列出在使用 RDS 的 VDI 部署中支援哪些新功能。

|  VDI 集合類型               |  受管理的集區 |  受管理的個人 |  未受管理的集區                                     |  未受管理的個人                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](/windows/security/identity-protection/credential-guard/credential-guard)                    | 是              | 是                | 是                                                    | 是                                                    |
| [Device Guard](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide)                        | 是              | 是                | 是                                                    | 是                                                    |
| [Remote Credential Guard](/windows/security/identity-protection/remote-credential-guard)             | 否               | 否                 | 否                                                     | 否                                                     |
| [受防護和支援加密的 VM](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | 否               | 否                 | 支援加密且具有額外設定的 VM | 支援加密且具有額外設定的 VM |

## <a name="remote-credential-guard"></a>Remote Credential Guard：

Remote Credential Guard 僅支援對目標機器的直接連線，而不支援透過遠端桌面連線代理人和遠端桌面閘道的直接連線。
> [!NOTE]
> 如果您在單一執行個體環境中有連線代理人，且 DNS 名稱符合電腦名稱，則或許可以使用 Remote Credential Guard，雖然此功能不受支援。

## <a name="shielded-vms-and-encryption-supported-vms"></a>受防護的 VM 和支援加密的 VM：

- 遠端桌面服務 VDI 中不支援受防護的 VM

若要使用支援加密的 VM：
- 在遠端桌面服務集合建立程序以外使用未受管理的集合和佈建技術，以佈建虛擬機器。
- 使用者設定檔磁碟依賴不同的磁碟，因此不受支援
