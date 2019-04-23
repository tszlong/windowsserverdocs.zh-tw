---
title: 顯示配接器應該在虛擬機器中啟用提供視訊功能
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870689"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>顯示配接器應該在虛擬機器中啟用提供視訊功能

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
  
*Microsoft 虛擬機器匯流排視訊裝置可能會停用虛擬機器中。*  
  
Microsoft 虛擬機器匯流排視訊裝置是適用於 HYPER-V 虛擬機器使用虛擬視訊卡。 當虛擬機器未設定為使用 Microsoft 虛擬機器匯流排視訊裝置時，會使用舊版的視訊卡。 Microsoft 虛擬機器匯流排視訊裝置的效能優於舊版的視訊卡。  
  
## <a name="impact"></a>影響  
  
*下列虛擬機器的視訊效能會降低：*  
  
\<清單中的虛擬機器名稱 >  
  
## <a name="resolution"></a>解析度  
  
*若要啟用 Microsoft 虛擬機器匯流排視訊裝置，在客體作業系統中使用裝置管理員。*  
  
使用 裝置管理員所需的步驟會因作業系統而有所不同。 如需指示，請參閱在客體作業系統中的說明。  
  


