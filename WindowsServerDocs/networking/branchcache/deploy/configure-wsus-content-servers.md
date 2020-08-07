---
title: 設定 Windows Server Update Services (WSUS) 內容伺服器
description: 本主題是 Windows Server 2016 的 BranchCache 部署指南的一部分，示範如何在分散式和託管快取模式中部署 BranchCache，以優化分公司的 WAN 頻寬使用量。
manager: brianlic
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2af2321a1f87eab1e29ecb6c483ee85c87b08ee7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971835"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>設定 Windows Server Update Services (WSUS) 內容伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

安裝 BranchCache 功能並啟動 BranchCache 服務之後，必須將 WSUS 伺服器設定為將更新檔案儲存在本機電腦上。

當您將 WSUS 伺服器設定成將更新檔案儲存在本機電腦上時，更新中繼資料和更新檔案都會由下載，並且直接儲存在 WSUS 伺服器上。 這可確保 BranchCache 用戶端電腦接收來自 WSUS 伺服器的 Microsoft 產品更新檔案，而不是直接從 Microsoft Update 的網站。

如需 WSUS 同步處理的詳細資訊，請參閱[設定更新同步](https://technet.microsoft.com/library/mt612311.aspx)處理