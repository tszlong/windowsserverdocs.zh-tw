---
title: 應該啟用所有虛擬網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0a769c3203f6c6946f01cd91b66fbec38af83bbd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837129"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>應該啟用所有虛擬網路介面卡

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*管理作業系統中，與實體網路介面卡相關聯的一或多個虛擬網路介面卡已停用的。*  
  
## <a name="impact"></a>影響  
  
*此伺服器的設定不是最佳的。*  
  
管理作業系統無法連接到其中一個實體網路介面卡使用這台電腦上，因為它有已停用的虛擬網路介面卡關聯的實體 （外部） 網路。  
  
## <a name="resolution"></a>解析度  
  
*若要啟用虛擬網路介面卡使用網路和網際網路設定。或者，您也可以使用虛擬交換器管理員 來重新設定外部虛擬交換器，讓它不會與管理作業系統共用。*  
  


