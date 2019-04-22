---
title: 增加 NPS 所處理的並行驗證
description: 本主題提供有關 Windows Server 2016 中設定網路原則伺服器的同時驗證的指示。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 2d9cdada-0625-41c8-8248-a32259b03e47
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fd930e34a4adf6c55812385b691df3e3575a4280
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818259"
---
# <a name="increase-concurrent-authentications-processed-by-nps"></a>增加 NPS 所處理的並行驗證

>適用於：Windows Server （半年通道），Windows Server 2016

如需有關設定網路原則伺服器的同時驗證，您可以使用本主題。

如果您安裝網路原則伺服器\(NPS\)網域以外的電腦上控制站與 NPS 伺服器正在接收大量每秒的驗證要求，您可以增加的數目，以改善 NPS 效能NPS 與網域控制站之間允許的同時驗證。

若要這樣做，您必須編輯下列登錄機碼： 

`HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Netlogon\Parameters`

加入新的值，名為**MaxConcurrentApi**並指派給它的值從 2 到 5。 

>[!CAUTION]
>如果您將值指派給**MaxConcurrentApi**就太高，您的 NPS 可能會在您的網域控制站上放置過度的負載。

如需管理 NPS 的詳細資訊，請參閱 <<c0> [ 管理網路原則伺服器](nps-manage-top.md)。

如需 NPS 的詳細資訊，請參閱[網路原則伺服器 (NPS)](nps-top.md)。