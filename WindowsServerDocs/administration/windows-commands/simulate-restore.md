---
title: 模擬還原
description: '\* * * * 的 Windows 命令主題 '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d6652fea4e74c706fcc03b8a547fab771a7c0191
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370879"
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

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)