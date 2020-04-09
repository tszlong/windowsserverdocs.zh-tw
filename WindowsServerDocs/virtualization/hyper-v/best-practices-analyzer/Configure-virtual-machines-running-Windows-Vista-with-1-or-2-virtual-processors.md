---
title: 使用1或2個虛擬處理器設定執行 Windows Vista 的虛擬機器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0319039dfc26d59b1045ffc60f52e56eb900ff76
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862021"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>使用1或2個虛擬處理器設定執行 Windows Vista 的虛擬機器

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|組態|  
|**類別**|錯誤|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*執行 Windows Vista 的虛擬機器設定了2個以上的虛擬處理器。*  
  
## <a name="impact"></a>影響  
  
*Microsoft 不支援下列虛擬機器的設定：*  
  
\<虛擬機器名稱清單 >  
  
## <a name="resolution"></a>解析度  
  
*關閉虛擬機器，並移除一或多個虛擬處理器。*  
  
### <a name="to-remove-virtual-processors"></a>若要移除虛擬處理器  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [**關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [**關機**]。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在流覽窗格中，按一下 [**處理器**]。  
  
5.  在 [**處理器**] 頁面上，將處理器數目設定為**1**或**2** ，然後按一下 **[確定]** 。  
  


