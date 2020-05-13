---
title: exit
description: Exit 的參考主題，它會結束命令直譯器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3cee4a2-6210-46f0-b8e4-7381c3c4e530
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97068232a7ffa82e59ba486b449af96638e3d8f0
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235065"
---
# <a name="exit"></a>exit

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

結束命令直譯器或目前的批次腳本。

## <a name="syntax"></a>語法

```
exit [/b] [<exitcode>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| /b | 結束目前的批次腳本，而不是結束 Cmd.exe。 如果是從批次腳本外部執行，則會結束 Cmd.exe。 |
| `<exitcode>` | 指定數位。 如果指定 **/b** ，ERRORLEVEL 環境變數會設定為該數位。 如果您要結束命令直譯器，進程結束代碼會設定為該數位。 |
| /? | 在命令提示字元顯示說明。 |

## <a name="examples"></a>範例

若要關閉命令直譯器，請輸入：

```
exit
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)
