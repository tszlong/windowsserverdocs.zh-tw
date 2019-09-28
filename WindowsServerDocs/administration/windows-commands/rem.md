---
title: rem
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a45b585-a83c-4ff6-badd-ff40f34cec23
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2da0b6e42858582c1485659f3bf8f59e8e2ed97e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384570"
---
# <a name="rem"></a>rem



記錄批次檔或設定中的批註（備註）。SYS.DATABASES. 如果未指定任何批註， **rem**會加入垂直間距。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
rem [<Comment>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Comment >|指定要包含在批註中的字元字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Rem**命令不會在螢幕上顯示批註。 您必須在批次或設定中使用**echo on**命令。要在螢幕上顯示批註的 SYS 檔案。
-   您不能在批次檔批註中使用重新導向字元（ **<** 或 **>** ）或管道（ **|** ）。
-   雖然您可以使用**rem**而不加批註來將垂直間距新增至批次檔，但您也可以使用空白行。 處理批次程式時，會忽略空白行。

## <a name="BKMK_examples"></a>典型

下列範例顯示的批次檔會使用備註和垂直間距：
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

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)