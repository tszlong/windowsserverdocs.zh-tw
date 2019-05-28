---
title: 建立新的 Windows Server 文件使用 GitHub 和 Visual Studio Code
description: 如何建立新 Windows Server 相關的文章，使用 GitHub 和 Visual Studio Code 中，身為 Microsoft 員工。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624558"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>建立新的 Windows Server 文件使用 GitHub 和 Visual Studio Code

有兩個不同的位置，我們用來將 Windows Server 技術內容。 其中一個位置為公用 (windowsserverdocs)，另一個是私用 (windowsserverdocs pr)。 您參與的位置來決定您的身分：

- **我是 Microsoft 員工。** 為 Microsoft 員工，您會有根據您要嘗試執行的選項：

    - **建立全新的發行項。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具，分叉和複製 windowsserverdocs pr 存放庫中，設定您的遠端分支，建立發行項，最後建立的核准及發行新的提取要求。 這些指示中，您可以繼續閱讀這篇文章。

    - **對現有的發行項中的大型的變更。** 若要大幅變更現有的發行項，您可以依照[編輯現有的 Windows Server 發行項使用 GitHub 和 Visual Studio Code](edit-existing-using-github.md)文章。

    - **對現有的文章進行次要的變更。** 若要讓現有的發行項微幅的變更，您可以依照中的指示[更新現有的 Windows Server 發行項使用網頁瀏覽器和 GitHub](github-browser-updates.md)文章。

- **我並不是 Microsoft 員工。** 為非 Microsoft 員工，您必須參與的公用位置。 如需如何執行該動作的資訊，請參閱[參與 Windows Server 技術文件](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)文章。

## <a name="prerequisites"></a>先決條件

您可以開始使用的存放庫之前，您必須建立和設定您的 GitHub 帳戶、 設定雙因素驗證和安裝和設定所有必要的工具。 如果您已這麼做，您可以往下跳到[派生存放庫區段](#fork-the-repository)這篇文章。

1. [建立 GitHub 帳戶和設定檔](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [將帳戶連結至您的 Microsoft 帳戶和 Microsoft 和 MicrosoftDocs 組織](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [開啟 雙因素驗證](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [授權存取您的 GitHub 帳戶的建置系統](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [安裝 Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [安裝 GitHub 和其工具](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [安裝的 Docs 編寫套件](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>設定您自己的存放庫的版本

您已建立並設定您的 GitHub 帳戶和工具之後，您可以建立個人存放庫的版本。 這是您可以在其中建立您的分支並進行所有變更。

### <a name="fork-the-repository"></a>分支存放庫

您需要原始程式檔中，本機的複本，因此您可以從您的分支建立提取要求，到實際執行存放庫。

#### <a name="to-fork-the-repository"></a>若要將存放庫派生

1. 登入您的 GitHub 帳戶，並移至 https://github.com/microsoftdocs/windowsserverdocs-pr。

2. 選取 **分岔**。

    ![反白顯示在頁面上的 「 分叉 」 按鈕](media/create-new-using-github/fork-button.png)

3. 選取您的 GitHub 帳戶，作為 「 分叉 」 位置。

    ![反白顯示在頁面上的 「 分叉 」 按鈕](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>複製存放庫

您要複製存放庫取得您的本機裝置入存放庫本機複本。

#### <a name="to-clone-the-repository"></a>若要複製存放庫

1. 移至 https://github.com/settings/developers，然後選取**個人存取權杖**從左窗格中。

2. 選取 **產生新的語彙基元**，為您的權杖提供有意義且唯一的名稱、 選取所有可用的範圍，並選取**產生的語彙基元**。

3. 複製權杖，並將它放在安全的地方。 您將需要此程序的其餘部分，您離開此頁面之後，您將無法重新開始著手進行。

4. 開啟 Git Bash 命令，並將目錄變更為您想要用來儲存您的存放庫。 我們建議使用， `C:\users\<your_name>\GitHub`。

5. 輸入下列命令來複製存放庫，然後設定您的遠端分支使用您的特定資訊，一次：

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. 執行此命令，請確定您的遠端已正確設定：

    `git remote -v`

7. 您應該會看到類似下列輸出：

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    如果您的遠端輸出看起來不像這樣，您可以再試一次的第一個執行`git remote remove upstream`。

## <a name="create-a-branch-and-a-new-article"></a>建立分支和新的發行項

請遵循下列步驟來建立發行項。

### <a name="create-a-new-branch-and-a-new-file"></a>建立新的分支和新的檔案

您可以開始著手在您的內容之前，您必須建立新的分支，本機存放庫中。

#### <a name="to-create-a-new-branch-in-git-bash"></a>在 Git Bash 中建立新的分支

- 開啟 Git Bash，並輸入命令 （一次一個）：

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >此外，我們強烈建議您命名您的分支明顯且唯一的項目，您可以稍後再找到它。

    命令完成之後，您就可以在新的分支，並已準備好建立您的新檔案。 您只需要變更至 windowsserverdocs pr 存放庫的 Git Bash 的執行個體每一次。 如果您關閉 Git Bash，您必須再次變更目錄之後您將它開啟。

#### <a name="to-create-a-new-file-in-your-branch"></a>若要在您的分支建立新的檔案

1. 開啟 Windows 檔案總管，移至您的 GitHub 目錄，然後資料夾結構中建立新的文字檔。 您的檔案名稱必須全部小寫且以連字號分隔。 例如，_假設-時-windows-server.md_。

     您也必須從.txt，若要變更副檔名。 md。 在出現錯誤訊息中，選取**是**以使用新的檔案副檔名儲存檔案。

2. 開啟 Visual Studio Code 並移至**檔案**，選取**開啟資料夾**，然後移至 GitHub 位置，您在步驟 1 中建立的檔案。

3. 從**總管**] 窗格中，選取 [建立新的檔案。

4. 將文字加入至頁面上，然後按**檔案** > **儲存**。

### <a name="preview-your-text"></a>預覽您的文字

您將文字新增至您的新檔案之後，您必須預覽您的變更，以確定它們都正確出現。

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

完成您的文章之後，您必須從您的寫入器來取得核准 （允許一些時間） 的發行集。

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
