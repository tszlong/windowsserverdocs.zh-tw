---
title: 檔案檢測管理
description: 本文說明如何建立檔案檢測、產生通知、定義檔檢測範本，以及建立檔案檢測例外
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ac5032630f960329675f896a303ef197d6a4dbb3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403102"
---
# <a name="file-screening-management"></a>檔案檢測管理

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

在檔案伺服器資源管理員 Microsoft® Management Console (MMC) 嵌入式管理單元的 **\[檔案檢測管理\]** 節點，您可以執行下列工作：

-   建立檔案檢測以控制使用者可以儲存的檔案類型，以及在使用者嘗試儲存未授權檔案時產生通知。
-   定義可套用至新磁碟區或資料夾且可供整個組織使用的檔案檢測範本。
-   建立擴大檔案檢測規則彈性的檔案檢測例外。

例如，您可以：

-   確保伺服器上的個人化資料夾沒有儲存任何音樂檔案，但您可以允許儲存支援合法版權管理或遵守公司原則的特定類型媒體檔案。 在同一個案例中，您可能會想要將特殊權限提供給公司的副總裁，以便將任何類型的檔案儲存在他的個人資料夾中。
-   實作檢測程序以便在將可執行檔存放於共用資料夾時透過電子郵件通知您 (包括儲存檔案的使用者、檔案的確切位置的相關資訊)，讓您可以執行適當的預防性步驟。

本節包含下列主題：

-   [定義檢測的檔案群組](define-file-groups-for-screening.md)
-   [建立檔案檢測](create-file-screen.md)
-   [建立檔案檢測例外](create-file-screen-exception.md)
-   [-建立檔案檢測範本](create-file-screen-template.md)
-   [編輯檔案檢測範本內容](edit-file-screen-template-properties.md)

> [!Note]
> 若要設定電子郵件通知和某些報告功能，您必須先設定檔案伺服器資源管理員的一般選項。

## <a name="see-also"></a>另請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)


