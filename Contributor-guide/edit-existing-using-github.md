---
title: 使用 GitHub 和 Visual Studio Code 編輯現有的 Windows Server 文章
description: 如何使用 GitHub 和 Visual Studio Code，做為 Microsoft 員工來編輯現有的 Windows Server 相關文章。
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865076"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>使用 GitHub 和 Visual Studio Code 編輯現有的 Windows Server 文章

我們提供兩個不同的位置，讓我們保留 Windows Server 技術內容。 其中一個位置是公用（windowsserverdocs），另一個則是私用（windowsserverdocs pr）。 決定您要參與的位置：

- **我是 Microsoft 員工。** 身為 Microsoft 員工，您可以根據嘗試執行的動作來選擇：

    - **建立全新的文章。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具、派生和複製 windowsserverdocs pr 存放庫、設定遠端分支、建立發行項，最後建立新的提取要求以供核准和發行。 如需這些指示，您可以遵循[使用 GitHub 建立新的 Windows Server 文章和 Visual Studio Code](create-new-using-github.md)一文中的指示。

    - **對現有的文章進行大量變更。** 若要對現有的文章進行大幅變更，您可以遵循這篇文章中的指示。

    - **對現有的文章進行次要變更。** 若要對現有的文章進行次要變更，您可以遵循[使用網頁瀏覽器和 GitHub 更新現有的 Windows Server 文章](github-browser-updates.md)中的指示。

- **我不是 Microsoft 員工。** 身為非 Microsoft 員工，您必須參與公用位置。 如需如何執行這項操作的詳細資訊，請參閱[參與 Windows Server 技術檔](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)一文。

## <a name="switch-your-repo-and-create-a-new-branch"></a>切換您的存放庫，並建立新的分支

請遵循下列步驟來編輯現有的文章。

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>建立新的分支，並找出您想要更新的檔案

開始處理內容之前，您必須先變更為 windowsserverdocs pr 存放庫，然後找出您想要更新的文章。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中建立新的分支

- 開啟 Git Bash 並輸入命令（一次一個）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我們強烈建議您將分支命名為明確和唯一的名稱，讓您稍後可以再次找到。

    在命令完成之後，您將會進入新的分支，並準備好編輯您的檔案。

#### <a name="to-locate-your-article-and-make-your-edits"></a>尋找您的文章並進行編輯

1. 開啟 Visual Studio Code 並移至 [檔案]，選取 [**開啟資料夾** **]，然後**移至含有您要編輯之文章的資料夾的 GitHub 位置。

2. 在 [**瀏覽器**] 窗格中，選取檔案。

3. 更新頁面上的資訊，**然後選取** > [檔案] [**儲存**]。

### <a name="preview-your-text"></a>預覽您的文字

更新文字之後，您必須預覽變更，以確定其顯示正確。

#### <a name="to-preview-your-text"></a>預覽您的文字

1. 在 Visual Studio Code 中，選取右上角的其中一個 **預覽** 按鈕。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-full-page.png):切換至內容的整頁預覽。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-side-by-side.png):在您的工作頁面旁，並排開啟 [預覽] 頁面。

2. 請確定您的文章看起來會是您預期的外觀。

    確定正確之後，您可以認可您的變更，並為發行集建立提取要求。

### <a name="commit-your-changes"></a>認可您的變更

確定您的文字正確之後，您可以將變更認可到您的存放庫本機版本。

#### <a name="to-commit-your-changes"></a>認可您的變更

- 開啟 Git Bash 並輸入命令（一次一個，移除選擇性標記）：

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    選擇性的 git status 命令會顯示已在此認可中修改哪些檔案。 選擇性的 git pull 上游主機會從 MicrosoftDocs master 分支提取最新的內容變更，並同步處理您的本機內容與主要內容。 這可協助您預先向您顯示任何可能的合併衝突，以便在到達 PR 階段之前進行修正。

### <a name="submit-a-pull-request-for-review-and-publication"></a>提交用於審查和發行的提取要求

完成更新之後，您必須從您的寫入器（允許一些時間）取得發行集的核准。

#### <a name="to-submit-your-pull-request"></a>提交您的提取要求

1. 移至 https://github.com/MicrosoftDocs/windowsserverdocs-pr 並選取 [**提取要求**] 索引標籤。

2. 在右窗格的 [**審核者**] 區域中，選取齒輪圖示，然後輸入要進行審核的_windowsservercontent_別名。

    _Windowsservercontent_別名的成員將會檢查您的變更，或加入必須變更才能進行合併的批註。

3. 在批註中輸入 **#sign** ，讓審核者知道您要進行審核和發行。 **#Sign 關閉**批註：

    - 將提取要求的標籤從 [**不要合併**] 更新為 [**準備合併**]。

    - 讓別名和寫入器知道您已經準備好要審核您的內容。

    - 讓系統管理員知道在核准之後，您的內容已準備好上線。

    >[!Important]
    >在您新增 #sign 關閉批註之後，windowsservercontent 小組的成員將會檢查文字並將其推送至 master，以便在下一次排程的 [發行至 live （10： 30am] 和 [3： 1:30] 工作日）時消失。
    >
    >如果您未將 #sign 關閉作為 PR 的最後批註，您的內容將會保留在佇列中，而不會被推送至 Master，而且最終會上線。

## <a name="related-information"></a>相關資訊

如需 GitHub 和 markdown 語言的詳細資訊，請參閱：

### <a name="git-concepts"></a>Git 概念

- [GitHub 指南-Git 手冊簡介](https://guides.github.com/introduction/git-handbook/)

- [GitHub 指南-分支專案](https://guides.github.com/activities/forking/)

- [GitHub 指南-瞭解 GitHub 流程](https://guides.github.com/introduction/flow/)

- [瞭解 Git 分支](https://learngitbranching.js.org/ (適用于 visual 工具學習！))

### <a name="markdown"></a>Markdown

- [我們的內部 markdown 指引](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [外部，GitHub 教學課程](https://www.markdowntutorial.com/)