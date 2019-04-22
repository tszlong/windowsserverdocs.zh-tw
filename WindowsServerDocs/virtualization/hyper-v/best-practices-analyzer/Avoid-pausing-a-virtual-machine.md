---
title: 避免暫停虛擬機器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4492ac385a289d075ebcd48b1c7c1c78c1af2f8c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814349"
---
# <a name="avoid-pausing-a-virtual-machine"></a>避免暫停虛擬機器

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  

在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。

## <a name="issue"></a>問題  
  
*此伺服器擁有一或多個虛擬機器處於暫停狀態。*  
  
## <a name="impact"></a>影響  
  
*根據可用的記憶體數量，您可能無法執行其他的虛擬機器。*  
  
已暫停的虛擬機器不會釋放其配置的記憶體，這表示記憶體無法用來啟動其他虛擬機器。  
  
## <a name="resolution"></a>解析度  
  
*如果這是刻意設計的任何進一步的動作不是必要的。否則，請考慮繼續這些虛擬機器，或加以關閉。*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>使用 HYPER-V 管理員，繼續虛擬機器  
  
1.  開啟 \[Hyper-V 管理員\]。 (從**工具**功能表的 伺服器管理員 中，按一下  **HYPER-V 管理員**。)  
  
2.  從**虛擬機器**清單中，尋找具有狀態的虛擬機器**已暫停**。  
  
    > [!IMPORTANT]  
    > 狀態**重大已暫停**幾乎該虛擬機器的實體儲存體上剩餘的可用空間時，就會發生。 您嘗試將繼續在此狀態的虛擬機器之前，釋放實體儲存體上的可用空間。  
  
3.  以滑鼠右鍵按一下每個虛擬機器名稱，然後按一下  **Resume**。 這會將虛擬機器返回執行中狀態。 在那之後，如果您想要關閉虛擬機器，再按一下滑鼠右鍵，然後選擇**關閉**。  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>若要恢復虛擬機器使用 Windows PowerShell  
  
您可以在單一命令中使用篩選之後您, 管線取得所有虛擬機器主機上。 輸入：  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


