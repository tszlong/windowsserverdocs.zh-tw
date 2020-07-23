---
title: ftp literal
description: Ftp 常值命令的參考文章，它會將逐字引數傳送到遠端 ftp 伺服器。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c13458a80aa78b377f8859f7e81b8789bd014dc3
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957750"
---
# <a name="ftp-literal"></a>ftp literal

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將逐字引數傳送到遠端 ftp 伺服器。 會傳回單一 ftp 回復碼。

> [!NOTE]
> 此命令與 [ [ftp 引號] 命令](ftp-quote.md)相同。

## <a name="syntax"></a>語法

```
literal <argument> [ ]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<argument>` | 指定要傳送到 ftp 伺服器的引數。 |

### <a name="examples"></a>範例

若要將**quit**命令傳送到遠端 ftp 伺服器，請輸入：

```
literal quit
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 引號命令](ftp-quote.md)

- [其他 FTP 指引](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
