---
title: writer
description: 寫入器的 Windows 命令主題，它會確認寫入器或元件是否包含在備份或還原程式中，或排除寫入器或元件。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fb13de162b8e5eb8150d145a4afacccf47bb25f0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828981"
---
# <a name="writer"></a>writer



確認已包含寫入器或元件，或從備份或還原程式中排除寫入器或元件。 如果在沒有參數的情況下使用，**寫入器**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

### <a name="parameters"></a>參數

| 參數  |                                                                                      描述                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | 驗證指定的寫入器或元件是否包含在備份或還原程式中。 如果未包含寫入器或元件，備份或還原程式將會失敗。 |
|  排除   |                                                   從備份或還原程式中排除指定的寫入器或元件。                                                    |
| [\<寫入器 > |                                                                                     <Component>]                                                                                      |

## <a name="examples"></a><a name=BKMK_examples></a>典型

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