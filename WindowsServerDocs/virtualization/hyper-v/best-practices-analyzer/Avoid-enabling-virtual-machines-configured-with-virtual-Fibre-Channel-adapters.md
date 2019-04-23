---
title: 避免在使用時有較少的路徑來光纖通道邏輯單元 (Lun)，來源與目的地上允許即時移轉的虛擬光纖通道介面卡設定的虛擬機器
description: 此最佳做法分析程式規則之文字的線上版本。
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6ff69d5cb09133a806c2a2df3446713264a4e892
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849549"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>避免在使用時有較少的路徑來光纖通道邏輯單元 (Lun)，來源與目的地上允許即時移轉的虛擬光纖通道介面卡設定的虛擬機器

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
*一或多個虛擬機器有 AllowReducedFcRedunancy 屬性中的虛擬化 WMI 提供者設定。*  
  
## <a name="impact"></a>**影響**  
*下列虛擬機器的即時移轉可能會導致資料遺失，或到儲存體中斷 I/O:*  
  
\<虛擬機器清單 >  
  
## <a name="resolution"></a>**解決方法**  
*請考慮清除受影響的虛擬機器上的 AllowReducedFcRedundancy WMI 屬性。清除這個屬性時，您可以執行即時移轉到目的地上的光纖通道的路徑數目是相同或多個來源的路徑數目時，才設定虛擬光纖通道介面卡的虛擬機器上。這些檢查有助於避免資料遺失或 I/O 中斷儲存體。* 