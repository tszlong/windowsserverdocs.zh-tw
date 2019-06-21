---
title: 將受防護網狀架構升級至 Windows Server 2019
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/21/2018
ms.openlocfilehash: 39974806c02e55b37d3d16748c4ca0e3f361ee45
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284113"
---
# <a name="upgrade-a-guarded-fabric-to-windows-server-2019"></a>將受防護網狀架構升級至 Windows Server 2019

> 適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

本文說明從 Windows Server 2016 中，Windows Server 1709 版或 Windows Server 1803 版到 Windows Server 2019 升級現有的受防護網狀架構所需的步驟。

## <a name="whats-new-in-windows-server-2019"></a>Windows Server 2019 的新功能

當您在 Windows Server 2019 上執行受防護網狀架構時，您可以利用數種新功能：

**裝載金鑰證明**是我們最新的證明模式，旨在讓您更輕鬆地與 HYPER-V 主機並沒有 TPM 2.0 裝置可以使用 TPM 證明時，執行受防護的 Vm。 主機金鑰證明使用金鑰組，來驗證主機與 HGS，移除主機加入至 Active Directory 網域，需要排除 AD 之間的信任 HGS 和公司樹系中，並減少開啟防火牆連接埠數目。 主機金鑰證明會取代 Active Directory 的證明，在 Windows Server 2019 中已被取代。

**V2 證明版本**-若要支援主機金鑰證明，以及新功能在未來，我們已導入到 HGS 的版本控制。 於 Windows Server 2019 HGS 的全新安裝將會導致使用 v2 證明，這表示它可以支援 Windows Server 2019 主機的主機金鑰證明，並仍然支援 Windows Server 2016 上的 v1 主機的伺服器。 2019 的就地升級會保留在版本 v1 中，直到您手動啟用 v2。 大多數的 cmdlet 現在有-HgsVersion 參數，可讓您指定您是否要使用傳統或新式證明原則。

**適用於 Linux 的支援受防護的 Vm** -執行 Windows Server 2019 的 HYPER-V 主機可以執行的 Linux 受防護的 Vm。 雖然 Linux 受防護的 Vm 已經從 Windows Server 1709 版開始，Windows Server 2019 來支援他們的第一個長時間詞彙維護通道版本。

**分支辦公室改進**-已變得更容易在 HYPER-V 主機上，執行受防護的 Vm 分公司支援離線受防護的 Vm 和後援設定。

**繫結的 TPM 主機**-的最安全的工作負載，您想只在第一部主機上執行的建立，但是沒有其他受防護的 VM，您現在可以繫結的 VM 使用主機的 TPM 該主控件。 這是最適合用於特殊權限的存取工作站和分公司辦公室，而不是一般的資料中心的工作負載，需要主機之間移轉。

## <a name="compatibility-matrix"></a>相容性比較表

您升級至 Windows Server 2019 的受防護網狀架構之前，請檢閱下列的相容性比較表，以查看您的組態是否支援。

|  | WS2016 HGS | WS2019 HGS|
|---|---|---|
|**WS2016 Hyper V 主機** | 支援 | 支援<sup>1</sup>|
|**WS2019 Hyper V 主機** | 不支援<sup>2</sup> | 支援|

<sup>1</sup> Windows Server 2016 主機只可以證明對 Windows Server 2019 HGS 伺服器使用 v1 證明通訊協定。 Windows Server 2016 主機不支援專門用於 v2 證明通訊協定，包括主機金鑰證明的新功能。

<sup>2</sup> Microsoft 所知的問題，導致使用 TPM 證明，從針對 Windows Server 2016 HGS 伺服器成功證明的 Windows Server 2019 主機。 將在未來的更新中解決這項限制適用於 Windows Server 2016。

## <a name="upgrade-hgs-to-windows-server-2019"></a>升級到 Windows Server 2019 的 HGS

我們建議您升級至 Windows Server 2019 的 HGS 叢集，然後再升級您的 HYPER-V 主機，以確保所有的主機，不論它們是 Windows Server 2016 或 2019，可以繼續以證明成功。

升級您的 HGS 叢集會要求您一次從叢集暫時移除一個節點時升級。 這會減少您的叢集的容量，以回應您的 HYPER-V 主機的要求，並可能會導致回應時間緩慢或服務中斷您的租用戶。 請確定您擁有足夠容量來處理您的證明和金鑰發行要求，然後再升級 HGS 伺服器。

若要升級您的 HGS 叢集，請在您的叢集，一次一個節點的每個節點上執行下列步驟：

1.  從叢集移除 HGS 伺服器，藉由執行`Clear-HgsServer`提升權限的 PowerShell 提示字元中。 此 cmdlet 會移除複寫的 HGS 存放區、 HGS 網站和節點的容錯移轉叢集。
2.  如果您的 HGS 伺服器是網域控制站 （預設設定），您必須執行`adprep /forestprep`和`adprep /domainprep`正在升級為網域準備 OS 升級的第一個節點上。 請參閱[Active Directory 網域服務升級文件](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/upgrade-domain-controllers#supported-in-place-upgrade-paths)如需詳細資訊。
3.  執行[就地升級](../../get-started-19/install-upgrade-migrate-19.md)到 Windows Server 2019。
4.  執行[Initialize HgsServer](guarded-fabric-configure-additional-hgs-nodes.md)回叢集中加入節點。

之後的所有節點都已都升級為 Windows Server 2019 中，您可以選擇性地 HGS 版本升級為 v2，以支援新功能，例如主機金鑰證明。

```powershell
Set-HgsServerVersion  v2
```

## <a name="upgrade-hyper-v-hosts-to-windows-server-2019"></a>升級至 Windows Server 2019 的 HYPER-V 主機

升級至 Windows Server 2019 您 HYPER-V 主機之前，請確定您的 HGS 叢集已升級至 Windows Server 2019 和您已移出 HYPER-V 伺服器的所有 Vm。

1.  如果您使用 Windows Defender 應用程式控制碼完整性原則在您的伺服器 （一律的情況下使用 TPM 證明時），請確定原則處於稽核模式，或停用然後再嘗試升級伺服器。 [了解如何停用 WDAC 原則](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/disable-windows-defender-application-control-policies)
2.  請依照下列中的指導方針[Windows Server 升級中心](http://aka.ms/upgradecenter)升級您的 Windows Server 2019 主機。 如果您的 HYPER-V 主機容錯移轉叢集的一部分，請考慮使用[叢集作業系統輪流升級](../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md)。
3.  [測試並重新啟用](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/audit-windows-defender-application-control-policies)您的 Windows Defender 應用程式控制原則，如果您有一個在升級之前啟用。
4.  執行`Get-HgsClientConfiguration`來檢查是否**IsHostGuarded = True**，這表示主機已成功傳遞與您的 HGS 伺服器證明。
5.  如果您使用 TPM 證明，您可能需要[重新擷取 TPM 基準或程式碼完整性原則](guarded-fabric-add-host-information-for-tpm-trusted-attestation.md)通過證明，要在升級之後。
6.  開始執行受防護的 Vm 主機上一次 ！

## <a name="switch-to-host-key-attestation"></a>切換到裝載金鑰證明

如果您目前正在執行 Active Directory 型證明，而且想要升級至主應用程式金鑰證明，請遵循下列步驟。 請注意，Active Directory 型證明 Windows Server 2019 中已被取代，而且可能在未來版本中移除。

1.  請確定您的 HGS 伺服器，藉由執行下列命令以 v2 證明模式運作。 現有的 v1 主機將會繼續即使 HGS 伺服器升級為 v2，證明客戶資料。

    ```powershell
    Set-HgsServerVersion v2
    ```

2.  [產生主應用程式金鑰](guarded-fabric-create-host-key.md)從每個您 HYPER-V 裝載，並向 HGS 註冊它們。 HGS 仍在 Active Directory 模式中運作，因為您會收到警告，新的主索引鍵不會立即生效。 這是刻意設計的您不想變更主索引鍵模式，直到所有的主機可以成功證明具有主索引鍵。

3.  一旦已註冊每一部主機的主機金鑰，您可以設定 HGS 使用主機金鑰證明模式：

    ```powershell
    Set-HgsServer -TrustHostKey
    ```

    如果您遇到問題，與主機金鑰的模式，並需要還原回至 Active Directory 型證明，請 HGS 上執行下列命令：

    ```powershell
    Set-HgsServer -TrustActiveDirectory
    ```