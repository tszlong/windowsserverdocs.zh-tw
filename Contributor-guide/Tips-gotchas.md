# <a name="tips-and-gotchas"></a>祕訣和錯誤

如果您還不熟悉 Git，學習如何有效地使用很令人沮喪。 因此很容易找到比，如果它已納入本指南中的其他資訊時，會在此收集資訊。

## <a name="basics"></a>基本概念：

這個參與者的指南中的命令假設您已設定 Github 來指定您用來提取來自檔案的預設存放庫。 在 Github 中術語中，您用來提取檔案是您*上游*。 您用來推送檔案是您*原點*。 

因為在我們的存放庫和工作流程的設計方式，您的上游應該設定為我們的存放庫，也就是在 Microsoft 企業 https://github.com/Microsoft/WindowsServerDocs-pr，而且您的來源應該是您自己的 Github 帳戶下此存放庫的分叉。 例如： https://github.com/MY-GITHUB-ALIAS/WindowsServerDocs-pr  

>若要檢查您的設定，請輸入```git config -l```。 查看 Url，以確定它們參考您想要的位置。

基本的分支秘訣：

-  不在本機複本的主要分支中的工作。 這可讓您更輕鬆地從您的分支中的問題中復原並回到良好且已知點。

-  您的本機分支的基礎共用的存放庫，不是在遠端分叉中的分支。 然後，您會將您的本機分支推送至遠端分叉中。 從您的遠端分支，您會要求已合併至共用的存放庫分叉中的更新。 這篇文章中的命令會顯示如何執行這項操作。

-  當您建立本機分支時，您的命令會告訴 Git 您想要將新分支的哪些分支。 當在以及如何指定主機或里程碑或功能分支。 比方說，此命令從上游分支建立新的分支，並讓您準備好要使用它，然後檢查出新的分支：

        git checkout upstream/upstream-branch-name -b your-local-branch-name

Markdown 的說明，請啟動與這個[文章](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)在 Github 中。 對於資料表，請參閱此[一個](https://help.github.com/articles/organizing-information-with-tables/)。

## <a name="gotchas"></a>錯誤

### <a name="deleting-files"></a>刪除檔案
它無法運作，只會從檔案系統刪除檔案。 使用 Git 命令，以讓系統知道您要執行這項操作，否則已刪除的檔案可能會再次出現的殭屍檔案。 而永遠不會在的好時機。 畢竟，這是原始檔控制系統，而且如果您不要告訴它您真的想要移除這些檔案，它將有助於將它們新增為您。 謝謝您，Git ！ 如需相關指示，請參閱 <<c0> [ 重新命名或停用文章](rename-r-retire.md)。

### <a name="merge-conflicts"></a>合併衝突

如果您在本機使用時，才能取得合併衝突，您需要修正此問題，或取消它。 大部分的時間最好只是修正問題，除非它們並非您的檔案。 請注意如果您不執行任何動作，您可能會被阻止將變更推送至您的來源，因此您必須做為項目。

**如何修正**-Azure 指引有更多的資訊，請參閱和指示來解決衝突。 **合併衝突的提取要求應該永遠不會接受**。 請務必取代我們的存放庫和分支，所有的範例中的名稱，當您檢閱[指示](https://microsoft.sharepoint.com/teams/azurecontentguidance/wiki/Pages/Resolve%20a%20local%20merge%20conflict%20yourself.aspx)。 

**如何恢復**--如果衝突是在檔案中不屬於您，或沒有的理由，您無法加以修正時，這裡的如何恢復。這會重設您的本機檔案一組回如何之前發生合併衝突。 執行此命令︰

```git merge --abort```

如果您還沒有接移和認可您的本機工作，您可以這樣現在。 但是，如果您將這些變更推送，並開啟 PR，衝突會可能會重新出現如果它是在變更的檔案。 您必須修正此問題，或從其他人來解決問題，可以接受要求之前取得協助。 因此，最好是只使用方法來自行解除鎖定有緊急工作，例如重大更新。

