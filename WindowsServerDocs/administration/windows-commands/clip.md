---
title: clip
description: 剪輯命令的參考文章，此命令會將命令列的命令輸出重新導向至 Windows 剪貼簿。
ms.topic: reference
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9f6820d60a8c94ed07bbb23bbbd86be3f1f289b7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629620"
---
# <a name="clip"></a>clip

從命令列將命令輸出重新導向至 Windows 剪貼簿。 您可以使用此命令，將資料直接複製到可從剪貼簿接收文字的任何應用程式。 您也可以將此文字輸出貼到其他程式中。

## <a name="syntax"></a>語法

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<command>` | 指定您想要傳送至 Windows 剪貼簿之輸出的命令。 |
| `<filename>` | 指定您想要傳送至 Windows 剪貼簿之內容的檔案。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將目前目錄清單複製到 Windows 剪貼簿，請輸入：

```
dir | clip
```

若要將稱為 *generic* 的程式輸出複製到 Windows 剪貼簿，請輸入：

```
awk -f generic.awk input.txt | clip
```

若要將稱為 *readme.txt* 的檔案內容複寫到 Windows 剪貼簿，請輸入：

```
clip < readme.txt
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)