---
title: 一或多個網路介面卡應該設定為來源連接埠鏡像
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0843c1f302b96d334e8d6649ac5503bbe2b12d02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831789"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>一或多個網路介面卡應該設定為來源連接埠鏡像

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|  
  
在下列章節中，斜體表示會出現在此問題的最佳做法分析程式工具的 UI 文字。  
  
## <a name="issue"></a>**問題**  
*一或多個虛擬機器已設定為目的地的連接埠鏡像的網路介面卡，但虛擬交換器上沒有對應來源。*  
  
## <a name="impact"></a>**影響**  
*下列虛擬交換器和虛擬機器，連接埠鏡像會無法正確運作：*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*使用 Windows PowerShell] 或 [HYPER-V 管理員來完成，或更正連接埠鏡像組態。*  
  


