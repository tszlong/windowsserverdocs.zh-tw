---
title: 步驟8：設定 DirectAccess 叢集的快照-NLB 設定
description: 本主題是測試實驗室指南的一部分-示範 windows Server 2016 的 Windows NLB 叢集中的 DirectAccess
manager: brianlic
ms.topic: article
ms.assetid: 915ef7dd-169d-4d58-9174-438d8ffa3584
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c2f458570f4e584be1f73f9825d39d396546edf3
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87951003"
---
# <a name="step-8-snapshot-the-directaccess-cluster-nlb-configuration"></a>步驟8：設定 DirectAccess 叢集的快照-NLB 設定

>適用於：Windows Server (半年度管道)、Windows Server 2016

這會完成 DirectAccess 測試實驗室。 若要儲存此設定，讓您可以快速地回到具有 NLB 叢集設定的工作中 DirectAccess，您可以在其中測試其他 DirectAccess 模組化測試實驗室指南、測試實驗室指南延伸模組，或是進行您自己的實驗和學習作業，請執行下列動作：

1.  在測試實驗室的所有實體電腦或虛擬機器上，關閉所有的視窗，然後執行正常關機。

2.  如果您的實驗室是以虛擬機器為基礎，請儲存每部虛擬機器的快照集，並將這些快照集命名為 DirectAccess 叢集和 NLB。 如果您的實驗室使用實體電腦，請建立磁片映射以儲存 DirectAccess 測試實驗室設定。
