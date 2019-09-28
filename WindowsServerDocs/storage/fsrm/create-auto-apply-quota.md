---
title: 建立自動套用配額
description: 本文說明如何以配額範本為基礎建立自動套用配額
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 68967ff920f25c05affc206ed45bad9275e781b6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394244"
---
# <a name="create-an-auto-apply-quota"></a>建立自動套用配額

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

您可以利用自動套用配額，將配額範本指派給上層磁碟區或資料夾。 檔案伺服器資源管理員接著就會自動根據該範本產生配額。 配額會針對每個現有的子資料夾和未來建立的子資料夾產生。

例如，您可以針對隨選建立的子資料夾、漫遊設定檔的使用者或新的使用者來定義自動套用配額。 每次建立子資料夾時，系統都會使用上層資料夾的範本自動產生新的配額項目。 這些自動產生的配額接著便可視為 **\[配額\]** 節點底下的個別配額。 每個配額項目都可以個別進行維護。

## <a name="to-create-an-auto-apply-quota"></a>若要建立自動套用配額

1.  按一下 **\[配額管理\]** 中的 **\[配額\]** 節點。

2.  在 **\[配額\]** 上按滑鼠右鍵，再按一下 **\[建立配額\]** (或從 **\[動作\]** 窗格中選取 **\[建立配額\]** )。 如此會開啟 **\[建立配額\]** 對話方塊。

3.  在 **\[配額路徑\]** 下方，輸入配額設定檔要套用到的上層資料夾的名稱，或瀏覽至該資料夾。 自動套用配額將會套用至此子資料夾中的每個 (目前及未來) 子資料夾。

4.  按一下 **\[在現有和新子資料夾自動套用範本並建立配額\]** 。

5.  在 **\[從這個配額範本衍生內容\]** 中，從下拉式清單選取您要套用的配額範本。 請注意，每個範本的內容都會顯示在 **\[配額內容摘要\]** 下方。

6.  按一下 [建立]。

> [!Note]
> 您可以選取 **\[配額\]** 節點然後選取 **\[重新整理\]** ，來檢查所有自動產生的配額。 將會列出每個子資料夾的個別配額套以及上層磁碟區或資料夾的自動套用配額設定檔。

## <a name="see-also"></a>另請參閱

-   [配額管理](quota-management.md)
-   [編輯自動套用配額內容](edit-auto-apply-quota-properties.md)