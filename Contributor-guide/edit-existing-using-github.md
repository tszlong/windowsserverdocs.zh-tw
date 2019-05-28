---
title: 編輯現有的 Windows Server 發行項使用 GitHub 和 Visual Studio Code
description: 如何編輯現有 Windows Server 相關文章，使用 GitHub 和 Visual Studio Code 中，身為 Microsoft 員工。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624581"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>編輯現有的 Windows Server 發行項使用 GitHub 和 Visual Studio Code

有兩個不同的位置，我們用來將 Windows Server 技術內容。 其中一個位置為公用 (windowsserverdocs)，另一個是私用 (windowsserverdocs pr)。 您參與的位置來決定您的身分：

- **我是 Microsoft 員工。** 為 Microsoft 員工，您會有根據您要嘗試執行的選項：

    - **建立全新的發行項。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具，分叉和複製 windowsserverdocs pr 存放庫中，設定您的遠端分支，建立發行項，最後建立的核准及發行新的提取要求。 這些指示中，您可以依照中的指示[建立新的 Windows Server 文件使用 GitHub 和 Visual Studio Code](create-new-using-github.md)文章。

    - **對現有的發行項中的大型的變更。** 若要對現有的發行項進行大幅變更，您可以依照這篇文章中的指示。

    - **對現有的文章進行次要的變更。** 若要讓現有的發行項微幅的變更，您可以依照中的指示[更新現有的 Windows Server 發行項使用網頁瀏覽器和 GitHub](github-browser-updates.md)文章。

- **我並不是 Microsoft 員工。** 為非 Microsoft 員工，您必須參與的公用位置。 如需如何執行該動作的資訊，請參閱[參與 Windows Server 技術文件](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)文章。

## <a name="switch-your-repo-and-create-a-new-branch"></a>切換您的存放庫，並建立新的分支

請遵循下列步驟來編輯現有的發行項。

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>建立新的分支，並找出您想要更新的檔案

您可以開始著手在您的內容之前，您必須先將變更為 windowsserverdocs pr 存放庫，然後找出您想要更新的發行項。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中建立新的分支

- 開啟 Git Bash，並輸入命令 （一次一個）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >此外，我們強烈建議您命名您的分支明顯且唯一的項目，您可以稍後再找到它。

    命令完成之後，您就可以在新的分支，並已準備好編輯您的檔案。

#### <a name="to-locate-your-article-and-make-your-edits"></a>若要找出您的文章，並進行編輯

1. 開啟 Visual Studio Code 並移至**檔案**，選取**開啟資料夾**，然後移至 GitHub 資料夾的位置具有您想要編輯的發行項。

2. 從**總管** 窗格中，選取的檔案。

3. 更新的資訊，在頁面上，然後按**檔案** > **儲存**。

### <a name="preview-your-text"></a>預覽您的文字

更新文字之後，您必須預覽您的變更，以確定它們都正確出現。

#### <a name="to-preview-your-text"></a>若要預覽文字

1. 在 Visual Studio Code 中，選取任一**預覽**從右上角的按鈕。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-full-page.png):切換至 整頁預覽您的內容。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-side-by-side.png):開啟旁您工作 頁面上，並排顯示的 預覽 頁面。

2. 請確定您的文章會尋找您對它看起來所做的預期。

    您確定外觀之後，您就可以認可您的變更，並建立發行集的提取要求。

### <a name="commit-your-changes"></a>認可您的變更

之後，請確定您的文字看起來對不對您就可以將變更認可到存放庫的本機版本。

#### <a name="to-commit-your-changes"></a>若要認可您的變更

- 開啟 Git Bash，並輸入命令 （一次，移除選擇性標記一個）：

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    [選擇性的 git 狀態] 命令會顯示在這個認可已修改的檔案。 選擇性的 git 提取上游的主要提取下 MicrosoftDocs 主要分支，同步處理本機內容與主要的主要內容的最新內容變更。 如此可讓您可以加以修正，才能得到 PR 階段，任何潛在的合併衝突，事先告訴您。

### <a name="submit-a-pull-request-for-review-and-publication"></a>提交進行審查與發行集的提取要求

您已完成更新之後，您必須從您的寫入器來取得核准 （允許一些時間） 的發行集。

#### <a name="to-submit-your-pull-request"></a>若要提交您的提取要求

1. 移至 https://github.com/MicrosoftDocs/windowsserverdocs-pr，然後選取**提取要求** 索引標籤。

2. 在 **檢閱者**區域中的右窗格中，選取齒輪圖示，，然後輸入_windowsservercontent_供檢閱的別名。

    成員_windowsservercontent_別名會檢閱您的變更或新增註解，必須先變更合併可能會發生的事情。

3. 型別 **#sign 關閉**成註解，讓檢閱者知道您要交由供您檢閱和發佈。 **#Sign 關閉**註解：

    - 更新您的提取要求中的標籤**do not 合併**要**已準備好要合併**。

    - 可讓別名和寫入器知道您準備好讓您檢閱的內容。

    - 可讓系統管理員知道核准之後，您的內容已準備好移至即時。

    >[!Important]
    >Windowsservercontent 小組成員加入 #sign 關閉註解之後，會檢閱文字，然後推送至 master，因此它會在下一個送出排定發行至即時 （10:30 和下午 3:30 工作日）。
    >
    >如果您不將 #sign 關閉為最後一個註解加入您的 PR，您的內容仍在佇列中沒有要推送至 Master 和最終存留。

## <a name="related-information"></a>相關的資訊

如需 GitHub 和 markdown 語言的詳細資訊，請參閱：

### <a name="git-concepts"></a>Git 概念

- [GitHub 指南-Git Handbook 簡介](https://guides.github.com/introduction/git-handbook/)

- [GitHub 指南分叉的專案](https://guides.github.com/activities/forking/)

- [GitHub 指南-了解 GitHub 流程](https://guides.github.com/introduction/flow/)

- [了解 Git 分支](https://learngitbranching.js.org/ (適合 visual 觀摩者 ！))

### <a name="markdown"></a>Markdown

- [我們的內部 markdown 指引](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [外部，GitHub 教學課程](https://www.markdowntutorial.com/)