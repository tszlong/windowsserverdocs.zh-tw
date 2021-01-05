---
title: 具有分頁檔案的虛擬硬碟應從複寫中排除
description: 瞭解當分頁檔案體驗大量的輸入/輸出活動時該怎麼辦，這將會不必要地需要更多資源來參與複寫。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
ms.date: 8/16/2016
ms.openlocfilehash: c710891396ffe8914819796c18abaf0ddc41fea4
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97845755"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>具有分頁檔案的虛擬硬碟應從複寫中排除

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|資訊|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*分頁檔案應排除于參與複寫之外，但未排除任何磁片。*

## <a name="impact"></a>影響
*分頁檔案會遇到大量的輸入/輸出活動，不必要地需要更多資源來參與複寫。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*如果您尚未這麼做，請為 Windows 分頁檔案建立個別的虛擬硬碟。如果已完成初始複寫，請使用 Hyper-v 管理員來移除複寫。然後，再次設定複寫，並將含有分頁檔的虛擬硬碟從複寫中排除。*



