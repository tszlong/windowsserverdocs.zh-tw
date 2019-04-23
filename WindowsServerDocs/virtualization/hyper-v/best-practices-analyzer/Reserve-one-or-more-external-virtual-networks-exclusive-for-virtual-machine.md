---
title: 保留一或多個外部虛擬網路專用的虛擬機器
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c8c90a74352bae0b348608db0fc05107e4d09010
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884739"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>保留一或多個外部虛擬網路專用的虛擬機器

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*所有的外部虛擬網路設定成使用由管理作業系統和虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*在管理作業系統中，可能會降低網路效能。*  
  
## <a name="resolution"></a>解析度  
  
*若要停止與管理作業系統共用的外部虛擬網路中使用虛擬交換器管理員。*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>若要停止與管理作業系統共用的外部虛擬網路  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  從 [動作] 功能表上，按一下 [虛擬交換器管理員]。  
  
3.  底下**虛擬交換器**，按一下 外部虛擬交換器的名稱。  
  
4.  在 **連線類型**區域中，在實體網路介面卡的名稱下清除**允許管理作業系統共用此網路介面卡**核取方塊。  
  
5.  按一下 [確定] 。  
  


