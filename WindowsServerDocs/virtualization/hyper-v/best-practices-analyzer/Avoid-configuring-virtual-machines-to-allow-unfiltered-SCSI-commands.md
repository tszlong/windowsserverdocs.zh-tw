---
title: 避免將虛擬機器設定為允許未篩選的 SCSI 命令
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5deb20862ed0e359febd4a9b58202d53c85058ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365267"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>避免將虛擬機器設定為允許未篩選的 SCSI 命令

>適用於︰Windows Server 2016


  
*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**低於**|警告|  
|**類別**|操作|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器已設定為允許未篩選的 SCSI 命令。*  
  
## <a name="impact"></a>影響  
  
*略過 SCSI 命令篩選會造成安全性風險。只有在與客體作業系統中執行的存放裝置應用程式相容時，才需要啟用此設定。下列虛擬機器已設定為允許未篩選的 SCSI 命令：*  
  
\<虛擬機器名稱清單 >  
  
## <a name="resolution"></a>解析度  
  
*請洽詢您的存放裝置廠商，以判斷是否需要此設定。此外，如果管理作業系統或其他客體作業系統遭到入侵，或呈現不尋常的行為，請重新設定虛擬機器以封鎖命令。*  
  


