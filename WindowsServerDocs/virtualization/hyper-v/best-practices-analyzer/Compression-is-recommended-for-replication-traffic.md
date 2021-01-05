---
title: 建議將壓縮用於複寫流量
description: 瞭解當透過網路從主伺服器傳送到複本伺服器的複寫流量未壓縮時，該怎麼辦。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
ms.date: 8/16/2016
ms.openlocfilehash: 41825e6484c7cef85b11795a68df501b13c4881e
ms.sourcegitcommit: 48d45b2adf44afb0207214be9c57fe589360d177
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/31/2020
ms.locfileid: "97833643"
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

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*透過網路從主伺服器傳送到複本伺服器的複寫流量會解壓縮。*

## <a name="impact"></a>影響
*複寫流量會使用比所需更多的頻寬。這會影響下列虛擬機器：*

\<list of virtual machines>

## <a name="resolution"></a>解決方案
*設定 Hyper-v 複本，以在 Hyper-v 管理員的虛擬機器設定中壓縮透過網路傳輸的資料。您也可以使用 Hyper-v 以外的工具來執行壓縮。*



