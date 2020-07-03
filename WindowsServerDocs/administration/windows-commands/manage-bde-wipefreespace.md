---
title: manage-bde wipefreespace
description: Wipefreespace 命令的參考文章，它會抹除磁片區上的可用空間，以移除可能已存在於空間中的任何資料片段。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 872c3722028af1612fb80e3b98650ee0a39261ce
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922174"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde wipefreespace

抹除磁片區上的可用空間，移除空間中可能存在的任何資料片段。 在使用 [**僅使用的空間**加密] 方法加密的磁片區上執行這個命令，會提供與**完整磁片區加密**加密方法相同的保護層級。

## <a name="syntax"></a>語法

```
manage-bde -wipefreespace|-w [<drive>] [-cancel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<drive>` | 表示後面接著冒號的磁碟機號。 |
| -取消 | 取消清除正在處理的可用空間。 |
| -computername | 指定 manage-bde.exe 將用來修改另一部電腦上的 BitLocker 保護。 您也可以使用 **-cn**做為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整的說明。 |

### <a name="examples"></a>範例

若要抹除 C 磁片磁碟機上的可用空間，請輸入： \

```
manage-bde -w C:
```

```
manage-bde -wipefreespace C:
```

若要取消抹除磁片磁碟機 C 上的可用空間，請輸入：

```
manage-bde -w -cancel C:
```

```
manage-bde -wipefreespace -cancel C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
