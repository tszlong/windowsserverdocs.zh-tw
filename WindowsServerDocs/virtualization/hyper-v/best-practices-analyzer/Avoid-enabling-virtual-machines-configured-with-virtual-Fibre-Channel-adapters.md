---
title: 避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以便在目的地上光纖通道邏輯單元 (Lun) 時，允許即時移轉，而不是從來源
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2b85793cd5a680b0fd13fca3da5881b8622710fb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963674"
---
# <a name="avoid-enabling-virtual-machines-configured-with-virtual-fibre-channel-adapters-to-allow-live-migrations-when-there-are-fewer-paths-to-fibre-channel-logical-units-luns-on-the-destination-than-on-the-source"></a>避免啟用使用虛擬光纖通道介面卡設定的虛擬機器，以便在目的地上光纖通道邏輯單元 (Lun) 時，允許即時移轉，而不是從來源

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>**問題**
*一或多部虛擬機器在虛擬化 WMI 提供者中設定了 AllowReducedFcRedunancy 屬性。*

## <a name="impact"></a>**影響**
*下列虛擬機器的即時移轉可能會導致資料遺失或中斷 i/o 給儲存體：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方案**
*請考慮清除受影響虛擬機器上的 AllowReducedFcRedundancy WMI 屬性。清除此屬性時，只有當目的地上光纖通道的路徑數目等於或大於來源上的路徑數目時，您才可以在使用虛擬光纖通道介面卡設定的虛擬機器上執行即時移轉。這些檢查有助於防止遺失資料或中斷儲存體的 i/o。*