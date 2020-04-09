---
title: 應該將複寫的重新同步處理排程為離峰時段
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae630f93ef50ebc977de2bffcefa3b27b03622ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861791"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>應該將複寫的重新同步處理排程為離峰時段

>適用於︰Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*主要虛擬機器的複寫重新同步處理未排定在離峰時間執行。*  
  
## <a name="impact"></a>影響  
*虛擬機器處於需要重新同步處理的狀態愈久，複寫記錄檔的成長越長，主要虛擬機器上發生的變更也越多。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
*使用 [Hyper-v 管理員] 修改虛擬機器的複寫設定，以在離峰時段自動執行重新同步處理。*  
  


