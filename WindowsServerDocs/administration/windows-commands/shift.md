---
title: shift
description: Shift 的參考文章，這會變更批次檔中批次參數的位置。
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985c1271a1da8f486e2313ba9aeb266803664b93
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882397"
---
# <a name="shift"></a>shift

變更批次檔中批次參數的位置。



## <a name="syntax"></a>語法

```
shift [/n <N>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|/n\<N>|指定在第*N*個引數開始轉移，其中*N*是0到8之間的任何值。 需要預設啟用的命令延伸模組。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- **Shift**命令會藉由將每個參數複製到前一個參數來變更批次參數 **%0**到 **%9**的值- **%1**的值會複製到 **%0**，而 **%2**的值會複製到 **%1**，依此類推。 這適用于撰寫在任何數目的參數上執行相同作業的批次檔。
- 如果已啟用命令延伸模組， **shift**命令會支援 **/n**命令列選項。 **/N**選項指定在第 n 個引數開始轉移，其中**n**是0到8之間的任何值。 例如， **shift/2**會將 **%3**改成% **2**， **%4**到 **%3**，依此類推，而不會影響 **%0**和 **%1** 。 預設會啟用命令延伸模組。
- 您可以使用**shift**命令來建立可接受10個以上批次參數的批次檔。 如果您在命令列上指定超過10個參數，則會在第十個 (**%9**) 之後，將其一次移位至 **%9**。
- **Shift**命令對 batch 參數不會有任何作用 **%\*** 。
- 沒有回溯**shift**命令。 在您執行**shift**命令之後，您無法復原 (**%0**) 的批次參數，這是在轉移之前存在的。

## <a name="examples"></a>範例

來自稱為的範例批次檔的下列幾行 Mycopy.bat 示範如何使用**shift**搭配任意數目的批次參數。 在此範例中，Mycopy.bat 會將檔案清單複製到特定目錄。 批次參數是以目錄和檔案名引數來表示。
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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
