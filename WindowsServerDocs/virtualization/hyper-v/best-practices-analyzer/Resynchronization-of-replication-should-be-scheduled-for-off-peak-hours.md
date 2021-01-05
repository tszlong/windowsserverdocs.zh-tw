---
title: 應將複寫的重新同步處理排程在離峰時間
description: 瞭解當主要虛擬機器的複寫重新同步處理未排程在離峰時段時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
ms.date: 8/16/2016
ms.openlocfilehash: 3669abf4da79db82d0ba7ca985b886a02a180ea9
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845816"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>應將複寫的重新同步處理排程在離峰時間

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*主要虛擬機器的複寫重新同步處理不會排程在離峰時間。*

## <a name="impact"></a>影響
*虛擬機器處於需要重新同步處理的狀態越久，複寫記錄檔的成長時間越長，主要虛擬機器上的複寫變更就越多。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*您可以使用 Hyper-v 管理員來修改虛擬機器的複寫設定，以便在離峰時段自動執行重新同步處理。*



