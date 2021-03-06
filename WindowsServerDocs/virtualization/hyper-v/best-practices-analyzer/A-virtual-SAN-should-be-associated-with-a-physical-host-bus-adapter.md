---
title: 虛擬 SAN 應該與實體主機匯流排介面卡相關聯
description: 瞭解當虛擬存放區域網路 (SAN) 已設定，而沒有與主機匯流排介面卡 (HBA) 的關聯時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
ms.date: 8/16/2016
ms.openlocfilehash: 73ce82a03e4831ab5056cb6e75c2bf08cc52b494
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834203"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>虛擬 SAN 應該與實體主機匯流排介面卡相關聯

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|


在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*已設定虛擬存放區域網路 (SAN) ，而沒有與主機匯流排介面卡 (HBA) 的關聯。*

## <a name="impact"></a>**影響**
*當虛擬機器設定的虛擬光纖通道介面卡連線至設定錯誤的虛擬 SAN 時，虛擬機器將無法啟動。這會影響下列虛擬 San：*


\<list of virtual SANs>


## <a name="resolution"></a>**解決方法**
*將虛擬 SAN 連接至主機匯流排介面卡，以進行重新設定。*





