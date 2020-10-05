---
title: simulate restore
description: 模擬還原命令的參考文章，此命令會測試電腦上的寫入器介入是否會成功，而不會對寫入器發出 PreRestore 或 PostRestore 事件。
ms.topic: reference
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e9ca1760cd1d6a125267e152274ea26e1a9ae11f
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718295"
---
# <a name="simulate-restore"></a>模擬還原

測試還原會話中的寫入器是否會成功，而不會將 **PreRestore** 或 **PostRestore** 事件發出至寫入器。

> [!NOTE]
> 必須選取 DiskShadow 中繼資料檔案， **模擬還原** 命令才能成功。 使用 [load metadata 命令](load-metadata.md) 載入選取的寫入器和用於還原的元件。

## <a name="syntax"></a>Syntax

```
simulate restore
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [載入中繼資料命令](load-metadata.md)
