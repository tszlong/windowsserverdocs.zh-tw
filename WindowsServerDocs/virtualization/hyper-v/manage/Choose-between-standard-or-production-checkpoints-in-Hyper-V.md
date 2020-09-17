---
title: 在 Hyper-v 中的標準或生產檢查點之間進行選擇
description: 提供將虛擬機器設定為使用標準或生產檢查點的指示
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
ms.author: benarm
author: BenjaminArmstrong
ms.date: 10/04/2016
ms.openlocfilehash: 00a3c8d94fc18d180faa8927b33b90dc98854da0
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746473"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>在 Hyper-v 中的標準或生產檢查點之間進行選擇

>適用于： Windows 10、Windows Server 2016、Microsoft Hyper-V Server 2016、Windows Server 2019、Microsoft Hyper-V Server 2019


從 Windows Server 2016 和 Windows 10 開始，您可以在每個虛擬機器的標準和生產檢查點之間進行選擇。 生產檢查點是新虛擬機器的預設值。

- 生產檢查點是虛擬機器的「時間點」映射，可以在稍後以所有生產工作負載完全支援的方式還原。 這是利用在客體內使用備份技術建立檢查點，而不是使用儲存狀態技術來達成。

- 標準檢查點會取得執行中虛擬機器的狀態、資料和硬體設定，以供開發和測試案例使用。 如果您需要重新建立執行中虛擬機器的特定狀態或條件，讓您可以針對問題進行疑難排解，則標準檢查點可能會很有用。

  ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>將檢查點變更為生產或標準檢查點

1.  在 [ **Hyper-v 管理員**] 中，以滑鼠右鍵按一下虛擬機器，然後按一下 [ **設定**]。

2.  在 [ **管理** ] 區段下，選取 [ **檢查點**]。

3.  選取實際執行檢查點或標準檢查點。

    如果您選擇生產檢查點，也可以指定如果無法取得生產檢查點，主機是否應採用標準檢查點。 如果您清除此核取方塊，而且無法取得生產檢查點，則不會採取檢查點。

4.  如果您想要將檢查點設定檔儲存在不同的位置，請在 [ **檢查點檔案位置** ] 區段中加以變更。

5.  按一下 [套用] 以儲存 **您的變更** 。 如果您已經完成，請按一下 **[確定** ] 關閉對話方塊。

> [!NOTE]
> 執行 Active Directory Domain Services 角色 (網域控制站) 或 Active Directory 輕量型目錄服務角色的來賓僅支援 **生產檢查點** 。

## <a name="additional-references"></a>其他參考資料

-   [生產檢查點](../What-s-new-in-Hyper-V-on-Windows.md#production-checkpoints-new)

-   [啟用或停用檢查點](Enable-or-disable-checkpoints-in-Hyper-V.md)



