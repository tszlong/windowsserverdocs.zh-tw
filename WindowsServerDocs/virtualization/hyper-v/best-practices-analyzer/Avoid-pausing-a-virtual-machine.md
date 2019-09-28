---
title: 避免暫停虛擬機器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 930f927c-e414-4a36-9786-028941e886e4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 406b24edd4a7e87e32058006590ac7cd37206568
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366447"
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

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題  
  
*這部伺服器有一或多部虛擬機器處於暫停狀態。*  
  
## <a name="impact"></a>影響  
  
*視可用的記憶體數量而定，您可能無法執行額外的虛擬機器。*  
  
暫停的虛擬機器不會釋放其配置的記憶體，這表示記憶體無法供啟動其他虛擬機器使用。  
  
## <a name="resolution"></a>解析度  
  
*If 這是刻意的，不需要採取進一步的動作。否則，請考慮繼續這些虛擬機器，或將其關閉。*  
  
#### <a name="use-hyper-v-manager-to-resume-the-virtual-machine"></a>使用 Hyper-v 管理員繼續虛擬機器  
  
1.  開啟 \[Hyper-V 管理員\]。 （從伺服器管理員的 [**工具**] 功能表中，按一下 [ **hyper-v 管理員**]）。  
  
2.  從 [**虛擬機器**] 清單中，尋找狀態為 [已**暫停**] 的虛擬機器。  
  
    > [!IMPORTANT]  
    > 當該虛擬機器的實體存放裝置上有極少的可用空間時，就會發生「已**暫停-重大**」狀態。 在您嘗試繼續處於此狀態的虛擬機器之前，請先釋放實體儲存體上的可用空間。  
  
3.  以滑鼠右鍵按一下每個虛擬機器名稱，然後按一下 [**繼續**]。 這會將虛擬機器返回執行中狀態。 之後，如果您想要關閉虛擬機器，請以滑鼠右鍵按一下它，然後選擇 [**關機**]。  
  
#### <a name="use-windows-powershell-to-resume-the-virtual-machine"></a>使用 Windows PowerShell 繼續虛擬機器  
  
您可以在取得主機上的所有虛擬機器之後，使用篩選和管線在一個命令中執行此動作。 輸入：  
  
```  
get-vm | where state -eq 'paused' | resume-vm  
```  
  


