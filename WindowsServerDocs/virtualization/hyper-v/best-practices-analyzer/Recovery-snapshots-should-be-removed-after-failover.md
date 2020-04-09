---
title: 容錯移轉後應移除復原快照集
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: c995293ca67b4cad0837affa854fb4ac366856e1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861841"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>容錯移轉後應移除復原快照集

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016| 
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*已故障的虛擬機器有一或多個復原快照集。*  
  
## <a name="impact"></a>**產生**  
*儲存快照集檔案的實體磁片上可能會耗盡可用空間。如果發生這種情況，就無法在實體儲存體上執行額外的磁片作業。依賴實體存放裝置的任何虛擬機器都可能受到影響。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*針對每個已容錯移轉的虛擬機器，在 Windows PowerShell 中使用 Start-vmfailover 指令程式來移除復原快照，並指出容錯移轉完成。*  
  


