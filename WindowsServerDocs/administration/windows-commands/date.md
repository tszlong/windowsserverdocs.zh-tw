---
title: date
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e774f7bfabb9b574255691dd97d2cfff36f034e7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877949"
---
# <a name="date"></a>date



顯示或設定系統日期。 如果未指定參數，使用**日期**會顯示目前的系統日期設定，並會提示您輸入新的日期。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
date [/t | <Month-Day-Year>]
```

## <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<Month-Day-Year>|其中設定指定的日期*月份*是月 （一或兩個位數），*天*是一天 （一或兩個位數），和*年*是一年 （兩個或四位數字）。|
|/t|顯示目前的日期，而不提示您輸入新的日期。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要變更目前的日期，您必須具有系統管理認證。
-   您必須分隔的值*月份*，*天*，並*年*使用句號 （.）、 連字號 （-） 或斜線標記 （/）。
-   有效*月份*的值為 1 到 12。
-   有效*天*的值為 1 到 31。
-   有效*年份*值是 00 到 99 或透過 2099年 1980年。 如果您使用的兩位數，80 到 99 的值會對應到 1999 年 1980年年。

## <a name="BKMK_examples"></a>範例

如果已啟用命令延伸模組，以顯示目前的系統日期，輸入：
```
date /t
```
若要將目前的系統日期變更為 2007 年 8 月 3 日，您可以輸入下列其中一項：
```
date 08.03.2007
date 08-03-07
date 8/3/07
```
若要顯示目前的系統日期，後面接著提示字元中輸入新的日期，請輸入：
```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yy)
```
若要保留目前的日期，並返回命令提示字元，按 ENTER 鍵。 若要變更目前的日期，輸入新的日期，然後按 ENTER。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)