---
title:
- 文章標題
description: ''
author:
- GITHUB USERNAME
ms.author:
- MICROSOFT ALIAS
ms.date:
- DATE
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: ''
ms.localizationpriority:
- high/medium/low
ms.openlocfilehash: 4f885680426c0bfa55d5f73a7ef0c2143a8dd5a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879559"
---
# <a name="metadata-and-markdown-template"></a>中繼資料與 Markdown 範本

此作業範本包含 Markdown 語法，以及設定中繼資料的指引的範例。 若要取得最佳的您必須檢閱[原始的 Markdown](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D)並[轉譯檢視](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md)。 （原始的 Markdown 顯示中繼資料區塊中，而轉譯後的檢視則否。）

建立 Markdown 檔案時，您應該將此範本複製到新的檔案、 填寫中繼資料所指定以下設定的文章，標題上方的 H1 標題和刪除內容。 在方括號中的大寫字的任何項目需要注意。


## <a name="metadata"></a>中繼資料 

高於的完整中繼資料區塊。 一些重點：

- 您**必須**冒號 （:） 之間有一個空格和中繼資料元素的值。
- 中的值 （例如標題） 的冒號會中斷中繼資料剖析器。 在適當位置，使用冒號的 HTML 編碼`&#58;`(例如`"title: Azure Rights Management&#58; the basics | Azure RMS"`)。
- **標題**:這個標題會出現在搜尋引擎結果中。 
- **作者**:Author 欄位應該包含**的 GitHub 使用者名稱**的作者，而不是別名。
- **ms.prod**， **ms.technology**:使用 「 windows-f6cc-4890"ms.prod （或如果您使用此範本建立的內容適用於 Windows 10 的 w10）。 請連絡您的 CX 連絡人，以取得 ms.technology 值。

## <a name="basic-markdown-gfm-and-special-characters"></a>基本 Markdown、 GFM 和特殊字元

支援所有基本與 GitHub 版 Markdown。 如需這些的詳細資訊，請參閱：

- [基準 Markdown 語法](https://daringfireball.net/projects/markdown/syntax)
- [具備 GitHub 特色的 Markdown (GFM) 文件](https://guides.github.com/features/mastering-markdown)

使用特殊字元，例如 markdown \*， \`，和\#進行格式化。 如果您想要在內容中包含其中一個字元，您必須執行下列其中一種：

- 加上的反斜線 「 逸出 」 該特殊字元之前 (例如\\\*如\*)
- 使用[HTML 實體代碼](http://www.ascii.cl/htmlcodes.htm)字元 (例如\& \#42\;的&#42;)。

## <a name="headings"></a>標題

標題時應使用 atx 樣式，也就是使用 1-6 雜湊字元 （#） 開頭的行來表示標題，對應到 HTML 標題層級 H1 至 H6。 上面使用的是第一個和第二個層級標頭的範例。 

那里**必須**是您的主題，將會顯示為頁面標題中只有一個的第一個層級標題 (H1)。  

第二層標題將會產生頁面上會出現在 「 本文 」 區段，在頁面上的標題底下的目錄。

### <a name="third-level-heading"></a>第三層標題
#### <a name="fourth-level-heading"></a>第四層標題
##### <a name="fifth-level-heading"></a>第五個層級的標題
###### <a name="sixth-level-heading"></a>第六層標題

## <a name="text-styling"></a>文字樣式設定

*斜體* 

**粗體** 

~~刪除線~~

## <a name="links"></a>連結

### <a name="internal-links"></a>內部連結

若要連結到同一個 Markdown 檔案中的標頭，請檢視發行項的來源，尋找標頭的識別碼 (例如`id="blockquote"`)，使用 # + 識別碼進行連結 (比方說， `#blockquote`)。

- 範例：[區塊引述](#blockquote)

若要連結到相同的儲存機制中的 Markdown 檔案，請使用[相對連結](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)，包含".md"結尾的檔案名稱。

- 範例：[祕訣和陷阱](tips-gotchas.md)
- 範例：[工具和參與者的安裝程式](../readme.md)

若要連結至相同的存放庫中的 Markdown 檔案中的標頭，請使用相對連結 + 主題標籤連結。

- 範例：[刪除檔案](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>外部連結

若要連結至外部檔案，請為連結使用的完整 URL。

- 範例：[GitHub](http://www.github.com)

如果 URL 會出現在 Markdown 檔案，它將會轉換為可點選的連結。

- 範例： http://www.github.com

## <a name="lists"></a>清單

### <a name="ordered-lists"></a>已排序的清單

1. 此 
1. 是
1. 根據紅外線強度拍攝灰階 (黑白) 相片或影片的
1. 排序
1. 清單  


#### <a name="ordered-list-with-an-embedded-list"></a>排序清單的內嵌清單

1. 這裡
1. 來自
1. 的
1. 內嵌
    1. 遺漏 Scarlett
    1. 教授梅紅
1. 排序
1. list


### <a name="unordered-lists"></a>未排序的清單

- 此
- 為
- a
- 項目符號
- list


##### <a name="unordered-list-with-an-embedded-list"></a>具有內嵌清單的未排序的清單

- 此 
- 項目符號 
- list
    - Mrs 皮克
    - Mr.綠色
- 包含  
- 其他
    1. 諾芥
    1. Mrs.白皮書
- 列出


## <a name="horizontal-rule"></a>水平尺規

---

## <a name="tables"></a>資料表

在幾乎每個執行個體，使用 MD 設定資料表格式化的。 雖然 HTML 資料表提供更大的彈性我們並 「 不在我們的內容中使用它們。 如果您在文章中有一個 HTML 資料表，我們不會將合併該文章。

| 資料表        | 是           | 非經常性存取  |
| ------------- |:-------------:| -----:|
| 第 3 欄是      | 靠右對齊 | $1600 |
| 第 2 欄是      | 置中對齊      |   $12 |
| 第 1 欄是預設值 | left-aligned     |    $1 |


## <a name="code"></a>程式碼

### <a name="generic-codeblock"></a>泛型的 codeblock

縮排泛型的 codeblock 撰寫程式碼的程式碼四個空格。

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>Codeblocks 具有語言識別碼

使用三個倒引號 (&#96;&#96;&#96;) + 語言識別碼，若要將語言特定的色彩套用程式碼撰寫到程式碼區塊。  以下是 整份[GitHub Flavored Markdown (GFM) 語言識別碼](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs)。

##### <a name="c9839"></a>C&#9839;

```c#
using System;
namespace HelloWorld
{
    class Hello 
    {
        static void Main() 
        {
            Console.WriteLine("Hello World!");

            // Keep the console window open in debug mode.
            Console.WriteLine("Press any key to exit.");
            Console.ReadKey();
        }
    }
}
```
#### <a name="python"></a>Python

```python
friends = ['john', 'pat', 'gary', 'michael']
for i, name in enumerate(friends):
    print "iteration {iteration} is {name}".format(iteration=i, name=name)
```
#### <a name="powershell"></a>PowerShell

```powershell
Clear-Host
$Directory = "C:\Windows\"
$Files = Get-Childitem $Directory -recurse -Include *.log `
-ErrorAction SilentlyContinue
```

### <a name="inline-code"></a>內嵌程式碼

使用反引號 (&#96;) 的`inline code`。

## <a name="blockquotes"></a>區塊引述

> 夭有一千萬多年來，現在持續和桃之可怕聽到有灼灼。 這裡赤道中也會一天就非洲大陸, 上存在戰鬥聯繫過的凶，桃和 victor 尚無法深入解析。 此數目和蓁 land 的小視窗或 swift / 激烈無法蓬勃發展密不可分，或蓁。

## <a name="images"></a>映像

### <a name="static-image"></a>靜態影像

![這是替代文字](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>連結的影像

[![連結影像的替代文字](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

## <a name="alerts"></a>警示

### <a name="note"></a>注意

> [!NOTE]
> 這是附註

### <a name="warning"></a>警告

> [!WARNING]
> 這警告

### <a name="tip"></a>提示

> [!TIP]
> 這是提示

### <a name="important"></a>重要事項

> [!IMPORTANT]
> 這是重要事項

