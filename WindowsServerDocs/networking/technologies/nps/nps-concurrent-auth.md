---
title: 增加 NPS 所處理的並行驗證
description: 本主題提供在 Windows Server 2016 中設定網路原則伺服器並行驗證的指示。
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 31dec30ba3c843b78974daa7a1e4ed6893b1ebbd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396435"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加 NPS 所處理的並行驗證

>適用於：Windows Server (半年通道)、Windows Server 2016

您可以使用本主題，取得設定網路原則伺服器並行驗證的指示。

如果您已在網域控制站以外的電腦上安裝網路原則伺服器 \(NPS\)，而 NPS 每秒都會收到大量的驗證要求，您可以增加 NPS 和網域控制站之間允許的並行驗證次數，藉此改善 NPS 效能。

若要這樣做，您必須編輯下列登錄機碼： 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

新增名為**MaxConcurrentApi**的新值，並為其指派2到5的值。 

>[!CAUTION]
>如果您將值指派給**MaxConcurrentApi**太高，NPS 可能會在網域控制站上放置過多的負載。

如需管理 NPS 的詳細資訊，請參閱[管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器（NPS）](nps-top.md)。