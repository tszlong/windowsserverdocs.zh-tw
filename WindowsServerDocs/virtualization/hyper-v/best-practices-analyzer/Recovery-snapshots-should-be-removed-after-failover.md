---
title: 容錯移轉之後應移除復原快照集
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
ms.date: 8/16/2016
ms.openlocfilehash: b30dbf9996f2406e3d260c825dbe2dbbc6918324
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745543"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>容錯移轉之後應移除復原快照集

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|警告|
|**類別**|作業|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>**問題**
*已容錯移轉的虛擬機器有一或多個修復快照集。*

## <a name="impact"></a>**影響**
*可用空間可能會在儲存快照集檔案的實體磁片上執行。如果發生這種情況，就不能在實體儲存體上執行任何額外的磁片作業。依賴實體儲存體的虛擬機器可能會受到影響。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>**解決方法**
*針對每個已容錯移轉的虛擬機器，請使用 Windows PowerShell 中的 Start-vmfailover 指令程式來移除復原快照，並指出容錯移轉完成。*



