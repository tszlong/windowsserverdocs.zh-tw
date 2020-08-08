---
title: 不在伺服器核心容器中的角色、角色服務和功能-Windows Server，版本1803
description: 瞭解我們從 Windows Server 的 Server Core 容器映射中移除的角色和功能。
ms.mktglfcycl: manage
ms.sitesec: library
author: lizap
ms.localizationpriority: medium
ms.date: 05/07/2018
ms.openlocfilehash: 2092e330af479ae0cbdb1da88ba87cf233307b59
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993253"
---
# <a name="roles-role-services-and-features-not-in-server-core-containers---windows-server-version-1803"></a>不在伺服器核心容器中的角色、角色服務和功能-Windows Server，版本1803

> 適用於：Windows Server 版本 1803

在 Windows Server 1803 版中，我們將[伺服器核心容器映射的整體大小縮減為**1.58 GB**](https://blogs.technet.microsoft.com/virtualization/2018/01/22/a-smaller-windows-server-core-container-with-better-application-compatibility/)。 我們完成這項作業的方式是，將架構優化，並移除您不需要的[Server Core 容器](/virtualization/windowscontainers/about/)中的專案。 有些是在容器中無法運作的專案，有些則是沒有人使用的角色和功能。

> [!IMPORTANT]
> 我們已從 Server Core**容器**映射中移除這些服務，而不是[server core 本身](server-core-roles-and-services.md)。

以下是從 Server Core 容器映射中移除的功能和角色的完整清單：

<div style='font-size:9.0pt'>

<br>ADCertificateServicesRole
<br>AuthManager
<br>Bitlocker-公用程式
<br>BitLocker
<br>BITS
<br>BITSExtensions-上傳
<br>CCFFilter
<br>CertificateEnrollmentPolicyServer
<br>CertificateEnrollmentServer
<br>CertificateServices
<br>ClientForNFS-基礎結構
<br>容器
<br>CoreFileServer
<br>DataCenterBridging-LLDP-工具
<br>DataCenterBridging
<br>重復資料刪除-核心
<br>DeviceHealthAttestationService
<br>DFSN-伺服器
<br>DFSR-基礎結構-ServerEdition
<br>Microsoft.directoryservices-ADAM
<br>Microsoft.directoryservices-控制器
<br>DiskIo-QoS
<br>EnhancedStorage
<br>容錯移轉叢集-Adminpack.msi
<br>容錯移轉叢集-AutomationServer
<br>容錯移轉叢集-CmdInterface
<br>容錯移轉叢集-FullServer
<br>容錯移轉叢集-PowerShell
<br>檔案服務
<br>FileServerVSSAgent
<br>FRS-基礎結構
<br>FSRM-基礎結構-服務
<br>FSRM-基礎結構
<br>HardenedFabricEncryptionTask
<br>HostGuardian
<br>HostGuardianService-封裝
<br>IdentityServer-如何
<br>IPAMClientFeature
<br>IPAMServerFeature
<br>程式 add-windowsfeature fs-iscsitargetserver-PowerShell
<br>程式 add-windowsfeature fs-iscsitargetserver
<br>iSCSITargetStorageProviders
<br>iSNS_Service
<br>授權
<br>LightweightServer
<br>Microsoft-Hyper-v-管理-用戶端
<br>Microsoft-Hyper-v-離線
<br>Microsoft-Hyper-v-線上
<br>Microsoft-Hyper-v
<br>FCI-用戶端-套件
<br>GroupPolicy-ServerAdminTools-更新
<br>Microsoft-Windows-子系統-Linux
<br>MSRDC-基礎結構
<br>MultipathIo
<br>NetworkController
<br>NetworkControllerTools
<br>NetworkDeviceEnrollmentServices
<br>NetworkLoadBalancingFullServer
<br>NetworkVirtualization
<br>OnlineRevocationServices
<br>P2P-PnrpOnly
<br>PeerDist
<br>列印-用戶端-Gui
<br>列印-LPDPrintService
<br>列印-Server-Foundation-功能
<br>列印-伺服器-角色
<br>QWAVE
<br>RasRoutingProtocols
<br>遠端桌面-服務
<br>RemoteAccess
<br>RemoteAccessMgmtTools
<br>RemoteAccessPowerShell
<br>RemoteAccessServer
<br>ResumeKeyFilter
<br>RightsManagementServices-角色
<br>RightsManagementServices
<br>RMS-同盟
<br>SBMgr-UI
<br>ServerCore-驅動程式-一般-WOW64
<br>ServerCore-驅動程式-一般
<br>ServerForNFS-基礎結構
<br>ServerManager-核心-RSAT-功能工具
<br>ServerMediaFoundation
<br>ServerMigration
<br>SessionDirectory
<br>SetupAndBootEventCollection
<br>ShieldedVMToolsAdminPack
<br>SMB1Protocol-伺服器
<br>SmbDirect
<br>SMBHashGeneration
<br>SmbWitness
<br>SNMP
<br>SoftwareLoadBalancer
<br>儲存體-複本-AdminPack
<br>儲存體-複本
<br>Tpm-PSH-Cmdlet
<br>UpdateServices-資料庫
<br>UpdateServices-服務
<br>UpdateServices-WidDatabase
<br>UpdateServices
<br>VmHostAgent
<br>Volumeactivation-full-role-完整角色
<br>Web 應用程式-Proxy
<br>Mtm
<br>WebEnrollmentServices
<br>Windows-Defender
<br>WindowsServerBackup
<br>WindowsStorageManagementService
<br>WINSRuntime
<br>WMISnmpProvider
<br>WorkFolders-伺服器
<br>WSS-產品套件

</div>