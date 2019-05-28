---
title: 編輯使用網頁瀏覽器和 GitHub 的現有 Windows Server 文件
description: 如何使用網頁瀏覽器和 GitHub，身為 Microsoft 員工的現有 Windows Server 文件進行快速的編輯。
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624594"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>更新使用網頁瀏覽器和 GitHub 的現有 Windows Server 文件

有兩個不同的位置，我們用來將 Windows Server 技術內容。 其中一個位置為公用 (windowsserverdocs)，另一個是私用 (windowsserverdocs pr)。 您參與的位置來決定您的身分：

- **我並不是 Microsoft 員工。** 為非 Microsoft 員工，您必須參與的公用位置。 如需如何執行該動作的資訊，請參閱[Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)檔案。

- **我是 Microsoft 員工。** 為 Microsoft 員工，您會有根據您要嘗試執行的選項：

    - **建立全新的發行項。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具，分叉和複製 windowsserverdocs pr 存放庫中，設定您的遠端分支，建立發行項，最後建立的核准及發行新的提取要求。 這些指示，請參閱[建立新的 Windows Server 文件使用 GitHub 和 Visual Studio Code](create-new-using-github.md)文章。

    - **對現有的發行項中的大型的變更。** 若要大幅變更現有的發行項，您可以依照[編輯現有的 Windows Server 發行項使用 GitHub 和 Visual Studio Code](edit-existing-using-github.md)文章。

    - **對現有的文章進行次要的變更。** 若要對現有的發行項進行次要變更，您可以依照這篇文章中的指示。

    > [!IMPORTANT]
    > 所有發佈至 docs.microsoft.com 的存放庫皆採用[Microsoft 開放來源的管理辦法](https://opensource.microsoft.com/codeofconduct/)或[.NET Foundation 管理辦法](https://dotnetfoundation.org/code-of-conduct)。 如需詳細資訊，請參閱 <<c0> [ 程式碼的管理辦法常見問題集](https://opensource.microsoft.com/codeofconduct/faq/)。 或連絡[ opencode@microsoft.com ](mailto:opencode@microsoft.com)，或[ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org)有任何問題或意見。
    >
    > 小幅度修正或釐清公用存放庫中的文件和程式碼範例係由其[docs.microsoft.com 使用規定](https://docs.microsoft.com/legal/termsofuse)。 全新或重要的變更會產生註解，在提取要求中，要求您提交線上投稿授權合約 (CLA)，如果您不是 Microsoft 的員工。 您必須先完成線上表單，我們才能檢閱或接受您的提取要求。

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>使用 GitHub 和網頁瀏覽器的現有文章的快速編輯

快速編輯可簡化回報並修正中小錯誤和遺漏文件中的程序。 儘管所有努力，小型的文法和拼字錯誤_請勿_手至我們已發佈的文件。

1. 移至 https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs。

2. 瀏覽至您想要編輯的發行項，然後選取**編輯此檔案** 按鈕。

   ![編輯此檔案 按鈕](media/github-browser-updates/edit-this-file.png)

3. 編輯主題，然後捲動到底部，簡要說明的變更，然後選取**認可變更**。

    ![認可變更，使用標題的資訊](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>提交提取要求

建立您的提取要求之後，您必須核准及發佈提交它。

### <a name="to-submit-your-pull-request"></a>若要提交您的提取要求

1. 在 **開啟提取要求**頁面中，更新您的認可訊息，讓它更適合的項目。 例如:  第一個段落中修正錯字。

2. 請確定，只有認可，而包含您預期要包含的檔案。 此外也請確認提取要求會移至上游存放庫，主要在正確的分支 （通常） 或者 （有時候） 發行分支。

3. 在 **檢閱者**區域中的右窗格中，選取齒輪圖示，，然後輸入_windowsservercontent_。 別名的成員會檢閱您的變更的點上，而且是合併您的提取要求] 或 [新增項目，若要變更合併之前的相關註解。

4. 選取 **建立提取要求**。 新的 PR 會連結至您分叉中的工作分支。 合併提取要求，直到您推送至相同的工作分支，分叉中的任何新認可都會自動納入的項目。 新的標籤新增至您說的提取要求**do not 合併**。 這只表示您的內容仍在進行中和不應被檢閱或推送到即時網站。

5. 當您準備好進行其他人的別名，以檢閱您的內容時，您必須新增文字 **#sign 關閉**的註解。 此註解：

    - 更新您的提取要求中的標籤**do not 合併**要**已準備好要合併**。

    - 可讓別名和寫入器知道您準備好讓您檢閱的內容。

    - 可讓系統管理員知道核准之後，您的內容已準備好移至即時。