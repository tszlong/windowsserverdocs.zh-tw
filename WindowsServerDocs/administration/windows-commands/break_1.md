---
title: break
description: Break_1 的 Windows 命令主題，它會在 MS-DOS 系統上設定或清除擴充的 CTRL + C 檢查。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c89b7357-d69e-4141-826e-73c9ba0fc630
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 809a9321b8b4f8b2d201582f767da132076826d4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848361"
---
# <a name="break"></a>break

設定或清除 MS-DOS 系統上的擴充 CTRL + C 檢查。 如果在沒有參數的情況下使用， **break**會顯示目前的設定。

> [!NOTE]
> 此命令不再使用。 這個命令只是為了保留與現有 MS-DOS 檔案之間的相容性而存在，但是在命令列執行時並沒有作用，因為這是自動執行的功能。

## <a name="syntax"></a>語法

```
break=[on|off]
```

## <a name="remarks"></a>備註

如果命令延伸模組已在 Windows 平臺上啟用並執行，則將**break**命令插入批次檔時，如果偵錯工具正在進行調試，則會進入硬式編碼的中斷點。

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)