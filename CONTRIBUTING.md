# <a name="contributing-to-windows-server-technical-documentation"></a>提供給 Windows Server 技術文件

感謝您對 Windows Server 技術文件的興趣 ！ 我們非常感謝您對我們文件的意見、編輯和新增。有兩個不同的位置，我們用來將 Windows Server 技術內容。 其中一個位置為公用 (windowsserverdocs)，另一個是私用 (windowsserverdocs pr)。 您參與的位置來決定您的身分：

- **我並不是 Microsoft 員工。** 為非 Microsoft 員工，您必須參與的公用位置。 如需如何執行該動作的資訊，請繼續閱讀這篇文章。

- **我是 Microsoft 員工。** 為 Microsoft 員工，您會有根據您要嘗試執行的選項：

    - **建立全新的發行項。** 若要建立全新的文章，您必須建立並設定您的 GitHub 帳戶和工具，分叉和複製 windowsserverdocs pr 存放庫中，設定您的遠端分支，建立發行項，最後建立的核准及發行新的提取要求。 這些指示，請參閱[建立新的 Windows Server 文件使用 GitHub 和 Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md)文章。

    - **對現有的發行項中的大型的變更。** 若要大幅變更現有的發行項，您可以依照[編輯現有的 Windows Server 發行項使用 GitHub 和 Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md)文章。

    - **對現有的文章進行次要的變更。** 若要讓現有的發行項微幅的變更，您可以依照中的指示[更新現有的 Windows Server 發行項使用網頁瀏覽器和 GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md)文章。

## <a name="sign-a-cla"></a>簽署 CLA

所有參與者***未***Microsoft 員工必須[簽署 Microsoft 貢獻授權合約 (CLA)](https://cla.microsoft.com/)編輯任何 Microsoft 存放庫之前。 如果您已經了在 Microsoft 存放庫內編輯，在過去，恭喜您 ！
您已經完成此步驟。

## <a name="editing-topics"></a>編輯主題

我們試著把編輯現有的公用檔案越簡單越好。

### <a name="to-edit-a-topic"></a>若要編輯的主題

1. 移至頁面 https://docs.microsoft.com/windows-server想要更新，然後按**編輯**。

    ![GitHub 的網頁，顯示 [編輯] 連結](media/contribute-link.png)

2. 登入 （或註冊） 的 GitHub 帳戶。

    您必須有 GitHub 帳戶才能進入可讓您編輯某個主題的頁面。

3. 選取 **鉛筆**（以紅色方塊） 圖示來編輯內容。

    ![GitHub Web，紅色方塊中顯示的鉛筆圖示](media/pencil-icon.png)

4. 使用 Markdown 語言，主題進行變更。 如需如何編輯資訊內容使用 Markdown，請參閱：

    - **如果您要連結至 GitHub 中的 Microsoft 組織：** [Windows Server 參與者的指南](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **如果您是 Microsoft 的外部項目：** [精通 Markdown](https://guides.github.com/features/mastering-markdown/)

5. 建議變更，然後按**預覽變更**以確定它看起來正確無誤。

    ![GitHub 的網頁，顯示 [預覽變更] 索引標籤](media/preview-changes.png)

6. 當您完成編輯主題，請捲動到頁面底部，輸入您的分叉的描述性名稱，然後按一下**建議檔案變更**您個人的 GitHub 帳戶中建立分支。

    ![GitHub 的網頁，顯示 [提議檔案變更] 按鈕](media/propose-file-change.png)

    **比較變更**查看有哪些變更您的分叉和原始內容之間出現的畫面。

7. 在 [**比較變更**] 畫面中，您會看到是否有任何問題，與您正在簽入檔案。

    如果不有任何問題，您會看到訊息：**能夠合併**。

    ![GitHub Web 顯示比較變更畫面](media/compare-changes.png)

8. 選取 **建立提取要求**。

    提取要求可讓您告訴其他人已推送至儲存機制中分支 GitHub 的變更。 開啟提取要求後，您可以討論和檢閱與共同作業者可能變更並新增待處理的認可，您的變更合併至基底分支之前。 如需詳細資訊，請參閱[需提取要求](https://help.github.com/articles/about-pull-requests)

9. 輸入標題和描述給核准者的相關功能的要求適當的內容。

10. 捲動到底部的頁面上，確定只有您已變更的檔案位於此提取要求。 否則，您可能會覆寫的其他人的變更。

11. 選取 **建立提取要求**以實際送出提取要求。

    提取要求會傳送至主題的寫入器，會檢閱您的編輯。 如果接受您的要求，則會發佈您的更新。

## <a name="resources"></a>資源

- 您可以使用慣用的文字編輯器來編輯 Markdown。 我們建議[Visual Studio Code](https://code.visualstudio.com/)，從 Microsoft 免費的輕量級開放原始碼編輯器。