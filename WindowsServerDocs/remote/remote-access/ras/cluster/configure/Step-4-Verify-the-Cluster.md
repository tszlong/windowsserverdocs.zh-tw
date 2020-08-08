---
title: 步驟4驗證叢集
description: 本主題是在 Windows Server 2016 的叢集中部署遠端存取指南的一部分。
manager: brianlic
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4fa053519116ddadbe1469223001a33384aebe91
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963894"
---
# <a name="step-4-verify-the-cluster"></a>步驟4驗證叢集

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定 DirectAccess 叢集部署。

### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>確認透過叢集存取內部資源

1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。

2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。

3.  藉由關閉或中斷連接外部網路（除了其中一個叢集伺服器），藉此測試叢集中每部伺服器的連線。 在用戶端電腦上，嘗試存取公司資源。 在不同的叢集伺服器上重複測試。

    您應該能夠透過每部叢集伺服器存取所有公司資源。



