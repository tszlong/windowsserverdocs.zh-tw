---
title: 使用提供錯誤更正的 RAM
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cd12125307c99956b9207a1e56c8d5f66a2e6bd2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854221"
---
# <a name="use-ram-that-provides-error-correction"></a>使用提供錯誤更正的 RAM

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
  
*這部電腦上使用的 RAM 不是錯誤更正（ECC） RAM。*  
  
## <a name="impact"></a>影響  
  
*Microsoft 不支援電腦上的 Windows Server 2016，而不會錯誤修正 RAM。*  
  
## <a name="resolution"></a>解析度  
  
*確認伺服器列在 Windows Server catalog 中，並且適用于 Hyper-v。*  
  
若要檢查是否列出伺服器，請參閱[Windows server catalog](https://www.windowsservercatalog.com/)。  
  


