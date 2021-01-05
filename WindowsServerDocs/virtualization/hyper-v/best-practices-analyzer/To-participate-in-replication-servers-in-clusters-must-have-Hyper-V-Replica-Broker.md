---
title: 若要參與複寫，容錯移轉叢集中的伺服器必須已設定 Hyper-v 複本代理程式
description: 瞭解當容錯移轉叢集時，Hyper-v 複本需要使用 Hyper-v 複本代理人名稱，而不是個別的伺服器名稱。
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
ms.date: 8/16/2016
ms.openlocfilehash: df1b6143072255f4b835c380f36d1cf59186a676
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846351"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>若要參與複寫，容錯移轉叢集中的伺服器必須已設定 Hyper-v 複本代理程式

>適用於：Windows Server 2016

如需最佳做法與掃描的相關詳細資訊，請參閱[執行最佳做法分析程式掃描及管理掃描結果](https://go.microsoft.com/fwlink/p/?LinkID=223177)。

|屬性|詳細資料|
|-|-|
|**作業系統**|Windows Server 2016|
|**產品/功能**|Hyper-V|
|**嚴重性**|錯誤|
|**類別**|組態|

在下列各節中，斜體指出出現在此問題的最佳做法分析程式工具中的 UI 文字。

## <a name="issue"></a>問題
*針對容錯移轉叢集，Hyper-v 複本需要使用 Hyper-v 複本代理程式名稱，而不是個別的伺服器名稱。*

## <a name="impact"></a>影響
*如果虛擬機器移至不同的容錯移轉叢集節點，則無法繼續複寫。*

## <a name="resolution"></a>解決方案
*使用容錯移轉叢集管理員來設定 Hyper-v 複本代理程式。在 [Hyper-v 管理員] 中，確定複寫設定會使用 Hyper-v 複本代理程式名稱做為伺服器名稱。*



