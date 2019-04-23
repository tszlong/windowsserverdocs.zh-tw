---
title: 應可使用多張網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884599"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>應可使用多張網路介面卡

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  

在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。

## <a name="issue"></a>問題  
  
*此伺服器被設定一個網路介面卡，這必須由管理作業系統共用及所有需要存取實體網路的虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*在管理作業系統中，可能會降低網路效能。*  
  
## <a name="resolution"></a>解析度  
  
*將多個網路介面卡新增到此電腦。管理作業系統中保留一個專用的網路介面卡，請勿用於外部虛擬網路中設定它。*  
  
如需將網路介面卡新增至 電腦資訊，請參閱電腦或網路介面卡的文件。 然後，保留它專屬於管理作業系統，不會連線到虛擬交換器。   
  


