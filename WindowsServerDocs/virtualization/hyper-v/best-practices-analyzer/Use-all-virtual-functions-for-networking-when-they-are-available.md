---
title: 使用所有虛擬函式以提供網路功能
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
ms.date: 8/16/2016
ms.openlocfilehash: 4a1502180350c338793bf811cddc675785e9c67a
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746803"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>使用所有虛擬函式以提供網路功能

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*某些硬體加速功能未使用*

## <a name="impact"></a>影響
*這種設定可能會導致整體 CPU 使用率高於所需。下列虛擬機器的網路效能可能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*如果實體硬體支援 SR-IOV，且此設定與虛擬機器所需的網路功能不衝突，請考慮設定 SR-IOV 的虛擬網路介面卡。*



