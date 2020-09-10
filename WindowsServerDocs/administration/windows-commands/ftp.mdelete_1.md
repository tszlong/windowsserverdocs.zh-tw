---
title: ftp mdelete
description: Ftp mdelete 命令的參考文章，此命令會刪除遠端電腦上的檔案。
ms.topic: reference
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4b9ca1843fd405aef2a1c28c8b7f94bbc650f2d4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627619"
---
# <a name="ftp-mdelete"></a>ftp mdelete

> 適用于： Windows Server (半年通道) 、Windows Server 2019、Windows Server 2016、Windows Server 2012 R2、Windows Server 2012

刪除遠端電腦上的檔案。

## <a name="syntax"></a>語法
```
mdelete <remotefile>[...]
```

### <a name="parameters"></a>參數

| 參數 | 描述 |
| --------- | ----------- |
| `<remotefile>` | 指定要刪除的遠端檔案。 |

### <a name="examples"></a>範例

若要刪除 *a.exe* 和 *b.exe*的遠端檔案，請輸入：

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>其他參考資料

- [命令列語法關鍵](command-line-syntax-key.md)

- [其他 FTP 指導方針](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
