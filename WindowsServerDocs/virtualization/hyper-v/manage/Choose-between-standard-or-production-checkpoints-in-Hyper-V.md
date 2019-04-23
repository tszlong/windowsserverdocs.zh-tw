---
title: 在 HYPER-V 中的標準或生產檢查點之間選擇
description: 提供指示來設定虛擬機器可讓使用標準或生產檢查點
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 92bb573b-03b7-470e-b72e-e35edf52b349
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 239cce3c9f1acb2d45935e0f60fb1875b004485b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880949"
---
# <a name="choose-between-standard-or-production-checkpoints-in-hyper-v"></a>在 HYPER-V 中的標準或生產檢查點之間選擇

>適用於：Windows 10，Windows Server 2016、 Microsoft HYPER-V Server 2016、windows Server 2019，Microsoft HYPER-V Server 2019

  
從 Windows Server 2016 和 Windows 10 開始，您可以選擇每個虛擬機器的標準與實際執行檢查點之間。 生產檢查點是新的虛擬機器的預設值。
  
- 生產檢查點是虛擬機器，完全支援所有的生產工作負載的方式可在稍後還原的 「 時間點 」 映像。 這是利用在客體內使用備份技術建立檢查點，而不是使用儲存狀態技術來達成。  
  
- 標準檢查點會擷取執行中虛擬機器的狀態、 資料和硬體組態，並僅供開發和測試案例中使用。 標準檢查點可以是您需要重新建立特定狀態或條件的執行中虛擬機器，以便您可以針對問題進行疑難排解時相當實用。  
 
 ## <a name="change-checkpoints-to-production-or-standard-checkpoints"></a>將檢查點變更為生產環境或標準檢查點  
  
1.  在  **HYPER-V 管理員**，以滑鼠右鍵按一下 虛擬機器，然後按一下**設定**。  
  
2.  底下**管理**區段中，選取**檢查點**。  
  
3.  選取實際執行檢查點或標準檢查點。  
  
    如果您選擇生產檢查點時，您也可以指定如果無法建立生產檢查點，主機是否應執行標準檢查點。 如果您清除此核取方塊，而且無法建立生產檢查點，然後會不建立任何檢查點。  
  
4.  如果您想要儲存檢查點組態檔中的不同位置，將它中變更**檢查點檔案位置**一節。  
  
5.  按一下 **套用**以儲存變更。 如果您完成時，按一下**確定**以關閉對話方塊。  
  
> [!NOTE]
> 只有**生產檢查點**支援在執行 Active Directory 網域服務角色 （在 網域控制站中） 或 Active Directory 輕量型目錄服務角色的來賓。

## <a name="see-also"></a>另請參閱  
  
-   [生產檢查點](../What-s-new-in-Hyper-V-on-Windows.md#BKMK_check)  
  
-   [啟用或停用檢查點](Enable-or-disable-checkpoints-in-Hyper-V.md)  
  


