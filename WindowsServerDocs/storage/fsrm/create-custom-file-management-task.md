---
title: 建立自訂檔案管理工作
description: 本文說明如何建立自訂檔案管理工作及自訂工作。
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 894c4e3c0b9fa0fde7b749e6effce531c3f999bb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403178"
---
# <a name="create-a-custom-file-management-task"></a>建立自訂檔案管理工作

> 適用於：Windows Server （半年通道）、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2

不一定都要對檔案執行到期動作。 檔案管理工作也可讓您執行自訂的命令。

> [!Note]
> 此程序假設您熟悉檔案管理工作，因此僅探討設定自訂設定所在的 **\[動作\]** 索引標籤。

## <a name="to-create-a-custom-task"></a>若要建立自訂工作

1.  按一下 [檔案管理工作] 節點。

2.  在 **\[檔案管理工作\]** 上按滑鼠右鍵，再按一下 **\[建立檔案管理工作\]** (或按一下 **\[動作\]** 窗格中的 **\[建立檔案管理工作\]** )。 如此會開啟 [建立檔案管理工作] 對話方塊。

3.  在 [動作] 索引標籤中，輸入下列資訊：

    -   [類型]。 從下拉式功能表選取 **\[自訂\]** 。
    -   **\[可執行檔\]** 。 輸入要在檔案管理工作處理檔案時執行的命令，或瀏覽至該命令。 這個可執行檔必須設定為僅可由系統管理員及系統寫入。 如果任何其他使用者擁有可執行檔的寫入存取權，可執行檔將無法正確執行。
    -   **\[命令設定\]** 。 若要設定要在檔案管理工作處理檔案時傳遞給可執行檔的引數，請編輯標籤為 **\[引數\]** 的文字方塊。 若要在文字中插入其他變數，請將游標放在要插入變數的文字方塊中、選取您想要插入的變數，然後按一下 **\[插入變數\]** 。 方括弧中的文字會插入可執行檔可以接收的變數資訊。 例如，@no__t 0Source 檔案路徑 @ no__t-1 變數會插入可執行檔應處理的檔案名。 或者，按一下 **\[工作目錄\]** 按鈕以指定自訂可執行檔的位置。
    -   **\[命令安全性\]** 。 設定這個可執行檔的安全性設定。 預設會以本機服務身分執行命令，這是限制最多的可用帳戶。

4.  按一下 [確定]。

## <a name="see-also"></a>另請參閱

-   [分類管理](classification-management.md)
-   [檔案管理工作](file-management-tasks.md)