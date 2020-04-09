---
title: 避免在生產環境中執行伺服器工作負載的虛擬機器上使用檢查點
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1be75890-d316-495a-b9b7-be75fc1aac10
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: f0e9d40fa6e28b515621402b853012cb59086a07
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857711"
---
# <a name="avoid-using-checkpoints-on-a-virtual-machine-that-runs-a-server-workload-in-a-production-environment"></a>避免在生產環境中執行伺服器工作負載的虛擬機器上使用檢查點

>適用於︰Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

> [!NOTE]  
> 在 Windows Server 2012 R2 中，已將虛擬機器快照集重新命名為 Hyper-v 管理員中的虛擬機器檢查點，以符合 System Center 虛擬機器管理中所使用的術語。 如需詳細資訊，請參閱[檢查點和快照](https://technet.microsoft.com/library/dn818483.aspx)集。  
  
## <a name="issue"></a>問題  
  
*找到具有一或多個檢查點的虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*儲存檢查點檔案的實體磁片可能會耗盡可用空間。如果發生這種情況，就無法在實體儲存體上執行額外的磁片作業。依賴實體存放裝置的任何虛擬機器都可能受到影響。*  
  
當實體磁碟空間不足時，任何已儲存在該磁片上的檢查點或虛擬硬碟的執行中虛擬機器，都可能會自動暫停。 [Hyper-v 管理員] 會將這些虛擬機器的狀態顯示為 [已暫停-重大]。  
  
## <a name="resolution"></a>解析度  
  
*如果虛擬機器在生產環境中執行伺服器工作負載，請讓虛擬機器離線，然後使用 Hyper-v 管理員來套用或刪除檢查點。若要刪除檢查點，您必須關閉虛擬機器以完成此程式。*  
  
> [!NOTE]  
> 生產檢查點現在可作為標準檢查點的替代方案。 如需詳細資訊，請參閱[在標準或生產檢查點之間選擇](../manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md)。  
  


