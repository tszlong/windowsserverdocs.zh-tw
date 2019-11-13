---
title: 當父系和子系虛擬硬碟位於不同磁片區時，請避免在使用差異虛擬硬碟時啟用存放裝置服務品質
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366444"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>當父系和子系虛擬硬碟位於不同磁片區時，請避免在使用差異虛擬硬碟時啟用存放裝置服務品質

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|設定|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>**問題**  
*在不同磁片區上具有父系和子系虛擬硬碟的差異虛擬硬碟，已啟用存放裝置服務品質。*  
  
## <a name="impact"></a>**產生**  
*此設定可能會導致差異虛擬硬碟的非預期儲存服務行為，以及父和子磁片區上的其他虛擬硬碟。這會影響下列虛擬硬碟：*  
  
\<虛擬硬碟 > 清單  
  
## <a name="resolution"></a>**解決方法**  
*停用參照虛擬硬碟上的存放裝置服務品質，或執行存放裝置遷移，將父系和子虛擬硬碟移至相同的磁片區。*  
  


