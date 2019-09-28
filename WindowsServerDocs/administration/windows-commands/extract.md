---
title: extract
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 967f08e271019cc33970419179c9ddbf902b1882
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377256"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>語法

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|檔案櫃|檔案包含兩個或多個檔案。|
|filename|要從封包檔解壓縮的檔案名。 您可以使用萬用字元和多個檔案名（以空格分隔）。|
|來源|壓縮檔案（只有一個檔案的封包）。|
|newname|要提供解壓縮檔案的新檔案名。 如果未提供，則會使用原始名稱。|
|/A|處理所有的封包。 遵循第一個封包中所述的機櫃鏈。|
|/C|將來源檔案複製到目的地（以從 DMF 磁碟複製）。|
|/D|顯示封包目錄（搭配 filename 使用以避免解壓縮）。|
|/E|解壓縮（使用，而不是 *。* 以解壓縮所有檔案）。|
|/L dir|要放置解壓縮檔案的位置（預設為目前的目錄）。|
|/Y|覆寫現有檔案之前不提示。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)