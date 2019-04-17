---
title: 選擇 BranchCache 設計
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4fe40b3d9ece771a46af8ecc70297b8713d65875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-branchcache-design"></a>選擇 BranchCache 設計

>適用於：Windows Server（以每年次管道）、Windows Server 2016

深入了解 BranchCache 模式，並選取您的部署的最佳模式，您可以使用此主題。  
  
您可以使用此指南部署 BranchCache 下列模式和模式組合。  
  
-   所有分公司被都設定為分散式快取模式。  
  
-   所有分公司裝載快取模式中的設定，並在網站上有裝載快取伺服器。  
  
-   某些分公司設定為分散式快取模式及某些分公司在網站上有裝載快取伺服器裝載快取模式中的設定。  
  
下圖一個分公司分散式快取模式設定與設定為裝載快取模式一個分公司描述 dual 模式安裝。  
  
![選擇 BranchCache 設計](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
部署 BranchCache 之前，請選取您想要在組織中的每個分公司模式。  
  


