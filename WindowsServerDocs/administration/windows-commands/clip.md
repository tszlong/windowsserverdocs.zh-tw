---
title: clip
description: 剪輯命令的參考主題，它會從命令列將命令輸出重新導向至 Windows 剪貼簿。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61c905e3dcce52f3a3d35adeac55fc5df574f664
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82712784"
---
# <a name="clip"></a>clip

從命令列將命令輸出重新導向至 Windows 剪貼簿。 您可以使用此命令，將資料直接複製到任何可以從剪貼簿接收文字的應用程式。 您也可以將此文字輸出貼入其他程式中。

## <a name="syntax"></a>語法

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<command>` | 指定要傳送至 Windows 剪貼簿之輸出的命令。 |
| `<filename>` | 指定您想要傳送至 Windows 剪貼簿之內容的檔案。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要將目前的目錄清單複製到 Windows 剪貼簿，請輸入：

```
dir | clip
```

若要將名為*generic*的程式輸出複製到 Windows 剪貼簿，請輸入：

```
awk -f generic.awk input.txt | clip
```

*若要將稱為 readme.txt*的檔案內容複寫到 Windows 剪貼簿，請輸入：

```
clip < readme.txt
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)