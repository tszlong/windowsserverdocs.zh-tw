---
title: 選擇 BranchCache 設計
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 330dcbee26f52ff69cd85ef8dc78d2e161b943d1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811909"
---
# <a name="choosing-a-branchcache-design"></a>選擇 BranchCache 設計

>適用於：Windows Server （半年通道），Windows Server 2016

若要了解 BranchCache 模式，並選取您的部署的最佳模式，您可以使用本主題。  
  
您可以使用本指南中的下列模式和模式組合部署 BranchCache。  
  
-   所有的分公司已針對分散式快取模式。  
  
-   所有的分公司設定為託管快取模式，並在網站上有託管快取伺服器。  
  
-   一些分公司辦公室設定分散式快取模式，而且一些分公司辦公室站台上有託管快取伺服器，並設定為託管快取模式。  
  
下圖說明雙重模式安裝中，設定為分散式快取模式的其中一個分公司與設定為託管快取模式的其中一個分公司。  
  
![選擇 BranchCache 設計](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
部署 BranchCache 之前，請選取您想為每個分公司在組織中的模式。  
  


