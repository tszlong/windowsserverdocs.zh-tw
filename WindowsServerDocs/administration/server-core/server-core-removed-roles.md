---
title: 角色、 角色服務和不在 Windows Server-伺服器核心功能
description: 了解角色和功能不包含在 Windows Server 的伺服器核心安裝選項。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 308bc8a5d25e2ec67438f0ee03cbfce6f7411ca2
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2018
ms.locfileid: "2604786"
---
# 角色、 角色服務和不在 Windows Server-伺服器核心功能

> 適用於： Windows Server （分號每年次通道） 和 Windows Server 2016

下列角色、 角色服務和功能已移除從 Windows Server 的伺服器核心安裝選項。 使用此資訊可協助找出 [伺服器核心] 選項如果適用於您的環境。

> [!NOTE]
> 您也可以參閱清單中的角色、 角色服務和功能[都包含在 Server Core](server-core-roles-and-services.md)。 它是一個非常大型的清單，因此為了獲得最佳結果，請搜尋該清單中的特定角色或您感興趣的功能。

## 不在 Server Core 角色

- 傳真
- MultiPointServerRole
- NPAS
- WDS

## 不在 Server Core 角色服務
請注意，某些遠端桌面角色服務已包含在 Server Core （連線代理人、 授權、 虛擬化主機），但未 （閘道、 RD 工作階段主機、 Web 存取）。

- 列印掃描伺服器
- 列印網際網路
- RDS 閘道
- RDS RD 伺服器
- RDS Web Access
- Web 管理主控台
- Web Lgcy-管理主控台
- WDS 部署
- WDS 傳輸 *（之前 Windows Server 版本 1803年)*

## 不在 Server Core 功能

- 位元-IIS-Ext
- BitLocker NetworkUnlock
- 直接播放
- 網際網路列印用戶端
- LPR 連接埠監視器
- MSMQ 多點傳送
- 組
- 遠端協助
- RSAT SMTP
- RSAT-功能-工具-BitLocker-RemoteAdminTool
- RSAT 位元伺服器
- RSAT NLB
- RSAT SNMP
- RSAT WINS
- Hyper-v V 工具
- RSAT-RDS-工具
- RSAT RDS 閘道
- RSAT-RDS-授權-診斷-UI
- RDS 授權 UI
- UpdateServices UI
- RSAT ADC
- RSAT-ADC-管理
- RSAT 線上回應
- RSAT ADRMS
- RSAT 傳真
- RSAT 檔案服務
- RSAT DFS 管理-詐騙
- RSAT-FSRM-管理
- RSAT NFS 管理員
- RSAT NPAS
- RSAT 列印服務
- RSAT-VA-工具
- WDS AdminPack
- SMTP 伺服器
- TFTP 用戶端
- WebDAV 重新導向程式
- 生物特徵架構
- Windows 防禦者 Gui
- Windows Identity Foundation
- PowerShell ISE
- 搜尋服務
- Windows TIFF IFilter
- 無線網路
- XPS 檢視器

