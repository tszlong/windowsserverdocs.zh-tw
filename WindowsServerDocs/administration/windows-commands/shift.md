---
title: shift
description: Shift 命令的參考文章，會變更批次檔中批次參數的位置。
ms.topic: reference
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4553826aa7c3def335ae1d9ec9594ee9d878fc63
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718335"
---
# <a name="shift"></a>shift

變更批次檔中批次參數的位置。

## <a name="syntax"></a>語法

```
shift [/n <N>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| n `<N>` | 指定開始在第 *n*個引數進行移位，其中 *n* 是從 *0* 到 *8*的任意值。 需要預設啟用的命令延伸模組。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="remarks"></a>備註

- **Shift**命令會將每個參數複製到前一個參數，以變更批次參數 **%0**到 **%9**的值- **%1**的值會複製到 **%0**，而 **%2**的值會複製到 **%1**，依此類推。 這對於撰寫在任意數目的參數上執行相同作業的批次檔很有用。

- 如果命令延伸模組已啟用，則 **shift** 命令支援 **/n** 命令列選項。 **/N**選項指定在第 n 個引數開始移動，其中**n**是介於0到8之間的任何值。 例如， **shift/2** 會將 **%3** 移至 **%2**， **%4** 到 **%3**，依此類推，而不會影響 **%0** 和 **%1** 。 預設會啟用命令延伸模組。

- 您可以使用 **shift** 命令來建立可接受超過10個批次參數的批次檔。 如果您在命令列上指定10個以上的參數，則在第十個 (**%9**) 之後出現的參數，將會一次向 **%9**移動一次。

- **Shift**命令對 batch 參數沒有任何作用 **%\*** 。

- 沒有回溯 **shift** 命令。 在您執行 **shift** 命令之後，就無法復原 (**%0**) 的批次參數，該參數會在移位之前存在。

## <a name="examples"></a>範例

若要使用稱為 *Mycopy.bat*的批次檔，將檔案清單複製到特定目錄，請輸入：

```
@echo off
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ...
set todir=%1
:getfile
shift
if %1== goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
