<properties title="" pageTitle="檔案名稱和 Windows Server 2016 技術文件的位置" description="說明文章和當您建立新的發行項時，您應該遵循的命名慣例的檔案結構。" metaKeywords="" services="" solutions="" documentationCenter="" authors="Kathy-Davies" videoId="" scriptId="" manager="required" />

<tags ms.service="contributor-guide" ms.devlang="" ms.topic="article" ms.tgt_pltfrm="" ms.workload="" ms.date="03/14/2016" ms.author="jimpark; tysonn" />

# <a name="file-names-and-locations-for-windows-server-technical-articles"></a>檔案名稱和 Windows Server 技術文件的位置

了解，並遵循規則，可協助您取得您接受更快的提取要求。

+ [規則]
+ [Pattern]
+ [檔案名稱核准]

## <a name="rules"></a>規則

- 所有檔案必須為 markdown 格式，並使用.md 副檔名。
- 盡可能保持 3 到 5 個字的檔案名稱，-10 字真的是太久的可讀性和 SEO。
- 使用大小寫和字母、 數字和連字號。
- 沒有空格或標點符號字元。 您可以使用連字號來分隔文字和檔案名稱中的數字。
- 使用動作的動詞命令的簡短形式而非 '-ing' 表單：`deploy-nano-server`不 `Deploying-Nano-Server`
- 不使用短字詞，例如 a、 and、，在中，或。
- 拼字字出;避免使用未核准 」 或不需要檔案名稱中的縮寫
- 檔案名稱必須是唯一的-而不是`overview.md`使用 `storage-spaces-overview.md`

縮略字和 initialisms 在檔案名稱的特定指導方針：

- 請遵循現有的 Microsoft 指引，可接受的名稱縮寫
- 業界標準縮寫是視需要在檔案名稱中可接受的。

## <a name="pattern"></a>模式

檢閱文章以了解現有名稱的存放庫中的清單。 以下是一般的模式：

 **component-topic-title.md**
 
例如： `storage-spaces-direct-overview.md` 

## <a name="file-name-approval"></a>檔案名稱核准

當您提交新的檔案時，提取要求檢閱者檢閱的名稱，並提供意見反應透過提取要求註解的資料流，如果需要變更。 檔案名稱必須加以更正，才能接受提取要求。 參與者可以輕鬆地將更新推送至擱置中的提取要求。

## <a name="folders-in-the-repo"></a>存放庫中的資料夾

使用現有的資料夾結構。 不建立資料夾，而不需要取得核准從存放庫系統管理員。如果您認為您需要新的資料夾與他們溝通。

應該有一個簡單的 GitHub 存放庫，並將其相對較一般資料夾結構 –\<區域 >\\\<技術 >-範例 storage\data-重複資料刪除。 這麼做有下列優點：
 - 其實很簡單。
 - 很接近 Azure.com 的運作方式
 - 它可輕鬆快速找到其主題 – 無須翻西找其他尋找位於特定技術的資料夾的寫入器/Pm。
 - 則保持簡短，很適合 SEO 的 URL 和使用者體驗，有時候客戶查看提示的 URL。

## <a name="changing-case-in-file-names"></a>變更檔案名稱中的大小寫

Windows 作業系統不區分大小寫。 如果您需要變更檔案名稱，若要修正的大小寫，最好是變更檔案內容，除非您是在 Linux 或 mac 上 例如: 

  biztalk-administration-and-Development-Task-List-in-BizTalk-Services --> biztalk-services-administration-and-development-task-list

使用`git mv`命令來重新命名檔案：
```
  git mv <WindowsServerDocs/tech-area/subarea/current-file-name.md> <WindowsServerDocs/tech-area/subarea/new-file-name.md>
```

### <a name="contributors-guide-links"></a>參與者指南的連結

- [指引文章的索引](./contributor-guide-index.md)


<!--Anchors-->
[規則]: #rules
[Pattern]: #pattern
[檔案名稱核准]: #file-name-approval
