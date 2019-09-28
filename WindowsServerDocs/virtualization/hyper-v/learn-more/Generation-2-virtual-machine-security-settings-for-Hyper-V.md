---
title: Hyper-v 的第2代虛擬機器安全性設定
description: 說明 Hyper-v 管理員針對第2代虛擬機器提供的安全性設定
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 06ab4f5f-6b8e-4058-8108-76785aa93d4c
author: larsiwer
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 82544a58a8d46b3063605557be3c63cfa799e4fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364244"
---
# <a name="generation-2-virtual-machine-security-settings-for-hyper-v"></a>Hyper-v 的第2代虛擬機器安全性設定

>適用於：Windows Server 2016，Microsoft Hyper-v Server 2016，Windows Server 2019，Microsoft Hyper-v Server 2019

使用 Hyper-v 管理員中的虛擬機器安全性設定，協助保護虛擬機器的資料與狀態。 您可以保護虛擬機器免于檢查、遭竊，以及從可能在主機上執行的惡意程式碼以及資料中心系統管理員進行篡改。 您所取得的安全性層級取決於您執行的主機硬體、虛擬機器產生，以及是否設定服務（稱為「主機守護者服務」）來授權主機啟動受防護的虛擬機器。  

主機守護者服務是 Windows Server 2016 中的新角色。 它會識別合法的 Hyper-v 主機，並允許它們執行指定的虛擬機器。 您最常設定資料中心的主機守護者服務。 但是，您可以建立受防護的虛擬機器以在本機執行，而不需要設定主機守護者服務。 您稍後可以將受防護的虛擬機器散發到主機守護者網狀架構。  

如果您尚未設定主機守護者服務，或在 Hyper-v 主機上以原生模式執行它，而且主機具有虛擬機器擁有者的監護人金鑰，您可以變更本主題中所描述的設定。   監護人金鑰的擁有者是一種組織，它會建立並共用私用或公開金鑰，以擁有使用該金鑰建立的所有虛擬機器。  

若要瞭解如何讓您的虛擬機器更安全地使用主機守護者服務，請參閱下列資源。  

- [強化網狀架構：保護 Hyper-v 中的租使用者秘密（Ignite video） ](https://go.microsoft.com/fwlink/?LinkId=746379)
- [受防護的網狀架構與受防護的 Vm](https://go.microsoft.com/fwlink/?LinkId=746381)

## <a name="secure-boot-setting-in-hyper-v-manager"></a>Hyper-v 管理員中的安全開機設定  

「安全開機」是第2代虛擬機器可用的功能，可協助防止未經授權的固件、作業系統或整合可延伸韌體介面（UEFI）驅動程式（也稱為「選項 Rom」）在開機時執行。 預設會啟用安全開機。 您可以搭配執行 Windows 或 Linux 散發作業系統的第2代虛擬機器使用安全開機。  

下表所述的範本會參考您需要的憑證，以確認開機程式的完整性。  

|範本名稱|描述|  
|-----------------|---------------|  
|Microsoft Windows|選取即可安全地開機 Windows 作業系統的虛擬機器。|  
|Microsoft UEFI 憑證授權單位單位|選取以安全地開機 Linux 散發作業系統的虛擬機器。|  
|開放原始碼受防護的 VM|此範本可用於保護以 Linux 為[基礎之受防護 vm](https://docs.microsoft.com/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-create-a-linux-shielded-vm-template)的開機。|

如需詳細資訊，請參閱下列主題。  

- [Windows 10 安全性總覽](https://docs.microsoft.com/windows/security/threat-protection/overview-of-threat-mitigations-in-windows-10)  
- [我應該在 Hyper-v 中建立第1代或第2代虛擬機器嗎？](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
- [Hyper-v 上的 Linux 和 FreeBSD 虛擬機器](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  

## <a name="encryption-support-settings-in-hyper-v-manager"></a>Hyper-v 管理員中的加密支援設定

您可以選取下列加密支援選項，以協助保護虛擬機器的資料和狀態。  

- **啟用可信賴平臺模組**-此設定可讓虛擬化的可信賴平臺模組（TPM）晶片供您的虛擬機器使用。 這可讓來賓使用 BitLocker 來加密虛擬機器磁片。
  - 如果您的 Hyper-v 主機執行 Windows 10 1511，您必須啟用隔離使用者模式。 
- **加密狀態和 VM 遷移流量**-加密虛擬機器儲存狀態和即時移轉流量。

### <a name="enable-isolated-user-mode"></a>啟用隔離使用者模式

如果您在執行 Windows 10 年度更新版之前的 Windows 版本的 Hyper-v 主機上選取 [**啟用可信賴平臺模組**]，則必須啟用隔離的使用者模式。 您不需要針對執行 Windows Server 2016 或 Windows 10 年度更新版或更新版本的 Hyper-v 主機執行此動作。

隔離的使用者模式是將安全性應用程式裝載在 Hyper-v 主機上的虛擬安全模式中的執行時間環境。 虛擬安全模式是用來保護和保護虛擬 TPM 晶片的狀態。  

若要在執行舊版 Windows 10 的 Hyper-v 主機上啟用隔離使用者模式，  

1.  以系統管理員身分開啟 Windows PowerShell。  

2.  執行下列命令：  

    ```  
    Enable-WindowsOptionalFeature -Feature IsolatedUserMode -Online  
    New-Item -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Force  
    New-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name EnableVirtualizationBasedSecurity -Value 1 -PropertyType DWord -Force  

    ```  

您可以將已啟用虛擬 TPM 的虛擬機器，遷移至任何執行 Windows Server 2016、Windows 10 組建10586或更新版本的主機。 但是，如果您將它遷移至另一部主機，可能就無法啟動它。 您必須更新該虛擬機器的金鑰保護裝置，以授權新主機執行虛擬機器。 如需詳細資訊，請參閱[受保護的網狀架構和受防護的 vm](https://go.microsoft.com/fwlink/?LinkId=746381)和[Windows Server 上的 hyper-v 系統需求](../System-requirements-for-Hyper-V-on-Windows.md)。  

## <a name="security-policy-in-hyper-v-manager"></a>Hyper-v 管理員中的安全性原則  
如需更多虛擬機器的安全性，請使用 [**啟用防護**] 選項來停用管理功能，例如主控台連線、PowerShell Direct 和一些整合元件。 如果您選取此選項，則會選取並強制執行 [**安全開機**]、[**啟用可信賴平臺模組**] 和 [**加密狀態] 和 [VM 遷移流量**] 選項。   

您可以在本機執行受防護的虛擬機器，而不需要設定主機守護者服務。 但是，如果您將它遷移至另一部主機，可能就無法啟動它。 您必須更新該虛擬機器的金鑰保護裝置，以授權新主機執行虛擬機器。 如需詳細資訊，請參閱[受防護網狀架構與受防護的 VM](https://go.microsoft.com/fwlink/?LinkId=746381)。  

如需 Windows Server 安全性的詳細資訊，請參閱[安全性和保證](../../../security/Security-and-Assurance.md)。  
