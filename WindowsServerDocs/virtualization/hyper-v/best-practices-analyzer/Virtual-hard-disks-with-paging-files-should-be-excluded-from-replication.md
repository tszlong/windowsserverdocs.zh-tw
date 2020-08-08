---
title: 具有分頁檔案的虛擬硬碟應從複寫中排除
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8f5c0cffa986e658d1ca750c11a6204bf8780a32
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960221"
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

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題
*分頁檔案應排除不參與複寫，但未排除任何磁片。*

## <a name="impact"></a>影響
*分頁檔案遇到大量的輸入/輸出活動，因此不必要地需要更多資源來參與複寫。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*如果您尚未這麼做，請為 Windows 分頁檔建立個別的虛擬硬碟。如果已完成初始複寫，請使用 Hyper-v 管理員來移除複寫。然後，再次設定複寫，並從複寫中排除具有分頁檔案的虛擬硬碟。*



