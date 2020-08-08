---
title: 避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 02a38c88d131e4f3aef06174abc4c62526adbeeb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948571"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免動態虛擬硬碟或差異磁片上的虛擬區塊和實體磁片磁區之間的對齊不一致

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
*偵測到一或多個虛擬硬碟的對齊不一致。*

### <a name="impact"></a>影響
*如果虛擬硬碟儲存在磁區大小為4K 的實體磁片上，則使用虛擬硬碟的虛擬機器或應用程式可能會遇到效能問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*使用 [建立虛擬硬碟] 嚮導來建立新的 VHD 格式或 VHDX 格式的虛擬硬碟，並將現有的虛擬硬碟指定為來源磁片。將會建立新的虛擬硬碟，並在虛擬區塊和實體磁片之間進行對齊。*



