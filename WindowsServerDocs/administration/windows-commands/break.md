---
title: break
description: Break 命令的參考文章，它會將具有焦點的鏡像磁碟區分成兩個簡單磁片區。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ffc4901c-457b-46a6-a671-3052355f8a3c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c5de6bb682e5094f555b6310d5c712c0739f9454
ms.sourcegitcommit: 145cf75f89f4e7460e737861b7407b5cee7c6645
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2020
ms.locfileid: "87409759"
---
# <a name="break"></a>break

> [!IMPORTANT]
> 此命令不再使用。 而僅是為了維持與現有 MS-DOS 檔案的相容性才將此命令包含在內，而且因為功能是自動的，它在命令列並不會有任何作用。

設定或清除 MS-DOS 系統上的擴充 CTRL + C 檢查。 如果在沒有參數的情況下使用， **break**會顯示現有的設定值。

如果命令延伸模組已在 Windows 平臺上啟用並執行，則將**break**命令插入批次檔時，如果偵錯工具正在進行調試，則會進入硬式編碼的中斷點。

## <a name="syntax"></a>語法

```
break=[on|off]
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [break 命令](break.md)