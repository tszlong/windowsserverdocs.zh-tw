---
title: ftp quote
description: Ftp 引號命令的參考文章，此命令會將逐字引數傳送到遠端 ftp 伺服器。
ms.topic: reference
ms.assetid: 4500a1d3-c091-42c7-a909-f61df7f2e993
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5e70c93e7a8fe5c32038eec08c7115aa1fec80bf
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2020
ms.locfileid: "89035796"
---
# <a name="ftp-quote"></a>ftp quote

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將逐字引數傳送到遠端 ftp 伺服器。 傳回單一 ftp 回復碼。

> [!NOTE]
> 此命令與 [ftp literal 命令](ftp-literal_1.md)相同。

## <a name="syntax"></a>語法

```
quote <argument>[ ]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<argument>` | 指定要傳送至 ftp 伺服器的引數。 |

### <a name="examples"></a>範例

若要將 **quit** 命令傳送到遠端 ftp 伺服器，請輸入：

```
quote quit
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 常值命令](ftp-literal_1.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
