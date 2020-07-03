---
title: 寫入器
description: 寫入器的參考發行項，它會確認寫入器或元件是否包含在備份或還原程式中，或排除寫入器或元件。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16746f2f070b87e0c287f3a49b19a480ba5399c9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936088"
---
# <a name="writer"></a>寫入器



確認已包含寫入器或元件，或從備份或還原程式中排除寫入器或元件。 如果在沒有參數的情況下使用，**寫入器**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>參數

| 參數  |                                                                                      說明                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   驗證   | 驗證指定的寫入器或元件是否包含在備份或還原程式中。 如果未包含寫入器或元件，備份或還原程式將會失敗。 |
|  排除   |                                                   從備份或還原程式中排除指定的寫入器或元件。                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a>範例

若要藉由指定 GUID 來驗證寫入器（在此範例中為4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f），請輸入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除具有 name System Writer 的寫入器，請輸入：
```
writer exclude System Writer
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)