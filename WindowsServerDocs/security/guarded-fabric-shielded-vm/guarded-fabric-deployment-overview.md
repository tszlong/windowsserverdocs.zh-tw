---
description: 深入瞭解：受防護網狀架構部署的快速入門
title: 受防護網狀架構部署的快速入門
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.date: 01/30/2019
ms.openlocfilehash: 0f91d156797fdac9e7c724d663fdcc6485c77903
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97047466"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>受防護網狀架構部署的快速入門

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明什麼是受防護網狀架構、其需求，以及部署程式的摘要。
如需詳細的部署步驟，請參閱 [部署受防護主機和受防護 vm 的主機守護者服務](./guarded-fabric-deploying-hgs-overview.md)。

想要看影片？ 請參閱 Microsoft 虛擬學術課程： [使用 Windows Server 2016 部署受防護的 vm 和受防護的網狀架構](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)。

## <a name="what-is-a-guarded-fabric"></a>什麼是受防護網狀架構

受防護網狀 _架構_ 是一種 Windows Server 2016 hyper-v 網狀架構，能夠保護租使用者工作負載免于檢查、遭竊和篡改主機上執行的惡意程式碼，以及系統管理員。
這些虛擬化的租使用者工作負載（同時保護）稱為 _受防護的 vm_。

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>受防護網狀架構的需求為何

受防護網狀架構的需求包括：

- **可從惡意軟體免費執行受防護 Vm 的位置。**

    這些稱為 _受防護主機_。
    受防護主機是 Windows Server 2016 Datacenter edition 的 Hyper-v 主機，只有在可以證明它們是以已知、受信任狀態執行，並將其稱為主機守護者服務 (HGS) 的外部授權單位時，才能執行受防護的 Vm。
    HGS 是 Windows Server 2016 中的新伺服器角色，通常會部署為三個節點的叢集。

- **驗證主機處於狀況良好狀態的方式。**

    HGS 會執行 _證明_，它會測量受保護主機的健康情況。

- **將金鑰安全地釋放至狀況良好主機的程式。**

    HGS 會執行 _金鑰保護和金鑰釋放_，將金鑰釋放回狀況良好的主機。

- **管理工具，可自動化受防護 Vm 的安全布建和裝載。**

    （選擇性）您可以將這些管理工具新增至受防護網狀架構：

    - System Center 2016 Virtual Machine Manager (VMM) 。 建議使用 VMM，因為它提供的其他管理工具，不只是使用 Hyper-v 隨附的 PowerShell Cmdlet 以及) 的受防護網狀架構工作負載。
    - System Center 2016 Service Provider Foundation (SPF) 。 這是 Windows Azure 套件和 VMM 之間的 API 層，以及使用 Windows Azure 套件的先決條件。
    - Windows Azure 套件提供良好的圖形化 web 介面，以管理受防護的網狀架構和受防護的 Vm。

在實務上，必須先進行一項決策：受防護網狀架構使用的 [證明模式](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution) 。
有兩種方法：兩個互斥的模式，而 HGS 可以測量 Hyper-v 主機的健康情況。
當您初始化 HGS 時，您需要選擇模式：

- 主機金鑰證明或金鑰模式較不安全，但更容易採用
- 以 TPM 為基礎的證明或 TPM 模式比較安全，但需要更多設定和特定硬體

如有必要，您可以使用已升級至 Windows Server 2019 Datacenter edition 的現有 Hyper-v 主機，在金鑰模式中進行部署，然後在支援伺服器硬體 (（包括 TPM 2.0) 可供使用）時，轉換成更安全的 TPM 模式。

既然您已經知道各部分，讓我們逐步解說部署模型的範例。

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>如何從目前的 Hyper-v 網狀架構進入受防護網狀架構

讓我們假設您有現有的 Hyper-v 網狀架構，例如下圖中的 Contoso.com，而且您想要建立 Windows Server 2016 受防護網狀架構。

![現有的 Hyper-v 網狀架構](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>步驟1：部署執行 Windows Server 2016 的 Hyper-v 主機

Hyper-v 主機必須執行 Windows Server 2016 Datacenter edition 或更新版本。 如果您要升級主機，您可以從 Standard edition [升級](../../get-started/installation-and-upgrade.md) 至 Datacenter edition。

![升級 Hyper-v 主機](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>步驟2：將主機守護者服務部署 (HGS) 

然後安裝 HGS 伺服器角色，並將其部署為三個節點的叢集，例如下圖中的 relecloud.com 範例。
這需要三個 PowerShell Cmdlet：

- 若要新增 HGS 角色，請使用 `Install-WindowsFeature`
- 若要安裝 HGS，請使用 `Install-HgsServer`
- 若要使用您所選擇的證明模式初始化 HGS，請使用 `Initialize-HgsServer`

如果您現有的 Hyper-v 伺服器不符合 TPM 模式的必要條件 (例如，它們沒有 TPM 2.0) ，您可以使用以系統管理員為基礎的證明 (AD 模式) （需要與網狀架構網域 Active Directory 信任）來初始化 HGS。

在我們的範例中，假設 Contoso 一開始是以 AD 模式進行部署，以立即符合合規性需求，並計畫在可以購買適當的伺服器硬體之後，轉換成更安全的 TPM 型證明。

![安裝 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>步驟3：將身分識別、硬體基準和程式碼完整性原則解壓縮

從 Hyper-v 主機解壓縮身分識別的程式，取決於所使用的證明模式。

在 AD 模式中，主機的識別碼是已加入網域的電腦帳戶，必須是網狀架構網域中指定之安全性群組的成員。
指定群組的成員資格是判斷主機是否狀況良好的唯一。

在此模式中，網狀架構系統管理員僅負責確保 Hyper-v 主機的健康狀態。
因為 HGS 沒有任何部分可決定要執行什麼或不能執行，所以惡意程式碼和偵錯工具會依照設計的方式運作。

不過，嘗試直接附加至進程 (（例如 WinDbg.exe) ）的偵錯工具會封鎖受防護的 Vm，因為 VM 的背景工作進程 ( # A1) 是受保護的進程淺 (PPL) 。
替代的偵錯工具（例如 LiveKd.exe 所使用的技術）不會遭到封鎖。
與受防護的 Vm 不同的是，加密支援的 Vm 的背景工作進程不會以 PPL 的形式執行，因此 WinDbg.exe 之類的傳統偵錯工具會繼續正常運作。

以另一種方式陳述，用於 TPM 模式的嚴格驗證步驟不會以任何方式用於 AD 模式。

針對 TPM 模式，需要三個專案：

1.    _公開簽署金鑰_ (，或從每個 hyper-v 主機上的 TPM 2.0) _EKpub_ 。 若要捕獲 EKpub，請使用 `Get-PlatformIdentifier` 。
2.    _硬體基準_。 如果您的 Hyper-v 主機全都相同，則您只需要一個基準。 如果不是，則每個硬體類別都需要一個。 基準的形式為可信賴的計算群組記錄檔或 TCGlog。 TCGlog 包含主機從 UEFI 固件到核心的所有專案，最高到主機完全開機的位置。 若要捕獲硬體基準，請安裝 Hyper-v 角色與主機守護者 Hyper-v 支援功能，並使用 `Get-HgsAttestationBaselinePolicy` 。
3.    程式 _代碼完整性原則_。 如果您的 Hyper-v 主機全都相同，則您只需要單一 CI 原則。 如果不是，則每個硬體類別都需要一個。 Windows Server 2016 和 Windows 10 都有新形式的 CI 原則強制執行，稱為「 _程式碼完整性」 (HVCI)_。 HVCI 可提供強式強制執行，並確保主機只能執行受信任系統管理員允許它執行的二進位檔。 這些指示會包裝在加入 HGS 的 CI 原則中。 HGS 會測量每個主機的 CI 原則，然後才允許它們執行受防護的 Vm。 若要捕捉 CI 原則，請使用 `New-CIPolicy` 。 接著，必須使用將原則轉換成其二進位格式 `ConvertFrom-CIPolicy` 。

![將身分識別、基準和 CI 原則解壓縮](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

這就是-根據執行的基礎結構來建立受防護的網狀架構。
現在您可以建立受防護的 VM 範本磁片和防護資料檔案，讓受防護的 Vm 可以簡單且安全的方式布建。

## <a name="step-4-create-a-template-for-shielded-vms"></a>步驟4：建立受防護 Vm 的範本

受防護的 VM 範本會在已知的可信賴時間點建立磁片的簽章，以保護範本磁片。 如果範本磁片之後受到惡意程式碼感染，則其簽章將會與安全受防護的 VM 布建程式所偵測到的原始範本不同。 受防護的範本磁片是藉由執行 **受防護的範本磁片建立嚮導** 或 `Protect-TemplateDisk` 一般範本磁片所建立。

每個都包含在 [Windows 10 的遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)中 **受防護的 VM 工具** 功能。
下載 RSAT 之後，請執行此命令來安裝 **受防護的 VM 工具** 功能：

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

信任的系統管理員（例如網狀架構系統管理員或 VM 擁有者）將需要一個憑證 (通常由主機服務提供者提供，) 簽署 VHDX 範本磁片。

![受防護的範本磁片嚮導](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

磁片簽章是透過虛擬磁片的 OS 磁碟分割來計算。
如果作業系統分割區有任何變更，簽章也會變更。
這可讓使用者藉由指定適當的簽章，以強識別所信任的磁片。

開始之前，請先檢查 [範本磁片需求](guarded-fabric-create-a-shielded-vm-template.md) 。

## <a name="step-5-create-a-shielding-data-file"></a>步驟5：建立防護資料檔案

防護資料檔案（也稱為 pdk 檔案）會捕捉虛擬機器的相關機密資訊，例如系統管理員密碼。

![防護資料](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

防護資料檔案也包含受防護 VM 的安全性原則設定。 當您建立防護資料檔案時，必須選擇兩個安全性原則的其中一種：

- 受防護

    最安全的選項，可排除許多系統管理攻擊媒介。

- 支援加密

    較低層級的保護仍提供可加密 VM 的合規性優點，但可讓 Hyper-v 系統管理員進行像是使用 VM 主控台連線和 PowerShell Direct 等作業。

    ![新的加密支援 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

您可以新增選用的管理元件，例如 VMM 或 Windows Azure 套件。 如果您想要在不安裝這些元件的情況下建立 VM，請參閱 [逐步解說-建立受防護的 vm （不使用 VMM](/archive/blogs/datacentersecurity/step-by-step-creating-shielded-vms-without-vmm)）。

## <a name="step-6-create-a-shielded-vm"></a>步驟6：建立受防護的 VM

建立受防護的虛擬機器與一般虛擬機器的差異很小。
在 Windows Azure 套件中，此體驗比建立一般 VM 更為容易，因為您只需要提供名稱、防護資料檔案 (包含) 和 VM 網路的其餘特製化資訊。

![Windows Azure 套件中的新受防護 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [HGS 必要條件](guarded-fabric-prepare-for-hgs.md)
