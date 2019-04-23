---
title: extract
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882309"
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
|封包|檔案包含兩個或多個檔案。|
|filename|從封包擷取檔案名稱。 可能使用萬用字元和多個檔案名稱 （以空格分隔）。|
|來源|壓縮的檔案 （其中只包含一個檔案是封包）。|
|newname|已解壓縮的檔案提供給新的檔案名稱。 如果未提供，則會使用原始名稱。|
|/A|處理所有的封包。 以下所述的第一個封包從封包的鏈結。|
|/C|將原始程式檔複製到目的地 （若要從 DMF 磁碟複製）。|
|/D|顯示封包的目錄 （用於以避免擷取的檔案名稱）。|
|/E|擷取 (而不是使用 *。* 若要解壓縮所有檔案）。|
|/L dir|將解壓縮的檔案 （預設為目前的目錄） 的位置。|
|/Y|不提示覆寫現有檔案之前。|

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)