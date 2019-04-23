# <a name="rename-or-delete-an-article"></a>重新命名或刪除發行項

在您重新命名或刪除發行項之前，您需要遵循此程序，若要避免或減少的中斷連結。 客戶不喜歡中斷的連結，而且不 shy 關於關閉它們時叫用之一的發音。 請注意，重新命名或刪除某篇文章是最後一個步驟，在此程序，並非第一個。


## <a name="step-1-manage-inbound-links"></a>步驟 1：管理輸入的連結

判斷是否有任何非 Microsoft 輸入連結連結到您的內容，例如部落格、 論壇，以及在網站上的其他內容。 請連絡部落格擁有者可以變更這些連結，然後移除或更新來自論壇貼文的連結。 Web 分析工具可協助識別您可能需要以這種方式管理的高流量輸入的連結。

## <a name="step-2-remove-all-crosslinks-to-the-article-from-the-windowsserverdocs-pr-repository"></a>步驟 2：發行項的所有交互連結移除 WindowsServerDocs pr 存放庫

1. 重新整理您的本機分支 – 執行`git pull upstream <branch>`，亦即以從主要重新整理，請執行 `git pull upstream master`

2.  掃描 WindowsServerDocs pr 資料夾，以尋找已連結，以您想要重新命名或淘汰，發行項的發行項，然後更新的文章，以移除連結，或取代另一篇文章的連結。 您可以使用搜尋和取代公用程式來尋找交互連結，如果您沒有安裝。 如果不這麼做，您可以使用 Windows PowerShell:

 a. 啟動 Windows PowerShell。

 b. 在 PowerShell 提示字元中，變更至 WindowsServerDocs pr 資料夾：

 `cd WindowsServerDocs-pr\WindowsServerDocs-pr`

 c.  執行命令，列出所有的檔案，包含要重新命名或刪除的發行項的連結：

 `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name`

  若要將檔案名稱的清單傳送至文字檔 （在此案例中，名為 psoutput.txt），執行：

  `Get-ChildItem -Recurse -Include *.md* | Select-String "<the name of the topic you are deleting>" | group path | select name | Out-File C:\Users\<your account>\psoutput.txt`

3. 新增並認可所有變更、 將其推送至您的分叉，和開啟提取要求。 如需相關指示，請參閱 < [Git 命令，以建立或更新發行項](git-steps-create-update-content.md)。

## <a name="step-3-update-fwlinks"></a>步驟 3：更新 Fwlink

檢查任何發行項所指向的 Fwlink FWLink 工具。 任何 Fwlink 指向取代內容;如果您不是在擁有連結的別名，請將它加入。 如果擁有者不會更新該連結，提出票證與 MSCOM 有變更的連結。 詳細資訊-[內部的 wiki](http://sharepoint/sites/azurecontentguidance/wiki/Pages/Manage%20inbound%20links%20to%20retired%20topics.aspx)。

## <a name="step-4-remove-crosslinks-to-the-article-from-table-of-contents"></a>步驟 4：發行項中移除的目錄的交互連結

使用維護 toc.md 的人員。 此檔案會填入左側的技術文件庫中的內容。 如果您不知道誰連絡，傳送電子郵件至wssc-pra@microsoft.com。

## <a name="step-5-add-redirects"></a>步驟 5：新增重新導向
如果您要重新命名或刪除檔案，將重新導向，如此可不會中斷現有的連結：

1. 將舊的檔案保留在現有的檔案名稱與現有的位置。
2. 取代這段中繼資料檔案中的內容：
   ```
   ---
   redirect_url: <redirection-URL-or-file>
   ---
   ```
   \<重新導向 URL-或-檔案 > 是的不同位置的完整 URL 或路徑 + 檔案名稱，以便在相同的 OPS 存放庫中的其他主題。

   例如-這會是整個檔案：

   ```
   ---
   redirect_url: ../../failover-clustering/whats-new-in-failover-clustering
   ---
   ```

3. 之後建立提取要求重新導向，並按一下 PR-如果重新導向正常運作您的註解中的連結，應該會收到目標主題。

## <a name="step-6-rename-or-delete-the-article"></a>步驟 6：重新命名或刪除文件

如果您未使用重新導向，您已完成上一個步驟之後，執行此步驟中，而且所有受影響的文件會發行。 如果您使用重新導向時，會復原這項重新命名或刪除發行項，並會導致中斷的連結。 將檔案重新命名，只要將它重新命名在檔案系統中，然後新增、 認可和推送變更，然後再開啟提取要求。
若要刪除的檔案，首先需要知道它不能只刪除檔案從檔案系統，因為這是原始檔控制系統，而且它需要知道您要執行這項操作。 否則可能會重新顯示刪除的檔案。
有兩種方式可以執行這項操作：

- 檔案系統與 git:從檔案系統中刪除檔案。 然後從您的 git 工具，執行下列其中一個項目  ```Git add -A``` | ```Git add --all``` | ```Git add -u```
- 只是 git: 執行 ```git rm foo.md```

    進一歩[ http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo ](http://stackoverflow.com/questions/2047465/how-can-i-delete-a-file-from-git-repo)和 [https://git-scm.com/docs/git-rm](https://git-scm.com/docs/git-rm) 

## <a name="step-7-find-and-fix-straggler-broken-links"></a>步驟 7：找出並修正 straggler 中斷的連結

使用內容的 QA 工具來尋找上一個步驟未攔截，然後移除或修正該連結中斷的連結。

## <a name="step-8-remove-cached-pages-from-search-engines"></a>步驟 8：從搜尋引擎移除快取的頁面

請移至這些網頁來從搜尋引擎移除快取的網頁：[Bing](https://www.bing.com/webmaster/tools/content-removal?rflid=1)
[Google](https://www.google.com/webmasters/tools/removals?pli=1)


### <a name="contributors-guide-links"></a>參與者指南的連結

- [指引文章的索引](./contributor-guide-index.md)

