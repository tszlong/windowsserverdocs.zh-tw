---
title: manage-bde changepassword
description: Manage-bde changepassword 命令的參考文章，此命令會修改資料磁片磁碟機的密碼。
ms.topic: reference
ms.assetid: b174e152-8442-4fba-8b33-56a81ff4f547
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 97bae4c10756818ec8475a114aa048a4cae58617
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030536"
---
# <a name="manage-bde-changepassword"></a>manage-bde changepassword

修改資料磁片磁碟機的密碼。 系統會提示使用者輸入新密碼。

## <a name="syntax"></a>語法

```
manage-bde -changepassword [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要變更用來解除鎖定資料磁片磁碟機 D 上 BitLocker 的密碼，請輸入：

```
manage-bde –changepassword D:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
