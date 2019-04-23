---
title: 設定支援的客體作業系統時，才的 SCSI 控制器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3dc48602ab6c71c60fdb734ca98cf1359f58d87c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830389"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>設定支援的客體作業系統時，才的 SCSI 控制器

>適用於：Windows Server 2016


  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器已無法使用，因為客體作業系統不支援 SCSI 控制器的 SCSI 控制器。*  
  
## <a name="impact"></a>影響  
  
*虛擬機器無法使用儲存體連結到 SCSI 控制器。這會影響下列虛擬機器：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>解析度  
  
*關閉虛擬機器，並使用 HYPER-V 管理員從虛擬機器移除 SCSI 控制器。然後，重新啟動虛擬機器。*  
  


