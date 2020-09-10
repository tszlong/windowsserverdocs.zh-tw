---
title: extract
description: 可從來源位置解壓縮檔案的 [解壓縮] 命令的參考文章。
ms.topic: reference
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 38d73bb18c210c27859add9a419adedfb4fecac4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627664"
---
# <a name="extract"></a>extract

從封包或來源解壓縮檔案。

## <a name="syntax"></a>語法

```
extract [/y] [/a] [/d | /e] [/l dir] cabinet [filename ...]
extract [/y] source [newname]
extract [/y] /c source destination
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| 內閣 | 如果您想要解壓縮兩個以上的檔案，請使用。 |
| filename | 要從封包解壓縮的檔案名。 萬用字元和多個檔案名 (使用空格分隔，) 可能會用到。 |
| source | 壓縮檔 (只有一個檔案) 的封包。 |
| newname | 要提供解壓縮檔案的新檔案名。 如果未提供，則會使用原始名稱。 |
| /a | 處理所有的封包。 接下來的封包鏈從第一封的第一封包開始。 |
| /C | 將來源檔案複製到目的地 (從 DMF 磁片) 複製。 |
| /d | 顯示 (使用檔案名的封包目錄，以避免解壓縮) 。 |
| /e |  (使用而不是來解壓縮 *。* ) 解壓縮所有檔案。 |
| /l dir | 放置解壓縮檔案的位置 (預設為目前目錄) 。 |
| /y | 覆寫現有檔案之前不要提示。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
