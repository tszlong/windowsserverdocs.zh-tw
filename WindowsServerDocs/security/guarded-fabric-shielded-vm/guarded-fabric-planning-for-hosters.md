---
description: 深入瞭解：適用于主控者的受防護網狀架構與受防護 VM 規劃指南
title: 適用于主控者的受防護網狀架構與受防護 VM 規劃指南
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.author: nirb
ms.date: 08/29/2018
ms.openlocfilehash: 58d29de19959bad617c9db3e07c12b316624056c
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97043946"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>適用于主控者的受防護網狀架構與受防護 VM 規劃指南

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本主題涵蓋的規劃決策將必須進行，才能讓受防護的虛擬機器在網狀架構上執行。 無論您是升級現有的 Hyper-v 網狀架構，還是建立新的網狀架構，執行受防護的 Vm 都是由兩個主要元件所組成：

- 主機守護者服務 (HGS) 提供證明和金鑰保護，讓您可以確定受防護的 Vm 只會在已核准且健康的 Hyper-v 主機上執行。 
- 已核准且健康良好的 Hyper-v 主機，受防護的 Vm (和一般 Vm) 可執行，這些都稱為受防護主機。

![HGS 和受防護主機](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>決策 #1：網狀架構中的信任層級

您如何實行主機守護者服務和受防護的 Hyper-v 主機，主要取決於您想要在網狀架構中達成的信任強度。 信任強度受證明模式控管。 有兩個互斥的選項：

1. TPM-信任的證明

    如果您的目標是要協助保護虛擬機器免于惡意管理員或遭入侵的網狀架構，則您將使用受 TPM 信任的證明。 此選項適用于多租使用者裝載案例，以及企業環境中的高價值資產（例如，SQL 或 SharePoint 等網域控制站或內容伺服器）。
    受管理程式保護的程式碼完整性 (HVCI) 原則，並在允許主機執行受防護的 Vm 之前，由 HGS 強制執行它們的有效性。

2. 主機金鑰證明

    如果您的需求主要是透過合規性驅動，而這需要在待用及進行中加密虛擬機器，則您將使用主機金鑰證明。 此選項適用于一般用途的資料中心，您可以在其中熟悉 Hyper-v 主機和網狀架構系統管理員，讓您能夠存取虛擬機器的客體作業系統，以進行日常維護和操作。

    在此模式中，網狀架構系統管理員僅負責確保 Hyper-v 主機的健康狀態。
    因為 HGS 沒有任何部分可決定要執行什麼或不能執行，所以惡意程式碼和偵錯工具會依照設計的方式運作。

    不過，嘗試直接附加至進程 (（例如 WinDbg.exe) ）的偵錯工具會封鎖受防護的 Vm，因為 VM 的背景工作進程 ( # A1) 是受保護的進程淺 (PPL) 。
    替代的偵錯工具（例如 LiveKd.exe 所使用的技術）不會遭到封鎖。
    與受防護的 Vm 不同的是，加密支援的 Vm 的背景工作進程不會以 PPL 的形式執行，因此 WinDbg.exe 之類的傳統偵錯工具會繼續正常運作。

    從 Windows Server 2019 開始，名為「管理-信任證明」 (Active Directory 型) 的類似證明模式已被取代。

您選擇的信任層級將會指定您的 Hyper-v 主機的硬體需求，以及您在網狀架構上套用的原則。 如有必要，您可以使用現有的硬體和系統管理員信任的證明來部署受防護的網狀架構，然後在硬體已升級且您需要強化網狀架構安全性時，將其轉換成受 TPM 信任的證明。

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>決策 #2：現有的 Hyper-v 網狀架構與新的不同 Hyper-v 網狀架構

如果您有現有的網狀架構 (Hyper-v 或) ，很可能您可以使用它來執行受防護的 Vm 以及一般 Vm。 某些客戶會選擇將受防護的 Vm 整合至其現有的工具和網狀架構中，而其他客戶則會根據商務原因來分隔網狀架構

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>主機守護者服務的 HGS 系統管理員規劃

在高度安全的環境中 (HGS) 部署主機守護者服務，無論是在專用的實體伺服器、受防護的 VM、隔離的 Hyper-v 主機上的 VM， (與保護) 的網狀架構分開，或使用不同的 Azure 訂用帳戶以邏輯方式區隔。

| 區域 | 詳細資料 |
|------|---------|
| 安裝需求 | <ul><li>一部伺服器 (針對高可用性建議的三節點叢集) </li><li>針對 fallback，至少需要兩部 HGS 伺服器</li><li>伺服器可以是建議使用 TPM 2.0 的虛擬或實體 (實體伺服器;也支援 TPM 1.2) </li><li>Windows Server 2016 或更新版本的 Server Core 安裝</li><li>允許 HTTP 或[fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)設定的網狀架構網路線</li><li>建議存取驗證的 HTTPS 憑證</li></ul> |
| 調整大小 | 每個中型 (8 核心/4 GB) HGS 伺服器節點都可以處理 1000 Hyper-v 主機。 |
| 管理 | 指定將管理 HGS 的特定人員。 它們應該與網狀架構系統管理員分開。 為了進行比較，您可以利用與憑證授權單位單位相同的方式來考慮 HGS 叢集 (CA) 管理隔離、實體部署和整體安全性敏感度層級。 |
| 主機守護者服務 Active Directory | 根據預設，HGS 會安裝自己的內部 Active Directory 進行管理。 這是獨立、自我管理的樹系，而且是建議的設定，可協助隔離 HGS 與您的網狀架構。<br><br>如果您已經有要用於隔離的高度許可權 Active Directory 樹系，您可以使用該樹系，而不是 HGS 預設樹系。 如果 HGS 未加入與 Hyper-v 主機或網狀架構管理工具相同之樹系中的網域，就很重要。 這樣做可能會讓網狀架構系統管理員能夠掌控 HGS。 |
| 災害復原 | 選項有三個：<br><ol><li>在每個資料中心安裝個別的 HGS 叢集，並授權受防護的 Vm 在主要和備份資料中心內執行。 這可避免跨 WAN 延伸叢集的需求，並可讓您將虛擬機器隔離，使其只在其指定的網站中執行。</li><li>在兩個 (或更多) 資料中心之間的延展叢集上安裝 HGS。 這可在 WAN 中斷時提供復原功能，但會推送容錯移轉叢集的限制。 您無法將工作負載隔離到一個網站;授權在一個網站中執行的 VM 可以在任何其他網站上執行。</li><li>向另一個 HGS 註冊 Hyper-v 主機做為容錯移轉。</li></ol>您也應該匯出其設定來備份每個 HGS，如此一來，您就可以隨時在本機復原。 如需詳細資訊，請參閱 [Export-HgsServerState](/powershell/module/hgsserver/export-hgsserverstate) 和 [Import-HgsServerState](/powershell/module/hgsserver/import-hgsserverstate)。 |
| 主機守護者服務金鑰 | 主機守護者服務會使用兩個非對稱金鑰組（加密金鑰和簽署金鑰），每個金鑰都以 SSL 憑證表示。 有兩個選項可以產生這些金鑰：<br><ol><li>內部憑證授權單位單位–您可以使用內部 PKI 基礎結構來產生這些金鑰。 這適用于資料中心環境。</li><li>公開信任的憑證授權單位單位–使用一組從公開信任的憑證授權單位單位取得的金鑰。 這是主控者應該使用的選項。</li></ol>請注意，雖然您可以使用自我簽署憑證，但不建議用於概念證明實驗室以外的部署案例。<br><br>除了擁有 HGS 金鑰，主機服務提供者可以使用「自備金鑰」，其中租使用者可以提供自己的金鑰，讓某些 (或所有) 租使用者都可以有自己的特定 HGS 金鑰。 此選項適用于可為租使用者提供頻外程式來上傳其金鑰的主控者。 |
| 主機守護者服務金鑰儲存空間 | 為了獲得最強的安全性，建議您建立 HGS 金鑰，並以獨佔方式儲存在硬體安全性模組 (HSM) 。 如果您不是使用 Hsm，強烈建議您在 HGS 伺服器上套用 BitLocker。 |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>受防護主機的網狀架構系統管理員規劃

| 區域 | 詳細資料 |
|------|---------|
| 硬體 | <ul><li>主機金鑰證明：您可以使用任何現有的硬體作為受防護主機。 有幾個例外 (可確保您的主機可以使用從 Windows Server 2016 開始的新安全性機制，請參閱 [使用 Windows server 2016 虛擬化型的程式碼完整性保護的相容硬體](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。</li><li>受 TPM 信任的證明：您可以使用具有 [硬體保證額外限定](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance) 性的任何硬體，只要它已適當地設定 (請參閱 [與受防護的 Vm 相容的伺服器設定，以及特定設定) 的程式碼完整性的虛擬化保護](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md) 。 這包括 TPM 2.0 和 UEFI 版本 2.3.1 c 和更新版本。</li></ul> |
| OS | 建議您針對 Hyper-v 主機 OS 使用 Server Core 選項。 |
| 效能影響 | 根據效能測試，我們預期執行受防護的 Vm 和未受防護的 Vm 之間有大約5% 的密度差異。 這表示，如果指定的 Hyper-v 主機可以執行20個未受防護的 Vm，我們會預期它可以執行19個受防護的 Vm。<br><br>請務必確認您的一般工作負載是否調整大小。 例如，可能有一些具有大量寫入導向 IO 工作負載的極端值，會進一步影響密度差異。 |
| 分公司考量 | 從 Windows Server 1709 版開始，您可以針對在本機執行的虛擬 HGS 伺服器，為分公司中受防護的 VM 指定一個 fallback URL。 當分公司失去與資料中心中 HGS 伺服器的連線能力時，就可以使用 fallback URL。 在舊版 Windows Server 上，在分公司執行的 Hyper-v 主機必須連線至主機守護者服務，才能開啟電源或即時移轉受防護的 Vm。 如需詳細資訊，請參閱 [分公司考慮](guarded-fabric-manage-branch-office.md)。 |
