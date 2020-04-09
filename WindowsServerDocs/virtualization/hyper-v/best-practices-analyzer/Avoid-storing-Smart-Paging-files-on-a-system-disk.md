---
title: 避免在系統磁片上儲存智慧型分頁檔案
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 111cf3b3b95f50d6d36a6b30b5a0bb46e255ee28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857731"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>避免在系統磁片上儲存智慧型分頁檔案

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的文字。  
  
## <a name="issue"></a>問題  
*一或多部虛擬機器的記憶體設定可能需要在虛擬機器重新開機時使用智慧型分頁，而智慧分頁檔案的指定位置是執行 Hyper-v 之伺服器的系統磁片。*  
  
## <a name="impact"></a>影響  
*使用系統磁片進行智慧型分頁可能會導致執行 Hyper-v 的伺服器發生問題。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*重新設定虛擬機器，將智慧分頁檔案儲存在非系統磁片上。*  
  


