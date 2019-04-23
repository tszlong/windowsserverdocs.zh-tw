---
title: 避免當父系和子系虛擬硬碟位於不同的磁碟區時，使用差異虛擬硬碟時，啟用儲存體服務品質
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856199"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>避免當父系和子系虛擬硬碟位於不同的磁碟區時，使用差異虛擬硬碟時，啟用儲存體服務品質

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。
  
## <a name="issue"></a>**問題**  
*差異虛擬硬碟的父系和子系虛擬硬碟在不同的磁碟區上有儲存體服務品質啟用。*  
  
## <a name="impact"></a>**影響**  
*這項設定可能會導致非預期的儲存體服務品質差異虛擬硬碟，以及其他虛擬硬碟的父系和子系的磁碟區上的行為。這會影響下列虛擬硬碟：*  
  
\<虛擬硬碟清單 >  
  
## <a name="resolution"></a>**解決方法**  
*停用參考虛擬硬碟上的儲存體服務品質，或將存放裝置移轉，若要將父代和子系虛擬硬碟移至相同的磁碟區。*  
  


