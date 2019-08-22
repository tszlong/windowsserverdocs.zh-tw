---
title: 使用 GitHub 和 Visual Studio Code 建立新的 Windows Server 文章
description: 如何使用 GitHub 和 Visual Studio Code, 以 Microsoft 員工的身分建立新的 Windows Server 相關文章。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: f5e7e3d0cd17c64175fddaaac73c12daa2c2a32c
ms.sourcegitcommit: ffd9c42374c7448deb5f53f7a865cb427b5e4e9e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887953"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>使用 GitHub 和 Visual Studio Code 建立新的 Windows Server 文章

我們提供兩個不同的位置, 讓我們保留 Windows Server 技術內容。 其中一個位置是公用 (windowsserverdocs), 另一個則是私用 (windowsserverdocs pr)。 決定您要參與的位置:

- **我是 Microsoft 員工。** 身為 Microsoft 員工, 您可以根據嘗試執行的動作來選擇:

    - **建立全新的文章。** 若要建立全新的文章, 您必須建立並設定您的 GitHub 帳戶和工具、派生和複製 windowsserverdocs pr 存放庫、設定遠端分支、建立發行項, 最後建立新的提取要求以供核准和發行。 如需這些指示, 請繼續閱讀本文。

    - **對現有的文章進行大量變更。** 若要對現有的文章進行重大變更, 您可以遵循[使用 GitHub 編輯現有的 Windows Server 文章和 Visual Studio Code](edit-existing-using-github.md)一文中的指示。

    - **對現有的文章進行次要變更。** 若要對現有的文章進行次要變更, 您可以遵循[使用網頁瀏覽器和 GitHub 更新現有的 Windows Server 文章](github-browser-updates.md)中的指示。

- **我不是 Microsoft 員工。** 身為非 Microsoft 員工, 您必須參與公用位置。 如需如何執行這項操作的詳細資訊, 請參閱[參與 Windows Server 技術檔](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)一文。

## <a name="prerequisites"></a>必要條件

開始使用存放庫之前, 您必須先建立並設定您的 GitHub 帳戶、設定雙因素驗證, 以及安裝和設定所有必要的工具。 如果您已經這麼做, 您可以跳到本文的 [[派生存放庫的分支] 區段](#fork-the-repository)。

1. [建立 GitHub 帳戶和設定檔](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [將您的帳戶連結至您的 Microsoft 帳戶以及 Microsoft 和 MicrosoftDocs 組織](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [開啟雙因素驗證](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [授權組建系統存取您的 GitHub 帳戶](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [安裝 Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [安裝 GitHub 和其工具](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [安裝檔製作套件](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>設定您自己的存放庫版本

建立並設定您的 GitHub 帳戶和工具之後, 您可以建立個人版的存放庫。 這是您將建立分支並進行所有變更的位置。

### <a name="fork-the-repository"></a>分支存放庫

您需要原始程式檔的本機複本, 才能從您的分支建立提取要求到生產存放庫。

#### <a name="to-fork-the-repository"></a>若要分叉存放庫

1. 登入您的 GitHub 帳戶, 並移 https://github.com/microsoftdocs/windowsserverdocs-pr 至。

2. 選取 [**分叉**]。

    ![頁面上反白顯示的分叉按鈕](media/create-new-using-github/fork-button.png)

3. 選取您的 GitHub 帳戶作為 [分叉位置]。

    ![頁面上反白顯示的分叉按鈕](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>複製存放庫

您需要將存放庫的本機複本複製到本機裝置上。

#### <a name="to-clone-the-repository"></a>複製存放庫

1. 移至 https://github.com/settings/developers , 然後從左窗格中選取 [**個人存取權杖**]。

2. 選取 [**產生新權杖**], 為您的權杖提供有意義且唯一的名稱、選取所有可用的範圍, 然後選取 [**產生權杖**]。

3. 複製權杖, 並將它放在安全的地方。 在其餘的程式中, 您將需要此檔案, 而離開該頁面之後, 您將無法再回來。

4. 開啟 Git Bash 命令, 並將目錄變更為您要儲存存放庫的位置。 我們建議使用、 `C:\users\<your_name>\GitHub`。

5. 使用您的特定資訊 (一次一個) 輸入下列命令, 以複製您的存放庫並設定遠端分支:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. 執行此命令以確定您的遠端已正確設定:

    `git remote -v`

7. 您應該會看到類似以下的輸出:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    如果您的遠端輸出看起來不像這樣, 您可以先`git remote remove upstream`執行, 再試一次。

## <a name="create-a-branch-and-a-new-article"></a>建立分支和新的文章

請遵循下列步驟來建立發行項。

### <a name="create-a-new-branch-and-a-new-file"></a>建立新的分支和新的檔案

在您開始處理內容之前, 必須先在本機存放庫中建立新的分支。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中建立新的分支

- 開啟 Git Bash 並輸入命令 (一次一個):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >我們強烈建議您將分支命名為明確和唯一的名稱, 讓您稍後可以再次找到。

    命令完成之後, 您將會進入新的分支, 並準備好建立新檔案。 您只需要將每個 Git Bash 實例變更為 windowsserverdocs pr 存放庫一次。 如果您關閉 Git Bash, 您必須在開啟後再次變更目錄。

#### <a name="to-create-a-new-file-in-your-branch"></a>在您的分支中建立新的檔案

1. 開啟 Windows Explorer, 移至您的 GitHub 目錄, 然後在資料夾結構中建立新的文字檔。 您的檔案名必須全部小寫, 並以連字號分隔。 例如, _what-is-windows-server.md_。

     您也必須將副檔名從 .txt 變更為. md。 在出現的錯誤訊息中, 選取 [**是]** 以使用新的副檔名儲存檔案。

2. 開啟 Visual Studio Code 並移至[檔案], 選取 [**開啟資料夾**], 然後移至您在步驟1中建立之檔案的 GitHub 位置。

3. 從 [**瀏覽器**] 窗格中, 選取您的新檔案。

4. 將您的文字加入至頁面。 如果您要建立新的文章, 請確定您已[將必要的元資料標記新增至 Windows Server 相關的文章](metadata-requirements-for-articles.md)。

5. 選取 [檔案] [**儲存**]。  > 

### <a name="preview-your-text"></a>預覽您的文字

將文字新增至新檔案之後, 您必須預覽變更, 以確定其正確顯示。

#### <a name="to-preview-your-text"></a>預覽您的文字

1. 在 Visual Studio Code 中, 選取右上角的其中一個 **預覽** 按鈕。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-full-page.png):切換至內容的整頁預覽。

    ![預覽按鈕圖示](media/create-new-using-github/preview-button-side-by-side.png):在您的工作頁面旁, 並排開啟 [預覽] 頁面。

2. 請確定您的文章看起來會是您預期的外觀。

    確定正確之後, 您可以認可您的變更, 並為發行集建立提取要求。

### <a name="commit-your-changes"></a>認可您的變更

確定您的文字正確之後, 您可以將變更認可到您的存放庫本機版本。

#### <a name="to-commit-your-changes"></a>認可您的變更

- 開啟 Git Bash 並輸入命令 (一次一個, 移除選擇性標記):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    選擇性的 git status 命令會顯示已在此認可中修改哪些檔案。 選擇性的 git pull 上游主機會從 MicrosoftDocs master 分支提取最新的內容變更, 並同步處理您的本機內容與主要內容。 這可協助您預先向您顯示任何可能的合併衝突, 以便在到達 PR 階段之前進行修正。

### <a name="submit-a-pull-request-for-review-and-publication"></a>提交用於審查和發行的提取要求

在您完成文章之後, 您必須從您的寫入器 (允許一些時間) 取得發行集的核准。

#### <a name="to-submit-your-pull-request"></a>提交您的提取要求

1. 移至 https://github.com/MicrosoftDocs/windowsserverdocs-pr 並選取 [**提取要求**] 索引標籤。

2. 在右窗格的 [**審核者**] 區域中, 選取齒輪圖示, 然後輸入要進行審核的_windowsservercontent_別名。

    _Windowsservercontent_別名的成員將會檢查您的變更, 或加入必須變更才能進行合併的批註。

3. 在批註中輸入 **#sign** , 讓審核者知道您要進行審核和發行。 **#Sign 關閉**批註:

    - 將提取要求的標籤從 [**不要合併**] 更新為 [**準備合併**]。

    - 讓別名和寫入器知道您已經準備好要審核您的內容。

    - 讓系統管理員知道在核准之後, 您的內容已準備好上線。

    >[!Important]
    >在您新增 #sign 關閉批註之後, windowsservercontent 小組的成員將會檢查文字並將其推送至 master, 以便在下一次排程的 [發行至 live (10: 30am] 和 [3: 1:30] 工作日) 時消失。
    >
    >如果您未將 #sign 關閉作為 PR 的最後批註, 您的內容將會保留在佇列中, 而不會被推送至 Master, 而且最終會上線。

## <a name="related-information"></a>相關資訊

如需 GitHub 和 markdown 語言的詳細資訊, 請參閱:

### <a name="git-concepts"></a>Git 概念

- [GitHub 指南-Git 手冊簡介](https://guides.github.com/introduction/git-handbook/)

- [GitHub 指南-分支專案](https://guides.github.com/activities/forking/)

- [GitHub 指南-瞭解 GitHub 流程](https://guides.github.com/introduction/flow/)

- [瞭解 Git 分支](https://learngitbranching.js.org/ (適用于 visual 工具學習!))

### <a name="markdown"></a>Markdown

- [我們的內部 markdown 指引](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [外部, GitHub 教學課程](https://www.markdowntutorial.com/)
