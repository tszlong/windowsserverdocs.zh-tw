---
title: manage-bde resume
description: Manage-bde resume 命令的參考文章，此命令會在暫停之後繼續 BitLocker 加密或解密。
ms.topic: reference
ms.assetid: ca3cd1ca-6f2c-4190-b68f-27816635facb
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ebae22e60a8110bc2ba5e4649fc6cf143431b359
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639297"
---
# <a name="manage-bde-resume"></a>manage-bde resume

在 BitLocker 加密或解密暫停之後繼續進行。

## <a name="syntax"></a>語法

```
manage-bde -resume [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要在磁片磁碟機 C 上繼續 BitLocker 加密，請輸入：

```
manage-bde –resume C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [命令上的 manage-bde](manage-bde-on.md)

- [manage-bde off 命令](manage-bde-off.md)

- [manage-bde pause 命令](manage-bde-pause.md)

- [manage-bde 命令](manage-bde.md)
