---
title: 執行 WSUS 複本模式
description: 'Windows Server Update Service (WSUS) 主題-如何設定複寫模式 '
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4139354a3f0f7b1f1a97107d2f6b28db2b02c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878739"
---
# <a name="running-wsus-replica-mode"></a>執行 WSUS 複本模式

>適用於：Windows Server （半年通道），Windows Server 2016 中，Windows Server 2012 R2 中，Windows Server 2012

複寫模式中執行的 WSUS 伺服器會繼承更新核准以及建立管理伺服器上的電腦群組。 在案例中，使用複本模式，您通常會有單一管理伺服器，而一或多個從屬複寫 WSUS 伺服器分散在整個組織，根據站台或組織的拓撲。 核准更新，並管理伺服器，然後會鏡像複本模式伺服器上建立電腦群組。 複本模式伺服器可以設定只有在 WSUS 安裝期間，如果您實作此案例中，則可能是因為它是在您更新核准的組織中重要和集中管理電腦群組。

如果在複寫模式中執行您的 WSUS 伺服器，您將能夠將主要包含在伺服器上執行僅有限的管理功能：

-   新增和移除電腦群組中的電腦。 電腦群組成員資格不會發佈到複本伺服器，只在電腦群組本身。 因此，在複本模式伺服器上，您會繼承您在管理伺服器建立的電腦群組。 不過，電腦群組會是空的。 連接到複本伺服器的電腦群組的電腦時，必須指派用戶端。

-   設定同步處理排程

-   指定 proxy 伺服器設定

-   指定更新來源。 這可以是管理伺服器以外的伺服器

-   檢視可用的更新

-   監視更新、 同步處理、 電腦狀態，以及在伺服器上的 WSUS 設定

-   執行所有標準 WSUS 報告複本模式伺服器上可用



