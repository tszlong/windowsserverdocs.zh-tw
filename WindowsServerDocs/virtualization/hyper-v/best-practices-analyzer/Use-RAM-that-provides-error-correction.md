---
title: 使用提供錯誤修正的 RAM
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 67eb6cef-b045-4748-90e1-406af5345d6a
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b220465aed0cf9c634eb35424709195ced0d6df9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871119"
---
# <a name="use-ram-that-provides-error-correction"></a>使用提供錯誤修正的 RAM

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
  
*在此電腦上使用的 RAM 沒有錯誤修正 (ECC) 的 RAM。*  
  
## <a name="impact"></a>影響  
  
*Microsoft 不支援 Windows Server 2016 的電腦上沒有任何錯誤修正的 RAM。*  
  
## <a name="resolution"></a>解析度  
  
*請確認伺服器已在 Windows Server catalog 中列出且限定適用於 HYPER-V。*  
  
若要檢查是否列出的伺服器，請參閱[Windows Server catalog](https://www.windowsservercatalog.com/)。  
  


