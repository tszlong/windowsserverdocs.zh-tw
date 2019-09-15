---
title: 不在 Windows Server Core 中的角色、角色服務和功能
description: 瞭解 Windows Server 的 Server Core 安裝選項中未包含的角色和功能。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 02/23/2018
ms.openlocfilehash: 882410792c7b8df8a8275c357d64fc17c9f3479e
ms.sourcegitcommit: feec5cbe983c8c5800ccd4fc214914084fcceaba
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2019
ms.locfileid: "70975255"
---
# <a name="roles-role-services-and-features-not-in-windows-server---server-core"></a>不在 Windows Server Core 中的角色、角色服務和功能

> 適用於：Windows Server 2019、Windows Server 2016 和 Windows Server （半年通道）

已從 Windows Server 的 Server Core 安裝選項中移除下列角色、角色服務和功能。 使用這項資訊有助於找出伺服器核心選項是否適用于您的環境。

> [!NOTE]
> 您也可以查看[Server Core 中包含](server-core-roles-and-services.md)的角色、角色服務和功能的清單。 這是非常大的清單，因此為了得到最佳結果，請在清單中搜尋您感興趣的特定角色或功能。

## <a name="roles-not-in-server-core"></a>不在 Server Core 中的角色

- 傳真伺服器（**傳真**）
- MultiPoint 服務（**MultiPointServerRole**）
- 網路原則與存取服務（**NPAS**）
- Windows 部署服務（**WDS**） *（Windows Server 1803 版之前）*

## <a name="role-services-not-in-server-core"></a>不在 Server Core 中的角色服務
請注意，某些遠端桌面角色服務已包含在 Server Core （連線代理人、授權、虛擬化主機）中，但其他則不是（閘道、RD 工作階段主機、Web 存取）。

- 列印和檔服務 \ 分散式掃描伺服器（**列印-掃描-伺服器**）
- 列印和檔服務 \ 網際網路列印（**列印-網際網路**）
- 遠端桌面服務 \ 遠端桌面閘道（**RDS-閘道**）
- 遠端桌面服務 \ 遠端桌面工作階段主機（**RDS-RD-伺服器**）
- 遠端桌面服務 \ 遠端桌面 Web 存取（**RDS-Web 存取**）
- 角色服務網頁伺服器（IIS） \ 管理工具 \ IIS 管理主控台（**Web 管理-主控台**）
- 角色服務網頁伺服器（IIS） \ 管理工具 \ IIS 6 管理相容性 \ IIS 6 管理主控台（**Web Lgcy-管理主控台**）
- Windows 部署服務 \ 部署伺服器（**WDS-部署**）
- Windows 部署服務 \ 傳輸伺服器（**WDS-傳輸**） *（Windows Server 1803 版之前）*

## <a name="features-not-in-server-core"></a>不在 Server Core 中的功能
- 背景智慧型傳送服務（BITS） \ IIS 伺服器延伸模組（**bits-iis-Ext**）
- BitLocker 網路解除鎖定（**bitlocker-NetworkUnlock**）
- 直接播放（**直接播放**）
- 網際網路列印用戶端（**網際網路-列印-用戶端**）
- LPR 埠監視器（**lpr-埠-監視**）
- 訊息佇列 \ 訊息佇列服務 \ 多播支援（**MSMQ-多播**）
- RAS 連線管理員系統管理元件（**CMAK**）
- 遠端協助（**遠端協助**）
- 遠端伺服器管理工具 \ 功能管理工具 \ SMTP 伺服器工具（**RSAT-SMTP**）
- 遠端伺服器管理工具 \ 功能管理工具 \ BitLocker 磁碟機加密管理公用程式 \ BitLocker 磁碟機加密工具（**RSAT-功能工具-BitLocker-RemoteAdminTool**）
- 遠端伺服器管理工具 \ 功能管理工具 \ BITS 伺服器擴充功能工具（**RSAT-bits-伺服器**）
- 遠端伺服器管理工具 \ 功能管理工具 \ 網路負載平衡工具（**RSAT-NLB**）
- 遠端伺服器管理工具 \ 功能管理工具 \ SNMP 工具（**RSAT-snmp**）
- 遠端伺服器管理工具 \ 功能管理工具 \ WINS 伺服器工具（**RSAT-WINS**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Hyper-v 管理工具 \ hyper-v-v GUI 管理工具（**hyper-v-工具**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 遠端桌面服務工具 \ 遠端桌面閘道工具（**RSAT-RDS-工具**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 遠端桌面服務工具 \ 遠端桌面閘道工具（**RSAT-RDS-閘道**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 遠端桌面服務工具 \ 遠端桌面授權診斷程式工具（**RSAT-RDS-授權-診斷-UI**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 遠端桌面服務工具 \ 遠端桌面授權工具（**RDS-授權-UI**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Windows Server Update Services 工具 \ 使用者介面管理主控台（**UpdateServices-UI**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Active Directory 憑證服務工具（**RSAT-ADCS**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Active Directory 憑證服務工具 \ 憑證授權單位單位管理工具（**RSAT-ADCS-管理**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Active Directory 憑證服務工具 \ 線上回應工具（**RSAT-線上-回應**者）
- 遠端伺服器管理工具 \ 角色管理工具 \ Active Directory Rights Management Services 工具（**RSAT-ADRMS**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 傳真伺服器工具（**RSAT-傳真**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 檔案服務工具（**RSAT-檔案服務**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 檔案服務工具 \ DFS 管理工具（**RSAT-DFS-管理-Con**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 檔案服務工具 \ 檔案伺服器 Resource Manager 工具（**RSAT-FSRM-管理**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 檔案服務工具 \ 服務（適用于網路檔案系統管理工具）（**RSAT-NFS-系統管理員**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 網路原則與存取服務工具（**RSAT-NPAS**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 列印和檔服務工具（**RSAT-列印-服務**）
- 遠端伺服器管理工具 \ 角色管理工具 \ 大量啟用工具（**RSAT-VA-工具**）
- 遠端伺服器管理工具 \ 角色管理工具 \ Windows 部署服務工具（**WDS-AdminPack**）
- 簡單 TCP/IP 服務（**簡單 TCPIP**）
- SMTP 伺服器（**smtp-伺服器**）
- TFTP 用戶端（**tftp-用戶端**）
- WebDAV 重新導向器（**webdav-重新導向**器）
- Windows 生物特徵辨識架構（**生物識別架構**）
- Windows Defender 功能 \ GUI （適用于 Windows Defender）（**windows-Defender-gui**）
- Windows Identity Foundation 3.5 （**windows-身分識別基礎**）
- Windows PowerShell \ Windows PowerShell ISE （**powershell-ISE**）
- Windows 搜尋服務（**搜尋服務**）
- Windows TIFF IFilter （**windows-TIFF-IFilter**）
- 無線局域網路服務（**無線網路**）
- XPS 檢視器（**xps-檢視器**）
