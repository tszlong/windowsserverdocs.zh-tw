---
title: 避免設定虛擬機器，以允許未篩選的 SCSI 命令
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888269"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>避免設定虛擬機器，以允許未篩選的 SCSI 命令

>適用於：Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|操作|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器會設定為允許未篩選的 SCSI 命令。*  
  
## <a name="impact"></a>影響  
  
*略過 SCSI 命令篩選會帶來安全性風險。只有當需要在客體作業系統中執行的儲存體應用程式相容性，應該啟用這項設定。下列虛擬機器設定為允許未篩選的 SCSI 命令：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*請連絡您的存放裝置廠商，以判斷是否需要此設定。此外，如果管理作業系統或其他的客體作業系統元件遭到入侵，或出現異常的行為，重新設定虛擬機器，以封鎖命令。*  
  


