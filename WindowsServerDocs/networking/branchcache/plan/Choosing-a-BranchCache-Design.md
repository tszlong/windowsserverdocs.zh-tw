---
title: 選擇 BranchCache 設計
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f64d27f3ef36c587e2ec5c08f51723942ba6a28c
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937507"
---
# <a name="choosing-a-branchcache-design"></a>選擇 BranchCache 設計

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 BranchCache 模式，並為您的部署選取最佳模式。

您可以使用本指南，以下列模式和模式組合來部署 BranchCache。

-   所有分公司都設定為分散式快取模式。

-   所有分公司都設定為託管快取模式，並在網站上擁有託管快取伺服器。

-   某些分公司已設定為分散式快取模式，而某些分公司在網站上有託管快取伺服器，並已設定為託管快取模式。

下圖說明雙重模式安裝，其中有一個分公司設定為分散式快取模式，另一個分公司設定為託管快取模式。

![選擇 BranchCache 設計](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)

部署 BranchCache 之前，請為組織中的每個分公司選取您偏好的模式。



