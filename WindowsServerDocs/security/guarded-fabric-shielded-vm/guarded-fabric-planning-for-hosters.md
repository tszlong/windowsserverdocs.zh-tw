---
title: 受防護網狀架構與受防護的 VM 規劃的主機服務提供者指南
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 854defc8-99f8-4573-82c0-f484e0785859
manager: dongill
author: nirb-ms
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 320723f7a0a25784180b232ce05d42c2ced933c8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284180"
---
# <a name="guarded-fabric-and-shielded-vm-planning-guide-for-hosters"></a>受防護網狀架構與受防護的 VM 規劃的主機服務提供者指南

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

本主題涵蓋將需要對您的網狀架構上執行啟用受防護的虛擬機器的規劃決策。 不論您升級現有的 HYPER-V 網狀架構，或建立新的網狀架構，執行受防護 Vm 包含兩個主要元件：

- 主機守護者服務 (HGS) 提供證明與金鑰保護，讓您可以確定只在已核准且狀況良好的 HYPER-V 主機上，將執行受防護的 Vm。 
- 已核准且狀況良好的 HYPER-V 主機可以執行受防護的 Vm （和一般虛擬機器），這些值稱為受防護主機。

![HGS 和受防護的主機](../media/Guarded-Fabric-Shielded-VM/guarded-host-hgs-plus-host-diagram-basic.png)

## <a name="decision-1-trust-level-in-the-fabric"></a>決策 #1:信任網狀架構中的層級

強度信任您想要達到您的網狀架構中，主要取決於您實作的主機守護者服務和受防護的 HYPER-V 主機。 強度的信任受到證明模式。 有兩個互斥的選項：

1. TPM 信任證明

    如果您的目標是要協助防止惡意的系統管理員或遭入侵的網狀架構中的虛擬機器，您將使用 TPM 信任證明。 此選項適用於多租用戶裝載案例也與高價值的資產，在企業環境中，例如 SQL 或 SharePoint 等的內容伺服器或網域控制站。
    以 Hypervisor 受保護的程式碼完整性 （hvci 原則） 原則，並允許主應用程式執行之前，由 HGS 強制執行其有效性受防護的 Vm。 

2. 主機金鑰證明

    如果您的需求主要取決於所需的合規性虛擬機器加密同時待用以及進行中，您將會使用主機金鑰證明。 此選項適用於一般用途的資料中心，您可以輕鬆地與 HYPER-V 主機和網狀架構系統管理員無法存取客體作業系統的虛擬機器的日常維護和作業。 

    在此模式中，網狀架構系統管理員只負責確保 HYPER-V 主機的健全狀況。 
    HGS 不並決定什麼或不允許執行無任何作用，因為惡意程式碼和偵錯工具將會正常運作。 
    
    不過，嘗試直接附加到處理程序 （例如 WinDbg.exe) 的偵錯工具會封鎖受防護的 vm，因為 VM 的背景工作處理序 (VMWP.exe) 是受保護的處理序燈 (PPL)。 
    替代偵錯技術的詳細資訊，例如所使用的 LiveKd.exe，不會封鎖。 
    不同於受防護的 Vm，支援加密的 Vm 背景工作處理序不會執行，因此傳統偵錯工具要 WinDbg.exe PPL 會繼續正常運作。

    類似的證明模式，名為 「 系統管理信任證明 （Active Directory 為基礎） 從 Windows Server 2019 已被取代。 

您所選擇的信任層級會指定硬體需求的 HYPER-V 主機，以及您在 網狀架構套用的原則。 如有必要，您可以部署您使用現有的硬體和系統管理信任證明的受防護網狀架構，並將它轉換至 TPM 信任證明時已升級硬體，而且您需要加強 fabric 安全性。

## <a name="decision-2-existing-hyper-v-fabric-versus-a-new-separate-hyper-v-fabric"></a>決策 #2:與新的獨立 HYPER-V 網狀架構的現有 HYPER-V 網狀架構

如果您有現有的網狀架構 （HYPER-V 或其他原因），就很可能是，您可用它來執行受防護的 Vm，以及一般的 Vm。 某些客戶選擇將整合到其現有的工具和網狀架構的受防護的 Vm，而其他項目不同的商業理由的網狀架構。

## <a name="hgs-admin-planning-for-the-host-guardian-service"></a>規劃主機守護者服務的 HGS 系統管理員

不論是在專用實體伺服器、 受防護的 VM、 隔離的 HYPER-V 主機 （從它所保護的網狀架構分隔） 或以邏輯方式分隔使用不同的 Azure 上的 VM 上部署的 「 主機守護者服務 (HGS) 」，在高度安全的環境中，訂用帳戶。   

| 區域 | 詳細資料 |
|------|---------|
| 安裝需求 | <ul><li>一部伺服器 （三個節點叢集的高可用性建議使用）</li><li>遞補，至少兩部 HGS 伺服器所需</li><li>伺服器可以是虛擬或實體 （實體伺服器與 TPM 2.0 建議;TPM 1.2 也支援)</li><li>Server Core 安裝的 Windows Server 2016 或更新版本</li><li>網路直視允許 HTTP 網狀架構或[後援設定](guarded-fabric-manage-branch-office.md#fallback-configuration)</li><li>建議的存取驗證的 HTTPS 憑證</li></ul> |
| 調整大小 | 每個中型大小 (8 核心/4 GB) HGS 伺服器節點可以處理 1000 的 HYPER-V 主機。 |
| Management | 指定將管理 HGS 的特定人員。 它們應該不同於網狀架構系統管理員。 如需比較，HGS 叢集可以視為相同的方式為憑證授權單位 (CA) 系統管理隔離中，實際的部署和整體的層級的安全性敏感度方面。 |
| 主機守護者服務的 Active Directory | 根據預設，HGS 會安裝它自己內部的 Active Directory 進行管理。 這是獨立的自我管理的樹系，而且是建議的設定，以協助找出從您的網狀架構的 HGS。<br><br>如果您已經具備高度權限的 Active Directory 樹系用於隔離，您可以使用該樹系，而不是 HGS 預設樹系。 請務必 HGS 未加入 HYPER-V 主機或您的網狀架構管理工具所在的樹系中的網域。 如此一來可能會允許全面掌控 HGS 網狀架構系統管理員。 |
| 嚴重損壞修復 | 有三個選項：<br><ol><li>安裝個別的 HGS 叢集中每個資料中心，並授權受防護的 Vm，在主要和備份的資料中心中執行。 這可避免您不必透過 WAN 延展叢集，並可讓您隔離虛擬機器，使它們只能在其指定的網站中執行。</li><li>安裝兩個 （含） 以上的資料中心之間延展式叢集上的 HGS。 如果 WAN 中斷，但限制的容錯移轉叢集，這會提供恢復功能。 您無法將隔離至一個網站; 的工作負載獲授權在一個站台執行的 VM 可以在其他任何執行。</li><li>向另一部 HGS 註冊您的 HYPER-V 主機進行容錯移轉。</li></ol>您也應該藉由將匯出其組態，以便您一律可以在本機復原備份每個 HGS。 如需詳細資訊，請參閱 <<c0> [ 匯出 HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/export-hgsserverstate)並[匯入 HgsServerState](https://docs.microsoft.com/powershell/module/hgsserver/import-hgsserverstate)。 |
| 主機守護者服務金鑰 | 主機守護者服務會使用兩個非對稱金鑰組，加密金鑰和簽署金鑰，分別表示使用 SSL 憑證。 有兩個選項來產生這些金鑰：<br><ol><li>內部憑證授權單位 – 您可以產生這些金鑰，使用您的內部 PKI 基礎結構。 這是適用於資料中心環境。</li><li>公開受信任的憑證授權單位 – 使用一組從公開的受信任的憑證授權單位取得的金鑰。 這是主機服務提供者應該使用的選項。</li></ol>請注意，雖然您可以使用自我簽署的憑證，不建議用於概念證明實驗室以外的部署案例。<br><br>除了擁有 HGS 金鑰，主機服務提供者可以使用 「 攜帶您自己的金鑰 」，租用戶可以提供他們自己的金鑰，讓部分 （或所有） 的租用戶可以有自己特定的 HGS 金鑰。 這個選項適合主機服務提供者中可供上傳其索引鍵的租用戶的頻外程序。 |
| 主機守護者服務的主要儲存體 | 可能最強的安全性，建議 HGS 金鑰會建立並儲存以獨佔方式在硬體安全性模組 (HSM)。 如果您未使用 Hsm，強烈建議將 BitLocker 套用 HGS 伺服器上。 |

## <a name="fabric-admin-planning-for-guarded-hosts"></a>網狀架構系統管理員計劃的受防護主機

| 區域 | 詳細資料 |
|------|---------|
| 硬體 | <ul><li>主機金鑰證明：您可以使用任何現有的硬體與您的受防護主機。 有少數的例外狀況 (若要確定您的主機可以使用新的安全性機制，從 Windows Server 2016 開始，請參閱[相容的硬體與 Windows Server 2016 的虛擬化保護的程式碼完整性](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。</li><li>TPM 信任證明：您可以使用具有的任何硬體[硬體保證額外認證](https://msdn.microsoft.com/windows/hardware/commercialize/design/compatibility/systems#system-server-assurance)只要適當地設定 (請參閱[伺服器組態符合受防護的 Vm 和虛擬化架構程式碼完整性保護](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)的特定組態)。 這包括 TPM 2.0 和 UEFI 2.3.1c 的版本和更新版本。</li></ul> |
| 作業系統 | 我們建議使用 HYPER-V 主機作業系統的 Server Core 選項。 |
| 效能的影響 | 根據效能測試，我們預期大約 5%密度-之間執行差異防護的 Vm 和受防護的 Vm。 這表示，如果指定的 HYPER-V 主機可以執行 20 個受防護的 Vm，我們希望它可以執行 19 受防護的 Vm。<br><br>請務必確認與您的一般工作負載的調整大小。 例如，可能有一些極端值與密集寫入導向 IO 的工作負載會進一步影響密度差異。 |
| 分公司考量 | 從 Windows Server 1709 版開始，您可以指定虛擬化的 HGS 伺服器，在本機執行，做為受防護的 VM，分公司中的後援 URL。 分公司失去連線能力的資料中心內的 HGS 伺服器時，可用的後援的 URL。 在舊版的 Windows Server 中，分公司辦公室的需求連線能力的電源開啟或即時移轉時，主機守護者服務執行的 HYPER-V 主機受防護的 Vm。 如需詳細資訊，請參閱 <<c0> [ 分支辦公室考量](guarded-fabric-manage-branch-office.md)。 |
