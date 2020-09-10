---
title: endlocal
description: Endlocal 命令的參考文章，此命令會結束批次檔中環境變更的當地語系化，並在執行對應的 setlocal 命令之前，將環境變數還原為其值。
ms.topic: reference
ms.assetid: 765fae3c-0c0a-4639-99a4-cf613489b949
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4220d143be2e3af9378854aa1a649c2358b44560
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636114"
---
# <a name="endlocal"></a>endlocal

結束批次檔中環境變更的當地語系化，並在執行對應的 **setlocal** 命令之前，將環境變數還原為其值。

## <a name="syntax"></a>語法

```
endlocal
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- **Endlocal**命令在腳本或批次檔以外沒有任何作用。

- 在批次檔的結尾有一個隱含的 **endlocal** 命令。

- 如果已啟用命令延伸模組 (預設會啟用命令延伸模組) ， **endlocal** 命令會還原 (的命令延伸模組的狀態，也就是啟用或停用) 執行對應的 **setlocal** 命令之前的內容。

> [!NOTE]
> 如需啟用和停用命令延伸模組的詳細資訊，請參閱 [Cmd 命令](cmd.md)。

### <a name="examples"></a>範例

您可以將批次檔中的環境變數當地語系化。 例如，下列程式會在網路上啟動 *superapp* 批次程式、將輸出導向至檔案，然後在 [記事本] 中顯示檔案：

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
