---
title: manage-bde 升級
description: Manage-bde 升級命令的參考主題，它會升級 BitLocker 版本。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 23bfa824-6ff0-44cc-9b8b-b199a769fb8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e1789e119db8a2886c19dab9acfe4c7b12d651d
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222134"
---
# <a name="manage-bde-upgrade"></a>manage-bde 升級

升級 BitLocker 版本。

## <a name="syntax"></a>語法

```
manage-bde -upgrade [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -computername | 指定 manage-bde.wsf 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

## <a name="examples"></a>範例

若要升級 C 磁片磁碟機上的 BitLocker 加密，請輸入：

```
manage-bde –upgrade C:
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
