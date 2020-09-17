---
title: 當父系和子系虛擬硬碟位於不同的磁片區時，請避免在使用差異虛擬硬碟時啟用存放裝置服務品質
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
ms.date: 8/16/2016
ms.openlocfilehash: 792ee7f84694171b7b44602c8bb1a5c307d80c6c
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747073"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>當父系和子系虛擬硬碟位於不同的磁片區時，請避免在使用差異虛擬硬碟時啟用存放裝置服務品質

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
*在不同磁片區上具有父系和子虛擬硬碟的差異虛擬硬碟已啟用存放裝置服務品質。*

## <a name="impact"></a>**影響**
*這項設定可能會導致差異虛擬硬碟的非預期儲存體服務行為，以及父磁片區和子磁片區上的其他虛擬硬碟。這會影響下列虛擬硬碟：*

\<list of virtual hard disks>

## <a name="resolution"></a>**解決方法**
*在參考的虛擬硬碟上停用存放裝置服務品質，或執行儲存體遷移，將父系和子系虛擬硬碟移至相同的磁片區。*



