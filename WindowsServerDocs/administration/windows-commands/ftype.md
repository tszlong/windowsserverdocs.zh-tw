---
title: ftype
description: '\* * * * 的 Windows 命令主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f982f68f25a4decbc9c572b533fa1ecc5e893a8c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842701"
---
# <a name="ftype"></a>ftype



顯示或修改副檔名關聯中所使用的檔案類型。 如果使用時沒有指派運算子（ **=** ）， **ftype**會針對指定的檔案類型顯示目前開啟的命令字串。 如果在沒有參數的情況下使用， **ftype**會顯示已定義 open 命令字串的檔案類型。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<的類型 >|指定要顯示或變更的檔案類型。|
|\<OpenCommandString >|指定開啟指定檔案類型的檔案時，所要使用的 open 命令字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

下表描述**ftype**如何在開啟的命令字串中替代變數：

|變數|取代值|
|--------|-----------------|
|%0或 %1|會以透過關聯啟動的檔案名取代。|
|%*|取得所有參數。|
|%2，%3，。|取得第一個參數（%2）、第二個參數（%3）等等。|
|%~\<N >|取得所有開頭為第*N*個參數的剩餘參數，其中*N*可以是介於2到9之間的任何數位。|

## <a name="examples"></a><a name=BKMK_examples></a>典型

若要顯示目前已定義 open 命令字串的檔案類型，請輸入：
```
ftype
```
若要顯示*txtfile*檔案類型目前開啟的命令字串，請輸入：
```
ftype txtfile
```
此命令會產生類似下列的輸出：
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
若要刪除稱為*範例*之檔案類型的開啟命令字串，請輸入：
```
ftype example=
```
將 pl 副檔名與 PerlScript 檔案類型產生關聯，並啟用 PerlScript 檔案類型以執行 PERL。EXE，輸入下列命令：
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
若要在叫用 Perl 腳本時不必輸入 pl 副檔名，請輸入：
```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)