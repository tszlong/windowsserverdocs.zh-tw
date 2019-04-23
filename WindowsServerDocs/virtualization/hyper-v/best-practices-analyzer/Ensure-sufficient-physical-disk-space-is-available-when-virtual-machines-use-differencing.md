---
title: 確保足夠的實體磁碟空間時才可以使用虛擬機器使用差異虛擬硬碟
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855569"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>確保足夠的實體磁碟空間時才可以使用虛擬機器使用差異虛擬硬碟

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
*一或多個虛擬機器正在使用差異虛擬硬碟。*  
  
## <a name="impact"></a>影響  
*差異虛擬硬碟需要裝載的磁碟區上的可用空間，以便將虛擬硬碟的寫入發生時可以配置的空間。如果已用完可用空間，可能會影響依賴的實體儲存體的任何虛擬機器。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*監視可用磁碟空間，以確保有足夠的空間可供虛擬硬碟的擴充。請考慮合併至其父代差異虛擬硬碟。在 [HYPER-V 管理員] 中，檢查以判斷父虛擬硬碟的差異磁碟。如果您合併至共用的其他差異磁碟父系磁碟的差異磁碟時，該動作會損毀之間其他差異磁碟父系磁碟，使其無法使用的關聯性。在驗證之後不會共用父虛擬硬碟，您可以使用 [編輯磁碟精靈] 合併差異磁碟父系虛擬硬碟。*  
  


