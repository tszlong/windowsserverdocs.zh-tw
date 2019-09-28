---
title: 為虛擬機器提供所有可用的整合服務
description: 提供解決此最佳做法分析程式規則所回報之問題的指示。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5ec2dc73cea8b8356d832bf9fdb960985df2df6c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393598"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>為虛擬機器提供所有可用的整合服務

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*虛擬機器上未啟用一或多個可用的整合服務。*  
  
## <a name="impact"></a>影響  
  
*下列虛擬機器將無法使用某些功能：*  
  
@no__t 0list 的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*If 這是刻意的，不需要採取進一步的動作。否則，請考慮在這些虛擬機器的設定中提供所有整合服務。*  
  
某些整合服務的可用性可透過虛擬機器設定來管理。   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>管理整合服務對虛擬機器的可用性  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格的 [**虛擬機器**] 底下，選取您要設定的虛擬機器。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  在 [**管理**] 下，按一下 [ **Integration Services**]。  
  
5.  在整合服務清單中，選取您要提供給虛擬機器的每個服務的核取方塊。 如果核取方塊無法使用，則虛擬機器中執行的客體作業系統不支援該特定的整合服務。  
  


