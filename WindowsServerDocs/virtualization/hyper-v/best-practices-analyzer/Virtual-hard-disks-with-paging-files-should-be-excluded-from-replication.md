---
title: 應從複寫排除分頁檔使用的虛擬硬碟
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850119"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>應從複寫排除分頁檔使用的虛擬硬碟

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|資訊|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
*分頁檔應該排除參與複寫，但沒有磁碟已被排除。*  
  
## <a name="impact"></a>影響  
*分頁檔遭遇大量輸入/輸出活動，而這不必要地需要多大的資源，參與複寫。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*如果您尚未這樣做，請建立個別虛擬硬碟的 Windows 分頁檔。如果已完成初始複寫之後，使用 HYPER-V 管理員來移除複寫。然後，再次設定複寫，並排除分頁檔從複寫的虛擬硬碟。*  
  


