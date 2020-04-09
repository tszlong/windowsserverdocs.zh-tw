---
title: 避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 3f090f015f2179ba372e56d580477ef8d72d977f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857801"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*偵測到一或多個虛擬硬碟的對齊不一致。*  
  
### <a name="impact"></a>影響  
*如果虛擬硬碟儲存在磁區大小為4K 的實體磁片上，則使用虛擬硬碟的虛擬機器或應用程式可能會遇到效能問題。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用 [建立虛擬硬碟] 嚮導來建立新的 VHD 格式或 VHDX 格式的虛擬硬碟，並將現有的虛擬硬碟指定為來源磁片。將會建立新的虛擬硬碟，並在虛擬區塊和實體磁片之間進行對齊。*  
  


