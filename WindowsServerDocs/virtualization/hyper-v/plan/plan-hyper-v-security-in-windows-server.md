---
title: 規劃 Windows Server 中的 HYPER-V 安全性
description: 提供安全性考量適用於 hyper-v 主機和虛擬機器的清單
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 462124e1907ef03aa7746dd050be3e8c84c7abda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877409"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>規劃 Windows Server 中的 HYPER-V 安全性

>適用於：Windows Server 2016、 Microsoft HYPER-V Server 2016 中，Windows Server 2019，Microsoft HYPER-V Server 2019

保護 HYPER-V 主機作業系統、 虛擬機器、 設定檔和虛擬機器資料。 使用下列清單中的建議做法為檢查清單，可協助您保護您的 HYPER-V 環境。

## <a name="secure-the-hyper-v-host"></a>保護 HYPER-V 主機
- **將的主機作業系統安全的保留。**
    - 使用管理作業系統所需的最小 Windows 伺服器安裝選項，以減少受攻擊面。 如需詳細資訊，請參閱 <<c0> [ 安裝選項 > 一節](/windows-server/windows-server#installation-options)的 Windows Server 技術的內容庫。 我們不建議您在 Windows 10 上的 HYPER-V 上執行生產工作負載。
    - 請為 HYPER-V 主機作業系統、 韌體及裝置驅動程式的最新的最新的安全性更新。 請檢查您的廠商的建議，以更新韌體和驅動程式。
    - 請勿使用 HYPER-V 主機做為工作站，或安裝任何不必要的軟體。
    - 從遠端管理 HYPER-V 主機。 如果您必須管理的 HYPER-V 主機在本機，使用 Credential Guard。 如需詳細資訊，請參閱[使用 Credential Guard 保護衍生的網域認證](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard)。
    - 啟用程式碼完整性原則。 使用虛擬化型安全性受保護的程式碼完整性服務。 如需詳細資訊，請參閱 < [Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
- **使用安全的網路。**
    - 實體的 HYPER-V 電腦專用的網路介面卡使用個別的網路。
    - 使用私用或安全網路存取的 VM 組態和虛擬硬碟檔案。
    - 使用適用於您的即時移轉流量的私用/專用的網路。 請考慮使用加密來保護您的 VM 資料會透過網路在移轉期間，此網路上啟用 IPSec。 如需詳細資訊，請參閱 <<c0> [ 設定即時移轉，而不容錯移轉叢集的主機](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)。
- **安全存放裝置移轉流量。** 

    您可以使用 SMB 3.0 進行端對端加密的 SMB 資料和資料保護遭到竄改或不受信任網路上遭到竊聽。 您可以使用私人網路來存取 SMB 共用內容，以防止攔截攻擊。 如需詳細資訊，請參閱 < [SMB 安全性增強功能](https://technet.microsoft.com/library/dn551363.aspx)。 
- **設定受防護網狀架構的一部分的主機。** 

    如需詳細資訊，請參閱 <<c0> [ 受防護網狀架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
- **保護裝置。** 

    保護您用來保留虛擬機器的資源檔案的儲存體裝置。
    
- **保護硬碟機。** 

    您可以使用 BitLocker 磁碟機加密來保護資源。
    
- **強化 HYPER-V 主機作業系統。** 

    使用基準安全性建議的設定中所述[Windows Server 安全性基準](https://docs.microsoft.com/windows/device-security/windows-security-baselines)。
    
- **授與適當的權限。**
    - 新增要管理 HYPER-V 主機的 HYPER-V administrators 群組的使用者。
    - 不，授與虛擬機器系統管理員權限，HYPER-V 主機作業系統。

- **設定防毒程式排除項目和 hyper-v 的選項。**  

    已經有 Windows Defender[自動排除項目](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus)設定。 如需排除的詳細資訊，請參閱[建議的 HYPER-V 主機的防毒軟體排除](https://support.microsoft.com/kb/3105657)。 

- **不會將未知的 Vhd 掛接。** 這樣可能會公開主應用程式檔案系統層級的攻擊。

- **請勿啟用巢狀生產環境中，除非它是必要。**

    如果您啟用巢狀結構時，不執行不受支援的 hypervisor 的虛擬機器上。  

更安全的環境：

- **使用可信賴平台模組 (TPM) 2.0 晶片的硬體來設定受防護網狀架構。** 

    如需詳細資訊，請參閱 <<c0> [ 適用於 Windows Server 2016 上的 HYPER-V 系統需求](../system-requirements-for-hyper-v-on-windows.md)。

## <a name="secure-virtual-machines"></a>保護虛擬機器
- **建立層代 2 的支援的客體作業系統的虛擬機器。** 

    如需詳細資訊，請參閱 <<c0> [ 第 2 代的安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **啟用安全開機。** 

    如需詳細資訊，請參閱 <<c0> [ 第 2 代的安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。
    
- **保留的客體 OS 的安全。**

    - 在生產環境中的虛擬機器開啟前，請安裝最新的安全性更新。
    - 安裝適用於支援的客體作業系統，需要它，並將它保持在最新的 integration services。 執行支援的 Windows 版本的客體整合服務更新都是透過 Windows Update。
    - 強化根據它所執行的角色每個虛擬機器中執行的作業系統。 使用基準安全性建議的設定中所述[Windows 安全性基準](https://docs.microsoft.com/windows/device-security/windows-security-baselines)。
    
- **使用安全的網路。** 

    請確定虛擬網路介面卡連線到正確的虛擬交換器，並將適當的安全性設定和限制套用。
    
- **將虛擬硬碟以及快照集檔案儲存在安全的位置。**

- **保護裝置。** 

    設定虛擬機器只需要的裝置。 請勿啟用您的生產環境中的不連續的裝置指派，除非您需要為特定案例。 如果您啟用它，請確定只會公開來自受信任的廠商的裝置。 
    
- **設定防毒軟體、 防火牆和入侵偵測軟體**內適當的虛擬機器為基礎的虛擬機器角色。

- **啟用執行 Windows 10 或 Windows Server 2016 或更新版本的來賓虛擬化型安全性。** 

    如需詳細資訊，請參閱 < [Device Guard 部署指南](https://docs.microsoft.com/windows/device-security/device-guard/device-guard-deployment-guide)。
    
- **如果需要特定的工作負載，只啟用離散的裝置指派**。 

    由於通過實體裝置，會使用裝置製造商，以了解是否應該使用安全的環境中。

更安全的環境：

- **部署虛擬機器與防護 已啟用，並將其部署至受防護網狀架構。** 

    如需詳細資訊，請參閱 <<c0> [ 第 2 代的安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)並[受防護網狀架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
