---
title: 使用所有虛擬函式來取得可用的網路功能
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6be1d32b22d5619ff40d0121bcc60a6bfa3c225f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960301"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>使用所有虛擬函式來取得可用的網路功能

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*未使用某些硬體加速功能*

## <a name="impact"></a>影響
*此設定可能會導致整體 CPU 使用率高於所需。下列虛擬機器的網路效能可能不佳：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*如果實體硬體支援 SR-IOV，而且此設定不會與虛擬機器所需的網路功能發生衝突，請考慮設定 SR-IOV 的虛擬網路介面卡。*



