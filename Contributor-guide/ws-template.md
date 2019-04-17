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
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/22/2018
ms.locfileid: "2081965"
---
# <a name="metadata-and-markdown-template"></a>中繼資料與 Markdown 範本

此作業之範本包含 Markdown 語法如下指導設定中繼資料的範例。 若要取得的最大它，您必須檢視[原始 Markdown](https://raw.githubusercontent.com/Microsoft/WindowsServerDocs-pr/master/Contributor-guide/ws-template.md?token=AG1vEhARRHNLtPgKXP35BGjNZGajKOArks5YLNIwwA%3D%3D)與[呈現的檢視](https://github.com/Microsoft/WindowsServerDocs-pr/blob/master/Contributor-guide/ws-template.md)。 （雖然轉譯的檢視並不支援原始 Markdown 顯示中繼資料區塊中，）。

建立 Markdown 檔案，應將此範本複製到新的檔案、 填寫中繼資料為指定下方組 H1 標題上方的文章，標題和刪除內容。 以方括弧括 CAPS 中的任何項目需要注意。


## <a name="metadata"></a>中繼資料 

完整的中繼資料區塊會上方。 一些重要的附註：

- 您**必須**具備冒號 （:） 和中繼資料元素的值之間有空格。
- 分隔值 （例如標題） 中的自動換行的中繼資料剖析器。 其就地使用 [HTML 編碼的冒號`&#58;`(例如`"title: Azure Rights Management&#58; the basics | Azure RMS"`)。
- **標題**： 此標題出現在搜尋引擎結果。 
- **作者**： [作者] 欄位應該包含未其別名作者**GitHub 使用者名稱**。
- **ms.prod**、 **ms.technology**： 使用 「 windows server 位臨界值"ms.prod （或如果您使用此範本來建立內容的 Windows 10 w10）。 詢問您的 CX 連絡人取得 ms.technology 值。

## <a name="basic-markdown-gfm-and-special-characters"></a>基本 Markdown、 GFM、 和特殊字元

支援所有的基本和 GitHub flavored Markdown。 如需這些詳細資訊，請參閱：

- [[比較基準 Markdown 語法](https://daringfireball.net/projects/markdown/syntax)
- [GitHub flavored Markdown (GFM) 文件](https://guides.github.com/features/mastering-markdown)

Markdown 例如使用特殊字元 \ *、 \'，及 \ # 進行格式化。 如果您想要在您的內容中包含其中一個這些字元，您必須執行兩件事的其中一個動作：

- 放到"逸出"它的特殊字元前面反斜線 (例如 \\\ * 的 \ *)
- 使用[HTML 實體程式碼](http://www.ascii.cl/htmlcodes.htm)的字元 (例如 \ & \#42\ ； 如 & #42 ；)。

## <a name="headings"></a>標題

標題應使用 atx 樣式，也就是使用 1-6 雜湊字元 （#） 一行的開頭表示對應至 HTML 標題階層 H1 到 H6 的標題。 第一個及第二層標頭的範例會使用上方。 

那里**必須**是您的主題，將會顯示為頁面上標題中只有一的第一層標題 (H1)。  

第二層標題將會產生顯示在 「 文中所"] 區段下方] 頁面上標題] 頁面上 TOC。

### <a name="third-level-heading"></a>第 3 級標題
#### <a name="fourth-level-heading"></a>第四層級標題
##### <a name="fifth-level-heading"></a>第五個層級的標題
###### <a name="sixth-level-heading"></a>第六個層級標題

## <a name="text-styling"></a>文字樣式

*斜體* 

**粗體** 

~~刪除線~~

## <a name="links"></a>連結

### <a name="internal-links"></a>內部連結

要連結至相同的 Markdown 檔案中的標頭，檢視已發佈的文件的來源、 尋找標題的識別碼 (例如`id="blockquote"`)，並將連結使用 # + 識別碼 (例如`#blockquote`)。

- 範例： [Blockquotes](#blockquote)

若要連結至相同的 repo Markdown 檔案，請使用[相對的連結](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2)，包括".md"結尾的檔案名稱。

- 範例：[秘訣與問題](tips-gotchas.md)
- 範例：[工具和安裝程式參與者](../readme.md)

若要連結至在相同的 repo Markdown 檔案中的標頭，請使用相對連結 + 湊標記連結。

- 範例：[刪除檔案](tips-gotchas.md#deleting-files)

### <a name="external-links"></a>外部連結

若要連結至外部檔案，請使用完整的 URL 為] 連結。

- 範例： [GitHub](http://www.github.com)

如果 URL 會出現在 Markdown 檔案，它會轉換為可點選的連結。

- 範例：http://www.github.com

## <a name="lists"></a>清單

### <a name="ordered-lists"></a>排序的清單

1. 這 
1. 是
1. 為
1. 排序
1. 清單  


#### <a name="ordered-list-with-an-embedded-list"></a>排序的清單與內嵌的清單

1. 以下是
1. 而言
1. 為
1. 內嵌
    1. 遺漏 Scarlett
    1. 教授梅
1. 排序
1. list


### <a name="unordered-lists"></a>未排序的清單

- 這
-  為 
- a
- 項目符號
- list


##### <a name="unordered-list-with-an-embedded-list"></a>未排序的清單與內嵌的清單

- 這 
- 項目符號 
- list
    - Mrs 孔雀
    - 先生綠色
- 包含  
- 其他
    1. 諾芥
    1. Mrs 白皮書
- 清單


## <a name="horizontal-rule"></a>水平的規則

---

## <a name="tables"></a>表格

在幾乎每個執行個體，請使用 MD 表格格式設定。 雖然 HTML 表格提供更多彈性我們不執行我們內容中使用它們。 如果您在本文中有的 HTML 表格，我們不會合併的文章。

| 表格        | 是           | 冷卻  |
| ------------- |:-------------:| -----:|
| col 3 是      | 靠右對齊 | $ 1600 |
| col 2 是      | 置中對齊      |   $12 |
| col 1 是預設值 | 靠左對齊     |    $ 1 |


## <a name="code"></a>代碼

### <a name="generic-codeblock"></a>一般 codeblock

縮排一般 codeblock 編寫程式碼四空格。

    function fancyAlert(arg) {
      if(arg) {
        $.docs({div:'#foo'})
      }
    }


### <a name="codeblocks-with-language-identifier"></a>語言識別碼與 Codeblocks

使用三個 backticks (& #96; & #96; & #96;) + 語言特有的色彩套用的語言識別碼編碼至程式碼區塊。  以下是整個[GitHub Flavored Markdown (GFM) 語言識別碼](https://github.com/jmm/gfm-lang-ids/wiki/GitHub-Flavored-Markdown-(GFM)-language-IDs)清單。

##### <a name="c9839"></a>C & #9839;

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

使用 backticks (& #96;) 的`inline code`。

## <a name="blockquotes"></a>Blockquotes

> 乾旱鎖現在一千萬年度的持續和可怕聽到的天皇曆有時間之後結束。 以下上赤道，其會一天稱為 「 非洲、 大陸中存在戰役鎖達到凶、 新 climax 和維多不尚未目視。 在此數目和枯竭園地、 僅限小型或 swift 激烈無法善，或甚至是希望倖免於。

## <a name="images"></a>Images

### <a name="static-image"></a>靜態圖像

![這是替代文字](../windowsserverdocs/get-started/media/wsbanner.png)

### <a name="linked-image"></a>連結的圖像

[![a連結的圖像 lt 文字](../windowsserverdocs/get-started/nano.png)](../windowsserverdocs/get-started/getting-started-with-nano-server.md) 

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

### <a name="important"></a>重要

> [!IMPORTANT]
> 這是重要

