---
title: 增加處理 NPS 同時驗證
description: 本主題提供指示設定 Windows Server 2016 中的網路原則伺服器同時驗證。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: aa70c1a26e2c22d26545e1b46a6151d71a2b4095
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加處理 NPS 同時驗證

>適用於：Windows Server（以每年次管道）、Windows Server 2016

您可以使用本主題適用於設定同時驗證的網路原則伺服器上的指示操作。

如果您的網域控制站以外的電腦上安裝的網路原則伺服器 \(NPS\) NPS 伺服器接收大量驗證要求秒，您可以增加允許 NPS 伺服器之間的網域控制站同時驗證的數目改善 NPS 效能。

若要這樣做，您必須編輯下列機碼： 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

新增新的值名為**MaxConcurrentApi**並指派給其值從到 5 2。 

>[!CAUTION]
>如果您將指派值，以**MaxConcurrentApi**得太高，NPS 伺服器可能對您的網域控制站的負荷。

如需有關管理 NPS 的詳細資訊，請查看[管理的網路原則伺服器]](nps-manage-top.md)。

如需 NPS 的詳細資訊，請查看[的網路原則 Server (NPS)](nps-top.md)。