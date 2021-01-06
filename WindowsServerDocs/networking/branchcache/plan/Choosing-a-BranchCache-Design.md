---
title: 選擇 BranchCache 設計
description: 瞭解如何瞭解 BranchCache 模式，以及為您的部署選取最佳模式。
ms.topic: how-to
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: 972f2941afa2544307933ec422eb40578e084672
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/06/2021
ms.locfileid: "97946704"
---
# <a name="choosing-a-branchcache-design"></a>選擇 BranchCache 設計

>適用於：Windows Server (半年度管道)、Windows Server 2016

您可以使用本主題來瞭解 BranchCache 模式，以及為您的部署選取最佳模式。

您可以使用本指南，以下列模式和模式組合部署 BranchCache。

-   所有分公司都設定為分散式快取模式。

-   所有分公司都設定為託管快取模式，並在網站上擁有託管快取伺服器。

-   某些分公司是針對分散式快取模式進行設定，而某些分公司在網站上有託管快取伺服器，並設定為託管快取模式。

下圖描述雙重模式安裝，其中有一個分公司設定為分散式快取模式，另一個分公司設定為託管快取模式。

![選擇 BranchCache 設計](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)

在您部署 BranchCache 之前，請為組織中的每個分公司選取您偏好的模式。



