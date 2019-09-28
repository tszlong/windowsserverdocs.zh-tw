---
title: 規劃 Windows Server 中的 Hyper-v 安全性
description: 提供 Hyper-v 主機和虛擬機器的安全性考慮清單
ms.prod: windows-server
ms.service: na
ms.suite: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
manager: dongill
author: larsiwer
ms.author: kathyDav
ms.date: 08/03/2018
ms.openlocfilehash: 8fd86ae500fff1e6b8c27b0d34d1dcbeeade9f81
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392956"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>規劃 Windows Server 中的 Hyper-v 安全性

>適用於：Windows Server 2016，Microsoft Hyper-v Server 2016，Windows Server 2019，Microsoft Hyper-v Server 2019

保護 Hyper-v 主機作業系統、虛擬機器、設定檔和虛擬機器資料的安全。 使用下列建議做法清單做為檢查清單，以協助您保護 Hyper-v 環境。

## <a name="secure-the-hyper-v-host"></a>保護 Hyper-v 主機
- **確保主機 OS 安全。**
    - 使用管理作業系統所需的最小 Windows Server 安裝選項，將受攻擊面降至最低。 如需詳細資訊，請參閱 Windows Server 技術內容庫的[安裝選項一節](/windows-server/windows-server#installation-options)。 我們不建議您在 Windows 10 上的 Hyper-v 上執行生產工作負載。
    - 使用最新的安全性更新，讓 Hyper-v 主機作業系統、固件和設備磁碟機保持在最新狀態。 請檢查廠商的建議以更新固件和驅動程式。
    - 請勿使用 Hyper-v 主機做為工作站，或安裝任何不必要的軟體。
    - 從遠端系統管理 Hyper-v 主機。 如果您必須在本機管理 Hyper-v 主機，請使用 Credential Guard。 如需詳細資訊，請參閱[使用 Credential Guard 保護衍生的網域認證](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。
    - 啟用程式碼完整性原則。 使用虛擬化安全性保護的程式碼完整性服務。 如需詳細資訊，請參閱[Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
- **使用安全的網路。**
    - 針對實體 Hyper-v 電腦使用具有專用網路介面卡的個別網路。
    - 使用私人或安全網路來存取 VM 設定和虛擬硬碟檔案。
    - 針對您的即時移轉流量使用私人/私人網路絡。 請考慮在此網路上啟用 IPSec，以使用加密，並在遷移期間保護 VM 的資料。 如需詳細資訊，請參閱[設定不含容錯移轉叢集的即時移轉主機](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。
- **保護儲存體遷移流量。** 

    使用 SMB 3.0 進行 SMB 資料的端對端加密，以及在不受信任的網路上進行資料保護的篡改或竊聽。 使用私人網路來存取 SMB 共用內容，以防止攔截式攻擊。 如需詳細資訊，請參閱[SMB 安全性增強功能](https://technet.microsoft.com/library/dn551363.aspx)。 
- **將主機設定為受防護網狀架構的一部分。** 

    如需詳細資訊，請參閱受防護的網狀[架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
- **保護裝置。** 

    保護存放虛擬機器資源檔的儲存裝置。
    
- **保護硬碟。** 

    使用 BitLocker 磁碟機加密來保護資源。
    
- **強化 Hyper-v 主機作業系統。** 

    使用[Windows Server 安全性基準](https://docs.microsoft.com/windows/device-security/windows-security-baselines)中所述的基準安全性設定建議。
    
- **授與適當的許可權。**
    - 將需要管理 Hyper-v 主機的使用者新增至 Hyper-v 系統管理員群組。
    - 不要授與 Hyper-v 主機作業系統的虛擬機器系統管理員許可權。

- **設定 Hyper-v 的防毒程式排除專案和選項。**  

    Windows Defender 已設定[自動排除](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)專案。 如需有關排除的詳細資訊，請參閱[建議的 hyper-v 主機防毒程式排除](https://support.microsoft.com/kb/3105657)專案。 

- **請勿掛接未知的 Vhd。** 這可能會讓主機暴露在檔案系統層級的攻擊之下。

- **除非必要，否則請勿在生產環境中啟用嵌套。**

    如果您啟用了 [嵌套]，請不要在虛擬機器上執行不受支援的系統平臺。  

針對更安全的環境：

- **使用具有信賴平臺模組（TPM）2.0 晶片的硬體來設定受防護的網狀架構。** 

    如需詳細資訊，請參閱[Windows Server 2016 上的 Hyper-v 系統需求](../system-requirements-for-hyper-v-on-windows.md)。

## <a name="secure-virtual-machines"></a>保護虛擬機器
- **為支援的客體作業系統建立第2代虛擬機器。** 

    如需詳細資訊，請參閱[第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **啟用安全開機。** 

    如需詳細資訊，請參閱[第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **保護虛擬機器作業系統的安全。**

    - 在生產環境中開啟虛擬機器之前，請先安裝最新的安全性更新。
    - 為需要的支援的客體作業系統安裝整合服務，並將其保持在最新狀態。 執行支援的 Windows 版本之來賓的整合服務更新可透過 Windows Update 取得。
    - 根據執行的角色，強化每部虛擬機器中執行的作業系統。 使用[Windows 安全性基準](https://docs.microsoft.com/windows/device-security/windows-security-baselines)中所述的基準安全性設定建議。
    
- **使用安全的網路。** 

    請確定虛擬網路介面卡連接到正確的虛擬交換器，並套用適當的安全性設定和限制。
    
- **將虛擬硬碟和快照集檔案儲存在安全的位置。**

- **保護裝置。** 

    只設定虛擬機器所需的裝置。 請勿在實際執行環境中啟用離散裝置指派，除非您在特定案例中需要它。 如果您啟用它，請務必只向受信任的廠商公開裝置。 
    
- 根據虛擬機器角色，適當地在虛擬機器中**設定防毒軟體、防火牆和入侵偵測軟體**。

- **針對執行 Windows 10 或 Windows Server 2016 或更新版本的來賓啟用以虛擬化為基礎的安全性。** 

    如需詳細資訊，請參閱[Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
    
- **只有在特定工作負載需要時，才啟用離散裝置指派**。 

    由於傳遞實體裝置的本質，請與裝置製造商合作，以瞭解是否應在安全的環境中使用它。

針對更安全的環境：

- **部署已啟用防護的虛擬機器，並將其部署到受保護的網狀架構。** 

    如需詳細資訊，請參閱[第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)和受防護網狀[架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
