---
title: ftp remotehelp
description: Ftp remotehelp 命令的參考文章，它會顯示遠端命令的說明。
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ef23adf3-ead4-44c8-ac1d-c8a6f4b2bf73
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 54d784751291515a022a561ca9600e3e967a9d8e
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925745"
---
# <a name="ftp-remotehelp"></a>ftp remotehelp

> 適用于： Windows Server （半年通道）、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

顯示遠端命令的說明。

## <a name="syntax"></a>語法

```
remotehelp [<command>]
```

### <a name="parameters"></a>參數

| 參數 | 說明 |
| ------- | -------- |
| `[<command>]` | 指定您想要協助的命令名稱。 如果 `<command>` 未指定，此命令會顯示所有遠端命令的清單。 您也可以使用 [ [ftp 引號](ftp-quote.md)] 或 [ [ftp 常](ftp-literal_1.md)值] 來執行遠端命令。 |

### <a name="examples"></a>範例

若要顯示遠端命令的清單，請輸入：

```
remotehelp
```

若要顯示 [*適用于] 遠端命令*的語法，請輸入：

```
remotehelp feat
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp quote](ftp-quote.md)

- [ftp literal](ftp-literal_1.md)

- [其他 FTP 指引](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
