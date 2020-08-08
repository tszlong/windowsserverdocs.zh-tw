---
title: 以憑證為基礎的驗證已設定，但指定的憑證未安裝在複本伺服器或容錯移轉叢集節點上
description: 此最佳做法分析程式規則的線上版本文字。
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8205ea267750e3fec78a756da00b0bd063ed8baf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948511"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>以憑證為基礎的驗證已設定，但指定的憑證未安裝在複本伺服器或容錯移轉叢集節點上

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體表示在此問題的最佳做法分析程式工具中出現的 UI 文字。

## <a name="issue"></a>問題

*Hyper-v 複本已設定用來提供以憑證為基礎之複寫的安全性憑證，未安裝在複本伺服器 (或任何) 的容錯移轉叢集節點上。*

## <a name="impact"></a>影響

*當叢集容錯移轉或移至另一個節點時，如果新節點也沒有安裝適當的憑證，Hyper-v 複寫就會暫停。這會影響下列節點：*

\<list of nodes>

## <a name="resolution"></a>解決方法

*在複本伺服器 (和容錯移轉叢集中所有相關聯的節點（如果有) 的話）上安裝已設定的憑證。*



