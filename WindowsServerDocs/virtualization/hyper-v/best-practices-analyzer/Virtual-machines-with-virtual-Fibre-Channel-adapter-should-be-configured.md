---
title: 以虛擬光纖通道介面卡設定的虛擬機器應該設定為與光纖通道型存放裝置的高可用性
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 203477a022f7c5f819ef7b99f1b8e37a0b8b0a9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816939"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>以虛擬光纖通道介面卡設定的虛擬機器應該設定為與光纖通道型存放裝置的高可用性

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|資訊|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。
  
## <a name="issue"></a>**問題**  
*由於這些虛擬機器設定虛擬光纖通道介面卡只有一個主機匯流排介面卡 (HBA) 連接一或多個虛擬機器就會缺少光纖通道型儲存體的高可用性連線。*  
  
## <a name="impact"></a>**影響**  
*主機匯流排介面卡的失敗可能會封鎖儲存體和虛擬機器之間的光纖通道連線。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*從虛擬機器中將主機匯流排介面卡的另一個連接，並設定在客體作業系統建立備援光纖通道連線的多重路徑 I/O (MPIO)。*  
  


