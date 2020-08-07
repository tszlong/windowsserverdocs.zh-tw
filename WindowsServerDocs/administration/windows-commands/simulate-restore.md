---
title: simulate restore
description: 模擬還原的參考文章，其會測試作者參與電腦上還原會話的工作，而不會對寫入器發出 PreRestore 或 PostRestore 事件。
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec2c49f0dfcb68f6b3b6ef0567778a4317e7dc24
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882346"
---
# <a name="simulate-restore"></a>模擬還原

測試 writer 會參與電腦上的還原會話，而不會對寫入器發出**PreRestore**或**PostRestore**事件。

## <a name="syntax"></a>語法

```
simulate restore
```

## <a name="remarks"></a>備註

-   [**模擬還原**] 用來測試寫入器是否可以成功進行還原。
-   在您可以使用 [**模擬還原**] 之前，您必須使用 [**載入中繼資料**] 命令來載入 DiskShadow 中繼資料檔案。 這會載入選取的寫入器和還原的元件。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)