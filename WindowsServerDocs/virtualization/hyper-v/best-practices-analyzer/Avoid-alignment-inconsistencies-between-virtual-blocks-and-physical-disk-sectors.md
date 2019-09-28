---
title: 避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f77d80db1e2454eb460043cacef632e979fc16de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366488"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*偵測到一或多個虛擬硬碟的對齊不一致。*  
  
### <a name="impact"></a>影響  
@no__t 0If 虛擬硬碟儲存在磁區大小為4K 的實體磁片上，則使用虛擬硬碟的虛擬機器或應用程式可能會遇到效能問題。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
*Use 建立虛擬硬碟嚮導 來建立新的 VHD 格式或 VHDX 格式的虛擬硬碟，並將現有的虛擬硬碟指定為來源磁片。將建立新的虛擬硬碟，並在虛擬區塊和實體磁片之間進行對齊。*  
  


