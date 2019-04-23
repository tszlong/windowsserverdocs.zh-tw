---
title: rem
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 85c8a69bf21a386cd36e45bbca6dacd35aef2509
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846999"
---
# <a name="rem"></a>rem



記錄中的註解 （備註） 批次檔或組態。SYS。 如果未不指定任何註解，則**rem**將垂直間距。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
rem [<Comment>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<註解 >|指定要加入做為註解字元的字串。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   **Rem**命令不會顯示在螢幕上的註解。 您必須使用**回應上**命令批次或組態中。若要在螢幕上顯示註解的 SYS 檔案。
-   您無法使用重新導向字元 (**<** 或是**>**) 或管道 (**|**) 批次檔案註解中。
-   雖然您可以使用**rem**而不需要將垂直間距新增至批次檔的註解，您也可以使用空白的行。 處理批次程式時，會忽略空白行。

## <a name="BKMK_examples"></a>範例

下列範例會使用註解的註解並將垂直間距的批次檔：
```
@echo off
rem  This batch program formats and checks new disks.
rem  It is named Checknew.bat.
rem
rem echo Insert new disk in Drive B.
pause 
format b: /v chkdsk b: 
```
要包含在之前的說明性註解**提示字元**命令，在您的設定。SYS 檔案中，將下列幾行新增至組態。SYS:
```
rem Set prompt to indicate current directory
prompt $p$g
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)