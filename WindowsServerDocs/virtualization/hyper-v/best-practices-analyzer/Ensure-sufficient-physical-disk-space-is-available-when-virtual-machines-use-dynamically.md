---
title: 當虛擬機器使用動態擴充的虛擬硬碟時，請確定有足夠的可用實體磁碟空間
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
ms.date: 8/16/2016
ms.openlocfilehash: 1e67b51aa3c41c9642a8972d25e0501b26e91cd5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746863"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>當虛擬機器使用動態擴充的虛擬硬碟時，請確定有足夠的可用實體磁碟空間

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
*一或多部虛擬機器正在使用動態擴充的虛擬硬碟。*

## <a name="impact"></a>影響
*動態擴充的虛擬硬碟需要主機磁片區上的可用空間，以便在寫入虛擬硬碟時可以配置空間。如果可用空間不足，依賴實體儲存體的虛擬機器可能會受到影響。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*監視可用的磁碟空間，以確保有足夠的空間可進行擴充。請考慮關閉虛擬機器，並使用 [Hyper-v 管理員] 中的 [編輯磁片] 嚮導，將此虛擬機器的每個動態擴充虛擬硬碟轉換為固定大小的虛擬硬碟。*



