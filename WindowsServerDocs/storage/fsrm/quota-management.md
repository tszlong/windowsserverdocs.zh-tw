---
title: 配額管理
description: 本文說明如何建立和管理配額
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5a655e28020d08bb1c10fa862c007f914a8cf566
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403074"
---
# <a name="quota-management"></a>配額管理

> 適用於：Windows Server 2019、Windows Server 2016、Windows Server （半年通道）、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

在檔案伺服器資源管理員 Microsoft<sup>®</sup> Management Console (MMC) 嵌入式管理單元的 **\[配額管理\]** 節點，您可以執行下列工作：

-   建立配額以限制磁碟區或資料夾允許使用的空間，並在接近或超過配額限制時產生通知。
-   產生套用至磁碟區或資料夾中所有現有子資料夾以及任何於日後建立之子資料夾的自動套用配額。
-   定義可輕鬆套用至新磁碟區或資料夾供整個組織使用的配額範本。

例如，您可以：

-   在使用者的個人伺服器資料夾上加上 200 mb 的限制，並在超過 180 MB 的儲存空間時，將電子郵件通知傳送給您和使用者。
-   在群組的共用資料夾上設定彈性的 500 MB 配額。 達到此儲存空間限制時，群組中的所有使用者都會收到電子郵件通知，指出存放裝置配額已暫時延伸到 520 MB，讓他們可以刪除不必要的檔案，並符合預設的 500 MB 配額原則。
-   在暫存資料夾達到 2 GB 使用量時收到通知，但為了要讓服務持續執行於您的伺服器，並不限制該資料夾的配額。

本節包含下列主題：

-   [建立配額](create-quota.md)
-   [建立自動套用配額](create-auto-apply-quota.md)
-   [建立配額範本](create-quota-template.md)
-   [編輯配額範本內容](edit-quota-template-properties.md)
-   [編輯自動套用配額內容](edit-auto-apply-quota-properties.md)

> [!Note]
> 若要設定電子郵件通知和報告功能，您必須先設定檔案伺服器資源管理員的一般選項。

## <a name="see-also"></a>另請參閱

-   [設定檔案伺服器資源管理員選項](setting-file-server-resource-manager-options.md)


