---
title: 請避免使用虛擬硬碟磁區大小會儲存虛擬硬碟檔案的實體儲存體的磁區大小大於或等於
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b7cf994e-bf4b-4b1b-bad6-ecf7d46d105e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b6ec2e0995180ecf9ae5986447fdd460a1c7a337
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846259"
---
# <a name="avoid-using-virtual-hard-disks-with-a-sector-size-less-than-the-sector-size-of-the-physical-storage-that-stores-the-virtual-hard-disk-file"></a>請避免使用虛擬硬碟磁區大小會儲存虛擬硬碟檔案的實體儲存體的磁區大小大於或等於

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統** <br />**System**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一個或多個虛擬硬碟必須小於虛擬硬碟檔案所在之儲存體的實體磁區大小的實體磁區大小。*  
  
## <a name="impact"></a>**影響**  
*虛擬機器或使用虛擬硬碟的應用程式上可能會發生效能問題。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*執行下列其中一項：*  
  
-   *若要將虛擬硬碟移至新的實體系統將存放裝置移轉*  
  
-   *使用 Windows PowerShell 或 WMI 來啟用 VHDX 格式虛擬硬碟來報告特定的磁區大小*  
  
-   *使用登錄設定，以啟用報告的實體磁區大小為 4k VHD 格式虛擬硬碟*  
  


