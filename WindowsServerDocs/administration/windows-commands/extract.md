---
title: extract
description: 解壓縮命令的參考文章，它會從來源位置解壓縮檔案。
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 130f9ee22e0603deaa50dfde267df1c39342e38c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890375"
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
| 檔案櫃 | 如果您想要解壓縮兩個或多個檔案，請使用。 |
| filename | 要從封包檔解壓縮的檔案名。 萬用字元和多個檔案名 (以空格分隔，) 可以使用。 |
| source | 壓縮檔 (只有一個檔案) 的封包。 |
| newname | 要提供解壓縮檔案的新檔案名。 如果未提供，則會使用原始名稱。 |
| /a | 處理所有的封包。 遵循第一個封包中所述的機櫃鏈。 |
| /C | 將來源檔案複製到目的地 (從 DMF 磁碟複製) 。 |
| /d | 顯示封包目錄 (搭配 filename 使用，以避免解壓縮) 。 |
| /e | 解壓縮 (使用，而不是 *。* ) 解壓縮所有檔案。 |
| /l dir | 要放置解壓縮檔案的位置 (預設為目前目錄) 。 |
| /y | 在覆寫現有的檔案之前，不要提示。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
