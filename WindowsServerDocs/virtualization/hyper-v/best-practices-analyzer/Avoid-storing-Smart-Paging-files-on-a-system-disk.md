---
title: 避免將儲存為系統磁碟機上的 智慧型分頁處理檔案
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6abc84b406de7e7c33628ccee4e3af706efe5c70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886169"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>避免將儲存為系統磁碟機上的 智慧型分頁處理檔案

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的文字。  
  
## <a name="issue"></a>問題  
*如果虛擬機器重新開機時，和智慧型分頁處理檔案的指定的位置是執行 HYPER-V 之伺服器的系統磁碟，一或多個虛擬機器的記憶體組態可能需要使用智慧型分頁處理。*  
  
## <a name="impact"></a>影響  
*系統磁碟使用智慧型分頁處理可能會導致執行 HYPER-V 時遇到問題的伺服器。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*重新設定虛擬機器，來儲存智慧型分頁處理檔案，為非系統磁碟機上。*  
  


