---
title: 使用 SCSI 控制站要能夠使用經常性存取隨插即用和熱移除儲存體設定的虛擬機器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 511e1172-aeef-463d-b5dd-2bffae411ff1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 755e7485e54ee58e0acd7ebd75a7ee591aa655f9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843279"
---
# <a name="configure-a-virtual-machine-with-a-scsi-controller-to-be-able-to-hot-plug-and-hot-unplug-storage"></a>使用 SCSI 控制站要能夠使用經常性存取隨插即用和熱移除儲存體設定的虛擬機器

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*找不到未使用 SCSI 控制站的虛擬機器。*  
  
## <a name="impact"></a>影響  
  
*您無法為經常性存取隨插即用或熱拔下的下列虛擬機器的儲存體：*  
  
\<清單中的虛擬機器名稱 >  
  
經常性存取 「 隨插即用或熱拔下儲存體的能力可讓您更輕鬆地管理虛擬機器的儲存體需求，而不需要停機時間。 沒有 SCSI 控制器的虛擬機器必須關閉之前，您可以新增或移除儲存體。  
  
## <a name="resolution"></a>解析度  
  
*如果您不需要為經常性存取隨插即用或熱移除此虛擬機器的儲存體，不不需要任何動作。否則，關閉虛擬機器，並新增至設定的 SCSI 控制器。*  
  
若要使用隨插即用 SCSI 控制器熱拔下儲存體，客體作業系統必須執行的目前版本的 integration services。  
  


