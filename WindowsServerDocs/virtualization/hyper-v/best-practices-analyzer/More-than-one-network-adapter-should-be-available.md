---
title: 應可使用多張網路介面卡
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 56cb747ac44d48b115dbf105ea96e4623d458b28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861891"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>應可使用多張網路介面卡

>適用於︰Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|錯誤|  
|**類別**|組態|  

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題  
  
*此伺服器已設定一張網路介面卡，必須由管理作業系統和需要存取實體網路的所有虛擬機器共用。*  
  
## <a name="impact"></a>影響  
  
*管理作業系統中的網路效能可能會降低。*  
  
## <a name="resolution"></a>解析度  
  
*新增更多網路介面卡到這部電腦。若要保留一張網路介面卡供管理作業系統獨佔使用，請勿將其設定為與外部虛擬網路搭配使用。*  
  
如需將網路介面卡新增到電腦的相關資訊，請參閱電腦或網路介面卡的檔。 然後，若要以獨佔方式將它保留給管理作業系統，請不要將它連線到虛擬交換器。   
  


