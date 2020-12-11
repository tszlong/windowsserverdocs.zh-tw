---
description: 深入瞭解：將受防護網狀架構升級至 Windows Server 2019
title: 將受防護網狀架構升級至 Windows Server 2019
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 11/21/2018
ms.openlocfilehash: bc7cedb2c232a61593dcce630e365b375c9d4744
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049096"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>將受防護網狀架構升級至 Windows Server 2019

> 適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

本文說明將現有的受防護網狀架構從 Windows Server 2016、Windows Server 1709 版或 Windows Server 1803 版升級到 Windows Server 2019 的必要步驟。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

當您在 Windows Server 2019 上執行受防護網狀架構時，您可以利用數個新功能：

**主機金鑰證明** 是我們最新的證明模式，其設計目的是要讓您在 hyper-v 主機沒有 tpm 2.0 裝置可用來進行 tpm 證明時，更輕鬆地執行受防護的 vm。 主機金鑰證明使用金鑰組來向 HGS 驗證主機，移除主機加入 Active Directory 網域的需求、消除 HGS 與公司樹系之間的 AD 信任，以及減少開啟的防火牆埠數目。 主機金鑰證明取代了 Windows Server 2019 中已淘汰的 Active Directory 證明。

**V2 證明版本** -為了在未來支援主機金鑰證明和新功能，我們引進了對 HGS 的版本控制。 Windows Server 2019 上的 HGS 全新安裝將會導致伺服器使用 v2 證明，這表示它可以支援 Windows Server 2019 主機的主機金鑰證明，並且仍支援 Windows Server 2016 上的 v1 主機。 在您手動啟用 v2 之前，對2019的就地升級將維持在版本 v1。 大部分的 Cmdlet 現在都有一個-HgsVersion 參數，可讓您指定是否要使用舊版或新式證明原則。

**支援 linux 受防護的 vm** -執行 Windows Server 2019 的 hyper-v 主機可以執行 linux 受防護的 vm。 雖然 Linux 受防護的 Vm 是自 Windows Server 1709 版以來，但 Windows Server 2019 是第一個長期維護通道版本，可支援它們。

**分公司改進** -我們讓您更輕鬆地在分公司執行受防護的 vm，並支援 hyper-v 主機上的離線受防護 vm 和回溯設定。

**TPM 主機** 系結-針對最安全的工作負載，您希望受防護的 VM 只能在建立它的第一部主機上執行，但您現在可以使用主機的 TPM，將 vm 系結至該主機。 這最適合用於特殊許可權的存取工作站和分公司，而不是需要在主機之間遷移的一般資料中心工作負載。

## <a name="compatibility-matrix"></a>相容性矩陣

將受防護網狀架構升級至 Windows Server 2019 之前，請先檢查下列相容性對照表，以查看是否支援您的設定。

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper-v 主機** | 支援 | 支援<sup>1</sup>|
|**WS2019 Hyper-v 主機** | 不支援的<sup>2</sup> | 支援|

<sup>1</sup> windows server 2016 主機只能針對使用 v1 證明通訊協定的 windows SERVER 2019 HGS 伺服器進行證明。 Windows Server 2016 主機不支援 v2 證明通訊協定中提供的新功能，包括主機金鑰證明。

<sup>2</sup> Microsoft 注意到無法針對 windows SERVER 2016 HGS 伺服器順利證明使用 TPM 證明的 windows server 2019 主機的問題。 未來的 Windows Server 2016 更新將會解決這項限制。

## <a name="upgrade-hgs-to-windows-server-2019"></a>將 HGS 升級至 Windows Server 2019

建議您先將 HGS 叢集升級至 Windows Server 2019，再升級 Hyper-v 主機，以確保所有主機（不論是執行 Windows Server 2016 或2019）都能繼續證明。

升級 HGS 叢集時，您將需要在升級時暫時從叢集移除一個節點。 這會減少叢集的容量，以回應來自 Hyper-v 主機的要求，而且可能會導致您的租使用者回應時間變慢或服務中斷。 升級 HGS 伺服器之前，請確定您有足夠的容量來處理證明和金鑰發行要求。

若要升級 HGS 叢集，請在叢集的每個節點上執行下列步驟，一次一個節點：

1.  `Clear-HgsServer`在提高許可權的 PowerShell 提示字元中執行，以從您的叢集中移除 HGS 伺服器。 此 Cmdlet 會從容錯移轉叢集中移除 HGS 複寫的存放區、HGS 網站和節點。
2.  如果您的 HGS 伺服器 (預設設定) 的網域控制站，您必須 `adprep /forestprep` `adprep /domainprep` 在要升級的第一個節點上執行和，以準備網域以進行 OS 升級。 如需詳細資訊，請參閱 [Active Directory Domain Services 升級檔](../../identity/ad-ds/deploy/upgrade-domain-controllers.md#supported-in-place-upgrade-paths) 。
3.  執行就地 [升級](../../get-started-19/install-upgrade-migrate-19.md) 至 Windows Server 2019。
4.  執行 [Initialize-HgsServer](guarded-fabric-configure-additional-hgs-nodes.md) ，將節點聯結回叢集。

當所有節點都升級為 Windows Server 2019 之後，您可以選擇性地將 HGS 版本升級為 v2，以支援主機金鑰證明之類的新功能。

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>將 Hyper-v 主機升級至 Windows Server 2019

將 Hyper-v 主機升級至 Windows Server 2019 之前，請確定您的 HGS 叢集已升級為 Windows Server 2019，且您已將所有 Vm 移出 Hyper-v 伺服器。

1.  如果您在伺服器上使用 Windows Defender 應用程式控制程式碼完整性原則 (在使用 TPM 證明) 時一律會發生這種情況，請確定原則處於 audit 模式或已停用，然後再嘗試升級伺服器。 [瞭解如何停用 WDAC 原則](/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  遵循 [Windows server 升級內容](../../upgrade/upgrade-overview.md) 中的指引，將您的主機升級至 windows server 2019。 如果您的 Hyper-v 主機是容錯移轉叢集的一部分，請考慮使用叢集 [作業系統輪流升級](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。
3.  [測試並重新啟用](/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies) 您的 Windows Defender 應用程式控制原則（如果您在升級前啟用了）。
4.  執行 `Get-HgsClientConfiguration` 以檢查 **IsHostGuarded = True**，這表示主機已成功通過您 HGS 伺服器的證明。
5.  如果您是使用 TPM 證明，您可能需要在升級為通過證明之後， [重新捕獲 TPM 基準或程式碼完整性原則](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md) 。
6.  再次開始在主機上執行受防護的 Vm！

## <a name="switch-to-host-key-attestation"></a>切換至主機金鑰證明

如果您目前正在執行以 Active Directory 為基礎的證明，並想要升級至主機金鑰證明，請遵循下列步驟。 請注意，以 Active Directory 為基礎的證明在 Windows Server 2019 中已淘汰，可能會在未來的版本中移除。

1.  藉由執行下列命令，確定您的 HGS 伺服器正在 v2 證明模式下運作。 即使 HGS 伺服器升級為 v2，現有的 v1 主機仍將繼續進行證明。

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  從您的每個 Hyper-v 主機[產生主機金鑰](guarded-fabric-create-host-key.md)，並向 HGS 註冊它們。 因為 HGS 仍在 Active Directory 模式下運作，您將會收到一則警告，指出新的主機金鑰不會立即生效。 這是故意的，因為您不想要變更為主機金鑰模式，直到您的所有主機都能成功證明主機金鑰為止。

3.  一旦為每個主機註冊主機金鑰之後，您就可以設定 HGS 使用主機金鑰證明模式：

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    如果您在使用主機金鑰模式時遇到問題，而且需要還原回 Active Directory 為基礎的證明，請在 HGS 上執行下列命令：

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```
