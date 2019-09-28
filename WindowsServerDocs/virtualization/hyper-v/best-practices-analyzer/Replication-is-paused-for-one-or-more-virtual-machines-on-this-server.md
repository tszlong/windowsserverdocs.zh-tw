---
title: 這部伺服器上的一或多部虛擬機器的複寫已暫停
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 17d50f116c6cee488367c924bfbce3791a8d879f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393538"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>這部伺服器上的一或多部虛擬機器的複寫已暫停

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|作業|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
已暫停一或多部虛擬機器的 @no__t 0Replication。當主要虛擬機器暫停時，發生的任何變更都會累積，並在複寫繼續後傳送到複本虛擬機器。 *  
  
## <a name="impact"></a>影響  
*As 只要複寫已暫停，主要虛擬機器中所發生的累積變更就會耗用主伺服器上的可用磁碟空間。繼續複寫之後，可能會有大量高載的網路流量傳送到複本伺服器。這會影響下列虛擬機器：*  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
@no__t 暫停複寫的0Confirm。如果複寫已暫停以解決磁碟空間不足或網路連線能力，請在解決這些問題後立即繼續複寫。 *  
  


