---
title: 設定檔案檢測稽核
description: 本文說明如何設定檔檢測稽核以產生 [檔案檢測稽核] 報告
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: cf1824e514c34ee89870daa6d15190bffd822a8b
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475315"
---
# <a name="configure-file-screen-audit"></a>設定檔案檢測稽核

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您可以使用檔案伺服器資源管理員，將檔案檢測活動記錄在稽核資料庫中。 儲存在此資料庫中的資訊可用來產生 [檔案檢測稽核] 報告。

> [!Important]
> 如果 **\[在稽核資料庫中記錄檔案檢測活動\]** 核取方塊已清除，檔案檢測稽核報告就會不包含任何資訊。

## <a name="to-configure-file-screen-audit"></a>若要設定檔案檢測稽核

1.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]**，然後按一下 **\[設定選項\]**。 [檔案伺服器資源管理員選項]**** 對話方塊隨即開啟。

2.  在 **\[檔案檢測稽核\]** 索引標籤上，選取 **\[在稽核資料庫中記錄檔案檢測活動\]** 核取方塊。

3.  按一下 [確定]****。 所有的檔案檢測活動現在都會都儲存在稽核資料庫中，您可以執行 [檔檢測稽核] 報告來檢視該資料庫。

## <a name="additional-references"></a>其他參考

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)
-   [存放裝置報告管理](storage-reports-management.md)