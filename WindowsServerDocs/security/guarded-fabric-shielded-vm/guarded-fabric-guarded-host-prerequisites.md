---
description: 深入瞭解：受防護主機的必要條件
title: 受防護主機的必要條件
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 6f6a4e111e5622ab0a78041e4e94c76f0a8acb13
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/10/2020
ms.locfileid: "97049776"
---
# <a name="prerequisites-for-guarded-hosts"></a>受防護主機的必要條件

>適用于： Windows Server 2019、Windows Server (半年通道) 、Windows Server 2016

檢查您所選證明模式的主機必要條件，然後按一下下一個步驟以新增受防護主機。

## <a name="tpm-trusted-attestation"></a>TPM-信任的證明

使用 TPM 模式的受防護主機必須符合下列必要條件：

-   **硬體**：需要有一部主機才能進行初始部署。 若要測試受防護 Vm 的 Hyper-v 即時移轉，您至少必須有兩部主機。

    主機必須有：

    - IOMMU 和第二層位址轉譯 (SLAT) 
    - TPM 2.0
    - UEFI 2.3.1 或更新版本
    - 設定為使用 UEFI 開機 (不是 BIOS 或「舊版」模式) 
    - 安全開機已啟用

-   **作業系統**： Windows Server 2016 Datacenter edition 或更新版本

    > [!IMPORTANT]
    > 請務必安裝 [最新的累積更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色和功能**： hyper-v 角色與主機守護者 hyper-v 支援功能。 主機守護者 Hyper-v 支援功能僅適用于 Windows Server Datacenter edition。

> [!WARNING]
> 主機守護者 Hyper-v 支援功能可針對可能與某些裝置不相容的程式碼完整性，啟用虛擬化型保護。
> 強烈建議您先在實驗室中測試此設定，再啟用此功能。
> 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。
> 如需詳細資訊，請參閱 [使用 Windows Server 虛擬化型的程式碼完整性保護的相容硬體](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：**
> [!div class="nextstepaction"]
> [Capture TPM 資訊](guarded-fabric-tpm-trusted-attestation-capturing-hardware.md)

## <a name="host-key-attestation"></a>主機金鑰證明

使用主機金鑰證明的受防護主機必須符合下列必要條件：

- **硬體**：任何能夠執行 hyper-v 的伺服器（從 Windows server 2019 開始）
- **作業系統**： Windows Server 2019 Datacenter 版本
- **角色和功能**： hyper-v 角色與主機守護者 hyper-v 支援功能

主機可以加入網域或工作組。

若為主機密鑰證明，HGS 必須執行 Windows Server 2019 並使用 v2 證明進行操作。 如需詳細資訊，請參閱 [HGS 必要條件](guarded-fabric-prepare-for-hgs.md#prerequisites)。

**下一步：**
> [!div class="nextstepaction"]
> [建立金鑰組](guarded-fabric-create-host-key.md)

## <a name="admin-trusted-attestation"></a>管理-信任的證明

>[!IMPORTANT]
>從 Windows Server 2019 開始，已淘汰 (AD 模式) 的管理員信任證明。 針對不可能進行 TPM 證明的環境，請設定 [主機金鑰證明](#host-key-attestation)。 主機金鑰證明提供類似于 AD 模式的保證，而且更容易設定。

Hyper-v 主機必須符合下列 AD 模式必要條件：

-   **硬體**：任何能夠執行 hyper-v 的伺服器，從 Windows server 2016 開始。 初始部署需要一個主機。 若要測試受防護 Vm 的 Hyper-v 即時移轉，您需要至少兩部主機。

-   **作業系統**： Windows Server 2016 Datacenter 版本

    > [!IMPORTANT]
    > 安裝 [最新的累積更新](https://support.microsoft.com/help/4000825/windows-10-and-windows-server-2016-update-history)。

-   **角色和功能**： hyper-v 角色與主機守護者 hyper-v 支援功能，僅適用于 Windows Server 2016 Datacenter edition。

> [!WARNING]
> 主機守護者 Hyper-v 支援功能可針對可能與某些裝置不相容的程式碼完整性，啟用虛擬化型保護。
> 強烈建議您先在實驗室中測試此設定，再啟用此功能。
> 若未能這麼做，可能會造成非預期的失敗，最嚴重可能包括資料遺失或藍色畫面錯誤 (也稱為停止錯誤)。
> 如需詳細資訊，請參閱 [使用 Windows Server 2016 以虛擬化為基礎的程式碼完整性保護的相容硬體](guarded-fabric-compatible-hardware-with-virtualization-based-protection-of-code-integrity.md)。

**下一步：**
> [!div class="nextstepaction"]
> [將受防護主機放置在安全性群組中](guarded-fabric-admin-trusted-attestation-creating-a-security-group.md)
