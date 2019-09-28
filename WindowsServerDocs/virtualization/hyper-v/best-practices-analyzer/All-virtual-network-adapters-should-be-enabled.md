---
title: 應啟用所有虛擬網路介面卡
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fce564fdb47d0677b36078f3d8446579bc06816c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366587"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>應啟用所有虛擬網路介面卡

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*與實體網路介面卡相關聯的一或多個虛擬網路介面卡，在管理作業系統中已停用。*  
  
## <a name="impact"></a>影響  
  
*此伺服器的設定不是最佳設定。*  
  
管理作業系統無法使用這部電腦上的其中一個實體網路介面卡連線到實體（外部）網路，因為它與已停用的虛擬網路介面卡相關聯。  
  
## <a name="resolution"></a>解析度  
  
@no__t 0Use 網路 & 網際網路設定 以啟用虛擬網路介面卡。或者，使用虛擬交換器管理員重新設定外部虛擬交換器，使其不會與管理作業系統共用。 *  
  


