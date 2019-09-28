---
title: 定義檢測的檔案群組
description: '本文說明如何定義檔案群組，以建立檔案檢測、檔案檢測例外或 [檔案: 依檔案群組列表] 存放裝置報告的命名空間'
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b56d7b0439e3dc6f1a2e0a1c96f761dbb77cb0a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394394"
---
# <a name="define-file-groups-for-screening"></a>定義檢測的檔案群組

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

*檔案群組*可用來定義檔案檢測、檔案檢測例外或 **\[檔案: 依檔案群組列表\]** 存放裝置報告的命名空間。 其中包含一組依下列設定分組的檔案名稱模式：

-   **包含的檔案**：屬於群組的檔案
-   **排除的檔案**：不屬於群組的檔案

> [!Note]
> 為了方便起見，您可以在編輯檔案檢測、檔案檢測例外、檔案檢測範本及 **\[檔案: 依檔案群組列表\]** 報告的屬性同時，建立並編輯檔案群組。 您從這些屬性工作表中進行的任何檔案群組變更不會只限於您正在處理的目前項目。

## <a name="to-create-a-file-group"></a>若要建立檔案群組

1.  在 **\[檔案檢測管理\]** 中，按一下 **\[檔案群組\]** 節點。

2.  在 **\[動作\]** 窗格中，按一下 **\[建立檔案群組\]** 。 如此會開啟 **\[建立檔案群組內容\]** 對話方塊

    (或者，在編輯檔案檢測、檔案檢測例外、檔案檢測範本或 **\[檔案: 依檔案群組列表\]** 報告的屬性同時，按一下 **\[維護檔案群組\]** 下方的 **\[建立\]** )。

3.  在 **\[建立檔案群組內容\]** 對話方塊中，輸入檔案群組的名稱。

4.  新增要包含的檔案和要排除的檔案：

    -   在 **\[包含的檔案\]** 方塊中，為您要在檔案群組中包含的每一組檔案輸入檔案名稱模式，然後按一下 **\[新增\]** 。
    -   在 **\[排除的檔案\]** 方塊中，為您要從檔案群組中排除的每一組檔案輸入檔案名稱模式，然後按一下 **\[新增\]** 。
        請注意，標準萬用字元規則適用，例如 **\*** 會選取所有可執行檔。

5.  按一下 [確定]。

## <a name="see-also"></a>另請參閱

-   [檔案檢測管理](file-screening-management.md)
-   [建立檔案檢測](create-file-screen.md)
-   [建立檔案檢測例外](create-file-screen-exception.md)
-   [-建立檔案檢測範本](create-file-screen-template.md)
-   [存放裝置報告管理](storage-reports-management.md)


