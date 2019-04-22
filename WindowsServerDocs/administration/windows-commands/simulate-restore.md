---
title: 模擬還原
description: '適用於 Windows 命令主題 * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d883d94c-3cb1-4848-9d74-1b4378044b31
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 09b626939c13d4e38a983435b45d8c47ee2b93a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817249"
---
# <a name="simulate-restore"></a>模擬還原



測試電腦上的還原工作階段中的寫入器的參與，但不會發出**PreRestore**或是**PostRestore**寫入器的事件。

## <a name="syntax"></a>語法

```
simulate restore
```

## <a name="remarks"></a>備註

-   **模擬還原**用來測試是否可以成功還原與寫入器。
-   您可以使用之前**模擬還原**，您必須使用載入 DiskShadow 中繼資料檔**載入中繼資料**命令。 這會載入所選的寫入器和還原元件。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)