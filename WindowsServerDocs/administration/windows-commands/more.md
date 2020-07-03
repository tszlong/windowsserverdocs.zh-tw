---
title: 等等
description: '[更多] 命令的參考文章，一次會顯示一個輸出畫面。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: fec0ffbd7f2ce5d1efe1953cb4ab283d33f06ec8
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935736"
---
# <a name="more"></a>等等

一次顯示一個輸出畫面。

> [!NOTE]
> 您也可以從修復主控台中，使用具有不同參數的**更多**命令。

## <a name="syntax"></a>語法

```
<command> | more [/c] [/p] [/s] [/t<n>] [+<n>]
more [[/c] [/p] [/s] [/t<n>] [+<n>]] < [<drive>:][<path>]<filename>
more [/c] [/p] [/s] [/t<n>] [+<n>] [<files>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<command>` | 指定您想要顯示輸出的命令。 |
| /C | 在顯示頁面之前清除畫面。 |
| /p | 展開表單換頁字元。 |
| /s | 將多個空白行顯示成一行。 |
| 一起`<n>` | 將索引標籤顯示為*n*所指定的空間數目。 |
| +`<n>` | 顯示第一個檔案，從*n*所指定的行開始。 |
| `[<drive>:][<path>]<filename>` | 指定要顯示之檔案的位置和名稱。 |
| `<files>` | 指定要顯示的檔案清單。 檔案必須使用空格分隔。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 在**更多**提示（）中可接受下列子命令 `-- More --` ，包括：

    | Key | 動作 |
    | --- | ------ |
    | 空白鍵 | 按**空格鍵**以顯示下一個畫面。 |
    | ENTER | 按**enter**鍵，一次顯示一行檔案。 |
    | f | 按**F**以顯示命令列上列出的下一個檔案。 |
    | q | 按**Q**結束 [**更多**] 命令。 |
    | = | 顯示行號。 |
    | p&id`<n>` | 按**P**以顯示接下來的*n*行。 |
    | 今日`<n>` | 按**S**以略過接下來的*n*行。 |
    | ? | 按下 **？** 以顯示**更多**提示中可用的命令。|

- 如果您使用重新導向字元（ `<` ），您也必須指定檔案名做為來源。

- 如果您使用管道（ `|` ），您可以使用**目錄**、**排序**和**類型**之類的命令。

### <a name="examples"></a>範例

若要查看名為 [*用戶端*] 之檔案的第一個畫面，請輸入下列其中一個命令：

```
more < clients.new
type clients.new | more
```

[**更多**] 命令會顯示用戶端的第一個畫面資訊。新增，您可以按空格鍵查看下一個畫面的資訊。

若要清除畫面並移除所有額外的空白行，然後再顯示檔案*用戶端。新*的，請輸入下列其中一個命令：

```
more /c /s < clients.new
type clients.new | more /c /s
```

若要顯示目前的行號，**請輸入：**

```
more =
```

目前的行號已加入至**更多**提示，如下所示：`-- More [Line: 24] --`

若要在出現**更多**提示時顯示特定的行數，請輸入：

```
more p
```

[**更多**] 提示會詢問您要顯示的行數，如下所示： `-- More -- Lines:` 。 輸入要顯示的行數，然後按 ENTER 鍵。 畫面會變更為只顯示該數目的行數。

若要在**更多**提示字元中略過特定的行數，請輸入：

```
more s
```

[**更多**] 提示會詢問您要略過的行數，如下所示： `-- More -- Lines:` 。 輸入要略過的行數，然後按 ENTER 鍵。 畫面會變更，以顯示這些線條會略過。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 修復環境（WinRE）](https://docs.microsoft.com/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
