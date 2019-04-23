---
title: ftype
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c29ca0aa027d11fa8f981134e5367021227d3096
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881679"
---
# <a name="ftype"></a>ftype



顯示或修改用於檔案名稱副檔名關聯的檔案類型。 如果沒有指派運算子 (**=**)， **ftype**會顯示目前開啟的命令字串指定的檔案類型。 如果未指定參數，使用**ftype**會顯示已定義的 [開啟] 命令字串的檔案類型。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<FileType>|指定要顯示或變更的檔案類型。|
|\<OpenCommandString>|指定開啟指定的檔案類型的檔案時要使用的 [開啟] 命令字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

下表描述如何**ftype**取代為 [開啟] 命令字串內的變數：

|變數|取代值|
|--------|-----------------|
|%0或 %1|取得用來替代透過關聯所啟動的檔案名稱。|
|%*|取得所有參數。|
|%2, %3, ...|取得第一個參數 (%2)、 第二個參數 (%3) 等等。|
|%~\<N>|取得所有剩餘的參數開頭*N*個參數，其中*N*可以是任何從 2 到 9 的數字。|

## <a name="BKMK_examples"></a>範例

若要顯示目前已定義的 [開啟] 命令字串的檔案類型，請輸入：
```
ftype
```
若要顯示目前開啟的命令字串*txtfile*檔案類型，類型：
```
ftype txtfile
```
此命令會產生類似下列輸出：
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
若要刪除的檔案類型，稱為 [開啟] 命令字串*範例*，型別：
```
ftype example=
```
若要 perlscript 這樣的檔案類型相關聯.pl 副檔名，並啟用 perlscript 這樣的檔案類型，若要執行 PERL。EXE，輸入下列命令：
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
若要消除不必輸入.pl 副檔名，叫用 Perl 指令碼時，請輸入：
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)