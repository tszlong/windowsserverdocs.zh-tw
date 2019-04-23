---
title: 編輯自動套用配額內容
description: 本文說明如何編輯自動套用配額內容
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: aa2155268d42293ade925d53da5e29142d13aae4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878059"
---
# <a name="edit-auto-apply-quota-properties"></a>編輯自動套用配額內容

> 適用於：Windows Server （半年通道）、 Windows Server 2016、 Windows Server 2012 R2、 Windows Server 2012、 Windows Server 2008 R2

對自動套用配額進行變更時，您可以選擇將這些變更擴展到自動套用配額路徑中的現有配額。 您可以選擇只修改那些仍然符合原始自動套用配額的配額，或是修改自動套用配額路徑中的所有配額，無視於這些配額自建立以來所經過的任何修改。 這項功能提供一個可進行所有變更的中心點，以簡化衍生於自動套用配額之配額的內容更新流程。

> [!Note]
> 如果您選擇將變更套用到自動套用配額路徑中的所有配額，將會覆寫您所建立的任何自訂配額內容。

## <a name="to-edit-an-auto-apply-quota"></a>若要編輯自動套用配額

1.  在 **\[配額\]** 中，選取您要修改的自動套用配額。 您可以篩選配額，只顯示自動套用配額。

2.  在配額項目上按滑鼠右鍵，再按一下 **\[編輯配額內容\]** (或在 **\[動作\]** 窗格的 **\[選取的配額\]** 下選取 **\[編輯配額內容\]**)。 如此會開啟 **\[編輯自動套用配額\]** 對話方塊。

3.  在 **\[從這個配額範本衍生內容\]** 中，選取您要套用的配額範本。 您可以在摘要清單方塊中檢閱每個配額範本的內容。

4.  按一下 [確定] 。 這樣會開啟 **\[更新衍生自自動套用配額的配額\]** 對話方塊。

5.  選取您要套用的更新類型：

    -   如果您的配額已在其自動產生後經過修改，但您不想要變更這些配額時，請選取 **\[僅將自動套用配額，套用到符合原始自動套用配額的衍生配額\]**。 此選項只會更新那些在自動套用配額路徑中、自其自動產生以來未經編輯過的配額。
    -   如果您想要修改自動套用配額路徑中的所有現有配額，請選取 **\[將自動套用配額套用到所有的衍生配額\]**。
    -   如果您想要保持現有的配額不變，但是會讓修改過的自動套用配額對自動套用配額路徑中新的子資料夾生效時，請選取 **\[不要將自動套用配額套用到衍生配額\]**。

6.  按一下 [確定] 。

## <a name="see-also"></a>另請參閱

-   [配額管理](quota-management.md)
-   [建立自動套用配額](create-auto-apply-quota.md)


