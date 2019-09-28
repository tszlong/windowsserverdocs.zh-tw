---
title: 設定以 VSS 為基礎的備份的客體作業系統，為 Hyper-v 複本啟用應用程式一致快照
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 032ca585da1c556fff6f9e06b3bde0662a5d64db
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364956"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>設定以 VSS 為基礎的備份的客體作業系統，為 Hyper-v 複本啟用應用程式一致快照

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|Error|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
*應用程式一致快照集需要在參與複寫之虛擬機器的客體作業系統中啟用和設定「磁片區陰影複製服務」（VSS）。*  
  
## <a name="impact"></a>影響  
@no__t 0Even 如果在複寫設定中指定了應用程式一致快照集，Hyper-v 將不會使用它們，除非已設定 VSS。這會影響下列虛擬機器： *  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>解析度  
*使用虛擬機器連線，在虛擬機器中安裝整合服務。*  
  


