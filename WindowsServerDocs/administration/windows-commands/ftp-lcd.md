---
title: ftp lcd
description: Ftp lcd 命令的參考文章，它會變更本機電腦上的工作目錄。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d98b300de74ec537dedd3f43ccc2404c7350ac0c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957780"
---
# <a name="ftp-lcd"></a>ftp lcd

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

變更本機電腦上的工作目錄。 根據預設，工作目錄是**ftp**命令啟動所在的目錄。

## <a name="syntax"></a>語法

```
lcd [<directory>]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `[<directory>]` | 指定要變更之本機電腦上的目錄。 如果未指定*目錄*，則目前的工作目錄會變更為預設目錄。 |

### <a name="examples"></a>範例

若要將本機電腦上的工作目錄變更為*c:\dir1*，請輸入：

```
lcd c:\dir1
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
