---
title: 避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以允許在目的地上光纖通道邏輯單元 (Lun) 的路徑較少時進行即時移轉，而非來源
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.date: 8/16/2016
ms.openlocfilehash: 71617bbf6718e77f004b57e38035f5277c45c3bc
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747063"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以允許在目的地上光纖通道邏輯單元 (Lun) 的路徑較少時進行即時移轉，而非來源

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*一或多部虛擬機器的虛擬化 WMI 提供者中已設定 AllowReducedFcRedunancy 屬性。*

## <a name="impact"></a>**影響**
*下列虛擬機器的即時移轉可能會導致資料遺失或中斷 i/o 至儲存體：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*請考慮在受影響的虛擬機器上清除 AllowReducedFcRedundancy WMI 屬性。若清除此屬性，只有當目的地上光纖通道的路徑數目等於或大於來源上的路徑數目時，您才能在以 virtual 光纖通道 adapter 設定的虛擬機器上執行即時移轉。這些檢查有助於防止資料遺失或中斷儲存體的 i/o。*