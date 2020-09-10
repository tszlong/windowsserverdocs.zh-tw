---
title: exit
description: Exit 的參考文章，結束命令直譯器。
ms.topic: reference
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c1fbf37b80c55a9620c2e72d20ea13c6766b7da9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635957"
---
# <a name="exit"></a>exit

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束命令直譯器或目前的批次腳本。

## <a name="syntax"></a>語法

```
exit [/b] [<exitcode>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| /b | 結束目前的批次腳本，而不是結束 Cmd.exe。 如果是從批次腳本外部執行，則結束 Cmd.exe。 |
| `<exitcode>` | 指定數位。 如果指定 **/b** ，ERRORLEVEL 環境變數就會設定為該數位。 如果您要結束命令直譯器，進程結束代碼會設定為該數位。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要關閉命令直譯器，請輸入：

```
exit
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)
