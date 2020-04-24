---
title: Hyper-V 的第 2 代虛擬機器安全性設定
description: 說明 Hyper-V 管理員針對第 2 代虛擬機器提供的安全性設定
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 7eb867529d38ab21ee21c19f92c89ed4128b0ea4
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2020
ms.locfileid: "80860801"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Hyper-V 的第 2 代虛擬機器安全性設定

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

使用 Hyper-V 管理員中的虛擬機器安全性設定，有助於保護虛擬機器的資料與狀態。 您可以保護虛擬機器，使其免於遭受可能在主機上執行的惡意程式碼和資料中心管理員的探查、竊取和篡改。 您所取得的安全性層級取決於您執行的主機硬體、虛擬機器產生，以及您是否設定服務 (名為「主機守護者服務」)，為主機授權以啟動受防護的虛擬機器。  

主機守護者服務是 Windows Server 2016 中的新角色。 此服務會識別合法的 Hyper-V 主機，並允許這些主機執行指定的虛擬機器。 最常設定主機守護者服務的是資料中心。 但是，您可以建立受防護的虛擬機器，並使其在本機執行，而不需要設定主機守護者服務。 您後續可將受防護的虛擬機器散發到主機守護者網狀架構。  

如果您尚未設定主機守護者服務，或是在 Hyper-V 主機上以本機模式加以執行，且主機具有虛擬機器擁有者的守護者金鑰，您可以變更本主題中說明的設定。   守護者金鑰的擁有者是一個組織，會建立並共用私密或公開金鑰，以擁有使用該金鑰建立的所有虛擬機器。  

若要了解如何透過主機守護者服務讓您的虛擬機器更加安全，請參閱下列資源。  

- [強化網狀架構：保護 Hyper-V 中的租用戶密碼 (Ignite 影片)](https://go.microsoft.com/fwlink/?LinkId=746379)
- [受防護網狀架構與受防護的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Hyper-V 管理員中的安全開機設定  

安全開機是適用於第 2 代虛擬機器的一項功能，有助於防止未經授權的韌體、作業系統或統一可延伸韌體介面 (UEFI) 驅動程式 (也稱為選項 ROM) 在開機時執行。 依預設會啟用安全開機。 您可以將安全開機用於執行 Windows 或 Linux 發行版本作業系統的第 2 代虛擬機器。  

下表所述的範本會參考您在確認開機程序的完整性時所需的憑證。  

|範本名稱|說明|  
|-----------------|---------------|  
|Microsoft Windows|選取此項目，可讓 Windows 作業系統的虛擬機器進行安全開機。|  
|Microsoft UEFI 憑證授權單位|選取此項目，可讓 Linux 發行版本作業系統的虛擬機器進行安全開機。|  
|開放原始碼防護的 VM|使用此範本可讓[以 Linux 為基礎的受防護 VM](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template) 進行安全開機。|

如需詳細資訊，請參閱下列主題。  

- [Windows 10 安全性概觀](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [我應在 Hyper-V 建立第 1 或 2 代的虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Hyper-V 上的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-V 管理員中的加密支援設定

您可以選取下列加密支援選項，以利保護虛擬機器的資料和狀態。  

- **啟用信賴平台模組** - 此設定可讓虛擬機器使用虛擬化的信賴平台模組 (TPM) 晶片。 這可讓來賓使用 BitLocker 為虛擬機器磁碟加密。
  - 如果您的 Hyper-V 主機執行 Windows 10 1511，則必須啟用隔離使用者模式。 
- **加密狀態和 VM 移轉流量** - 加密虛擬機器儲存的狀態和即時移轉流量。

### <a name="enable-isolated-user-mode"></a>啟用隔離使用者模式

如果您的 Hyper-V 主機執行 Windows 10 年度更新版之前的 Windows 版本，而您在該主機上選取了 [啟用信賴平台模組]  ，則必須啟用隔離使用者模式。 對於執行 Windows Server 2016 或 Windows 10 年度更新版或更新版本的 Hyper-V 主機，則無須執行此動作。

隔離使用者模式是可將安全性應用程式裝載在 Hyper-V 主機上的虛擬安全模式中的執行階段環境。 虛擬安全模式可用來保護虛擬 TPM 晶片的狀態。  

若要在執行舊版 Windows 10 的 Hyper-V 主機上啟用隔離使用者模式，請  

1.  以系統管理員身分開啟 Windows PowerShell。  

2.  執行下列命令：  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

您可以將已啟用虛擬 TPM 的虛擬機器移轉至任何執行 Windows Server 2016、Windows 10 組建 10586 或更新版本的主機。 但是，如果您將其移轉至其他主機，就可能無法加以啟動。 您必須更新該虛擬機器的金鑰保護裝置，為新主機授與執行虛擬機器的權限。 如需詳細資訊，請參閱[受防護網狀架構與受防護的 VM](https://go.microsoft.com/fwlink/?LinkId=746381) 和 [Windows Server 上的 Hyper-V 系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。  

## <a name="security-policy-in-hyper-v-manager"></a>Hyper-V 管理員中的安全性原則  
如需更高的虛擬機器安全性，請使用 [啟用防護]  選項以停用主控台連線、PowerShell Direct 之類的管理功能，和一些整合元件。 如果您選取此選項，則會選取並強制執行 [安全開機]  、[啟用信賴平台模組]  和 [加密狀態和 VM 移轉流量]  選項。   

您可以在本機執行受防護的虛擬機器，而不需要設定主機守護者服務。 但是，如果您將其移轉至其他主機，就可能無法加以啟動。 您必須更新該虛擬機器的金鑰保護裝置，為新主機授與執行虛擬機器的權限。 如需詳細資訊，請參閱[受防護網狀架構與受防護的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)。  

如需與 Windows Server 中的安全性有關的詳細資訊，請參閱[安全性和保證](../../../security/Security-and-Assurance.md)。  
