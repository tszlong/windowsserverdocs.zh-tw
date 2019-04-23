---
title: 提供給虛擬機器的所有可用的 integration services
description: 提供指示來解決此 Best Practices Analyzer 規則所回報的問題。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 2c4b2043-ad81-495e-aa7a-467f813bb3d2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c2b5137594f78980f87f6520ae4b4af8203aef32
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883779"
---
# <a name="offer-all-available-integration-services-to-virtual-machines"></a>提供給虛擬機器的所有可用的 integration services

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱 [最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>問題  
  
*一或多個可用的整合服務不會啟用虛擬機器上。*  
  
## <a name="impact"></a>影響  
  
*某些功能將無法使用下列虛擬機器：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*如果這是刻意設計的任何進一步的動作不是必要的。否則，請考慮在這些虛擬機器設定中的所有整合服務供應項都目。*  
  
某些整合服務的可用性可透過虛擬機器設定進行管理。   
  
#### <a name="to-manage-the-availability-of-integration-services-to-a-virtual-machine"></a>若要管理的虛擬機器的整合服務的可用性  
  
1.  開啟 \[Hyper-V 管理員\]。 按一下 [開始]，指向 [系統管理工具]，然後按一下 [Hyper-V 管理員]。  
  
2.  在 [結果] 窗格中，在**虛擬機器**，選取您想要設定的虛擬機器。  
  
3.  在 [執行] 窗格的虛擬機器名稱之下，按一下 [設定]。  
  
4.  底下**管理**，按一下**Integration Services**。  
  
5.  在 integration services 的清單中，選取您想要提供給虛擬機器每個服務的核取方塊。 如果無法使用，會在虛擬機器中執行的作業系統不支援特定的整合服務 核取方塊。  
  


