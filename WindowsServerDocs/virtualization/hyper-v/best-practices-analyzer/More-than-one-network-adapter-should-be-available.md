---
title: 應可使用多張網路介面卡
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
ms.date: 8/16/2016
ms.openlocfilehash: 05dc0583424ed155c4780f9f0b4c016be850a71c
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746263"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>應可使用多張網路介面卡

>適用於：Windows Server 2016

如需最佳做法和掃描的詳細資訊，請參閱[最佳做法分析程式](https://go.microsoft.com/fwlink/?LinkId=122786)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*此伺服器設定了一個網路介面卡，必須由管理作業系統和所有需要存取實體網路的虛擬機器所共用。*

## <a name="impact"></a>影響

*管理作業系統中的網路效能可能會降低。*

## <a name="resolution"></a>解決方案

*將其他網路介面卡新增至此電腦。若要保留一張網路介面卡供管理作業系統獨佔使用，請勿將其設定為與外部虛擬網路搭配使用。*

如需將網路介面卡新增到電腦的相關資訊，請參閱電腦或網路介面卡的說明文件。 然後，若要將它專門保留給管理作業系統，請不要將它連接至虛擬交換器。



