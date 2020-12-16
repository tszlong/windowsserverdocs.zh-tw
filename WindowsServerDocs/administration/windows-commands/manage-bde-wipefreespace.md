---
title: manage-bde wipefreespace
description: Manage-bde wipefreespace 命令的參考文章，它會清除磁片區上的可用空間，以移除空間中可能存在的任何資料片段。
ms.topic: reference
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d263a38545cd5cc59298c7e8bfdf7bc9a35b1b84
ms.sourcegitcommit: 6fbe337587050300e90340f9aa3e899ff5ce1028
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599661"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde wipefreespace

清除磁片區上的可用空間，以移除空間中可能存在的任何資料片段。 在使用「 **僅限使用的空間** 加密」方法加密的磁片區上執行此命令，會提供與 **完整磁片區加密** 加密方法相同的保護層級。

## <a name="syntax"></a>語法

```
manage-bde -wipefreespace|-w [<drive>] [-cancel] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<drive>` | 代表後面加上冒號的磁碟機號。 |
| -取消 | 取消抹除正在處理中的可用空間。 |
| -computername | 指定 manage-bde.exe 將用來修改不同電腦上的 BitLocker 保護。 您也可以使用 **-cn** 作為此命令的縮寫版本。 |
| `<name>` | 代表要修改 BitLocker 保護的電腦名稱稱。 接受的值包括電腦的 NetBIOS 名稱和電腦的 IP 位址。 |
| -? 或/？ | 在命令提示字元中顯示簡短說明。 |
| -help 或-h | 在命令提示字元中顯示完整說明。 |

### <a name="examples"></a>範例

若要抹除磁片磁碟機 C 的可用空間，請輸入下列其中一項：

```
manage-bde -w C:
```

```
manage-bde -wipefreespace C:
```

若要取消抹除磁片磁碟機 C 上的可用空間，請輸入下列其中一項：

```
manage-bde -w -cancel C:
```

```
manage-bde -wipefreespace -cancel C:
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [manage-bde 命令](manage-bde.md)
