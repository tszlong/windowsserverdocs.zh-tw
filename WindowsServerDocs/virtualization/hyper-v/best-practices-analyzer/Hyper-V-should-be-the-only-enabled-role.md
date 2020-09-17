---
title: Hyper-v 應該是唯一啟用的角色
description: 提供指示以解決這個最佳做法分析程式規則所報告的問題。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
ms.date: 8/16/2016
ms.openlocfilehash: a59f4ebd1bf3ce7ce93d2eb098302b5bd2c42cce
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746833"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-v 應該是唯一啟用的角色

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*此伺服器上已啟用 Hyper-v 以外的角色。*

在大多數情況下，在執行 Hyper-v 角色的伺服器上安裝其他角色並不是個好主意。 遠端桌面虛擬主機角色服務是例外狀況，因為它是遠端桌面服務角色的一部分，而且需要在相同的伺服器上安裝 Hyper-v。

## <a name="impact"></a>影響

*Hyper-v 角色應該是伺服器上唯一啟用的角色。*

這項最佳作法有助於讓主機作業系統不需要執行 Hyper-v 的角色、功能和應用程式。 遵循此最佳做法，並在 Nano Server 上執行 Hyper-v 有助於減少您需要的更新數目，因為只有 Nano Server、Hyper-v 服務元件和 Windows 虛擬程式會受限於軟體更新。

## <a name="resolution"></a>解決方案

*使用伺服器管理員移除 Hyper-v 以外的所有角色。*

伺服器管理員包含 [移除角色] Wizard。 此嚮導可讓您一次移除一個以上的角色。 移除角色之前，[移除角色] 嚮導會檢查相依性，以降低移除其他角色所依賴之軟體的風險。 如果找到相依性，則嚮導會提示您核准移除已安裝角色所需的其他角色、角色服務或軟體。

若要使用伺服器管理員，您必須以系統管理員身分登入電腦。

#### <a name="to-remove-a-role"></a>移除角色

1.  使用 [ **開始** ] 功能表、Windows 工作列或 [系統管理工具] 中的快捷方式開啟伺服器管理員。
2.   在伺服器管理員主視窗的 [ **角色摘要** ] 區域中，按一下 [ **移除角色**]。 依照嚮導中的指示移除角色。





