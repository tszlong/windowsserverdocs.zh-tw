---
title: 請確定虛擬函式的驅動程式運作正常的虛擬機器設定為使用 SR-IOV 時
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d3d0a5008b55d4823cef9a8dd2a7bce4a6a2a33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852079"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>請確定虛擬函式的驅動程式運作正常的虛擬機器設定為使用 SR-IOV 時

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
*客體作業系統中一或多個虛擬機器的虛擬函式的驅動程式未正確運作。*  
  
## <a name="impact"></a>影響  
*網路效能並非最佳化下列虛擬機器上：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*在 客體作業系統中，執行下列作業：請確認已安裝適當的驅動程式和所有網路裝置已啟用，並檢查事件記錄檔，取得錯誤或警告。*  
  


