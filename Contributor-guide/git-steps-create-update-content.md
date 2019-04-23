<properties pageTitle="建立新文章或更新現有文章的 Git 命令" description="建立和更新發行項 WindowsServerDocs pr 中的步驟。" metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy Davies" videoId="" scriptId="" manager="dongill" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="08/24/16" ms.author="kathydav" />

# <a name="git-commands-to-create-or-update-an-article"></a>Git 命令，以建立或更新發行項

>!注意：這些命令假設您已設定 Github 來指定您用來提取來自檔案的預設存放庫。 在 Github 中術語中，您用來提取檔案是您*上游*。 您用來推送檔案是您*原點*。 根據我們的存放庫和工作流程的設計方式，您的上游應該設定為我們的存放庫，也就是在 Microsoft 企業 https://github.com/Microsoft/WindowsServerDocs-pr和您的來源應該是您的分叉，請在此存放庫，在您自己的 Github 帳戶下。 比方說，是 Kathy 的 https://github.com/KBDAzure/WindowsServerDocs-pr 

>若要檢查您的設定，請輸入```git config -l```。 查看 Url，以確定它們參考您想要的位置。

## <a name="add-or-update-an-article"></a>新增或更新發行項

以下是如何建立本機分支，儲存變更，然後將其推送至遠端分叉。

1. 啟動 Git Bash （或您使用適用於 Git 的命令列工具）。

2. 將變更 WindowsServerDocs 提取要求：

        cd WindowsServerDocs-pr

3. 它是讓您本機的主要分支和遠端的主要分支分叉存放庫的主要使用的同步處理中分支。 這有助於節省您許多倍感挫折，遺失時間--你更容易早期揪出問題並保持在良好且已知的工作狀態的項目。 執行：

        git checkout master
        git pull upstream master
        git push origin master

4. 現在您可以開始建立分支，以執行您每日或交付項目為基礎運作。 若要從發行分支建立本機工作分支，最好是用來執行：

        git checkout upstream/upstream-branch-name -b your-local-branch-name

   這會直接從上游分支的本機分支，並且可協助您避免合併了錯誤的檔案至您新的本機分支。 比方說，若要建立工作分支，根據 ga 臨界值分支，您可以執行如下命令：
      
        git checkout upstream/master -b working-11-18

   如果您正在操作應該合併至分支所沒有的內容         

5. 新增至您的分叉的本機工作分支：

        git push origin <working branch>

6. 建立新的發行項，或對現有的發行項的變更。 使用 Windows 檔案總管開啟 markdown 檔案和您的 markdown 編輯器，來建立及編輯檔案。 如需基本格式的說明，請參閱此[文章](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)在 Github 中。

7. 新增必要的中繼資料和版本資訊根據[中繼資料和產品版本](metadata-OSversioning-and-trademarks.md)。

8. 檢查狀態，然後新增並認可您的變更：

        git status

   檢閱結果，以確定列出的檔案是您所變更的項目。 如果所有檔案看都起來是正確的執行下列命令以新增的所有檔案：

        git add .
        git commit –m "<comment>"

   若要將特定的檔案 (例如，如果```git status```列出您不想要提交的檔案)，相反地，您必須執行：

        git add <file path>
        git commit –m "<comment>"

   >[!IMPORTANT]
   >此命令```git add .```將所有暫止的變更報告```git status```。 這表示，如果```git status```會顯示未被追蹤的更新，您不想要新增，請使用```git add <file path>```改。  

9. 從上游的變更更新您的本機工作分支：

        git pull upstream master

10. 將變更推送至您的分叉 GitHub 上：

        git push origin <working branch>

## <a name="submit-your-changes"></a>提交您的變更

當您準備好提交內容以進行暫存、 驗證和/或發行時，使用 GitHub UI 來建立提取要求。 

當您開啟提取要求 (PR) 時，這會觸發測試、 建置專案並將發佈到我們的內部暫存網站。 就可以開啟提取要求，您不可以有合併，因為這是如何取得測試，以及檢查您的預備網站上的更新。 組建詳細資料和預備連結張貼做為註解至項目。 

您必須負責執行下列**您提出來合併變更之前**:
  - 檢閱組建詳細資料，以確定它沒有任何錯誤。 
  - 檢閱您的預備網站上的更新。

您完成此動作之後，請透過下列方式表示：
- 將 「 已準備好-合併 」 標籤新增至項目。 \(按一下 **標籤**或註解中的資料流項目。 右邊的齒輪圖示)
- 加入可供-合併成註解，並將電子郵件傳送至 WSSC 提取檢閱者別名： wssc pra

提取要求檢閱者會檢查您的變更，並接受提取要求，除非有問題或疑問。 意見反應或修正問題的要求會加入做為註解。 檢閱[品質準則的提取要求檢閱](contributor-guide-pr-criteria.md)若要了解預期的。

## <a name="publishing"></a>Publishing

- 文件發佈於大約 10:00 AM 和下午 3:00 星期一至星期五，太平洋時間。 請記住，提取要求檢閱者需要一些時間檢閱並接受您的變更，他們可以之前合併。 必須可供挑選下一個排程的發行週期中合併變更。 如果您需要為特定發行集週期發行的發行項，可讓提取要求檢閱者事先知道。 接受您的 PR 時，您的變更會合併成存放庫，並將會包含在下一次執行的時間發行。 可能需要 30 分鐘的時間之文章發佈上線。 
