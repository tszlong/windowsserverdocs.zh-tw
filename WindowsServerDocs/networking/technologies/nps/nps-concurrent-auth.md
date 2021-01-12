---
title: 增加 NPS 所處理的並行驗證
description: 瞭解如何藉由增加 NPS 與網域控制站之間允許的並行驗證數目來改善 NPS 效能。
manager: brianlic
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: fd48dc876cd2b8a24199ffc4fc95b8fa5a6d0d72
ms.sourcegitcommit: d42b80f947dbfa8660d982be67d77745a28081e5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2021
ms.locfileid: "98113374"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加 NPS 所處理的並行驗證

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題，以取得設定網路原則伺服器並行驗證的指示。

如果您在網域控制站以外的電腦上安裝了網路原則伺服器 \( NPS， \) 且 nps 每秒都會收到大量的驗證要求，您可以藉由增加 nps 與網域控制站之間允許的並行驗證數目來改善 nps 效能。

若要這樣做，您必須編輯下列登錄機碼：

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

加入名為 **MaxConcurrentApi** 的新值，並指派2到5的值給它。

>[!CAUTION]
>如果您將值指派給太高的 **MaxConcurrentApi** ，您的 NPS 可能會在您的網域控制站上產生過多的負載。

如需有關管理 NPS 的詳細資訊，請參閱 [管理網路原則伺服器](nps-manage-top.md)。

如需有關 NPS 的詳細資訊，請參閱 [網路原則伺服器 (nps) ](nps-top.md)。