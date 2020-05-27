---
title: ftp 報價
description: '[Ftp 引號] 命令的參考主題，它會將逐字引數傳送到遠端 ftp 伺服器。'
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 87dd81d4feb6a5509a8609f5c479e3352d5fb7ea
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820338"
---
# <a name="ftp-quote"></a>ftp 報價

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將逐字引數傳送到遠端 ftp 伺服器。 會傳回單一 ftp 回復碼。

> [!NOTE]
> 此命令與[ftp literal 命令](ftp-literal_1.md)相同。

## <a name="syntax"></a>語法

```
quote <argument>[ ]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| --------- | ----------- |
| `<argument>` | 指定要傳送到 ftp 伺服器的引數。 |

### <a name="examples"></a>範例

若要將**quit**命令傳送到遠端 ftp 伺服器，請輸入：

```
quote quit
```

## <a name="additional-references"></a>其他參考

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 常值命令](ftp-literal_1.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))