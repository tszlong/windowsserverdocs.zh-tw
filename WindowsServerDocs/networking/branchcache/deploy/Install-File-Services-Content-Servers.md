---
title: 安裝檔案服務內容伺服器
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>安裝檔案服務內容伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

若要部署服務檔案伺服器角色執行內容伺服器，您必須安裝 BranchCache 服務檔案伺服器角色網路的檔案角色服務。 此外，您必須在檔案共用讓 BranchCache 根據您的需求。  
  
在設定期間的內容伺服器，您可以允許 BranchCache 發行的所有檔案共用 content 或您可以選取一個子集發行檔案共用。  
  
> [!NOTE]  
> 當您部署 BranchCache 讓的檔案伺服器或內容伺服器與 Web 伺服器時，內容資訊現在計算 offline、 之前 BranchCache client 要求檔案。 這項改良功能，因為您不需要設定內容伺服器，hash 發行版本中的上一個 BranchCache 一樣。  
>   
> 這個自動 hash 代提供更快的效能，更多節省的頻寬，因為內容資訊可要求 content，第一個 client 的和已執行計算。  
  
請查看下列主題部署內容伺服器。  
  
-   [設定檔服務伺服器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [支援檔案伺服器 Hash 的發行](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [可讓共用檔案和 #40; 在 BranchCache 選擇性和 #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


