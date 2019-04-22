---
title: 遠端桌面服務 VDI 支援的 Windows 10 安全性組態
description: 提供與 Windows Server 2016 中的 RDS 的 Windows 10 VDI 的支援組態的相關資訊。
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 10/27/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8f164f5d-a498-4f91-a12f-3e01d554f810
author: lizap
manager: dongill
ms.openlocfilehash: ff890150dcea30c425267dcaae9b1bdbc6d78b8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820079"
---
# <a name="supported-windows-10-security-configurations-for-remote-desktop-services-vdi"></a>遠端桌面服務 VDI 支援的 Windows 10 安全性組態

> 適用於：Windows Server 2016

Windows 10 和 Windows Server 2016 有新的層級的內建作業系統，若要進一步防範安全性缺口，協助封鎖惡意的攻擊，並加強虛擬機器、 應用程式和資料安全性的保護。

> [!NOTE]
> 請務必檢閱[遠端桌面服務支援的組態資訊](rds-supported-config.md)。

下表概述支援在 VDI 部署中使用 RDS 的這些新功能

|  VDI 集合類型               |  受管理的集區 |  管理個人 |  未受管理集區                                     |  未受管理的個人                                    |
|-------------------------------------|------------------|--------------------|--------------------------------------------------------|--------------------------------------------------------|
| [Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/credential-guard)                    | 是              | 是                | 是                                                    | 是                                                    |
| [Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/device-guard-deployment-guide)                        | 是              | 是                | 是                                                    | 是                                                    |
| [遠端 Credential Guard](https://technet.microsoft.com/itpro/windows/keep-secure/remote-credential-guard)             | 否               | 否                 | 否                                                     | 否                                                     |
| [受防護的和支援加密的 Vm](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md) | 否               | 否                 | 使用額外的組態支援 Vm 的加密 | 使用額外的組態支援 Vm 的加密 |

## <a name="remote-credential-guard"></a>遠端 Credential Guard:

遠端 Credential Guard 僅適用於直接連線到目標電腦，而不適用於透過遠端桌面連線代理人和遠端桌面閘道的項目。
> [!NOTE]
> 如果您有連線代理人在單一執行個體環境中，而且 DNS 名稱符合電腦名稱，您可以使用遠端 Credential Guard，雖然不支援此功能。

## <a name="shielded-vms-and-encryption-supported-vms"></a>受防護的 Vm 和加密支援的 Vm: 

- 遠端桌面服務 VDI 中不支援受防護的 Vm 

利用加密支援的 Vm:
- 佈建虛擬機器中使用未受管理的集合及佈建的技術在遠端桌面服務集合建立程序之外。 
- 因為它們都依賴差異磁碟，都不支援使用者設定檔磁碟 

