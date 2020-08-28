---
title: date
description: 日期命令的參考文章，此命令會顯示或設定系統日期。 如果使用時不含參數，
ms.topic: reference
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 78c8ca51882b2559e78cc457d1fd7fa5ec8e87b8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033036"
---
# <a name="date"></a>date

顯示或設定系統日期。 如果使用時不含參數， **日期** 會顯示目前的系統日期設定，並提示您輸入新的日期。

>[!IMPORTANT]
> 您必須是系統管理員才能使用此命令。

## <a name="syntax"></a>語法

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<month-day-year>` | 設定指定的日期，其中 *month* 是月份 (一或兩位數，包括1到12的值) 、 *day* 是 (一或兩位數的日期，包括值1到 31) ，而 *year* 是年份 (二或四位數，包括值00到99或1980到 2099) 。 您必須將 *month*、 *day*和 *year* 的值與句號分隔 (。 ) 、連字號 ( ) 或斜線標記 (/) 。<p>**注意：** 請注意，如果您使用2位數來代表年份，則值80-99 會對應到1980到1999。 |
| /t | 顯示目前的日期，而不提示您輸入新的日期。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

如果已啟用 [命令延伸模組]，若要顯示目前的系統日期，請輸入：

```
date /t
```

若要將目前的系統日期變更為2007年8月3日，您可以輸入下列其中一項：

```
date 08.03.2007
date 08-03-07
date 8/3/07
```

若要顯示目前的系統日期，後面接著輸入新日期的提示，請輸入：

```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yyyy)
```

若要保留目前的日期並返回命令提示字元，請按 **enter**。 若要變更目前的日期，請輸入新的日期，然後按 **enter**。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)