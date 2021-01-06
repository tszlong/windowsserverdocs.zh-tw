---
title: 步驟3驗證 Advanced DirectAccess 部署
description: 瞭解如何確認您已正確設定您的 advanced DirectAccess 部署。
manager: brianlic
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 38e93e9c40b1e87557bd077f7b9d2129a2e9bae4
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97949814"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>步驟3驗證 Advanced DirectAccess 部署

>適用於：Windows Server (半年度管道)、Windows Server 2016

本主題說明如何確認您已正確設定 DirectAccess 部署。

### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>確認透過 DirectAccess 存取內部資源

1.  將 DirectAccess 用戶端電腦連線到公司網路，並取得群組原則物件。

2.  按一下通知區域中的 [ **網路** 連線] 圖示，以存取 DirectAccess 媒體管理員。

3.  按一下 [ **DirectAccess 連接**]，您會看到狀態是在 **本機連線**。

4.  將用戶端電腦連線到外部網路，並嘗試存取內部資源。

    您應該能夠存取所有公司資源。

## <a name="previous-step"></a><a name="BKMK_Links"></a>上一個步驟

-   [步驟2：設定 DirectAccess 伺服器](./da-adv-configure-s2-servers.md)