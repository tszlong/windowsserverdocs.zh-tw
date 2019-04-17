---
title: "編輯檔案檢測範本內容"
description: "本文說明如何編輯檔案檢測範本內容"
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 31ca46707a32d23a5dd9606c57bcaec5d6e53a80
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2017
---
# <a name="edit-file-screen-template-properties"></a>編輯檔案檢測範本內容

> 適用於：Windows Server (半年度管道)、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

對檔案檢測範本進行變更時，您可以選擇將這些變更擴展到使用原始檔案檢測範本所建立的檔案檢測。 您可以選擇只修改那些符合原始範本的檔案檢測，或是修改所有衍生自原始範本的檔案檢測，無視於這些檔案檢測自建立以來所經過的任何修改。 這項功能提供一個可進行所有變更的中心點，以簡化檔案檢測內容的更新流程。

> [!Note]
> 如果您將變更套用至所有衍生自原始範本的檔案檢測，將會覆寫您所建立的任何自訂檔案檢測內容。

## <a name="to-edit-file-screen-template-properties"></a>若要編輯檔案檢測範本內容

1.  在 **\[檔案檢測範本\]** 中，選取您要修改的範本。

2.  在檔案檢測範本上按滑鼠右鍵，再按一下 **\[編輯範本內容\]** (或在 **\[動作\]** 窗格的 **\[選取的檔案檢測範本\]** 下選取 **\[編輯範本內容\]**)。如此會開啟 **\[檔案檢測範本內容\]** 對話方塊。

3.  如果您要複製其他範本的內容做為已修改範本的基礎，請從 **\[從範本複製內容\]** 下拉式清單中選取範本， 然後按一下 **\[複製\]**。

4.  進行所有必要的變更。 設定及通知選項與您建立檔案檢測範本時提供的那些選項完全相同。

5.  完成範本內容的編輯時，按一下 **\[確定\]**。 這樣會開啟 **\[更新衍生自範本的檔案檢測\]** 對話方塊。

6.  選取您要套用的更新類型：

    -   如果您的檔案檢測已在使用原始範本建立後經過修改，但您不希望加以變更時，請按一下 **\[僅將範本套用到符合原始範本的衍生檔案檢測\]**。 這個選項只會更新那些自使用原始範本內容建立以來未經編輯過的檔案檢測。
    -   如果您想要修改所有使用原始範本所建立的現有檔案檢測，請按一下 **\[將範本套用到所有的衍生檔案檢測\]**。
    -   如果您想要保持現有的檔案檢測不變，請按一下 **\[不要將範本套用到衍生檔案檢測\]**。

7.  按一下 **\[確定\]**。

## <a name="see-also"></a>請參閱

-   [檔案檢測管理](file-screening-management.md)
-   [建立檔案檢測範本](create-file-screen-template.md)


