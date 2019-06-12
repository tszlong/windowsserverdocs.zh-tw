---
title: 受防護網狀架構部署的快速入門
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: e060e052-39a0-4154-90bb-b97cc6dde68e
manager: dongill
author: justinha
ms.author: justinha
ms.technology: security-guarded-fabric
ms.date: 01/30/2019
ms.openlocfilehash: 8e1ef34370b1459cd55705bc0069b49a572de303
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447537"
---
# <a name="quick-start-for-guarded-fabric-deployment"></a>受防護網狀架構部署的快速入門

>適用於：Windows Server （半年通道），Windows Server 2016

本主題說明什麼是受防護網狀架構，其需求，以及部署程序的摘要。 如需詳細的部署步驟，請參閱 <<c0> [ 部署主機守護者服務的受防護主機和受防護的 Vm](https://technet.microsoft.com/windows-server-docs/security/guarded-fabric-shielded-vm/guarded-fabric-deploying-hgs-overview)。

想要看影片？ 請參閱 Microsoft Virtual Academy 課程[部署受防護的 Vm 和 Windows Server 2016 的受防護網狀架構](https://mva.microsoft.com/en-US/training-courses/deploying-shielded-vms-and-a-guarded-fabric-with-windows-server-2016-17131?l=WFLef7vUD_4604300474)。

## <a name="what-is-a-guarded-fabric"></a>什麼是受防護網狀架構

A_受防護網狀架構_可以保護針對檢查、 竊取與竄改，從主機上執行的惡意程式碼，以及從系統管理員的租用戶工作負載的 Windows Server 2016 HYPER-V 網狀架構。 這些虛擬化的租用戶工作負載 — 受保護的 rest 和飛行中 — 統稱_受防護的 Vm_。 

## <a name="what-are-the-requirements-for-a-guarded-fabric"></a>受防護網狀架構的需求有哪些？

受防護網狀架構的需求包括：

- **若要執行的位置受防護的 Vm，免於遭受惡意軟體。**

    這些稱為_受防護主機_。 
    受防護的主機是它們可以證明已知且信任的狀態在外部的授權單位，稱為 「 主機守護者服務 (HGS) 執行時，才可以執行受防護的 Vm 的 Windows Server 2016 Datacenter edition HYPER-V 主機。 
    HGS 是在 Windows Server 2016 中，新的伺服器角色，並通常會部署為一個三節點的叢集。 

- **若要確認主機的方式是處於狀況良好的狀態。**

    執行的 HGS_證明_、 其量值的受防護主機的健全狀況。

- **若要安全地發行到狀況良好的主機金鑰的程序。**

    執行的 HGS_金鑰保護和重要發行_，它用來釋放回到狀況良好的主機金鑰。

- **管理工具，來自動佈建的安全和裝載的受防護的 Vm。**

    （選擇性） 您可以將這些管理工具新增至受防護網狀架構中：

    - System Center 2016 Virtual Machine Manager (VMM)。 VMM 建議，因為它提供額外的管理超過您獲得使用只具備 HYPER-V 和受防護網狀架構工作負載的 PowerShell cmdlet 工具）。
    - System Center 2016 Service Provider Foundation (SPF)。 這是 API 層會與 Windows Azure Pack 和 VMM 中，使用 Windows Azure Pack 的必要條件。
    - Windows Azure Pack 提供良好的圖形化 web 介面來管理受防護網狀架構與受防護的 Vm。 

在實務上，其中一個決定必須由前端：[證明模式](guarded-fabric-and-shielded-vms.md#attestation-modes-in-the-guarded-fabric-solution)受防護網狀架構所使用。 方法有兩種 — 兩個互斥的模式，由其 HGS 可測量 HYPER-V 主機是狀況良好。 當您初始化 HGS 時，您需要選擇的模式：  

- 主機金鑰證明或金鑰的模式，是較不安全，但是您更輕鬆地採用  
- TPM 型證明或 TPM 模式較為安全，但需要更多組態和特定的硬體

如有必要，您可以在使用現有已升級至 Windows Server 2019 Datacenter edition 的 HYPER-V 主機的索引鍵模式中部署，並再轉換成更安全的 TPM 模式提供支援 （包括 TPM 2.0） 的伺服器硬體時。 

既然您知道項目，讓我們逐步部署模型的範例。

## <a name="how-to-get-from-a-current-hyper-v-fabric-to-a-guarded-fabric"></a>如何取得從目前的 HYPER-V 網狀架構中，對受防護網狀架構

我們假設這種情況下，您有現有 HYPER-V 網狀架構，例如在下圖中，Contoso.com，而您想要建置的 Windows Server 2016 受防護網狀架構。

![現有的 HYPER-V 網狀架構](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-existing-hyper-v.png)

## <a name="step-1-deploy-the-hyper-v-hosts-running-windows-server-2016"></a>步驟 1：部署執行 Windows Server 2016 的 HYPER-V 主機 

HYPER-V 主機必須執行 Windows Server 2016 Datacenter edition 或更新版本。 如果您要升級的主機，您可以[升級](https://technet.microsoft.com/windowsserver/dn527667.aspx)從 Datacenter edition 的 Standard edition。

![升級 HYPER-V 主機](../../security/media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-one-upgrade-hyper-v.png)

## <a name="step-2-deploy-the-host-guardian-service-hgs"></a>步驟 2：部署主機守護者服務 (HGS)

然後安裝 HGS 伺服器角色，並將它部署為三個節點叢集，如下列圖片中 relecloud.com 範例所示。 這需要三個 PowerShell cmdlet:

- 若要新增的 HGS 角色，請使用 `Install-WindowsFeature` 
- 若要安裝 HGS，使用 `Install-HgsServer` 
- 若要初始化 HGS 的證明您所選擇的模式，請使用 `Initialize-HgsServer` 

如果您現有的 HYPER-V 伺服器不符合 TPM 模式的必要條件 （例如，在沒有 TPM 2.0），您可以初始化 HGS 使用系統管理員為基礎證明 （AD 模式），而這需要與網狀架構之網域的 Active Directory 信任。 

在我們的範例，假設 Contoso 一開始立即符合 合規性需求，才能部署 AD 模式中，而且可以購買的適當的伺服器硬體之後，將轉換成更安全的 TPM 型證明計劃。 

![安裝 HGS](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-two-deploy-hgs.png)

## <a name="step-3-extract-identities-hardware-baselines-and-code-integrity-policies"></a>步驟 3：擷取身分識別、 硬體基準和程式碼完整性原則

從 HYPER-V 主機擷取身分識別的程序取決於所使用的證明模式。

對於 AD 模式中，主應用程式的識別碼。 其已加入網域的電腦帳戶，這必須是 fabric 網域中的指定之的安全性群組的成員
指定群組的成員資格是唯一判斷主機是否狀況良好。 

在此模式中，網狀架構系統管理員只負責確保 HYPER-V 主機的健全狀況。 HGS 不並決定什麼或不允許執行無任何作用，因為惡意程式碼和偵錯工具將會正常運作。 

不過，嘗試直接附加到處理程序 （例如 WinDbg.exe) 的偵錯工具會封鎖受防護的 vm，因為 VM 的背景工作處理序 (VMWP.exe) 是受保護的處理序燈 (PPL)。
替代偵錯技術的詳細資訊，例如所使用的 LiveKd.exe，不會封鎖。 不同於受防護的 Vm，支援加密的 Vm 背景工作處理序不會執行，因此傳統偵錯工具要 WinDbg.exe PPL 會繼續正常運作。

換句話說，使用 TPM 模式的嚴格的驗證步驟不會用於 AD 模式以任何方式。

TPM 模式中，三個項目則是必要項目： 

1.  A_公開簽署金鑰_(或_EKpub_) 從每個 HYPER-V 主機上的 TPM 2.0。 若要擷取 EKpub，使用`Get-PlatformIdentifier`。 
2.  A_硬體基準_。 如果每個 HYPER-V 主機的完全相同，則單一的基準。 您只需要 如果他們不這樣做，則您必須另一個用於硬體的每個類別。 高可信度電腦運算小組記錄檔，或 TCGlog 的格式為基準。 TCGlog 包含 UEFI 韌體，透過核心，至於其中完全啟動主機從主機執行的所有項目。 若要擷取硬體基準，安裝 HYPER-V 角色和 「 主機守護者 HYPER-V 支援功能，並使用`Get-HgsAttestationBaselinePolicy`。 
3.  A_程式碼完整性原則_。 如果每個 HYPER-V 主機的完全相同，則單一的 CI 原則。 您只需要 如果他們不這樣做，則您必須另一個用於硬體的每個類別。 Windows Server 2016 和 Windows 10 有新形式的 CI 原則，稱為 「 強制_hvci 施行 Hypervisor 程式碼完整性 （原則）_ 。 Hvci 原則提供增強的強制，並可確保，主機只允許執行信任的系統管理員允許它執行的二進位檔。 這些指示會包裝在新增至 HGS 的 CI 原則。 HGS 會測量每一部主機的 CI 原則之前它們要允許執行受防護的 Vm。 若要擷取的 CI 原則，請使用`New-CIPolicy`。 原則必須再轉換成其二進位格式使用`ConvertFrom-CIPolicy`。

![擷取身分識別、 基準和 CI 原則](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-three-extract-identity-baseline-ci-policy.png)

這就是，建立受防護網狀架構，來執行它的基礎結構。  
現在您可以建立受防護的 VM 範本磁碟和防護資料檔案，因此受防護的 Vm 可佈建輕鬆、 更安全。 

## <a name="step-4-create-a-template-for-shielded-vms"></a>步驟 4：為受防護的 Vm 建立範本

受防護的 VM 範本會建立在已知的可靠時間點的磁碟的簽章的時間用來保護的範本磁碟。 
如果範本磁碟稍後會感染了惡意軟體，其簽章將不同原始範本將會偵測到安全的受防護 VM 佈建程序。 
防護的範本磁碟藉由執行**受防護的範本磁碟建立精靈**或`Protect-TemplateDisk`對一般範本磁碟。 

每一個都是隨附**受防護的 VM 工具**功能[遠端伺服器管理工具適用於 Windows 10](https://www.microsoft.com/download/details.aspx?id=45520)。
下載 RSAT 之後，執行下列命令來安裝**受防護的 VM 工具**功能：

```powershell
Install-WindowsFeature RSAT-Shielded-VM-Tools -Restart
```

為可信任的系統管理員，例如網狀架構系統管理員或 VM 擁有者，需要 （通常是由主機服務提供者提供） 的憑證來簽署 VHDX 的範本磁碟。 

![受防護的範本磁碟精靈](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-shielded-template-wizard.png)

磁碟簽章是計算出虛擬磁碟的作業系統磁碟分割。
如果發生任何變更作業系統磁碟分割上，也會變更簽章。
這可讓使用者將強式識別其信任藉由指定適當的簽章的磁碟。

檢閱[範本磁碟需求](guarded-fabric-create-a-shielded-vm-template.md)開始之前。 

## <a name="step-5-create-a-shielding-data-file"></a>步驟 5：建立虛擬機器防護資料檔案 

防護資料檔案，也稱為 pdk 檔案，會擷取虛擬機器，例如系統管理員密碼的機密資訊。 

![虛擬機器防護資料](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-deployment-step-five-create-shielding-data.png)

防護資料檔案也包含受防護的 VM 的安全性原則設定。 當您建立防護資料檔案時，您必須選擇兩個安全性原則的其中一個：

- 受防護
   
    最安全選項，可消除許多系統管理的攻擊誘因。

- 支援加密

    較低層保護，仍然提供合規性優點是能夠將 VM 加密，但可讓 HYPER-V 系統管理員執行下列動作會使用 VM 主控台連線和 PowerShell Direct。 

    ![新的加密支援的 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-shielded-vm.png)

您可以新增選用的管理項目，例如 VMM 或 Windows Azure Pack。 如果您想要建立的 VM，而不需要安裝這些項目，請參閱 <<c0> [ 逐步解說： 建立受防護的 Vm 不含 VMM](https://blogs.technet.microsoft.com/datacentersecurity/2016/06/06/step-by-step-creating-shielded-vms-without-vmm/)。

## <a name="step-6-create-a-shielded-vm"></a>步驟 6：建立受防護的 VM

建立受防護的虛擬機器與很少從一般的虛擬機器。 在 Windows Azure Pack 中，會於建立一般的 VM，因為您只需要提供名稱，防護資料檔案 （包含其餘的特製化資訊） 和 VM 網路，甚至更簡單的體驗。 

![在 Windows Azure Pack 中的新受防護的 VM](../media/Guarded-Fabric-Shielded-VM/guarded-fabric-new-vm-config.png)

## <a name="next-step"></a>後續步驟

> [!div class="nextstepaction"]
> [HGS 必要條件](guarded-fabric-prepare-for-hgs.md)
