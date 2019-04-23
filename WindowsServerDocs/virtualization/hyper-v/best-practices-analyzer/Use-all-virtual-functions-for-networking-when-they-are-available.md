---
title: 使用網路可供使用時的所有虛擬函式
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3ad120ffa689f1f7dcae832432e216ebda57e62f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877789"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>使用網路可供使用時的所有虛擬函式

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*不被使用某些硬體加速功能*  
  
## <a name="impact"></a>影響  
*這項設定可能會導致整體 CPU 使用率高於所需。網路效能可能會無法在下列虛擬機器上達到最佳：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*請考慮設定虛擬網路介面卡的 SR-IOV，如果實體硬體支援 SR-IOV，如果此設定不會衝突與虛擬機器所需的網路功能。*  
  


