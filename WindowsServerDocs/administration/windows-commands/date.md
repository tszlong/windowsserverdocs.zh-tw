---
title: date
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7328b2b5d3c78fdfd741918d76e26195f0af4a1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378823"
---
# <a name="date"></a>date



顯示或設定系統日期。 如果在沒有參數的情況下使用， **date**會顯示目前的系統日期設定，並提示您輸入新的日期。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Month-每年 >|設定指定的日期，其中*month*是月份（一或兩位數）、 *day*是日（一或兩位數），而*year*是年份（二或四位數）。|
|一起|顯示目前的日期，而不提示您輸入新的日期。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要變更目前的日期，您必須具有系統管理認證。
-   您必須以句點（.）、連字號（-）或斜線（/）分隔*月*、*日*和*年*的值。
-   有效的*月份*值為1到12。
-   有效的*日期*值為1到31。
-   有效的*年份*值為00到99或1980到2099。 如果您使用兩位數，值80到99會對應至1980年到1999的年份。

## <a name="BKMK_examples"></a>典型

如果已啟用命令延伸模組，若要顯示目前的系統日期，請輸入：
```
date /t
```
若要將目前的系統日期變更為2007年8月3日，您可以輸入下列任何一項：
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
若要顯示目前的系統日期，並接著輸入新日期的提示，請輸入：
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
若要保留目前的日期並返回命令提示字元，請按 ENTER。 若要變更目前的日期，請輸入新的日期，然後按 ENTER 鍵。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)