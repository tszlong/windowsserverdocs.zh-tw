---
title: extract
description: 解壓縮命令的參考文章，它會從來源位置解壓縮檔案。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1de26d444f8c8fdc838fc2fe0c662afefe8c172c
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932381"
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

| 參數 | 說明 |
| --------- | ----------- |
| 檔案櫃 | 如果您想要解壓縮兩個或多個檔案，請使用。 |
| filename | 要從封包檔解壓縮的檔案名。 您可以使用萬用字元和多個檔案名（以空格分隔）。 |
| source | 壓縮檔案（只有一個檔案的封包）。 |
| newname | 要提供解壓縮檔案的新檔案名。 如果未提供，則會使用原始名稱。 |
| /a | 處理所有的封包。 遵循第一個封包中所述的機櫃鏈。 |
| /C | 將來源檔案複製到目的地（以從 DMF 磁碟複製）。 |
| /d | 顯示封包目錄（搭配 filename 使用以避免解壓縮）。 |
| /e | 解壓縮（使用，而不是 *。* 以解壓縮所有檔案）。 |
| /l dir | 要放置解壓縮檔案的位置（預設為目前的目錄）。 |
| /y | 在覆寫現有的檔案之前，不要提示。 |

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
