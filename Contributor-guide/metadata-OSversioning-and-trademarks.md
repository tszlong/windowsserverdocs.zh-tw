# <a name="metadata-and-version-identifiers"></a>中繼資料和版本識別碼

以下是您需要了解商標，版本控制和 windowsserverdocs pr 存放庫中的文件的中繼資料。 文章作者有責任確保其發行符合這些標準和需求。

## <a name="trademarks"></a>商標
請勿使用商標符號之後在技術文件庫中的文章中提及的產品。 因為 technet.microsoft.com 9、docs.microsoft.com 及其他發佈管道的官方 Microsoft 包含不需要[商標](https://www.microsoft.com/trademarks)頁尾連結到一份 Microsoft 商標。 該連結，可滿足法規需求。 如背景，請參閱指引[CELA](https://microsoft.sharepoint.com/sites/LCAWeb/Home/Copyrights-Trademarks-and-Patents/Trademarks/Trademark-List-and-Usage)、 「 網站 」 和 「 Microsoft 撰寫樣式指南底下[著作權及商標](https://worldready.cloudapp.net/Styleguide/Read?id=2700&topicid=26696)"網頁上 Microsoft.com"下的頁面， 

## <a name="versioning"></a>版本設定
此存放庫中的文章適用於數種類型的版本控制： 

-  指示文章適用的產品版本的版本控制是兩種方式：
    - 以手動方式在文件，因此它會顯示為已發行之發行項上的文字中的單一行。
    - 由中繼資料，如下所述。
-  來源內容的版本控制是透過 Github 的歷程記錄的檔案處理。 

## <a name="metadata-structure-and-format"></a>中繼資料結構和格式

- 將中繼資料放在上方的 H1 標題的檔案頂端。
- 區塊檔案內容的其餘部分使用分隔只有三個連字號區塊的第一個和最後一個行中。 不要將任何其他文字放在這幾行。
- 將每個中繼資料名稱/值組放置於不同行。
- 根據中繼資料是否需要使用下列語法模式的其中一個預先定義的值，或接受自訂的值。 

        ---
        name1: predefined-value
        name2: "my custom value"
        ---

## <a name="metadata-names-and-values"></a>中繼資料名稱和值

發行為 TechNet 技術文件庫的發行項，而其他中繼資料，建議使用但非必要的所有檔案需要特定的中繼資料。 在某些情況下，只有特定值允許特定的中繼資料。 

|名稱|值|
|---|---|
|title|比對的 H1 標題的自訂值。 決定顯示在瀏覽器索引標籤中。|
|description|在搜尋結果中顯示，並會影響 SEO。 包含適當的關鍵字，但保留為不超過 160 個字元的長度。|
|作者|主要作者的文章的 Github 別名|
|ms.author|文章的作者或內容開發人員負責的文章涵蓋的技術領域的 MSFT 別名。|
|ms.date|上一次的內容更新日期。 不要更新僅限中繼資料的變更。 使用格式為 mm/dd/yyyy。|
|ms.prod|識別 BI 報告，根據預先定義的 Windows Server 版本[值](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)。|
|ms.technology|識別技術領域，BI 報告，根據預先定義[值](https://microsoft.sharepoint.com/teams/STBCSI/Insights/_layouts/15/WopiFrame.aspx?sourcedoc=%7b7A321BF1-0611-4184-84DA-A0E964C435FA%7d&file=WEDCS_MasterList_CSIValues.xlsx&action=default&IsList=1&ListId=%7b46B17C8A-CD7E-47ED-A1B6-F2B654B55E2B%7d&ListItemId=969)|
|ms.asset|在所有地區設定識別的發行項，bi 報告的 GUID。 從先前撰寫的文章移轉系統您尚未擁有此資料庫。 針對在 Github 中建立的發行項會使用這類工具[ https://guidgenerator.com/ ](https://guidgenerator.com/)。| 

## <a name="troubleshooting"></a>疑難排解

發行項的上方顯示的中繼資料會在原始程式檔有格式錯誤時發生。 常見的錯誤包括：

- 遺漏的區塊中或連字號數目錯誤的第一個和最後一個行在三個連字號。
- 中繼資料不是採用所需的語法：\<名稱\>:\<單一空間\>\<值 >

檢查您的檔案，它們與任何其他明顯的錯誤，然後提交 PR 來發佈更新的檔案。 如果您被卡，電子郵件的提取要求檢閱者別名： wssc-pra@microsoft.com

## <a name="see-also"></a>另請參閱
使用此存放庫中的中繼資料根據中繼資料用於內容的服務和國際\(CSI\)。 詳細資訊，包括選擇性的中繼資料，將會位於[ http://aka.ms/skyeye/meta ](http://aka.ms/skyeye/meta)。
Business intelligence 資源，請參閱 CSI BI 團隊[wiki](https://microsoft.sharepoint.com/teams/STBCSI/Insights/Selfserve%20BI%20wiki/Self-serve%20BI%20wiki.aspx)。