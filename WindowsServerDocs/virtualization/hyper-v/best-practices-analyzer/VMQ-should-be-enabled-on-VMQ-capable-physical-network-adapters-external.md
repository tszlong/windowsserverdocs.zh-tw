---
title: 必須在系結至外部虛擬交換器的具備 VMQ 功能的實體網路介面卡上啟用 VMQ
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a66c1c4580f8ecda90caa5446e74bc9b12ac0476
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855041"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>必須在系結至外部虛擬交換器的具備 VMQ 功能的實體網路介面卡上啟用 VMQ

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*下列網路介面卡具備虛擬機器佇列（VMQ）的能力，但功能已停用。*  
  
## <a name="impact"></a>**產生**  
*Windows 無法充分利用下列網路介面卡上可用的硬體卸載：*  
  
\<的網路介面卡清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Get-netadaptervmq Windows PowerShell Cmdlet 或使用網路介面卡的 Advanced Properties 使用者介面來啟用 VMQ。*  
  


