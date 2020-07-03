---
title: endlocal
description: Endlocal 命令的參考文章，會在批次檔中結束環境變更的當地語系化，並在執行對應的 setlocal 命令之前，將環境變數還原到其值。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a17ef4b25a0b0bb4d77068aa3bff3d879955aec5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932125"
---
# <a name="endlocal"></a>endlocal

結束批次檔中環境變更的當地語系化，並在執行對應的**setlocal**命令之前，將環境變數還原到其值。

## <a name="syntax"></a>語法

```
endlocal
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Endlocal**命令在腳本或批次檔外沒有任何作用。

- 批次檔的結尾處有隱含的**endlocal**命令。

- 如果已啟用命令延伸模組（預設會啟用命令延伸模組）， **endlocal**命令會將命令延伸模組的狀態（也就是啟用或停用）還原為執行對應的**setlocal**命令之前的內容。

> [!NOTE]
> 如需有關啟用和停用命令延伸模組的詳細資訊，請參閱[Cmd 命令](cmd.md)。

### <a name="examples"></a>範例

您可以將批次檔中的環境變數當地語系化。 例如，下列程式會在網路上啟動*superapp* batch 程式、將輸出導向至檔案，然後在 [記事本] 中顯示檔案：

```
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
