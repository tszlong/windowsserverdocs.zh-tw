---
title: 避免虛擬區塊與動態虛擬硬碟或差異磁片上的實體磁片磁區之間的對齊不一致
description: 瞭解當偵測到一或多個虛擬硬碟的對齊不一致時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
ms.date: 8/16/2016
ms.openlocfilehash: a35906161a51e2884a15944b51416a2404cce38a
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97834723"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>避免虛擬區塊與動態虛擬硬碟或差異磁片上的實體磁片磁區之間的對齊不一致

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*偵測到一或多個虛擬硬碟有對齊不一致的情況。*

### <a name="impact"></a>影響
*如果虛擬硬碟儲存在磁區大小為4K 的實體磁片上，則使用虛擬硬碟的虛擬機器或應用程式可能會遇到效能問題。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*使用 [建立虛擬硬碟] 建立新的 VHD 格式或 VHDX 格式的虛擬硬碟，並將現有的虛擬硬碟指定為來源磁片。將會建立新的虛擬硬碟，並在虛擬區塊與實體磁片之間保持一致。*



