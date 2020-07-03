---
title: 編輯
description: '[編輯] 命令的參考文章，它會啟動 MS-DOS 編輯器，讓您可以建立和變更 ASCII 文字檔。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a28af13c5f627010dce1321027b8a246560829f1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930496"
---
# <a name="edit"></a>編輯

啟動 MS-DOS 編輯器，它會建立及變更 ASCII 文字檔。

## <a name="syntax"></a>語法

```
edit [/b] [/h] [/r] [/s] [/<nnn>] [[<drive>:][<path>]<filename> [<filename2> [...]]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `[<drive>:][<path>]<filename> [<filename2> [...]]` | 指定一或多個 ASCII 文字檔的位置和名稱。 如果檔案不存在，MS-DOS 編輯器會建立檔案。 如果檔案已存在，MS-DOS 編輯器會開啟該檔案，並在螢幕上顯示其內容。 *Filename*選項可以包含萬用字元（**&#42;** 和 **？**）。 以空格分隔多個檔案名。 |
| /b | 強制執行單色模式，讓 MS-DOS 編輯器以黑色和白色顯示。 |
| /h | 顯示目前監視器可能的最大行數。 |
| /r | 以唯讀模式載入檔案。 |
| /s | 強制使用短檔案名。 |
| `<nnn>` | 載入二進位檔案，將線條換行至*nnn*個字元寬。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 如需其他說明，請開啟 [MS-DOS 編輯器]，然後按下 F1 鍵。

- 有些監視器預設不支援顯示快速鍵。 如果您的監視器未顯示快捷方式鍵，請使用 **/b**。

### <a name="examples"></a>範例

若要開啟 MS-DOS 編輯器，請輸入：

```
edit
```

若要建立和編輯目前目錄中名為*newtextfile.txt*的檔案，請輸入：

```
edit newtextfile.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
