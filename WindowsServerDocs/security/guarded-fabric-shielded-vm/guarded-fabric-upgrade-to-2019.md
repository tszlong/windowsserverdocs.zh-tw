---
title: 將受防護網狀架構升級至 Windows Server 2019
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 50e35939031a74173fb031cf963af97bf8bb6dba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856351"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>將受防護網狀架構升級至 Windows Server 2019

> 適用于： Windows Server 2019、Windows Server （半年通道）、Windows Server 2016

本文說明將現有的受防護網狀架構從 Windows Server 2016、Windows Server 1709 版或 Windows Server 1803 版升級至 Windows Server 2019 所需的步驟。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

當您在 Windows Server 2019 上執行受防護的網狀架構時，您可以利用數個新功能：

**主機金鑰證明**是我們最新的證明模式，其設計目的是為了讓您的 hyper-v 主機沒有 tpm 2.0 裝置可供 tpm 證明時，更容易執行受防護的 vm。 主機金鑰證明會使用金鑰組來向 HGS 驗證主機、移除主機加入 Active Directory 網域的需求、排除 HGS 與企業樹系之間的 AD 信任，以及減少開啟的防火牆埠數目。 主機金鑰證明會取代 Windows Server 2019 中已淘汰 Active Directory 證明。

**V2 證明版本**-為了支援未來的主機金鑰證明和新功能，我們引進了 HGS 版本設定。 Windows Server 2019 上的 HGS 全新安裝將會導致伺服器使用 v2 證明，這表示它可以支援 Windows Server 2019 主機的主機金鑰證明，而且仍然支援 Windows Server 2016 上的 v1 主機。 就地升級至2019將保留在版本 v1，直到您手動啟用 v2 為止。 大部分的 Cmdlet 現在都有-HgsVersion 參數，可讓您指定是否要使用舊版或新式證明原則。

**支援 linux 受防護的 vm** -執行 Windows Server 2019 的 hyper-v 主機可以執行 Linux 受防護的 vm。 從 Windows Server 1709 版起，Linux 受防護的 Vm 已存在，而 Windows Server 2019 是第一次長期維護通道版本來支援它們。

**分公司**的改良功能-我們讓您更輕鬆地在分公司執行受防護的 vm，並支援 hyper-v 主機上的離線受防護 vm 和回退設定。

**TPM 主機**系結-針對最安全的工作負載，您想要讓受防護的 VM 只在建立它的第一個主機上執行，而不是其他，您現在可以使用主機的 TPM 將 VM 系結至該主機。 這最適合用於特殊許可權存取工作站和分公司，而不是需要在主機間遷移的一般資料中心工作負載。

## <a name="compatibility-matrix"></a>相容性矩陣

將您的受防護網狀架構升級至 Windows Server 2019 之前，請先檢查下列相容性對照表，以瞭解是否支援您的設定。

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper-v 主機** | 支援 | 支援<sup>1</sup>|
|**WS2019 Hyper-v 主機** | 不支援<sup>2</sup> | 支援|

<sup>1</sup> windows server 2016 主機只能針對使用 v1 證明通訊協定的 windows SERVER 2019 HGS 伺服器進行證明。 Windows Server 2016 主機不支援 v2 證明通訊協定中專門提供的新功能，包括主機金鑰證明。

<sup>2</sup> Microsoft 注意到防止使用 TPM 證明的 windows server 2019 主機成功證明 windows SERVER 2016 HGS 伺服器的問題。 Windows Server 2016 未來的更新將會解決這項限制。

## <a name="upgrade-hgs-to-windows-server-2019"></a>將 HGS 升級至 Windows Server 2019

在升級 Hyper-v 主機之前，建議您先將 HGS 叢集升級至 Windows Server 2019，以確保所有主機（不論是執行 Windows Server 2016 或2019）都能繼續證明。

升級 HGS 叢集時，您將需要在升級叢集時，暫時從叢集移除一個節點。 這會降低叢集的容量，以回應來自 Hyper-v 主機的要求，而且可能會導致您的租使用者回應時間變慢或服務中斷。 請確定您有足夠的容量來處理您的證明和金鑰發行要求，然後再升級 HGS 伺服器。

若要升級 HGS 叢集，請在叢集的每個節點上（一次一個節點）執行下列步驟：

1.  在提高許可權的 PowerShell 提示字元中執行 `Clear-HgsServer`，以從您的叢集中移除 HGS 伺服器。 此 Cmdlet 會從容錯移轉叢集移除 HGS 複寫的存放區、HGS 網站和節點。
2.  如果您的 HGS 伺服器是網域控制站（預設設定），您將需要在升級的第一個節點上執行 `adprep /forestprep` 和 `adprep /domainprep`，以準備網域以進行 OS 升級。 如需詳細資訊，請參閱[Active Directory Domain Services 升級檔](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths)。
3.  執行 Windows Server 2019 的就地[升級](../../get-started-19/install-upgrade-migrate-19.md)。
4.  執行[HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) ，將節點聯結回叢集。

當所有節點都已升級至 Windows Server 2019 之後，您可以選擇性地將 HGS 版本升級為 v2，以支援新功能，例如主機金鑰證明。

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>將 Hyper-v 主機升級至 Windows Server 2019

在您將 Hyper-v 主機升級至 Windows Server 2019 之前，請確定您的 HGS 叢集已升級至 Windows Server 2019，而且您已將所有 Vm 從 Hyper-v 伺服器移出。

1.  如果您在伺服器上使用 Windows Defender 應用程式控制程式碼完整性原則（在使用 TPM 證明時一律是如此），請在嘗試升級伺服器之前，先確定該原則處於 audit 模式或已停用。 [瞭解如何停用 WDAC 原則](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  遵循[Windows server 升級內容](../../upgrade/upgrade-overview.md)中的指導方針，將您的主機升級至 Windows server 2019。 如果您的 Hyper-v 主機是容錯移轉叢集的一部分，請考慮使用叢集[作業系統輪流升級](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。
3.  [測試並重新啟用](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies)您的 Windows Defender 應用程式控制原則（如果您已在升級前啟用）。
4.  執行 `Get-HgsClientConfiguration` 以檢查**IsHostGuarded = True**，這表示主機已成功地將證明傳遞至您的 HGS 伺服器。
5.  如果您使用 TPM 證明，則在升級至通過證明之後，您可能需要[重新捕獲 TPM 基準或程式碼完整性原則](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md)。
6.  再次開始在主機上執行受防護的 Vm！

## <a name="switch-to-host-key-attestation"></a>切換至主機金鑰證明

如果您目前正在執行以 Active Directory 為基礎的證明，而且想要升級為主機密鑰證明，請遵循下列步驟。 請注意，以 Active Directory 為基礎的證明已在 Windows Server 2019 中淘汰，可能會在未來的版本中移除。

1.  執行下列命令，確定您的 HGS 伺服器以 v2 證明模式運作。 即使 HGS 伺服器升級為 v2，現有 v1 主機仍會繼續進行證明。

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  從您的每一部 Hyper-v 主機[產生主機金鑰](guarded-fabric-create-host-key.md)，並向 HGS 註冊。 由於 HGS 仍在 Active Directory 模式中運作，您會收到警告，指出新的主機金鑰不會立即生效。 這是刻意的，因為您不想要變更為主機金鑰模式，直到所有主機都能成功地證明主機金鑰為止。

3.  為每個主機註冊主機金鑰之後，您可以設定 HGS 使用主機金鑰證明模式：

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    如果您遇到「主機金鑰模式」的問題，而且需要還原回 Active Directory 型證明，請在 HGS 上執行下列命令：

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```