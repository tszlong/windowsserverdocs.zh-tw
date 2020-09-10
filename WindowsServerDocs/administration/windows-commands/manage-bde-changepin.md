---
title: manage-bde changepin
description: Manage-bde changepin 命令的參考文章，此命令會修改作業系統磁片磁碟機的 PIN。
ms.topic: reference
ms.assetid: c85aa1c7-3485-4839-a292-99dfcd6db252
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7b920e9c4580fced678e9d7dddd30fff66dad802
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622635"
---
# <a name="manage-bde-changepin"></a>manage-bde changepin

修改作業系統磁片磁碟機的 PIN。 系統會提示使用者輸入新的 PIN。

## <a name="syntax"></a>語法

```
manage-bde -changepin [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要變更與磁片磁碟機 C 上的 BitLocker 搭配使用的 PIN 碼，請輸入：

```
manage-bde –changepin C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
