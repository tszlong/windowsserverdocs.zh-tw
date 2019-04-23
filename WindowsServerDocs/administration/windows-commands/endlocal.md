---
title: endlocal
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3e516b2bf9e8a45ada910dfbd93e3ed5e7d86c14
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862139"
---
# <a name="endlocal"></a>endlocal



結束的環境變更批次檔，並還原為其值的環境變數，相對應**setlocal**執行命令。

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

-   **Endlocal**命令之外的指令碼或批次檔中沒有任何作用。
-   沒有隱含**endlocal**命令批次檔的結尾。
-   如果已啟用命令擴充功能 （命令會預設啟用延伸模組）， **endlocal**命令會將命令延伸模組 （也就是啟用或停用） 的狀態還原到之前對應**setlocal**執行命令。

> [!NOTE]
> 如需有關啟用和停用擴充命令的詳細資訊，請參閱 < [Cmd](cmd.md)。

## <a name="BKMK_examples"></a>範例

您可以當地語系化的批次檔中的環境變數。 比方說，下列程式會啟動 superapp 批次程式在網路上，將輸出導向檔案，並在 [記事本] 中顯示的檔案：
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