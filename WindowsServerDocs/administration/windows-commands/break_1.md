---
title: break
description: '適用于**break_1**的 Windows 命令主題-設定或清除 MS-DOS 系統上的擴充 CTRL + C 檢查。 如果在沒有參數的情況下使用， **break**會顯示目前的設定。 '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 73afdac29efbfd9efec88d297cf4185ca1b92d62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/27/2019
ms.locfileid: "71380486"
---
# <a name="break"></a>break



設定或清除 MS-DOS 系統上的擴充 CTRL + C 檢查。 如果在沒有參數的情況下使用， **break**會顯示目前的設定。

> [!NOTE]
> 此命令不再使用。 只是為了保持與現有 MS-DOS 檔案相容才包含它，但是在命令列上它沒有效果，因為此功能是自動的。

## <a name="syntax"></a>語法

```
break=[on|off]
```

## <a name="remarks"></a>備註

如果命令延伸模組已在 Windows 平臺上啟用並執行，則將**break**命令插入批次檔時，如果偵錯工具正在進行調試，則會進入硬式編碼的中斷點。

#### <a name="additional-references"></a>其他參考資料

[命令列語法關鍵](command-line-syntax-key.md)