---
title: 設定 Windows Server Update Services (WSUS) 內容伺服器
description: 瞭解如何設定 Windows Server Update Services (WSUS) 內容伺服器，以將更新檔案儲存在本機電腦上。
manager: brianlic
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1ecd2e601a793690791451bfd39cb949bd08715f
ms.sourcegitcommit: 029b1e19ce11160d5f988046e04a83e8ab5a60dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/05/2021
ms.locfileid: "97904433"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>設定 Windows Server Update Services (WSUS) 內容伺服器

>適用於：Windows Server (半年度管道)、Windows Server 2016

安裝 BranchCache 功能並啟動 BranchCache 服務之後，必須將 WSUS 伺服器設定為將更新檔案儲存在本機電腦上。

當您將 WSUS 伺服器設定為將更新檔案儲存在本機電腦上時，更新中繼資料和更新檔案都會由下載，並且直接儲存在 WSUS 伺服器上。 這可確保 BranchCache 用戶端電腦會從 WSUS 伺服器接收 Microsoft 產品更新檔案，而不是直接從 Microsoft Update 的網站接收。

如需 WSUS 同步處理的詳細資訊，請參閱[設定更新同步](../../../administration/windows-server-update-services/manage/setting-up-update-synchronizations.md)處理