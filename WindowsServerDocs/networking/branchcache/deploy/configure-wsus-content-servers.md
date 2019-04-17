---
title: 設定 Windows Server Update Services (WSUS) 內容伺服器
description: 本主題是 BranchCache 部署節目表適用於 Windows Server 2016 的示範如何將 BranchCache 部署最佳化分公司 WAN 頻寬分散與裝載快取模式中的一部分。
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>設定 Windows Server Update Services (WSUS) 內容伺服器

>適用於：Windows Server（以每年次管道）、Windows Server 2016

安裝 BranchCache 功能，並開始 BranchCache 服務之後, 必須 WSUS 伺服器設定儲存在本機電腦上的 [更新檔案。 

當您設定 WSUS 伺服器以儲存在本機電腦上的 [更新檔案時，將更新檔案和中繼資料更新下載，並儲存直接在 WSUS 伺服器。 這樣可確保 BranchCache client 電腦，會收到 Microsoft product 更新檔案 WSUS 伺服器而不是直接從 Microsoft 網站更新。  
  
如需 WSUS 同步，請查看[更新同步設定](https://technet.microsoft.com/en-us/library/mt612311.aspx)  