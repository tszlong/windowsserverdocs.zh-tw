---
title: 建立檔案檢測例外
description: 本文說明如何建立檔案檢測例外
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6a0fa660db6b03104b585c8ee78a4f20aafe5c88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403148"
---
# <a name="create-a-file-screen-exception"></a>建立檔案檢測例外

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您有時需要允許檔案檢測有例外。 例如，您可能會想要封鎖檔案伺服器的視訊檔案，但卻必須允許訓練小組儲存視訊檔案以進行他們的電腦輔助訓練。 若要允許其他檔案檢測封鎖中的檔案，請建立*檔案檢測例外*。

檔案檢測例外是特殊類型的檔案檢測，這些檢測會覆寫任何以其他方式套用至位於指定例外路徑之資料夾及其子資料夾的檔案檢測。 也就是，它會建立任何衍生自上層資料夾之規則的例外。

> [!Note]
> 您無法在已定義檔案檢測的上層資料夾中建立檔案檢測例外。 您必須將例外指派給子資料夾，或變更現有的檔案檢測。

<br />
您可以指派檔案群組來決定檔案檢測例外允許哪些檔案類型。

## <a name="to-create-a-file-screen-exception"></a>若要建立檔案檢測例外

1.  在 **\[檔案檢測管理\]** 中，按一下 **\[檔案檢測\]** 節點。

2.  在 **\[檔案檢測\]** 上按滑鼠右鍵，再按一下 **\[建立檔案檢測例外\]** (或從 **\[動作\]** 窗格中選取 **\[建立檔案檢測例外\]** )。 如此會開啟 **\[建立檔案檢測例外\]** 對話方塊。

3.  在 **\[例外路徑\]** 文字方塊中，輸入或選取例外會套用到的路徑。 此例外將會套用至選取的資料夾及其所有的子資料夾。

4.  若要指定從檢測檔案排除哪些檔案：

    -   在 **\[檔案群組\]** 中，選取您要從檔案檢測排除的每個檔案群組 (若要選取檔案群組的核取方塊，按兩下檔案群組標籤即可)。
    -   如果您想要查看檔案群組包含和排除的檔案類型，請按一下 [檔案群組] 標籤，然後按一下 [ **編輯**]。
    -   若要建立新的檔案群組，按一下 **\[建立\]** 。

5.  按一下 [確定]。

## <a name="see-also"></a>另請參閱

-   [檔案檢測管理](file-screening-management.md)
-   [定義檢測的檔案群組](define-file-groups-for-screening.md)


