---
title: 適用于主控者的受防護網狀架構與受防護的 VM 規劃指南
ms.prod: windows-server
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.author: nirb
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2e64f8a43318f10db3bfcb604adcef4b0bdc9837
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856511"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>適用于主控者的受防護網狀架構與受防護的 VM 規劃指南

>適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

本主題涵蓋的規劃決策，將必須進行，才能讓受防護的虛擬機器在您的網狀架構上執行。 無論您是升級現有的 Hyper-v 網狀架構，或是建立新的網狀架構，執行受防護的 Vm 都是由兩個主要元件所組成：

- 主機守護者服務（HGS）提供證明和金鑰保護，讓您可以確保受防護的 Vm 只會在已核准且狀況良好的 Hyper-v 主機上執行。 
- 已核准且健康狀態良好的 Hyper-v 主機，受防護的 Vm （和一般 Vm）可在其上執行，也就是所謂的受防護主機。

![HGS 和受防護主機](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>決策 #1：網狀架構中的信任層級

您如何執行主機守護者服務和受防護的 Hyper-v 主機，主要取決於您想要在網狀架構中達成的信任強度。 信任的強度是由證明模式所控制。 有兩個互斥選項：

1. TPM 信任證明

    如果您的目標是要協助保護虛擬機器免于惡意的系統管理員或遭入侵的網狀架構，則您將使用 TPM 信任的證明。 此選項適用于多租使用者裝載案例以及企業環境中高價值的資產，例如網域控制站或 SQL 或 SharePoint 等內容伺服器。
    在允許主機執行受防護的 Vm 之前，會測量以系統管理器保護的程式碼完整性（HVCI）原則，並由 HGS 強制執行其有效性。 

2. 主機金鑰證明

    如果您的需求主要是由需要在待用和進行中加密虛擬機器的合規性來驅動，則您將使用主機金鑰證明。 此選項適用于一般用途的資料中心，您可以使用 Hyper-v 主機和網狀架構系統管理員存取虛擬機器的客體作業系統，以進行日常維護和作業。 

    在此模式中，網狀架構系統管理員會獨自負責確保 Hyper-v 主機的健全狀況。 
    由於 HGS 在決定不允許執行的情況下沒有任何部分，因此惡意程式碼和偵錯工具將會以設計的方式運作。 
    
    不過，嘗試直接附加至進程的偵錯工具（例如 WinDbg）會封鎖受防護的 Vm，因為 VM 的背景工作進程（VMWP .exe）是受保護的進程光源（PPL）。 
    替代的偵錯工具技術（例如 LiveKd 所使用的）不會遭到封鎖。 
    不同于受防護的 Vm，加密支援的 Vm 的工作者進程並不會以 PPL 的形式執行，因此 WinDbg 等傳統偵錯工具將會繼續正常運作。

    從 Windows Server 2019 開始已淘汰名為 [系統管理員信任的證明（以 Active Directory 為基礎）] 的類似證明模式。 

您選擇的信任層級將會指示 Hyper-v 主機的硬體需求，以及您在網狀架構上套用的原則。 如有需要，您可以使用現有的硬體和系統管理員信任的證明來部署受防護的網狀架構，然後在硬體已升級且您需要強化網狀架構安全性時，將它轉換成受 TPM 信任的證明。

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>決策 #2：現有的 Hyper-v 網狀架構與新的個別 Hyper-v 網狀架構

如果您有現有的網狀架構（Hyper-v 或其他），您很可能會使用它來執行受防護的 Vm 以及一般 Vm。 有些客戶選擇將受防護的 Vm 整合到現有的工具和網狀架構中，而有些則因應商業理由而將其隔離。

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>主機守護者服務的 HGS 系統管理員規劃

在高度安全的環境中部署主機守護者服務（HGS），無論是在專用的實體伺服器、受防護的 VM、隔離的 Hyper-v 主機上的 VM （與它所保護的網狀架構分開），或是使用不同的 Azure 訂用帳戶以邏輯方式分隔的一個。   

| 區域 | 詳細資料 |
|------|---------|
| 安裝需求 | <ul><li>一部伺服器（建議用於高可用性的三節點叢集）</li><li>針對 fallback，至少需要兩部 HGS 伺服器</li><li>伺服器可以是虛擬或實體（建議使用 TPM 2.0 的實體伺服器;也支援 TPM 1.2）</li><li>Windows Server 2016 或更新版本的 Server Core 安裝</li><li>允許 HTTP 或[fallback](guarded-fabric-manage-branch-office.md#fallback-configuration)設定的網狀架構網路線</li><li>建議用於存取驗證的 HTTPS 憑證</li></ul> |
| 調整大小 | 每個中間大小（8核心/4 GB） HGS 伺服器節點都可以處理 1000 Hyper-v 主機。 |
| 管理 | 指定將管理 HGS 的特定人員。 它們應該與網狀架構系統管理員分開。 為了進行比較，HGS 叢集的考慮方式與憑證授權單位單位（CA）的管理隔離、實體部署和整體安全性敏感度層級相同。 |
| 主機守護者服務 Active Directory | 根據預設，HGS 會安裝自己的內部 Active Directory 以進行管理。 這是獨立、自我管理的樹系，而且是建議的設定，可協助隔離 HGS 與您的網狀架構。<br><br>如果您已經有用於隔離的高許可權 Active Directory 樹系，則可以使用該樹系，而不是 HGS 預設樹系。 在 Hyper-v 主機或網狀架構管理工具所在的樹系中，HGS 不會加入網域是很重要的。 這麼做可能會讓網狀架構系統管理員取得 HGS 的控制權。 |
| 嚴重損壞修復 | 您有三種選擇：<br><ol><li>在每個資料中心安裝個別的 HGS 叢集，並授權受防護的 Vm 在主要和備份資料中心內執行。 這可避免跨 WAN 延展叢集的需求，並可讓您隔離虛擬機器，使其只能在其指定的網站中執行。</li><li>在兩個（或多個）資料中心之間的延展叢集上安裝 HGS。 如果 WAN 中斷，這會提供復原功能，但會推送容錯移轉叢集的限制。 您無法將工作負載隔離到一個網站;授權在一個網站中執行的 VM 可以在任何其他網站上執行。</li><li>將您的 Hyper-v 主機註冊為另一個 HGS 作為容錯移轉。</li></ol>您也應該藉由匯出其設定來備份每個 HGS，以便隨時在本機進行復原。 如需詳細資訊，請參閱[Export-HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate)和[HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate)。 |
| 主機守護者服務金鑰 | 主機守護者服務會使用兩個非對稱金鑰組（加密金鑰和簽署金鑰），分別以 SSL 憑證表示。 有兩個選項可產生這些金鑰：<br><ol><li>內部憑證授權單位單位-您可以使用您的內部 PKI 基礎結構來產生這些金鑰。 這適用于資料中心環境。</li><li>公開信任的憑證授權單位單位–使用從公開信任的憑證授權單位單位取得的一組金鑰。 這是主控者應該使用的選項。</li></ol>請注意，雖然您可以使用自我簽署憑證，但不建議用於概念證明實驗室以外的部署案例。<br><br>除了擁有 HGS 金鑰以外，主機服務提供者可以使用「攜帶您自己的金鑰」，租使用者可以在其中提供自己的金鑰，讓部分（或所有）租使用者可以有自己的特定 HGS 金鑰。 此選項適用于可提供頻外程式的主控者，讓租使用者上傳其金鑰。 |
| 主機守護者服務金鑰儲存 | 為了獲得最強的安全性，我們建議您建立 HGS 金鑰，並以獨佔方式儲存在硬體安全性模組（HSM）中。 如果您不是使用 Hsm，強烈建議您在 HGS 伺服器上套用 BitLocker。 |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>受防護主機的網狀架構系統管理員規劃

| 區域 | 詳細資料 |
|------|---------|
| 硬體 | <ul><li>主機金鑰證明：您可以使用任何現有的硬體做為受防護主機。 有幾個例外（為了確保您的主機可以使用從 Windows Server 2016 開始的新安全性機制，請參閱[相容硬體與 Windows server 2016 虛擬化型保護程式碼完整性](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。</li><li>TPM 信任證明：您可以使用任何具有[硬體保證額外限定](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance)性的硬體（請參閱[與受防護的 Vm 相容的伺服器設定，以及特定設定的程式碼完整性的虛擬化保護](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)）。 這包括 TPM 2.0 和 UEFI 2.3.1 版 c 和更新版本。</li></ul> |
| 作業系統 | 我們建議您針對 Hyper-v 主機 OS 使用 Server Core 選項。 |
| 效能影響 | 根據效能測試，我們預期執行受防護的 Vm 與不受防護的 Vm 之間大約有5% 的密度差異。 這表示如果指定的 Hyper-v 主機可以執行20個未受防護的 Vm，我們預期它可以執行19個受防護的 Vm。<br><br>請務必確認您的一般工作負載大小。 例如，可能會有一些密集寫入導向 IO 工作負載的極端值，會進一步影響密度差異。 |
| 分公司考量 | 從 Windows Server 1709 版開始，您可以為分公司的受防護 VM，指定在本機執行之虛擬化 HGS 伺服器的 fallback URL。 當分公司無法連線到資料中心的 HGS 伺服器時，就可以使用 fallback URL。 在舊版的 Windows Server 上，在分公司執行的 Hyper-v 主機需要連線到主機守護者服務，才能開啟電源或即時移轉受防護的 Vm。 如需詳細資訊，請參閱[分公司考慮](guarded-fabric-manage-branch-office.md)。 |
