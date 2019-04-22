---
title: PVLAN 設定在虛擬交換器上的必須一致
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813249"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>PVLAN 設定在虛擬交換器上的必須一致

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016| 
|**產品/功能**|Hyper-V|  
|**Severity**|錯誤|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。
  
## <a name="issue"></a>**問題**  
*一或多個虛擬網路介面卡上未正確設定私人虛擬區域網路 (PVLAN)。*  
  
## <a name="impact"></a>**影響**  
*PVLAN 不可能正確地隔離虛擬機器之間的網路流量。*  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell cmdlet，Set-vmnetworkadaptervlan，正確設定 PVLAN。*  
  


