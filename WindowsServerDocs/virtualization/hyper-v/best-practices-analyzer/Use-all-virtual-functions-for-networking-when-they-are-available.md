---
title: 使用所有虛擬函式來取得可用的網路功能
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 0fab06ae21a4632df73b7a4d8b17b12665ffed98
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854211"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>使用所有虛擬函式來取得可用的網路功能

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*未使用某些硬體加速功能*  
  
## <a name="impact"></a>影響  
*此設定可能會導致整體 CPU 使用率高於所需。下列虛擬機器的網路效能可能不佳：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果實體硬體支援 SR-IOV，而且此設定不會與虛擬機器所需的網路功能發生衝突，請考慮設定 SR-IOV 的虛擬網路介面卡。*  
  


