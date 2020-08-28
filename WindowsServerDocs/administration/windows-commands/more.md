---
title: 等等
description: 更多命令的參考文章，此命令會一次顯示一個畫面的輸出。
ms.topic: reference
ms.assetid: ded14f6a-d82f-4aeb-a2d8-7ec1c94dfb8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/26/2019
ms.openlocfilehash: 2c8d7a21220701bf46685d4c87ca02a4810aff1b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89038839"
---
# <a name="more"></a>等等

一次顯示輸出的一個畫面。

> [!NOTE]
> 使用不同參數的命令愈 **多** ，也可以從 [修復主控台] 取得。

## <a name="syntax"></a>語法

```
<command> | more [/c] [/p] [/s] [/t<n>] [+<n>]
more [[/c] [/p] [/s] [/t<n>] [+<n>]] < [<drive>:][<path>]<filename>
more [/c] [/p] [/s] [/t<n>] [+<n>] [<files>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<command>` | 指定要顯示輸出的命令。 |
| /C | 在顯示頁面之前清除畫面。 |
| /p | 展開表單摘要字元。 |
| /s | 將多個空白行顯示為單一空白行。 |
| 一起`<n>` | 將索引標籤顯示為 *n*指定的空格數目。 |
| +`<n>` | 顯示第一個檔案，從 *n*指定的行開始。 |
| `[<drive>:][<path>]<filename>` | 指定要顯示之檔案的位置和名稱。 |
| `<files>` | 指定要顯示的檔案清單。 檔案必須使用空格分隔。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 下列子命令會在 **更多** 的提示字元中接受 (`-- More --`) ，包括：

    | Key | 動作 |
    | --- | ------ |
    | 空白鍵 | 按 **空格鍵** 以顯示下一個畫面。 |
    | ENTER | 按 **enter** 鍵，一次顯示一行檔案。 |
    | f | 按 **F** 鍵以顯示命令列上所列的下一個檔案。 |
    | q | 按 **Q** 鍵以結束 [ **其他** ] 命令。 |
    | = | 顯示行號。 |
    | P `<n>` | 按下 **P** 以顯示下 *n* 行。 |
    | ！ `<n>` | 按 **S** 跳過下一個 *n* 行。 |
    | ? | 按 **？** 以顯示 **更多** 提示字元中可用的命令。|

- 如果您使用 () 的重新導向字元 `<` ，您也必須指定檔案名作為來源。

- 如果您使用管道 (`|`) ，您可以使用像是 **dir**、 **sort**和 **type**等命令。

### <a name="examples"></a>範例

若要查看名為「 *用戶端*」之檔案的第一個畫面，請輸入下列其中一個命令：

```
more < clients.new
type clients.new | more
```

[ **更多** ] 命令會顯示用戶端資訊的第一個畫面，您可以按下空格鍵來查看下一個畫面的資訊。

若要在顯示檔 *用戶端*之前清除畫面並移除所有多餘的空白行，請輸入下列其中一個命令：

```
more /c /s < clients.new
type clients.new | more /c /s
```

若要在 **其他** 提示字元中顯示目前的行號，請輸入：

```
more =
```

目前的行號會新增至 [ **更多** ] 提示字元，如下所示： `-- More [Line: 24] --`

若要在 **其他** 提示字元中顯示特定數目的行，請輸入：

```
more p
```

出現 **的提示會** 詢問您要顯示的行數，如下所示： `-- More -- Lines:` 。 輸入要顯示的行數，然後按 ENTER 鍵。 畫面會變更為只顯示該行的行數。

若要在 **其他** 提示字元略過特定數目的行，請輸入：

```
more s
```

出現 **的提示會** 詢問您要跳過的行數，如下所示： `-- More -- Lines:` 。 輸入要略過的行數，然後按 ENTER 鍵。 畫面會變更，顯示這些線條已略過。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [Windows 修復環境 (WinRE) ](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference)
