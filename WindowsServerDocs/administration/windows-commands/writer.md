---
title: 寫入器
description: 寫入器命令的參考文章，可驗證寫入器或元件是否包含在備份或還原程式中，或從備份或還原程式中排除寫入器或元件。
ms.topic: reference
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d6af778ca49f8be88a08e04ecfe4165752cc796d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524863"
---
# <a name="writer"></a>寫入器

驗證寫入器或元件是否包含在備份或還原程式中，或從寫入器或元件中排除。 如果使用時不含參數， **寫入器** 會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
writer verify [writer> | <component>]
writer exclude [<writer> | <component>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
|--|--|
| 驗證 | 確認指定的寫入器或元件包含在備份或還原程式中。 如果未包含寫入器或元件，則備份或還原程式將會失敗。 |
| 排除 | 從備份或還原程式中排除指定的寫入器或元件。 |

## <a name="examples"></a>範例

若要藉由指定此範例的 GUID (來驗證寫入器，請 4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f) ，輸入：

```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```

若要排除具有名稱 *系統寫入*器的寫入器，請輸入：

```
writer exclude System Writer
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
