---
title: 步驟4驗證叢集
description: 瞭解如何確認您已正確設定 DirectAccess 叢集部署。
manager: brianlic
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 92f51a3b6fa00a06006223abe4f6d28bc40d744f
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947774"
---
# <a name="step-4-verify-the-cluster"></a>步驟4驗證叢集

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定 DirectAccess 叢集部署。

### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>確認透過叢集存取內部資源

1.  將 DirectAccess 用戶端電腦連線到公司網路並取得群組原則。

2.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。

3.  藉由關閉或中斷外部網路（除了其中一部叢集伺服器）的方式，在叢集中的每部伺服器之間測試連線能力。 在用戶端電腦上，嘗試存取公司資源。 在不同的叢集伺服器上重複測試。

    您應該能夠透過每部叢集伺服器來存取所有公司資源。



