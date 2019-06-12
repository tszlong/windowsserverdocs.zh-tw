---
title: 寫入器
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 8aee4ecca85c7d5f46ee79f3ad928b746c02e7bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439981"
---
# <a name="writer"></a>寫入器



確認寫入器或元件就會包含或排除在備份或還原程序中的寫入器或元件。 如果未指定參數，使用**寫入器**在命令提示字元中顯示說明。

## <a name="syntax"></a>語法

```
writer verify [<Writer> | <Component>]
writer exclude [<Writer> | <Component>]
```

## <a name="parameters"></a>參數

| 參數  |                                                                                      描述                                                                                      |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   verify   | 確認指定的寫入器或元件包含在備份或還原程序中。 如果寫入器或元件未包含，將會失敗的備份或還原程序。 |
|  exclude   |                                                   從備份或還原程序中排除指定的寫入器或元件。                                                    |
| [\<Writer> |                                                                                     <Component>]                                                                                      |

## <a name="BKMK_examples"></a>範例

若要確認寫入器指定 （適用於此範例中，4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f） 其 GUID，請輸入：
```
writer verify {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
```
若要排除具有名稱為 「 系統寫入器 」 的寫入器，請輸入：
```
writer exclude "System Writer"
```

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)