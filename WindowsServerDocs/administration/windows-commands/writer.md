---
title: 編寫
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7cf98295-411d-4705-8573-f898ff45c140
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c00f6067cd5f6cf741cddbd6d62c5bcbb1f37a9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361864"
---
# <a name="writer"></a>編寫



確認已包含寫入器或元件，或從備份或還原程式中排除寫入器或元件。 如果在沒有參數的情況下使用，**寫入器**會在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>參數

| 參數  |                                                                                      描述                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | 驗證指定的寫入器或元件是否包含在備份或還原程式中。 如果未包含寫入器或元件，備份或還原程式將會失敗。 |
|  exclude   |                                                   從備份或還原程式中排除指定的寫入器或元件。                                                    |
| [\<Writer > |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>典型

若要藉由指定 GUID 來驗證寫入器（在此範例中為4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f），請輸入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除名稱為「系統寫入器」的寫入器，請輸入：
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)