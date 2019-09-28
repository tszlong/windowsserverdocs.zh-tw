---
title: 只有在客體作業系統支援時，才設定 SCSI 控制器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da8d929a8f06f58610913d28d2f1e90299efb235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366425"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>只有在客體作業系統支援時，才設定 SCSI 控制器

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器設定了無法使用的 SCSI 控制器，因為客體作業系統不支援 SCSI 控制器。*  
  
## <a name="impact"></a>影響  
  
@no__t 0Virtual 的機器無法使用連接到 SCSI 控制器的存放裝置。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
  
@no__t-向下0Shut 虛擬機器，並使用 Hyper-v 管理員從虛擬機器移除 SCSI 控制器。然後重新開機虛擬機器。 *  
  


