---
title: date
description: 日期的 Windows 命令主題，會顯示或設定系統日期。 如果使用時不含參數，
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8f9e32240eb27d651e324becefd72e9b1a545215
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846741"
---
# <a name="date"></a>date

顯示或設定系統日期。 如果在沒有參數的情況下使用， **date**會顯示目前的系統日期設定，並提示您輸入新的日期。

如需如何使用此命令的範例，請參閱[範例](#BKMK_examples)。

## <a name="syntax"></a>語法

```
date [/t | <Month-Day-Year>]
```

### <a name="parameters"></a>參數

|參數|描述|
|---------|-----------|
|\<月份-每年 >|設定指定的日期，其中*month*是月份（一或兩位數）、 *day*是日（一或兩位數），而*year*是年份（二或四位數）。|
|/t|顯示目前的日期，而不提示您輸入新的日期。|
|/?|在命令提示字元顯示說明。|

## <a name="remarks"></a>備註

-   若要變更目前的日期，您必須具有系統管理認證。
-   您必須以句點（.）、連字號（-）或斜線（/）分隔*月*、*日*和*年*的值。
-   有效的*月份*值為1到12。
-   有效的*日期*值為1到31。
-   有效的*年份*值為00到99或1980到2099。 如果您使用兩位數，值80到99會對應至1980年到1999的年份。

## <a name="examples"></a><a name=BKMK_examples></a>典型

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

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)