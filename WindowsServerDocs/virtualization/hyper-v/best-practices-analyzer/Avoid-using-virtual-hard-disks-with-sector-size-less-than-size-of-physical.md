---
title: 避免使用磁區大小小於儲存虛擬硬碟檔案之實體儲存體磁區大小的虛擬硬碟
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 944fdce68a2f0b8e9c122f5f9134f0e07de18bbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366414"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>避免使用磁區大小小於儲存虛擬硬碟檔案之實體儲存體磁區大小的虛擬硬碟

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**工作** <br />**筆記本電腦**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多個虛擬硬碟的實體磁區大小，小於虛擬硬碟檔案所在存放裝置的實體磁區大小。*  
  
## <a name="impact"></a>**產生**  
在使用虛擬硬碟的虛擬機器或應用程式上，可能會發生 @no__t 0Performance 問題。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>**解決方法**  
*執行下列其中一項動作：*  
  
-   *執行存放裝置遷移，將虛擬硬碟移至新的實體系統*  
  
-   *使用 Windows PowerShell 或 WMI 來啟用 VHDX 格式的虛擬硬碟，以報告特定磁區大小*  
  
-   *使用登錄設定來啟用 VHD 格式的虛擬硬碟，以報告4k 的實體磁區大小*  
  


