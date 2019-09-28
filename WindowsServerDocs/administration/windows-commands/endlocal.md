---
title: endlocal
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16d2b7b445a2220a10f88f21029948ed10ee96e4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377568"
---
# <a name="endlocal"></a>endlocal



結束批次檔中環境變更的當地語系化，並在執行對應的**setlocal**命令之前，將環境變數還原到其值。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
endlocal
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Endlocal**命令在腳本或批次檔外沒有任何作用。
-   批次檔的結尾處有隱含的**endlocal**命令。
-   如果已啟用命令延伸模組（預設會啟用命令延伸模組）， **endlocal**命令會將命令延伸模組的狀態（也就是啟用或停用）還原為執行對應的**setlocal**命令之前的內容。

> [!NOTE]
> 如需啟用和停用命令延伸的詳細資訊，請參閱[Cmd](cmd.md)。

## <a name="BKMK_examples"></a>典型

您可以將批次檔中的環境變數當地語系化。 例如，下列程式會在網路上啟動 superapp batch 程式、將輸出導向至檔案，然後在 [記事本] 中顯示檔案：
```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)