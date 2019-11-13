---
title: 當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函數驅動程式運作正常
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5e2c666973aa1ac0d5eb2c4e0d5d29793dc0ce75
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393621"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>當虛擬機器設定為使用 SR-IOV 時，請確定虛擬函數驅動程式運作正常

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*虛擬函式驅動程式未在一或多部虛擬機器的客體作業系統中正常運作。*  
  
## <a name="impact"></a>影響  
*下列虛擬機器的網路效能不佳：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*在 [來賓] 作業系統中，執行下列動作：確認已安裝適當的驅動程式，並已啟用所有網路裝置，並檢查事件記錄檔中的錯誤或警告。*  
  


