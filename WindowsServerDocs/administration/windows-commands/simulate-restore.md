---
title: simulate restore
description: 模擬還原的參考檔，它會測試寫入器對電腦上的還原會話進行的操作，而不會將 PreRestore 或 PostRestore 事件發行到寫入器。
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d72e4b473b3913bff744ff7a34b6508bde52ae0e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640929"
---
# <a name="simulate-restore"></a>模擬還原

測試寫入器在電腦上還原會話的參與，而不會將 **PreRestore** 或 **PostRestore** 事件發行到寫入器。

## <a name="syntax"></a>語法

```
simulate restore
```

## <a name="remarks"></a>備註

-   **模擬還原** 是用來測試 restore with 寫入器是否可以成功。
-   您必須先使用**load metadata**命令載入 DiskShadow 中繼資料檔案，才能使用**模擬還原**。 這會載入選取的寫入器和用於還原的元件。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)