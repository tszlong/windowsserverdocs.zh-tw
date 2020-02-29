---
title: 執行 WSUS 複本模式
description: 'Windows Server Update Service （WSUS）主題-如何設定複本模式 '
ms.prod: windows-server
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
ms.openlocfilehash: b7da68fa9cbe71f8a67e74671d64d11908ae4654
ms.sourcegitcommit: 9687d3eb221b89061a48bf1e73fb3b25bee69f9a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/28/2020
ms.locfileid: "78169558"
---
# <a name="running-wsus-replica-mode"></a>執行 WSUS 複本模式

>適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

在 [複本] 模式中執行的 WSUS 伺服器會繼承在管理伺服器上建立的更新核准和電腦群組。 在使用「複本」模式的案例中，您通常會有一部管理伺服器，而且一或多個次級複本 WSUS 伺服器會根據網站或組織的拓撲分散到整個組織。 您在管理伺服器上核准更新並建立電腦群組，然後複本模式伺服器就會進行鏡像。 只有在安裝 WSUS 時，才可以設定複本模式伺服器，而且如果您執行此案例，很可能是因為您的組織中的更新核准和電腦群組是集中管理的，所以這是很重要的。

如果您的 WSUS 伺服器是在「複本」模式下執行，您就只能在伺服器上執行有限的系統管理功能，其主要包含：

-   新增和移除電腦群組中的電腦。 電腦群組成員資格不會散發到複本伺服器，只有電腦群組本身。 因此，在複本模式伺服器上，您將會繼承在管理伺服器上建立的電腦群組。 不過，電腦群組會是空的。 接著，您必須將連接到複本伺服器的用戶端電腦指派給電腦群組。

-   設定同步處理排程

-   指定 proxy 伺服器設定

-   指定更新來源。 這可以是系統管理伺服器以外的伺服器。

-   檢視可用的更新

-   監視伺服器上的更新、同步處理、電腦狀態和 WSUS 設定

-   執行複本模式伺服器上所有可用的標準 WSUS 報告



