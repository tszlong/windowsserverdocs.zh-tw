---
title: manage-bde changekey
description: Manage-bde changekey 命令的參考文章，此命令會修改作業系統磁片磁碟機的啟動金鑰。
ms.topic: reference
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3752d0a3a990488129b99a5ebbe44e830552c00f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622739"
---
# <a name="manage-bde-changekey"></a>manage-bde changekey

修改作業系統磁片磁碟機的啟動金鑰。

## <a name="syntax"></a>語法

```
manage-bde -changekey [<drive>] [<pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

若要在磁片磁碟機 E 上建立新的啟動金鑰，以在磁片磁碟機 C 上使用 BitLocker 加密，請輸入：

```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
