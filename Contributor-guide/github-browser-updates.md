---
title: 使用網頁瀏覽器和 GitHub 編輯現有的 Windows Server 文章
description: 如何使用網頁瀏覽器和 GitHub 以 Microsoft 員工的身分，對現有的 Windows Server 檔進行快速編輯。
author: eross-msft
ms.author: lizross
ms.date: 07/02/2020
ms.openlocfilehash: 61ceb05b6cb9602faaa4d17b4dc2d978cb571ddb
ms.sourcegitcommit: d754f9e39fc28e4c8768b77bcc9d02ffe2fa6535
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85911227"
---
# <a name="update-existing-windows-server-and-azure-stack-hci-articles-using-a-web-browser-and-github"></a>使用網頁瀏覽器和 GitHub 更新現有的 Windows Server 和 Azure Stack HCI 文章

我們提供兩個不同的位置，讓我們保留 Windows Server 技術內容。 其中一個位置是公用（windowsserverdocs），另一個則是私用（windowsserverdocs pr）。 決定您要參與的位置：

- **我不是 Microsoft 員工。** 身為非 Microsoft 員工，您必須參與公用位置。 如需如何執行此動作的詳細資訊，請參閱[Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md)檔案。

- **我是 Microsoft 員工。** 身為 Microsoft 員工，您可以根據嘗試執行的動作來選擇：

    - **建立全新的文章。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具、派生和複製 windowsserverdocs pr 存放庫、設定遠端分支、建立發行項，最後建立新的提取要求以供核准和發行。 如需這些指示，請參閱[使用 GitHub 建立新的 Windows Server 文章和 Visual Studio Code](create-new-using-github.md)一文。

    - **對現有的文章進行大量變更。** 若要對現有的文章進行重大變更，您可以遵循[使用 GitHub 編輯現有的 Windows Server 文章和 Visual Studio Code](edit-existing-using-github.md)一文中的指示。

    - **對現有的文章進行次要變更。** 若要對現有的文章進行次要變更，請遵循這篇文章中的指示。

    > [!IMPORTANT]
    > 所有發佈至 docs.microsoft.com 的存放庫皆採用 [Microsoft 開放原始碼管理辦法](https://opensource.microsoft.com/codeofconduct/) \(英文\) 或 [.NET Foundation 管理辦法](https://dotnetfoundation.org/code-of-conduct) \(英文\)。 如需詳細資訊，請參閱[管理辦法常見問題集](https://opensource.microsoft.com/codeofconduct/faq/) \(英文\)。 若有任何問題或意見，也可以連絡 [opencode@microsoft.com](mailto:opencode@microsoft.com) 或 [conduct@dotnetfoundation.org](mailto:conduct@dotnetfoundation.org)。
    >
    > 您在公用存放庫中針對文件和程式碼範例所提交的次要修正或釐清，將受到 [docs.microsoft.com 使用條款](https://docs.microsoft.com/legal/termsofuse)的約束。 如果您不是 Microsoft 的員工，在做出全新或大規模的變更時，系統會在提取要求中產生意見，要求您提交線上版的貢獻授權合約 (CLA)。 您必須先完成填寫線上表單，我們才會檢閱或接受您的提取要求。

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>使用 GitHub 和網頁瀏覽器快速編輯現有文章

快速編輯可簡化回報並修正文件中小錯誤和遺漏的流程。 即便我們非常努力，發佈的文件中仍不免存在少許文法和拼字錯誤。

1. 依照[GitHub 帳戶設定](https://review.docs.microsoft.com/en-us/help/contribute/contribute-get-started-setup-github?branch=master)中的指示進行。

1. 移至[Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs)或[Azure Stack HCI](https://github.com/MicrosoftDocs/azure-stack-docs-pr/tree/master/azure-stack/hci)私人存放庫。 私用存放庫的監視頻率較高，因此我們的核准時間較快，其受益于增加品質檢查，並提供在預備環境中顯示內容的能力，因為它會出現在即時網站上。

2. 流覽至您想要編輯的文章，然後選取 [**編輯此**檔案] 按鈕。

   ![編輯此檔案按鈕](media/github-browser-updates/edit-this-file.png)

3. 編輯主題，然後向下滾動到底部，簡要描述變更，然後選取 [**認可變更**]。

    ![認可具有標題資訊的變更](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>提交提取要求

建立提取要求之後，您必須提交它以供核准和發行。

### <a name="to-submit-your-pull-request"></a>提交您的提取要求

1. 在 [**開啟提取要求**] 頁面上，更新您的認可訊息，使其更適合用於 PR。 例如：修正第一個段落中的打字錯誤。

2. 請確定只包含您預期包含的認可和檔案。 此外，請檢查 PR 是否進入上游存放庫中的正確分支，主要（通常是）或發行分支（偶爾）。

3. 在右窗格的 [**審核者**] 區域中，選取齒輪圖示，然後輸入_windowsservercontent_。 別名的成員將會在檢查您所做的變更時，合併您的提取要求，或在合併之前新增要變更之專案的批註。

4. 選取 [建立提取要求]。 新的 PR 會連結至您分叉中的工作分支。 在合併 PR 之前，您推送至分叉中相同工作分支的任何新認可都會自動包含在 PR 中。 系統會將新的標籤新增至您的提取要求，並顯示 [**不合並**]。 這只是表示您的內容仍在進行中，而且不應該檢查或推送到即時網站。

5. 當您準備好讓別名中的某人審查您的內容時，您必須將文字新增至批註 **#sign** 。 此批註：

    - 將提取要求的標籤從 [**不要合併**] 更新為 [**準備合併**]。

    - 讓別名和寫入器知道您已經準備好要審核您的內容。

    - 讓系統管理員知道在核准之後，您的內容已準備好上線。
