---
title: 避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以允許在目的地上光纖通道邏輯單元（Lun）的路徑少於來源時進行即時移轉
description: 此最佳做法分析程式規則的線上版本文字。
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c55a8c76391ae1b01f43492dc5c72e3760371b80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365283"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以允許在目的地上光纖通道邏輯單元（Lun）的路徑少於來源時進行即時移轉

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。  
  
|屬性|詳細資料|  
|-|-|  
|**作業系統**|Windows Server 2016|  
|**產品/功能**|Hyper-V|  
|**Severity**|警告|  
|**分類**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。
  
## <a name="issue"></a>**問題**  
*一或多部虛擬機器在虛擬化 WMI 提供者中設定了 AllowReducedFcRedunancy 屬性。*  
  
## <a name="impact"></a>**產生**  
*下列虛擬機器的即時移轉可能會導致資料遺失或中斷 i/o 給儲存體：*  
  
@no__t 0list 的虛擬機器 >  
  
## <a name="resolution"></a>**解決方法**  
@no__t 0Consider 清除受影響虛擬機器上的 AllowReducedFcRedundancy WMI 屬性。清除此屬性時，只有當目的地上光纖通道的路徑數目等於或大於來源上的路徑數目時，您才可以在使用虛擬光纖通道介面卡設定的虛擬機器上執行即時移轉。這些檢查有助於防止遺失資料或中斷對儲存體的 i/o。 * 