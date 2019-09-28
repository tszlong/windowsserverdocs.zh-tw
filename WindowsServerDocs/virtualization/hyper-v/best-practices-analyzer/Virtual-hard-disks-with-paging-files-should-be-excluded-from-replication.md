---
title: 具有分頁檔案的虛擬硬碟應從複寫中排除
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 215e265d69af1b384d3461c627558ff0a59c8e91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364549"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>具有分頁檔案的虛擬硬碟應從複寫中排除

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|內容|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*分頁檔案應排除不參與複寫，但未排除任何磁片。*  
  
## <a name="impact"></a>影響  
@no__t 0Paging 檔案遇到大量的輸入/輸出活動，因此不必要地需要更大的資源來參與複寫。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
@no__t 0If 您尚未這麼做，請為 Windows 分頁檔建立個別的虛擬硬碟。如果已完成初始複寫，請使用 Hyper-v 管理員來移除複寫。然後，再次設定複寫，並將具有分頁檔案的虛擬硬碟從複寫中排除。 *  
  


