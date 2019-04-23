---
title: 安裝檔案服務內容伺服器
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496c0c1408c64216f29a31d5b22d3d9b48d4f44c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855059"
---
# <a name="install-file-services-content-servers"></a>安裝檔案服務內容伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

若要部署執行的檔案服務伺服器角色的內容伺服器，您必須安裝網路檔案的檔案服務伺服器角色 」 角色服務的 BranchCache。 此外，您必須在檔案共用上啟用 BranchCache，根據您的需求。  
  
在設定期間的內容伺服器，您可以允許 BranchCache 發行集的所有檔案共用的內容，或您可以選取要發佈的檔案共用的子集。  
  
> [!NOTE]  
> 當您部署的 BranchCache 啟用的檔案伺服器或 Web 伺服器，做為內容的伺服器時，內容的資訊現在離線計算，BranchCache 用戶端要求檔案之前。 這項改善，因為您不需要設定雜湊發行 內容伺服器使用，如同在舊版的 BranchCache。  
>   
> 此自動雜湊產生功能提供效能更快且節省更多頻寬，因為內容資訊可供要求的內容，第一個用戶端，並已執行計算。  
  
請參閱下列主題，以部署內容伺服器。  
  
-   [設定檔案服務伺服器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [啟用檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [啟用檔案共用上的 BranchCache&#40;選擇性&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


