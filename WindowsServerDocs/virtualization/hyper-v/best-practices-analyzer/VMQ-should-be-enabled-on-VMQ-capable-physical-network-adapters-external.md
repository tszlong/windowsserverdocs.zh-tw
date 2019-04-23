---
title: 支援 VMQ 的繫結至外部虛擬交換器的實體網路介面卡上應該啟用 VMQ
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889469"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>支援 VMQ 的繫結至外部虛擬交換器的實體網路介面卡上應該啟用 VMQ

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*下列的網路介面卡的虛擬機器佇列 (VMQ) 支援但功能已停用。*  
  
## <a name="impact"></a>**影響**  
*Windows 是無法充分利用可用的硬體卸載下列網路介面卡上：*  
  
\<網路介面卡清單 >  
  
## <a name="resolution"></a>**解決方法**  
*啟用使用 Enable-netadaptervmq Windows PowerShell cmdlet 或使用 進階內容使用者介面的網路介面卡的 VMQ。*  
  


