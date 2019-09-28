---
title: 編輯配額範本內容
description: 本文說明如何編輯配額範本內容，以將變更擴展到由原始配額範本中建立的配額
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 37719656e107869b97045af98c1a63744e4f6b38
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403031"
---
# <a name="edit-quota-template-properties"></a>編輯配額範本內容

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

對配額範本進行變更時，您可以選擇將這些變更擴展到由原始配額範本中建立的配額。 您可以選擇只修改那些仍然符合原始範本的配額，或是修改所有衍生自原始範本的配額，無視於這些配額自建立以來所經過的任何修改。 這項功能提供一個可進行所有變更的中心點，以簡化配額內容的更新流程。

> [!Note]
> 如果您選擇將變更套用至所有衍生自原始範本的配額，將會覆寫您所建立的任何自訂配額內容。

## <a name="to-edit-quota-template-properties"></a>若要編輯配額範本內容

1.  在 **\[配額範本\]** 中，選取您要修改的範本。

2.  在配額範本上按滑鼠右鍵，再按一下 **\[編輯範本內容\]** (或在 **\[動作\]** 窗格的 **\[選取的配額範本\]** 下選取 **\[編輯範本內容\]** )。 如此會開啟 **\[配額範本內容\]** 對話方塊。

3.  進行所有必要的變更。 設定及通知選項與您建立配額範本時所能設定的那些選項完全相同。 或者，也可以將其他範本的內容複製過來，再針對此範本修改這些內容。

4.  完成範本內容的編輯時，按一下 **\[確定\]** 。 這樣會開啟 **\[更新衍生自範本的配額\]** 對話方塊。

5.  選取您要套用的更新類型：

    -   如果您的配額已在使用原始範本建立後經過修改，但您不希望加以變更時，請選取 **\[僅將範本套用到符合原始範本的衍生配額\]** 。 這個選項只會更新那些自使用原始範本建立以來未經編輯過的配額。
    -   如果您想要修改所有由原始範本中建立的現有配額，請選取 **\[將範本套用到所有的衍生配額\]** 。
    -   如果您想要保持現有的配額不變，請選取 **\[不要將範本套用到衍生配額\]** 。

6.  按一下 [確定]。

## <a name="see-also"></a>另請參閱

-   [配額管理](quota-management.md)
-   [建立配額範本](create-quota-template.md)


