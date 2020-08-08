---
title: 建議將壓縮用於複寫流量
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2184a2ec0441af7fdbfc7566d783bee82f6bbbb9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954525"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>建議將壓縮用於複寫流量

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
*從主伺服器傳送到複本伺服器的網路複寫流量已解壓縮。*

## <a name="impact"></a>影響
*複寫流量會使用比所需更多的頻寬。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方法
*設定 Hyper-v 複本，以在 Hyper-v 管理員的虛擬機器設定中壓縮透過網路傳輸的資料。您也可以使用 Hyper-v 以外的工具來執行壓縮。*



