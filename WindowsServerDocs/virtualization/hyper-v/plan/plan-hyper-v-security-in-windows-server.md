---
title: 規劃 Windows Server 中的 Hyper-v 安全性
description: 提供 Hyper-v 主機和虛擬機器的安全性考慮清單
ms.topic: article
ms.assetid: 115db481-b57e-41c3-8354-504f4bc6113a
ms.author: benarm
author: BenjaminArmstrong
ms.date: 08/03/2018
ms.openlocfilehash: e41a97846a9adc6ff849fbece4c4ac2ee7590c89
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745913"
---
# <a name="plan-for-hyper-v-security-in-windows-server"></a>規劃 Windows Server 中的 Hyper-v 安全性

>適用於：Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019

保護 Hyper-v 主機作業系統、虛擬機器、設定檔和虛擬機器資料。 使用下列建議做法清單作為檢查清單，以協助您保護 Hyper-v 環境。

## <a name="secure-the-hyper-v-host"></a>保護 Hyper-v 主機的安全
- **保持主機 OS 的安全。**
    - 使用管理作業系統所需的最小 Windows Server 安裝選項，將攻擊面最小化。 如需詳細資訊，請參閱 Windows Server 技術內容庫的 [安裝選項一節](../../../get-started-19/install-upgrade-migrate-19.md) 。 我們不建議您在 Windows 10 上的 Hyper-v 上執行生產工作負載。
    - 使用最新的安全性更新，讓 Hyper-v 主機作業系統、固件和設備磁碟機保持在最新狀態。 請檢查廠商的建議，以更新固件和驅動程式。
    - 請勿使用 Hyper-v 主機做為工作站或安裝任何不必要的軟體。
    - 從遠端系統管理 Hyper-v 主機。 如果您必須在本機管理 Hyper-v 主機，請使用 Credential Guard。 如需詳細資訊，請參閱[使用 Credential Guard 保護衍生的網域認證](/windows/access-protection/credential-guard/credential-guard)。
    - 啟用程式碼完整性原則。 使用以虛擬化為基礎的安全性保護程式碼完整性服務。 如需詳細資訊，請參閱 [Device Guard 部署指南](/windows/device-security/device-guard/device-guard-deployment-guide)。
- **使用安全的網路。**
    - 針對實體 Hyper-v 電腦使用具有專用網路介面卡的個別網路。
    - 使用私人或安全網路來存取 VM 設定和虛擬硬碟檔案。
    - 針對您的即時移轉流量使用私人/私人網路絡。 請考慮在此網路上啟用 IPSec，以在遷移期間透過網路使用加密及保護您 VM 的資料。 如需詳細資訊，請參閱 [設定主機以進行即時移轉，而不需要容錯移轉](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)叢集。
- **保護儲存體遷移流量。**

    在不受信任的網路上使用 SMB 3.0 進行 SMB 資料的端對端加密，以及資料保護的篡改或竊聽。 使用私人網路來存取 SMB 共用內容，以防止攔截式攻擊。 如需詳細資訊，請參閱 [SMB 安全性增強功能](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn551363(v=ws.11))。
- **將主機設定為受防護網狀架構的一部分。**

    如需詳細資訊，請參閱受防護網狀 [架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。
- **安全裝置。**

    保護您保存虛擬機器資源檔的存放裝置。

- **保護硬碟的安全。**

    使用 BitLocker 磁碟機加密來保護資源。

- **強化 Hyper-v 主機作業系統。**

    使用 [Windows Server 安全性基準](/windows/device-security/windows-security-baselines)中所述的基準安全性設定建議。

- **授與適當的許可權。**
    - 將需要管理 Hyper-v 主機的使用者新增至 Hyper-v 系統管理員群組。
    - 請勿授與 Hyper-v 主機作業系統的虛擬機器系統管理員許可權。

- **設定 Hyper-v 的防毒軟體排除專案與選項。**

    Windows Defender 已設定 [自動排除](/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus) 專案。 如需排除的詳細資訊，請參閱 [hyper-v 主機的建議防毒軟體排除](https://support.microsoft.com/kb/3105657)專案。

- **請勿掛接未知的 Vhd。** 這可能會讓主機暴露在檔案系統層級的攻擊中。

- **除非必要，否則請勿在生產環境中啟用嵌套。**

    如果您啟用了 [嵌套]，請勿在虛擬機器上執行不受支援的虛擬平臺。

針對更安全的環境：

- **使用具有信賴平臺模組的硬體 (TPM) 2.0 晶片來設定受防護的網狀架構。**

    如需詳細資訊，請參閱 [Windows Server 2016 上的 Hyper-v 系統需求](../system-requirements-for-hyper-v-on-windows.md)。

## <a name="secure-virtual-machines"></a>保護虛擬機器
- **為支援的客體作業系統建立第2代虛擬機器。**

    如需詳細資訊，請參閱 [第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。

- **啟用安全開機。**

    如需詳細資訊，請參閱 [第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md)。

- **讓客體作業系統保持安全。**

    - 在生產環境中開啟虛擬機器之前，請先安裝最新的安全性更新。
    - 針對需要的支援的客體作業系統安裝 integration services，並將其保持在最新狀態。 執行支援的 Windows 版本之來賓的整合服務更新可透過 Windows Update 取得。
    - 根據執行的角色，強化在每部虛擬機器中執行的作業系統。 使用 [Windows 安全性基準](/windows/device-security/windows-security-baselines)中所述的基準安全性設定建議。

- **使用安全的網路。**

    請確定虛擬網路介面卡連接到正確的虛擬交換器，並套用適當的安全性設定和限制。

- **將虛擬硬碟和快照集檔案儲存在安全的位置。**

- **安全裝置。**

    只設定虛擬機器所需的裝置。 除非您在特定情況下需要，否則請勿在生產環境中啟用離散裝置指派。 如果您啟用它，請務必只向受信任的廠商公開裝置。

- 根據虛擬機器角色，適當地在虛擬機器中**設定防毒軟體、防火牆和入侵偵測軟體**。

- **針對執行 Windows 10 或 Windows Server 2016 或更新版本的來賓啟用虛擬化安全性。**

    如需詳細資訊，請參閱 [Device Guard 部署指南](/windows/device-security/device-guard/device-guard-deployment-guide)。

- **只有在特定工作負載需要時，才啟用離散裝置指派**。

    由於傳遞實體裝置的本質，請與裝置製造商合作，瞭解是否應該在安全的環境中使用它。

針對更安全的環境：

- **部署啟用防護的虛擬機器，並將其部署到受防護網狀架構。**

    如需詳細資訊，請參閱 [第2代安全性設定](../learn-more/Generation-2-virtual-machine-security-settings-for-Hyper-V.md) 和受防護網狀 [架構](../../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node.md)。