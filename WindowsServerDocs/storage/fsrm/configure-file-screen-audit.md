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
ms.openlocfilehash: a9444e158a935f4e931aa7a5d634d970c5acc88e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394573"
---
# <a name="configure-file-screen-audit"></a>設定檔案檢測稽核

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您可以使用檔案伺服器資源管理員，將檔案檢測活動記錄在稽核資料庫中。 儲存在此資料庫中的資訊可用來產生 [檔案檢測稽核] 報告。

> [!Important]
> 如果 **\[在稽核資料庫中記錄檔案檢測活動\]** 核取方塊已清除，檔案檢測稽核報告就會不包含任何資訊。

## <a name="to-configure-file-screen-audit"></a>若要設定檔案檢測稽核

1.  在主控台樹狀目錄中，以滑鼠右鍵按一下 **\[檔案伺服器資源管理員\]** ，然後按一下 **\[設定選項\]** 。 [檔案伺服器資源管理員選項] 對話方塊隨即開啟。

2.  在 **\[檔案檢測稽核\]** 索引標籤上，選取 **\[在稽核資料庫中記錄檔案檢測活動\]** 核取方塊。

3.  按一下 [確定]。 所有的檔案檢測活動現在都會都儲存在稽核資料庫中，您可以執行 [檔檢測稽核] 報告來檢視該資料庫。

## <a name="see-also"></a>另請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)
-   [存放裝置報告管理](storage-reports-management.md)