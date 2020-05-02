---
title: rem
description: '* * * * 的參考主題'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d115548f15ff45087a771458062da8a3ef919eb3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722458"
---
# <a name="rem"></a>rem



記錄批次檔或設定中的批註（備註）。SYS.DATABASES. 如果未指定任何批註， **rem**會加入垂直間距。



## <a name="syntax"></a>語法

```
rem [<Comment>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<批註>|指定要包含在批註中的字元字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Rem**命令不會在螢幕上顯示批註。 您必須在批次或設定中使用**echo on**命令。要在螢幕上顯示批註的 SYS 檔案。
-   您不能在批次檔批註**<** 中**>** 使用重新導向字元**|**（或）或管道（）。
-   雖然您可以使用**rem**而不加批註來將垂直間距新增至批次檔，但您也可以使用空白行。 處理批次程式時，會忽略空白行。

## <a name="examples"></a>範例

若要顯示批次檔，以使用備註和垂直間距：
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
在您的設定中，于**prompt**命令前面加入說明性批註。SYS 檔案中，將下列幾行新增至 CONFIG。SYS.DATABASES
```
rem Set prompt to indicate current directory
prompt $p$g
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)