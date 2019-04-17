---
title: 角色、 角色服務和功能不在 Server Core 容器-Windows Server 版本 1803
description: 了解角色和我們移除 Windows Server 的伺服器核心容器映像的功能。
ms.prod: windows-server-threshold
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 0ad574a04ba7ecd235f1825bd25c247a1565edf6
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/01/2018
ms.locfileid: "1859896"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>角色、 角色服務和功能不在 Server Core 容器-Windows Server 版本 1803

> 適用於： Windows Server 版本 1803年

Windows Server 版本 1803、 中我們已[降低伺服器核心容器圖像**1.58 GB**的整體大小](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/)。 我們已完成此作業的方法是最佳化架構和[伺服器核心容器](https://docs.microsoft.com/virtualization/windowscontainers/about/)中移除您不需要的事項。 部分已在容器未運作的事項，部分已角色和任何人所使用的功能。 

> [!IMPORTANT]
> 我們已移除這些從伺服器核心**容器**映像，[伺服器核心本身擷取](server-core-roles-and-services.md)。 

以下是完整的功能及移除伺服器核心容器映像的角色清單：

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Bitlocker 公用程式
<br>BitLocker
<br>位元
<br>BITSExtensions 上傳
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS 基礎結構
<br>容器
<br>CoreFileServer
<br>DataCenterBridging-LLDP-工具
<br>DataCenterBridging
<br>Dedup 核心
<br>DeviceHealthAttestationService
<br>DFSN 伺服器
<br>DFSR-基礎結構-ServerEdition
<br>對 ADAM
<br>對-DomainController
<br>DiskIo QoS
<br>EnhancedStorage
<br>FailoverCluster AdminPak
<br>FailoverCluster AutomationServer
<br>FailoverCluster CmdInterface
<br>FailoverCluster FullServer
<br>FailoverCluster PowerShell
<br>檔案服務
<br>FileServerVSSAgent
<br>FRS 基礎結構
<br>FSRM 基礎結構服務
<br>FSRM 基礎結構
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService 套件
<br>IdentityServer SecurityTokenService
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>iSCSITargetServer PowerShell
<br>iSCSITargetServer
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>授權
<br>LightweightServer
<br>Microsoft-Hyper-v-V-管理-用戶端
<br>Microsoft-Hyper-v-V-離線
<br>Microsoft-Hyper-v-V-線上
<br>Microsoft-Hyper-v
<br>Microsoft Windows-FCI-用戶端層封裝
<br>Microsoft Windows-群組原則-ServerAdminTools-更新
<br>Microsoft Windows-子系統 Linux
<br>MSRDC 基礎結構
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P PnrpOnly
<br>PeerDist
<br>列印-用戶端層 Gui
<br>列印 LPDPrintService
<br>列印伺服器-Foundation 功能
<br>列印伺服器角色
<br>QWAVE
<br>RasRoutingProtocols
<br>遠端桌面服務
<br>遠端存取
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices 角色
<br>RightsManagementServices
<br>RMS 同盟
<br>SBMgr UI
<br>ServerCore 驅動程式-一般 WOW64
<br>ServerCore-驅動程式-一般
<br>ServerForNFS 基礎結構
<br>ServerManager-核心-RSAT-功能-工具
<br>ServerMediaFoundation
<br>ServerMigration
<br>來
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol 伺服器
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>儲存層複本-AdminPack
<br>儲存複本
<br>Tpm-PSH-指令程式
<br>UpdateServices 資料庫
<br>UpdateServices 服務
<br>UpdateServices WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>VolumeActivation 完整角色
<br>Web 應用程式 Proxy
<br>WebAccess
<br>WebEnrollmentServices
<br>Windows 防禦者
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders 伺服器
<br>WSS 產品套件

</div>