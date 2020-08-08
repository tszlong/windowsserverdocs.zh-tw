---
title: Hyper-v 應該是唯一啟用的角色
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 5a0ed176-048f-40b1-b56c-8391b805fd37
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2e2c392ad75f4f0c84216db637a0106be8821b41
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968335"
---
# <a name="hyper-v-should-be-the-only-enabled-role"></a>Hyper-v 應該是唯一啟用的角色

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*此伺服器上已啟用 Hyper-v 以外的角色。*

在大多數情況下，在執行 Hyper-v 角色的伺服器上安裝其他角色並不是個好主意。 遠端桌面虛擬主機角色服務是例外狀況，因為它是遠端桌面服務角色的一部分，而且需要將 Hyper-v 安裝在相同的伺服器上。

## <a name="impact"></a>影響

*Hyper-v 角色應該是伺服器上唯一啟用的角色。*

這種最佳作法有助於讓主機作業系統不需要執行 Hyper-v 所需的角色、功能和應用程式。 遵循此最佳做法並在 Nano Server 上執行 Hyper-v 有助於減少所需的更新數目，因為只有 Nano Server、Hyper-v 服務元件和 Windows 虛擬服務管理者會受到軟體更新的需求。

## <a name="resolution"></a>解決方法

*使用伺服器管理員移除 Hyper-v 以外的所有角色。*

伺服器管理員包含 [移除角色] Wizard。 此嚮導可讓您一次移除一個以上的角色。 移除角色之前，「移除角色」 Wizard 會檢查相依性，以降低移除其他角色所依賴之軟體的風險。 如果找到相依性，則 wizard 會提示您核准移除已安裝角色所需的其他角色、角色服務或軟體。

若要使用伺服器管理員，您必須以系統管理員的身分登入電腦。

#### <a name="to-remove-a-role"></a>移除角色

1.  使用 [**開始**] 功能表、Windows 工作列或 [系統管理工具] 中的快捷方式來開啟伺服器管理員。
2.   在伺服器管理員主視窗的 [**角色摘要**] 區域中，按一下 [**移除角色**]。 遵循嚮導中的指示來移除角色。





