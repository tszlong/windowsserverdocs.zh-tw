---
title: 當虛擬機器使用差異虛擬硬碟時，請確定有足夠的可用實體磁碟空間
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
ms.date: 8/16/2016
ms.openlocfilehash: d5a8c5d38aa47845a88077eaa6b785dc5ebb148c
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746273"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>當虛擬機器使用差異虛擬硬碟時，請確定有足夠的可用實體磁碟空間

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
*一或多部虛擬機器正在使用差異虛擬硬碟。*

## <a name="impact"></a>影響
*差異虛擬硬碟需要裝載磁片區上的可用空間，以便在寫入虛擬硬碟時可以配置空間。如果可用空間不足，依賴實體儲存體的虛擬機器可能會受到影響。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*監視可用的磁碟空間，以確保有足夠的空間可用於虛擬硬碟擴充。考慮將差異虛擬硬碟合併到其父系。在 [Hyper-v 管理員] 中，檢查差異磁片以判斷父虛擬硬碟。如果您將差異磁片合併到其他差異磁片所共用的父磁片，該動作將會損毀其他差異磁片與父磁片之間的關聯性，使其無法使用。確認父虛擬硬碟尚未共用之後，您可以使用 [編輯磁片] 嚮導，將差異磁片合併到父虛擬硬碟。*



