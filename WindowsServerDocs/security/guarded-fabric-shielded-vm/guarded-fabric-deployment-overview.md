---
title: 針對受防護網狀架構部署快速入門
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.date: 01/30/2019
ms.openlocfilehash: 8ddd4699358a6725ed5e2f80683a363a1120caf7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944248"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>針對受防護網狀架構部署快速入門

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明什麼是受防護的網狀架構、其需求，以及部署程式的摘要。
如需詳細的部署步驟，請參閱為[受防護的主機和受防護的 Vm 部署主機守護者服務](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)。

想要看影片？ 請參閱 Microsoft 虛擬學院課程[部署受防護的 vm 和具有 Windows Server 2016 的受保護網狀架構](https://mva.microsoft.com/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)。

## <a name="what-is-a-guarded-fabric"></a>什麼是受防護的網狀架構

受防護的網狀_架構_是一種 Windows Server 2016 hyper-v 網狀架構，能夠保護租使用者工作負載，以防止在主機上執行的惡意程式碼，以及系統管理員進行檢查、遭竊及篡改。
這些虛擬化的租使用者工作負載（待用或進行中的保護）稱為受_防護的 vm_。

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>受防護網狀架構的需求為何

受防護網狀架構的需求包括：

- **一種可執行受防護的 Vm 的位置，並不會受到惡意軟體的攻擊。**

    這些稱為_受防護主機_。
    受防護主機是 Windows Server 2016 Datacenter edition 的 Hyper-v 主機，只有在可以證明它們是在已知的受信任狀態下執行，才能將其識別為主機守護者服務 (HGS) 的外部授權單位。
    HGS 是 Windows Server 2016 中的新伺服器角色，而且通常會部署為三個節點的叢集。

- **確認主機處於狀況良好狀態的方式。**

    HGS 會執行_證明_，其中測量受防護主機的健全狀況。

- **安全地將金鑰發行到狀況良好主機的程式。**

    HGS 會執行_金鑰保護和金鑰釋放_，讓金鑰釋放回狀況良好的主機。

- **可將受防護 Vm 的安全布建和裝載自動化的管理工具。**

    （選擇性）您可以將這些管理工具新增至受防護的網狀架構：

    - System Center 2016 Virtual Machine Manager (VMM) 。 建議使用 VMM，因為它會提供其他管理工具，而不只是使用 Hyper-v 隨附的 PowerShell Cmdlet 和受防護網狀架構工作負載) 。
    - System Center 2016 Service Provider Foundation (SPF) 。 這是 Windows Azure 套件與 VMM 之間的 API 層，以及使用 Windows Azure 套件的必要條件。
    - Windows Azure 套件提供良好的圖形化 web 介面來管理受防護網狀架構與受防護的 Vm。

在實務上，必須事先決定一個決策：受防護網狀架構所使用的[證明模式](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution)。
有兩種方法：兩個互斥的模式，HGS 可以測量 Hyper-v 主機的狀況是否良好。
當您初始化 HGS 時，您需要選擇模式：

- 主機金鑰證明或金鑰模式較不安全，但更容易採用
- 以 TPM 為基礎的證明或 TPM 模式較安全，但需要更多設定和特定硬體

如有需要，您可以使用已升級至 Windows Server 2019 Datacenter edition 的現有 Hyper-v 主機在金鑰模式中部署，然後在支援伺服器硬體 (包括 TPM 2.0) 可用時，轉換為更安全的 TPM 模式。

既然您已經知道各部分，讓我們逐步解說部署模型的範例。

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>如何從目前的 Hyper-v 網狀架構取得受防護的網狀架構

假設這種情況，您有現有的 Hyper-v 網狀架構，例如下圖中的 Contoso.com，而且您想要建立 Windows Server 2016 的受防護網狀架構。

![現有的 Hyper-v 網狀架構](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>步驟1：部署執行 Windows Server 2016 的 Hyper-v 主機

Hyper-v 主機必須執行 Windows Server 2016 Datacenter edition 或更新版本。 如果您要升級主機，可以從 Standard edition[升級](https://technet.microsoft.com/windowsserver/dn527667.aspx)到 Datacenter edition。

![升級 Hyper-v 主機](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>步驟2：將主機守護者服務部署 (HGS) 

然後安裝 HGS 伺服器角色，並將它部署為三個節點的叢集，例如下圖中的 relecloud.com 範例。
這需要三個 PowerShell Cmdlet：

- 若要新增 HGS 角色，請使用`Install-WindowsFeature`
- 若要安裝 HGS，請使用`Install-HgsServer`
- 若要使用您選擇的證明模式來初始化 HGS，請使用`Initialize-HgsServer`

如果您現有的 Hyper-v 伺服器不符合 TPM 模式的必要條件 (例如，它們沒有 TPM 2.0) ，您可以使用以系統管理員為基礎的證明 (AD 模式) 來初始化 HGS，這需要與網狀架構網域 Active Directory 的信任。

在我們的範例中，假設 Contoso 一開始會以 AD 模式部署，以便立即符合合規性需求，並規劃在購買適當的伺服器硬體之後，轉換成更安全的 TPM 型證明。

![安裝 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>步驟3：解壓縮身分識別、硬體基準和程式碼完整性原則

從 Hyper-v 主機解壓縮身分識別的程式，取決於所使用的證明模式。

就 AD 模式而言，主機的識別碼是已加入網域的電腦帳戶，必須是網狀架構網域中指定之安全性群組的成員。
指定群組中的成員資格是唯一判斷主機是否狀況良好。

在此模式中，網狀架構系統管理員會獨自負責確保 Hyper-v 主機的健全狀況。
由於 HGS 在決定不允許執行的情況下沒有任何部分，因此惡意程式碼和偵錯工具將會以設計的方式運作。

不過，嘗試直接附加至進程 (例如 WinDbg.exe) 的偵錯工具會封鎖受防護的 Vm，因為 VM 的背景工作進程 ( # A1) 是受保護的進程 light (PPL) 。
替代的調試技術（例如 LiveKd.exe 所使用的）不會遭到封鎖。
不同于受防護的 Vm，加密支援的 Vm 的工作者進程不會以 PPL 的形式執行，因此傳統的偵錯工具（例如 WinDbg.exe）將會繼續正常運作。

以另一種方式表示，用於 TPM 模式的嚴格驗證步驟不會以任何方式用於 AD 模式。

針對 TPM 模式，需要三件事：

1.    _公用簽署金鑰_ (或_EKpub_從每個 HYPER-V 主機上的 TPM 2.0) 。 若要捕捉 EKpub，請使用 `Get-PlatformIdentifier` 。
2.    _硬體基準_。 如果您的每一部 Hyper-v 主機都相同，則您只需要單一基準。如果不是，則每個硬體類別都需要一個。 基準的形式為可信任的運算群組記錄檔，或 TCGlog。 此 TCGlog 包含主機從 UEFI 固件到核心為止的所有專案，最高可由主機完全啟動。 若要捕獲硬體基準，請安裝 Hyper-v 角色和主機守護者 Hyper-v 支援功能並使用 `Get-HgsAttestationBaselinePolicy` 。
3.    程式_代碼完整性原則_。 如果您的每一部 Hyper-v 主機都相同，則您只需要單一 CI 原則。 如果不是，則每個硬體類別都需要一個。 Windows Server 2016 和 Windows 10 都有新的 CI 原則形式強制執行，稱為「_虛擬程式碼完整性」 (HVCI) _。 HVCI 提供強式強制執行，並確保主機只能執行受信任的系統管理員允許它執行的二進位檔。 這些指示會包裝在新增至 HGS 的 CI 原則中。 HGS 會測量每個主機的 CI 原則，然後才允許它們執行受防護的 Vm。 若要捕獲 CI 原則，請使用 `New-CIPolicy` 。 接著，必須使用將原則轉換成其二進位格式 `ConvertFrom-CIPolicy` 。

![解壓縮身分識別、基準和 CI 原則](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

這就是建立受防護網狀架構的基礎結構，也就是要執行它的基礎結構。
現在您可以建立受防護的 VM 範本磁片和防護資料檔案，讓受防護的 Vm 可以簡單且安全的方式布建。

## <a name="step-4-create-a-template-for-shielded-vms"></a>步驟4：建立受防護 Vm 的範本

受防護的 VM 範本會藉由在已知的可信任時間點建立磁片的簽章，來保護範本磁片。 
如果範本磁片稍後受到惡意程式碼感染，其簽章將會不同于安全受防護的 VM 布建程式所偵測到的原始範本。 
受防護的範本磁片是藉由執行 [**受防護的範本磁片建立嚮導]** 或 `Protect-TemplateDisk` 針對一般範本磁片來建立。

每個都包含在[適用于 Windows 10 的遠端伺服器管理工具](https://www.microsoft.com/download/details.aspx?id=45520)中**受防護的 VM 工具**功能。
下載 RSAT 之後，請執行此命令來安裝**受防護的 VM 工具**功能：

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

值得信任的系統管理員（例如網狀架構系統管理員或 VM 擁有者）將需要一個憑證， (通常由主機服務提供者提供，) 來簽署 VHDX 範本磁片。

![受防護的範本磁片 wizard](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

磁片簽章是透過虛擬磁片的 OS 磁碟分割來計算。
如果 OS 分割區上有任何變更，簽章也會變更。
這可讓使用者藉由指定適當的簽章，來強識別所信任的磁片。

開始之前，請先參閱[範本磁片需求](guarded-fabric-create-a-shielded-vm-template.md)。

## <a name="step-5-create-a-shielding-data-file"></a>步驟5：建立防護資料檔案

防護資料檔案（也稱為 pdk 檔案）會捕獲有關虛擬機器的機密資訊，例如系統管理員密碼。

![防護資料](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

防護資料檔案也包含受防護 VM 的安全性原則設定。 當您建立防護資料檔案時，必須選擇兩個安全性原則的其中一個：

- 受防護

    最安全的選項，可排除許多系統管理攻擊的媒介。

- 支援加密

    較低層級的保護仍然提供可加密 VM 的相容性優勢，但可讓 Hyper-v 系統管理員執行像是使用 VM 主控台連線和 PowerShell Direct 之類的動作。

    ![支援新的加密 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

您可以新增選用的管理元件，例如 VMM 或 Windows Azure 套件。 如果您想要在不安裝這些部分的情況下建立 VM，請參閱[逐步執行–不使用 VMM 建立受防護的 vm](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/)。

## <a name="step-6-create-a-shielded-vm"></a>步驟6：建立受防護的 VM

建立受防護的虛擬機器與一般虛擬機器的差異很小。
在 Windows Azure 套件中，體驗甚至會比建立一般 VM 更為容易，因為您只需要提供名稱、防護資料檔案 (包含其餘的特製化資訊) 和 VM 網路。

![Windows Azure 套件中的新受防護 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [HGS 必要條件](guarded-fabric-prepare-for-hgs.md)
