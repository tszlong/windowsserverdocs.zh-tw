---
title: ftp literal
description: Ftp literal 命令的參考檔，它會將逐字引數傳送到遠端 ftp 伺服器。
ms.topic: reference
ms.assetid: fb81aa2d-07fa-4e79-bf44-1fb5526fdf14
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 67c881788e96ef825ad097e2ebd68ae20d753ebe
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621581"
---
# <a name="ftp-literal"></a>ftp literal

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

將逐字引數傳送到遠端 ftp 伺服器。 傳回單一 ftp 回復碼。

> [!NOTE]
> 此命令與 [ftp 引號命令](ftp-quote.md)相同。

## <a name="syntax"></a>語法

```
literal <argument> [ ]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<argument>` | 指定要傳送至 ftp 伺服器的引數。 |

### <a name="examples"></a>範例

若要將 **quit** 命令傳送到遠端 ftp 伺服器，請輸入：

```
literal quit
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [ftp 引號命令](ftp-quote.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
