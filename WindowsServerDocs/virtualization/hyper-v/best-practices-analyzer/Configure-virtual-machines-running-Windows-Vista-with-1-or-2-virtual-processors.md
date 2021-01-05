---
title: 使用1或2個虛擬處理器來設定執行 Windows Vista 的虛擬機器
description: 瞭解當執行 Windows Vista 的虛擬機器設定了2個以上的虛擬處理器時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
ms.date: 8/16/2016
ms.openlocfilehash: 3e49a55ed27d62fa955800867a7a4e3c9dacf319
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846414"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>使用1或2個虛擬處理器來設定執行 Windows Vista 的虛擬機器

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|組態|
|**類別**|錯誤|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*執行 Windows Vista 的虛擬機器設定超過2個虛擬處理器。*

## <a name="impact"></a>影響

*Microsoft 不支援下列虛擬機器的設定：*

\<list of virtual machine names>

## <a name="resolution"></a>解決方案

*關閉虛擬機器，並移除一或多個虛擬處理器。*

### <a name="to-remove-virtual-processors"></a>移除虛擬處理器

1.  開啟 Hyper-V 管理員。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。

2.  在結果窗格的 [ **虛擬機器**] 下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [ **關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [ **關機**]。

3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。

4.  在流覽窗格中，按一下 [ **處理器**]。

5.  在 [ **處理器** ] 頁面上，將處理器的數目設定為 **1** 或 **2** ，然後按一下 **[確定]**。



