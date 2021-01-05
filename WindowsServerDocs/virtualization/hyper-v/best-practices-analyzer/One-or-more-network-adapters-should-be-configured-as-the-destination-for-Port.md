---
title: 應將一或多個網路介面卡設定為埠鏡像的目的地
description: 瞭解當一或多部虛擬機器的網路介面卡設定為埠鏡像的來源時，該怎麼辦，但虛擬交換器上沒有對應的目的地。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: b83c166d-f010-47c4-a4bb-02167f2e3361
ms.date: 8/16/2016
ms.openlocfilehash: f69bd6da7d2451deaee97c3f3d23b5c861223388
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846363"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-destination-for-port-mirroring"></a>應將一或多個網路介面卡設定為埠鏡像的目的地

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
*一或多部虛擬機器的網路介面卡設定為埠鏡像的來源，但虛擬交換器上沒有對應的目的地。*

## <a name="impact"></a>**影響**
*下列虛擬交換器和虛擬機器的埠鏡像將無法正確運作：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*使用 Windows PowerShell 或 Hyper-v 管理員來完成或更正埠鏡像設定。*



