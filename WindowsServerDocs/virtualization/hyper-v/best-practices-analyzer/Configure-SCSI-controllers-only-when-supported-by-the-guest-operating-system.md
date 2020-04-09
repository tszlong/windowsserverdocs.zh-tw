---
title: 只有在客體作業系統支援時，才設定 SCSI 控制器
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: cf206d9568ef7634d724f3fce450985c34ebfac5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862161"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>只有在客體作業系統支援時，才設定 SCSI 控制器

>適用於︰Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器設定了無法使用的 SCSI 控制器，因為客體作業系統不支援 SCSI 控制器。*  
  
## <a name="impact"></a>影響  
  
*虛擬機器無法使用連接到 SCSI 控制器的儲存體。這會影響下列虛擬機器：*  
  
\<的虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
  
*關閉虛擬機器，並使用 Hyper-v 管理員從虛擬機器移除 SCSI 控制器。然後，重新開機虛擬機器。*  
  


