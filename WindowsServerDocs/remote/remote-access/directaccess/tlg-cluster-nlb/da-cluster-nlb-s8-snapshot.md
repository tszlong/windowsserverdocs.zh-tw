---
title: 步驟8：設定 DirectAccess 叢集的快照-NLB 設定
description: 瞭解如何建立和儲存 DirectAccess 叢集-NLB 設定測試實驗室的快照。
manager: brianlic
ms.topic: article
ms.assetid: 915ef7dd-169d-4d58-9174-438d8ffa3584
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: c9eb0664d878c0050aae238cd0ab3fbc7a716d89
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2021
ms.locfileid: "98040228"
---
# <a name="step-8-snapshot-the-directaccess-cluster-nlb-configuration"></a>步驟8：設定 DirectAccess 叢集的快照-NLB 設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

這會完成 DirectAccess 測試實驗室。 若要儲存這項設定，讓您可以快速返回具有 NLB 叢集設定的運作中 DirectAccess，讓您可以測試其他 DirectAccess 模組化測試實驗室指南、測試實驗室指南延伸模組，或進行您自己的實驗和學習，請執行下列動作：

1.  在測試實驗室的所有實體電腦或虛擬機器上，關閉所有的視窗，然後執行正常關機。

2.  如果您的實驗室以虛擬機器為基礎，請儲存每部虛擬機器的快照集，並將快照集命名為 DirectAccess 叢集和 NLB。 如果您的實驗室使用實體電腦，請建立磁片映射以儲存 DirectAccess 測試實驗室設定。
