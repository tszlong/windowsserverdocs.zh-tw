---
title: 安裝檔案服務內容伺服器
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4fb9e40ce34a82a8797db1bf6d61c739f742c2d3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319245"
---
# <a name="install-file-services-content-servers"></a>安裝檔案服務內容伺服器

>適用於：Windows Server (半年通道)、Windows Server 2016

若要部署執行檔案服務伺服器角色的內容伺服器，您必須安裝檔案服務伺服器角色的 [網路檔案的 BranchCache] 角色服務。 此外，您必須根據您的需求，在檔案共用上啟用 BranchCache。  
  
在設定內容伺服器期間，您可以允許 BranchCache 發佈所有檔案共用的內容，也可以選取要發佈的檔案共用子集。  
  
> [!NOTE]  
> 當您部署已啟用 BranchCache 的檔案伺服器或網頁伺服器做為內容伺服器時，現在會在 BranchCache 用戶端要求檔案之前，先離線計算內容資訊。 由於這項改善，您不需要設定內容伺服器的雜湊發行，如同您在先前的 BranchCache 版本中所做的一樣。  
>   
> 此自動雜湊產生可提供更快的效能和更多的頻寬節約，因為內容資訊已準備好供要求內容的第一個用戶端使用，而且已經執行過計算。  
  
請參閱下列主題以部署內容伺服器。  
  
-   [設定檔案服務伺服器角色](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [啟用檔案伺服器的雜湊發行](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [在檔案共用&#40;上啟用 BranchCache （選擇性）&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


