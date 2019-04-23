---
title: 避免虛擬區塊和動態虛擬硬碟或差異磁碟上的實體磁碟磁區對齊方式不一致
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4973c199a5507d00e15da8f621a09f0c602a29fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833859"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免虛擬區塊和動態虛擬硬碟或差異磁碟上的實體磁碟磁區對齊方式不一致

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
*對齊方式不一致之處偵測到的一或多個虛擬的硬碟。*  
  
### <a name="impact"></a>影響  
*如果虛擬硬碟儲存在實體磁碟的磁區大小為 4k、 虛擬機器或使用虛擬硬碟的應用程式，可能會遇到效能問題。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用 [建立虛擬硬碟精靈] 建立新的 VHD 格式或 VHDX 格式虛擬硬碟，並指定為來源磁碟的現有虛擬硬碟。新的虛擬硬碟將會建立與虛擬區塊與實體磁碟之間的對齊方式。*  
  


