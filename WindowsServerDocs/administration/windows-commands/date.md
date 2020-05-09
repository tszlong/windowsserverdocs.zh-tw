---
title: date
description: Date 命令的參考主題，它會顯示或設定系統日期。 如果使用時不含參數，
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64d0d94061e1b5c7891b364f4c0fe153b44a564e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993195"
---
# <a name="date"></a>date

顯示或設定系統日期。 如果在沒有參數的情況下使用， **date**會顯示目前的系統日期設定，並提示您輸入新的日期。

>[!IMPORTANT]
> 您必須是系統管理員才能使用此命令。

## <a name="syntax"></a>語法

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<month-day-year>` | 設定指定的日期，其中*month*是月份（一或兩個數字，包括值1到12）、 *day*是一天（一或兩個數字，包括值1到31），而*year*是年份（兩個或四個數字，包括值00到99或1980到2099）。 您必須以句點（.）、連字號（-）或斜線（/）分隔*月*、*日*和*年*的值。<p>**注意：** 請注意，如果您使用2位數來代表年份，則值80-99 會對應到1980到1999。 |
| /t | 顯示目前的日期，而不提示您輸入新的日期。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

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
Enter the new date: (mm-dd-yyyy)
```

若要保留目前的日期並返回命令提示字元，請按**enter**。 若要變更目前的日期，請輸入新的日期，然後按**enter**鍵。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)