---
title: 適用於 HYPER-V 第 2 代虛擬機器的安全性設定
description: 描述第 2 代虛擬機器的 HYPER-V Manager 中可用的安全性設定
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 90a2b7234ee55d8469b6e02ba3de3a0efc080a3e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889499"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>適用於 HYPER-V 第 2 代虛擬機器的安全性設定

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

使用 HYPER-V 管理員中的虛擬機器的安全性設定，來協助保護資料和虛擬機器的狀態。 您可以保護虛擬機器從檢查、 竊取與竄改，從這兩個惡意程式碼，可能會在主控件和資料中心系統管理員執行。 您所取得的安全性層級取決於您執行時，虛擬機器世代，主機硬體並是否您設定服務，稱為 「 主機守護者服務，可授權啟動受防護的虛擬機器的主機。  

主機守護者服務是 Windows Server 2016 中新的角色。 它會識別合法的 HYPER-V 主機，並讓它們順利執行指定的虛擬機器。 您通常會設定主機守護者服務，做為資料中心。 但是，您可以建立受防護的虛擬機器，而不需設定主機守護者服務在本機執行。 您稍後可以將受防護的虛擬機器至主機守護者網狀架構。  

如果您尚未設定主機守護者服務，或在執行本機模式中的 HYPER-V 主機和主機有虛擬機器擁有者的守護者金鑰，您可以變更此主題中所述的設定。   組織可建立和擁有的所有虛擬機器的私用或公用金鑰建立與該索引鍵的共用守護者金鑰的擁有者。  

若要了解您如何讓您的虛擬機器主機守護者服務更加安全，請參閱下列資源。  

- [強化網狀架構：保護 HYPER-V (Ignite 影片) 中的租用戶密碼](https://go.microsoft.com/fwlink/?LinkId=746379)
- [受防護網狀架構與受防護的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>保護在 HYPER-V Manager 中的開機設定  

安全開機是適用於第 2 代虛擬機器可協助防止未經授權的韌體、 作業系統或整合可延伸韌體介面 (UEFI) 驅動程式 （也稱為選項 Rom） 在開機時執行的功能。 根據預設，已啟用安全開機。 您可以使用安全開機第 2 代虛擬機器執行 Windows 或 Linux 發佈作業系統。  

下表所述的範本，請參閱您要驗證的開機程序完整性的憑證。  

|範本名稱|描述|  
|-----------------|---------------|  
|Microsoft Windows|選取以安全開機的 Windows 作業系統的虛擬機器。|  
|Microsoft UEFI 憑證授權單位|選取安全開機的虛擬機器 Linux 發佈的作業系統。|  
|開放原始碼受防護的 VM|此範本可用來安全開機[linux 受防護的 Vm](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template)。|

如需詳細資訊，請參閱下列主題。  

- [Windows 10 安全性概觀](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [應該在 HYPER-V 中建立 1 或 2 代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Linux 和 FreeBSD 虛擬機器之 HYPER-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>在 HYPER-V 管理員中的加密支援設定

您可以協助保護資料和虛擬機器的狀態，藉由設定下列的加密支援選項。  

- **啟用信賴平台模組**-此設定可讓您的虛擬化的信賴平台模組 (TPM) 晶片可供您的虛擬機器。 這可讓客體利用 BitLocker 來加密虛擬機器磁碟。
  - 如果您的 HYPER-V 主機正在執行 Windows 10 1511 版，您必須啟用隔離的使用者模式。 
- **加密的狀態和 VM 移轉流量**-加密儲存的虛擬機器的狀態和即時移轉流量。

### <a name="enable-isolated-user-mode"></a>啟用隔離的使用者模式

如果您選取**啟用信賴平台模組**上執行 Windows 10 年度更新版之前的 Windows 版本的 HYPER-V 主機，您必須啟用隔離的使用者模式。 您不需要這樣做，HYPER-V 主機，該執行的 Windows Server 2016 或 Windows 10 年度更新版或更新版本。

裝載虛擬安全模式中的 HYPER-V 主機上的安全性應用程式的執行階段環境隔離的使用者模式。 虛擬安全模式用來保護虛擬的 TPM 晶片的狀態。  

若要執行舊版的 Windows 10 的 HYPER-V 主機上啟用隔離的使用者模式  

1.  以系統管理員身分開啟 Windows PowerShell。  

2.  執行下列命令：  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

您可以將虛擬機器移轉至執行 Windows Server 2016 的任何主機啟用的虛擬 TPM 使用，Windows 10 組建 10586 或更新版本的版本。 但是，如果您移轉到另一個主機，您可能無法啟動它。 您必須更新該虛擬機器授權新的主機，以執行虛擬機器金鑰保護裝置。 如需詳細資訊，請參閱 <<c0> [ 受防護網狀架構與受防護的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)並[適用於 Windows Server 上的 HYPER-V 系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。  

## <a name="security-policy-in-hyper-v-manager"></a>在 HYPER-V 管理員中的安全性原則  
針對多個虛擬機器的安全性，使用**啟用防護**選項以停用管理功能，例如主控台連線、 PowerShell Direct 和一些整合元件。 如果您選取此選項時，**安全開機**，**啟用信賴平台模組**，並**加密的狀態和 VM 移轉流量**選取並強制執行選項。   

您可以在本機執行受防護的虛擬機器，而不需設定主機守護者服務。 但是，如果您移轉到另一個主機，您可能無法啟動它。 您必須更新該虛擬機器授權新的主機，以執行虛擬機器金鑰保護裝置。 如需詳細資訊，請參閱[受防護網狀架構與受防護的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)。  

如需有關 Windows Server 中的安全性的詳細資訊，請參閱[安全性和保證](../../../security/Security-and-Assurance.md)。  
