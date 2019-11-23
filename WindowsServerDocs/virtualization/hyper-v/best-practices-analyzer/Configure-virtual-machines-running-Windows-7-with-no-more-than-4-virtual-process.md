---
title: 設定執行 Windows 7 的虛擬機器，且不超過4個虛擬處理器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 14b5e0637ad2e6462e13f0e1f18af651bbcc5fc3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366274"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>設定執行 Windows 7 的虛擬機器，且不超過4個虛擬處理器

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*執行 Windows 7 的虛擬機器設定了4個以上的虛擬處理器。*  
  
## <a name="impact"></a>**產生**  
*Microsoft 不支援下列虛擬機器的設定：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*關閉虛擬機器，並移除一或多個虛擬處理器。*  
  
#### <a name="to-remove-virtual-processors"></a>若要移除虛擬處理器  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。 虛擬機器的狀態應列為 [**關閉**]。 如果不是，請在虛擬機器上按一下滑鼠右鍵，然後按一下 [**關機**]。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在流覽窗格中，按一下 [**處理器**]。  
  
5.  在 [**處理器**] 頁面上，將處理器數目設定為**3**或更少，然後按一下 **[確定]** 。  
  


