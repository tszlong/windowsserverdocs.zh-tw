---
title: shift
description: Shift 的參考主題，會變更批次檔中批次參數的位置。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 870bda19de3426fd7007020efb2f3db39bf654c8
ms.sourcegitcommit: 7116460855701eed4e09d615693efa4fffc40006
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/15/2020
ms.locfileid: "83433162"
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
|/n \< n>|指定在第*N*個引數開始轉移，其中*N*是0到8之間的任何值。 需要預設啟用的命令延伸模組。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

- **Shift**命令會藉由將每個參數複製到前一個參數來變更批次參數 **%0**到 **%9**的值- **%1**的值會複製到 **%0**，而 **%2**的值會複製到 **%1**，依此類推。 這適用于撰寫在任何數目的參數上執行相同作業的批次檔。
- 如果已啟用命令延伸模組， **shift**命令會支援 **/n**命令列選項。 **/N**選項指定在第 n 個引數開始轉移，其中**n**是0到8之間的任何值。 例如， **shift/2**會將 **%3**改成% **2**， **%4**到 **%3**，依此類推，而不會影響 **%0**和 **%1** 。 預設會啟用命令延伸模組。
- 您可以使用**shift**命令來建立可接受10個以上批次參數的批次檔。 如果您在命令列上指定超過10個參數，則在第十個（**%9**）之後出現的參數，將會一次移位一個到 **%9**。
- **Shift**命令對 batch 參數不會有任何作用 **%\*** 。
- 沒有回溯**shift**命令。 在您執行**shift**命令之後，就無法復原在轉移之前已存在的批次參數（**%0**）。

## <a name="examples"></a>範例

來自名為 Mycopy 之範例批次檔的下列幾行會示範如何搭配使用**shift**與任意數目的批次參數。 在此範例中，Mycopy 會將檔案清單複製到特定目錄。 批次參數是以目錄和檔案名引數來表示。
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
