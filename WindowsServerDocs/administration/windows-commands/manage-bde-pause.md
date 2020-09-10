---
title: 管理-manage-bde 暫停
description: Manage-bde pause 命令的參考文章，會暫停 BitLocker 加密或解密。
ms.topic: reference
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ca15aa1b48e0b06a036eaf8906c7d5b7d43b881b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639641"
---
# <a name="manage-bde-pause"></a>管理-manage-bde 暫停

暫停 BitLocker 加密或解密。

## <a name="syntax"></a>語法

```
manage-bde -pause [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<volume>` | 指定磁碟機號，後面接著冒號、磁片區 GUID 路徑或已載入的磁片區。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要暫停磁片磁碟機 C 上的 BitLocker 加密，請輸入：

```
manage-bde pause C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [命令上的 manage-bde](manage-bde-on.md)

- [manage-bde off 命令](manage-bde-off.md)

- [manage-bde resume 命令](manage-bde-resume.md)

- [manage-bde 命令](manage-bde.md)
