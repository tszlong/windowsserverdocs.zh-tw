---
title: 執行 WSUS 複本模式
description: Windows Server Update Service (WSUS) 主題-如何設定複本模式
ms.topic: article
ms.assetid: d218cd6b-3b6b-4429-913b-31d412ce3356
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1a2d16ddfbd563ae8ef1abf3829849b81e867972
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89641204"
---
# <a name="running-wsus-replica-mode"></a>執行 WSUS 複本模式

>適用於：Windows Server (半年通道)、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在「複本模式」中執行的 WSUS 伺服器會繼承在系統管理伺服器上建立的「更新核准」和「電腦群組」。 在使用複本模式的案例中，您通常會有一部管理伺服器，以及一或多個從屬複本 WSUS 伺服器，會根據網站或組織拓撲分散到整個組織中。 您核准更新，並在管理伺服器上建立電腦群組，然後複本模式伺服器就會將它鏡像。 只有在安裝 WSUS 時，才可以設定複本模式伺服器，如果您執行此案例，很可能是因為您的組織必須集中管理更新核准和電腦群組。

如果您的 WSUS 伺服器是在複本模式中執行，您將只能在伺服器上執行有限的系統管理功能，這主要是由下列各項組成：

-   新增和移除電腦群組中的電腦。 電腦群組成員資格不會散佈到複本伺服器，只有電腦群組本身。 因此，在「複本模式」伺服器上，您將會繼承在系統管理伺服器上建立的電腦群組。 但是，電腦群組將會是空的。 接著，您必須將連接到複本伺服器的用戶端電腦指派給電腦群組。

-   設定同步處理排程

-   指定 proxy 伺服器設定

-   指定更新來源。 這可以是系統管理伺服器以外的伺服器

-   檢視可用的更新

-   監視伺服器上的更新、同步處理、電腦狀態和 WSUS 設定

-   執行可在複本模式伺服器上使用的所有標準 WSUS 報表



