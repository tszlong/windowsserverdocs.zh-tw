---
title: 受防護的主機必要條件
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 5f2c3ec4b2c434ea945d86c4b1593e2e416a5123
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819229"
---
# <a name="prerequisites-for-guarded-hosts"></a>針對受防護主機的必要條件

>適用於：Windows Server 2019，Windows Server （半年通道），Windows Server 2016

檢閱您選擇，證明模式的主機必要條件，然後按一下 下一步，將受防護的主機。

## <a name="tpm-trusted-attestation"></a>TPM 信任證明

受防護的主機使用 TPM 模式必須符合下列必要條件：

-   **硬體**:需要初始部署一部主機。 若要測試的受防護的 Vm 的 HYPER-V 即時移轉，您必須擁有至少兩部主機。

    主控件必須具有：
    
    - Iommu 所致，第二層位址轉譯 (SLAT)
    - TPM 2.0
    - UEFI 2.3.1 或更新版本
    - 設定為使用 （不 BIOS 或 「 舊版 」 模式） 的 UEFI 開機
    - 已啟用安全開機
        
-   **作業系統**:Windows Server 2016 Datacenter edition 或更新版本

    > [!IMPORTANT]
    > 請確定您已安裝[最新累積更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。  

-   **角色和功能**:HYPER-V 角色和 「 主機守護者 HYPER-V 支援功能。 只有使用 Datacenter 版的 Windows Server 上的主機守護者 HYPER-V 支援功能。 

> [!WARNING]
> 主機守護者 HYPER-V 支援功能可讓虛擬化保護可能與某些裝置不相容的程式碼完整性。 我們強烈建議您在實驗室中測試此組態，再啟用這項功能。 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。 如需詳細資訊，請參閱 <<c0> [ 相容的硬體與 Windows Server 虛擬化保護的程式碼完整性](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一個步驟：** 
>[!div class="nextstepaction"]
[擷取 TPM 資訊](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>主機金鑰證明

使用主機金鑰證明的受防護的主機必須符合下列必要條件：

- **硬體**:任何伺服器，可使用 Windows Server 2019 執行 HYPER-V 的開頭
- **作業系統**:Windows Server 2019 Datacenter edition
- **角色和功能**:HYPER-V 角色和主機守護者 HYPER-V 支援功能 

主機可聯結至網域或工作群組。 

針對主機金鑰證明，必須執行 Windows Server 2019 和作業的 v2 證明 HGS。 如需詳細資訊，請參閱[HGS 必要條件](guarded-fabric-prepare-for-hgs.md#prerequisites)。 

**下一個步驟：** 
>[!div class="nextstepaction"]
[建立金鑰組](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>系統管理信任證明

>[!IMPORTANT]
>已被取代開頭為 Windows Server 2019 的系統管理信任證明 （AD 模式）。 TPM 證明不可能的環境下，設定[裝載金鑰證明](#host-key-attestation)。 主機金鑰證明提供類似的保證，能 AD 模式，並已設定的工作變得更容易。 

HYPER-V 主機必須符合下列必要條件為 AD 模式：

-   **硬體**:任何能夠執行 HYPER-V 開始使用 Windows Server 2016 的伺服器。 需要初始部署一部主機。 若要測試的受防護的 Vm 的 HYPER-V 即時移轉，您需要至少兩部主機。

-   **作業系統**:Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > 安裝[最新累積更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色和功能**:HYPER-V 角色和主機守護者 HYPER-V 支援功能，也就是只適用於 Windows Server 2016 Datacenter edition。 

> [!WARNING]
> 主機守護者 HYPER-V 支援功能可讓虛擬化保護可能與某些裝置不相容的程式碼完整性。 我們強烈建議您在實驗室中測試此組態，再啟用這項功能。 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。 如需詳細資訊，請參閱 <<c0> [ 相容的硬體與 Windows Server 2016 的虛擬化保護的程式碼完整性](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一個步驟：** 
>[!div class="nextstepaction"]
[在安全性群組中放置受防護的主機](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)