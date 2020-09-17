---
title: 已設定以憑證為基礎的驗證，但在複本伺服器或容錯移轉叢集節點上未安裝指定的憑證
description: 此最佳做法分析程式規則之文字的線上版本。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
ms.date: 8/16/2016
ms.openlocfilehash: 545a51f110a264e1fb456039362e373a51bcb2f8
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745863"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>已設定以憑證為基礎的驗證，但在複本伺服器或容錯移轉叢集節點上未安裝指定的憑證

>適用於：Windows Server 2016



*如需最佳做法和掃描的詳細資訊，請參閱*[最佳做法分析](https://go.microsoft.com/fwlink/?LinkId=122786)程式。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|設定|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題

*複本伺服器上未安裝 Hyper-v 複本用來提供以憑證為基礎之複寫的安全性憑證 (或任何容錯移轉叢集節點) 。*

## <a name="impact"></a>影響

*發生叢集容錯移轉或移至另一個節點時，如果新節點尚未安裝適當的憑證，Hyper-v 複寫將會暫停。這會影響下列節點：*

\<list of nodes>

## <a name="resolution"></a>解決方案

*將設定的憑證安裝在複本伺服器 (以及容錯移轉叢集中所有相關聯的節點（如果有任何) ）。*



