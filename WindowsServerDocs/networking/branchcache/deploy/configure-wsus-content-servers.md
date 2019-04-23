---
title: 設定 Windows Server Update Services (WSUS) 內容伺服器
description: 本主題是 BranchCache 部署指南的 Windows Server 2016 中，示範如何以最佳化 WAN 頻寬使用量，在分公司的分散式和裝載式快取模式部署 BranchCache 的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873839"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>設定 Windows Server Update Services (WSUS) 內容伺服器

>適用於：Windows Server （半年通道），Windows Server 2016

安裝 BranchCache 功能啟動 BranchCache 服務後，WSUS 伺服器必須設定儲存在本機電腦上的更新檔案。 

當您設定 WSUS 伺服器，來儲存在本機電腦上的更新檔案時，會下載更新中繼資料和更新檔，並儲存直接在 WSUS 伺服器時。 這可確保 BranchCache 用戶端電腦接收 Microsoft 產品更新檔案，從 WSUS 伺服器，而不是直接從 Microsoft Update 網頁站台。  
  
如需有關 WSUS 同步處理的詳細資訊，請參閱[更新同步處理設定](https://technet.microsoft.com/library/mt612311.aspx)  