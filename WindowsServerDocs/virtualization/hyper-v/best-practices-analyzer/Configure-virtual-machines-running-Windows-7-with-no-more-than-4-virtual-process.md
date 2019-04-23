---
title: 設定執行 Windows 7 不超過 4 個虛擬處理器與虛擬機器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ee450f3510e6dcc1d0a32ed5d5a0be549ac8809e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848039"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>設定執行 Windows 7 不超過 4 個虛擬處理器與虛擬機器

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*執行 Windows 7 的虛擬機器已使用 4 個以上的虛擬處理器。*  
  
## <a name="impact"></a>**影響**  
*Microsoft 不支援下列虛擬機器的組態：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*關閉虛擬機器，並移除一或多個虛擬處理器。*  
  
#### <a name="to-remove-virtual-processors"></a>若要移除的虛擬處理器  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格中，在**虛擬機器**，選取您想要設定的虛擬機器。 虛擬機器的狀態應該會列為**關閉**。 如果沒有，請以滑鼠右鍵按一下 虛擬機器，然後按一下**關機**。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在 [導覽] 窗格中，按一下**處理器**。  
  
5.  在上**處理器**頁面上，設定處理器數目**3**或更少，然後按一下**確定**。  
  


