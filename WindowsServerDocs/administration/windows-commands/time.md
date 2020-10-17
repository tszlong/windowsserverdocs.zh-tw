---
title: time
description: Time 命令的參考文章，此命令會顯示或設定系統時間。
ms.topic: reference
ms.assetid: 1276a257-7283-41da-ae80-fb4cfb311f9d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: b69db2fce1af52d8f284e79fe83f5ac20c398cd6
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156472"
---
# <a name="time"></a>time

顯示或設定系統時間。 如果使用時不含參數， **時間** 會顯示目前的系統時間，並提示您輸入新的時間。

> [!NOTE]
> 您必須是系統管理員才能變更目前的時間。

## <a name="syntax"></a>語法

```
time [/t | [<HH>[:<MM>[:<SS>]] [am|pm]]]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| `<HH>[:<MM>[:<SS>[.<NN>]]] [am | pm]` | 將系統時間設定為指定的新時間，其中 *HH* 是以小時 (所需的時間) ， *MM* 是以分鐘為單位，而 *SS* 則以秒為單位。 *NN* 可以用來指定百分之一秒。 您必須將 *HH*、 *MM*和 *SS* 的值分隔為冒號 (： ) 。 *SS* 和 *NN* 必須以句號分隔 (。 ) 。<p>如果未指定 **am** 或 **pm** ，則 **時間** 預設會使用24小時制。 |
| /t | 顯示目前的時間，而不提示您新的時間。 |
| /? | 在命令提示字元顯示說明。 |

#### <a name="remarks"></a>備註

- 有效的 *HH* 值為0到24。

- 有效的 *MM* 和 *SS* 值為0到59。

## <a name="examples"></a>範例

如果已啟用命令延伸模組，若要顯示目前的系統時間，請輸入：

```
time /t
```

若要將目前的系統時間變更為 5:30 PM，請輸入下列其中一項：

```
time 17:30:00
time 5:30 pm
```

若要顯示目前的系統時間，然後輸入新時間的提示，請輸入：

```
The current time is: 17:33:31.35
Enter the new time:
```

若要保留目前的時間並返回命令提示字元，請按 ENTER 鍵。 若要變更目前的時間，請輸入新的時間，然後按 ENTER。

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
