---
title: 受防護主機的必要條件
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 81a78d5af23329e2b1972a93582accc36277f8a5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953634"
---
# <a name="prerequisites-for-guarded-hosts"></a>受防護主機的必要條件

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

檢查您所選擇之證明模式的主機必要條件，然後按一下下一步以新增受防護的主機。

## <a name="tpm-trusted-attestation"></a>TPM 信任證明

使用 TPM 模式的受防護主機必須符合下列必要條件：

-   **硬體**：初始部署需要一部主機。 若要測試受防護 Vm 的 Hyper-v 即時移轉，您至少必須有兩部主機。

    主機必須有：

    - IOMMU 和第二層位址轉譯 (SLAT) 
    - TPM 2.0
    - UEFI 2.3.1 或更新版本
    - 設定為使用 UEFI (而非 BIOS 或「舊版」模式開機) 
    - 已啟用安全開機

-   **作業系統**： Windows Server 2016 Datacenter edition 或更新版本

    > [!IMPORTANT]
    > 請確定您已安裝[最新的累計更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色與功能**： hyper-v 角色與主機守護者 hyper-v 支援功能。 主機守護者 Hyper-v 支援功能僅適用于 Windows Server 的 Datacenter edition。

> [!WARNING]
> 主機守護者 Hyper-v 支援功能可啟用程式碼完整性的虛擬化保護，這可能會與某些裝置不相容。
> 我們強烈建議您先在實驗室中測試這項設定，再啟用此功能。
> 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。
> 如需詳細資訊，請參閱[具有 Windows Server 虛擬化型保護程式碼完整性的相容硬體](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：**
> [!div class="nextstepaction"]
> [捕捉 TPM 資訊](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>主機金鑰證明

使用主機金鑰證明的受防護主機必須符合下列必要條件：

- **硬體**：任何能夠執行 hyper-v 的伺服器，從 Windows server 2019 開始
- **作業系統**： Windows Server 2019 Datacenter edition
- **角色與功能**： hyper-v 角色與主機守護者 hyper-v 支援功能

主機可以聯結至網域或工作組。

若為主機密鑰證明，HGS 必須執行 Windows Server 2019 並與 v2 證明搭配運作。 如需詳細資訊，請參閱[HGS 必要條件](guarded-fabric-prepare-for-hgs.md#prerequisites)。

**下一步：**
> [!div class="nextstepaction"]
> [建立金鑰組](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>管理員-信任的證明

>[!IMPORTANT]
>從 Windows Server 2019 開始，系統管理員信任的證明 (AD 模式) 已淘汰。 針對不可能進行 TPM 證明的環境，請設定[主機金鑰證明](#host-key-attestation)。 主機金鑰證明提供與 AD 模式類似的保證，而且設定起來較簡單。

Hyper-v 主機必須符合下列 AD 模式的必要條件：

-   **硬體**：任何能夠從 Windows server 2016 開始執行 hyper-v 的伺服器。 初始部署需要一部主機。 若要測試受防護 Vm 的 Hyper-v 即時移轉，您至少需要兩部主機。

-   **作業系統**： Windows Server 2016 Datacenter edition

    > [!IMPORTANT]
    > 安裝[最新的累計更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色與功能**： hyper-v 角色與主機守護者 hyper-v 支援功能（僅適用于 Windows Server 2016 Datacenter edition）。

> [!WARNING]
> 主機守護者 Hyper-v 支援功能可啟用程式碼完整性的虛擬化保護，這可能會與某些裝置不相容。
> 我們強烈建議您先在實驗室中測試這項設定，再啟用此功能。
> 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。
> 如需詳細資訊，請參閱[相容硬體與 Windows Server 2016 虛擬化型的程式碼完整性保護](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：**
> [!div class="nextstepaction"]
> [將受防護主機放在安全性群組中](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)