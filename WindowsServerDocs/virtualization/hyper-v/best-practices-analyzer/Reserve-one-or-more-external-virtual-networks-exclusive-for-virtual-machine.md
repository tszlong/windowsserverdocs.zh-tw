---
title: 保留一或多個外部虛擬網路供虛擬機器獨佔使用
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: f7732258-93f1-44e8-835b-5ad2d1c45cd9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: a72f3d616bb0c520e49c27f90686196463f25953
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364785"
---
# <a name="reserve-one-or-more-external-virtual-networks-for-exclusive-use-by-virtual-machines"></a>保留一或多個外部虛擬網路供虛擬機器獨佔使用

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*所有外部虛擬網路都設定為可供管理作業系統和虛擬機器使用。*  
  
## <a name="impact"></a>影響  
  
*管理作業系統中的網路效能可能會降低。*  
  
## <a name="resolution"></a>解析度  
  
*使用虛擬交換器管理員，以停止與管理作業系統共用外部虛擬網路。*  
  
#### <a name="to-stop-sharing-the-external-virtual-network-with-the-management-operating-system"></a>停止與管理作業系統共用外部虛擬網路  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  從 [動作] 功能表上，按一下 [虛擬交換器管理員]。  
  
3.  在 [**虛擬交換器**] 底下，按一下外部虛擬交換器的名稱。  
  
4.  在 [連線**類型**] 區域中，于實體網路介面卡的名稱底下，清除 [**允許管理作業系統共用此網路介面卡**] 核取方塊。  
  
5.  按一下 [確定]。  
  


